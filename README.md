# icyChessZero 中国象棋alpha zero 

这个项目受到alpha go zero的启发，旨在训练一个中等人类水平或高于中等人类水平的深度神经网络，来完成下中国象棋的任务。目前这个项目仍在积极开发中，并且仍然没有完成全部的开发，欢迎pull request 或者star。 然而受到计算资源限制，这样庞大的任务不可能在一台机器上完成训练，这也是我完成了分布式训练代码的原因，希望各位小伙伴能够加入，一起训练这样一个中国象棋alpha go的网络。

我的估计是达到4000～5000elo分数的时候深度网络可以达到目标，现在深度网络已经到了3000分的边缘，达到人类中上水平的目标并不是不可能的。

目前的elo：

![elo](elos.png)

详细胜率表：

![table](table.png)

当然，目前棋力还比较一般，因为是从完全随机开始训练的，比方说某个对局片段(800 playouts)：

![demo](imgs/demo.gif)

# 推荐代码运行环境
* python==3.6.0
* tensorflow==1.4.0
* threadpool==1.3.2
* xmltodict==0.11.0
* urllib3==1.22
* numpy==1.14.3
* tflearn==0.3.2
* pandas==0.19.2
* scipy==1.1.0
* matplotlib==2.0.0
* tqdm==4.19.4
* tornado==4.5.1(集群master必须安装，slave不需要)
* uvloop==0.9.1(windows可不装)


```

意味着在0号GPU上用python3环境跑10个进程（与上面windows版本对应)




# 自组集群
如果校外有一些机器，希望能跑起来这样一个分布式程序，那么请按照下面的步骤做：

1. 确定你要这么做，这是一个耗时，昂贵，不讨好,但是有点意思的工作
2. 推荐你的机器（们）的环境满足推荐配置，并且安装好应该装的包
3. master 机器一定要是linux（目前没有支持master也是windows）
4. fork一份icyChessZero的代码,找到 ``` config/conf.py  ```这个文件，把server的ip改成你希望的master的ip
5. master 和slave分别clone这份fork的代码
6. 在master上```cd scripts```运行 ``` initize_weight.py ``` 生成第一份随机权重
7. 在master上```cd distribute```运行 ```distributed_server.py```开启master服务端口
8. 在slave机器上起slave进程的方法同上文"加入集群"
9. master上如果有空闲的资源可以起几个slave进程
10. 模型更新和validate的方法在scripts/daily_update.sh中，按照你的需求改这个shell文件，并且把它放到crontab中设置为每小时运行一次（它会检查棋谱数量，数量足够后它会执行模型更新和评估工作）

# 查看棋谱
slave机器运行出来的棋谱在 ```data/distributed``` 目录下，是cbf文件，可以通过"象棋桥"软件查看，也可以在 ``` ipynbs/see_gameplay.ipynb ``` 中查看

# 查看训练状态
master 机器可以在```ipynbs/elo_graph.ipynb``` 中查看集群训练的模型的elo到什么水平了。


# 没做的事
还有挺多东西可以做的，工程也还在快速开发,比如：
1. 给棋谱加上一些meta，比如每一步的mcts分析，方便查个别case

~~2.长将和长捉的判断还没有做~~

3. 给代码加上版本限制，master只接受与自己版本相同的slave的棋谱
4. 专门搞一个web ui实时展示elo和棋谱等
5. readme写清楚模块划分
.....

等等等等
如果你发现有你想做的，提提pull request或者联系我
