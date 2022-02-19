。使用React+SpringBoot+MySQL技术完成项目搭建。 用户上传宠物描述信息和领养天数数据以后，后台训练预测模型。用户再输入想要预测的宠物的信息，后台就会根据这些信息给出预测结果。

功能有上传文件，特征展示，特征选择，模型训练和预测。

通过切片上传和md5标识的方法，优化了上传功能。实现了大文件秒传和断点续传功能，大大提升了用户在上传大文件和在网络不稳定时的使用体验。

PetFinder 

带领两人小组开发一个宠物领养速度预测的机器学习平台，帮助收容所工作人员提高宠物领养的概率。

为提升开发效率，采用前后端分离架构。主要功能有上传数据，特征展示及选择，模型训练，以及预测领养天数。

前端使用React.JS 框架构建单页web应用，并使用Material-UI和Bootstrap客制化原生组件，提高页面美观性。

后端使用SpringBoot开发，采用Controller-Service-Repository-Model设计模式减少代码耦合，并使用JPA与MySQL进行交互，减少了该模块50%的开发时间。

为优化超大文件传输及网络拥塞导致的文件上传失败问题，通过文件切片和md5标识的方法，实现了大文件秒传和断点续传功能，提升了30%的上传速度并大大提升了用户体验。



软件开发实习生

用Java语言开发了用于Android和IOS应用的本地化自动翻译工具，是公司内第一个投入使用的内部工具。

用XML框架dom4j实现了XML文件和CSV文件之间的转换，并用Google Sheet API生成统一的翻译配置页面，极大提高了Android 和IOS 工程师的协同工作效率。

用Google Translate API将英文字符串翻译成14国语言，并自动生成各语言的字符串文件，减少了开发人员50%的开发时间。

XML to CSV: 

XMLToCSV类： ReadDataFromxml()

Utils类：Generatecsvfromxml()

用google sheet打开，googlesheet作为配置页面

GoogleSheet类：getCSVFromGoogleSheet(). googlesheet产出本地csv文件

Translate类：GeneralTranslate()将本地csv文件翻译成14国语言

​						GenerateTranslatedcsv() 并输出到本地

ExportToAndroid类：



算法实习生

协助算法工程师从零到一搭建推荐算法底层框架，完成了第一期算法项目。

通过SQL脚本分析整站用户流量分布以及链路转化，并主动向数仓同学请教，构建了每日自动化报表，大大提升观测数据的效率。

跑通了2个经典召回算法（协同过滤和矩阵分解），并通过调整训练数据集调优，最终算法上线后带来了月度GMV5%的提升。

调研各大电商平台的用户画像指标体系，从零到一搭建了公司第一个用户画像系统。

