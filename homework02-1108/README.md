第一次训练
看来我Mr Zhang还是纯正的中国人~
原来我媳妇Mrs Ma是个韩国人~早知道她以前在韩国公司上班还认识一堆韩国人，原来就是韩国人啊~怪不得她不喜欢看韩剧都看腻了才来中国的吧~话说韩国妹子都爱整容她不会也整过吧~怪不得和身份证上的样子不像呢~可是身份证上写的和我是同乡啊，办的假证吧~为了我她还真是拼命了呢HEHEHEHE~那她的一堆中国亲属又是怎么回事呢？都是被她收买的吧~#…*！…#￥#@&%*省略一万字中二病已经无法阻挡了
不知道上面这段鬼话用NLP处理一下能生成什么呢？

第二次训练
epoch = 5 loss = 1.54
参数相同
> Ma
(-0.51) Vietnamese
(-1.36) Chinese
(-2.24) Korean
我媳妇Mrs Ma又变成了越南人！这也太不靠谱了吧~不记得从越南买过媳妇啊~严重怀疑这个LSTM模型的能力了。。。看来还需要改进啊！训练时间长一点会不会好呢？

第三次训练
epoch = 10 loss = 1.35
> Zhang
(-0.70) Korean
(-0.95) Chinese
(-2.42) Vietnamese

> Ma
(-0.86) Chinese
(-1.21) Vietnamese
(-1.29) Korean
我媳妇Mrs Ma倒是中国人了，可是我Mr Zhang却变成了韩国人~捂脸

第四次训练
epoch = 20，使用保存的模型加载后继续训练，初始loss = 0.96，结束loss = 0.95
> Zhang
(-0.21) Chinese
(-2.69) Korean
(-3.46) Vietnamese

> Ma
(-0.72) Vietnamese
(-1.35) Chinese
(-1.38) Korean
我恢复了中国人身份，媳妇又成了越南人。话说这次的loss曲线为什么初始值与上次不一致？又为什么先升后降？
检查数据发现Ma这个姓在汉语、韩语和越南语中都有，所以会这么纠结。
但是Zhang这个姓韩语和越南语中并没有，为什么训练3中会判为韩国人呢？

训练5
epoch = 30，加载模型，初始loss = 0.44, 结束loss = 0.87
> Zhang
(-0.09) Chinese
(-2.94) Korean
(-4.52) English

> Ma
(-0.65) Vietnamese
(-0.97) Korean
(-2.30) Chinese
明白了，曲线先升后降是因为第一次的mean loss只包含了第一次predict的loss，所以不具代表性。而且,losses值应该在每次产生曲线值后清空，一直累加是不对的
多语种同姓的问题怎么解决呢？

embedding层的输出维度和隐含层维度没有关系，embedding使输出向量与隐含层维度相同只是由于想让序列中的每个元素有一个对应的隐含状态值。
但是这只是人类的想法，事实上机器并不一定这样处理就最好。
那么, 是不是增加隐含层节点数量可以改善性能呢？

训练6
先减少隐含节点，这必然能提高训练效率，要看的是能否达到相同的loss。
epoch = 10，hiden = 5，初始loss = 2.84, 结束loss = 1.49
> Zhang
(-0.50) Korean
(-1.05) Chinese
(-3.85) Vietnamese

> Ma
(-0.68) Vietnamese
(-1.03) Chinese
(-2.12) Korean
 loss = 1.49略高于10个隐含单元时的1.35，矩阵图上看明显没有10个隐含单元时好。
 
 训练7
epoch = 30，hiden = 5，初始loss = 2.83, 结束loss = 1.42
> Zhang
(-0.63) Chinese
(-1.10) Korean
(-3.18) Irish

> Ma
(-0.79) Korean
(-1.04) Chinese
(-1.74) Vietnamese
 loss = 1.42远远高于10个隐含单元时的0.87，矩阵图同样没有10个隐含单元时好。这说明5个隐含节点可能已经不足以记忆任务所需的复杂模式。
 
 训练8
epoch = 30，hiden = 20，初始loss = 2.53, 结束loss = 0.45
> Zhang
(-0.20) Chinese
(-1.84) Vietnamese
(-4.10) Korean

> Ma
(-0.30) Korean
(-1.57) Vietnamese
(-3.90) Polish
loss = 0.45远远低于10个隐含单元时的0.87，矩阵图明显较好。不知道有没有过拟合。
