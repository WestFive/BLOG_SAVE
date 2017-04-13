---
title: Signal以控制台作为宿主无需IIS。
---
![](http://ohe5u4k9s.bkt.clouddn.com/proxy.jpg)

**控制台宿主**

1.引入NuGut包
Microsoft.AspNet.Signal.Hubs
2.引入跨域包
Microsoft.Owim.Cors
3.引入Host包
Microsoft.AspNet.SignalR.SelfHost


**Code**
```
 class Program
    {
        static void Main(string[] args)
        {
            string url = "http://localhost:8080";
            using (WebApp.Start(url))
            {
                Console.WriteLine("服务开启成功：请勿关闭此窗口。地址{0}", url);
                Console.ReadLine();
            }


        }
    }

```

 StartUpclass :
 ```

    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            // 有关如何配置应用程序的详细信息，请访问 http://go.microsoft.com/fwlink/?LinkID=316888

            app.UseCors(CorsOptions.AllowAll);//跨域
            app.MapSignalR();
        }

    }

 ```

Hub就随便写了
 ```
  public class CenterHub : Hub
    {
        public void Hello()
        {
          clients.All.GetHello("HELLO BAD");
        }

        //public override   重写连接和断开连接的方法。
    }

 ```

 客户端调用如下 以winform举例
 ```
全局变量：
        IHubProxy hub = null;
        HubConnection conncetion = null;
        
 private void Form1_Load(object sender, EventArgs e)
        {
             conncetion = new HubConnection("http://localhost:8080");
            hub = conncetion.CreateHubProxy("CenterHub");

            conncetion.Start().Wait();

            hub.On("GetHello",(data)=>
            {
                 Messagebox.Show(data);
            });//注册事件监听。

            hub.Invoke("Hello");//去调用服务端的事件


        }

 ```





**————————————————————————————————我是有底线的———————————————————————————————————————**

Everybody hurts.
