
文献链接:  [Chen, S.et al.Chromosome‐level genome assembly of a triploid poplar Populus alba ‘ Berolinensis ’.Molecular Ecology Resources 23, 1092–1107 (2023).](https://doi.org/10.1111/1755-0998.13770)

## 通讯作者简介

 [陈肃](https://tblab.nefu.edu.cn/info/1041/1443.htm)

- 经历

  - 2006年于东北林业大学，获学士学位；

  - 2009年于东北林业大学，获硕士学位；

  - 2012年于东北林业大学，获博士学位。

  - 2012年—2017年，东北林业大学，博士后

  - 2017年—至  今，东北林业大学，副教授。

- 研究方向
  - 林木遗传改良及林木基因工程育种

[曲冠证](https://forestry.nefu.edu.cn/info/1035/1677.htm)

- 经历
  - 1997年于东北林业大学林学专业获学士学位
  - 2002年于东北林业大学林木遗传育种专业获硕士学位
  - 2007年于韩国江原大学获博士学位
- 研究方向
  - 从事林木遗传育种研究，近几年主要研究重点为杨树单倍体育种及杨树等基因工程改良研究。

[汪国华](https://ccec.nefu.edu.cn/info/1057/1668.htm)

- 经历
  - 哈尔滨工业大学计算机应用技术专业博对来发现SNP和InDels, 是因为作者认为这三个亚基因组足够相似吗, 那为什么不直接进行全基因组士学位
  - 约翰霍普金斯大学博士后
  - 2009年起在哈尔滨工业大学计算机科学与技术学院历任副教授，教授，博士生导师
  - 2019年调入东北林业大学信息与计算机工程学院任院长，林木遗传育种国家重点实验室PI
- 研究方向
  - 生物信息学、人工智能。



## 背景

- 多倍化的优势: 抗逆和适应极端环境(eg.多倍体拟南芥更加耐盐等..)
- 多倍体耐性假说之一: 特定基因家族的扩张引起的剂量效应是主要原因 https://doi.org/10.1534/g3.119.400913
- 杨树是分布最广泛的驯化森林品种, 多倍体品种均是通过种间杂交形成的. 银中杨属于中国典型的三倍体杨树, 由银白杨和中东杨经人工杂交选育而成, 生长快速, 抗旱抗寒且耐盐. 
- 多倍体育种

## 方法

- 流式细胞术测量倍型
- 基因组测序: PacBio Sequal II+ BGI + Hi-C(BGISEQ-500)
- 组装: CANU + ALLHIC + Juicebox
- 注释
  - 重复序列: repeatmodeler, EDTA, repeatmasker
  - 编码基因: MAKER
  - 功能注释: eggnog-mapper, Plangregmap, AHRD
- 共线性: Mcscanx, syri, nucmer
- 系统发育树: orthofinder, mafft, raxml-ng
- SNPs和InDels: GATK, Snpeff
- RNAseq: nextflow/rnaseq

## 结果

### Triploid Populus alba ‘Berolinensis’ has a growth advantage

银中杨(YZY) 与其亲本相比存在显著的生长优势

![image-20240321094653012](/img/poplar_Populus/image-20240321094653012.png)

### De novo assembly of the *P. alba ‘Berolinensis’* genome

二代和三代数据分别做了kmer-survery, 判断其基因组大小, 杂合度等指标

![image-20240321095222092](/img/poplar_Populus/image-20240321095222092.png)

基因组组装情况概览

![image-20240321095509009](/img/poplar_Populus/image-20240321095509009.png)

### Annotation of the *P. alba ‘Berolinensis’* genome

结构注释表明其基因组内重复序列占比约为45.23%, 注释得到的编码基因为128,281个(43255,43076,41950), 其中80.35%得到了功能注释.

### Comparative analysis of the *P. alba ‘Berolinensis’* genome

亚基因组共线性和系统发育分析, 三套亚基因组分别和*P. alba*, *P.bolleana*及*P.tremula*较近, 亚基因组内共线性较高. 同时作者也对每个亚基因组与其亲本的基因组做了共线性分析. 

![image-20240321100708342](/img/poplar_Populus/image-20240321100708342.png)

![image-20240321102518605](/img/poplar_Populus/image-20240321102518605.png)

接下来这里咱很困惑, 作者用二代基因组数据(不是重测序)分别和每个亚基因组比对来发现SNP和InDels, 是因为作者认为这三个亚基因组足够相似吗, 那为什么不直接进行全基因组比较, 是伪滑窗减少计算量吗? 为了发现等位基因吗? 但是这个材料不是单倍体吗

同时作者也从公共数据库选择了银白杨祖先二倍体的全基因组测序数据查找SNP和InDels, 其中有5.66%的变异位于外显子中, 平均23.59%的变异位于启动子区域.


### Allelic variations among the three haploid chromosomes

亚基因组间的等位基因鉴定: MCSCANX 得到的共线性块作为候选等位基因群

作者主要举例分析了以下两个基因在亚基因组内的情况

- RD22: 是脱甲酸信号传导(abscisic acid  signalling)和非生物应激(abiotic stress)的分子联系
  - ![image-20240326151710734](/img/poplar_Populus/image-20240326151710734.png)
  - B亚基因组对应基因与A和C的相比, 存在21个SNP和63碱基的缺失.
- CHI: 编码乙烯/茉莉酸介导途径(ethylene/jasmonic acidmediated signalling pathway)的几丁质酶
  - ![image-20240326173110720](/img/poplar_Populus/image-20240326173110720.png)
  - B亚基因组对应基因中间插入了丝氨酸, 同时有两个碱基突变

### Effects of salinity stress on the three haploid genomes

作者测了六个时期的转录组, 每个转录组包含三个重复, 分析前面鉴定得到的等位基因中含有差异表达基因的情况, 发现4768个等位基因至少在这六个时期中包含有一个DEG. 同时作者分析了等位基因上游2kb的序列(启动子通常位于此), 发现多态性很高, 可能可以解释部分等位基因在盐胁迫情况下的特异性表达. 同时举了LEA7(LATE EMBRYOGENESIS ABUNDANT)的例子

![image-20240328165956574](/img/poplar_Populus/image-20240328165956574.png)

## 讨论

讨论大概包括一下三个方面

- 多倍体化的重要性, 同时银中杨高质量基因组的组装是研究多倍体的优秀模型
- ABA途径中不同亚基因组基因的特异性表达可能是银中杨一些新特性形成的原因之一
- 长期营养繁殖产生的非编码突变促进其对各种环境的适应
