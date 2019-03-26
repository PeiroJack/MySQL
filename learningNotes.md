# MySQL

## 1.MySQL安装

注意事项截图：

![01](C:\Users\Hpr\MySQL\pictures\1.png)

![2](C:\Users\Hpr\MySQL\pictures\2.png)

![3](C:\Users\Hpr\MySQL\pictures\3.png)

![4](C:\Users\Hpr\MySQL\pictures\4.png)

![5](C:\Users\Hpr\MySQL\pictures\5.png)

## 2.测试安装是否成功

![6](C:\Users\Hpr\MySQL\pictures\6.png)

![7](C:\Users\Hpr\MySQL\pictures\7.png)



## 3.SQLyog

![8](C:\Users\Hpr\MySQL\pictures\8.png)

## 4.SQL语句

满足需求的最小存储空间

varchar(适用于可变长度，例如身高，体重...)

char(适用不可变长度，例如手机号码(11),邮政编码(6))

区别Varchar动态的分配数据存储的空间，char固定数据存储的空间

```mysql
#创建gg15数据库
CREATE DATABASE gg15;
#打开gg15数据库
USE gg15;
#创建t_stu表
CREATE TABLE t_stu (id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, NAME VARCHAR(20) NOT NULL, PASSWORD VARCHAR(20) NOT NULL, 
role VARCHAR(20) NOT NULL, sex BOOLEAN NOT NULL);
#插入数据进t_stu
INSERT INTO t_stu (NAME, PASSWORD, role, sex)VALUES ("小明", "123456", "学生", 1);
INSERT INTO t_stu (NAME, PASSWORD, role, sex)VALUES ('小红', '123456', '学生', 0);

INSERT INTO t_stu (NAME, PASSWORD, role, sex)VALUES ('钱多多', '123456', 'java', 0);
INSERT INTO t_stu (NAME, PASSWORD, role, sex)VALUES ('多钱多', '123456', 'html', 0);
INSERT INTO t_stu (NAME, PASSWORD, role, sex)VALUES ('多多钱', '123456', 'PHP', 0);

INSERT INTO t_stu (NAME, PASSWORD, role, sex)VALUES ('小绿', '123456', '等待删除', 0);
INSERT INTO t_stu (NAME, PASSWORD, role, sex)VALUES ('小三', '123456', '等待删除', 0);
#更新数据
UPDATE t_stu SET role ='修改后的学生' WHERE NAME='小绿';
#删除
DELETE FROM t_stu WHERE role='等待删除';
#查询
SELECT * FROM t_stu WHERE id=1;
#模糊查询
SELECT * FROM t_stu WHERE NAME LIKE '小%';SELECT * FROM t_stu WHERE NAME LIKE '小%';

SELECT * FROM t_stu WHERE NAME LIKE '_钱_';


```





## 5.常见问题

### 1.在cmd里面用mysql会有编码问题，所以插入含有中文字符串数据会出错

解决方法：用SQLyog

### 2.字符串可以用单引号('')或者双引号("")吗

是的

### 3.sex用boolean还是char(1)

都可以，推荐用boolean

### 4.char(1)为啥可以存储"男"

因为5.0版本以上，varchar(20)，指的是20字符，无论存放的是数字、字母还是UTF8汉字（每个汉字3字节），都可以存放20个，最大大小是  65532字节，char也是一样？

### 5.char 和 varchar区别

CHAR(M)定义的列的长度为固定的，M取值可以为0～255之间，当保存CHAR值时，在它们的右边填充空格以达到指定的长度。当检  索到CHAR值时，尾部的空格被删除掉。在存储或检索过程中不进行大小写转换。CHAR存储定长数据很方便，CHAR字段上的索引效率级高，比如定义  char(10)，那么不论你存储的数据是否达到了10个字节，都要占去10个字节的空间,不足的自动用空格填充。

VARCHAR(M)定义的列的长度为可变长字符串，M取值可以为0~65535之间，(VARCHAR的最大有效长度由最大行大小和使用  的字符集确定。整体最大长度是65,532字节）。VARCHAR值保存时只保存需要的字符数，另加一个字节来记录长度(如果列声明的长度超过255，则  使用两个字节)。VARCHAR值保存时不进行填充。当值保存和检索时尾部的空格仍保留，符合标准SQL。varchar存储变长数据，但存储效率没有  CHAR高。如果一个字段可能的值是不固定长度的，我们只知道它不可能超过10个字符，把它定义为  VARCHAR(10)是最合算的。VARCHAR类型的实际长度是它的值的实际长度+1。为什么”+1″呢？这一个字节用于保存实际使用了多大的长度。  从空间上考虑，用varchar合适；从效率上考虑，用char合适，关键是根据实际情况找到权衡点。

CHAR和VARCHAR最大的不同就是一个是固定长度，一个是可变长度。由于是可变长度，因此实际存储的时候是实际字符串再加上一个记录  字符串长度的字节(如果超过255则需要两个字节)。如果分配给CHAR或VARCHAR列的值超过列的最大长度，则对值进行裁剪以使其适合。如果被裁掉  的字符不是空格，则会产生一条警告。如果裁剪非空格字符，则会造成错误(而不是警告)并通过使用严格SQL模式禁用值的插入。

### 6.Varchar跟数据库版本关系

MySQL  数据库的varchar类型在4.1以下的版本中的最大长度限制为255，其数据范围可以是0~255或1~255（根据不同版本数据库来定）。在  MySQL5.0以上的版本中，varchar数据类型的长度支持到了65535，也就是说可以存放65532个字节的数据，起始位和结束位占去了3个字   节，也就是说，在4.1或以下版本中需要使用固定的TEXT或BLOB格式存放的数据可以使用可变长的varchar来存放，这样就能有效的减少数据库文  件的大小。

MySQL  数据库的varchar类型在4.1以下的版本中,nvarchar（存储的是Unicode数据类型的字符）不管是一个字符还是一个汉字,都存为2个字  节 ，一般用作中文或者其他语言输入，这样不容易乱码 ;varchar: 汉字是2个字节,其他字符存为1个字节  ，varchar适合输入英文和数字。

4.0版本以下，varchar(20)，指的是20字节，如果存放UTF8汉字时，只能存6个（每个汉字3字节）  ；5.0版本以上，varchar(20)，指的是20字符，无论存放的是数字、字母还是UTF8汉字（每个汉字3字节），都可以存放20个，最大大小是  65532字节 ；varchar(20)在Mysql4中最大也不过是20个字节,但是Mysql5根据编码不同,存储大小也不同，具体有以下规则：

**a) 存储限制**

varchar 字段是将实际内容单独存储在聚簇索引之外，内容开头用1到2个字节表示实际长度（长度超过255时需要2个字节），因此最大长度不能超过65535。

**b) 编码长度限制**

字符类型若为gbk，每个字符最多占2个字节，最大长度不能超过32766;

字符类型若为utf8，每个字符最多占3个字节，最大长度不能超过21845。

若定义的时候超过上述限制，则varchar字段会被强行转为text类型，并产生warning。

**c) 行长度限制**

导致实际应用中varchar长度限制的是一个行定义的长度。 MySQL要求一个行的定义长度不能超过65535。若定义的表长度超过这个值，则提示

ERROR 1118 (42000): Row size too large. The maximum row size for the  used table type, not counting BLOBs, is 65535. You have to change some  columns to TEXT or BLOBs。

### 7.char，varchar和text的区别

长度的区别，char范围是0～255，varchar最长是64k，但是注意这里的64k是整个row的长度，要考虑到其它的  column，还有如果存在not  null的时候也会占用一位，对不同的字符集，有效长度还不一样，比如utf8的，最多21845，还要除去别的column，但是varchar在一般  情况下存储都够用了。如果遇到了大文本，考虑使用text，最大能到4G。

效率来说基本是char>varchar>text，但是如果使用的是Innodb引擎的话，推荐使用varchar代替char。

char和varchar可以有默认值，text不能指定默认值。

数据库选择合适的数据类型存储还是很有必要的，对性能有一定影响。这里在零碎记录两笔，对于int类型的，如果不需要存取负值，最好加上unsigned；对于经常出现在where语句中的字段，考虑加索引，整形的尤其适合加索引。



