# hash_videos_2
para análisis de grandes cantidades de videos con los hash en linux
!/bin/bash
read -p "Introduce la ruta del archivo A (original): " archivo_a

# Si es un directorio, tomar el primer archivo encontrado
if [ -d "$archivo_a" ]; then
    archivo_a=$(find "$archivo_a" -type f | head -n 1)
    echo "Usando archivo: $archivo_a"
fi

# Validar que existe el archivo
if [ ! -f "$archivo_a" ]; then
    echo "No se encontró un archivo válido en la ruta proporcionada."
    exit 1
fi

hash_original=$(sha256sum "$archivo_a" | awk '{print $1}')

read -p "Introduce la ruta del archivo o carpeta B: " ruta_b

encontrado=0

while IFS= read -r -d '' archivo; do
    hash=$(sha256sum "$archivo" | awk '{print $1}')
    if [ "$hash" = "$hash_original" ]; then
        echo "Archivo igual encontrado: $archivo"
        encontrado=1
