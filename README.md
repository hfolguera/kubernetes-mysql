# kubernetes-mysql

Repository to deploy a MySQL database on Kubernetes cluster

# Generate the mysql root password
```
kubectl create secret generic mysql-pass --from-literal=password=welcome1 -n mysql
```

# Databases
Once the MySQL instance is deployed, databases and users must have to be created manually.
Execute the following commands to connect to the MySQL instance:
```
POD=`kubectl get pod -n mysql -o jsonpath='{.items[0].metadata.name}'`
kubectl exec -it -n mysql $POD -- mysql -u root -p
```

Then, create new databases and user:
```
create database <DATABASE_NAME>;
create user '<USER>'@'%' identified by '<PASSWORD>';
grant all privileges on <DATABASE_NAME>.* to '<USER>'@'%';
```

# Backup
TODO
In order to perform a backup on a MySQL database, run the following command:
```
POD=`kubectl get pod -n mysql -o jsonpath='{.items[0].metadata.name}'`
kubectl exec -it -n mysql $POD -- /usr/bin/mysqldump -u root --password=<PASSWORD> <DB_NAME> > $DEST_PATH/mysql_${DB}_backup_${DATE}.sql
```

# Restore
To restore a database, run the following command:
```
POD=`kubectl get pod -n mysql -o jsonpath='{.items[0].metadata.name}'`
kubectl exec -i ${POD} -n mysql -- mysql -u root -p<PASSWORD> <DB_NAME> < /nfs/homelab/backup/mysql/<DUMPFILE>
```
