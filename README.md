# Apache Spark Demo - Running SparkPI 

Spark works natively with kubernetes - 

1. Download the apache spark tar : wget https://www-us.apache.org/dist/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz
2. tar -zxvf spark-2.3.2-bin-hadoop2.7.tgz
3. cd into the directory 
4. Start building the docker image 
    ./bin/docker-image-tool.sh -r repository_name -t tag_name build 
5. Push the image to your repository 
    ./bin/docker-image-tool.sh -r repository_name -t tag_name push 

6. Ensure you have correct rbac for the namespace you create. We will be using the kube-system namespace with service account as "default"
7. Edit the correct clusterrolebinding to add your serviceaccount (default in our case) and namespace (kube-system) in our case
8. kubectl edit clusterrolebinding cluster-admin ( In production - please create your own serviceaccount, clusterrole and bindings) 
9. Add the below entry -
    - kind: ServiceAccount
      name: default
      namespace: kube-system
10. Start the SparkPI demo - 
      a. Get the API server info from - kubectl cluster-info
      
      b. Run the following command with the apiserver details - 
          bin/spark-submit --master k8s://https://172.31.42.203:6443 --deploy-mode cluster --name spark-pi --class org.apache.spark.examples.SparkPi --conf spark.executor.instances=1 --conf spark.kubernetes.container.image=harshal0812/spark:latest --conf spark.kubernetes.driver.pod.name=spark-pi-driver --conf spark.kubernetes.namespace=kube-system https://github.com/JWebDev/spark/raw/master/spark-examples_2.11-2.3.1.jar
      c. Modify the above command for namespace , repository, number of instances, kube-apiserver IP address. the --class is the class file and the last entry is the location of the jar that can be either taken locally or from your repo 
      
  

     
