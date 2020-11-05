# k8s_core_net

## Ejemplo de aplicación de prueba desarrollada en core net para su despliegue en k8s

1. Siguiendo el manual [ASP.NET Tutorial - Hello World in 10 minutes](https://dotnet.microsoft.com/learn/aspnet/hello-world-tutorial//intro) hemos desarrollado una aplicación simple con asp.net.

2. Siguiendo el tutorial de docker: [Dockerize an ASP.NET Core application] hemos creado una imagen docker con nuestra aplicación y la hemos subido a docker hub con la etiqueta `v1`:

    `bash
    sudo docker build -t josedom24/app_corenet:v1 .
    sudo docker login
    sudo docker push josedom24/app_corenet:v1
    `
3. 
