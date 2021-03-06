1. B树
2. B+树
3. AVL树与红黑树

红黑树的5个性质：
1. 每个结点为红色或黑色
2. 根结点为黑色
3. 每个叶结点(NIL)为黑色
4. 如果一个结点为红色，则两个子结点都是黑色
5. 对于每个结点，从该结点到叶结点的简单路径上，包含相同数目的黑色结点

红黑树新插入的结点需要标记为红色的原因：
如果插入的是黑色结点，会引起整棵树黑高（黑色节点的高度）的变化，违反性质5，如果重标记，需要调整整颗树，不划算。插入红色节点，仅在父节点也为红色节点时才需要调整。
这里假设出现了结点与父节点都为红色，则有三种情况（不包含对称情况）：
1. 当前结点的叔结点为红色
   > 这种情况说明祖父结点两边的黑高相同，直接将祖父结点标记为红色，并且将叔结点和父节点标记为黑色即可。此时祖父结点所在的子树满足性质1,3,4,5
2. 当前结点的叔结点为黑色，且当前结点是一个右子节点
3. 当前结点的叔结点为黑色，且当前结点是一个左子结点
   > 2,3可以看作是类似情况，2通过在父结点上做一次左旋，即为情况3，这次左旋仅仅是两种情况的转换。在情况3中，将父节点标记为黑色，祖父结点标记为红色，并在祖父结点上做一次右旋，右旋之前，祖父结点左右子树的黑高是相同的，因此需要将祖父结点标记为红色，将父节点标记为黑色，此时右旋之后，父节点的左右子树黑高依然相同。同时没有破坏其他性质

在整个树都不存在结点与父节点同时为红色的情况下，将根节点标记为黑色，整个调整结束。

红黑树对平衡要求不高，相比AVL树，更适合插入，删除较多的操作，若主要用于查找，且树在构造时倾斜严重的话，AVL树有更好的查找性能。