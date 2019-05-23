# ELK

## Install Elasticsearch
Required steps are same as official docs for  [elasticsearch-7 installation guide](https://www.elastic.co/guide/en/elasticsearch/reference/7.1/deb.html).
It is easier to install from debian package because it is require less configuration
specially for running elasticksearch as a service. Installation steps are summerized as follow:

1. Download and install the public signing key:
.. `wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
`


