### Online
````
1. Add the network addresses of the new nodes to the include file
   dfs.hosts
   hdfs-site.xml
2. hdfs dfsadmin -refreshNodes
3. yarn rmadmin -refreshNodes
4. Update the slaves fil with the new nodes
5. ssh newNode hadoop-daemon.sh start datanode | yarn-daemon.sh start nodemanager
6. hdfs balancer --help
````

Balancer bandwith
````
<property>
  <name>dfs.datanode.balance.bandwidthPerSec</name>
  <value>1048576</value>
  <description>
        Specifies the maximum amount of bandwidth that each datanode
        can utilize for the balancing purpose in term of
        the number of bytes per second.
  </description>
</property>
````

### Offline
````
1. Add the network addresses of the nodes to be decommisioned to the exclude file.
   Do not update the include file at this point.
2. hdfs dfsadmin -refreshNodes
3. yarn rmadmin -refreshNodes
4. Go to the webUI and check whether the admin state has changed to "Decommission in Progress"
5. Wait the status to "Decommissioned"
   hadoop-daemon.sh stop datanode
   yarn-daemon.sh stop nodemanager
6. Remove the nodes from the include file(dfs.hosts)
   hdfs dfsadmin -refreshNodes
   yarn rmadmin -refreshNodes 
````