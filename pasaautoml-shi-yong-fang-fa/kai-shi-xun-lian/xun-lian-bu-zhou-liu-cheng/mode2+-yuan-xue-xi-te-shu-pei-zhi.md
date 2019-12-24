# Mode2+元学习特殊配置

mode2下使用Metalearning是指在使用mode2时，根据数据集的元特征来推断该数据集的来源，并在离线数据集仓库中匹配最为相似的数据集，以提取该数据集的运行信息（Q-Table）作为本次自动化机器学习任务中强化阶段的初始Q-Table，以此来加速强化学习阶段的收敛过程。**其调用过程与mode2除了设置use\_meta\_learning=True外完全相同。**

![](../../../.gitbook/assets/image%20%288%29.png)

![mode2+&#x5143;&#x5B66;&#x4E60;&#x6838;&#x5FC3;&#x8C03;&#x7528;&#x6D41;&#x7A0B;](../../../.gitbook/assets/image%20%2853%29.png)

