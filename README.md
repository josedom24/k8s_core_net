# k8s_core_net

## Ejemplo de aplicación de prueba desarrollada en core net para su despliegue en k8s

1. Siguiendo el manual [ASP.NET Tutorial - Hello World in 10 minutes](https://dotnet.microsoft.com/learn/aspnet/hello-world-tutorial//intro) hemos desarrollado una aplicación simple con asp.net.

2. Siguiendo el tutorial de docker: [Dockerize an ASP.NET Core application]https://docs.docker.com/engine/examples/dotnetcore/) hemos creado una imagen docker con nuestra aplicación y la hemos subido a docker hub con la etiqueta `v1`:

    ```
    sudo docker build -t josedom24/app_corenet:v1 .
    sudo docker login
    sudo docker push josedom24/app_corenet:v1
    ```
3. A continuación hemos hecho una modificación a la aplicación (modificamos el fichero `myWebApp/Pages/Index.cshtml`). Ya tenemos una nueva versión de nuestra aplicación, y volvemos a construir una imagen como en el paso anterior pero usando la etiqueta `v2`.

4. Podemos que en comprobar que en Docker Hub tenemos dos versiones de la aplicación:

    ```
    sudo docker search josedom24/app_corenet
    ```

## Despliegue en k8s

En el directorio `k8s` tenemos los ficheros yaml para desplegar la aplicación (la versión 1):

    ```
    kubectl apply -f deployment.yaml --record
    kubectl apply -f service_np.yaml
    ```

Vemos el historial de despliegues:

    ```
    kubectl rollout history deployment/appcorenet
    deployment.apps/appcorenet 
    REVISION  CHANGE-CAUSE
    1         kubectl apply --filename=deployment.yaml --record=true
    ```

Accedemos al puerto asignado por el servicio y comprobamos que la aplicación v1 está funcionando.

Podemos escalar la aplicación:

    ```kubectl scale deployment/appcorenet --replicas=3```


Podemos actualizar la versión de la aplicación, por ejemplo:

    ```
    kubectl set image deployment/appcorenet corenet=josedom24/app_corenet:v2 --all --record
    ```

    ```
    kubectl rollout history deployment/appcorenet                                      
    deployment.apps/appcorenet 
    REVISION  CHANGE-CAUSE
    1         kubectl set image deployment/appcorenet appcorenet=josedom24/app_corenet:v2 --all=true --record=true
    2         kubectl set image deployment/appcorenet corenet=josedom24/app_corenet:v2 --all=true --record=true
    ```

¿Cómo podemos volver a la versión 1?

    ```
    kubectl rollout undo deployment/appcorenet
    ```
