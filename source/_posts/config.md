---
title: C#配置文件读写
---
![](http://ohe5u4k9s.bkt.clouddn.com/rdn_52b06f484a105.jpg)

*致我现在仍未学会的unity*


代码：
```
 public class ConfigHelper
    {

        /// <summary>
        /// 根据键值获取配置文件
        /// </summary>
        /// <param name="key"></param>
        /// <returns></returns>
        public static string GetConfig(string key)
        {
            
            string val = string.Empty;
           
            if (ConfigurationManager.AppSettings.AllKeys.Contains(key))
            { val = ConfigurationManager.AppSettings[key]; }
            return val;
        }

        /// <summary>
        /// 获取所有配置文件
        /// </summary>
        /// <returns></returns>
        public static Dictionary<string, string> GetConfig()
        {
            ConfigurationManager.RefreshSection("appSettings");
            Dictionary<string, string> dict = new Dictionary<string, string>();
            foreach (string key in ConfigurationManager.AppSettings.AllKeys)
            {
                dict.Add(key, ConfigurationManager.AppSettings[key]);
            }
            return dict;
        }

        /// <summary>
        /// 根据键值获取配置文件
        /// </summary>
        /// <param name="key">键值</param>
        /// <param name="defaultValue">默认值</param>
        /// <returns></returns>
        public static string GetConfig(string key, string defaultValue)
        {
            string val = defaultValue;
            if (ConfigurationManager.AppSettings.AllKeys.Contains(key))
                val = ConfigurationManager.AppSettings[key];
            if (val == null)
                val = defaultValue;
            return val;
        }

        /// <summary>
        /// 写配置文件,如果节点不存在则自动创建
        /// </summary>
        /// <param name="key">键值</param>
        /// <param name="value">值</param>
        /// <returns></returns>
        public static bool SetConfig(string key, string value)
        {
            try
            {
                Configuration conf = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
                if (!conf.AppSettings.Settings.AllKeys.Contains(key))
                { conf.AppSettings.Settings.Add(key, value); }
                else
                { conf.AppSettings.Settings[key].Value = value; }
             
                conf.Save(ConfigurationSaveMode.Modified);
            
                return true;
            }
            catch { return false; }
        }

        /// <summary>
        /// 写配置文件(用键值创建),如果节点不存在则自动创建
        /// </summary>
        /// <param name="dict">键值集合</param>
        /// <returns></returns>
        public static bool SetConfig(Dictionary<string, string> dict)
        {
            try
            {
                if (dict == null || dict.Count == 0)
                    return false;
                Configuration conf = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
                foreach (string key in dict.Keys)
                {
                    if (!conf.AppSettings.Settings.AllKeys.Contains(key))
                    { conf.AppSettings.Settings.Add(key, dict[key]);
                    }
                    else
                    { conf.AppSettings.Settings[key].Value = dict[key]; }
                   
                }
                conf.Save(ConfigurationSaveMode.Modified);
                
                return true;
            }
            catch { return false; }
        }

    }
```


**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)


