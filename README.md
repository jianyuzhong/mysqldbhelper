# Mysql Dbhelper

* ocdbhelper ,mysql dbhelper with python*

---

## Constructor

```python

from mysql_helper import DBHelper
db_conf = {
            'user' : 'root',
            'passwd' : '123456',
            'host' : 'localhost',
            'schema' : 'IDataBase',
            'charset' : 'utf8mb4'
        }
# open a connect
idatabase=DBHelper(**db_conf)
# open anther connect
idatabase1=DBHelper(**db_conf)

```

*多线程共用一个连接的时候需要 加锁。*

```python
lock = threading.Lock()
  
def Selectxxx(self,helper:DBHelper):
        self.lock.acquire()
        try:
            sql='select * from xxx*'
            helper.execute(sql)
            logger.info(f"code:{self.code}, name:{self.name}")
        except Exception as ex:
            logger.error(f"insert into data error with messages {str(ex)} ")
        finally:
            self.lock.release()

```

*不过建议多个链接然后关掉*

```
from mysql_helper import DBHelper
db_conf = {
            'user' : 'root',
            'passwd' : '123456',
            'host' : 'localhost',
            'schema' : 'IDataBase',
            'charset' : 'utf8mb4'
        }
# open a connect
idatabase=DBHelper(**db_conf)
idatabase.excute(sql)
idatabase.close()
# open anther connect
idatabase1=DBHelper(**db_conf)
idatabase1.close()
```

---

## Function

---

* 带参数执行sql脚本

```python

        sql='insert into ifund (Charge,available,code,dflow,date,ipoint,mflow,name,price,publishtime,top,wflow,variance,wavg,mavg,url,lowlevel,cguess)'
        sql+=f" values(%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"
        params=[s_data.Charge,s_data.available,s_data.code,s_data.d_flow,s_data.date,s_data.ipoint,s_data.m_flow,s_data.name,s_data.price,s_data.publishtime,s_data.top,s_data.w_flow,s_data.variance,s_data.w_avg,s_data.m_avg,s_data.url,s_data.lowlevel,s_data.cguess]
        con=DBHelper(**self._db_conf)
        con.execute(sql=sql,params=params)
        con.close()
```



* 管理事务（默认情况下是自动管理事务）python

```python
with DBHelper(**db_conf) as db:
    db.begin() # 开启事务
    db.execute(sql)
    db.execute_batch(sql)
    db.insert_batch(sql)
    db.end() # 关闭事务，这里一定要记得关闭操作，关闭操作包括提交事务和还原成自动管理事务
```

* **获取字典 get_dicts(self, sql, params=None, convert=True)**

```python
with DBHelper(**db_conf) as db:
     data=db.get_dicts(sql,params)
```
