<div align="center">

# 📥 Instalación de ROS 2 Humble en Ubuntu 22.04
### Método: paquetes `deb` (recomendado)

![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-orange?style=flat-square&logo=ubuntu)
![ROS2](https://img.shields.io/badge/ROS2-Humble%20Hawksbill-blue?style=flat-square&logo=ros)
![Arch](https://img.shields.io/badge/Arch-amd64%20%7C%20arm64-lightgrey?style=flat-square)

[← Volver al README principal](../../README.md)

</div>

---

## Tabla de contenidos

- [Características y/o Requisitos](#características-yo-requisitos)
- [Paso 1 — Configurar Locale](#paso-1--configurar-locale)
- [Paso 2 — Configurar fuentes APT](#paso-2--configurar-fuentes-apt)
- [Paso 3 — Instalar ROS 2](#paso-3--instalar-ros-2)
- [Paso 4 — Configurar el entorno](#paso-4--configurar-el-entorno)
- [Pasos siguientes](#pasos-siguientes)
- [Desinstalación](#desinstalación)

---

## Características y/o Requisitos

| Característica / Requisito | Detalle |
|---|---|
| 💻 Sistema operativo | Ubuntu **22.04 LTS** (Jammy Jellyfish) |
| 📅 Lanzamiento | Mayo 2022 |
| 🔒 Fin de soporte | Mayo 2027 |
| 🐧 Plataforma principal | Ubuntu 22.04 LTS (Jammy) |
| 🏗️ Arquitecturas | `amd64`, `arm64` |
| 💾 Espacio en disco | ~2 GB (base) / ~5 GB (desktop) |
| 🤖 Compatible con Jetson | JetPack 6.x (Orin NX, Orin Nano, AGX Orin) |
| 📡 Middleware por defecto | Fast DDS (eProsima) |

> **Nota:** Los paquetes `deb` de ROS 2 Humble están disponibles **únicamente** para Ubuntu 22.04. Si usas otra versión de Ubuntu, consulta la [guía de instalación desde fuente](https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html).

---

## Paso 1 — Configurar Locale

ROS 2 requiere que el sistema tenga un locale con soporte **UTF-8**. Si estás en un entorno mínimo (por ejemplo, un contenedor Docker), el locale puede estar configurado como `POSIX` — en ese caso este paso es obligatorio.

Verifica el locale actual:

```bash
sudo sed -i 's|co.archive.ubuntu.com|archive.ubuntu.com|g' /etc/apt/sources.list
sudo apt update
```

Configurar locale `UTF-8`: ROS 2 requiere locale en inglés con `UTF-8`:

```bash
sudo apt install locales -y
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

Verifica que el cambio se aplicó:

```bash
locale
```

La salida debe contener líneas como `LANG=en_US.UTF-8`.

---

## Paso 2 — Configurar fuentes APT

### 2.1 Habilitar el repositorio Universe de Ubuntu

El repositorio **Universe** de Ubuntu debe estar habilitado antes de agregar el repositorio de ROS 2:

```bash
sudo apt install software-properties-common -y
sudo add-apt-repository universe
```

### 2.2 Instalar el paquete de fuentes de ROS 2

El paquete `ros-apt-source` configura automáticamente las claves GPG y las fuentes APT del repositorio oficial de ROS 2. Las actualizaciones de configuración del repositorio ocurren de forma automática cuando se publican nuevas versiones de este paquete.

```bash
sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Descarga e instala el paquete de fuentes:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
  http://packages.ros.org/ros2/ubuntu \
  $(. /etc/os-release && echo $UBUNTU_CODENAME) main" \
  | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

---

## Paso 3 — Instalar ROS 2

### 3.1 Actualizar el sistema

Actualiza los cachés de APT y luego el sistema completo.

```bash
sudo apt update
sudo apt upgrade -y
```

> ⚠️ **Advertencia importante:** En Ubuntu 22.04, los paquetes de `systemd` y `udev` deben actualizarse **antes** de instalar ROS 2. Instalar las dependencias de ROS 2 en un sistema recién instalado sin haber hecho `upgrade` primero puede desencadenar la **eliminación de paquetes críticos del sistema**.
>
> Ver: [ros2/ros2#1272](https://github.com/ros2/ros2/issues/1272) y [Launchpad #1974196](https://bugs.launchpad.net/ubuntu/+source/systemd/+bug/1974196)

### 3.2 Elegir el tipo de instalación

Hay tres opciones. Elige **una** según tu caso de uso:

#### 🖥️ Opción A — Desktop (Recomendada)

Incluye ROS 2, RViz, demos y tutoriales. **Es la opción recomendada** para desarrollo y aprendizaje.

```bash
sudo apt install ros-humble-desktop -y
```

#### 🔨 Herramientas de desarrollo

Compiladores y herramientas para construir paquetes ROS desde fuente. Se puede instalar junto con cualquiera de las opciones anteriores.  Ahora vamos a inicializar rosdep, estos son pasos de inicialización que se hacen una sola vez después de instalar ROS 2.

```bash
sudo apt install python3-colcon-common-extensions python3-rosdep python3-argcomplete -y
sudo rosdep init
rosdep update
```

## Paso 4 — Configurar el entorno

Para usar ROS 2, debes cargar su script de configuración en cada sesión de terminal. Hay dos formas:

### Carga manual (por sesión)

```bash
source /opt/ros/humble/setup.bash
```

### Carga automática (recomendado)

Agrega la línea al final de tu `~/.bashrc` para que se cargue automáticamente en cada terminal:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> **Nota:** Si usas un shell diferente a `bash`, reemplaza el archivo según corresponda:
>
> | Shell | Archivo a agregar | Script a usar |
> |---|---|---|
> | bash | `~/.bashrc` | `setup.bash` |
> | zsh | `~/.zshrc` | `setup.zsh` |
> | sh | `~/.profile` | `setup.sh` |

---


## Pasos siguientes

Una vez completada la instalación, te recomendamos continuar con:

- 📖 [Configuración del entorno](../configuration/environment-setup.md) — Variables de entorno y dominio ROS

<!--
- 🧪 [Verificar la instalación a fondo](../verification/talker-listener.md) — Más ejemplos de prueba
- 📡 [Configurar el middleware RMW](../configuration/rmw-setup.md) — Cambiar Fast DDS por Cyclone DDS u otro
- 🔍 [Solución de problemas](../troubleshooting/common-issues.md) — Si algo no funcionó
-->

O directamente los [tutoriales oficiales de ROS 2 Humble](https://docs.ros.org/en/humble/Tutorials.html):

1. [Beginner: CLI Tools](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools.html)
2. [Beginner: Client Libraries](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries.html)
3. [Intermediate](https://docs.ros.org/en/humble/Tutorials/Intermediate.html)

---

## Desinstalación

Si necesitas desinstalar ROS 2 Humble:

```bash
sudo apt remove '~nros-humble-*' && sudo apt autoremove
```

Para eliminar también el repositorio de fuentes:

```bash
sudo apt remove ros2-apt-source
sudo apt update
sudo apt autoremove
sudo apt upgrade
```

---

<div align="center">

[← Volver a SDK Manager NVIDIA — Jetson Orin NX](../sdk-manager/jetson-orin-sdk-manager.md) &nbsp;|&nbsp; [Verificar la instalación →](../../docs/verification/talker-listener.md)

**Fuente oficial:** [docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)

</div>
