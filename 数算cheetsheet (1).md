# 数算

Ascii表 chr(0),ord('A')

65-A
97-a

dijiaslkdj

（dijkstra，每次都会输错）

    import heapq
    
    def dijkstra(s,mat,e):#s,e分别是起点和终点.起点s=(0,x0,y0),终点e=(xe,ye).
        MAXN=float('inf')
        weight=[[MAXN]*len(mat[0]) for _ in range(len(mat))]
        q=[s]
        weight[s[1]][s[2]]=0
        d=[(-1,0),(1,0),(0,-1),(0,1)]
    #开始bfs
    while q:
        w,x,y=heapq.heappop(q)
      
        #先处理到终点的情况
        if (x,y)==e:
            return weight[x][y]
      
        #然后探路
        for dx,dy in d:
            nx,ny=x+dx,y+dy
            if 0<=nx<len(mat) and 0<=ny<len(mat[0]):#不用not in visited
                new_w=weight[x][y]+______#这段填上点(x,y)到(nx,ny)的权重
                if new_w<weight[nx][ny]:
                    weight[nx][ny]=new_w
                    heapq.heappush(q,(new_w,nx,ny))
    
    return -1

欧拉筛



```
def euler(n):
    origen=set(range(2,n+1))
    primes=[]
    not_primes=[1]*(n+2)
    for i in origen:
        if not_primes[i]:
            primes.append(i)
        for j in primes:
            if i*j > n+1:
                break
            not_primes[i*j]=0
            if i%j==0:
                break
    return not_primes

not_primes=euler(1000000)
```


深拷贝

  from copy import deepcopy
a=[1,2,3]
b=a[:]
c=[[1,2],[2,3],[3,4]]
d=c[:]
e=deepcopy(c)

dfs

```
def dfs(x,y):
    d=[(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]
    for dx,dy in d:
        nx,ny=x+dx,y+dy
        if 0<=nx<len(mat) and 0<=ny<len(mat[0]):
            if mat[nx][ny]==1:
                mat[x][y]=0
                dfs(nx,ny)
                mat[x][y]=1
```



bfs

```
from collections import deque
def bfs(x0,y0,mat):
    q=deque([(x0,y0)])
    d=[(-1,0),(1,0),(0,1),(0,-1)]
    v=set()
    while q:#确定当前位置
        x,y=q.popleft()
        v.add((x,y))
        for dx,dy in d:#从当前位置开始探路
            nx,ny=x+dx,y+dy
            if 0<=nx<len(mat) and 0<=ny<len(mat[0]) and mat[nx][ny]==1:
                q.append((nx,ny))
```





二分

```
lo,hi=0,len(a)
while lo < hi:
	mid = (lo + hi) // 2
    if a[mid] < x:
        lo = mid + 1
    else:
        hi = mid
```



保留小数
"{:.2f}".format(number)






mergesort



```
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    # 分割
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    # 合并
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0

    # 合并两个有序数组
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    # 剩余元素
    result.extend(left[i:])
    result.extend(right[j:])
    return result

# 示例用法
data = [4, 2, 7, 1, 3]
sorted_data = merge_sort(data)
print("归并排序结果：", sorted_data)

```



# 数算部分

计概部分直接沿用去年的cheetsheet

主要拣了几个自己感觉比较重要/记不住的代码贴上去了

由于数算比较简单，投入的时间也不少，机考没用到cheetsheet

下面的代码，权当回顾这半年的学习



1 oop入门

令人印象最深刻的永远是第一次见到

去年在力扣做题，由于不知道什么是

class Solution:

​	def function_name(self,data:List):

以为要input和print，结果完全跑不动



oop比较喜欢的一点是，可以自己定义类方法，比如我们可以自己定义一个矩阵，定义+，*，也可以重写

```
__eq__
__str__
__lt__
__mul__
```

这些原有的方法

当然就是如果每个程序都要写一次那太麻烦了。不过我们也才刚入门，所定义的类不过Node，Graph





第一次看见链表：

```
class Node:

	def __init__(self,key,next=None):

		self.key=key

		self.next=next
```

一时无法理解

链表+字典

```
if key not in dic:
	node=Node(key)
	dic[key]=node
```

解决了链表知道key找节点要o(n)的问题

最震撼的反转链表

```
def reverselinkedlist(head):
	if not head or not head.next:
		return head
	dum=reverselinkedlist(head.next)
	head.next.next=head
	head.next=None
	return dum
	#此时dum是头部，head是尾部
```

快慢指针

```
fast=slow=head
while fast.next and fast.next.next:
	fast=fast.next.next
	slow=slow.next
```

相交链表判断相交节点

具体代码忘了，思路是这样

```
a=ahead
b=bhead
while a and b:
	a=a.next
	b=b.next
if not a:
	a=bhead
elif not b:
	b=ahead
while a and b:
	a=a.next
	b=b.next
if not a:
	a=bhead
elif not b:
	b=ahead
while a!=b:
	a=a.next
	b=b.next
最后如果两个链表有交，a=b=交点，反之都是None
```

然后是双链表

不过是比链表多一个self.prev

下面以双链表一例展示链表优势区间：

频繁地插入删除

首先使用dic建立key与node的映射，下面省略

```
def insert(index,key):
	newnode=dic[key]
	if not index:
		head.prev=newnode
		newnode.next=head
		head=nownode
		return
	node=dic[index]
	if node.next:
		node.next.prev=None
		newnode.next=node.next
	node.next=newnode
	newnode.prev=node
def del(key):
	pre=key.prev
	nxt=key.next
	if pre:
		pre.next=nxt
	if nxt:
		nxt.prev=pre
	

```





最喜欢的树

定义非常简单

```
class Node:
	def __init__(self,val):
		self.val=val
		self.left=None
		self.right=None
```



树的遍历用到递归

比如可以定义__iter(self)__

（别的班课件里的，顺便了解了一下yield）

```
class Node:
	def __init__(self,val):
		self.val=val
		self.left=None
		self.right=None
	def __iter__(self):
		if self.left:
			for i in self.left:
				yield i
		yield self.val
		if self.right:
			for i in self.right:
				yield i

```

```
def preorder(root):
	if not node:
		return []
	res=[root.val]
	return res+preorder(root.left)+preorder(right)
```

层序遍历

```
res=[]
cur=[root]
while cur:
	nxt=[]
	for i in cur:
		res.append(i.val)
		if i.left:
			nxt.append(i.left)
		if i.right:
			nxt.append(i.right)
	cur=nxt
```



像一些经典的求树高度，节点个数，最大最小值这种都可以递归进行

写代码时一个可以帮助理清思路的想法就是，知道有一个给出根节点即可得到目标的函数f，然后确定f(root)和f(root.children)的递推关系式。随后只需写好递推关系和初始条件即可。这一点和dp的情况很像。

有的时候实在不好想递推关系，不妨从简单的情况写起，写着写着就知道要做什么了

建树

```
def build(pre,inorder):
    if not pre:
        return
    root=Node(pre[0])
    ind=inorder.index(pre[0])
    root.left=build(pre[1:ind+1],inorder[:ind])
    root.right=build(pre[ind+1:],inorder[ind+1:])
    return root
```

堆的实现

知识联系在一起的感觉很好

```
def push(root,val):
	if not root:
		root=Node(val)
	if root.val>=val:
		push(root.left,val)
	else:
		push(root.right,val)
def pop(root):
	if root.left:
		pop(root.left)
	else:
		val=root.val
		root=root.right
		return val		
```

是的，空堆pop会报错（但是没写报什么错）



还有很多，如搜索树，huffman编码树，太久没用，目前还在记忆的角落暂存，期待与他们下次重逢（说起来树上手也快）



还是写一个huffman编码树吧，当时只是向gpt问过，自己第一次写

两条性质：

1，只存放在叶子节点：a不能是b的前缀

2，权重*高度最小

```
import heapq
weights=[xxxxxx]
arr=[]
for i in weights:
	heapq.heappush((i,Node(i)))
while len(arr)>=2:
	a,na=heapq.heappop(arr)
	b,nb=heapq.heappop(arr)
	node=Node(a+b)
	node.left=na
	node.right=nb
	heapq.heappush((a+b,node))
root=arr[0][1]
```

唉前辈们太厉害了，每次复现这些代码都不得不感叹其简洁高效



树的部分能想到的差不多了（都是些非常基本的东西），接下来是图

```
class vertex:
	def __init__(self,key):
		self.key=key
		self.neighbor={}
		#接下来还可以按需加入入度之类的东西
class graph:
	def __init__(self):
		self.vertices={}
	def add_vertex(self,key):
		if key not in self.vertices:
			self.vertices[key]=vertex(key)
	def add_edge(v1,v2,w):
		self.add_vertex(v1)
		self.add_vertex(v2)
		self.vertices[v1].neighbor[v2]=w
		self.vertices[v2].neighbor[v1]=w
```



图的遍历：bfs，dfs，和矩阵情形类似（矩阵不过是相邻边有连线的点阵罢了，也是图）

说到矩阵，还有邻接矩阵，但是在图比较稀疏的时候效率较低，占用空间也有点大。毕竟要得到一个点的neighbor要从1到n遍历一遍

下面是一些常用算法

并查集

```
class disjointset:
	def __init__(self,n):
		self.parent=list(range(n))
	def find(self,x):
		if x!=self.parent[x]:
			self.parent[x]=self.find(self.parent[x])
		return self.parent[x]
	def merge(self,x,y):
		rx=self.find(x)
		ry=self.find(y)
		if rx!=ry:
			self.parent[rx]=ry
```

效率真的很高！



拓扑排序

```
#假设u是点，neighbor的value是点，visited是defaultdict,neighbor是出边
def dfs(u):
	if visited[u]==2:
		print('成环')
		return
	if visited[u]==1:
		return
	visited[u]=2
	for i in u.neighbors.values():
		dfs(i)
	res.append(u)
	visited[u]=1
最后把res倒过来就是排序了。而且可以检测是否有环
```



kahn 

可以用于拓扑排序，还能检测拓扑排序方式是否唯一

```
#g 是图，vertex有入读ins，出边neighbors
def kahn(g,n):
	cur=[]
	c=0
	res=[]
	for i in g.vertices.values():
		if i.ins==0:
			cur.append(i)
	while cur:
		if len(cur)>1:
			print('拓扑排序方式不唯一')
		p=cur.pop(-1)
		for i in p.neighbors.values():
			i.ins-=1
			if i.ins==0:
			cur.append(i)
		c+=1
		res.append(i.key)
	if c<n:
		print('成环，无法排序')
		return
	return res
```



然后是最小生成树的算法，个人倾向以下方法

```
#首先建立并查集djs，dists是排序好的距离列表,dist[i]=(u,v,distance)
for u,v,d in dists:
	if djs.find(u)!=djs.find(v):
		djs.merge(u,v)
		dist+=d
#dist初始为0，最后得到总距离
```



dijkstra

上学期写过

感觉是最常用的了（

主要还是heap，在此就省略了



可以处理负权边的bellmanford

```
#cost[u]是到u最少花费，u.neighbor[v]是u到v花费
for i in range(k):
	newcost=defaultdict(lambda: float('inf'))
	for j in cost.keys():
		for h in j.neighbor.keys():
			newcost[h]=min(cost[h],cost[j]+j.neighbor[h])
	cost=newcost
#如果要检测负权环，若有n个节点，需要先循环n次，最后再来一次
newcost=defaultdict(lambda: float('inf'))
	for j in cost.keys():
		for h in j.neighbor.keys():
			newcost[h]=min(cost[h],cost[j]+j.neighbor[h])
for i in cost.keys():
	if cost[i]>newcost[i]:
		print('存在负权环')
		
```



SCC

不太记得了。比较难



然后是一些比较好用的算法



滑动窗口/双指针：想好什么时候左+，什么时候右+，非常快



binary

非常经典实用，通常要最小/最大/最小化最大/最大化最小，在数据量比较大的时候会用到一般情况o(n)的滑动窗口用不了，o（n^2)超时，就要请出binary来压时间了，logn的含金量！

不过话说回来，与老师推荐的用法不同，个人最习惯的还是这样

```
l=0
r=n
ans=0
while l<=r:
	mid=(l+r)//2
	if check(mid):
		ans=mid
		l=mid+1
	else:
		r=mid-1	
```



KMP

核心是next数组，即第i位前缀与后缀匹配的长度

双指针，i在short上，j在long上，成功i,j+=1,失败j不动，i回退（找short[0:new_i]==short[i-new_i : i]),即new_i=next[i-1]



如果要细说的话，还有好多，真的说不完

但是我的大脑内存已经满了

不妨直接使用索引[2025spring-cs201/20250526_dsa_mindmap.md at main · GMyhf/2025spring-cs201](https://github.com/GMyhf/2025spring-cs201/blob/main/20250526_dsa_mindmap.md)

前面整理的是我本学期使用最多，感受最深的代码，还有一些，或是由于时间太久记不清，或是比较易得不需放在这里。也许下次用到的时候，那时的我会想起曾经学计概&数算的时光罢。
