# 训练日志展示

模型开始训练后下面会开始打印日志：

![&#x6A21;&#x578B;&#x5F00;&#x59CB;&#x8BAD;&#x7EC3;&#x7684;&#x65E5;&#x5FD7;](../../.gitbook/assets/image%20%2823%29.png)

训练结束后会打印SUMMARY情况，报告最佳模型的情况，并对改模型进行保存：

![&#x6A21;&#x578B;&#x7ED3;&#x675F;&#x8BAD;&#x7EC3;&#x7684;&#x65E5;&#x5FD7;](../../.gitbook/assets/image%20%2815%29.png)

训练结束后当前目录会出现一个HypTuner\_log文件夹，里面的classifier\_test\_mode\_0文件夹名称是我们之前用name定义的，该文件夹内部存放了.pkl文件，其中model\_0\_2.pkl即训练处结果最优的模型。

![&#x8FD0;&#x884C;&#x5B8C;&#x6BD5;&#x751F;&#x6210;&#x6587;&#x4EF6;](../../.gitbook/assets/image%20%283%29.png)



