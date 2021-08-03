
一般二代调研中需要判断一下测序样品的污染情况，主要的原理就是对样品的测序数据随机抽取并进行blast，分析reads的来源情况，从而判断样品污染情况。

主要流程参考[这篇博客](https://blog.csdn.net/qq_42962326/article/details/105081327)

## 前期数据库下载及预处理

### nt库下载

```python
#nt.41.tar.gz
#https://ftp.ncbi.nlm.nih.gov/blast/db/nt.20.tar.gz
#----down.py
for i in range(42):
    if i <10:
        url="https://ftp.ncbi.nlm.nih.gov/blast/db/nt.{}{}.tar.gz".format('0',str(i))
        print(url)
    else:
        url="https://ftp.ncbi.nlm.nih.gov/blast/db/nt.{}.tar.gz".format(str(i))
        print(url)
```

```shell
python down.py>down
aria2c --input=down #进行下载
#分别解压
```

### 物种名与基因id对应数据库下载

```shell
aria2c 'https://ftp.ncbi.nlm.nih.gov/pub/taxonomy/accession2taxid/nucl_gb.accession2taxid.gz'#基因id对应taxonomy id
aria2c 'https://ftp.ncbi.nlm.nih.gov/pub/taxonomy/taxdump.tar.gz'#taxonomy id 对应学名
#分别解压
#---head nucl_gb.accession2taxid
accession	accession.version	taxid	gi
A00001	A00001.1	10641	58418
A00002	A00002.1	9913	2
A00003	A00003.1	9913	3
A00004	A00004.1	32630	57971
A00005	A00005.1	32630	57972
A00006	A00006.1	32630	57973
A00008	A00008.1	32630	57974
A00009	A00009.1	32630	57975
A00010	A00010.1	32630	57976

#---head taxdump/names.dmp
1	|	all	|		|	synonym	|
1	|	root	|		|	scientific name	|
2	|	Bacteria	|	Bacteria <bacteria>	|	scientific name	|
2	|	bacteria	|		|	blast name	|
2	|	eubacteria	|		|	genbank common name	|
2	|	Monera	|	Monera <bacteria>	|	in-part	|
2	|	Procaryotae	|	Procaryotae <bacteria>	|	in-part	|
2	|	Prokaryotae	|	Prokaryotae <bacteria>	|	in-part	|
2	|	Prokaryota	|	Prokaryota <bacteria>	|	in-part	|
2	|	prokaryote	|	prokaryote <bacteria>	|	in-part	|
2	|	prokaryotes	|	prokaryotes <bacteria>	|	in-part	|
6	|	Azorhizobium Dreyfus et al. 1988 emend. Lang et al. 2013	|		|	authority	|
6	|	Azorhizobium	|		|	scientific name	|
7	|	ATCC 43989	|	ATCC 43989 <type strain>	|	type material	|
7	|	Azorhizobium caulinodans Dreyfus et al. 1988	|		|	authority	|
7	|	Azorhizobium caulinodans	|		|	scientific name	|
7	|	Azotirhizobium caulinodans	|		|	equivalent name	|
7	|	CCUG 26647	|	CCUG 26647 <type strain>	|	type material	|
7	|	DSM 5975	|	DSM 5975 <type strain>	|	type material	|
7	|	IFO 14845	|	IFO 14845 <type strain>	|	type material	|

rga "scientific name" -g "names.dmp" >names_scientific#获取scientific name相应的行
```

## 对测序数据随机抽取并blast

```shell
#从fastq文件中抽取5000条数据
cat 1.fastq | head -n 20000 | awk '{if(NR%4==1){print ">"$1}else if(NR%4==2){print $0}}'|sed 's/@//g' > myfile.fa
#---head myfile.fa
>SRR15225403.1
TTATCGTAGATTTTTTGAATATGTTGTAACGAAAGCTGAGCCATGTTGATTCCTTTGCTATACGTTTTGCCAGAAGGCCGATAACCGCGGCCAGGTGAGTGATTCCGTTGAAGGCAACAGTAATTGCGCCCCGGTTAAGCCCGCGCCGATCCCCACCGAGCGCATACCGCTGGCGTTAATGGCGTCAATGCCCGCCTGCGCATCTTCAATGCCGATACATGCCTGCGGCGGCAC
>SRR15225403.2
GAAGGTTACTGCGGCTCCTGTCGCACCCGTCTGGTTGCAGGTCAGGTTGACTGGATTGCCGAACCGTTAGCCTTTATTCAGCCGGGGGAAATTTTGCCCTGTTGTTG
>SRR15225403.3
CGGTAAACGCCGTCAGGGAGATGGCGCAACCAATCGCCAACGGCAGATTAGCCCACAGACCCATCACGATAGAACCGAGTCCGGCAACCAGACAGGTCGCAACGAAAACTGCCGCAGGCGGGAAGCCCGCTTTACCCAACATGCCTGGAACGACGATAACCGAGTAGACCATCGCCAGAAACGTTGTTAACCCGGCAACCACTTCCTGACGGACAGTGCTTCCACGTTGTGAAAT
>SRR15225403.4
CAGGCGAAAGCCGCGGCGGTAAAAGTCCACGCTGACGCGCCCGGTACGTTTTATTGCGGATGTAAAATTAACTGGCAGGGCAAAAAAGGCGTTGTTGATCTGCAATCGTGCGGCTATCAGGTGCGCAAAAATGAAAACCGAGCCAGCCGCGTAGAGTGGGAACACGTCGTTCCCGCCTGGCAGTTCGGTCACCAGCGCCAGTGCTGGCAGGACGGGGGACGTAAAAACTGCGCTA
>SRR15225403.5
TGTATCTGAATCCGCAAGATTGCAGCGTGATTAATGATGAAGCGCTGAATCGTATTATCGCCGTAGGCCACCAGCATCATCTGAACGTTGTCGGCTGGAACCCGGGACCGGCGCTTTCAGTTAGCATGGGCGACATGCCGGATGATGGCTACAAACCATTGGTTGGGTAGAAAAGGCCTACGGCTTCGGAACGGCAAAAGGGAACAAAAGGGAAACTGCCACACCTGGCGAATC

#进行blastn 并将结果保存为xml格式
blastn -query myfile.fa -out xml/out00.xml -max_target_seqs 1 -outfmt 5 -db ~/usb/blastn/nt.00/nt.00 -num_threads 2 -evalue 1e-5

#---head out00.xml
<?xml version="1.0"?>
<!DOCTYPE BlastOutput PUBLIC "-//NCBI//NCBI BlastOutput/EN" "http://www.ncbi.nlm.nih.gov/dtd/NCBI_BlastOutput.dtd">
<BlastOutput>
  <BlastOutput_program>blastn</BlastOutput_program>
  <BlastOutput_version>BLASTN 2.12.0+</BlastOutput_version>
  <BlastOutput_reference>Zheng Zhang, Scott Schwartz, Lukas Wagner, and Webb Miller (2000), &quot;A greedy algorithm for aligning DNA sequences&quot;, J Comput Biol 2000; 7(1-2):203-14.</BlastOutput_reference>
  <BlastOutput_db>/home/with9/usb/blastn/nt.00/nt.00</BlastOutput_db>
  <BlastOutput_query-ID>Query_1</BlastOutput_query-ID>
  <BlastOutput_query-def>SRR15225403.1</BlastOutput_query-def>
  <BlastOutput_query-len>234</BlastOutput_query-len>

```

编写python脚本处理out.xml文件,将gid,hit_def和hit_accession写入csv文件

```python
import xml.etree.ElementTree as ET
import csv
import os

def readonexml(fid:int):
    if fid<10:
        xmlfile="xml/out0{}.xml".format(fid)
        csvfile="csv/out0{}.csv".format(fid)
    else:
        xmlfile="xml/out{}.xml".format(fid)
        csvfile="csv/out{}.csv".format(fid)

    tree = ET.parse(xmlfile)
    root = tree.getroot()
    hits=root.findall('.//Hit')

    with open(csvfile,'w',newline="")as f:
        csv_writer = csv.writer(f)
        csv_writer.writerow(["Hit_id","Hit_def","Hit_accession"])

        for hit in hits:
            hit_id=hit.find('Hit_id').text
            hit_def=hit.find('Hit_def').text
            hit_accession=hit.find('Hit_accession').text
            csv_writer.writerow([hit_id,hit_def,hit_accession])

readonexml(0)

#--head csv/out00.csv
Hit_id,Hit_def,Hit_accession
gi|85674274|dbj|AP009048.1|,"Escherichia coli str. K-12 substr. W3110 DNA, complete genome",AP009048
gi|85674274|dbj|AP009048.1|,"Escherichia coli str. K-12 substr. W3110 DNA, complete genome",AP009048
gi|81239530|gb|CP000034.1|,"Shigella dysenteriae Sd197, complete genome",CP000034
gi|81244029|gb|CP000036.1|,"Shigella boydii Sb227, complete genome",CP000036
gi|30043918|gb|AE014073.1|,"Shigella flexneri 2a str. 2457T, complete genome",AE014073
gi|30043918|gb|AE014073.1|,"Shigella flexneri 2a str. 2457T, complete genome",AE014073
gi|81244029|gb|CP000036.1|,"Shigella boydii Sb227, complete genome",CP000036
gi|85674274|dbj|AP009048.1|,"Escherichia coli str. K-12 substr. W3110 DNA, complete genome",AP009048
gi|30043918|gb|AE014073.1|,"Shigella flexneri 2a str. 2457T, complete genome",AE014073

```

利用Hit_accession从nucl_gb.accession2taxid中获取taxonomyid,因为文件过大,所以利用os模块调用[rga](https://github.com/phiresky/ripgrep-all)工具进行查找

```python
#统计出所有需要查找的Hit_accession,因为文件过大,所以利用os模块调用rga工具进行查找
import pandas as pd
import os
df=pd.read_csv('csv/out00.csv')
with open('Hitaccession.csv','w')as f:
    name_df=df['Hit_accession'].value_counts()
    name_df.to_csv(f)
    

df=pd.read_csv('Hitacession.csv')
df['tax_id']=0
tids=[]
for index,row in df.iterrows():
    hit_accession=row[0]
    cmd="rga '"+hit_accession+"' -g 'nucl_gb.accession2taxid.gz'|awk '{print$3}'"
    tid=os.popen(cmd).read().split('\n')[0]
    print(tid)
    tids.append(tid)

df['tax_id']=tids
df.to_csv('Tid_result,csv')
```

对一开始的读取xml脚本进行修改,同时获取物种id和物种的学名.

```python
import xml.etree.ElementTree as ET
import csv
import os

def readonexml(fid:int):
    if fid<10:
        xmlfile="xml/out0{}.xml".format(fid)
        csvfile="csv/out0{}.csv".format(fid)
    else:
        xmlfile="xml/out{}.xml".format(fid)
        csvfile="csv/out{}.csv".format(fid)

    tree = ET.parse(xmlfile)
    root = tree.getroot()
    hits=root.findall('.//Hit')

    with open(csvfile,'w',newline="")as f:
        csv_writer = csv.writer(f)
        csv_writer.writerow(["Hit_id","Hit_def","Hit_accession","taxonomy_id","scientific_name"])

        for hit in hits:
            hit_id=hit.find('Hit_id').text
            hit_def=hit.find('Hit_def').text
            hit_accession=hit.find('Hit_accession').text
            cmd="grep '{}' Tid_result.csv |cut -d , -f 4".format(hit_accession)
            taxonomy_id=os.popen(cmd).read().split('\n')[0]
            print(taxonomy_id)
            cmd2="grep ':{}' names_scientific |cut -d \| -f 2".format(taxonomy_id)
            scientific_name=os.popen(cmd2).read().split('\t')[1]
            csv_writer.writerow([hit_id,hit_def,hit_accession,taxonomy_id,scientific_name])
readonexml(0)
```

统计结果并绘图

```python
import pandas as pd

df=pd.read_csv('csv/out00.csv')
with open('temp_with_name.csv','w')as f:
    name_df=df['scientific_name'].value_counts()
    name_df.to_csv(f)

name_df.plot.pie()
df=pd.read_csv('temp_with_name.csv')
df['points']=df['scientific_name']/5000*100
df=df.set_axis(["scientific_name","hits","points"],axis=1)
with open("point.csv","w")as f:
    df.to_csv(f,float_format='%.3f',index=False)
#---head point.csv
scientific_name,hits,points
Escherichia coli str. K-12 substr. W3110,1785,35.700
Shigella boydii Sb227,668,13.360
Shigella flexneri 2a str. 2457T,614,12.280
Shigella sonnei Ss046,438,8.760
Escherichia coli,244,4.880
Escherichia coli CFT073,242,4.840
Escherichia coli O157:H7 str. EDL933,233,4.660
Shigella dysenteriae Sd197,149,2.980
Escherichia coli K-12,55,1.100

```

