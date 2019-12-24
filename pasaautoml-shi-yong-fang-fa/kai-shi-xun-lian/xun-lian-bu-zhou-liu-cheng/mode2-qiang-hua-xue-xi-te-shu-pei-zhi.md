# Mode2强化学习特殊配置

mode2下不固定模型和参数，并增加了预处理和特征工程使用强化学习进行训练。**mode2和mode1的调用类似，只是在初始化的时候对传入参数有所区别**，比如传入max\_cat\_preprocessing和max\_others\_preprocessing为3，即一个Pipeline使用多少种类别特征预处理算法和通用特征预处理算法，最大为3。

![mode2&#x6838;&#x5FC3;&#x8C03;&#x7528;&#x6D41;&#x7A0B;](../../../.gitbook/assets/4.png)



