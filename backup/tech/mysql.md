# mysql 笔记

## mysql 没有 grant 权限

date: 2019-10-10 01:39:01

```
show grants for root@"%";
GRANT Grant Option ON *.* TO `root`@`%`;
ERROR 1045 (28000): Access denied
```

解决：

```
select user,host,Grant_priv from user;
update mysql.user set Grant_priv="Y"  where user="root" and host="%";
GRANT Grant Option ON *.* TO `root`@`%`;
```
