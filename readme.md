# HadoopMapr
My project involves creating 6 VMs with Vagrant and installing MapR on them using Ansible.

**VM Details:**
  - **node01.cluster.local**: IP `192.168.50.101`, Ubuntu, 1 NFS disk
  - **node02.cluster.local**: IP `192.168.50.102`, Ubuntu, 3 NFS disks
  - **node03.cluster.local**: IP `192.168.50.103`, Ubuntu, 1 NFS disk
  - **edge.cluster.local**: IP `192.168.50.199`, Ubuntu, 1 NFS disk
  - **node04.cluster.local**: IP `192.168.50.104`, Ubuntu, 1 NFS disk
  - **node05.cluster.local**: IP `192.168.50.105`, Ubuntu, 1 NFS disk


- **VM Roles:**
  - `ApiServer_nodes`: edge.cluster.local
  - `CLDB_nodes`: node01.cluster.local
  - `Drill_nodes`: node03.cluster.local
  - `FileServer_nodes`: node01.cluster.local, node02.cluster.local, node03.cluster.local
  - `Gateway_nodes`: edge.cluster.local
  - `Hive_nodes`: node03.cluster.local
  - `HistoryServer_nodes`: node01.cluster.local
  - `NFS_nodes`: node01.cluster.local, node02.cluster.local, node03.cluster.local
  - `NodeManager_nodes`: node02.cluster.local
  - `WebServer_nodes`: edge.cluster.local
  - `ZooKeeper_nodes`: node01.cluster.local, node04.cluster.local, node05.cluster.local
  - `Licence_node`: node01.cluster.local
  - `edge_node`: edge.cluster.local  


For any questions or support, please contact [khemilioussama775@gmail.com]