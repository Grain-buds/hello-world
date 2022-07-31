MySQL的 JDBC URL格式：
https://www.cnblogs.com/EasonJim/p/7659475.html
https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-reference-configuration-properties.html



问题：
1、在mysql-connector-java 5.xxx该参数默认为true，在6.xxx以上默认为false，因此需要设置nullCatalogMeansCurrent=true
http://www.manongjc.com/detail/26-gilqnnfbihawqnb.html