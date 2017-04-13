---
title: 震惊！DotNetCore CORS竟然有如此大的坑
---
![](http://ohe5u4k9s.bkt.clouddn.com/14601643955179853.jpg)


**使用跨域请第一个注入跨域**


话不多说直接看代码
```
 public void ConfigureServices(IServiceCollection services)
        {
            services.AddCors(options => options.AddPolicy("AllowAll", p => p.AllowAnyOrigin()
                                                                 .AllowAnyMethod()
                                                                  .AllowAnyHeader()
                                                                  .AllowCredentials()));
            // Add framework services.
            services.AddMvc();
            services.AddSignalR(options =>
            {
                options.Hubs.EnableDetailedErrors = true;
            });




        }

```
```

 public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
        {
            app.UseCors("AllowAll");
            loggerFactory.AddConsole(Configuration.GetSection("Logging"));
            loggerFactory.AddDebug();

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();


            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
            }

            app.UseStaticFiles();
            app.UseWebSockets();//加入Websocket和SignalR支持
            app.UseSignalR();
          


            app.UseMvc(routes =>
            {
                routes.MapRoute(
                    name: "default",
                    template: "{controller=Home}/{action=Index}/{id?}");
            });

        }
```
然后在每个Controller方法中使用
```

  [EnableCors("AllowAll")]
        public IActionResult Index()
        {
            return View();
        }

        public IActionResult About()
        {
            //ViewData["Message"] = "Your application description page.";

            return View();
        }


        [EnableCors("AllowAll")]
        public IActionResult Contact()
        {
            //ViewData["Message"] = "Your contact page.";

            return View();
        }
        [EnableCors("AllowAll")]
        public IActionResult Chat()
        {
           
            return View();
        }

        [EnableCors("AllowAll")]
        public IActionResult Error()
        {
            return View();
        }
```

即使被坑了一个下午，不骄不躁，这个时候保持微笑就好了。 
艹。

**————————————————————————————————もう　やめろ———————————————————————————————————————**