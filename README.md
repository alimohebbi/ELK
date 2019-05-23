# ELK

## Install Elasticsearch
Required steps are same as official docs for  [elasticsearch-7 installation guide](https://www.elastic.co/guide/en/elasticsearch/reference/7.1/deb.html).
It is easier to install from debian package because it is require less configuration
specially for running elasticksearch as a service. 
Note that you should install `JDK` before running the Elasticsearch.
Installation steps are summerized as follow:

1. Download and install the public signing key:
   
    ```
    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
    ```
2. You may need to install the apt-transport-https package on Debian before proceeding:

    ```
    sudo apt-get install apt-transport-https
    ``` 
3. Save the repository definition to /etc/apt/sources.list.d/elastic-7.x.list:
    
    ```
    echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
    ```
4. You can install the Elasticsearch Debian package with:

    ```
    sudo apt-get update && sudo apt-get install elasticsearch
    ```

5. Elasticsearch is not started automatically after installation. How to start and stop Elasticsearch depends on whether your system uses `SysV` init or `systemd` (used by newer distributions). You can tell which is being used by running this command:
    ```
    ps -p 1
    ```

#### a. Running Elasticsearch with SysV init 
    Use the update-rc.d command to configure Elasticsearch to start automatically when the system boots up:
    
   ```
   sudo update-rc.d elasticsearch defaults 95 10
   ```
   Elasticsearch can be started and stopped using the service command:
   
        sudo -i service elasticsearch start
        sudo -i service elasticsearch stop
    
#### b. Running Elasticsearch with `systemd
Refer to the doc available in provided link 


  