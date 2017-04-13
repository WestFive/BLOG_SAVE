---
title: 应用程序配置
---
![](http://ohe5u4k9s.bkt.clouddn.com/proxy.jpg)


**C# ini文件操作**

 *来源:http://www.cnblogs.com/polk6/p/6052908.html

介绍C#如何对ini文件进行读写操作，C#可以通过调用【kernel32.dll】文件中的 WritePrivateProfileString()和GetPrivateProfileString()函数分别对ini文件进行读和写操作。包括：读取key的值、保存key的值、读取所有section、读取所有key、移除section、移除key等操作。*
**目录**
1. ini文件介绍

2. 读取操作：包括读取key的值、读取所有section、读取所有key等操作。

3. 写入操作： 包括保存key的值、移除section、移除key等操作。

4. 源码下载：展示运行图及源码下载

```` 
1. ini文件介绍
ini文件常用于存储各类应用的配置信息，而内部的文件结构主要包括三个概念：section、key和value。

其中section为各独立的区域块，名称可以为英文、中文。
>
>


````

````
C#可以通过调用【kernel32.dll】文件中的 GetPrivateProfileString()函数对ini文件进行读取操作。

官方API：https://msdn.microsoft.com/zh-cn/library/ms724353.aspx

函数签名：
>>[DllImport("kernel32")]
>>private static extern int GetPrivateProfileString(string sectionName, string key, string defaultValue, byte[] returnBuffer, int size, string filePath);　

成员：

sectionName  {string | null}：要读区的区域名。若传入null值，第4个参数returnBuffer将会获得所有的section name。

key {string | null}：key的名称。若传入null值，第4个参数returnBuffer将会获得所有的指定sectionName下的所有key name。

defaultValue {string}：key没找到时的返回值。

returnBuffer {byte[]}：key所对应的值。

filePath {string}：ini文件路径。

支持的操作：

1) 获取指定key的值。

2) 获取ini文件所有的section名称。

3) 获取指定section下的所有key名称。

````
**获取指定key的值**
```
/// <summary>
/// 根据Key读取Value
/// </summary>
/// <param name="sectionName">section名称</param>
/// <param name="key">key的名称</param>
/// <param name="filePath">文件路径</param>
public static string GetValue(string sectionName, string key, string filePath)
{
    byte[] buffer = new byte[2048];
    int length = GetPrivateProfileString(sectionName, key, "发生错误", buffer,999, filePath);
    string rs = System.Text.UTF8Encoding.Default.GetString(buffer, 0, length);
    return rs;
}
```
2.2 获取INI文件中所有的section名称
```
/// <summary>
/// 获取ini文件内所有的section名称
/// </summary>
/// <param name="filePath">文件路径</param>
/// <returns>返回一个包含section名称的集合</returns>
public static List<string> GetSectionNames(string filePath)
{
    byte[] buffer = new byte[2048];
    int length = GetPrivateProfileString(null, "", "", buffer, 999, filePath);
    String[] rs = System.Text.UTF8Encoding.Default.GetString(buffer, 0, length).Split(new string[] { "\0" },StringSplitOptions.RemoveEmptyEntries);
    return rs.ToList();
}
```
2.3 获取指定section下的所有key名称

```
/// <summary>
/// 获取指定section内的所有key
/// </summary>
/// <param name="sectionName">section名称</param>
/// <param name="filePath">文件路径</param>
/// <returns>返回一个包含key名称的集合</returns>
public static List<string> GetKeys(string sectionName, string filePath)
{
    byte[] buffer = new byte[2048];
    int length = GetPrivateProfileString(sectionName,null,"", buffer, 999, filePath);
    String[] rs = System.Text.UTF8Encoding.Default.GetString(buffer, 0, length).Split(new string[] { "\0" }, StringSplitOptions.RemoveEmptyEntries);
    return rs.ToList();
}
```
3. WritePrivateProfileString()函数：写入操作
```
C#可以通过调用【kernel32.dll】文件中的 WritePrivateProfileString()函数对ini文件进行写入操作。

官方API：https://msdn.microsoft.com/zh-cn/library/ms725501.aspx

函数签名：
>>[DllImport("kernel32")]
>>private static extern long WritePrivateProfileString(string sectionName, string key, string value, string filePath);
成员：

sectionName {string}：要写入的区域名。

key {string | null}：key的名称。若传入null值，将移除指定的section。

value {string | null}：设置key所对应的值。若传入null值，将移除指定的key。

filePath {string}：ini文件路径。

支持的操作：

1) 创建/设置key的值。

2) 移除指定的section。

3) 移除指定的key。

 
```
3.1 创建/设置key的值
注意：若此key不存在将会创建，否则就为修改此key的值。
```
/// <summary>
/// 保存内容到ini文件
/// <para>若存在相同的key，就覆盖，否则就增加</para>
/// </summary>
/// <param name="sectionName">section名称</param>
/// <param name="key">key的名称</param>
/// <param name="value">存储的值</param>
/// <param name="filePath">文件路径</param>
public static bool SetValue(string sectionName, string key, string value, string filePath)
{
    int rs = (int)WritePrivateProfileString(sectionName, key, value, filePath);
    return rs > 0;
}
```
3.2 移除指定的section
说明：key参数传入null就为移除指定的section。
```
/// <summary>
/// 移除指定的section
/// </summary>
/// <param name="sectionName">section名称</param>
/// <param name="filePath">文件路径</param>
/// <returns></returns>
public static bool RemoveSection(string sectionName, string filePath)
{
    int rs = (int)WritePrivateProfileString(sectionName, null, "", filePath);
    return rs > 0;
}
```
3.3 移除指定的key
说明：value参数传入null就为移除指定的key。
```
/// <summary>
/// 移除指定的key
/// </summary>
/// <param name="sectionName">section名称</param>
/// <param name="filePath">文件路径</param>
/// <returns></returns>
public static bool Removekey(string sectionName, string key, string filePath)
{
    int rs = (int)WritePrivateProfileString(sectionName, key, null, filePath);
    return rs > 0;
}

```
![](http://images2015.cnblogs.com/blog/153475/201611/153475-20161112170846780-595289799.png)


*附：  propertyGrid控件绑定*
```
  propertyGrid1.SelectedObject = obj;


[DefaultPropertyAttribute("配置")]
    public class Config
    {
        /// <summary>
        /// 软件设置
        /// </summary>
        private string tcpport;
        private string ftpuserName;
        private string ftppassWord;
        private string ftpinway;
        private string ftpoutway;

        /// <summary>
        /// 软件设置项
        /// </summary>
        private string name;//设置示例



        [DisplayName("姓名")]
        [BrowsableAttribute(true)]
        [CategoryAttribute("模块"), DescriptionAttribute("说明")]
        public string Name
        {
            get
            {
                return name;
            }
            set
            {
                name = value;
            }
        }


```
  End.





**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)
