# ProxySQL Setup

## Problem Statement
Create 3 mySql instances (1 master, 2 replicas). Use proxysql to route queries to mysql instances. 
write queries should go to master and read queries should go to replica


## Steps to Follow

1. Setup replica by following README in this repo : https://github.com/peruljain/mysql-replication 
2. go cli of proxysql -> mysql -u admin -padmin -h localhost -P6032 --prompt='Admin> '
2. UPDATE mysql_users SET default_hostgroup=1; # by default, all goes to HG1
4. LOAD MYSQL USERS TO RUNTIME;
5. SAVE MYSQL USERS TO DISK; # if you want this change to be permanent
6. INSERT INTO mysql_query_rules (rule_id,active,match_digest,destination_hostgroup,apply)
   VALUES
   (1,1,'^SELECT.*FOR UPDATE$',1,1),
   (2,1,'^SELECT',2,1);
7. INSERT INTO mysql_query_rules (rule_id, active, match_pattern, destination_hostgroup, apply) 
   VALUES (3, 1, 'INSERT INTO', 1, 1);
8. INSERT INTO mysql_query_rules (rule_id, active, match_pattern, destination_hostgroup, apply) 
   VALUES (4, 1, ‘UPDATE', 1, 1);
9. LOAD MYSQL QUERY RULES TO RUNTIME;
10. SAVE MYSQL QUERY RULES TO DISK; # 
