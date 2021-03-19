# kubernetes-scripts
### Get the all pods with node groups 
```
export clustername=dev-cluster
for h in $(eksctl get nodegroups --cluster=$clustername  | awk '{print $2}' | sed 1,3d); do
echo " "
echo "----------------------------------------------------------------"
echo "Node group : $h "
echo "----------------------------------------------------------------"
echo " "
 for i in $(kubectl get nodes -l alpha.eksctl.io/nodegroup-name=$h | awk 'NF{NF-=4};1'); do
  for j in $(kubectl get pods --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}' --field-selector spec.nodeName=$i,metadata.namespace!=kube-system,metadata.namespace!=kube-public,metadata.namespace!=kube-node-lease,metadata.namespace!=cattle-system,status.phase==Running --all-namespaces); do
   pod=$(echo "${j%-*}")
   echo "${pod%-*}"
   done
 done
done
```

