Our website:html+css+php+MySQL

1.Get summary of Gene from Entrez through biopython

```python
import pymysql
from Bio import Entrez
dbhost = "localhost"
dbuser = "xxxxxxxx"
dbpasswd = "xxxxxx"
dbname="xxxxxx"
Entrez.email = "xxxxxxx@outlook.com"     # Always tell NCBI who you are
handle=Entrez.esearch(db="Gene",term="txid3702[Organism:noexp]",retmax=100000)
record = Entrez.read(handle)
idlists=record["IdList"]
for gid in idlists:
    net_handle = Entrez.efetch(db="Gene", id=str(gid), retmode="text")
    out_handle = open("result.txt", "a")
    out_handle.write(net_handle.read())
    out_handle.close()
    net_handle.close()
    print("saved %s"%(str(gid)))

```
2.Use python script to read the result and insert into SQL

```python
# one streip to read ncbi gene summary
import csv
import pymysql
FileName="/home/with9/Documents/bioinformatic/biopython/gene_result4577.txt"
Taxid=4577
dbhost = "localhost"
dbuser = "xxxxxx"
dbpasswd = "xxxxxx"
dbname="xxxxxx"
db = pymysql.connect(host=dbhost, user=dbuser,password=dbpasswd, database=dbname)
cursor = db.cursor()
with open(FileName, "r")as f:
    one_gene = []
    one_g = {}
    for line in f:
        if line != "\n":
            one_gene.append(line[:-1])
        else:
            if one_gene:
                one_g["Name"] = one_gene[0].split(".")[1]
                one_g["Taxid"] = str(Taxid)
                for item in one_gene:
                    simple_split = item.split(":", 2)
                    if simple_split[0] == "ID":
                        one_g["ID"] = (simple_split[1])
                    elif simple_split[0] == "Annotation":
                sql = 'insert into gid(id,`Name`,`Annotation`,taxid) values(' + \
                    one_g["ID"]+",'"+one_g["Name"]+"','" + \
                    one_g["Annotation"]+"',"+one_g["Taxid"]+")"
                try:
                    cursor.execute(sql)
                    db.commit()
                except:
                    db.rollback()
            one_gene = []
db.close()
```

3.update data and build up this website