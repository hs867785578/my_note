
less对应“<”运算符，

greater对应">"运算符。

- 最近学习STL，发现STL默认都是使用()比较的，默认比较使用less（即'<'运算符），如sort(a,a+n)，默认将数组按照递增的顺序来排序（前面的元素<后面的嘛)。
- 但是优先队列的源码比较奇特，虽然按道理使用less比较应该默认是小根堆（即堆顶元素最小），但是priority_queue<int, vector\<int\>, less\<int\> >却是大根堆，要记住priority_queue和sort是相反的。

## less对应大顶堆，根因在于其底层实现
less：左数小于右数时，返回true，否则返回false。

在堆的调整过程中，对于大顶堆，如果当前插入的节点值大于其父节点，那么就应该向上调整。其父节点索引小于当前插入节点的索引，也就是父节点是左数，插入节点是右值，可以看到，左数小于右数时，要向上调整，也就是Compare函数应该返回true，正好是less。


## sort
```cpp
template <class T> struct greater {
  bool operator() (const T& x, const T& y) const {return x>y;}
  typedef T first_argument_type;
  typedef T second_argument_type;
  typedef bool result_type;
};

template <class T> struct less {
  bool operator() (const T& x, const T& y) const {return x<y;}
  typedef T first_argument_type;
  typedef T second_argument_type;
  typedef bool result_type;
};

#include<iostream>
#include<vector>
#include<iterator>
#include<functional>
#include<algorithm>
using namespace std;
 
int main()
{
	int A[]={1,4,3,7,10};
	const int N=sizeof(A)/sizeof(int);
	vector<int> vec(A,A+N);
	ostream_iterator<int> output(cout," ");
	cout<<"Vector vec contains:";
	copy(vec.begin(),vec.end(),output);
	
	cout<<"\nAfter greater<int>():";
	sort(vec.begin(),vec.end(),greater<int>());//内置类型从大到小 
	copy(vec.begin(),vec.end(),output);
	
	cout<<"\nAfter less<int>():";
	sort(vec.begin(),vec.end(),less<int>());   //内置类型小大到大 
	copy(vec.begin(),vec.end(),output);
	
	return 0;
}
```

## priority_queue(与sort相反)

```cpp

struct cmp{
	 
	    bool  operator ()  ( int a, int b){
	   	      return a > b;
	   } 
}; 
struct cmp1{
	   bool operator ()( int a ,int b ){
	   	   return a < b;
	   }

priority_queue< int > q;// 默认q.top()是从大到小。
priority_queue < int , vector<int> ,less<int> > q;//q.top()从大到小 大顶堆
priority_queue < int , vector<int>, greater<int> > q; //q.top()从小到大，需要vector，小顶堆
priority_queue < int , vector<int> , cmp1  > q;//q.top()从大到小，需要vector
priority_queue < int , vector<int> , cmp > q;//q.top()从小到大，需要vector

```