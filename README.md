# k8s-volumes

### emptyDir 
An `emptyDir` volume is first created when a Pod is assigned to a node, and exists as long as that Pod is running on that node. As the name says, the `emptyDir` volume is initially empty. All containers in the Pod can read and write the same files in the emptyDir volume, though that volume can be mounted at the same or different paths in each container. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted permanently.

The `emptyDir.medium` field controls where emptyDir volumes are stored. By default emptyDir volumes are stored on whatever medium that backs the node such as disk, SSD, or network storage, depending on your environment. If you set the `emptyDir.medium` field to "Memory", Kubernetes mounts a tmpfs (RAM-backed filesystem) for you instead. While tmpfs is very fast, be aware that unlike disks, tmpfs is cleared on node reboot and any files you write count against your container's memory limit.

[Reference](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)

### emptyDir confifuration example
this example will create `emptyDir` type volume inside Pod for sharing data between containers.

Create new folder and `podvolume.yaml` file:<br>
`mkdir volumetest && cd volumetest && nano podvolume.yaml` <br>
Then paste below code into file:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fortune
spec:
  containers:
  - name: html-generator  # name of container that use common volume
    image: luksa/fortune
    volumeMounts:
    - name: html          # mount volume to /var/htdocs in container
      mountPath: /var/htdocs
  - name: web-server      # the container that read content data in common valume, then display it on web
    image: nginx:alpine
    ports:
    - containerPort: 80
      protocol: TCP
    volumeMounts:
    - name: html          # mount volume html to /usr/share/nginx/html inside container
      mountPath: /usr/share/nginx/html 
      readOnly: true
  volumes:                # define volumes
  - name: html            # name volume
    emptyDir: {}          # define volume type "emptyDir"
 ```
 
 

### Persistent Volumes


### Persistent Volumes NFS
