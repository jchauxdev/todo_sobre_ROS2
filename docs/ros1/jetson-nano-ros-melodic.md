<div align="center">

# 🤖 Instalación de ROS Melodic en Jetson Nano

![ROS](https://img.shields.io/badge/ROS-Melodic%20Morenia-blue?style=flat-square&logo=ros)
![Ubuntu](https://img.shields.io/badge/Ubuntu-18.04%20LTS%20Bionic-orange?style=flat-square&logo=ubuntu)
![Jetson](https://img.shields.io/badge/Hardware-Jetson%20Nano%20Dev%20Kit-green?style=flat-square)
![JetPack](https://img.shields.io/badge/JetPack-4.6.x-lightgrey?style=flat-square)

[← Volver al README principal](../../README.md)

</div>

---

> ⚠️ **Importante:** Esta guía aplica únicamente a la **Jetson Nano Developer Kit clásica** (2019), que usa Ubuntu 18.04 con JetPack 4.6.x. Si tienes una Jetson Orin NX o Jetson Orin Nano, consulta la [guía de instalación de ROS 2 Humble](../../docs/installation/ubuntu-deb.md) ya que esas plataformas corren Ubuntu 22.04.

---

## Tabla de contenidos

- [Contexto de la plataforma](#contexto-de-la-plataforma)
- [Requisitos previos](#requisitos-previos)
- [Paso 1 — Configurar el repositorio de ROS](#paso-1--configurar-el-repositorio-de-ros)
- [Paso 2 — Instalar ROS Melodic](#paso-2--instalar-ros-melodic)
- [Paso 3 — Configurar el entorno](#paso-3--configurar-el-entorno)
- [Paso 4 — Instalar e inicializar rosdep](#paso-4--instalar-e-inicializar-rosdep)
- [Paso 5 — Configurar el workspace catkin](#paso-5--configurar-el-workspace-catkin)
- [Verificar la instalación](#verificar-la-instalación)
- [Notas sobre ROS 2 Humble en Jetson Nano](#notas-sobre-ros-2-humble-en-jetson-nano)

---

## Contexto de la plataforma

La Jetson Nano Developer Kit (2019) está basada en el SoC Tegra T210 (arquitectura Maxwell), el mismo núcleo que el Jetson TX1. Es una plataforma de bajo consumo y costo accesible, diseñada para visión por computadora y deep learning en el borde.

| Parámetro | Detalle |
|---|---|
| JetPack máximo soportado | 4.6.4 |
| Sistema operativo | Ubuntu 18.04 LTS (Bionic Beaver) |
| Kernel | 4.9.337-tegra (EOL) |
| ROS recomendado | **Melodic Morenia** |
| ROS 2 nativo | ❌ No soportado (ver nota al final) |

> ROS Melodic Morenia es la distribución de ROS 1 diseñada específicamente para Ubuntu 18.04, por lo que es la elección natural y la única con soporte oficial nativo en esta plataforma.

---

## Requisitos previos

- Jetson Nano Developer Kit con JetPack 4.6.x instalado y funcionando
- Ubuntu 18.04 activo (instalado vía imagen SD oficial de NVIDIA)
- Conexión a internet
- Terminal abierta (`Ctrl + Alt + T`)

---

## Paso 1 — Configurar el repositorio de ROS

Configura la Jetson Nano para aceptar software desde `packages.ros.org`:

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

Agrega la clave GPG del repositorio:

```bash
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

Actualiza el índice de paquetes:

```bash
sudo apt update
```

---

## Paso 2 — Instalar ROS Melodic

Instala el paquete **ROS Desktop**, que incluye soporte para `rqt`, `rviz` y otros paquetes esenciales para robótica:

```bash
sudo apt install ros-melodic-desktop -o Dpkg::Options::="--force-overwrite"
```

> **¿Por qué Desktop y no Desktop Full?**
>
> El paquete `ros-melodic-desktop-full` incluye simuladores 2D/3D como Gazebo, que consumen demasiado espacio en almacenamiento y recursos computacionales en una plataforma embebida como la Jetson Nano. El paquete `desktop` es el balance correcto entre funcionalidad y rendimiento para esta plataforma.

| Paquete | Contenido | Recomendado para Jetson Nano |
|---|---|---|
| `ros-melodic-desktop` | ROS base + rqt + rviz | ✅ Sí |
| `ros-melodic-desktop-full` | Todo lo anterior + simuladores Gazebo | ❌ No |
| `ros-melodic-ros-base` | Solo comunicación y mensajes, sin GUI | ⚠️ Solo si el espacio es crítico |

---

## Paso 3 — Configurar el entorno

Carga las variables de entorno de ROS automáticamente en cada nueva sesión de terminal:

```bash
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Verifica que el entorno quedó configurado correctamente:

```bash
printenv | grep ROS
```

Deberías ver variables como `ROS_DISTRO=melodic` y `ROS_ROOT=/opt/ros/melodic/share/ros`.

---

## Paso 4 — Instalar e inicializar rosdep

`rosdep` permite instalar automáticamente las dependencias del sistema para paquetes ROS que quieras compilar desde fuente. También es requerido por algunos componentes centrales de ROS.

Instala las herramientas necesarias:

```bash
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```

Inicializa `rosdep` (solo se hace una vez):

```bash
sudo rosdep init
rosdep update
```

> **Nota:** Si ves el error `sudo: rosdep: command not found`, asegúrate de que el paso anterior se completó sin errores y de haber ejecutado `source ~/.bashrc` antes de continuar.

---

## Paso 5 — Configurar el workspace catkin

`catkin` es el sistema de compilación de ROS 1. Necesitas un workspace para desarrollar o instalar paquetes desde fuente.

Instala las dependencias adicionales:

```bash
sudo apt-get install cmake python-catkin-pkg python-empy python-nose python-setuptools libgtest-dev python-rosinstall python-rosinstall-generator python-wstool build-essential git
```

Crea la estructura del workspace:

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
```

Inicializa el workspace con una compilación vacía:

```bash
catkin_make
```

Agrega el workspace al entorno de ROS:

```bash
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

La estructura final del workspace es:

```
~/catkin_ws/
├── src/          ← aquí van los paquetes fuente
├── build/        ← generado por catkin_make
└── devel/        ← binarios y setup.bash del workspace
```

---

## Verificar la instalación

Abre **dos terminales** para probar la comunicación básica de ROS.

**Terminal 1** — Inicia el nodo maestro de ROS:

```bash
roscore
```

**Terminal 2** — Verifica que ROS detecta el master y lista los topics:

```bash
rostopic list
```

Deberías ver al menos `/rosout` y `/rosout_agg`. Si es así, **¡ROS Melodic está funcionando correctamente!** 🎉

---

## Notas sobre ROS 2 Humble en Jetson Nano

Si tu proyecto requiere ROS 2 Humble en la Jetson Nano clásica, la única alternativa funcional es usar **Docker**. El sistema operativo host permanece en Ubuntu 18.04 con JetPack 4.6.x, y ROS 2 corre dentro de un contenedor.

El proyecto [jetson-containers](https://github.com/dusty-nv/jetson-containers) de Dustin Franklin (NVIDIA) provee las imágenes necesarias:

```bash
# Instalar jetson-containers
git clone https://github.com/dusty-nv/jetson-containers
bash jetson-containers/install.sh

# Construir contenedor con ROS 2 Humble
jetson-containers build --name=ros2_humble ros:humble-desktop

# Ejecutar el contenedor
jetson-containers run ros2_humble
```

> **Limitación:** El rendimiento dentro del contenedor es notablemente inferior al nativo. Para proyectos de robótica que requieren ROS 2 Humble con buen rendimiento, se recomienda migrar a la **Jetson Orin Nano** o **Jetson Orin NX**, que tienen soporte nativo para Ubuntu 22.04 y ROS 2 Humble.

---

<div align="center">

[← Volver a Grabado de imagen SD con Balena Etcher en Jetson Nano](docs/ros1/jetson-nano-sd-balena-etcher.md) &nbsp;


**Fuente:** [Stereolabs — Getting Started with ROS on Jetson Nano](https://www.stereolabs.com/blog/ros-and-nvidia-jetson-nano) &nbsp;|&nbsp; [ROS Wiki — Melodic](https://wiki.ros.org/melodic)

</div>
