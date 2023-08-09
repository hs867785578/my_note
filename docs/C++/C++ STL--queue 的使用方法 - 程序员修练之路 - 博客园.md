#   [C++ STL--queue 的使用方法](https://www.cnblogs.com/hdk1993/p/5809180.html)

2、queue
queue 模板类的定义在<queue>头文件中。
与stack 模板类很相似，queue 模板类也需要两个模板参数，一个是元素类型，一个容器类
型，元素类型是必要的，容器类型是可选的，默认为deque 类型。
定义queue 对象的示例代码如下：
queue<int> q1;
queue<double> q2;
queue 的基本操作有：
入队，如例：q.push(x); 将x 接到队列的末端。
出队，如例：q.pop(); 弹出队列的第一个元素，注意，并不会返回被弹出元素的值。
访问队首元素，如例：q.front()，即最早被压入队列的元素。
访问队尾元素，如例：q.back()，即最后被压入队列的元素。
判断队列空，如例：q.empty()，当队列空时，返回true。
访问队列中的元素个数，如例：q.size()
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
`#include <cstdlib>`
`#include <iostream>`
`#include <queue>`
``
`using`  `namespace`  `std;`
``
`int`  `main()`
`{`
```int`  `e,n,m;`
```queue<``int``> q1;`
```for``(``int`  `i=0;i<10;i++)`
```q1.push(i);`
```if``(!q1.empty())`
```cout<<``"dui lie  bu kong\n"``;`
```n=q1.size();`
```cout<<n<<endl;`
```m=q1.back();`
```cout<<m<<endl;`
```for``(``int`  `j=0;j<n;j++)`
```{`
```e=q1.front();`
```cout<<e<<``" "``;`
```q1.pop();`
```}`
```cout<<endl;`
```if``(q1.empty())`
```cout<<``"dui lie  bu kong\n"``;`
```system``(``"PAUSE"``);`
```return`  `0;`
`}`
“过一个平凡无趣的人生实在太容易了，你可以不读书，不冒险，不运动，不写作，不外出，不折腾……但是，人生最后悔的事情就是：我本可以。”——陈素封。

分类: [c++](https://www.cnblogs.com/hdk1993/category/668938.html)

 [好文要顶](#)  [关注我](#)  [收藏该文](#)  [![icon_weibo_24.png](../_resources/icon_weibo_24.png)](#)  [![wechat.png](../_resources/wechat.png)](#)

 [![sample_face.gif](../_resources/sample_face.gif)](https://home.cnblogs.com/u/hdk1993/)

 [程序员修练之路](https://home.cnblogs.com/u/hdk1993/)
 [关注 - 0](https://home.cnblogs.com/u/hdk1993/followees/)
 [粉丝 - 91](https://home.cnblogs.com/u/hdk1993/followers/)

 [+加关注](#)

 6

 1

 [«](https://www.cnblogs.com/hdk1993/p/5809161.html) 上一篇： [c++stack容器介绍](https://www.cnblogs.com/hdk1993/p/5809161.html)

 [»](https://www.cnblogs.com/hdk1993/p/5811577.html) 下一篇： [C++ STL set和multiset的使用](https://www.cnblogs.com/hdk1993/p/5811577.html)

posted @ 2016-08-26 09:53 [程序员修练之路](https://www.cnblogs.com/hdk1993/)  阅读(106215)  评论(0) [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=5809180) [收藏](#) [举报](#)