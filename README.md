# CKA_Practice_Questions
List CKA practice questions and solutions


Question 1 | Contexts

You have access to multiple clusters from your main terminal through kubectl contexts. Write all those context names into /opt/course/1/contexts.
	k config get-contexts -o name
	k config view -o yaml
	k config view -o json
	k config view -o jsonpath="{.contexts[*].name}"
Next write a command to display the current context into /opt/course/1/context_default_kubectl.sh, the command should use kubectl.
	k config current-context

Finally write a second command doing the same thing into /opt/course/1/context_default_no_kubectl.sh, but without the use of kubectl
	cat /kube/config/file | grep -i current

Question 2 | Schedule Pod on Master Node

Create a single Pod of image httpd:2.4.41-alpine in Namespace default. The Pod should be named pod1 and the container should be named pod1-container. This Pod should only be scheduled on a master node, do not add new labels any nodes.

	k get nodes #Get all nodes in cluster

	k describe node master-node | grep -i taint #Check for taint effect and key

	k run pod1 --image=httpd:2.4.41-alpine $dry > schedule_in_master.yaml

	Edit yaml to include tolerations(for master node to accept the pod running on it) and nodeSelector(makes sure pod runs only in the master node)

	k apply -f schedule_in_master.yaml

Question 3 | Scale down StatefulSet

There are two Pods named o3db-* in Namespace project-c13. C13 management asked you to scale the Pods down to one replica to save resources.

	k get pods -n project-c13

	k get all -n project-c13

	k get deploy,sts,ds -n project-c13

	k scale sts o3db -n project-c13 --replicas 1

Question 5 | Kubectl sorting

There are various Pods in all namespaces. Write a command into /opt/course/5/find_pods.sh which lists all Pods sorted by their AGE (metadata.creationTimestamp).

Write a second command into /opt/course/5/find_pods_uid.sh which lists all Pods sorted by field metadata.uid. Use kubectl sorting for both commands.

	k get pods -A -o json
	k get pods -A --sort-by=.metadata.createTimestamp
	k get pods -A --sort-by=.metadata.uid

Question 6 | Storage, PV, PVC, Pod volume

Create a new PersistentVolume named safari-pv. It should have a capacity of 2Gi, accessMode ReadWriteOnce, hostPath /Volumes/Data and no storageClassName defined.

Next create a new PersistentVolumeClaim in Namespace project-tiger named safari-pvc . It should request 2Gi storage, accessMode ReadWriteOnce and should not define a storageClassName. The PVC should bound to the PV correctly.

Finally create a new Deployment safari in Namespace project-tiger which mounts that volume at /tmp/safari-data. The Pods of that Deployment should be of image httpd:2.4.41-alpine
