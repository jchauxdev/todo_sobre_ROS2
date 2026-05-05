<div align="center">

# 🤖 ROS 2 Humble Hawksbill — Guía de Instalación y Configuración

![ROS2](https://img.shields.io/badge/ROS2-Humble%20Hawksbill-blue?style=for-the-badge&logo=ros)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-orange?style=for-the-badge&logo=ubuntu)
![LTS](https://img.shields.io/badge/Soporte-LTS%20hasta%202027-green?style=for-the-badge)
![License](https://img.shields.io/badge/Licencia-Apache%202.0-lightgrey?style=for-the-badge)

**Repositorio de documentación paso a paso para instalar, configurar y comenzar a usar ROS 2 Humble Hawksbill en Ubuntu 22.04.**

[📦 Instalación](#-contenido-del-repositorio) • [🚀 Inicio rápido](#-inicio-rápido) • [🤝 Contribuir](#-contribuir) • [📄 Licencia](#-licencia)

</div>

---

## ¿Qué es ROS 2?

**ROS 2** (*Robot Operating System 2*) es un framework de código abierto para el desarrollo de software para robótica. Provee herramientas, bibliotecas y convenciones que simplifican la tarea de crear comportamientos complejos y robustos en robots de todo tipo.

**ROS 2 Humble Hawksbill** es una distribución **LTS (Long Term Support)** con soporte oficial hasta **mayo de 2027**, siendo la versión recomendada para proyectos en producción sobre Ubuntu 22.04 (Jammy Jellyfish).

### ¿Por qué Humble?

| Característica | Detalle |
|---|---|
| 📅 Lanzamiento | Mayo 2022 |
| 🔒 Fin de soporte | Mayo 2027 |
| 🐧 Plataforma principal | Ubuntu 22.04 LTS (Jammy) |
| 🏗️ Arquitecturas | `amd64`, `arm64` |
| 🤖 Compatible con Jetson | JetPack 6.x (Orin NX, Orin Nano, AGX Orin) |
| 📡 Middleware por defecto | Fast DDS (eProsima) |

---

## 📁 Contenido del repositorio

Este repositorio está organizado en guías independientes. Cada una cubre un aspecto específico del ecosistema ROS 2 Humble.

### 🔧 Instalación

| Guía | Descripción | Plataforma |
|---|---|---|
| [📥 Instalación en Ubuntu 22.04 (paquetes deb)](docs/installation/ubuntu-deb.md) | Método oficial y recomendado usando `apt`. Incluye instalación Desktop, Base y herramientas de desarrollo | Ubuntu 22.04 |

<!--
| [🟢 Instalación en Jetson Orin NX (JetPack 6)](docs/installation/jetson-orin-nx.md) | Instalación vía imagen SD Card y SDK Manager para la Jetson Orin NX 16 GB | Jetson Orin NX |
-->
### ⚙️ Configuración

| Guía | Descripción |
|---|---|
| [🌍 Configuración del entorno](docs/configuration/environment-setup.md) | Variables de entorno, `.bashrc`, shells alternativos (zsh, sh) |
| [📡 Configuración de RMW](docs/configuration/rmw-setup.md) | Cambiar el middleware (Fast DDS, Cyclone DDS, Zenoh) |

### 🧪 Verificación y primeros pasos

| Guía | Descripción |
|---|---|
| [✅ Verificar la instalación](docs/verification/talker-listener.md) | Ejemplo talker-listener en C++ y Python para validar la instalación |
| [🔍 Solución de problemas](docs/troubleshooting/common-issues.md) | Errores frecuentes y cómo resolverlos |

---

## 🚀 Inicio rápido

Si tienes **Ubuntu 22.04** y quieres instalar ROS 2 Humble lo antes posible:

```bash
# 1. Configurar locale
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

# 2. Agregar repositorio ROS 2
sudo apt install software-properties-common curl -y
sudo add-apt-repository universe
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
export CODENAME=$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.${CODENAME}_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb

# 3. Instalar ROS 2
sudo apt update && sudo apt upgrade
sudo apt install ros-humble-desktop ros-dev-tools

# 4. Configurar entorno
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> Para el procedimiento completo con explicaciones y opciones, ve a la [guía de instalación detallada](docs/installation/ubuntu-deb.md).

---

## 🗂️ Estructura del repositorio

```
ros2-humble-guide/
│
├── README.md                          ← Estás aquí
│
└── docs/
    ├── installation/
    │   ├── ubuntu-deb.md              ← Instalación en Ubuntu 22.04
    │   └── jetson-orin-nx.md          ← Instalación en Jetson Orin NX
    │
    ├── configuration/
    │   ├── environment-setup.md       ← Configuración del entorno
    │   └── rmw-setup.md               ← Configuración del middleware
    │
    ├── verification/
    │   └── talker-listener.md         ← Verificar instalación
    │
    └── troubleshooting/
        └── common-issues.md           ← Solución de problemas
```

---

## 📋 Requisitos previos

Antes de comenzar asegúrese de cumplir con los siguientes requisitos:

- **Sistema operativo:** Ubuntu 22.04 LTS (Jammy Jellyfish) — 64 bits
- **Arquitectura:** `amd64` (PC/laptop) o `arm64` (Jetson, Raspberry Pi)
- **Acceso a internet** para descargar paquetes
- **Permisos `sudo`** en el sistema
- **Espacio en disco:** mínimo 5 GB libres (Desktop install)

> ⚠️ ROS 2 Humble **no** es compatible con Ubuntu 20.04 via paquetes `apt`. Para Ubuntu 20.04 se debe instalar desde fuente o usar Docker.

---

## 🔗 Referencias oficiales

- 📖 [Documentación oficial ROS 2 Humble](https://docs.ros.org/en/humble/)
- 📦 [Página de instalación Ubuntu (deb)](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)
- 🐛 [Repositorio ros2/ros2 en GitHub](https://github.com/ros2/ros2)
- 💬 [Foro de la comunidad ROS](https://discourse.ros.org/)
- 🆘 [ROS Answers (Q&A)](https://answers.ros.org/)
- 📊 [Estado de los paquetes amd64](http://repo.ros2.org/status_page/ros_humble_default.html)
- 📊 [Estado de los paquetes arm64](http://repo.ros2.org/status_page/ros_humble_ujv8.html)

---

## 🤝 Contribuir

¿Encontraste un error o quieres agregar contenido? Las contribuciones son bienvenidas.

1. Haz un fork del repositorio
2. Crea una rama: `git checkout -b mejora/descripcion-corta`
3. Realiza tus cambios y haz commit: `git commit -m "docs: descripción del cambio"`
4. Abre un Pull Request describiendo qué cambiaste y por qué

Por favor usa mensajes de commit en formato [Conventional Commits](https://www.conventionalcommits.org/).

---

## 📄 Licencia

Este repositorio está bajo la licencia **Apache 2.0**. Consulta el archivo [LICENSE](LICENSE) para más detalles.

La documentación de ROS 2 referenciada es propiedad de [Open Robotics](https://www.openrobotics.org/) bajo licencia Creative Commons Attribution 4.0.

---

<div align="center">

Hecho con 🤖 para la comunidad ROS hispanohablante

</div>
