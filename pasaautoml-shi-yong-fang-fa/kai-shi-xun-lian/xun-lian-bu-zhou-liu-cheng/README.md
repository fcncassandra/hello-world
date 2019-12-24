# 训练步骤流程

### 1 导入相关包

以mode0为例，mode0是指固定模型下选择超参数，**所以在mode0模式下需要确定一个或多个模型，这里选择导入的是RandomForest**。我们来看看需要导入相关的包：

```python
import logging
import sys
import os
import automl
import sklearn
from sklearn.model_selection import train_test_split
import sklearn.datasets
from automl.Classification.classifier import Classifier
import time
from automl.performance_evaluation import eval_performance
from automl.BaseAlgorithm.classification.SKLearnBaseAlgorithm import RandomForest
logger = logging.getLogger(automl.logger_name)
```

* 第8行导入了automl中的分类问题核心模块Classifier
* 第10行导入了automl中的评估指标
* 第11行导入了automl中的RandomForest模型

### 2 导入数据和模型

接下来本例中使用了sklearn.datasets中的wine数据集进行测试，并选用随机森林算法作为mode0的指定算法，用logger.info可以在日志内将其打印。

```python
X, y = sklearn.datasets.load_wine(return_X_y=True)
x_train, x_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
logger.info(len(x_train), len(x_test))

estimator = RandomForest.RandomForest()
```

### 3 初始化PASA-AutoML

接下来利用Classifier来初始化模型，主要需要设置：

* 第1行 total\_time 表示训练总时间，这里是3600秒
* 第2行 per\_run\_time 表示每轮运行的时间，这里是120秒
* 第3行 test\_metric 表示评价指标，分类问题这里选用了'accuracy\_score'
* 第4行 random\_state 随机种子，让实验结果可复现
* 第8行 automl\_mode 这里由于是指定模型进行参数训练，设置为0
* 第9行 validation的两个参数表示使用交叉验证，并且训练集占2/3，验证集占1/3
* 第12行 output\_folder表示模型产生在当前目录的HypTuner\_log文件夹下
* 第15行 addClassfier来添加之前初始化的模型供mode0模式来调参
* 第19行 表示设置automl模式，和第8行的作用一样，只是方便用户调整
* 第20行 表示设置分类模型的输出情况，由于是wine是多分类问题，这里设置为1，也可以在第8行的classfication\|\_mode下直接设置，只是方便用户调整

```python
total_time= 1*60*60
per_run_time= 2*60
test_metric="accuracy_score"
random_state=0
name='classifier_test_mode_0'

clf = Classifier(total_timebudget=total_time, per_run_timebudget=per_run_time,
        automl_mode=0, classification_mode=0, evaluation_rule=test_metric,
        validation_strategy="cv", validation_strategy_args=3,
        budget_type="datapoints",
        min_budget=27, max_budget=729, name=name,
        output_folder="./HypTuner_log/{}".format(name),
        log_level='INFO')

clf.addClassifier({"{}".format(name): estimator})  # fix classifier to tune its hyperparameter

clf.set_version(random_state)

clf.set_automl_mode(0)
clf.set_classification_mode(1)  # wine is a multi-class problem

```

### 4 开始运行

接下来用fit来传入训练集数据，并用测试集数据进行评估。

* 第2行 注释掉的refit在mode0下无需调用，该方法的意义是选择不同模型时用交叉验证的方式选取模型及参数，使用的训练数据每次只是交叉验证下训练集**x\_train再次分出的训练部分**。当模型选择好了之后，再用这个refit方法去**评估模型对整体训练集的情况**。
* 第6行 用来预测x\_test的结果
* 第7行 用来评估x\_test的结果
* 第10行 用来存储训练的模型

```python
clf.fit(x_train, y_train)
# clf.refit(x_train, y_train)  # hypertuner no need to refit
# logger.info("best model name: {}".format(clf.get_best_model_name()))
# clf.print_statistics()  # this method is only useful in mode 2

y_hat = clf.predict(x_test)
tmp_res = eval_performance(test_metric, y_true=y_test, y_score=y_hat)
logger.info("val score: {}".format(clf.hypTunerResult.get_incumbent_val_score()))
logger.info("test score: {}".format(tmp_res))
clf.hypTunerResult.save_trajectory("./HypTuner_log/{}".format(name),
        include_test=True,
        estimator=estimator,
        X_train=x_train,
        y_train=y_train, X_test=x_test, y_test=y_test,
        per_run_timebudget=per_run_time)

logger.info("==================Classifier mode 0 PASSED==================")
print("==================Classifier mode 0 PASSED==================")
```

### 5 函数包装和日志打印

为了方便运行，并且捕捉错误，可以将所有automl相关的函数封装到一起，并用try - except来捕捉错误。

* 第43行 模型之所以会可能出现失败问题，是因为我们给其分配了per\_run\_time参数，如果数据量太大或者模型过于复杂，改模型就会在训练前停止，即**FAILED**。
* 第46行 设置是否打印日志，由于训练时间如果过长，日志会很多，影响到用户的其他操作，所以有时候会在disable设置为False。

```python
def test_classifier_mode_0(total_time=1*60*60, per_run_time=2*60, test_metric="accuracy_score", random_state=0, name='classifier_test_mode_0'):
    try:
        X, y = sklearn.datasets.load_wine(return_X_y=True)
        x_train, x_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
        logger.info(len(x_train), len(x_test))
        estimator = RandomForest.RandomForest()

        clf = Classifier(total_timebudget=total_time, per_run_timebudget=per_run_time,
                         automl_mode=0, classification_mode=0, evaluation_rule=test_metric,
                         validation_strategy="cv", validation_strategy_args=3,
                         budget_type="datapoints",
                         min_budget=27, max_budget=729, name=name,
                         output_folder="./HypTuner_log/{}".format(name),
                         log_level='INFO')

        clf.addClassifier({"{}".format(name): estimator})  # fix classifier to tune its hyperparameter

        clf.set_version(random_state)

        clf.set_automl_mode(0)
        clf.set_classification_mode(1)  # wine is a multi-class problem
        clf.fit(x_train, y_train)
        # clf.refit(x_train, y_train)  # hypertuner no need to refit
        # logger.info("best model name: {}".format(clf.get_best_model_name()))
        # clf.print_statistics()  # this method is only useful in mode 2

        y_hat = clf.predict(x_test)
        tmp_res = eval_performance(test_metric, y_true=y_test, y_score=y_hat)
        logger.info("val score: {}".format(clf.hypTunerResult.get_incumbent_val_score()))
        logger.info("test score: {}".format(tmp_res))
        clf.hypTunerResult.save_trajectory("./HypTuner_log/{}".format(name),
                                           include_test=True,
                                           estimator=estimator,
                                           X_train=x_train,
                                           y_train=y_train, X_test=x_test, y_test=y_test,
                                           per_run_timebudget=per_run_time)

        logger.info("==================Classifier mode 0 PASSED==================")
        print("==================Classifier mode 0 PASSED==================")
    except Exception as e:
        import traceback
        traceback.print_exc()
        logger.info("==================Classifier mode 0 FAILED==================")
        print("==================Classifier mode 0 FAILED==================")

logging.getLogger('automl').disabled = False
test_classifier_mode_0()
```

