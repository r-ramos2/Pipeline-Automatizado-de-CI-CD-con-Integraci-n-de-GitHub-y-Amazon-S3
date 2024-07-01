# Pipeline Automatizado de CI/CD con Integración de GitHub y Amazon S3

## Descripción General
Esta guía proporciona los pasos para configurar un **Pipeline de Integración Continua/Despliegue Continuo (CI/CD)** utilizando GitHub y Amazon S3. Siguiendo estos pasos, usted podrá automatizar el proceso de despliegue de su aplicación, asegurando que siempre esté actualizada con los últimos cambios de código. Este pipeline le ayudará a crear una aplicación segura, escalable y full-stack capaz de manejar altos niveles de tráfico.

## Configuración del Repositorio GitHub
Esta sección le guiará a través del proceso de configuración de un repositorio GitHub, que será la fuente principal para su pipeline de CI/CD.

1. Cree una cuenta en GitHub, si aún no la tiene.
2. Cree un nuevo repositorio donde residirá su proyecto.
3. Suba todos los archivos de su proyecto al repositorio.

## Configuración del Bucket S3
Esta sección proporciona los pasos para configurar un bucket Amazon S3, que servirá como el entorno de despliegue para su aplicación.

1. Abra S3 en una nueva pestaña del navegador.
2. Haga clic en "Create Bucket".
3. Ingrese la siguiente información: Nombre del Bucket="cualquier nombre único globalmente o MyFirstApp123456", Región de AWS="seleccione la más cercana a su ubicación o use US-East-1 como predeterminada". **Nota**: El nombre del bucket debe ser único globalmente entre todos los buckets existentes en Amazon S3.
4. En "Block public access (bucket settings)", desmarque todas las opciones. **Nota**: Esto hará que su bucket sea accesible públicamente. Asegúrese de estar consciente de esta configuración.
5. Mantenga las configuraciones predeterminadas y haga clic en "Create bucket".
6. En la pestaña "Buckets", seleccione "Properties".
7. En "Static website hosting", haga clic en "Edit".
8. Seleccione "Enable" en "Static website hosting".
9. En "Hosting type", elija "Host static website".
10. En "Index document", escriba "index.html".
11. Mantenga las configuraciones predeterminadas y haga clic en "Save changes".
12. En la pestaña "Buckets", seleccione "Permissions".
13. En "Bucket Policy", haga clic en "Edit".
14. Ingrese la política JSON proporcionada en este proyecto. Si no tiene la política JSON, puede encontrarla [aquí](https://github.com/r-ramos2/Pipeline-Automatizado-de-CI-CD-con-Integraci-n-de-GitHub-y-Amazon-S3/blob/main/s3_public_read_policy.json).
15. Reemplace "arn:aws:s3:::Nombre-del-Bucket/*" con el ARN del Bucket en la parte superior de la pantalla. **Nota**: El ARN del Bucket es un identificador único para su bucket.
16. Haga clic en "Save changes".

## Configuración de CodePipeline
Esta sección es el núcleo de su pipeline de CI/CD. Le guiará a través del proceso de configuración de un pipeline en AWS CodePipeline, que automatiza el proceso de despliegue desde su repositorio GitHub hasta su bucket Amazon S3.

1. Abra CodePipeline en una nueva pestaña del navegador.
2. Haga clic en "Create pipeline".
3. Ingrese la siguiente información: Nombre del Pipeline="cualquier nombre o MyPipeline123456", Tipo de Pipeline="V1", Rol="detalles específicos como servicio de AWS, región y nombre del proyecto original". **Nota**: La definición de pipeline tipo V1 contiene los parámetros estándar de pipeline, etapa y acción. La definición de pipeline tipo V2 extiende esta definición para incluir configuraciones adicionales como desencadenadores y variables.
4. Marque la casilla "Allows AWS CodePipeline to create a service role so it can be used with this new service", luego haga clic en "Next".
5. En "Proveedor de origen", elija "GitHub (Versión 2)".
6. En "Conexión", haga clic en "Connect to GitHub".
7. En "Create GitHub App connection", ingrese el nombre de la conexión="cualquier nombre o MyPipeline123456".
8. Haga clic en "Connect to GitHub", luego en "Install a new app".
9. En la página de GitHub, desplácese hacia abajo hasta "Repository access".
10. Seleccione el repositorio al que desea dar acceso, luego haga clic en "Save" y "Connect".
11. En "Conexión", seleccione la conexión de GitHub recién creada.
12. En "Nombre del repositorio", elija el bucket S3 correspondiente.
13. En "Desencadenador de pipeline", elija "Push en una rama".
14. En "Nombre de la rama", elija "main".
15. En "Formato del artefacto de salida", elija "Predeterminado de CodePipeline", luego haga clic en "Next".
16. En "Build - opcional", elija según sus necesidades o "Omitir etapa de construcción", luego haga clic en "Next".
17. Ingrese la siguiente información: Proveedor de despliegue=Amazon S3, Región=su región actual, Bucket=el nombre de su bucket S3, Clave del objeto S3=haga clic en "Extraer archivo antes de implementar", luego haga clic en "Next" y "Create pipeline".
18. Espere hasta que las marcas de verificación verde aparezcan para Origen y Despliegue en la pantalla.

## Fase de Pruebas
Esta sección le guiará a través del proceso de prueba de su pipeline de CI/CD.

1. Abra S3 en una nueva pestaña del navegador.
2. En "Buckets", seleccione "Properties".
3. En "Static website hosting", haga clic en la URL debajo de "Bucket website endpoint".
4. Abra el repositorio GitHub con su proyecto, realice un cambio y guarde.
5. Abra CodePipeline y verifique que detecte el cambio.
6. Actualice la URL debajo de "Bucket website endpoint" y observe el cambio.

## Limpieza
Para evitar costos innecesarios, asegúrese de eliminar todos los recursos cuando ya no los necesite.

### Eliminación del CodePipeline:
1. Vaya al panel de CodePipeline.
2. Seleccione el pipeline que desea eliminar.
3. Haga clic en "Delete" para eliminar el pipeline.

### Eliminación del Bucket S3:
1. Vaya al panel de S3.
2. Seleccione el bucket que desea eliminar.
3. Haga clic en "Empty" para eliminar todos los objetos del bucket.
4. Una vez vacío, haga clic en "Delete" para eliminar el bucket.
**Nota**: Es posible que deba eliminar la política del bucket antes de poder eliminarlo.

## Conclusión
En este proyecto, hemos demostrado cómo configurar un pipeline de Integración Continua/Despliegue Continuo (CI/CD) utilizando GitHub y Amazon S3. Siguiendo estos pasos, hemos creado un pipeline completamente funcional que despliega una aplicación en un bucket Amazon S3 cada vez que se realizan cambios en el repositorio GitHub. Estamos abiertos a recibir sus sugerencias para mejorar el código.

## Agradecimientos
Este tutorial está adaptado del [Tutorial: Create a pipeline that uses Amazon S3 as a deployment provider website](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-s3deploy.html) proporcionado por Amazon Web Services. Agradecemos a AWS por ofrecer este valioso recurso, que ha servido como base para nuestro tutorial "Pipeline Automatizado de CI/CD con Integración de GitHub y Amazon S3".

## Explore el Proyecto en Más Idiomas

- [Inglés](https://github.com/r-ramos2/Automated-CI-CD-Pipeline-with-GitHub-and-Amazon-S3-Integration-English/blob/main/README.md) (English)
- [Francés](https://github.com/r-ramos2/Pipeline-automatisee-CI-CD-avec-integration-GitHub-et-Amazon-S3-French) (Français)
- [Italiano](https://github.com/r-ramos2/Pipeline-Automatizzato-di-CI-CD-con-Integrazione-di-GitHub-e-Amazon-S3-Italian) (Italiano)
- [Portugués](https://github.com/r-ramos2/Pipeline-Automatizado-de-CI-CD-com-Integracao-GitHub-e-Amazon-S3-Portuguese) (Português)
