<div align="center">

# 🤖 Transferencia de Información sobre ROS2

![ROS2](https://img.shields.io/badge/ROS2-Humble%20Hawksbill-blue?style=for-the-badge&logo=ros)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-orange?style=for-the-badge&logo=ubuntu)
![LTS](https://img.shields.io/badge/Soporte-LTS%20hasta%202027-green?style=for-the-badge)
![License](https://img.shields.io/badge/Licencia-Apache%202.0-lightgrey?style=for-the-badge)

**Repositorio para recopilar toda la información relacionada con ROS2, su aplicación en Simulación con Gazebo / Isaac Sim y el control de robots reales a la medida.**

</div>

---

## ¿Qué es ROS 2?

**ROS 2** (*Robot Operating System 2*) es un framework de código abierto para el desarrollo de software para robótica. Provee herramientas, bibliotecas y convenciones que simplifican la tarea de crear comportamientos complejos y robustos en robots de todo tipo.

La distribución de ROS2 con la que vamos a trabajar es con **ROS 2 Humble Hawksbill**, la cual es una distribución **LTS (Long Term Support)** con soporte oficial hasta **mayo de 2027**, siendo la versión recomendada para proyectos en producción sobre Ubuntu 22.04 (Jammy Jellyfish).

---

## 📁 Contenido

Este repositorio está organizado en guías independientes. Cada una cubre un aspecto específico del ecosistema ROS 2.

### 🔧 Instalación

| Guía | Descripción | Plataforma |
|---|---|---|
| [📥 Instalación ROS2 Humble Hawksbill en Ubuntu 22.04 (paquetes deb)](docs/installation/ubuntu-deb.md) | Método oficial y recomendado usando `apt`. Incluye instalación Desktop, Base y herramientas de desarrollo | Ubuntu 22.04 |

<!--
| [🟢 Instalación en Jetson Orin NX (JetPack 6)](docs/installation/jetson-orin-nx.md) | Instalación vía imagen SD Card y SDK Manager para la Jetson Orin NX 16 GB | Jetson Orin NX |
-->

### 🧪 Verificación y primeros pasos

| Guía | Descripción |
|---|---|
| [✅ Verificar la instalación](docs/verification/talker-listener.md) | Ejemplo talker-listener en C++ y Python para validar la instalación |
<!--
| [🔍 Solución de problemas](docs/troubleshooting/common-issues.md) | Errores frecuentes y cómo resolverlos |
-->

### ⚙️ Configuración del Espacio de trabajo (Workspace)

| Guía | Descripción |
|---|---|
| [🌍 Configuración del entorno](docs/configuration/environment-setup.md) | Variables de entorno, `.bashrc` |
<!--
| [📡 Configuración de RMW](docs/configuration/rmw-setup.md) | Cambiar el middleware (Fast DDS, Cyclone DDS, Zenoh) |
-->

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

## 📄 Licencia

Este repositorio está bajo la licencia **Apache 2.0**. Consulta el archivo [LICENSE](LICENSE) para más detalles.

La documentación de ROS 2 referenciada es propiedad de [Open Robotics](https://www.openrobotics.org/) bajo licencia Creative Commons Attribution 4.0.

---

<div align="center">

Hecho por **Julián René Cháux Cedeño** para la comunidad ROS hispanohablante

</div>
