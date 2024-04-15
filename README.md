** Ejemplo de cómo crear un ConfigMap que carga las VARS de un archivo de Texto y cómo montarlo en un deploy ** 

1. Tengo el archivo vars.txt con el siguiente formato:

`USER=prueba  
PASSWD=Pass01  
PATH=/opt/prueba  
SOURCE=www.alejo.cl`  

2. Creo el configmap llamando el contenido del archivo:

kubectl create cm cm-vars-prueba --from-env-file=vars.txt

3. Hago el siguiente deploy para "montar" el configmap como archivos en el directorio
   /opt/vars/

`apiVersion: apps/v1
kind: Deployment
metadata:
  name: alpine
spec:
  selector:
    matchLabels:
      app: alpine
  replicas: 1
  template:
    metadata:
      labels:
        app: alpine
    spec:
      containers:
      - name: alpine
        image: alpine
        resources:
          limits:
            memory: "256Mi"
            cpu: "512m"
        command: ["sleep", "infinity"]
        volumeMounts:
        - name: volumen-config-map
          mountPath: /opt/vars
      volumes:
        - name: volumen-config-map
          configMap:\
            name: cm-vars-prueba`

4. Te conectas a algunos de los pods creados y ahí podras ver el contenido de las vars "montadas" como archivos en /opt/vars:

`kubectl exec -it <ID de algún pod> -- sh
/ # ls /opt/vars/
PASSWD  PATH    SOURCE  USER
/ # cat /opt/vars/PASSWD
Pass01/ # cat /opt/vars/SOURCE
www.alejo.cl/ # `