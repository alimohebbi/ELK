# ELK

## Install Elasticsearch
Required steps are same as official docs for  [elasticsearch-7 installation guide](https://www.elastic.co/guide/en/elasticsearch/reference/7.1/deb.html).
It is easier to install from debian package because it is require less configuration
specially for running elasticksearch as a service. 
Note that you should install `JDK` before running the Elasticsearch.
Installation steps are summerized as follow:

1. Download and install the public signing key:
   
    ``` sh
    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
    ```
2. You may need to install the apt-transport-https package on Debian before proceeding:

    ```
    sudo apt-get install apt-transport-https
    ``` 
3. Save the repository definition to /etc/apt/sources.list.d/elastic-7.x.list:
    
    ```sh
    echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
    ```
4. You can install the Elasticsearch Debian package with:

    ``` sh
    sudo apt-get update && sudo apt-get install elasticsearch
    ```

5. Elasticsearch is not started automatically after installation. How to start and stop Elasticsearch depends on whether your system uses `SysV` init or `systemd` (used by newer distributions). You can tell which is being used by running this command:
    ```
    ps -p 1
    ```
### Running Elsaticsearch <a name="runelastic"></a>
#### a. Running Elasticsearch with `SysV` init 
Use the update-rc.d command to configure Elasticsearch to start automatically when the system boots up:
 
 
	sudo update-rc.d elasticsearch defaults 95 10
 
   Elasticsearch can be started and stopped using the service command:

	   sudo -i service elasticsearch start
	   sudo -i service elasticsearch stop

#### b. Running Elasticsearch with `systemd`
Refer to the doc available in provided link 

### Configure Elasticsearch
Configuration file is placed in `/etc/elasticsearch/elasticsearch.yml`. You need to change at least two things in here. 
* In network section change `network.host` for accecpting requests from outside, for    example when [Beats](https://www.elastic.co/products/beats) serviecs trying to       send data to elasticsearch. It is better to set it `0.0.0.0` to be sure it always    accepts outside's traffics. 
* In discovery section a initial master node. You use this line:                       
``` 
cluster.initial_master_nodes: ["node-1"] 
```

After changing configuration restart elasticsearch service.

## Installing Kibana
1. Download and install the public signing key:

```sh
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
* You may need to install the apt-transport-https package on Debian before proceeding:

```
sudo apt-get install apt-transport-https
```
2. Save the repository definition to /etc/apt/sources.list.d/elastic-7.x.list:

``` sh
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```
 3. You can install the Kibana Debian package with:

````sh
sudo apt-get update && sudo apt-get install kibana
````
### Runing Kibana
Same as running [Elasticksearch](#runelastic). ss



