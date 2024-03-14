
论文链接: [Li, X.et al.Origin and evolution of the triploid cultivated banana genome.Nat Genet 56, 136–142 (2024).](https://doi.org/10.1038/s41588-023-01589-3)

## 通讯作者简介
[吕培涛](https://hbmc.fafu.edu.cn/b0/65/c4949a241765/page.htm)

- 学习工作经历
  - 2003-2010 仲恺农业工程学院，学士、硕士
  - 2010-2015 中国农业大学，博士
  - 2015-2019 香港中文大学，博士后
  - 2019- 福建农林大学园艺学院、海峡联合研究院，教授
- 研究方向
  - 园艺作物品质形成及逆境响应由严格精细的网络调控
  - 课题组自主可控的植物ENCODE优化技术及配套的生物信息分析技术

[张亮生](https://person.zju.edu.cn/0020046)

- 学习工作经历
  - 2012年 获复旦大学遗传学博士学位，导师 马红教授；
  - 2012-2013 宾州州立大学，博士后；
  - 2013-2015 同济大学，副研究员；
  - 2015-2020 福建农林大学，教授（特聘）；
  - 2020-至今 浙江大学，教授
- 研究方向
  - 观赏植物（花卉）种质资源挖掘和利用、基因组学和功能基因挖掘

## 背景

- 香蕉 *Bananas (Musa ssp.)* 是世界重要的粮食作物和经济作物, 现代栽培品种基本都是通过野生的二倍体亲本A基因组 (*Musa acuminata*, 2n = 22) 和B基因组 (*Musa balbisiana*, 2n=22) 种内或种间杂交而形成的
- 目前主要的种植品种是香芽蕉(Cavendish), 但是在 20 世纪 50 年代以前主要栽培的品种是大麦克香蕉(Gros Michel), 之后黄叶病(Fusarium oxysporum f. sp. cubense (Foc) race 1)爆发, 大量种植大麦克香蕉的公司转种香芽蕉.
- 相比于香芽蕉, 大麦克香蕉更加香甜美味, 皮厚保质期较长, 同时果实数量也更多. 
- 香芽蕉目前受到Froc tropical race 4 (TR4)的威胁, 也有虚拟灭绝(virtual extinction)的风险.
- 栽培香蕉的三个A亚基因组的起源尚不清楚.

## 结果

### Genome assemblies of triploid Cavendish and Gros Michel

作者分别组装和注释了三倍体的香芽蕉和大麦克香蕉, 下面是主要的组装注释方法和其结果

| Statistic               | Cavendish                                       | Gros Michel                  |
| ----------------------- | ----------------------------------------------- | ---------------------------- |
| Sequence Methods        | PacBio sequencing, Illumina sequencing and Hi-C | PacBio HiFi sequencing, Hi-C |
| Software for Assembly   | Canu + Merqury + ALLHiC                         | Canu + Merqury +ALLHiC       |
| Software for Annotation | MAKER + InterProScan                            | MAKER + InterProScan         |
| Number of contigs       | 6,765                                           | 6,423                        |
| Assembled genome (Mb)   | 1,480                                           | 1,331                        |
| Contig N50 (kb)         | 241.2                                           | 1,038.0                      |
| G+C content (%)         | 39.3                                            | 39.3                         |
| Pseudo-chromosomes      | 33                                              | 33                           |
| Number of genes         | 106,540                                         | 120,653                      |
| BUSCO (%)               | 97.0                                            | 96.9                         |

### Origin of AAA triploid cultivated banana

这两个品种的所有祖先特异性K-Mers(作者方法里面写的是Merqury, 这是个基于kmer的基因组评估软件, 没太理解)都可以追溯到五个可能的野生二倍体祖先: M. acuminata ssp. banskii (Banskii), malaccensis (DH-Pahang), zebrina (Zebrina), burmannica (Calcutta 4)和Musa schizocarpa (S genome from New Guinea). 最后确定Ban (Banksii), Dh (DH-Pahang)及Ze (Zebrina)是最可能的供体. 编码基因做的系统发育树和Ks频率分析也支持以上的结论. 同时作者还对这两个品种的亚基因组做了滑窗(2kb), 分别比对判断每一段窗口序列的来源. 

![](/img/triploid-banana/3.2_1.png)

![](/img/triploid-banana/3.2_2.png)

### The evolution of Fusarium wilt resistance genes in banana

目前Froc TR4可以感染80%种香蕉品种, 其中香芽蕉尤其严重. 先前科学家从抗Froc TR4的M. acuminata ssp. malaccensis中克隆出了RGA2(resistance gene analog 2)基因. 作者探究了该基因在香芽蕉, 大麦克香蕉和这几个二倍体中的情况, 发现:

- RGA2基因为单拷贝基因, 在每套亚基因组内仅有一个拷贝
- RGA2基因的编码蛋白在以上物种内高度保守(相似度>97%)
- 检查该基因上游1-kb发现存在普遍的200bp以上重复序列的插入

![](/img/triploid-banana/3.3_1.png)

在前言中我们知道, Froc TR1菌株导致的黄叶病造成了香芽蕉大规模取代大麦克香蕉, 因此作者就想探究一下香芽蕉抗TR1的原因, 先前的学者已经发现过一个和TR1抗性有关系的数量性状位点(RLP, 下图中用红色表示). 作者分析了以上香蕉品种中该基因的情况, 发现唯独在大麦克香蕉中的一个个亚基因组内缺失(Zebrina同源的那一套亚基因组). 

![](/img/triploid-banana/3.3_2.png)


### Genes controlling banana fruit ripening

于其他依赖乙烯成熟的水果相比, 香蕉成熟涉及正反馈双循环(two positive-feedback loops), NAC转录因子MaNAP是它们的耦合节点. 作者分别在香芽蕉和大麦克香蕉中发现了五个和四个MaNAP同源物, 同时可以分成两个分支.

![](/img/triploid-banana/3.4_1.png)

香芽蕉在A分支中的MaNAP1-MaNAP3是MaNAP的同源基因, 在水果成熟和叶片衰老中均有表达. 同时其在B分支的二个同源基因仅在水果成熟时高表达. 这一表达模式在*Musa spp. ABB*也有观测到.

![](/img/triploid-banana/3.4_2.png)

![](/img/triploid-banana/3.4_3.png)

作者使用MaNAP4和MaNAP5特异性抗体进行ChIP-seq(染色体免疫共沉淀测序)来鉴定他们的结合基因. 鉴定得到一系列关键的果实发育基因(135个基因,其中有24个是已经被发现的), 并构建了MaNAP4-MaNAP5共表达网络. 

### Subgenome dominance in triploid bananas

Subgenome dominance[^1]

- 亚基因组优势是指其中亲本基因组（亚基因组）之一与其他亚基因组相比具有更高水平的基因表达和最终更多的基因保留.

- 分化程度较低且表达较高的亚基因组被称为“显性亚基因组”(dominant subgenome), 分化程度较高且表达水平较低的亚基因组被称为“顺从亚基因组”(submissive subgenome).



作者发现Ban亚基因组是显性亚基因组: 更多保留的祖先基因, 更高的同源表达, 更多的DHSs[^2] (DNase-hypersensitive sites), 同时上一节发现的MaNAP4和MaNAP5的结合位点数量偏向性也支持这一结论.

最后作者也鉴定了一些列的疾病抗性基因(NBS-LRR, RLP, RLK), 发现NBS-LRR在Ze亚基因组内显著偏见, 表明Ze亚基因组相比其他两个亚基因组更侧重调节疾病抗性.

![](/img/triploid-banana/3.5.png)

## 讨论

作者的讨论大致分为三个部分

- Ban亚基因组调控果实成熟, Ze亚基因组负责抗病, 说明三倍体香蕉的亚基因组存在功能分化
- RLP位点在大麦克香蕉中的丢失很好的解释了它对Foc race 1 的敏感性, 这一位点来源于Zebrina, 说明大麦克香蕉和香芽蕉的Ze亚基因组可能有不同的祖先, 也可能是分化后大麦克香蕉的Ze亚基因组丢失了这一位点.
- 新的NAP同源物(MaNAP4和MaNAP5)在果实成熟中高度表达, 与一系列已知基因和未知基因结合, 表明许多未知功能基因对果实成熟非常重要, 同时作者猜想MaNAP4和MaNAP5很可能特异性参与香蕉果实成熟的正反馈双循环


[^1]:Alger, E.I.& Edger, P.P.One subgenome to rule them all: underlying mechanisms of subgenome dominance.Current Opinion in Plant Biology 54, 108–113 (2020).

[^2]:指对DNase酶高度敏感的部位, 这些区域在染色质结构松弛或开放的状态下, 相对于其他区域更容易被DNase酶降解. 因此研究人员可以以此来确定基因组中的潜在调控区域和调控元件.
