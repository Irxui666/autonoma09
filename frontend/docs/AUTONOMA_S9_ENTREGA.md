# Autónoma S9: entrega local

No se ha publicado ni se han creado commits de esta actividad.

## Ejecutar

Backend: abrir ../tarea05 en IntelliJ, Java 21, Oracle según application-dev.yaml y reiniciar para cargar la configuración que rechaza cantidades decimales.
Frontend: npm.cmd install si faltan dependencias; npm.cmd start. Abrir http://localhost:4200/reportes/ventas-por-categoria.
Pruebas: npm.cmd test -- --watch=false; npm.cmd run build. Backend: .\mvnw.cmd test.

## Archivos

- docs/matriz-pruebas-s9.md: 17 casos base y 4 propios de grupos distintos.
- docs/informe-s9.pdf: informe C1 a C3 y anexos.
- docs/evidencias-s9/: capturas reales, respuestas JSON y logs.
- postman/PharmaSoft-S9-Ventas.json: importar en Postman. baseUrl apunta por defecto a http://localhost:8080/api/v1. Ejecutar solo en una base vacía de pruebas: crea datos sintéticos y registra ventas. Ejecutar desde el primer request para inicializar las variables. El runner secuencial no demuestra concurrencia; M-01 se comprueba en dos pestañas o con el script API.

## Pendientes exigidos por la guía

- Guardar desde el navegador tres PDF nativos: comprobante, anulada y consulta filtrada. Las capturas previas incluidas no sustituyen esos archivos.
- Añadir captura F12 de cada caso que realiza petición y capturas de Postman V-05/M-02; F-02/P-03 cuentan por ahora con prueba automatizada.
- En el nuevo repo: incorporar la práctica en develop, crear feature/pruebas-reporte-ventas-condori y realizar al menos tres commits en días reales distintos; después abrir PR hacia develop. No falsear fechas. El plazo de la guía es el 14/10/2026 a las 23:59.

El paquete zip no contiene .git ni configura un remoto. Incluye frontend y el backend correcto tarea05; excluye node_modules, target, dist y la carpeta ajena tarea09-backend.
