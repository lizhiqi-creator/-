Color the Tree

这个思路的核心在于“证明某些集合是区间”.大致想法是用子节点的情况判断该节点的情况

1.题目叙述

https://pintia.cn/problem-sets/1365872079935995904/problems/1368397110042693632

2.定义

在下面的叙述中需要用到下面的定义：

（1）对树T的染色满足*染色规则：如果在对二叉搜索树T进行染色时，没有遵守“根节点为黑色“这条规则，但是遵守红黑树定义中，除此之外的三条规则，则称这个染色满足染色规则\*。

（2）元组（n,C,h）满足条件\*\*:对于节点n，颜色C，黑高h，如果对于以节点n为根的子树，存在一种满足\*染色规则的染色方法，使得节点n染为颜色C并且黑高为h，则称元组（n,C,h）满足条件\*\*

（3）集合n.B和n.R：对于节点n，有对应的集合n.B={h|(n,黑色,h)满足条件\*\*}和n.R={h|(n,红色,h)满足条件\*\*}

（4）区间：在下面叙述中所说的区间都不是通常数学意义上的区间，而是指连续不间断的整数构成的集合。

3.基本思路

基于以下两个简单的命题

命题3.1 对于节点n和黑高h，(n,红色,h)满足条件\*\*当且仅当对于节点n的两个子节点n->left,n->right，(n->left,黑色,h-1)和(n->right,黑色,h-1)均满足条件\*\*.

命题3.2 对于节点n和黑高h，(n,红色,h)满足条件\*\*当且仅当对于节点n的两个子节点n->left,n->right，以下四种情况至少一种成立：

（1）(n->left,黑色,h-1)和(n->right,黑色,h-1)均满足条件\*\*

（2）(n->left,红色,h)和(n->right,红色,h)均满足条件\*\*

（3）(n->left,红色,h)和(n->right,黑色,h-1)均满足条件\*\*

（4）(n->left,黑色,h-1)和(n->right,红色,h)均满足条件**

这两个命题很直接，证明略去。

根据这两个命题，我们可以从二叉搜索树的最底层开始，对每一个节点n判断n.B以及n.R是否为空，并且利用该节点的信息去判断其父节点的情况，**直到n为root时，若root.B不为空，则二叉搜索树能被染成红黑树，否则不行**。根据前面的两个命题，有：
$$
\begin{align}
n.R=&n->left.B\bigcap n->right.B+1\\
n.B=&(n->left.B\bigcap n->right.B+1)\bigcup \\
&(n->left.R\bigcap n->right.R)\bigcup\\
&((n->left.B+1)\bigcap n->right.R)\bigcup\\
&(n->left.R\bigcap (n->right.B+1))\bigcup
\end{align}.....................[1]
$$
这样判断n.B和n.R是否为空转换为判断其子节点的对应集合的某些运算后的结果是否为空，但是直接这样做时间复杂度太高了（计算任意两个集合的交与并的时间复杂度大于O(1)）而且集合记录起来也较为困难。然而，如果命题“对于每一个节点n，n.R和n.B为区间”成立（命题3.3），那么计算n.R和n.B的并和交就很简单了，只需要计算端点值即可，这时有：
$$
\begin{align}
n.rl=max\{&n.left->bl+1,n.left->bl+1\}\\
n.ru=min\{&n.left->bu+1,n.left->bu+1\}\\
n.bl=min\{&max(n.left->bl+1,n.right->bl+1),\\
&max(n.left->bl+1,n.right->rl),\\
&max(n.left->rl,n.right->bl+1),\\
&max(n.left->rl,n.right->rl)\}\\
n.bu=max\{&min(n.left->bu+1,n.right->bu+1),\\
&min(n.left->bu+1,n.right->ru),\\
&min(n.left->ru,n.right->bu+1)),\\
&min(n.left->ru,n.right->ru)\}\\
\end{align}............[2]
$$
注：在计算中需要考虑空集的情况，比如在计算n.bl时，如果$n->left.R=\empty$，那么n.bl中不是取四项的最小值，而是取两项的最小值，此时有$n.bl=min\{max(n.left->bl+1,n.right->bl+1),max(n.left$      $->bl+1,n.right->rl)\}$。下面给出并证明上面用到的命题3.3。

引理1：$n.R\in n.B$

证明：由[1]式直接得到，因为$n.R=n->left.B\bigcap n->right.B+1\in n.B$

引理2：$A\subseteq B$,且A和B都是区间，则$(A+1)\bigcup B$和$(A-1)\bigcup B$都是区间

证明：只证$(A-1)\bigcup B$，$(A+1)\bigcup B$类似。若$A=\empty$,则$(A-1)\bigcup B=\empty$是区间。若$A\neq\empty$，则设A=[s,t],B=[u,v],当$t-1\ge u$时，$(A-1)\bigcup B$是区间；当当$t-1<u$时,由于$t\ge s\ge u$,有t=u，即t-1=u-1，此时注意到我们对区间的定义是连续的整数，所以仍然有$(A-1)\bigcup B$是区间（因为s-1,...,t-1,u,...,v是连续整数）。

引理3：如果A,B,C是区间且$A\bigcup B$和$A\bigcup C$都是区间且A非空，则$A\bigcup B\bigcup C$是区间

证明：这是因为对于集合U和V来说，如果$U\bigcap V\neq\empty$,则$U\bigcup V$是区间。令U=$A\bigcup B$，V=$A\bigcup C$即证（此时$U\bigcap V=A\neq\empty$）

命题3.3 对于每一个节点n，n.R和n.B为区间

证明：采用结构数学归纳法证明。对于叶节点Nil来说，由于其不能染为红色，所以Nil.H为空集，Nil.B=[0,0].对于内部节点n，假设其子节点满足命题，下面来证明n满足命题。由于区间的交为区间，所以由[1]直接得到n.R是区间。下面证明n.B是区间。先考虑第一种情况，$(n->left.B\bigcap n->right.B)\neq\empty$。

首先注意到由引理1，$(n->left.R\bigcap n->right.R)\subseteq n->left.B\bigcap n->right.B$,从而由引理2，有$(n->left.B\bigcap n->right.B+1)\bigcup (n->left.R\bigcap n->right.R)$是区间。根据下面的运算：
$$
\begin{align}
&(n->left.B\bigcap n->right.B+1)\bigcup ((n->left.B+1)\bigcap n->right.R)\\
&=(n->left.B+1)\bigcap((n->right.B+1)\bigcup n->right.R)
\end{align}
$$
同时注意到由引理2和引理1，$(n->right.B+1)\bigcup n->right.R$是区间，且区间的交是区间，从而$(n->left.B\bigcap n->right.B+1)\bigcup((n->left.B+1)\bigcap n->right.R)$是区间。同理可得$(n->left.B\bigcap n->right.B+1)\bigcup(n->left.R\bigcap (n->right.B+1))$是区间,最后由引理3可得，n.B是区间（注意这是依赖于$(n->left.B\bigcap n->right.B)\neq\empty$的）。

再考虑第二种情况，$(n->left.B\bigcap n->right.B)=\empty$此时一定有$(n->left.R\bigcap n->right.R)=\empty$,从而现在只用证明$((n->left.B+1)\bigcap n->right.R)\bigcup(n->left.R\bigcap (n->right.B+1))$是区间，若$((n->left.B+1)\bigcap n->right.R)$与$(n->left.R\bigcap (n->right.B+1))$中有空集，则其并集显然是区间。现在考虑他们都不为空的情况。注意到$((n->left.B+1)\bigcap n->right.R)$与$(n->left.R\bigcap (n->right.B+1))$都不为空，那么一定有$((n->left.B+1)\bigcap n->right.B)$与$(n->left.B\bigcap (n->right.B+1))$中都不为空，这是与$(n->left.B\bigcap n->right.B)=\empty$矛盾的。从而不可能$((n->left.B+1)\bigcap n->right.R)$与$(n->left.R\bigcap (n->right.B+1))$都不为空。

综上，命题3.3得证。

4.实现过程

​	对于二叉搜索树中的每一个节点具有下面的数据结构：

​	struct Node{

​		int element;

​		int bl,bu,rl,ru;

​		pNode left,right;

​	}

​	其中pNode表示指向Node类型的指针。bl,bu,rl,ru的含义同第3部分。实现过程是利用子节点的bl,bu,rl,ru利用[2]式，计算该节点的bl,bu,rl,ru。对于nil节点，其bl=0，bu=0，rl与ru的定义使得rl>ru即可(表示区间为空)。

下面是程序的伪代码

Procedure judge(n) Begin

​	if n.left!=nil Begin

​		Call judge(n.left)

​	End

​	if n.right!=nil Begin

​		Call judge(n.right)

​	End

​	calculate n.bl,n.bu,n.rl,n.ru accoding to [2]

End

再次强调在[2]中，如果有空区间（空区间的特点是左端点大于右端点），应该先进行检测，并划掉计算式中相应的项。比如在计算n.bl时，如果$n->left.R=\empty$，那么n.bl中不是取四项的最小值，而是取两项的最小值，此时有$n.bl=min\{max(n.left->bl+1,n.right->bl+1),max(n.left$  $->bl+1,n.right->rl)\}$。

由于每个节点的计算花费常数时间，算法是O(N)的