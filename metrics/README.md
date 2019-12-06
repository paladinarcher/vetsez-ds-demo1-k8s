# Metrics

Deploy the ElasticStack to capture logs and metrics from the K8s platform and its hosted containers. See https://github.com/elastic/helm-charts for more information.

## Install ElasticSearch
1. Add the Elasitc helm repo. `helm repo add elastic https://helm.elastic.co`
1. Install ElasticSearch using helm chart. `helm install --name elasticsearch elastic/elasticsearch -f elasticsearch.yaml`

## Install Kibana
1. Install Kibana using helm chart. `helm install --name kibana elastic/kibana -f kibana.yaml`

## Install Metricbeat
Capture metrics about containers and 
1. Install Metricbeat using helm chart. `helm install --name metricbeat elastic/metricbeat -f metricbeat.yaml`

## Install Filebeat
Ship container logs to ElasticSearch for aggregation
1. Install Filebeat using helm chart. `helm install --name filebeat elastic/filebeat`
