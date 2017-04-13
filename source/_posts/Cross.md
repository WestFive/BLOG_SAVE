---
title: Signal+.netCore+Ubuntu实现跨平台的数据转发方案
---
![](http://ohe5u4k9s.bkt.clouddn.com/14601643955179853.jpg)


![](http://oion935wi.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720170308174927.jpg)
实现效果如图
```


winform程序从ubuntu跑的服务中获取数据。
```

**实现方式记录相关：**

1 VS2017新建一个.netCore MvC项目
```
   引入
   Gray.Microsoft.AspNetCore.SignalR.Server 
   Gray.Microsoft.AspNetCore.SignalR.Messaging;
   Microsoft.AspNetCore.WebSockets
   当然了，还有跨域的也一并引入得了。
   Microsoft.AspNetCore.Cors
```

核心代码

 startup类（启动类）ConfigureServices方法中注入跨域以及signalR服务。
```
   public void ConfigureServices(IServiceCollection services)
        {
            // Add framework services.
            services.AddMvc();
            services.AddSignalR(options=> {
                options.Hubs.EnableDetailedErrors = true;
            });
            services.AddCors();
           

        }
```
startup类中Configure方法中 注册相关的服务。
```
          public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
        {
            loggerFactory.AddConsole(Configuration.GetSection("Logging"));
            loggerFactory.AddDebug();

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();

                // Browser Link is not compatible with Kestrel 1.1.0
                // For details on enabling Browser Link, see https://go.microsoft.com/fwlink/?linkid=840936
                // app.UseBrowserLink();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
            }
            ///*以下 */
            app.UseStaticFiles();
            app.UseWebSockets();//加入Websocket和SignalR支持
            app.UseSignalR();
            app.UseCors(builder=>builder.WithOrigins("http://localhost"));
            //以上、、

            app.UseMvc(routes =>
            {
                routes.MapRoute(
                    name: "default",
                    template: "{controller=Home}/{action=Index}/{id?}");
            });
            
        }
```
**特别注意** 我们是在本地进行调试，如果你想在同一局域网上不通的地址访问。请在Program类中加入这个
```
  public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseKestrel()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .UseApplicationInsights()
                .UseKestrel().UseUrls("http://*:5000")//添加IP地址访问监听。
                .Build();

            host.Run();
        }
```


启动类写完了，SignalR Hub的写法与上一篇文章类似。
这里提供一个最简单的。

```
     public class LeeHub:Hub
    {
        public void Send(string message)
        {
            Clients.All.MessageRecive(message);
        }
    }
```
winform调用举例
```
     hub.on("MessageRecive",(data)=>
     {
         //yourcode to dealwith (data);
     })

      hub.Invoke("Send","your words for example :\'Hello world\'");
    
```



**Ubuntu部分**
  [微软官方安装Core](https://www.microsoft.com/net/core#linuxubuntu)

  我用的是14.04桌面版的ubuntu
  安装流程如下
  ```
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    sudo apt-get update

  ```
  **注意**
  以下这段话的意思是 请卸载之前的.netCore版本，官方提供了一个linux脚本。请务必卸载干净。
  至于为什么卸载。拓展阅读
  千万不要去网上下载之前的版本。否则最后的编译运行会出问题。
    [.NET Core 计划弃用 project.json](http://www.oschina.net/news/73682/net-core-give-up-project-json)
  ```
  Install .NET Core SDK
Before you start, please remove any previous versions of .NET Core from your system by using this script.

To install .NET Core 1.1 on Ubuntu or Linux Mint, simply use apt-get.

.NET Core 1.1 is the latest version. For long term support versions and additional downloads check the all Linux downloads section.
  ```

  安装完成后将你VS中发布后的代码拷贝进ubuntu中。

这里默认已经安装好dotnet Core
在项目文件夹中打开终端。
或打开终端CD至项目文件夹，随你。
  ```
   命令流程Dotnet restore  //还原Nuget跨平台包，如果你1.0版本肯定会报错。提示找不到project.json 事实上1.0.0后的版本已经弃用JSON格式作为插件包
   //沿用之前的.csproj文件作为项目的支架。

   Dotnet restore //还原完成。
   Dotnet run 

  ```
  ![](http://oion935wi.bkt.clouddn.com/1.png)
  dotnet restore
 ![](http://oion935wi.bkt.clouddn.com/2.png)
  dotnet run

  记得打开Ubuntu的端口访问权限。
  如果你在windows下VS中运行没问题。那么到ubuntu下提示的就是这个。
  OK 
  打开http://localhost:5050 吧






**————————————————————————————————もう　やめろ———————————————————————————————————————**