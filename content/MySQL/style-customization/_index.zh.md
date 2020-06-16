---
date: 2016-04-09T16:50:16+02:00
title: 备份脚本
weight: 25
---



备份脚本

```bash
#!/bin/bash
Back_Path=/data/backup/mysqlback
LogFile=/data/backup/mysqlback/sqlbak.log
BAS=bladex
if [ ! -d $Back_Path ]; then
   mkdir -p $Back_Path
fi
cd $Back_Path
sudo echo "-------------------------------------------" >> $LogFile
sudo echo $(date +"%y-%m-%d %H:%M:%S") >> $LogFile
sudo echo "开始备份！" >> $LogFile
/usr/local/mysql/bin/mysqldump  --no-defaults  -uroot -poneinstack -F $BAS  > $BAS$(date +%Y_%m_%d).sql && tar -zcf $BAS$(date +%Y_%m_%d).tar.gz $BAS$(date +%Y_%m_%d).sql && rm -rf $BAS$(date +%Y_%m_%d).sql
#mysqldump   -uroot -pKeYpZrZx --database weskit  > weskit_$(date +%Y_%m_%d).sql
find $Back_Path -mtime +20 -name "*.gz" -exec rm -rf {} \;
echo $(date +"%y-%m-%d %H:%M:%S") >> $LogFile
echo "备份结束！" >> $LogFile
echo "-------------------------------------------" >> $LogFile
```

