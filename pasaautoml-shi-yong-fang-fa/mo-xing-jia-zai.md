# 模型加载

### 1 导入模型

PASA-AutoML训练过程完毕后可以去加载最优模型，需要从**automl.util.op\_model\_file**导入**load\_model**方法，该方法需要传入指定文件夹下的模型。在刚才的训练中，我们最优的模型是在**HpeTuner\_log/classifier\_test\_mode\_0**下的**model\_0\_2.pkl**文件，所以load\_model传入后面的两个参数分别对应文件名下划线的**0**和**2。**

导入模型后可以使用get\_model\(\)获取模型参数，发现该参数即我们训练的最佳模型结果。

```text
from automl.util.op_model_file import *
model = load_model('./HypTuner_log/classifier_test_mode_0',0,2)
model.get_model()
```

![&#x5BFC;&#x5165;&#x6A21;&#x578B;](../.gitbook/assets/image%20%2821%29.png)

### 2 模型预测

模型导入成功后可以用predict方法传入对应格式的特征数据进行预测。

```text
X, y = sklearn.datasets.load_wine(return_X_y=True)
x_train, x_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)

model.predict(x_test)
```

![&#x6A21;&#x578B;&#x9884;&#x6D4B;](../.gitbook/assets/image%20%2814%29.png)

### 3 交叉验证

模型交叉验证需要导入**automl.crossvalidate**进行预测，预测结果为几折交叉验证的均值情况和预测结果。

```text
from automl.crossvalidate import *
acc = cross_validate_score(model, X, y, evaluation_rule='accuracy_score', cv=10)
acc
```

![&#x4EA4;&#x53C9;&#x9A8C;&#x8BC1;](../.gitbook/assets/image%20%2810%29.png)

