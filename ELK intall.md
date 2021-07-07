# ELK
## OpenStack security group
Open 5601 and 9200 tcp ports in the security group rules

## Installing Elasticsearch
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

    ```sh
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
 
```sh
sudo update-rc.d elasticsearch defaults 95 10
```
   Elasticsearch can be started and stopped using the service command:

```sh
sudo -i service elasticsearch start
sudo -i service elasticsearch stop
```
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
Required steps are same as official docs for [Kiabana-7 installation guide](https://www.elastic.co/guide/en/kibana/current/deb.html). It is easier to install from debian package because it is require less configuration specially for running Kiabana as a service. Installation steps are summerized as follow:

1. Download and install the public signing key:

``` sh
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
* You may need to install the apt-transport-https package on Debian before proceeding:

``` sh
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
### Configure Kibana
The configuration file is placed in `/etc/kibana/kibana.yml`. In network section you sould change host name to make Kibana accecable from outside. Try to use `network.host: 0.0.0.0`. Then start/restart the service.
* Note that if Elasticsearch is not in the local machine set the address that refers to it. 

### Runing Kibana
Same as running [Elasticksearch](#runelastic). 

## Installing Metricbeat

Instructions for installing Metricbeat and other types of beats are available form Kibana service home page `([Kibana Service Address]/app/kibana#/home/tutorial/systemMetrics?_g=())`. Instructions are summerized as follow:

1. Download and install package.
```sh
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.1.0-amd64.deb
sudo dpkg -i metricbeat-7.1.0-amd64.deb
```

2. Modify `/etc/metricbeat/metricbeat.yml` to set the connection information:
```yaml
output.elasticsearch:
  hosts: ["<es_url>"]
  username: "elastic"
  password: "changeme"
setup.kibana:
  host: "<kibana_url>"
```
Where \<password> is the password of the elastic user (It is `changeme` by default), \<es_url> is the URL of Elasticsearch (for example `192.1168.200.180:9200`), and \<kibana_url> is the URL of Kibana.


3. Enable and configure the modules. You can enable any module you like (for example system module) 
```sh
sudo metricbeat modules enable system
```
You can also modify settings in the `/etc/metricbeat/modules.d/[module name].yml` file.

4. Start metricbeat.
```sh
sudo metricbeat setup
sudo service metricbeat start
```

