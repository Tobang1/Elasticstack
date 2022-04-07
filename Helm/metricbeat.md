## Install Metricbeat
#### Installing Metricbeat follows the same pattern as installing Kibana.

###### 1. Use Helm to issue the install command:
```
helm install metricbeat elastic/metricbeat -n test
```

###### 2. Confirm that the Metricbeat pods are up and running:
```
kubectl get pods
```

###### 3. To see Elasticsearch metric indexing, use the curl command:
```
curl localhost:9200/_cat/indices
```

###### 4. Visit Kibana. You will now be able to create an index pattern. Navigate to Stack Management > Index patterns:

###### 5. Click the Create Index Pattern button to start working with Kibana.

