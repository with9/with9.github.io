- HTML
  - 明白基本的html语法,可以用html写简单的文档,插入图片,超链接等
- PHP
  - 基本语法,变量,循环,字符串拼接,链接SQL
- SQL
  - 可以从命令行创建数据库,表
  - 设置主键,插入数据
  - select基本语法,可以通过like进行模糊查找
- python
  - 读取并处理简单的文本文件,其他语言也可
  - *biopython的Entrez模块*(大概了解即可)

强烈安利学一下git,几十分钟,终身收益. 版本管理真的非常方便.

```shell
cd /opt/lampp/htdocs/students/2020280....../homework1
rm * -rf
git clone https://gitee.com/with9/bioweb.git .
chmod 755 ./ -Rv
```





```php
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 's202028012215001';            // mysql用户名
$dbpass = '215001';          // mysql用户名密码
$dbname='s202028012215001';
$conn = mysqli_connect($dbhost, $dbuser, $dbpass,$dbname);
$sql="SELECT * FROM `s202028012215001`.`gid` WHERE CONVERT(`Name` USING utf8) LIKE '%".$_POST["like_name"]."%' ORDER BY `id`";
// echo $sql;
$res=mysqli_query($conn,$sql);
while($myrow=mysqli_fetch_assoc($res)){
    echo "<tr>";
    echo "<td><a href='https://www.ncbi.nlm.nih.gov/gene/".$myrow['id']."'>".$myrow['id']."</a></td>";
    echo "<td>".$myrow['Name']."</td>";
    echo "<td>".$myrow['Annotation']."</td>";
    echo "<td>".$myrow['taxid']."</td>";
    echo "</tr>";
}
?>
```

