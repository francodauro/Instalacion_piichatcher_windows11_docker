# Guía de Instalación de PII Catcher en Windows usando Docker

A continuación, se describe el procedimiento para instalar y configurar **PII Catcher** en un sistema Windows utilizando Docker y Windows Subsystem for Linux (WSL).

---

## **Prerrequisitos**

1. **Docker Desktop:**
   - Asegúrate de tener instalado Docker Desktop en tu sistema Windows.
   - Habilita la integración con WSL2 en la configuración de Docker Desktop.

2. **WSL:**
   - Instala WSL2 y asegúrate de tener una distribución de Linux instalada (por ejemplo, Ubuntu).

3. **Acceso a la terminal de Linux:**
   - Abre una terminal de WSL para ejecutar los comandos necesarios.

---

## **Instrucciones de Instalación**

### 1. Crear la estructura de directorios para configuración

PII Catcher requiere un directorio de configuración para almacenar datos como configuraciones y catálogos. Ejecuta los siguientes comandos en tu terminal de WSL:

```bash
mkdir -p ~/.config/tokern
mkdir -p ~/.config/goog-stats
```

### 2. Ajustar permisos de las carpetas

Asegúrate de que las carpetas creadas tengan los permisos necesarios:

```bash
chmod -R 777 ~/.config/tokern
chmod -R 777 ~/.config/goog-stats
```

### 3. Ejecutar PII Catcher con Docker

Usa el siguiente comando para iniciar PII Catcher desde Docker:

```bash
docker run \
    -v ${HOME}/.config:/root/.config \
    -e HOME=/root \
    -u root \
    -it --add-host=host.docker.internal:host-gateway \
    tokern/piicatcher:latest --help
```

**Explicación de los argumentos:**
- `-v ${HOME}/.config:/root/.config`: Monta el directorio de configuración de tu sistema al contenedor Docker.
- `-e HOME=/root`: Establece la variable de entorno `HOME` en el contenedor.
- `-u root`: Ejecuta el contenedor con permisos de administrador.
- `--add-host=host.docker.internal:host-gateway`: Permite la comunicación entre el contenedor y el host.

### 4. Verificar la instalación

Una vez ejecutado el comando anterior, deberías ver la ayuda de `piicatcher`:

```text
Usage: piicatcher [OPTIONS] COMMAND [ARGS]...
```
Esto confirma que PII Catcher está funcionando correctamente.

---

## **Uso Común**

### Listar los detectores disponibles:
```bash
piicatcher detectors
```

### Escanear un archivo local (por ejemplo, CSV o Parquet):
```bash
piicatcher detect --path /ruta/al/archivo.csv
```

### Generar salida en formato JSON:
```bash
piicatcher detect --path /ruta/al/archivo.csv --output-format json
```

---

## **Opcional: Configurar un Alias**

Para simplificar el uso de PII Catcher, puedes crear un alias en tu terminal:

1. Abre el archivo de configuración de tu shell (por ejemplo, `~/.bashrc` o `~/.zshrc`).
2. Agrega la siguiente línea:

   ```bash
   alias piicatcher='docker run -v ${HOME}/.config:/root/.config -e HOME=/root -u root -it --add-host=host.docker.internal:host-gateway tokern/piicatcher:latest'
   ```
3. Recarga la configuración del shell:

   ```bash
   source ~/.bashrc
   ```
4. Ahora puedes usar PII Catcher simplemente ejecutando:

   ```bash
   piicatcher --help
   ```

---

## **Solución de Problemas**

1. **Permisos denegados:**
   - Si encuentras errores de permisos, asegúrate de que las carpetas en `~/.config` tienen permisos amplios:
     ```bash
     chmod -R 777 ~/.config
     ```

2. **El contenedor no inicia:**
   - Verifica que Docker Desktop está corriendo y que tu distribución WSL está correctamente integrada con Docker.

3. **Problemas de red:**
   - Asegúrate de que el argumento `--add-host=host.docker.internal:host-gateway` está incluido para permitir la comunicación entre el contenedor y el host.

---

Con esta guía deberías poder instalar y usar PII Catcher sin problemas en tu sistema Windows. ¡Buena suerte y no dudes en pedir ayuda si encuentras dificultades adicionales!

