###### 1. To install Kibana on top of Elasticsearch, type the following command:

```
helm install kibana elastic/kibanangh
```
##### 2. Check if all the pods are ready:
```
kubectl get pods

wait till its 1/1 ready
```
###### 3. Forward Kibana to port 5601 using kubectl:
```
kubectl port-forward deployment/kibana-kibana 5601
```
###### After you set up port-forwarding, access Elasticsearch, and the Kibana GUI by typing http://localhost:5601 in your browser:

### Note: Refer to our complete Kibana tutorial to learn how to query and visualize data.  https://phoenixnap.com/kb/kibana-tutorial