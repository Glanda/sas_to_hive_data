# sas_to_hive_data
/*SAS代码：*/
filename test "/home/sasdemo/sasuser.v94/credit_20150803_1.txt" encoding=utf8;

PROC EXPORT DATA= ana.credit_20150803_1
            OUTFILE= test
            DBMS=TAB REPLACE;
RUN;

--HIVE代码：
drop table if exists cf.credit_20150803_1;
create external table if not exists cf.credit_20150803_1
( 
pin_bt string,
pin_nonbt string,
type  int,
type_cnt int)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY'\t'
STORED AS TEXTFILE
;

--LINUX代码：
scp sasdemo@172.23.146.104:/home/sasdemo/sasuser.v94/credit_20150803_1.txt /export/cfgroup/credit_20150803_1.txt
(之后需要键入sas表密码

--删除第一行：
sed -i '1d' /export/cfgroup/credit_20150803_1.txt

HIVE代码：
load data local inpath '/export/cfgroup/credit_20150803_1.txt' overwrite into table cf.credit_20150803_1;
