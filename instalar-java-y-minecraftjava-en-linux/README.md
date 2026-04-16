***

# Guía de Instalación: Java y Minecraft en Linux

Este documento proporciona los pasos necesarios para configurar el entorno de ejecución de Java (JRE) y la instalación de Minecraft en distribuciones basadas en Linux.

---

## 1. Instalación de Java (OpenJDK)

Para ejecutar Minecraft, se requiere **OpenJDK**. Aunque existen varias versiones, Minecraft moderno (1.20+) funciona mejor con **Java 17 o 21**.

### Comandos según tu distribución:

| Distribución | Comando de instalación |
| :--- | :--- |
| **Arch Linux** | `sudo pacman -S jre17-openjdk` |
| **Debian / Ubuntu** | `sudo apt update && sudo apt install openjdk-17-jre` |
| **Fedora** | `sudo dnf install java-17-openjdk` |
| **Void Linux** | `sudo xbps-install -S openjdk17` |
| **Gentoo** | `sudo emerge --ask dev-java/openjdk-bin:17` |

*Para verificar la instalación, ejecuta:* `java -version`

---

## 2. Ejecución de archivos .jar

Si has descargado un archivo `.jar` (como un instalador de mod o un launcher), puedes ejecutarlo mediante la terminal:

1. **Dar permisos de ejecución:**
   ```bash
   chmod +x archivo.jar
   ```
2. **Ejecutar el archivo:**
   ```bash
   java -jar archivo.jar
   ```

---

## 3. Instalación de Minecraft (Recomendada)

Aunque puedes usar el launcher oficial, se recomienda encarecidamente utilizar **Prism Launcher** o **MultiMC**. Estos launchers permiten gestionar versiones de Java, perfiles de Forge/Fabric y mods de forma aislada y eficiente.

### Instalación de Prism Launcher:

* **Arch Linux:** `sudo pacman -S prismlauncher`
* **Flatpak (Universal):** `flatpak install flathub org.prismlauncher.PrismLauncher`
* **Debian/Ubuntu:** Disponible vía PPA o mediante el paquete `.deb` en su [sitio oficial](https://prismlauncher.org/).

---

## 4. Instalación de Modloaders (Forge/Fabric/OptiFine)

Si decides hacer la instalación manual, sigue estos pasos estándar:

### Pasos Generales:
1. **Descarga el instalador:** Obtén el archivo `.jar` desde las páginas oficiales (FabricMC, MinecraftForge o OptiFine.net).
2. **Ejecución:** Usa el comando `java -jar nombre_del_instalador.jar`.
3. **Instalación:**
   * **Forge/Fabric:** Sigue la interfaz gráfica del instalador y apunta al directorio de tu instancia de Minecraft (usualmente `~/.minecraft/`).
   * **OptiFine:** * *Como mod:* Simplemente coloca el archivo `.jar` en la carpeta `mods` de tu carpeta de Minecraft.
     * *Como versión independiente:* Ejecuta el `.jar` y selecciona "Install".

---

## Solución de Problemas (Troubleshooting)

* **¿El .jar no abre?** Asegúrate de que Java esté correctamente instalado (`java -version`).
* **¿Problemas de RAM?** Si el juego se cierra al iniciar, aumenta la memoria asignada en tu launcher: `-Xmx4G` (para 4GB de RAM).
* **¿Error de gráficos?** Asegúrate de tener instalados los controladores de video (`mesa` para AMD/Intel, `nvidia` para tarjetas NVIDIA).

---

 **JVM (Java Virtual Machine)**

Para que Minecraft y tu servidor (especialmente con mods) funcionen de forma fluida en Arch Linux o cualquier otra distribución, es fundamental ajustar los parámetros de memoria. Aquí tienes una explicación de cómo optimizar este proceso:

### Argumentos de la JVM para rendimiento
Cuando ejecutas Java manualmente o a través de un launcher, puedes pasarle argumentos que optimizan el uso de la memoria y el recolector de basura (Garbage Collector).

Un ejemplo de comando optimizado para una instancia que requiere 4GB de RAM es:

```bash
java -Xmx4G -Xms4G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -jar minecraft_server.jar
```



### Explicación de los parámetros:
* `-Xmx4G`: Establece la memoria máxima a 4GB. Ajusta esto según la RAM total de tu equipo (nunca asignes más del 75% de tu RAM total).
* `-Xms4G`: Establece la memoria inicial. Igualar este valor con el `-Xmx` evita que Java tenga que reasignar memoria dinámicamente, lo que causa tirones (lag spikes).
* `-XX:+UseG1GC`: Activa el recolector de basura G1, que es el estándar moderno para aplicaciones que requieren baja latencia, ideal para juegos.
* `-XX:MaxGCPauseMillis=200`: Intenta limitar el tiempo que el juego "se congela" para limpiar la memoria a 200 milisegundos.

### Recomendación adicional
Dado que estás configurando un servidor, recuerda que los archivos de configuración (como `server.properties`) también afectan el rendimiento. Si experimentas problemas, verifica la opción `view-distance` en ese archivo; reducirla de 10 a 6-8 puede liberar muchísima carga en la CPU y RAM.

