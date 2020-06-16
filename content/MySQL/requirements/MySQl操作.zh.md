---
title: 	MySQL操作
weight: 10
---

---
命令行：mysql

```bash
mysql -u root -p -h 10.0.0.1 -P 3306

--host=host_name,    -h host_name：服务端地址；
--user=user_name,    -u user_name：用户名；
--port=port_num,     -P port_num：服务端端口；
--socket=path,       -S path
--database=db_name,  -D db_name：
--compress,          -C：数据压缩传输
--execute=statement, -e statement：非交互模式执行SQL语句；
--vertical,          -E：查询结果纵向显示；
--protocol={TCP|SOCKET|PIPE|MEMORY}：
```

