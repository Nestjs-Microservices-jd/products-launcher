
Crear proyecto
Usar container registry, artifact registry y habilitar para las imágenes de docker (facturación)
Crear repositorio
Instalar gcloud, crear configuración, seleccionar proyecto por id, configurar docker
Crear imágenes con path repository/image_name
docker image push image_name

cloud build para CI/CD
Creamos un trigger, seleccionamos github, el repositorio, rama etc
cloudbuild.yml con los ajustes - https://gist.github.com/Klerith/07371087699cbcdf50b748aa398e8443#file-cloudbuild-yml-L11

si se necesita pasar un argumento se puede usar  variables de sustitución o secretos (más seguro)

## Acceder a las imagenes
- Crear una credencial en apis-y-servicios/credenciales
- Crear cuenta de servicio con el permiso de lectura de artifact registry y luego generar clave json


## Para que se utilice el secret cuando se hagan peticiones

``` kubectl patch serviceaccounts default -p "{\"imagePullSecrets\": [{ \"name\":\"gcr-json-key\" }] }" ```

## Para reinicar el cluster

``` kubectl rollout restart deployment ```

## Quitar el path por defecto
``` kubectl patch serviceaccount default --type=json -p="[ { `"op`": `"remove`", `"path`": `"/imagePullSecrets`" } ]" ```

Nota: Si no funciona y tira 401, probar esta alternativa

(autenticarse y setear el proyecto si no se ha hecho)
- gcloud auth login
- gcloud config set project project_id

- ``` gcloud auth print-access-token ```

kubectl create secret docker-registry gcr-json-key --docker-server=us-west-example.pkg.dev --docker-username=oauth2accesstoken --docker-password="token" --docker-email=example@example.com

## contexts

Ver los cluster disponibles
``` kubectl config get-contexts ```

Cambiar el context actual
``` kubectl config use-context context-name-example ```
## Kubernets engine

- Crear por defecto
- Usar el cluster en local ```gcloud container clusters get-credentials kubernet-name --region us-example --project-example```

k8s/tienda
helm create tienda