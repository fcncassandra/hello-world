# 初始化参数介绍

PASA-AutoML支持分类、回归、聚类及深度学习超参调优等多种业务场景。用户可以通过创建Classifier、Regressor及Cluster对象快速开发自动化机器学习应用。 

```python
from automl.Clustering.cluster import Cluster
from automl.Regression.Regressor import Regressor
from automl.Classification.classifier import Classifier
```

Classifier、Regressor及Cluster实际上都继承了Estimator类。Estimator作为Classifier、Regressor及Cluster的基类，包含了一些共有的属性和方法，用户在初始化三个问题的时候都可以传入以下参数，其中重要参数已经在序号前用 **\*** 标出：

## 三个类不同的属性

{% hint style="info" %}
关于指标的详细说明参考sklearn：[https://scikit-learn.org/stable/modules/classes.html\#sklearn-metrics-metrics](https://scikit-learn.org/stable/modules/classes.html#sklearn-metrics-metrics)
{% endhint %}

{% tabs %}
{% tab title="Classifier" %}
**\*1 classification\_mode : int , default : 0**

* 0 二分类问题
* 1 多分类问题
* 2 多标签问题

**2 classification\_algorithm : None or dict, default : None**

分类算法，传入None或dict类型数据，默认为None，即使用默认的分类算法集；如果传入dict，其格式要为{'算法名称':算法实例}。

**\*3 evaluation\_method : str, default : 'accuracy\_score'**

分类问题的评估方法，默认是'accuracy\_score'，可以传入： `"accuracy_score", "auc", "average_precision_score", "balanced_accuracy_score", "brier_score_loss", "classification_report", "cohen_kappa_score", "confusion_matrix", "f1_score", "fbeta_score", "hamming_loss", "hinge_loss", "jaccard_similarity_score", "log_loss", "matthews_corrcoef", "precision_recall_curve", "precision_recall_fscore_support", "precision_score", "recall_score", "roc_auc_score", "roc_curve", "zero_one_loss", "coverage_error", "label_ranking_average_precision_score", "label_ranking_loss", "top_1_accuracy", "top_1_precision"。`
{% endtab %}

{% tab title="Regressor" %}
**1 regression\_algorithm : None or dict, default : None**

回归算法，传入None或dict类型数据，默认为None，即使用默认的回归算法集；如果传入dict，其格式要为{'算法名称':算法实例}。

**\*2 evaluation\_method : str, default : 'r2\_score'**

回归问题的评估方法，默认是'r2\_score'，可以传入： `"explained_variance_score", "mean_absolute_error", "mean_squared_error", "mean_squared_log_error", "median_absolute_error", "r2_score"。`
{% endtab %}

{% tab title="Cluster" %}
**1 clustering\_algorithm\_algorithm : None or dict, default : None**

聚类算法，传入None或dict类型数据，默认为None，即使用默认的聚类算法集；如果传入dict，其格式要为{'算法名称':算法实例}。

**\*2 evaluation\_method : str, default : 'silhouette\_score'**

聚类问题的评估方法，默认是'silhouette\_score'，可以传入： `"adjusted_mutual_info_score", "adjusted_rand_score", "completeness_score", "fowlkes_mallows_score", "homogeneity_score", "mutual_info_score", "normalized_mutual_info_score", "v_measure_score", "silhouette_score", "calinski_harabaz_score", "davies_bouldin_score"`
{% endtab %}
{% endtabs %}

## **三个类共有的属性**

**\*1 total\_timebudget : int, default : 3600**

自动化机器学习运行总时长，单位为秒，默认为3600秒。

**\*2 per\_run\_timebudget : int, default : 30**

每个算法评估过程的运行时长，单位为秒，默认为30秒。

**3 ml\_memory\_limit : int, default : 3072**

算法的内存限制，单位为MB，默认为3072MB。

**4 ensemble\_size : int, default : 1**

集成模型中子模型的个数，默认为1，则不使用集成学习。**聚类算法中无法使用集成学习。**

**\*5 validation\_strategy : str, default : 'cv'**

模型验证策略，可以传入'cv'和'holdout'，默认是'cv'。

**\*6 validation\_strategy\_args : int or float, default : 3 or 0.67**

模型验证策略参数，配合5 validation\_strategy使用，当验证策略为'cv'时，默认为3；当验证策略为'holdout'时，默认为0.67。

**\*7 automl\_mode : int,  default : 0**

自动化机器学习的模式，可以传入0，1，2三种模式，默认是第0种模式，三种模式分别代表：

* 0 固定模型，进行超参数调优；
* 1 选取最优模型，并进行超参调优，需要模型的评估指标在\[0 ,1\]内，且需要指标越大表示模型效果越好；
* 2 增加数据预处理和特征选取算法，自动进行Pipeline搜索。

**8 backend : str, default : 'sklearn'**

自动化机器学习后台设置，可以传入'sklearn'，'tensorflow'，'xgboost'，'lightgbm'四种参数，默认是'sklearn'。

**9 sample\_percent : float, default : 0.2**

当9 backend为'tensorflow'时设置，选取部分数据进行超参调优，默认为0.2。

**10 datapreprocess\_algorithm : None or dict, default : None**

数据预处理算法，传入None或dict类型数据，默认为None，即不使用数据预处理算法；如果传入dict，其格式要为{'算法名称':算法实例}，该dict会传入estimator中的others。

`{'rescaling': [], 'categorical_encoding': [], 'others': []}`

**11 feature\_select\_algorithm : None or dict, default : None**

特征选择算法，传入None或dict类型数据，默认为None，即不使用特征选择算法；如果传入dict，其格式要为{'算法名称':算法实例}。

**12 selfDefinedAlgorithmSet : bool, default : False**

算法集是否被用户指定，默认为False。

**\*13 predict\_method : str, default : 'predict'**

在分类算法的预测方法，可以传入'predict'，'predict\_proba'，默认是'predict'，即预测标签值；如果输入'predict\_proba'则返回预测每个标签的概率。

**14 bandit\_policy : str, default : 'rsh' or 'epsilon-greedy'**

设置bandit规则，**回归和分类问题默认是'rsh'，聚类问题默认是'epsilon-greedy'**。可以传入rsh'， 'epsilon\_greedy'， 'ucb1'， 'bayesian ucb'， 'thompson sampling'， 'softmax'。

**15 reject\_rate : float, default : 0.5**

拒绝率，默认值为0.5。

**16 q\_learning\_rate : float, default : 0.1**

Q-learning算法的学习率，默认为0.1。

**17 q\_epsilon : float, default : 0.5**

Q-learning的探索率，默认为0.5。

**18 q\_init\_method : str, default : 'uniform'**

Q-learning中q\_table的初始化方法，可以传入'uniform'，'random'默认为'uniform'，

**19 q\_gamma : float, default : 0.5**

Q-learning的折扣率，默认为0.5。

**20 n\_iterations : int, default : 1e9**

超参调优HyperBand的迭代次数，默认为1亿次，即不断迭代直到时间用完。

**\*21 use\_HypTuner : bool, default : False**

是否使用超参调优，默认为False，不使用。

**22 time\_budget\_pipeline\_HypTuner : float, default : 2.0**

当23 use\_HypTuner为True时，pipline搜索和超参调优的时间比例，默认为2.0，即2:1，意味着2/3的时间用来做pipeline搜索，1/3的时间用来做超参调优。

**23 budget\_type : str, default : 'datapoints'**

超参调优的预算类型，可以传入'datapoints'，'time'。默认是'datapoints'，意味着以数据采样作为预算类型。

**24 min\_budget : int, default : 27**

HypTuner中每一个摇臂运行的最小预算，默认是27，与max\_budget配合使用。

**25 max\_budget : int, default : 729**

HypTuner中每一个摇臂运行的最大预算，默认是729，与min\_budget配合使用。

> 当max\_budget=729，min\_budget=27时，由于默认eta=3，所以min\_budget对应的预算为$ log\_3\(27\)/log\_3\(729\) = 3/6$，即使用50%的数据采样进行训练。由于每个SH将预算增大eta=3倍，故而此后逐步采用$ log\_3\(81\)/log\_3\(729\) = 4/6$的数据采样，$ log\_3\(243\)/log\_3\(729\) = 5/6$，最后使用$ log\_3\(729\)/log\_3\(729\) = 6/6 = 100%$

**26 name : str, default : None**

标识HypTuner运行的名字，默认是None。

**27 n\_workers : int, default : 1**

用来进行超参调优的进程数，默认为1，目前只支持为1。

**28 save\_trajectory : bool, default : True**

是否保存超参调优的性能轨迹，默认为True。

**\*29 log\_folder : None or str, default : None**

日志文件存放的文件夹路径，默认为None，即在当前目录创建'my\_log'文件夹存放日志文件。

**\*30 output\_folder : None or str, default : None**

模型输出的文件夹路径，默认为None，即在当前目录创建'my\_output'文件夹存放模型文件。

**31 verbose : bool, default : True**

是否显示运行日志，默认为True，即显示。

**32 log\_level : str, default : 'INFO'**

日志的等级，可以传入'DEBUG'，'INFO'，'ERROR'，'CRITICAL'四种等级，默认为'INFO'。

**33 categorical\_columns : None or list, default : None**

指定哪些列为类别特征，可以传入None或list类型的数据，默认是None，即不指定任何列为类别数据；当输入list类型数据，如\[0, 1 ,2\]，则表示指定输入特征的第0列，第1列，第2列为类别特征。

**34 max\_cat\_preprocessing : int, default : 1**

最后使用多少种类别特征预处理算法，默认为1。

**35 max\_others\_preprocessing : int, default : 1**

最后使用多少种通用的特征预处理算法，即对类别和数值特征都有效。默认为1。

**36 meta\_file : None or str, default : None**

元学习的文件路径，默认为None，即不使用元学习。

**37 use\_meta\_learning : bool, default : False**

是否使用元学习，默认为False，即不使用。

**38 random\_state : None or int, default : None**

随机状态设置，默认为None，即不固定随机状态。



