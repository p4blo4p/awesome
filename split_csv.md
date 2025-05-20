*Script Bash para Dividir un Archivo CSV en Partes con Encabezados*
Aquí te muestro un ejemplo de script Bash que divide un archivo CSV en partes de un número específico de líneas, manteniendo los encabezados en cada archivo resultante. El script acepta argumentos para el archivo de entrada, el número de líneas por archivo y el prefijo para los archivos de salida.

*Script*
```
bash
#!/bin/bash

# Verificar si se proporcionaron los argumentos necesarios
if [ $# -ne 3 ]; then
    echo "Uso: $0 <archivo_entrada> <lineas_por_archivo> <prefijo_salida>"
    exit 1
fi

# Asignar argumentos a variables
ARCHIVO_ENTRADA=$1
LINEAS_POR_ARCHIVO=$2
PREFIJO_SALIDA=$3

# Verificar si el archivo de entrada existe
if [ ! -f "$ARCHIVO_ENTRADA" ]; then
    echo "El archivo $ARCHIVO_ENTRADA no existe."
    exit 1
fi

# Extraer el encabezado del archivo de entrada
head -n 1 "$ARCHIVO_ENTRADA" > encabezado.csv

# Omitir el encabezado y dividir el archivo
tail -n +2 "$ARCHIVO_ENTRADA" | split -l "$LINEAS_POR_ARCHIVO" - "${PREFIJO_SALIDA}_" --additional-suffix=.csv

# Agregar el encabezado a cada archivo dividido
for file in "${PREFIJO_SALIDA}"_*.csv; do
    cat encabezado.csv "$file" > temp.csv
    mv temp.csv "$file"
done

# Eliminar el archivo de encabezado temporal
rm encabezado.csv
```

*Uso*
Para utilizar este script, guárdalo en un archivo (por ejemplo, `dividir_csv.sh`), hazlo ejecutable con `chmod +x dividir_csv.sh`, y luego ejecútalo proporcionando los argumentos necesarios:

```
bash
./dividir_csv.sh archivo_entrada.csv 50000 prefijo_salida
```

Reemplaza `archivo_entrada.csv` con el nombre de tu archivo CSV, `50000` con el número de líneas deseado por archivo, y `prefijo_salida` con el prefijo que deseas para los archivos de salida.