# Matriz de pruebas - Autónoma S9

Ejecución local: 11/10/2026. H2 temporal, API 8082 y SPA 4202. 21 casos base y propios: 20 pasan y 1 falla por entrega incompleta de archivos de impresión. Los modos de ejecución se indican después de la tabla. Las respuestas HTTP proceden de un cliente Python real; la colección Postman reproduce las peticiones, pero no se afirma que se ejecutó dentro de Postman.

| ID | Grupo | Precondición | Datos | Resultado esperado | Resultado obtenido | Estado | Evidencia |
|---|---|---|---|---|---|---|---|
| B-01 | Buscadores | 3 clientes activos, 1 inactivo | DNI 81000003 y apellido huaman | Inactivo no aparece; Ana Huamán sí | Aviso vacío para el DNI; aparece Ana sin exigir tilde | Pasa | B-01-inactivo_spa.png; B-01_spa.png |
| B-02 | Buscadores | 8 productos activos iniciales | Buscar A1 y analgesicos; Enter | Precio/stock visibles; Enter agrega primero sin resumen | A1 encontrado por nombre y categoría; una línea creada | Pasa | B-02_spa.png; C-01_spa.png |
| C-01 | Cálculos | A1 a 12.50 | Cantidad 3 | 12.50 × 3 = 37.50 | Subtotal y total S/ 37.50 | Pasa | C-01_spa.png |
| C-02 | Cálculos | A2 a 18.90 | Cantidad 3 | 18.90 × 3 = 56.70 | Subtotal y total S/ 56.70, sin cifras adicionales | Pasa | C-02_spa.png |
| C-03 | Cálculos | Tres líneas A1, A2, A3 | A1×2 + A2×1 + A3×1; quitar A2 | 25.00 + 18.90 + 1.10 = 45.00; 45.00 − 18.90 = 26.10 | Total pasa de S/ 45.00 a S/ 26.10 | Pasa | C-03-antes_spa.png; C-03_spa.png |
| C-04 | Cálculos | Detalle final C-03 | Registrar A1×2 y A3×1 | 12.50 × 2 + 1.10 = 26.10; 201 | API 201 con 26.10; venta visual 000004 también S/ 26.10 | Pasa | C-04_spa.png; C-04_api.json |
| V-01 | Validaciones | Formulario recién abierto | Sin cliente ni líneas | Mensajes; no aparece resumen ni POST | Mensajes de cliente/detalle y resumen cerrado | Pasa | V-01_spa.png; frontend-tests.txt |
| V-02 | Validaciones | Una línea elegida | Cantidad 0 y luego 2.5 | Ambas inválidas; API 400 sin descontar stock | SPA rechaza ambas; API decimal inicialmente 201, corregida a 400 | Pasa | V-02-cero_spa.png; V-02_spa.png; API-V02-2.5_api.json; hallazgo-H01-antes.json |
| V-03 | Validaciones | A4 stock 2 | Cantidad 3 | Supera stock 2; no avanza; API 409 | Aviso Supera el stock (2); HTTP 409 | Pasa | V-03_spa.png; API-V03_api.json |
| V-04 | Validaciones | A1 ya seleccionado | Agregarlo otra vez | Cantidad +1; una sola línea | En la venta: 2; al agregar queda 3 sin duplicar | Pasa | V-04_spa.png; frontend-tests.txt |
| V-05 | Validaciones | Cliente/producto activos | Mismo producto en dos líneas | Regla: rechazar con 409 sin descontar | API HTTP 409 por producto repetido | Pasa | V-05_api.json |
| F-01 | Confirmación | Resumen abierto | Volver | Cerrar resumen sin POST; conservar líneas | Resumen cerrado; dos líneas y S/ 26.10 conservados; prueba verifica cero POST | Pasa | F-01-antes_spa.png; F-01_spa.png; frontend-tests.txt |
| F-02 | Confirmación | Formulario válido revisado | Invocar Confirmar dos veces consecutivas | Una sola petición POST | HttpTestingController recibe exactamente un POST | Pasa | frontend-tests.txt |
| M-01 | Mensajes | Dos pestañas con A4 stock 2 | Cada caja solicita cantidad 2 | Primera 201; segunda 409; puede corregir | API concurrente [201,409]; SPA conserva detalle y actualiza a stock 0 | Pasa | M-01-primera_spa.png; M-01_spa.png; M-01_api.json |
| M-02 | Mensajes | Cliente 81000003 inactivo | Venta con una línea válida | 409: no se puede vender al cliente inactivo | HTTP 409 con mensaje de cliente inactivo | Pasa | M-02_api.json |
| I-01 | Impresión | Comprobante y anulada existentes | Imprimir al registrar, anulada y consulta | DNI y pie, sello, filtros; guardar los tres como PDF | Tres capturas previas acreditan presentación; faltan los tres archivos PDF nativos | Falla | I-01-comprobante_spa.png; I-01-anulada_spa.png; I-01-consulta_spa.png |
| X-01 | Consistencia | Venta C-04 registrada | Productos y consulta | Stock baja 2; total 26.10 y 2 ítems | API: 40−2=38 en primera venta; SPA después de repetir C-04: 38−2=36. Comprobante 000004: 26.10 y 2 productos | Pasa | X-01_spa.png; C-04_spa.png; X-01_api.json |
| P-01 | Buscadores | Formulario abierto | Texto zzz | Aviso sin coincidencias | Ningún cliente activo coincide | Pasa | P-01_spa.png |
| P-02 | Validaciones | Cliente y A4 elegidos | Cantidad −1 | Rechazar; no confirmar | Cantidad entera mayor que cero; resumen no aparece | Pasa | P-02_spa.png; frontend-tests.txt |
| P-03 | Confirmación | Datos válidos sin revisar | Invocar confirmar directamente | No POST | Prueba verifica ninguna petición /ventas | Pasa | frontend-tests.txt |
| P-04 | Mensajes | Cliente activo | Producto inexistente 99999999 | HTTP 404 con mensaje | HTTP 404; no crea venta | Pasa | P-04_api.json |

## Modalidad y alcance de evidencia

- B-01: SPA + prueba automatizada
- B-02: SPA + prueba automatizada
- C-01: SPA + prueba automatizada
- C-02: SPA + prueba automatizada
- C-03: SPA + prueba automatizada
- C-04: SPA + API HTTP real
- V-01: SPA + prueba automatizada
- V-02: SPA + prueba automatizada
- V-03: SPA + prueba automatizada
- V-04: SPA + prueba automatizada
- V-05: API HTTP real; colección Postman preparada
- F-01: SPA + prueba automatizada
- F-02: Prueba automatizada de componente
- M-01: Dos pestañas SPA + API concurrente real
- M-02: API HTTP real; colección Postman preparada
- I-01: Inspección de capturas anteriores; entrega de archivos incompleta
- X-01: SPA + API HTTP real
- P-01: SPA + prueba automatizada
- P-02: SPA + prueba automatizada
- P-03: Prueba automatizada de componente
- P-04: API HTTP real

Los JSON y logs son evidencias de API/pruebas, no capturas de F12. F-02 y P-03 requieren una captura manual adicional para completar estrictamente el formato visual de la guía. V-05 y M-02 requieren capturas de Postman. No se han fabricado capturas de herramientas.
