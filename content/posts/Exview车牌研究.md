---
title: Exview 车牌研究
date: 2017-01-02 17:47:00
tags: [杂谈]
---
> 记载了一些 Exview 逆向工程。

## Exview 车牌研究
车牌其实是 t.cn 短网址
前排提醒本地修改已报废，只能使用远程的车牌来替代

## 老车牌 RIY6p9B
这是最最原始的车牌，指向的是
http://exview.gtool.ml/driver161217.jsonp
也就是
https://github.com/ghostgzt/ExView/raw/master/driver161217.jsonp
为了破解认证，我们必须解读js的内容
```js
eval(function(f,u,c,k,e,r){e=function(c){return(c<u?'':e(parseInt(c/u)))+((c=c%u)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)r[e(c)]=k[c]||e(c);k=[function(e){return r[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])f=f.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return f}('v(["s","m"].u(2.b.c.f(\'6 9!\'+0.4+\'6 9!\'))==-1&&!d.e){7 0.4;g.h="i";7 0.j;2.k.l("\\8\\8\\n\\o\\p\\q\\r~<5>\\t\\3\\a\\w\\x\\y\\z\\A\\B+\\3\\a\\C\\D\\E\\F\\G\\H<5>\\3\\I+\\J\\K\\L\\M",N.O,P(){2.Q.R.S(1)});T U();}',57,57,'localStorage||ExView|u6350|ExView_InitJSON|br|Keep|delete|u597d|Out|u8d60|tools|md5|window|debug|hex_md5|mySession|initjsonpath|RtuFEqa|ExView_InitType|fw|alert|552bc26417cb2969badc8f1229797571|u5750|u516c|u4ea4|u8f66|u5427|1b22289d656182b24547f307c9d368b7|u5df2|indexOf|if|u7684|u8001|u53f8|u673a|uff0c|u51ed10|u51ed|u636e|u627e|u4f5c|u8005|uff01|u8d6050|u53bb|u5e7f|u544a|u9001VIP|sessionStorage|modaltitleextra|function|modules|pluginpage|init|throw|SyntaxError'.split('|'),0,{}))
```
若无法解答，我们只能选择删除验证，而且需要另寻服务器，因为我发现如果content header不为下载，则无法被解析，于是我选择自己调试更新文件。

## 私家车牌 RIkcxgj
这指向的是我的服务器上的 jsonp 文件，唯一缺点是需要实时更新。
于是我写了个脚本每天跑，累死了。
```bash
#!/bin/sh
wget https://github.com/ghostgzt/ExView/raw/master/driver161217.jsonp -O owndriver.jsonp
sed -i '1d' owndriver.jsonp
sed -i "s/灵车漂移/下山的59/" owndriver.jsonp
sed -i "s/服务器上的来源信息.*\"/咱的信息\"/" owndriver.jsonp
```
去验证，去广告，去粗鄙之语。
与此同时，在翻源码时我看到了一些用户的vip信息以明文保存（现在已经base64加密了）。
https://github.com/ghostgzt/ExView/blob/master/dms.jsonp

昵称 | 称号 
-------- | -------- 
岚dalao | 荣誉内测用户
不笨 の 笨蛋→_→ | 资深测评师
IASUI | 资深设计师
Misaki | VIP
VoidKing | VIP
三俗爱好者 | VIP
RinMaki | VIP
Dwarf | SVIP
Bitch | VIP
咸鱼 | SVIP

我就不放qq了，这是隐私信息。
但是昵称改一下就能去广告，挺好的。

----------

更新一波，js 解出来了。
```js
if (["1b22289d656182b24547f307c9d368b7", "552bc26417cb2969badc8f1229797571"].indexOf(ExView.tools.md5.hex_md5('Keep Out!' + localStorage.ExView_InitJSON + 'Keep Out!')) == -1 && !window.debug) {
    delete localStorage.ExView_InitJSON;
    mySession.initjsonpath = "RtuFEqa";
    delete localStorage.ExView_InitType;
    ExView.fw.alert("\u597d\u597d\u5750\u516c\u4ea4\u8f66\u5427~<br>\u5df2\u6350\u8d60\u7684\u8001\u53f8\u673a\uff0c\u51ed10+\u6350\u8d60\u51ed\u636e\u627e\u4f5c\u8005\uff01<br>\u6350\u8d6050+\u53bb\u5e7f\u544a\u9001VIP", sessionStorage.modaltitleextra, function() {
        ExView.modules.pluginpage.init(1)
    });
    throw SyntaxError();
}
```

附个eval解码器。
```html
<script>
a=62;
function encode() {
var code = document.getElementById('code').value;
code = code.replace(/[\r\n]+/g, '');
code = code.replace(/'/g, "\\'");
var tmp = code.match(/\b(\w+)\b/g);
tmp.sort();
var dict = [];
var i, t = '';
for(var i=0; i<tmp.length; i++) {
if(tmp[i] != t) dict.push(t = tmp[i]);
}
var len = dict.length;
var ch;
for(i=0; i<len; i++) {
ch = num(i);
code = code.replace(new RegExp('\\b'+dict[i]+'\\b','g'), ch);
if(ch == dict[i]) dict[i] = '';
}
document.getElementById('code').value = "eval(function(p,a,c,k,e,d){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d[e(c)]=k[c]||e(c);k=[function(e){return d[e]}];e=function(){return'\\\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\\\b'+e(c)+'\\\\b','g'),k[c]);return p}("
+ "'"+code+"',"+a+","+len+",'"+ dict.join('|')+"'.split('|'),0,{}))";
}
function num(c) {
return(c<a?'':num(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36)); }
function run() {
eval(document.getElementById('code').value);
}
function decode() {
var code = document.getElementById('code').value;
code = code.replace(/^eval/, '');
document.getElementById('code').value = eval(code);
}
</script>
<textarea id="code" cols="80" rows="8" style="margin: 0px -32px 10px 0px; height: 184px; width: 600px;"></textarea>
<p><input type="button" onclick="encode()" value="编码"> <input type="button" onclick="run()" value="执行"> <input type="button" onclick="decode()" value="解码"></p>
```

好了，关键地方就在`if (["1b22289d656182b24547f307c9d368b7", "552bc26417cb2969badc8f1229797571"].indexOf(ExView.tools.md5.hex_md5('Keep Out!' + localStorage.ExView_InitJSON + 'Keep Out!')) == -1`

稍微解释一下，`1b22289d656182b24547f307c9d368b7`和`552bc26417cb2969badc8f1229797571`都是经过`ExView.tools.md5.hex_md5`的加密而成的md5哈希值，但是具体方法并不清楚，因为没有开源，而`localStorage.ExView_InitJSON`则是用户本地所输入的车牌号，`indexOf()`函数返回`-1`时只可能是在无法找到匹配项才能实现，从而抛出异常并重置。

所以，根据这样的推断，`ExView.tools.md5.hex_md5('Keep Out!RIY6p9BKeep Out!')`符合md5哈希值即可，其中`RIY6p9B`为R开头加6位大小写+数字的字符串，蛐蛐五百亿次尝试便可获得真正的车牌，价值10块钱。。

----------

开发者送给我了个车牌。。真是谢谢啦。
开发者用的是[GitHub Page](https://ghostgzt.github.io/ExView/driver161217.jsonp)的服务，而我并不是很熟悉。
在过程中少了一些逆向思维，我还是太年轻了。

----------

为了看起来不那么年轻，我打算拆一个车牌。
通过加载自己源的jsonp，我知道这样可以实现简单的调试，于是我尝试获取更多信息，使用了一些[语法](https://github.com/ghostgzt/ExView/blob/master/src/README.md)。
```js
ExView.fw.alert(localStorage.ExView_InitJSON)
```
返回的是带引号的车牌`"RIkcxgj"`
然后验证赠送的车牌。
```js
ExView.fw.alert(ExView.tools.md5.hex_md5('Keep Out!"车牌"Keep Out!'))
```
返回`1b22289d656182b24547f307c9d368b7`
看得出来这个值跟市面上的md5加密一样，没有盐。

[短网址](http://360app.ft12.com/index.html)
[MD5](http://www.cmd5.com/hash.aspx)
