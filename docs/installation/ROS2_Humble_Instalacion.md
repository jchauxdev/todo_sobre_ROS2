<div align="center">

# 📥 Instalación de ROS 2 Humble en Ubuntu 22.04
### Método: paquetes `deb` (recomendado)

![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-orange?style=flat-square&logo=ubuntu)
![ROS2](https://img.shields.io/badge/ROS2-Humble%20Hawksbill-blue?style=flat-square&logo=ros)
![Arch](https://img.shields.io/badge/Arch-amd64%20%7C%20arm64-lightgrey?style=flat-square)

[← Volver al README principal](../../README.md)

</div>

Guía completa paso a paso para instalar **ROS 2 Humble Hawksbill** en Ubuntu 22.04 LTS (Jammy Jellyfish).

> **Requisitos:** Ubuntu 22.04 LTS · arquitectura `amd64` o `arm64` · conexión a internet

---

## Tabla de contenidos

- [Paso 1 — Corregir mirror de Ubuntu (error regional)](#paso-1--corregir-mirror-de-ubuntu-error-regional)
- [Paso 2 — Configurar locale UTF-8](#paso-2--configurar-locale-utf-8)
- [Paso 3 — Habilitar repositorio Universe](#paso-3--habilitar-repositorio-universe)
- [Paso 4 — Agregar clave GPG y repositorio de ROS 2](#paso-4--agregar-clave-gpg-y-repositorio-de-ros-2)
- [Paso 5 — Instalar ROS 2 Humble](#paso-5--instalar-ros-2-humble)
- [Paso 6 — Instalar herramientas de desarrollo](#paso-6--instalar-herramientas-de-desarrollo)
- [Paso 7 — Configurar entorno](#paso-7--configurar-entorno)

---

## Paso 1 — Corregir mirror de Ubuntu (error regional)

En algunos países el mirror regional de Ubuntu (`co.archive.ubuntu.com`, `ar.archive.ubuntu.com`, etc.) puede estar caído o tener problemas de IPv6. Si al ejecutar `apt update` ves errores como:

```
Cannot initiate the connection to co.archive.ubuntu.com:80
connect (101: Network is unreachable)
```

Ejecuta esto para redirigir al mirror global:

```bash
sudo sed -i 's|co.archive.ubuntu.com|archive.ubuntu.com|g' /etc/apt/sources.list
sudo apt update
```

> **Nota:** Reemplaza `co` por el prefijo de tu país si es diferente (ej: `ar`, `br`, `mx`).

---

## Paso 2 — Configurar locale UTF-8

ROS 2 requiere un locale en inglés con codificación UTF-8.

```bash
sudo apt install locales -y
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

Verifica que quedó correcto:

```bash
locale
```

La salida debe mostrar `LANG=en_US.UTF-8`.

---

## Paso 3 — Habilitar repositorio Universe

```bash
sudo apt install software-properties-common -y
sudo add-apt-repository universe
```

---

## Paso 4 — Agregar clave GPG y repositorio de ROS 2

Descarga la clave de firma del repositorio oficial:

```bash
sudo apt install curl -y

sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Agrega el repositorio de ROS 2 a las fuentes de apt:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
  http://packages.ros.org/ros2/ubuntu \
  $(. /etc/os-release && echo $UBUNTU_CODENAME) main" \
  | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

Actualiza el índice de paquetes:

```bash
sudo apt update
```

---

## Paso 5 — Instalar ROS 2 Humble

Elige la variante según tu caso de uso:

| Variante | Paquete | Descripción |
|---|---|---|
| **Desktop** (recomendado) | `ros-humble-desktop` | Incluye RViz2, demos, herramientas gráficas y librerías de percepción |
| **Base** (servidores / embebidos) | `ros-humble-base` | Solo las librerías de comunicación, sin GUI |
| **Dev Tools** (adicional) | `ros-dev-tools` | Herramientas de desarrollo extra |

```bash
sudo apt upgrade -y

# Instalar versión Desktop (recomendada para desarrollo)
sudo apt install ros-humble-desktop -y
```

> **Tiempo estimado:** 5–15 minutos dependiendo de la conexión (~800 MB de descarga).

---

## Paso 6 — Instalar herramientas de desarrollo

```bash
sudo apt install python3-colcon-common-extensions \
  python3-rosdep \
  python3-argcomplete \
  python3-pip -y
```

Inicializa `rosdep` (gestor de dependencias de ROS):

```bash
sudo rosdep init
rosdep update
```

> Si `rosdep init` devuelve `ERROR: default sources list file already exists`, puedes ignorarlo — significa que ya fue inicializado antes.

---

## Paso 7 — Configurar entorno

Para que los comandos de ROS 2 estén disponibles en cada nueva terminal, agrega el source al archivo de configuración de tu shell:

### Bash (por defecto en Ubuntu)

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Verifica que ROS 2 está activo:

```bash
printenv | grep ROS
```

Deberías ver variables como `ROS_DISTRO=humble` y `ROS_VERSION=2`.

---

<div align="center">

[← Volver a SDK Manager NVIDIA — Jetson Orin NX](../sdk-manager/jetson-orin-sdk-manager.md) &nbsp;|&nbsp; [Verificar la instalación →](../../docs/verification/talker-listener.md)

**Fuente oficial:** [docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)

</div>
