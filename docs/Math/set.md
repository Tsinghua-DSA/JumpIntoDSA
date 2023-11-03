# 集合

有人说，集合论有深刻的意义，可以看作是数学大厦的基础。不过在数据结构课程中，只需要掌握集合作为一种简易通行的书写符号的含义。

下面的简单描述，仅用于学习过高中数学集合论的同学快速回忆起相关知识，不适合没听说过集合论的人初次学习。如果没听说过集合论，建议找一本高中数学教材进行学习。

“一个集合”代表一类元素的整体, 集合内不存在重复元素, 元素也不存在顺序。

例如“自然数集”包括0、1、2、3等等无限个自然数。我们就说每个自然数都**属于**自然数集。

再比如，集合$\{1,2,3,4,5\}$包括 1, 2, 3, 4, 5这五个整数，我们就说1,2,3,4,5都**属于** $\{1,2,3,4,5\}$这个集合。

“属于”是集合论中最基本的一个概念，表示某个**元素** 和 **集合** 之间的关系。因此，有一个符号专门代表“属于”。

例如 $1 \in \{1,2,3,4,5\}$ 中间的符号就代表“属于”，左边的数字 1 属于右边的集合。

也有一个符号代表“不属于”:

$6 \notin \{1,2,3,4,5\}$ , 左边的数字 6 不属于右边的集合。

集合的元素不一定只是整数, 我们可以对我们所关心的任何一类对象, 定义一些集合, 比如说一个二维平面上的点集: $\{(1,1),(0,1),(1,0)\}$

不包含任何元素的集合称为空集，它也有一个专门的符号: $\emptyset$

对于集合A和集合B, 如果A的每个元素都是B的元素, 那么我们称A是B的子集。记作 $A \subseteq B$, 或者 $B \supseteq A$

一个集合如果有$n$个元素，那么它有$2^n$个不同的子集。其中一个是空集，一个是它本身。

两个集合之间可以求交集、并集、差集。一个集合被它的子集求差集时，也叫做补集。还有一种运算叫做集合的对称差。

交集: 如果一个元素同时属于集合A和集合B，那么这个元素属于集合A和集合B的交集。

并集: 如果一个元素属于集合A或集合B，那么这个元素属于集合A和集合B的并集。

差集: 如果一个元素属于集合A但不属于集合B，那么这个元素属于集合A减去集合B的差集。

对称差: 如果一个元素属于集合A和集合B的并集，但不属于它们的交集，那么这个元素属于集合A和集合B的对称差。