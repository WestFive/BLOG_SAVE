---
title: Signal搭建一个聊天室。
---
![](http://ohe5u4k9s.bkt.clouddn.com/proxy.jpg)

**Signal框架学习**

1.引入NuGut包
ASPNET.net.Signal.Hubs

新建一个owin startup类
代码：
```
 public void Configuration(IAppBuilder app)
        {
            // 有关如何配置应用程序的详细信息，请访问 http://go.microsoft.com/fwlink/?LinkID=316888
           // ConfigureAuth(app);
            var hubConfiguration = new HubConfiguration();

            hubConfiguration.EnableDetailedErrors = true;
            GlobalHost.Configuration.DisconnectTimeout = TimeSpan.FromSeconds(8);
            app.MapSignalR(hubConfiguration);
        }

```
2.新建一个集线器signalR类

思路：
 服务端写法 :
 ```

    public void Send(@1) (string Message)
    {
        //发送给所有客户端
        Client.All.broadcastMessage(@2)(message);
    }

 ```

 客户端调用方法：
 ```
  new 一个 IHubProxy  hub;

  ///创建一个连接
  var connection = new HubConnection("Hub服务地址（调试的时候一般为 localhost:端口）”);

  ///调用服务端方法的接口
  hub = connection.Create("Hub类名");  


  客户端调用方式：
  ///这部分相当于注册事件
  hub.On("@2",(返回数据)=>{ 返回数据的处理 });

  hub.Invoke("@1",参数);//这步相当于触发事件。

 ```


服务端代码
```
  public class ChatHub : Hub
    {


        public static List<string> Onlineusers = new List<string>();

        public static List<string> messageCache = new List<string>();
        /// <summary>
        /// 广播消息
        /// </summary>
        /// <param name="username"></param>
        /// <param name="message"></param>     
        
        public void Send(string SendID,string message)
        {
            Clients.All.broadcastMessage(Context.ConnectionId, message);
            messageCache.Add(Context.ConnectionId + message);
            //Clients.All.refresh()
           // Clients.All.addMessage(username + message);
            
        }


        public void p2p(string ReciveID,string SendID,string SendMessage)
        {

            
            Clients.Client(ReciveID).sendP2pMessage(ReciveID, SendMessage);
            messageCache.Add(SendID + "发送了" + SendMessage + "给" + ReciveID);
            
        }


        /// <summary>
        /// 获取信息列表。详细看客户端。
        /// </summary>
        /// <param name="clinetID"></param>
        public void GetMesList(string clinetID)
        {
            StringBuilder sb = new StringBuilder();
            foreach (var item in messageCache)
            {
                sb.AppendLine(item);

            }
            Clients.Client(clinetID).SendList(sb.ToString());

        }
        /// <summary>
        /// 获取用户列表
        /// </summary>
        /// <returns></returns>

        public void GetOnline(string ClientID)
        {
            StringBuilder sb = new StringBuilder();
            foreach (var item in Onlineusers)
            {
                sb.Append("*"+item);
            }
            Clients.Client(ClientID).SendUserList(sb.ToString());
        }
        public override Task OnConnected()
        {
            Onlineusers.Add(Context.ConnectionId);
            Clients.All.broadcastMessage(Context.ConnectionId, "加入了服务器");

            StringBuilder sb = new StringBuilder();
            foreach (var item in Onlineusers)
            {
                sb.Append("*" + item);
            }
            Clients.All.SendUserList(sb.ToString());
            Clients.All.broadcastMessage("当前在线人数", Onlineusers.Count + "人");
            return base.OnConnected();
        }

        public override Task OnDisconnected(bool stopCalled)
        {
            Onlineusers.Remove(Context.ConnectionId);
            Clients.All.broadcastMessage(Context.ConnectionId, "离开了服务器");
            StringBuilder sb = new StringBuilder();
            foreach (var item in Onlineusers)
            {
                sb.Append("*" + item);
            }
            Clients.All.SendUserList(sb.ToString());
            Clients.All.broadcastMessage("当前在线人数", Onlineusers.Count + "人");
            return base.OnConnected();
        }



    }
```

客户端代码：

```
 public partial class Form1 : Form
    {


        /// <summary>
        /// hub句柄
        /// </summary>
        IHubProxy hub;
        string name = "";

        public Form1()
        {
            InitializeComponent();
            

        }
    

        private void button1_Click(object sender, EventArgs e)
        {
            var connection = new HubConnection(textBox1.Text);
            hub = connection.CreateHubProxy(textBox2.Text);

            connection.Error += error => MessageBox.Show(error.ToString());
            try
            {
                connection.Start().Wait();
                name = connection.ConnectionId;
                hub.On<string, string>("broadcastMessage", (m, s) =>
                {
                  
                        textBoxRecive.Invoke(new MethodInvoker(() => { this.textBoxRecive.AppendText(m +"说："+s + "\r\n"); }));
                });

                hub.On<string, string>("sendP2pMessage", (c, m) =>
                {
                    textBoxRecive.Invoke(new MethodInvoker(() => { this.textBoxRecive.AppendText(c+"说："+m + "\r\n"); }));
                });



                hub.On<string>("SendUserList", (listvalue) =>
                {
                    textBoxRecive.Invoke(new MethodInvoker(() =>
                    {
                      
                        string[] str = listvalue.Trim().Split('*');
                        List<string> values = new List<string>();
                        textBoxDic.AppendText("当前在线用户 ：" + "\r\n");
                        foreach (var item in str)
                        {
                            if(item!="")
                            {
                                values.Add(item);
                                textBoxDic.AppendText(item + "\r\n");
                            }

                        }
                        
                        comboBox1.DataSource = values;
                        comboBox1.SelectedIndex = 0;
                        
                    }));

                });

                ///这里是触发该次回调的调用过程。
                hub.Invoke("GetOnline", name);

                // hub.Invoke("Send", "", name);

                MessageBox.Show("连接成功!");
                label4.Text = name;
            }
            catch(Exception ex)
            {
                MessageBox.Show(ex.ToString());
            }

        }



        /// <summary>
        /// 表示调用信息列表。
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void button4_Click(object sender, EventArgs e)
        {
            

            ///这里可以理解为注册回调。 listValue为回调的接收data.
            hub.On<string>("SendList", (listvalue) =>
             {
                 textBoxRecive.Invoke(new MethodInvoker(() =>
                 {
                     textBoxDic.AppendText("缓存数据" + "\r\n" + listvalue+"\r\n");
                 }));

             });

            ///这里是触发该次回调的调用过程。
           hub.Invoke("getMesList",name);


        }

        private void button2_Click(object sender, EventArgs e)
        {
            if (radioButton2.Checked)
            {
                ///发送点对点消息 第一个参数 接收者ID 第二个参数 发送ID 第三个参数 发送内容。
                hub.Invoke("p2p", comboBox1.Text, name, textBoxSend.Text);

            }
            else
            {
                hub.Invoke("Send", name, this.textBoxSend.Text);
            }
           
        }

        private void button3_Click(object sender, EventArgs e)
        {
            hub.Invoke("Send", "测试数据");
        }


       
    }
```

Web页面客户端简单实现：
```
<!DOCTYPE html>
<html>
<head>
    <title>SignalR Simple Chat</title>
    <style type="text/css">
        .container {
            background-color: #99CCFF;
            border: thick solid #808080;
            padding: 20px;
            margin: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <input type="text" id="message" />
        <input type="button" id="sendmessage" value="Send" />
        <input type="hidden" id="displayname" />
        <div id="mineID"></div>
        <ul id="discussion"></ul>
    </div>
    <script src="Scripts/jquery-1.6.4.js"></script>
    <script src="Scripts/jquery.signalR-2.2.1.js"></script>
    <!--Reference the autogenerated SignalR hub script. -->
    <script src="signalr/hubs"></script>    
    <!--Add script to update the page and send messages.-->
    <script type="text/javascript">
        $(function () {
            // Declare a proxy to reference the hub.
            var chat = $.connection.chatHub;
            // Create a function that the hub can call to broadcast messages.

            //var name = chat.client.connection.oncl
            //$('#mineID').append("<p>" + name + "</p>");

            var name;
            chat.client.broadcastMessage = function (username, message) {
                // Html encode display name and message.
                var encodedName = $('<div />').text(username).html();
                var encodedMsg = $('<div />').text(message).html();
                // Add the message to the page.
                $('#discussion').append('<li><strong>' + encodedName
                    + '</strong>:&nbsp;&nbsp;' + encodedMsg + '</li>');
            };

          

             
           
            // Get the user name and store it to prepend to messages.
            //$('#displayname').val(prompt('Enter your name:', ''));
            // Set initial focus to message input box.
            //$('#message').focus();
            // Start the connection.
            $.connection.hub.start().done(function () {
                $('#sendmessage').click(function () {
                    // Call the Send method on the hub.
                    chat.server.send($('#displayname').val(), $('#message').val());
                    // Clear text box and reset focus for next comment.
                    $('#message').val('').focus();
                });
            });
        });
    </script>
</body>
</html>
```


**————————————————————————————————我是有底线的———————————————————————————————————————**

Everybody hurts.
