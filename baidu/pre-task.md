## 第一关



![task1](/assets/IFEE/pre-task/task1.png)

基本上这类题目的标准答案都会是一种比较具有对称性的图片。

## 第二关

使用开发者工具，键盘按下`F12`或者同时按下`Crtl+shift+i`

![task2-1](/assets/IFEE/pre-task/task2-1.png)

`Iy9CQzY3QTA4MzM1NkM4MzU5NEJDQzU3RUQxMzJEMkEwMjU2N0U0Qg==` 这是一段`Base64`编码。

**答案**：

使用控制台输入`atob("Iy9CQzY3QTA4MzM1NkM4MzU5NEJDQzU3RUQxMzJEMkEwMjU2N0U0Qg==")`。

得到结果`"#/BC67A083356C83594BCC57ED132D2A02567E4B"`

![task2-1](/assets/IFEE/pre-task/task2-2.png)

- Base64编码

计算机中任何数据都是按`ascii`码存储的，而ascii码的128～255（特殊字符）之间的值是不可见字符。

在网络上交换数据时，由于不同设备对字符的处理方式不同，这样就会存在一个**问题**：不可见字符可能会被错误处理。

**解决方法**：

先将数据进行Base64编码，统统变成可见字符，最后进行解码。

**作用**：

保证数据的完整并且不用在传输过程中修改这些数据。Base64也被一些应用（包括使用MIME的电子邮件）和在XML中储存复杂的数据时使用。

在JavaScript中，有2个函数分别用来处理解码和编码Base64字符串

    - atob()
    - btoa()
    
`atob()`函数能够解码通过base-64编码的字符串数据。相反地，`atob()`函数能够从二进制数据“字符串”创建一个base-64编码的ASCII字符

## 第三关

**题目含义**：当前窗口的高度值 = 密码锁

**思路**：
思路一：将窗口的高度调整为521px 
思路二：将密码锁的值调整为窗口的高度值

1> 直接拖拉窗口高度微调至521px或者有点麻烦，这里推荐一个谷歌浏览器有的一个小功能 “device-toolbar”

![task3-1](/assets/IFEE/pre-task/task3-2.png)

“device-toolbar” ： 可以直接定义成iPone、iPad大小的分辨路。这里直接输入521即可

![task3-2](/assets/IFEE/pre-task/task3-1.png)

2> 在控制台输入`document.body.clientHeight` 可以得到内容高度。然后将密码锁改成得到的高度，即可

![task3-2](/assets/IFEE/pre-task/task3-3.png)

每个数字之间的高度为-40px，对照下表，将得到的高度一一对应后，将对应的top值填入到`n1、n2、n3`的top值中即可。
```
密码值 top值:
1 -40
2 -100
3 -160
4 -220
5 -280
6 -340
7 -400
8 -460
9 -520
```

## 第三关














