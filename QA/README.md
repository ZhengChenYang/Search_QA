## 阅读理解模型

本方案阅读理解模型部分，借鉴了以下几个repo：

https://github.com/ymcui/Chinese-BERT-wwm

https://github.com/brightmart/roberta_zh

https://github.com/AI-confused/COVID19_qa_baseline

https://github.com/joleo/government_qa_assistant

在此表示感谢🙏

修改train.sh中对应的模型、数据路径，即可开始训练；

```shell
./train.sh
```
修改test.sh中对应的模型、数据路径，即可开始推理；

```shell
./test.sh
```
修改agg_test.sh中对应的模型目录和数据路径，即可开始模型集成；

```shell
./agg_test.sh
```

值得注意的是，本方案中模型们都放到一个目录下，按照以下命名规则：

```shell
(weight)_(name).pth
```

其中，weight是模型的权重，name是模型的名字，代码将根据权重对logits进行加权平均，根据name使用对应模型配置进行初始化，例如：

```shell
../models/8.2_robert2.pth
```

对于这样一个模型，将初始化robert模型，其logits将按0.082的权重进行集成。

本方案最终使用13个模型进行集成，分别是bert五折、robert五折、bert全量数据、robert全量数据、robert without fgm全量数据。权重按照测试集合得分分配，全量数据模型权重是拍的。

对于最终结果，选取每个文档答案logits得分最高的，再根据召回的排序进行得分衰减，最终得到最后的结果。

