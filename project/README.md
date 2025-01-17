file description:

1. **date collection**:     
(a)结点获取（从fandom的character列表中迭代获取，共4k个人物名称）    
(b)通过Fandom API下载各个character界面的文本    
(c)分析界面，得到界面和界面之间的链接信息，将此定义为网络的边。将结点和边的信息存储。     
**至此，我们获得（number of nodes:4264；number of links:113936）的哈利波特网络。**    
（此网络不仅限于哈利波特，也包括了神奇动物的人物，可以说是JK罗琳构建的魔法世界的全部人物。结点可能会存在一些问题，例如将非人物视为人物进行存储，因为哈利波特的Fandom网页的character category包含诸多子类别，子类别又包含子类别，我们已限制了搜索深度，但不可避免会获取到一些并无法算是“人物”的名称。其中也有一些是，本身就不是“人物”，但是Fandom上将其视为有perdonality的物品，比如，分院帽。）      
*对这个网络本身可能需要进行的处理：*  
* 筛选结点，去除明显不是人物的结点。（手动筛；根据hp-lexin automaticlly处理<-要找到一个比较reasonable的人物list源，人物还不能太少。）  
* 根据sune在project a上推荐的网络压缩（不确定是不是这么说）算法，简化网络。这步是合理的，因为目前的网络的结构会包含非常多的不重要的小人物（FANDOM界面过于全）。  

  
2. **add node attribute**      
(a) 读取已存储好的json文件构造网络（避免每次构建网络需要重新检索界面中的link）   
(b) 我们感兴趣的三个attribute, 对其中的“学院”属性进行属性添加：    
      i. 获取属性信息。此步根据fandom character category中“individual  by house”的信息获取属性。    
      ii. 讲属性信息添加到网络    
      iii. 获取subgraph 子网络（因为有标注的霍格沃兹学院的人在整个网络中的占比不大，不适合用整张网络进行分析）    
      iv. 进行简单的可视化（不同颜色代表不同学院）    
      v. 进行简单的社区检索并可视化（未分析）    

(c) 使用louvain和infomap算法分别进行社区检测。通过infomap的结果拥有较大的子社区，使用louvain进一步对于这个子社区进行检测（based on modularity方法的局限性以及ns中提到的解决方法）;其实这步我预想的是通过infomap再算一次，但是infomap并不会再给子社区进行划分（应该还是modularity的问题）。目前不知道如何解决。  
(d) 全图的社区检测以及可视化（for fun）：可以看到结点需要筛选。以及可以进去看看社区节点，看看是什么样的子社区。  

3. **fandom_page_clean**  
(a) 读取从fandom下载的网页，进行   
* 掐头去尾的工作（避免notes and reference后面的链接对link产生影响）
* 去除所有的html格式，保存为txt,为后续nlp分析做准备
(b) nodes_edges_new.json记录了新的网络节点和链接



{接下来：
1. 网络结点清理；
2. 网络压缩技术（尝试）；
3. 社区检测技术（better way?）;
4. 6度定律验证；
5. 生成社区后：(a) tfidf提取社区代表词汇;
6. 情感分析：(a) 对人物的情感分析；(b) 对社区的情感分析；
7. 文本输入预测社区划分（思路？）}

