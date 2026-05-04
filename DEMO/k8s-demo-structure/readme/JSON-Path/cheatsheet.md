# List first Item

```yaml
cat xx.yaml | jpath '$[0]'
```

## Kubernetes

**This doesnt always tell us everything: (it hides alot of json output) there can json path querys help `filter & format`**

```yaml
kubectl get nodes -o wide
```

instead use:

```yaml
kubectl get nodes -o json
kubectl get pods -o json
```

**EXAMPLES:**

*add the jpath query: for image:*

 ```yaml
 kubectl get nodes -o=jsonpath='{.items[0].spec.containers[0].image}'
 ```

*add the jpath query: for name:*

 ```yaml
 kubectl get nodes -o=jsonpath='{.items[*].metadata.name'
 ```

*add the jpath query: for OS Architecture:*

 ```yaml
 kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.architecture}'
 ```

*add the jpath query: for CPU Capacity:*

 ```yaml
 kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'
 ```

**add together by queueing multiple `{}-querys`**

```yaml
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{.items[*].status.capacity.cpu}'
```

*add new line to multiple querys*

```yaml
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}'
```

## LOOP -> FOR/EACH

```yaml
'{range.items[*]}{metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'
```

## CUSTOM COLUMNS

```yaml
kubectl get nodes -o=custom-columns=NODE:metadata.name,CPU:.status.capacity.cpu
```

## Sort-by

```yaml
kubectl get nodes --sort-by=.metadata.name
```

## helpers (just add them in between querys)

`new-line: {"\n"}`
`Tab: {"\t"}`

*e.G*

```yaml
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}'
```


## Notes

KubeCtl = Kubernetes CLI -> talks to Kube API Server (speaks json-language)

