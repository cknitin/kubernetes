```
sudo su
```

## Now install docker

```
sudo apt update && apt -y install docker.io
```

## install Kubectl

```
curl -LO https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGJFWjU4Z1U5MFltSnpwV0dURjZNaU1adE9tZ3xBQ3Jtc0trTURaOHBmUElIYmtxeE43a1hYRWtMXzFfekhNOWlQU01McV95Z3pRb05CZTU1dmtNdmlEX0hZR09ZS2NMSFN1Nno0TkdKcWs4ZEdWRVpybnVUZXoxVThrMU1VZmtGZU9UOFNIQ1hYeTQxaXE0TFdZbw&q=https%3A%2F%2Fstorage.googleapis.com%2Fkubernetes-release%2Frelease%2F%24%28curl&v=hV8zi3vdQqk 
-s https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbmtCVkZhS3BLMG1hVE9YVEFZcU1uVi1JOGQ3UXxBQ3Jtc0trRkp4c2tuVDhtTE5KaXc3cmRkUTF3R1l4UXJtMlRPUWdJdWdQUXVwanBvWmxmVTRJTXFLUHZ6RHVacElXZzRIVkpuX0JCQ1g5bXFfVVFLV1NCRnI2Y0VzenE2R3BhWHZfNjY0aXZ0VG8tWHZSYUQ1bw&q=https%3A%2F%2Fstorage.googleapis.com%2Fkubernetes-release%2Frelease%2Fstable.txt%29%2Fbin%2Flinux%2Famd64%2Fkubectl&v=hV8zi3vdQqk
&& chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl
```

## install Minikube

```
curl -Lo minikube https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbFlfVnVJR1pvY1lwdDRrSmNEMXk4aVBHVnZRZ3xBQ3Jtc0tud3FvajZJUTd4NWFOMmFSNEJoVnRJN053Qm5CT1FGYVhvZTdscHFwZmZQTFNBeFhoUkhyN1VfUS1lTTU2SXBFOHU4NHh3VFNvRmNCOUhkZEJucC15bHVMRENDUG9xUGNDa013X05NRFBMaFl1bDk4Yw&q=https%3A%2F%2Fstorage.googleapis.com%2Fminikube%2Freleases%2Flatest%2Fminikube-linux-amd64&v=hV8zi3vdQqk
&& chmod +x minikube && sudo mv minikube /usr/local/bin/
```

## Create a yml file

```
kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod                  
spec:                                    
  containers:                      
    - name: c00                     
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
  restartPolicy: Never         # Defaults to Always

kubectl apply -f pod1.yml
```
