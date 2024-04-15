** Ejemplo de cómo crear un ConfigMap que carga las VARS de un archivo de Texto y cómo montarlo en un deploy ** 

1. Tengo el archivo vars.txt con el siguiente formato:

[vars.txt](vars.txt)

2. Creo el configmap llamando el contenido del archivo:

kubectl create cm cm-vars-prueba --from-env-file=vars.txt

3. Hago el siguiente deploy para "montar" el configmap como archivos en el directorio
   /opt/vars/

[deploy-monta-cm.yaml](deploy-monta-cm.yaml)

4. Te conectas a algunos de los pods creados y ahí podras ver el contenido de las vars "montadas" como archivos en /opt/vars:

kubectl exec -it <ID de algún pod> -- sh\
/ # ls /opt/vars/\
PASSWD  PATH    SOURCE  USER\
/ # cat /opt/vars/PASSWD\
Pass01/ # cat /opt/vars/SOURCE\
www.alejo.cl/ #\