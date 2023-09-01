# 一、环境准备

### 1.安装miniconda3

如果服务器没有安装miniconda3，运行如下命令安装：

```
bash Miniconda3-latest-Linux-x86_64.sh
```

根据选择`miniconda3`的安装目录，如将miniconda3安装到用户目录下`~/miniconda3`。

### 2.创建和激活运行环境

使用conda来创建环境，运行环境可以选择使用python 3.7.16，其他的依赖包按照requirements.txt安装：

```bash
conda create -n klbr python=3.7.16
conda activate klbr
pip install -r requirements.txt
```

### 3.docker部署
（改）
```bash
方法一：
cd klbr
docker load -i code.tar
docker images
docker run -itd -p 5000:5000 klbr:v1.0
方法二：
cd klbr
docker build -t klbr:v1.0 .
docker run -itd -p 5000:5000 klbr:v1.0
```

# 二、数据和测试

### 1.post地址
#### 1.1 省略词填充
127.0.0.1:5000/se
#### 1.2 错别字纠正
127.0.0.1:5000/pc
#### 1.3 时间填充
127.0.0.1:5000/tf
#### 1.4 错别字纠正
127.0.0.1:5000/pun
#### 1.5 术语标准化
127.0.0.1:5000/ts


### 2.数据格式
#### 2.1 省略词填充
输入：输入一段文本信息，例如：“膀胱充盈尚可，膀胱壁略增厚，强化较均匀，其内可见导尿管影。”
输出：输出填充后的文本信息，例如：“乙状结肠管壁不规则增厚，乙状结肠管壁较厚处约2.5cm，乙状结肠管壁增强扫描明显强化，乙状结肠管壁浆膜面毛糙，乙状结肠管壁邻近脂肪间隙略模糊，可见多个短径小于1.0cm淋巴结影。”
#### 2.2 错别字纠正
输入：输入一段文本信息，例如：“消炎请吃头孢克洛克力。”
输出：输出纠正后的文本信息，例如：“消炎请吃头孢克洛颗粒。”
#### 2.3 时间填充
输入：输入一段文本信息，例如：“现病史:患者自诉因大便性状改变于4月至我院住院治疗，06盆腔MRI提示：乙状结肠和直肠交界区巨大肿块，考虑为结肠癌，周围淋巴结显示（多发）。胸腹部CT提示：1.盆腔肿块，建议行盆腔CT扫描；2.胸部CT平扫+增强未见明显异常。（2018-4-13）行肠镜检查后完善病理：1.（乙状结肠）腺癌，请密切结合临床进一步明确浸润情况；2.（直肠）腺癌，请密切结合临床进一步明确浸润情况。免疫组化：MLH1×2(+)；MSH2×2(+)；MSH6×2(+)；PMS2×2(+)。因肿瘤体积大，手术难度高，经肿瘤放疗科会诊后，分别于05-03、05-31及06-21行三周期奥沙利铂联合卡培他滨化疗，并于05-09至06-11同步行盆腔放疗，过程顺利，患者无明显不适症状。”
输出：输出填充后的文本信息，例如：“现病史:患者自诉因大便性状改变于2018年4月至我院住院治疗，06盆腔MRI提示：乙状结肠和直肠交界区巨大肿块，考虑为结肠癌，周围淋巴结显示（多发）。胸腹部CT提示：1.盆腔肿块，建议行盆腔CT扫描；2.胸部CT平扫+增强未见明显异常。（2018-4-13）行肠镜检查后完善病理：1.（乙状结肠）腺癌，请密切结合临床进一步明确浸润情况；2.（直肠）腺癌，请密切结合临床进一步明确浸润情况。免疫组化：MLH1×2(+)；MSH2×2(+)；MSH6×2(+)；PMS2×2(+)。因肿瘤体积大，手术难度高，经肿瘤放疗科会诊后，分别于2018-05-03、2018-05-31及2018-06-21行三周期奥沙利铂联合卡培他滨化疗，并于2018-05-09至2018-06-11同步行盆腔放疗，过程顺利，患者无明显不适症状。”
#### 2.4 标点符号纠正
输入：输入一段文本信息，例如：“临床提示“结肠癌术后”改变，未见复发，2、肝脏多发类圆形低密度影，与2018-5-7CT对比，所见大致同前，考虑囊肿可能3、脂肪肝，4、副脾，同前。”
输出：输出纠正后的文本信息，例如：“、临床提示“结肠癌术后”改变，未见复发。2、肝脏多发类圆形低密度影，与2018-5-7CT对比，所见大致同前，考虑囊肿可能。3、脂肪肝。4、副脾，同前。”
#### 2.5 术语标准化
输入：输入一个非标准词，例如：“维格尔牌西洋参精软胶囊”
输出：输出对应的标准词，例如：“西洋参精软胶囊”


# 三、代码说明

文件目录格式如下：

```
——code
	——data
	——punc_final
	——PyCorrector_final
	——subjec_ellipsis
	——Term_Standard_final
	——Time_Full_final
	——Flask.py
	——main.py
	——pc.py
	——pun.py
	——requirements.txt
	——se.py
	——Test.py
	——tf.py
	——ts.py
	——ts_word.py

```

data存放的是已知的标准词库，punc_final是标点符号处理模块，PyCorrector_final是错别字处理模块，subjec_ellipsis是省略填充模块，Term_Standard_final是术语标准化模块，Time_Full_final是时间填充模块，Flask.py是接口函数，main.py是整体函数，pc.py是错别字纠正接口，pun.py是标点符号纠正接口，requirements.txt是所需的依赖包文件，se.py是主语省略填充接口，Test.py是接口测试函数，tf.py是时间填充接口，ts.py是术语标准化接口（输入句子），ts_word.py是术语标准化接口（输入非标准词）。

#### 3.1 省略词填充
```
——subjec_ellipsis
   ——uie
	——$fineuned_model
	——checkpoint
	——evaluate.py
	——main_se.py
	——uils.py
```
$fineuned_model中保存的是微调后的模型，checkpoint是保存训练权重，evaluate.py是用于评估测试结果，main_se.py是主函数，通过调用模型得到结果并展示出来，uils.py是过程用到的方法。
#### 3.2 错别字纠正
```
——PyCorrector_final
   ——pycorrector
	——macbert
		——output
		——_init_.py
		——base_model.py
		——defaults.py
		——infer.py
		——lr_scheduler.py
		——macbert4csc.py
		——macbert_corrector.py
		——preprocess.py
		——reader.py
		——softmaskedbert4csc.py
		——train_macbert4csc.yml
	——util
	——connfig.py
```
output是存放模型权重，infer.py是主函数，通过调用模型实现错别字纠正，utils是模型预测时所需的操作，config.py是参数配置。
#### 3.3 时间填充
```
——time_Full
	——repair.py
	——time_full.py
```
repair.py中的方法是对年月日进行修复，time_full.py是主函数，通过正则提取时间并进行纠正，并将结果进行展示。
#### 3.4 错别字纠正
```
——punc_final
	——evalute.py
	——main_treefinal.py
```
evaluate.py是用于评估测试结果,——main_treefinal.py是主函数，通过调用预训练模型并结合纠正原则对标点进行纠正。
#### 3.5 术语标准化
```
——Term_Standard_final
  ——word_standard
	——checkpoint
	——export
        ——predictor
	    ——find.py
	    ——infer_classification.py
	    ——model.onnx
	    ——Predictor.py
	    ——标准词.json
  ——term_standard.py

```
term_standard.py是采用文本匹配法进行替换，该方法输入是一段文本信息。word_standard是对单个词进行纠正，checkpoint是保存模型参数配置，export是模型权重，find. py是寻找最相近的词，infer_classification.py是主函数，输入非标准词调用模型进行标准化，Predictor.py是纠正是所需要的方法，



