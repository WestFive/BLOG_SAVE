---
title: 恶俗古风生成器
---
![](http://ohe5u4k9s.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720161214120541.png)
[点我](/Pages/esu.html)


*west13*


**原为ruby版古风生成器**

**原 po：**
```
关键词：朱砂 天下 杀伐 人家 韶华 风华 繁华 血染 墨染 白衣 素衣 嫁衣 倾城 孤城 空城 旧城
 旧人 伊人 心疼 春风 古琴 无情 迷离 奈何 断弦 焚尽 散乱 陌路 乱世 笑靥 浅笑 明眸 轻叹 烟火 
一生 三生 浮生 桃花 梨花 落花 烟花 离殇 情殇 爱殇 剑殇 灼伤 仓皇 匆忙 陌上 清商 焚香 墨香 
微凉 断肠 痴狂 凄凉 黄梁 未央 成双 无恙 虚妄 凝霜 洛阳 长安 江南 忘川 千年 纸伞 烟雨 回眸 
公子 红尘 红颜 红衣 红豆 红线 青丝 青史 青冢 白发 白首 白骨 黄土 黄泉 碧落 紫陌情深缘浅 情深不寿 
莫失莫忘 阴阳相隔 如花美眷 似水流年 眉目如画 曲终人散 繁华落尽 不诉离殇 一世长安
基本句式：
1.xx，xx，xx了xx。 
2.xxxx，xxxx，不过是一场xxxx。
3.你说xxxx，我说xxxx，最后不过xxxx。
4.xx，xx，许我一场xxxx。
5一x一x一xx，半x半x半xx。
6.你说xxxx xxxx，后来xxxx xxxx。
7.xxxx，xxxx，终不敌xxxx。
注意事项：
1.使用一个句式时一定要多重复几次，形成看起来异常高端的排比句。
2.［殇］这个字恶俗到爆，一定要多用。
3.不要随意用连词，就让这些动词名词形容词堆在一起，发生奇妙的反应。
4.填句子千万不能有逻辑性！填句子千万不能有逻辑性！填句子千万不能有逻辑性！重要的事情说三遍。
例句：
1.江南烟雨，陌上白衣，不过是一场情深缘浅。伊人回眸，繁华落尽，不过是一场烟火迷离。浮生微凉，白骨成双，不过是一场三世离殇。
2.旧城，未央，许我一场墨染清商。乱世，无情，许我一场白衣仓皇。忘川，千年，许我一场奈何成双。
end
【简直丧心病狂精神污染，po主去吐一吐。】


```

考虑到，不要逻辑，那么最适合随机函数了。
于是我们得到了一位 Ruby 古风诗人（共 22 行），他每秒都能生产一句古风句子
```
@two_chars_words = %w"朱砂 天下 杀伐 人家 韶华 风华 繁华 血染 墨染 白衣 素衣 嫁衣 倾城 孤城 空城 旧城 旧人 伊人 心疼 春风 古琴 无情 迷离 奈何 断弦 焚尽 散乱 陌路 乱世 笑靥 浅笑 明眸 轻叹 烟火 一生 三生 浮生 桃花 梨花 落花 烟花 离殇 情殇 爱殇 剑殇 灼伤 仓皇 匆忙 陌上 清商 焚香 墨香 微凉 断肠 痴狂 凄凉 黄梁 未央 成双 无恙 虚妄 凝霜 洛阳 长安 江南 忘川 千年 纸伞 烟雨 回眸 公子 红尘 红颜 红衣 红豆 红线 青丝 青史 青冢 白发 白首 白骨 黄土 黄泉 碧落 紫陌"
@four_chars_words = %w"情深缘浅 情深不寿 莫失莫忘 阴阳相隔 如花美眷 似水流年 眉目如画 曲终人散 繁华落尽 不诉离殇 一世长安"
@sentence_model = %w"xx，xx，xx了xx。 xxxx，xxxx，不过是一场xxxx。 你说xxxx，我说xxxx，最后不过xxxx。 xx，xx，许我一场xxxx。 一x一x一xx，半x半x半xx。 你说xxxxxxxx，后来xxxxxxxx。 xxxx，xxxx，终不敌xxxx。"

def get_sentence
  model = @sentence_model.sample(1)[0].clone
  while model.include?'xxxx'
    model.sub!(/xxxx/, @four_chars_words.sample(1)[0])
  end
  while model.include?'xx'
    model.sub!(/xx/, @two_chars_words.sample(1)[0])
  end
  while model.include?'x'
    model.sub!(/x/, @two_chars_words.sample(1)[0][rand(0..1)])
  end
  puts model
end

while true
  get_sentence
  sleep 1
end

```

**然后搜了一下发现没人写JS版，花了半小时粗略的写了一下**

代码如下:
可以更优化，毕竟后端野路子写前端，没有考虑太多。
顺便说一句 这真的有够恶俗的O__O "…
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>恶俗古风生成器</title>
    <style>
        body {
            background-image: url("http://ohe5u4k9s.bkt.clouddn.com/bg.jpg");
            background-size: cover;
            background-attachment: fixed;
            background-position: center;
            font-family: "Helvetica Neue", Arial, Helvetica, sans-serif;
            color: #333333;
            background-color: #ffffff;
        }
        
        div {
            border: 1px solid;
            border-color: lightskyblue;
    
        }
        textarea{
            margin-left: 700px;
            width: 300px;
            height: 150px;
            border:4px solid;
            border-radius: 4px;
            border-color: lightskyblue;
        }
        input{
            margin-left: 700px;
            float: left;
            border:4px solid;
            border-radius: 4px;
            border-color: lightskyblue;
        }
    </style>
    <script>
  function startbuild(){
    var Arr = new Array("朱砂","天下", "杀伐", "人家", "韶华", "风华", "繁华", "血染", "墨染" ,"白衣" ,"素衣" ,"嫁衣", "倾城", "孤城", "空城", "旧城" ,"旧人", "伊人", "心疼", "春风", "古琴", "无情", "迷离" ,"奈何", "断弦" ,"焚尽" ,"散乱" ,"陌路", "乱世", "笑靥", "浅笑" ,"明眸", "轻叹", "烟火", "一生", "三生", "浮生", "桃花", "梨花", "落花", "烟花",   "离殇",  "情殇", "爱殇", "剑殇", "灼伤", "仓皇", "匆忙", "陌上", "清商", "焚香", "墨香" ,"微凉" ,"断肠" ,"痴狂" ,"凄凉" ,"黄梁" ,"未央", "成双", "无恙", "虚妄" ,"凝霜" ,"洛阳", "长安" ,"江南", "忘川", "千年", "纸伞", "烟雨", "回眸", "公子" ,"红尘" ,"红颜", "红衣", "红豆", "红线", "青丝", "青史", "青冢", "白发", "白首", "白骨", "黄土", "黄泉", "碧落", "紫陌", "情深缘浅", "情深不寿", "莫失莫忘", "阴阳相隔", "如花美眷", "似水流年", "眉目如画" ,"曲终人散", "繁华落尽", "不诉离殇", "一世长安");
    

     var n =  Math.floor(Math.random() * Arr.length + 1)-1;  
      
      function getTwo()
      {
          var result = Arr[Math.floor(Math.random() * Arr.length + 1)-1];
          while(true)
          {
          if(result.length!=2)
          {
             result =  Arr[Math.floor(Math.random() * Arr.length + 1)-1];
          }
          else
          {
             return result;
             
          }
          }
           
      }
      function getFour()
      {
           var result = Arr[Math.floor(Math.random() * Arr.length + 1)-1];
          while(true)
          {
          if(result.length!=4)
          {
             result =  Arr[Math.floor(Math.random() * Arr.length + 1)-1];
          }
          else
          {
             return result;
             
          }
          }
      }

      function getOne()
      {
          var result = getTwo();
          var newresult = result[0];
          return newresult;
      }

     var theText = getTwo()+","+getTwo()+","+getTwo()+"了"+getTwo()+"。"+"\r\n"
                   +getFour()+","+getFour()+","+"不过一场"+getFour()+"。"+"\r\n"
                   +"你说"+getFour()+","+"我说"+getFour()+","+"最后不过"+getFour()+"。"+"\r\n"
                   +getFour()+","+getFour()+","+"许我一场"+getFour()+"。"+"\r\n"
                   +"一"+getOne()+"一"+getOne()+"一"+getTwo()+","+"半"+getOne()+"半"+getOne()+"半"+getTwo()+"。"+"\r\n"
                   +"你说"+getFour()+","+getFour()+","+"后来"+getFour()+","+getFour()+"。"+"\r\n"
                   +getFour()+","+getFour()+","+"终不敌"+getFour()+"。"+"\r\n";
     
    

      
     
     document.getElementById('text1').value = theText;
    }
    </script>
</head>

<body>
    <div>
        <h1  style="text-align:center">恶俗古风生成器</h1>
    </div>
    <div>
    
    <textarea id="text1"></textarea>
    </div>
    <div>
        <input type="button" value="开始" onclick="startbuild()" />
    </div>
    
    
</body>

</html>

```


**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)
