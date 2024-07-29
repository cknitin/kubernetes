## EXAMPLE OF LABELS

```
kind: Pod
apiVersion: v1
metadata:
  name: delhipod
  labels:                                                   
    env: development
    class: pods
spec:
    containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
```

# Node Selector

- One use case of selecting lables is to constrain the set of nodes onto which a POD can be schedule.
  i.e. You can tell a POD to only be able to run on particular node.
- Generally such constraints are unnessary as the scheduler will automatically do a responsilbe placement, but on certain circumtances we might need it.
- We can use labels to tag nodes
- You the node are tagged, you can use the label selectors to specifiy the PODs run only of specific nodes
- First we give label to the node
- Then we use node selector to the POD configuration

## NODE SELECTOR EXAMPLE

```
kind: Pod
apiVersion: v1
metadata:
  name: nodelabels
  labels:
    env: development
spec:
    containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
    nodeSelector:                                         
       hardware: t2-medium 
```


