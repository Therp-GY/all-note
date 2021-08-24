

 

# 关于

- 时时回顾
- 记需要使用到的数据结构、初始化、过程
- 一个逻辑一个逻辑细细分析

# 链表

## 链表

- bug1

  head建立时没有malloc，即head指针指向一个空的值，那么head->i就会内存错误

  同时，insert又将一个新的表项插在head前面，此时整个链表混乱

  **修改前**

  ```c
  	List head;
  	head->i = 1;
  	head->next = NULL;
  	for(int i = 1; i<=n; i++){
  		insert(head, i,i);
  	}
  ```

  **修改后**

  ```c
  	List head = (List)malloc(sizeof(node));
  	head->i = 1;
  	head->next = NULL;
  	for(int i = 2; i<=n; i++){
  		insert(head, i,i);
  	}
  ```

## 链表实现队列

![1590824389529](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1590824389529.png)

- bug1

  malloc会将struct内部的指针也赋值，因此内部指针部位NULL，应该主动给它们赋值为NULL

  ```
  typedef struct node{
  	int element;
  	struct node* next;	
  }node;
  
  typedef node* ptrNode;
  typedef struct qnode{
  	ptrNode front;
  	ptrNode reer;
  }qnode;
  
  typedef qnode* ptrQnode; 
  int main(){
  	ptrQnode ptrQ = (ptrQnode)malloc(sizeof(qnode));
  	//	bug修改
  	ptrQ->front = NULL;
  	ptrQ->reer = NULL;
  	//	bug修改
  	....
  	}
  ```

# 树

![1590829138753](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1590829138753.png)

![1590829207551](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1590829207551.png)

**一般树都可表示为二叉树**（儿子兄弟表示法）

![1590829599615](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1590829599615.png)

## 数组存储

![1590830023430](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1590830023430.png)

## 链表存储

![1590829990317](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1590829990317.png)

## 静态链表存储

![1590937952943](C:\Users\GY\AppData\Roaming\Typora\typora-user-images\1590937952943.png)



# 栈、队

## 循环队列实现【数组】

- bug1

  循环队列front所指向的那个位置是空的，即出队时`(++front)%MAXSIZE`再`return queue[front]`

  reer指向的是队尾元素，入队时`(++reer)%MAXSIZE`再`queue[reer] = ele`

  判断已满`(reer+1)%MAXSIZE == front`

  

  

# 指针

# 题目：链表

## 1032链表

- bug1赋值问题

  修改结构体数组中得值不可以 Node nd  = nodeList[i]; nd.flag = true;

  修改得时nd而不是数组中得结构

- bug2格式问题

  输出为5个数字，则为printf("%05d\n",i)

- bug3边界问题，如输入节点为0

- 数据结构

  `typedef struct node{`
  	`char letter;`
  	`int next;`  
  	`bool flag;`
  `}Node;`

  `Node nodeList[MAX];`  nodelist[i] 表示地址为i的node

## 1052链表

- bug1

  注意链表可能有多条，因此我们仍然得先建立链表数组，从表头开始遍历筛选所选链表上得节点并置flag=true

- bug2

  给得头节点地址可能不存在，此时直接输出0 -1

- bug3

  sort(起始地址，结束地址下一个地址，比较函数)

  其中比较函数必须遵循严格弱排序，即两者相等时取false
  
- 数据结构

  `typedef struct node{`
  	`int address;`
  	`int key;`
  	`int next;`
  	`bool flag;`
  `}node;`

  `node nodelist[MAX];`   和1032同样方式存储链表

## 1074（链表倒转）

- 思路

  list存储方法如1052所示，先将node存入数组，然后从首地址开始遍历，将遍历到的节点存入output中，然后计算有几个k段，将每个k段倒转push入result中去，最后result顺序输出即可。

- 数据结构

  `node list[100010];`
  `vector<node> output, result;`

## 2019秋季第二题

- 思路

  使用Node node[]存储所有节点以及其信息，分别从两个起始地址开始遍历并压入list1、list2中，这两个list也可表示链表，list[i]的下一个节点就是list[i+1]。然后长的链表2个短的1个这个规律压入result中，result就是我们想要的链表，这时result[i]的下一个节点地址是result[i+1].address，即可将整个链表打印出来

- 数据结构

  `struct Node {`
  	`int address, data, next;`
  `} node[100001];`  存储链表初始信息

  `vector<Node> list1, list2, result;` list1、list2存储长短链表，它们由递归node[]得到。result由list1和list2压入得到

  

# 题目：树

## 1043二叉搜索树---前序转后序

- 思路

  前序转后序的关键，是定位一个二叉树左子树的根、尾节点与右子树的根、尾节点。每次递归，传入二叉树的根和尾，从根**往右遍历**找到第一个大于根的节点即为右子树的根节点，右子树尾节点即为该二叉树尾；从右**向左遍历**找到第一个小与根的节点即为左子树的尾节点，左子树根节点为根节点右移一位3
  
- bug

  不能从左到右找到第一个比根节点大的节点，然后只依靠它分左右子树，这样**无论**给定什么样的序列我们都会输出YES，因为我们每一次都会将所有子序列都给统计进去，就不能依靠post.size判断是否YES。

  正确做法见思路，这样的话如果NO，则post.size会大于n，因为会重复计算
  
- 数据结构

  `vector<int> pre,post;` 

  `void getPost(int root, int tail);`递归算法，前序转后序。

## 1119二叉树---前序后序转中序

- 前序后序转中序，出现不确定的树的情况为根节点下只有一颗子树，那么该子树可为左或者右子树

- 根据前序就可确定左子树的根节点（左根 = 根+1）后序可确定右字数的根节点（右根 = 根-1），结合两个就可确定前序和后序中左根和右根的位置，递归即可求解

- bug1

  for循环中当 `pre[j1]==post[j2]` 时退出，即条件为`j1<=tail1 && pre[j1]!=post[j2]` 注意是 &&！！
  
- 数据结构

  ``vector<int> pre;`
  `vector<int> post;`
  `vector<int> in;` 

  `void getIn(int root1, int tail1, int root2, int tail2);`递归函数

## 1115 二叉排序树-求最后两层节点个数

- 思路

  先通过链表建立二叉树，注意build函数写法（递归返回该根节点指针），然后使用dfs统计每一个深度的节点数，注意maxDepth是真实最深深度+1。	
  
- 数据结构

  `typedef struct node{`
  	`int v;`
  	`struct node *leftNode;`
  	`struct node *rightNode;`
  `}node;`

  `typedef node* ptrNode;` 

  `vector<int> nums(1000, 0);`统计每个深度的个数

  `ptrNode build(ptrNode root, int value);`建树函数，返回根节点地址	


## 1127（中后序求前序+bfs）

- 思路

  1. 中序后序求前序
2. **用一个二元数组tree表示该二叉树**，tree\[i][0] = val表示post[i]的左孩子是post[val]，tree\[i][1] = val表示post[i]的右孩子是post[val]。即**将二叉树用后序遍历序列和它的偏移值来表示**。
  3. 层序遍历：bfs遍历tree，遍历方法为使用队列，将根节点入队，然后只要队列不为空，则弹出第一个节点，并向队列放入该节点的左右子节点，同时将弹出节点记录到`vector<int> result[]`中。`result[depth]`表示第depth层所有节点
4. 将result依次输出，当result\[depth].size() == 0 则不管，当depth%2==1则逆序输出

- 数据结构

  `vector<int> in,post,result[35];`分别是中序、后序、输出数组

  `int tree[35][2],root;` tree存储二叉树的结构，`tree[i][0]`对应`post[i]`的左子树在post上的偏移 ， tree[i][1]对应post[i]的右子树在post上的偏移  ，root二叉树根节点的偏移  **为什么要这么存？因为给出的数字大小不知道，可能int无法存下，因此`tree[i][j]`中的i不能存数字大小，可能越界**

  `struct node{`
  	`int index, depth;`
  `};` 方便层序遍历

  `queue<node> q;` 层序遍历。 q.push()、q.front()、q.pop()

## 1138（前中序求后序）

- 思路
  1. 前序中序求后序
  2. 不必建立后序数组，设置全局变量isFind，找到第一个 preHead == preTail就可以打印并设置isFind = 1，以后所有的递归都直接return即可

## 1147（层序求后序）

- 思路
  1. 层序转后序，由于此处层序为满二叉树，因此直接递归遍历。**层序顺序存储在vector中，用level[index]表示某节点，则其子节点为level[index\*2+1]和level[index*2+2]**。先递归两个子节点再将根节点push入vector即可
  2. **liu神思路**：另max=1 min=1 直接遍历所有节点，观察其父节点index/2是否大于子节点（min=0）或者小于子节点（max=0）。最后min与max中一个为1则存在，全为0则Not Heap
  
- 数据结构

  `vector<int> level[101]`  

  level[i]对应

## 1130（中序遍历，中缀表达式）

- 思路    

  建树，中序遍历即可

- 数据结构

  `int tree[25][2];`  树的子节点表达
  `vector<string> val;` 树的值

## 1110（层序遍历，判断是否为完全二叉树）

- 思路

  从root开始层序遍历节点，一旦节点无左右子节点，则isLeaf置1，接下来若再有节点有子节点，则不为完全二叉树

  bug：内存超限！！！很有可能出现无限循环，多多造数据验证！！！

  bug：初始化！！isCBT为初始化为1造成问题

- 数据结构

  `tree[21][2]`存储节点信息

  `isRoot[21]`查找root

## 总结

- 关于前序后序中序相互转换实际上就是给定**一维数组**，找到根节点和左右子树节点不断递归，递归过程中将节点有序填入输出的数组
- 链表建树，关键是build要返回根节点指针
- 数组建树，常常使用二维数组，tree\[][0]和tree\[][1]表示左右子节点，这样知道根节点后就可以进行bfs、dfs遍历
- 数组建树，还可以在原来后序（或先序中序）数组上也对应使用二维数组建树(**其实是避免节点数字过大造成二维数组越界**)
- dfs：根据我们表达树的数据结构分别递归访问左右子树
- bfs：根节点入队q.push()，while循环判断队列是否为空，不为空则首元素出队q.front()、q.pop()，并将其两个子节点入队q.push()

## 1143（找共同祖先）

- 思路

  不需要建树。由于给出的为二叉排序树且为先序序列，节点a和节点b的祖先的值一定介于两者之间，且一定是先序序列从左到右遍历第一个满足条件的节点，因此问题迎刃而解

- 数据结构

  `vector<int> pre;`存储先序序列
  `unordered_map<int, int> exist;`判断节点是否出现

# 题目：图

## 1106（dfs）

- 思路

  说是图，实际上是树，因为没有供给环路。建立好树以后，dfs遍历从根节点出发的每条路径，并记录最小深度，**若等于则amount++，若小于则amout置1**。（**dfs遍历的停止条件都是人为设置的，要么根据visit数组访问过就结束，要么到叶节点就结束**）

- 数据结构

  `vector<int> map[100001]` ，map[i]表示i节点的所有子邻接节点	

  map[i] 对应i号的所有子节点

## 1103 （dfs）

- 思路

  > 分析：dfs深度优先搜索。先把i从0开始所有的i的p次方的值存储在v[i]中，直到v[i] > n为止。然后深度优先搜索，记录当前正在相加的index（即v[i]的i的值），当前的总和tempSum，当前K的总个数tempK，以及因为题目中要求输出因子的和最大的那个，所以保存一个facSum为当前因子的和，让它和maxFacSum比较，如果比maxFacSum大就更新maxFacSum和要求的ans数组的值。
  >
  > 在ans数组里面存储因子的序列，tempAns为当前深度优先遍历而来的序列，从v[i]的最后一个index开始一直到index == 1，因为这样才能保证ans和tempAns数组里面保存的是从大到小的因子的顺序。一开始maxFacSum == -1，如果dfs后maxFacSum并没有被更新，还是-1，那么就输出Impossible，否则输出答案。
  > ————————————————
  > 版权声明：本文为CSDN博主「柳婼」的原创文章
  > 原文链接：https://blog.csdn.net/liuchuo/article/details/52493390

  为什么是深度遍历，可以看作一个v[i]的全连通图（每两个节点都连通，方向**从大到小**），同时每个节点还有自环。**从大到小**对每个节点进行dfs。

  同时要进行剪枝，不然开销大且无限递归

  > 分析：这道题考的是DFS+剪枝，我认为主要剪枝的地方有三个：
  > 1. tempK==K但是tempSum!=n的时候需要剪枝
  > 2. 在枚举的时候，按顺序枚举，上界或者下界可进行剪枝
  > 3. 当且仅当tempSum + v[index] <= n时，进行下一层的DFS，而不要进入下一层DFS发现不满足条件再返回，这样开销会比较大
  > ————————————————
  > 版权声明：本文为CSDN博主「柳婼」的原创文章
  > 原文链接：https://blog.csdn.net/liuchuo/article/details/52493390

- 数据结构

  `vector<int> v,ans,tempAns;` 

  `int K,P,N,maxFacSum = -1;`
  
  这里没有用vector<int> map[]存储邻接点因为以及默认全联通，因此只要用for循环遍历比该节点小的节点即是它所有的邻接点

## 1091（bfs）

- 思路

  L层从上到下叠起来，构成一个三位数组，每一层M * N。每一个像素上下左右前后相邻就可以看作连接起来。需要使用bfs，因为dfs会爆栈。
  
- 数据结构

  `int v[65][1290][130];` 存储三位数组
  `int visit[65][1290][130];` 访问数组

  关于遍历三维数组上下左右可以使用

  ```c
  int L[6]={1,0,0,-1,0,0};
  int M[6]={0,1,0,0,-1,0};
  int N[6]={0,0,1,0,0,-1};
  for(int i=0; i<6; i++){
      l + L[i]
      m + M[i]
      n + N[i]  
      ....
  }
  ```

## 1013（连通分量）

- 思路

  dfs遍历，找到强连通分量个数n，那么只需要	添加n-1个桥即可让所有节点全连通

- 数据结构

  `int n,m,k,count;`
  `vector<int> v[1005],concern;`  
  `int visit[1005];`

  v存储邻接节点（我选择双向连通，即给一条边，两个节点都要添加邻接节点）

## 1111（dij最短路径）

- 思路

  在找len最短路的时候，更新len数组时同时维护一个weight数组`weight[v] = timeMap[u][v] + weight[u];`，当更新len出现`lenMap[u][v] + len[u] == len[v] && timeMap[u][v] + weight[u] < weight[v]`时，就需要更新lenPre[v]=u

  查找time最短路时做相同操作

- 数据结构

  `map[][]` 记录整张图，边以及权重

  `len[]` dj过程中记录原点到各个点的距离，len[i]=d表示原点到i点的距离为d。开头需置为**正无穷**

  `lenPre[]` dj过程中记录最短路径顺序的序列，lenPre[i]=val表示到i点的上一个节点编号为val。开头需置为每个节点**自己**

  `visit[]` dj过程中是否以及作为最近节点。开头需置为**0**
  
  `vector<int> lenPath`最后递归遍历lenPre获得的输出路径

## 1003(dij + 最短路径个数)

- 思路

  最短路径求法和1111相同，（我不小心建了pre数组，其实不需要）。

  同时维护了一个people数组，处理和1111也相同

  - **求最短路径个数**

    维护一个num[]数组，num[i]=n表示到i节点的路径个数为n。dij开始前预处理时，注意将num[st] = 1；

    一旦 `dis[u] + lenMap[u][v] < dis[v]`则num[v] = num[u]

    一旦`dis[u] + lenMap[u][v] == dis[v]`则num[v] = num[v] + num[u]

## 1018(dij + 遍历所有最短路径)

- 思路

  坑：后一个站多出来的车不可以向前面补足

  得到disPre之后，dfs遍历它，其中每个back只能补给后面的节点。

  bug：**数据初始化！！ minBack和minNeed记得初始化为inf**

- 数据结构

  与普通dij不同的是使用`vector<int> disPre[510]`存储，因为最短路径不一定为一条，因此其前序节点可能也为多个

  一旦 `dis[u] + lenMap[u][v] < dis[v]`则disPre[v].clear(); disPre[v].push_back(u)

  一旦`dis[u] + lenMap[u][v] == dis[v]`则disPre[v].push_back(u)
  
  - 遍历最短路径
  
    `void dfs(int index){`
  
    ​	`tempPath.push_back(index);`
  
    ​	`if(index == 0){遍历tempPath}`
  
    ​	`else {遍历disPre[index],递归dfs}`
  
    ​	`tempPath.pop_back();`
  
    `}`

## 1122（判断是否哈密顿回路）

- 思路

  只要清楚哈密顿回路定义即可

  > 哈密顿图:图G的一个回路,若它通过图的每一个节点一次,且仅一次,就是哈密顿回路.存在哈密顿回路的图就是哈密顿图.哈密顿图就是从一点出发,经过所有的必须且只能一次,最终回到起点的路径.图中有的边可以不经过,但是不会有边被经过两次.

  根据定义判断就行

- 数据结构

  `hamMap[][]`

  `visit[]`

  `vector<int> path`

## 1126（判断是否欧拉图）

- 思路

  首先判断是否连通，否则Non；

  然后判断每个节点度的个数，若有0个奇数的度则Eu，若2则Semi，其它则Non

- 数据结构

  `vector<int> v[510];` `visit[510];`  `visitNum`存储无权图邻接点用于bfs，visit用于bfs中单次访问节点，visitNum判断是否访问了所有节点

  `int degree[510]`记录每个节点的度

## 1150（旅行商问题）

- 思路

  与哈密顿回路类似，注意三种类型路径的判断。

  回路+访问所有节点：TS      回路+访问所有节点+简单：TSS      其它 ：NOT TS 

- 数据结构

  `int tsMap[210][210], visit[210];`因为是有权图，因此使用二维数组

## 1118（并查集、统计集合个数）

- 思路

  为所有鸟的编号建立并查集。并查集中每个集合对应一个父节点，若两个节点的父节点相同，则属于同一个并查集。将每一行输入的节点加入一个并查集下，具体加入方法就是两两节点Union

- 数据结构

  `int fa[]`记录每个节点的父节点，为并查集的**基本结构**，初始化为**fa[i]=i**

  `int cnt[]`每个节点拥有的子节点个数（包括自己），初始化为fa[i]=0。得到cnt的方法：fa建立完毕后遍历fa，cnt[fa[i]]++即可

  `int exist[]`用于区分该节点有无访问

- 重要函数

  `int findFather(int x)` 找到x的父节点，并且在查找过程中将所有子节点直接连到根上。

  ```c
  int findFather(int x){
  	int a = x;
  	while(fa[x] != x){
  		x = fa[x];
  	}
  	while(a != fa[a]){
  		int z = a;
  		a = fa[a];
  		fa[z] = x;
  	}
  	return a;
  }
  ```

  `void Union(int a, int b)`将a和b节点连到同一个并查集上

  ```c
  void Union(int a, int b){
  	int fA = findFather(a);
  	int fB = findFather(b);
  	if(fA != fB)fa[fA] = fB;
  }
  ```

## 1107（并查集、统计集合个数、打印每个集合数目）

- 思路

  需要将**人**建立并查集，而**兴趣**则建立一个hobb[]数组，初始化全为0。当编号为i的人有编号为h的兴趣，若hobb[h] == 0则hobb[h] = i。并且将i与fa[hobb[h]]归入同一个并查集，这样就能依靠兴趣建立人的并查集。

- 数据结构

  `int fa[1010]`并查集基础结构，初始fa[i]=i

  `int cntNum`记录并查集合个数，初始为0。如何得到：遍历fa，当fa[i] == i 则 cntNum++

  `vector<int> isRoot;`记录每个节点的子节点个数（包括自身），初始化isRoot.resize(n+1) 且最后输出时调用sort()函数从大到小排序，然后从0到cntNum-1遍历输出即可	

## 1146（判断是否为拓扑排序）

- 思路

  由于给定的序列用到了所有的点且**只用一次**，且给定的图**一定**有拓扑排序，因此可以简化。只需要每次判定该顶点是否入度为0，并将所有邻接点入度-1即可。

- 数据结构

  `vector<int>  mapTop[maxn];` 由于是无权图，所以直接使用vector存储邻接点

  `int indegree[maxn];` 存储每个节点的入度

  ` int temp[maxn];` 每次需要`memcpy(temp, indegree, maxn * sizeof(int)); ` 注意在#include<string.h>下

## 总结

- 关于建图

  无权重图可直接vector<int> map[]  map[i]表示i节点的所有邻接点

  有权重图可map\[][] map\[i][j]代表i到j的距离

- 关于dfs、bfs遍历

  常用于无权图/有权图求出来的所有最短路径

  dfs就是递归遍历该节点的所有邻接点

  bfs就是将根节点放入队列，然后从队列中取节点并将该节点邻接点再放入队列直到空

- 并查集

  并查集相当于森林，每棵树的根节点就是该集合的标识。在Union过程中往树中添加节点，此时该树是随意建立的（根节点也会变化），而findFather会将树的深度变为2，所有子节点都直接连到根节点上。



# 题目：堆

- 概念

  最大堆是一个完全二叉树（因此可以用数组存储）。最大堆：节点比它的两个子节点都大。

  - 堆建立

    将一个任意的序列填入数组，则已经形成一棵完全二叉树但不满足堆。然后从倒数第二层开始遍历到根节点，对遍历的每个节点和其所有的子节点通过上下节点调换满足堆的要求，从而使整个树满足堆的要求

    ```c
    //	r为要调整的节点序号，n为尾节点序号
    void adjustDown(int r, int n){	
    	int child = 2r;
    	int val = a[r];
    	while(child <= n){
    		if(child < n && a[child] > a[child+1]) child++;//选取孩子较小的那个
    		if(val < a[child]) break; //若父节点最小，则不需要调整
    		a[child/2] = a[child];
    		child *= 2; //到下一层
    	}
    	a[child/2] = val;
    }
    ```

  - 堆插入

    将元素插在尾部，再和其父节点不断调换直到满足

  - 堆删除

    根节点弹出，将尾节点调到根节点上，再通过堆建立的调整方式得到

  - 堆排序

    将根节点和尾节点调换，并将堆大小-1，再次生成堆。循环n-1次

## 1098(插入排序和堆排序)

- 思路

  判断方法：堆排序在中间过程中根节点一定大于子节点，而插入排序则开头一定满足顺序，即可区分。

  坑点：插入排序的下一个排列需要找到第一个不满足顺序的元素，然后将它加入到已排序列中再排序得到新的序列。

- 数据结构

  `vector<int> origin,partial;` 对堆、插入序列的操作都直接做在partial上

# 题目：动态规划

## 1045（最长不下降子序列）

- 思路

  我的思路：`color[10010]`存储所有颜色，`favor[205]`存储喜欢的颜色序列，用二维数组标识`ans[i][j]`表示从(`color[1]`->`color[j]`)的颜色序列上最多满足(`favor[1]`->`favor[i]`)喜爱序列多少个。递推式子：`ans[i][j]= max(1+ans[i][j-1]仅当color[j] == favor[i], ans[i-1][j],ans[i][j-1])`

  liu神思路：用book[i] = j表示i颜色的下标为j，然后将颜色序列所有颜色用下标来表示，如favor：{2 3 1 5 6} color：{2 2 4 1 5 5 6 3 1 1 5 6}，那么color就被表示为{1 1 3 4 4 5 2 3 3 4 5}，问题转化为求该式的最长不下降子序列。可以用一维数组dp

  ```c
     // num 转化后颜色数组长度   dp[] dp[i]指0~i子序列中包含a[i]的最大序列。  a[]颜色数组（{1 1 3 4 4 5 2 3 3 4 5}）  
  	for(int i = 0; i < num; i++) {
          dp[i] = 1;
          for(int j = 0; j < i; j++)
              if(a[i] >= a[j])
                  dp[i] = max(dp[i], dp[j] + 1);
          maxn = max(dp[i], maxn);
      }
  
  ```


## 1007(最大连续子序列和)

- 思路

  为啥这是dp？...

  如果有多段连续子序列，那么它们必定不是相邻的，不然可以连起来变成更大的。因此，我们需要leftindex、rightindex来记录最大子序列的左右偏移。同时我们用tempindex记录临时的左偏移，而临时右偏移就是我们目前读到的数字，临时左偏移变化的情况就是当我们临时子序列的和小于0，那么这一段子序列就是完全无用的。

- 数据结构

  `vector<int> v(n);` 记录总序列

  `int leftindex = 0, rightindex = n-1, sum = -1, temp = 0, tempindex = 0;`分别为最大子序列左右偏移，最大和，临时子序列和，临时子序列左偏移

## 1068（01背包）

- 思路

  实际就是求最大容量为M的01背包问题，唯一区别是若最大容量不为M则No Solution。

  要保证输出的是最小序列，就要将硬币从大到小排序，w[i]对应的是第i大的硬币。取值时也是能取到就取而不取上一序列的。

# 题目：贪心

## 1033（加油站）

- 思路

  设到达某个站后有两种情况：1. 在后面可到达站中有比现在更便宜的，选择**第一个更便宜**的站，只要加可以到达那个站的油即可 2. 没有更便宜的站，那么就把油加满

- 数据结构

  `struct gasStation{`
  	`double price,dis;`
  `};`  加油站

  `vector<gasStation> g	;`存储加油站，并用sort函数将其按照dis从小到大排序，并在尾部插入gasStation{0.0，d}作为终点

# 题目：STL

## 1153（unordered_map，string）

- stl操作

  - unordered_map

    - 定义：`unordered_map<string, int> m;`

    - 添加：数组形式插入 m[s]++ 即可  ||  m.insert(make_pair<string,int>（s,1）)

    - 遍历

      for(auto it:m){string a =  it.first;  int b = it.second}

      注意it类型为pair而不是迭代器，若为迭代器则需要it->first因为是指针

  - string

    - printf：需要 s.c_str转化为char[]类型

    - 截取string：s.substr(a,b)  a为要截取的起始偏移，b为长度

  - vector

    - 设置大小：resize(n)，初始化一个大小为n的vector
    - scanf：vector有数组的操作，因此可以&v[i]输入即可

- 数据结构

  1，2两种问题类型直接遍历即可

  `typedef struct node{`
  	`string t;`
  	`int value;`
  `}node;`   // 可存储 (名字-成绩 )|| (考场-人数)	

  `vector<node> all;` 存储所有的 名字-成绩

  `unordered_map<string, int> m;` 遍历所有学生找到符合日期的，m[all[i].t.substr(1,3)]++ 即对应考场号学生数加1

## 1129（set）

- stl操作

  - set操作

    set和map都是使用树存储，不同在于set键和值是同一个，因此set不能修改值。

    - 定义：`set<> s;`

    - 添加：`s.insert(【<>内定义的类型】)` 
    - 删除：`s.erase(iterator)` 
    - 遍历访问：`for(auto it = s.begin(); it != s.end(); it++){ it....}`  **it迭代器是指向元素的指针**
    - 查找：`auto it = s.find(元素)`

- 数据结构

  `struct node{
  	int num;
  	int freq;
   	bool operator < (const node &a) const{
   		return (freq != a.freq)? freq > a.freq: num < a.num;
   	} 
  };` **注意重写 < 函数！！！**

  `int freq[50001];` freq[i]表示数字i的访问次数 

  `set<node> s;` s是所有数字的set，且已经自动排序

## 1121（set）

- 思路

  用couple数组存储对端，couple[i]表示id为i的对象。

  遍历所有给出的id并压入in中，若id是有couple的且第一次遇到，则couple[id]置-2，若id的couple[id]为-2，则将couple[id]和couple[couple[id]]全置-3。

  再遍历in，只要couple[i]不为-3就输出

- 数据结构

  `vector<int> in;`
  `set<int> out;`
  `int couple[100000];   //初始化全为-1  -1 无couple  -2couple中一个存在  -3双双存在`

# 题目：基础题

## 1104找规律

- 思路

  纯找规律，循环需要控制在一层循环内，注意溢出，使用long double，cin输入，printf(“%.2Lf”)输出

## 1140回文串（字符串处理）

- 思路

  照做即可

  注意：string既可以 string + “字符串” 也可以 string + “字符”

  to_string函数可将整形转化为字符串 需要 `#include<iostream>`

## 1117

- 思路

  将骑行天数从小到大排序，再从后向前遍历，天数从1开始递增，而maxMile = m[i] - 1，直到天数大于maxMile就停止。再取两者最小值就是E

## 1136（字符串处理、倒转、大数加）

- 思路

  判断a为回文串：a与其倒转a‘完全相等。开一个10次循环，内部进行回文串判断和相加操作。

  bug：！！！由于位数可到999，因此是大数相加，需要定义加法函数！！！

- **字符串处理**

  `#include<string>`  才可使用string

  `#include<algorithm>`可使用reverse()函数实现字符串倒转，

  ​											格式：`reverse(ans.begin(), ans.end());`

  输出格式`cout << norm << " + " << rev <<" = " << sum << endl;` 

## 1105（螺旋矩阵）

- 思路

  ```c
  	//	m_横向宽度 n_纵向宽度
  	for(int i=0, j=0; m_ > 0 && n_ > 0;){
  		for(int t = j; t <= j+n_-1; t++){
  			ans[i][t] = input[index--];
  		}
  		for(int t = i+1; t<= i+m_-1; t++){
  			ans[t][j+n_-1] = input[index--];
  		}
  		for(int t = j+n_-2; t>=j && m_> 1; t--){
  			ans[i+m_-1][t] = input[index--];
  		}
  		for(int t = i+m_-2; t>=i+1 && n_ > 1; t--){ // 注意 n_ > 1！！不然会造成n_==1时重复
  			ans[t][j] = input[index--];
  		}
  		i++;
  		j++;
  		m_ -= 2;
  		n_ -= 2;
  	}
  ```

## 1112（字符串处理）

- 思路

  输入origin，遍历origin，一旦有重复k的字符就使judge[origin[i]] = 1, 若未重复k次则judge[origin[i]] = -1，且一旦为-1则无法再更改judge。

  再一次遍历origin，一旦judge[origin[i]] = 1则只输出重复字符的第一个，其它情况则照常输出。输出存储到change中。

  遍历origin，一旦judge[origin[i]] = 1则输出字符，并使judge[origin[i]] = 0以防止重复输出。 

- 数据结构

  `string origin, change;`origin存储初始字符串，change存储修改后字符串

  `int judge[500];` judge[i]用于判断i字符是否为重复字符，1为重复，-1为非重复

## 1108（字符串处理）

- 思路

  bug：#define中处理函数  `#define isnum(a) ((a >= '0')&&(a <= '9'))`一定要加最外面的小括号！，不然 !isnum(a) 就是 ! a>='0' && a<='9'

- **字符串处理**

  `#include<string>`
  `#include<iostream>`

  `stod() ：`字符串转浮点型 

## 1044（尺取法）

- 思路

  何时可以使用尺取法？当我们直到左右端点时，可以明确下一步如何推进端点。

  使用left和right记录我们选取的区间，**初始化**为left=1，right=1，sum = v[1]. 当sum>m时，left++；当sum<=m时，right++；**当right < left时，right ++**。当right > n时退出。

  原本准备用两个循环，第一次查可以取到的差的最小值，第二次再用尺取法取，但其实不需要。在一次循环中，当找到差值更小的值，将之前存的答案clear，然后重新存入即可。

- 数据结构

  `vector<int> v;`存储Di
  `vector<pair<int, int>> ans;`存储答案。注意`#include<map>`

  - pair用法：pair实际上是泛型的struct结构
    - 初始化： pair<int，int> a(1，2)；
    - 新建pair：make_pair(1，2)；
    - 取值  a.first   a.second