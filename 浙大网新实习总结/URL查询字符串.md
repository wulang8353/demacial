### 根据URL中的查询字符串得到某个键名对应的值

#### 知识点：encodeURI、encodeURIComponet、url地址、ajax

#### 需求简介

通过js，拿到当前页面的url中的查询字符串的键及其对应的值

#### 解决办法

····

function getUrlAttribute(name)

{

      //location.search是从当前URL的?号开始的字符串，即查询字符串

      var query = (location.search.length &gt; 0 ? location.search.substring(1) : null);

       if(null!=query)

       {

           var args = new Object( );

           var pairs = query.split("&");

           for(var i = 0; i &lt; pairs.length; i++)

           {

                var pos = pairs[i].indexOf("=");

                if (pos == -1)  continue;

                var argname = pairs[i].substring(0,pos);

                var value = pairs[i].substring(pos+1);

                value = decodeURIComponent(value); // 由于url地址是经过编码的，所以需要解码

                args[argname] = value;

           }

          //根据键名获取值

           return args[name];

      }

      return null;

}


!\[\]\(/浙大网新实习总结/imgs/url编码.png\)

#### encodeURI Vs encodeURIComponent

**为什么要编码**

*  url包含中文，浏览器无法识别，不能跳转

`var str1 = "`[`http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526`](http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526)`";`

`window.location.href=str1; // 无法跳转`

* 传递参数

Http协议中参数的传输是\`\`key=value\`\`这种键值对的形式，如果要传多个参数就需要用“&”符号对键值对进行分割

例如：\`\`"?name1=value1&name2=value2"\`\`这样在服务端在收到这种字符串的时候，会用“&”分割出每一个参数，然后再用“=”来分割出参数值

现在有这样一个问题：

如果参数值中就包含=或&这种特殊字符的时候该怎么办？比如说\`\`name1=value1\`\`,其中value1的值是“va&lu=e1”字符串，那么实际在传输过程中就会变成这样\`\`name1=va&lu=e1\`\`。

本意是就只有一个键值对，但是服务端会解析成两个键值对，这样就产生了奇异，如何解决上述问题带来的歧义呢？解决的办法就是对参数进行URL编码

URL编码只是简单的在特殊字符的各个字节前加上%，例如，我们对上述会产生奇异的字符进行URL编码后结果：\`\`name1=va%26lu%3D\`\`（%26='&'，%30='='）

这样服务端会把紧跟在“%”后的字节当成普通的字节，就是不会把它当成各个参数或键值对的分隔符。

**encodeURI\(\)**

> 该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码 \`\`- \_ . ! ~ \* ' \( \)\`\` 。
>
> 该方法的目的是对 URI 进行完整的编码，URI 中具有特殊含义的 ASCII 标点符号，如\`\`：;/?:@&=+$,\# \`\`也不会进行转码。



`document.write(encodeURI("[`[`http://www.w3school.com.cn")+](http://www.w3school.com.cn")+`](http://www.w3school.com.cn"%29+]%28http://www.w3school.com.cn"%29+)`) "<br />")`

`document.write(encodeURI("`[`http://www.w3school.com.cn/My`](http://www.w3school.com.cn/My)` first/"))`

`document.write(encodeURI(",/?:@&=+$#"))`

`var str1 = encodeURI("[`[`http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526"](http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526")\`](http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526"]%28http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526"%29\)`);`

`window.location.href=str1;`

`//只转换域名后面的部分，包括中文，并且对,/?:@&=+$#不处理。`

[`http://www.w3school.com.cn`](http://www.w3school.com.cn)

[`http://www.w3school.com.cn/My first/`](http://www.w3school.com.cn/My first/)

`,/?:@&=+$#`

[`http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526`](http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526)



**encodeURIComponent\(\)**

> 该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码\`\`- \_ . ! ~ \* ' \( \)\`\`
>
> 其他它字符（包括\`\`：;/?:@&=+$,\# \`\`这些用于分隔 URI 组件的标点符号）都会进行转码



`document.write(encodeURIComponent("[`[`http://www.w3school.com.cn")](http://www.w3school.com.cn"))`](http://www.w3school.com.cn"%29]%28http://www.w3school.com.cn"%29%29\)`)`

`document.write(encodeURIComponent("`[`http://www.w3school.com.cn/p`](http://www.w3school.com.cn/p)"))`

`document.write(encodeURIComponent(",/?:@&=+$#"))`

`var str1 = encodeURIComponent("[`[`http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526"](http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526")\`](http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526"]%28http://www.baidu.com/s?wd=中国&rsv_spt=1&issp=1&rsv_bp=0&ie=utf-8&tn=baiduhome_pg&inputT=2526"%29\)`);`

`window.location.href=str1;`

`// 对比`

`http%3A%2F%2Fwww.w3school.com.cn`

`http%3A%2F%2Fwww.w3school.com.cn%2Fp%201%2F`

`%2C%2F%3F%3A%40%26%3D%2B%24%23`

`http%3A%2F%2Fwww.baidu.com%2Fs%3Fwd%3D%E4%B8%AD%E5%9B%BD%26rsv_spt%3D1%26issp%3D1%26rsv_bp%3D0%26ie%3Dutf-8%26tn%3Dbaiduhome_pg%26inputT%3D2526`



**总结**

（1）encodeURI的目的是对 URI 进行整体编码，故适用于url跳转时使用；

（2）encodeURIComponent会把部分uri关键字符（如：&等）也进行转换，故适用于传递参数时使用。

