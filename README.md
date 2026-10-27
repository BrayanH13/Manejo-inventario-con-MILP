# Manejo de inventario con MILP

Proyecto de la Maestría en MOA para analizar información de facturas y avanzar hacia un modelo de manejo de inventario mediante programación lineal entera mixta (MILP).

> **Estado actual:** este repositorio contiene este README. Los scripts descritos abajo están en la carpeta de trabajo original y todavía no se han agregado al repositorio. Tampoco se ha implementado aquí un modelo MILP ni se incluyen datos de ejemplo.

## ¿Qué hace cada script?

| Archivo | Función |
| --- | --- |
| `MOA.py` | Accede a Gmail mediante su API y busca correos de facturación de Dispapeles con adjuntos. Descarga PDF o ZIP, extrae PDF de los ZIP (o XML si no hay PDF), registra los archivos procesados en un manifiesto y luego intenta extraer de los PDF datos de factura e ítems. Genera `historial_facturas.csv` con los registros leídos. |
| `tasas.py` | Lee el historial de facturas y analiza la serie mensual de unidades para los códigos configurados (838 y 839). Resta las cantidades de las notas crédito, calcula tasas diarias y compara meses pico con los demás mediante un modelo Poisson, permutaciones, consistencia entre años y bootstrap. También ofrece comprobaciones de calidad del parseo. |
| `prueba.py` | Comprueba si una cámara IP entrega imágenes por distintas rutas RTSP comunes, deteniéndose cuando recibe un fotograma. Es una prueba independiente de conectividad de cámara; no forma parte del análisis de facturas ni del modelo de inventario. |

## Flujo de trabajo previsto

1. `MOA.py` obtiene adjuntos de factura y produce `historial_facturas.csv` a partir de los PDF que logra leer.
2. `tasas.py` usa ese historial para resumir y analizar las unidades facturadas en el tiempo. Las cantidades facturadas son un insumo/proxy de demanda; no equivalen necesariamente a ventas o consumo observado.
3. Un modelo MILP de inventario deberá definirse e implementarse como etapa posterior. El código descrito no optimiza niveles de inventario, pedidos ni costos.

`prueba.py` es auxiliar y no participa en los dos primeros pasos.

## Requisitos y configuración

Los scripts no están versionados en este repositorio todavía, así que no hay una lista de dependencias instalable ni comandos de ejecución definidos aquí. Por el código revisado, `MOA.py` utiliza Python, `pandas`, `pdfplumber` y las bibliotecas de autenticación/cliente de Google; `tasas.py` utiliza `pandas`, `numpy` y `statsmodels`; `prueba.py` utiliza OpenCV (`cv2`).

Para usar `MOA.py` se necesita configurar la API de Gmail y disponer localmente de las credenciales de OAuth. Configura IP, usuario y contraseña de cámara localmente para `prueba.py`. No pongas secretos directamente en archivos que vayas a publicar.

## Seguridad de datos

No subas `credentials.json`, `token.json`, contraseñas, tokens, direcciones privadas, facturas, archivos adjuntos ni historiales con información comercial o personal. Mantén esos archivos fuera del control de versiones y usa mecanismos locales de configuración para los secretos.