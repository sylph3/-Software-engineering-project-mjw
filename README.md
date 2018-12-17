# -软件工程基础个人项目
生成数独终局 &amp; 求解数独终局

本次个人项目的需求可以分为两个主要部分：生成指定数量的不重复的数独终局以及读取文件内的数独终局，求解并将结果输出到文件sudoku.txt中。

【生成数独终局】

在生成数独矩阵时，左上角第一个数为（学号后两位相加）%9 + 1，这为唯一的一个硬性约束条件。
例如，对于以下数独终局，其形成中，从第二行开始，每行分别是第一行右移3、6、1、4、7、2、5、8列的结果。
（其中左上角第一个数为我的学号1120161752后两位相加模9+1，即为8）：

        8	9	1	2	3	4	5	6	7

        5	6	7	8	9	1	2	3	4

        2	3	4	5	6	7	8	9	1

        7	8	9	1	2	3	4	5	6

        4	5	6	7	8	9	1	2	3

        1	2	3	4	5	6	7	8	9

        6	7	8	9	1	2	3	4	5

        4	5	6	7	8	9	1	2	3

        9	1	2	3	4	5	6	7	8

按照同样的规律，可以获得8！即40320种不同的终局。

同时，如果任意交换2-3行、4-6行或7-9行三行的位置，可以使每一种原先的终局扩展为72个终局，得到最多2903040种不同的终局情况，即可以满足题目中1000000种情况的要求。

从这个规律中可以看出，其每三行内部之间平移的距离差都是3或者6，即保证了三行内每一行与前一行之间没有重复的可能性且一定包含1-9的全部数字，三乘三方格的规律被满足。同时，每行之间数值也都相互错开，即3、6、1、4、7、2、5、8序列中没有重复的数值，保证了每一列中数字的不重复。每一行的数字本身也为1-9不重复，故数独终局的全部条件都被满足。


据此分析后，就可以设计实现生成数独终局需求的代码了。

【求解数独】

在求解数独终局这个问题上，我想到的是使用回溯的方法，在每一个给定的题目中，根据空结点的排列顺序遍历数独的解，如果找不到合理的解时就进行回溯，直至当前解合理，再继续进行遍历，直到找到一个合理的数独解法为止。同时，为了降低程序的时间复杂度，应用一定的剪枝函数对其进行优化。

较为具体的过程如下：
首先，建立一个数组存放当前的求解题目矩阵。之后，面对矩阵中的一个需要填入的位置尝试性地放入一个数，如果其与当前数独状态不冲突，就继续考虑下一个位置，直到某一个位置找不到合理的数来放入的时候，进行回溯，在合理的情况下继续进行搜索，直到找到完整的可行解为止。其中，在放入数前利用剪枝函数提前评估当前数独矩阵状态，进行合理剪枝。
