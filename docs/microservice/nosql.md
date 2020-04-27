------



# Redis
# MongoDB

## 数据迁移

生产：
./mongod -config /home/mongodb/etc/mongo.conf -dbpath /home/mongodb/data/ -logpath=/home/mongodb/logs/mongo.log -logappend -port=27017 -fork -auth



测试：
./mongod -config /home/mongodb/etc/mongo.conf -dbpath /home/mongodb/data/ -logpath=/home/mongodb/logs/mongo.log -logappend -port=27017 -fork -auth


第一步：
在RDS上，创建临时表，创建当前地市的零售户数据；
CREATE TABLE uc_cst_customer0829 AS 
SELECT
	cust_uuid,
	license_code,
	cust_code,
	manage_unit_uuid,
	cust_name,
	address,
	trade_scope_code,
	other_trade_scope,
	status_code,
	area_code,
	area_name,
	is_active,
	terminal_level,
	gis_long,
	gis_lat,
	sysisdelete,
	data_gather_type,
	busi_place_code,
	sub_busi_code,
	trade_mode_code,
	market_type,
	sub_market_type,
	cust_type_uuid,
	cust_type_name,
	order_type_code,
	is_sample_cust,
	sample_level,
	position_code,
	sub_position_code,
	is_nonsmoke,
	is_sale_cigar,
	is_collect,
	is_zfb,
	limitorder_type
FROM
	uc_cst_customer
WHERE
	manage_unit_uuid = '00000000000000000000000000000009'


第二步：
登录客户端工具navicat，导出步骤一中生成的数据，文件格式为CSV文件。


第三步：
把CSV文件上传到mongo服务器的/home目录


第四步：
执行导入命令：
mongoimport -h 127.0.0.1 -d eapdb -u root -p EAP_mongo_1688 -c uc_cst_customer --type csv --columnsHaveTypes --fields "cust_uuid.string(),license_code.string(),cust_code.string(),manage_unit_uuid.string(),cust_name.string(),address.string(),trade_scope_code.string(),other_trade_scope.string(),status_code.string(),area_code.string(),area_name.string(),is_active.string(),terminal_level.string(),gis_long.string(),gis_lat.string(),sysisdelete.string(),data_gather_type.string(),busi_place_code.string(),sub_busi_code.string(),trade_mode_code.string(),market_type.string(),sub_market_type.string(),cust_type_uuid.string(),cust_type_name.string(),order_type_code.string(),is_sample_cust.string(),sample_level.string(),position_code.string(),sub_position_code.string(),is_nonsmoke.string(),is_sale_cigar.string(),is_collect.string(),is_zfb.string(),limitorder_type.string()" --file /home/uc_cst_customer0918.csv --upsert

mongoimport -h 127.0.0.1 -d testdb -c uc_cst_customer --type csv --columnsHaveTypes --fields "cust_uuid.string(),license_code.string(),cust_code.string(),manage_unit_uuid.string(),cust_name.string(),address.string(),trade_scope_code.string(),other_trade_scope.string(),status_code.string(),area_code.string(),area_name.string(),is_active.string(),terminal_level.string(),gis_long.string(),gis_lat.string(),sysisdelete.string(),data_gather_type.string(),busi_place_code.string(),sub_busi_code.string(),trade_mode_code.string(),market_type.string(),sub_market_type.string(),cust_type_uuid.string(),cust_type_name.string(),order_type_code.string(),is_sample_cust.string(),sample_level.string(),position_code.string(),sub_position_code.string(),is_nonsmoke.string(),is_sale_cigar.string(),is_collect.string(),is_zfb.string(),limitorder_type.string()" --file /home/uc_cst_customer0531.csv --upsert

第五步：
mongo
use eapdb

-- 转换gis_long和gis_lat字段类型
db.uc_cst_customer.find({ $and: [{ gis_long: { $exists: true } }, { gis_long: { $ne: null } }, { gis_long: { $ne: "" } },{ manage_unit_uuid:'00000000000000000000000000000011'} ] }).forEach(
   function(item){                 
       db.uc_cst_customer.update({"_id":item._id},{"$set":{"gis_long":parseFloat(item.gis_long),"gis_lat":parseFloat(item.gis_lat)}},false,true) 
    }
);
-- 增加loc字段
db.uc_cst_customer.find({ $and: [ { loc: { $exists:false } }, { gis_long: { $ne: null } }, { gis_long: { $ne: "" } } ] }).forEach(
   function(item){                 
       db.uc_cst_customer.update({"_id":item._id},{"$set":                    {"loc":{type:"Point",coordinates: [item.gis_long, item.gis_lat]}}},false,true) 
    }
);
-- 去掉area_code里面的空格
db.uc_cst_customer.find({ area_code: / $/ }).forEach(
   function(item){                 
       db.uc_cst_customer.update({"_id":item._id},{"$set":                    {"area_code":item.area_code.trim()}},false,true) 
    }
);
db.uc_cst_customer.find({is_active:{$exists : false}}).forEach(
   function(item){                 
       db.uc_cst_customer.update({"_id":item._id},{"$set":                    {"is_active":"1"}},false,true) 
    }
);

## 用户名密码auth

mongo-auth用户名密码

mongo中字段以下角色：
这些角色对应的作用如下：

Read：允许用户读取指定数据库
readWrite：允许用户读写指定数据库
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
root：只在admin数据库中可用。超级账号，超级权限






第一步需要创建管理员用户，使用管理员db
use admin // 切换到admin
db.createUser({user:'root',pwd:'Pa55w0rdzjyc',roles:['userAdminAnyDatabase']})
db.auth('root','Pa55w0rdzjyc')

use eapdb// 使用业务db
db.createUser({user:'root',pwd:'Admin123',roles:['readWrite']})
db.auth('root','Admin123')


开启auth的话，需要重启mongo

./mongod -config /home/mongodb/etc/mongo.conf -dbpath /home/mongodb/data/ -logpath=/home/mongodb/logs/mongo.log -logappend -port=27017 -fork -auth


use test
db.updateUser(
   "root",
   {
      pwd: "EAP_mongo_1688ut"
   }
)


今晚修改mongodb密码的步骤：

1、用ssh工具登录mongo服务器
	ip：10.44.42.180
	账号：root
	密码：Pa55w0rdzjyc
2、登录后在任意目录下敲入以下命令，进入管理员身份
	mongo		回车
	use admin 	回车
	db.auth('root','Pa55w0rdzjyc')	回车
3、修改eap库的密码
	use eapdb 	回车

db.updateUser(
   "root",
   {
      pwd: "EAP_mongo_1688"
   }
)
回车
4、修改nc库的密码
	use ncdb 	回车

db.updateUser(
   "root",
   {
      pwd: "EAP_mongo_1688"
   }
)
回车
5、测试密码修改是否成功
use eapdb	回车
db.auth('root','EAP_mongo_1688') 	回车


