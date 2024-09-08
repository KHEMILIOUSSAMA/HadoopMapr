![MAP_logo](https://awsmp-logos.s3.amazonaws.com/802b0a25-877e-4b57-9007-a3fd284815a5/0d64f493222427748b7c21e0104763be.png)


**Projet JEMS:** Création d'un mirroring de volume de A vers A'

Ce projet met en place un cluster Hadoop MapR avec mirroring de volumes pour assurer la haute disponibilité des données. 
Le déploiement est automatisé avec Ansible et utilise des machines virtuelles sous Vagrant.

➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖

- **Fonctionnalités clés:**
    - **Création des VMs avec Vagrant**: Déploiement et gestion des machines virtuelles pour le cluster à l'aide de Vagrant
    - **Automatisation Ansible**: Provisionnement, configuration et gestion complète du cluster via des playbooks Ansible
    - **Mirroring de Volume**: Réplication automatique des données d'un volume A vers un volume A' avec MapR pour garantir la  disponibilité.

➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖

- **Arborescence du projet:**

```
+---ansible
|   +---add_services.yml
|   +---ansible.cfg
|   +---deploy_mapr.yml
|   +---inventory.ini
|   +---inventory_creation.sh
|   +---manip.md
|   +---restart_history_server.yml
|   +---start_cluster.yml
|   
|   +---mapr_cluster_setup
|       +---defaults
|       +---files
|       +---handlers
|       +---meta
|       |   +---main.yml
|       +---tasks
|       |   +---attach_disks.yml
|       |   +---configure_cluster.yml
|       |   +---main.yml
|       |   +---modify_config_file.yml
|       |   +---start_cluster.yml
|       |   +---upload_license.yml
|       +---tests
|       |   +---inventory
|       |   +---test.yml
|       +---vars
|           +---main.yml
|   
|   +---mapr_configuration
|       +---template_inventory
|       +---defaults
|       +---files
|       |   +---maprgpg.key
|       +---handlers
|       +---meta
|       |   +---main.yml
|       +---tasks
|       |   +---add_repository_credential.yml
|       |   +---create_user_group.yml
|       |   +---get_license.yml
|       |   +---install_mapr_services.old.yml
|       |   +---install_mapr_services.yml
|       |   +---install_utilities.yml
|       |   +---main.yml
|       |   +---open_port.yml
|       |   +---update_hosts.yml
|       +---tests
|       +---vars
|           +---main.yml
|   
+---vagrant
|   +---config.yml
|   +---setup_ansible_user.sh
|   +---vagrantfile
|   
|   +---.vagrant
|       +---machines
|           +---edge.cluster.local
|           |   +---virtualbox
|           |       +---action_provision
|           |       +---action_set_name
|           |       +---box_meta
|           |       +---creator_uid
|           |       +---id
|           |       +---index_uuid
|           |       +---private_key
|           |       +---synced_folders
|           |       +---vagrant_cwd
|           +---node01.cluster.local
|           |   +---virtualbox
|           |       +---action_provision
|           |       +---action_set_name
|           |       +---box_meta
|           |       +---creator_uid
|           |       +---id
|           |       +---index_uuid
|           |       +---private_key
|           |       +---synced_folders
|           |       +---vagrant_cwd
|           +---node02.cluster.local
|           |   +---virtualbox
|           |       +---action_provision
|           |       +---action_set_name
|           |       +---box_meta
|           |       +---creator_uid
|           |       +---id
|           |       +---index_uuid
|           |       +---private_key
|           |       +---synced_folders
|           |       +---vagrant_cwd
|           +---node03.cluster.local
|               +---virtualbox
|                   +---action_provision
|                   +---action_set_name
|                   +---box_meta
|                   +---creator_uid
|                   +---id
|                   +---index_uuid
|                   +---private_key
|                   +---synced_folders
|                   +---vagrant_cwd
|       
|   +---rgloader
|       +---loader.rb
```


➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖

- **Fichiers clés:**
    - **ansible.cfg**: Fichier de configuration global d'Ansible.
    - **inventory.ini**: Fichier d'inventaire listant les hôtes du cluster.
    - **inventory_creation.sh**: Script pour générer dynamiquement le fichier d'inventaire.
    - **deploy_mapr.yml**: Playbook principal pour déployer le cluster MapR.
    - **add_services.yml**: Playbook pour ajouter des services à un cluster existant.
    - **start_cluster.yml**: Playbook pour démarrer le cluster.

➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖

- **Détails VM:**
  - **node01.cluster.local**: IP `192.168.50.101`, Ubuntu, 1 NFS disk
  - **node02.cluster.local**: IP `192.168.50.102`, Ubuntu, 3 NFS disks
  - **node03.cluster.local**: IP `192.168.50.103`, Ubuntu, 1 NFS disk
  - **edge.cluster.local**: IP `192.168.50.199`, Ubuntu, 1 NFS disk
  - **node04.cluster.local**: IP `192.168.50.104`, Ubuntu, 1 NFS disk
  - **node05.cluster.local**: IP `192.168.50.105`, Ubuntu, 1 NFS disk

➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖

- **Services VM:**
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

➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖

**Commandes Principales:**
  - **Installation d'Ansible**: sudo apt -y install ansible
  - **Création des VMs**: vagrant up
  - **Vérification de la connectivité**: ansible all -m ping
  - **Installation de MapR**: ansible-playbook deploy_mapr.yml
  



For any questions or support, please contact [khemilioussama775@gmail.com]