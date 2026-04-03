# draft
rpm -qa | grep -E "python3|python36|devel" | sort

把python3-devel-3.6.8-21.el7_9.x86_64.rpm上传到服务器，采用如下命令安装
yum -y localinstall python3-devel-3.6.8-21.el7_9.x86_64.rpm

代码文件分两块，
yashandb-python: https://github.com/yashan-technologies/yashandb-python
yashandb-yacapi：https://github.com/yashan-technologies/yashandb-yacapi

编译打包
cd /data/install/yashandb-python/yashandb-python

python3.6 setup.py build_ext --inplace

GRANT CREATE SESSION TO connect_user;
GRANT SELECT ON SYS.V_$DATABASE TO connect_user;
GRANT SELECT ON SYS.V_$TRANSACTION TO connect_user;
GRANT SELECT ON SYS.V_$YSTREAM_SERVER TO connect_user;
GRANT FLASHBACK ANY TABLE TO connect_user; 
GRANT SELECT ANY TABLE TO connect_user;
GRANT YSTREAM_CAPTURE TO connect_user;
GRANT ALTER SESSION TO connect_user;
GRANT SELECT ON SYS.DBA_LOBS TO connect_user;
GRANT SELECT ON SYS.V_$DATAFILE TO connect_user;
GRANT SELECT ON SYS.DBA_SEGMENTS TO connect_user;
GRANT FLASHBACK ON SYS.DBA_SEGMENTS TO connect_user;
GRANT FLASHBACK ON SYS.DBA_LOBS TO connect_user;
GRANT SELECT ON SYS.DBA_TABLES TO connect_user;
GRANT FLASHBACK ON SYS.DBA_TABLES TO connect_user;
GRANT SELECT ON SYS.DBA_TAB_PARTITIONS TO connect_user;
GRANT FLASHBACK ON SYS.DBA_TAB_PARTITIONS TO connect_user;
GRANT SELECT ON SYS.DBA_TAB_SUBPARTITIONS TO connect_user;
GRANT FLASHBACK ON SYS.DBA_TAB_SUBPARTITIONS TO connect_user; 


https://github.com/yashan-technologies/yashandb-python
https://docs.python.org/zh-cn/3/extending/index.html#extending-index
https://docs.python.org/zh-cn/3/c-api/index.html

CREATE USER NACOS_V312 IDENTIFIED BY 123456
GRANT DBA TO NACOS_V312;
GRANT ALL PRIVILEGES TO NACOS_V312;


python3 setup.py clean -a
python3 setup.py bdist_wheel
pip3 install dist/yaspy-xxxx.whl



测试代码：test_yaspy_crud_simple.py
# -*- coding: utf-8 -*-
import yaspy

DSN = "172.16.xxx.xxx:1688"   # 改成你的 host:port
USER = "your_user"
PASSWORD = "your_password"
TABLE = "t_yaspy_demo"

def main():
    conn = yaspy.connect(dsn=DSN, user=USER, password=PASSWORD)
    try:
        cur = conn.cursor()

        cur.execute(f"drop table if exists {TABLE}")
        cur.execute(f"create table {TABLE} (id int primary key, msg varchar(200))")
        conn.commit()

        cur.execute(f"insert into {TABLE} (id, msg) values (1, 'hello yaspy')")
        conn.commit()

        cur.execute(f"select id, msg from {TABLE} where id = 1")
        row = cur.fetchone()
        print("read row:", row)

        cur.execute(f"drop table if exists {TABLE}")
        conn.commit()

        cur.close()
    finally:
        conn.close()

if __name__ == "__main__":
    main()
