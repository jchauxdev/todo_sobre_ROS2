<div align="center">

# 🖥️ Triple Boot: Windows 11 + Ubuntu 24.04 + Ubuntu 22.04
### Configuración de arranque múltiple en hardware AMD Ryzen 7 5700G / NVMe

![Windows](https://img.shields.io/badge/Windows-11-0078D6?style=flat-square&logo=windows11&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04%20LTS-orange?style=flat-square&logo=ubuntu)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-orange?style=flat-square&logo=ubuntu)
![Hardware](https://img.shields.io/badge/Hardware-AMD%20Ryzen%207%205700G%20%2F%20NVMe-ED1C24?style=flat-square&logo=amd&logoColor=white)

[← Volver al README principal](../../README.md)

</div>

---

## Tabla de contenidos

- [Descripción general](#descripción-general)
- [Requisitos](#requisitos)
- [Paso 1 — Descargas oficiales y herramientas](#paso-1--descargas-oficiales-y-herramientas)
- [Paso 2 — Preparación crítica del host (Windows 11)](#paso-2--preparación-crítica-del-host-windows-11)
- [Paso 3 — Configuración del USB booteable (Rufus)](#paso-3--configuración-del-usb-booteable-rufus)
- [Paso 4 — Instalación manual (particionado)](#paso-4--instalación-manual-particionado)
- [Resolución de problemas comunes](#resolución-de-problemas-comunes)

---

## Descripción general

Esta guía detalla el procedimiento para configurar un sistema de **triple arranque** en hardware moderno (AMD Ryzen 7 5700G / NVMe), garantizando la integridad del **Windows Boot Manager** y la coexistencia de múltiples versiones de Linux para tareas de ingeniería y robótica.

---

## Requisitos

| Característica / Requisito | Detalle |
|---|---|
| 💻 Hardware validado | AMD Ryzen 7 5700G + almacenamiento NVMe |
| 🪟 Sistema host | Windows 11 |
| 🐧 Linux #1 | Ubuntu 24.04 LTS (Noble Numbat) — uso general y desarrollo moderno |
| 🐧 Linux #2 | Ubuntu 22.04 LTS (Jammy Jellyfish) — compatibilidad con NVIDIA SDK Manager (Jetson Orin Nano) |
| 💾 Espacio en disco | Mínimo 50 GB de espacio no asignado por cada versión de Linux |
| 🔥 Herramienta de grabado | [Rufus](https://rufus.ie/) — esquema de partición GPT |
| 🥾 Modo de arranque | UEFI (no CSM) |

> ⚠️ Antes de modificar las particiones, es obligatorio realizar los ajustes del [Paso 2](#paso-2--preparación-crítica-del-host-windows-11) para evitar bloqueos del TPM y del sistema de archivos.

---

## Paso 1 — Descargas oficiales y herramientas

**Sistemas operativos:**

- [Ubuntu 24.04 LTS (Noble Numbat)](https://releases.ubuntu.com/noble/) — Para uso general y desarrollo moderno.
- [Ubuntu 22.04 LTS (Jammy Jellyfish)](https://releases.ubuntu.com/22.04/) — Requerido para compatibilidad con NVIDIA SDK Manager (Jetson Orin Nano).

**Herramientas de grabación:**

- [Rufus](https://rufus.ie/) — Herramienta recomendada para garantizar el esquema GPT.

---

## Paso 2 — Preparación crítica del host (Windows 11)

Antes de modificar las particiones, es obligatorio realizar estos ajustes para evitar bloqueos del TPM y del sistema de archivos.

### 2.1 Desactivar Cifrado de Dispositivo (BitLocker)

Si el disco está cifrado, el instalador de Ubuntu puede no reconocer correctamente el espacio libre o dejar la partición Windows inaccesible tras el particionado.

1. Abrir **Configuración** (`Win + I`).
2. Ir a **Privacidad y seguridad**. La entrada **"Cifrado de dispositivo"** debería aparecer dentro del bloque **Seguridad**, junto a "Seguridad de Windows".
   - **Si no aparece ahí** (caso común en **PC de escritorio**, como un Ryzen 7 5700G sin soporte de *Modern Standby*): Windows Home requiere ese soporte para ofrecer Cifrado de dispositivo, así que en muchos equipos de escritorio la opción **directamente no existe** en el menú — lo cual significa que el disco **no está cifrado** y este paso no aplica.
   - Para confirmarlo, escribe `cmd` en el buscador de Windows, abre el **Símbolo del sistema** y ejecuta:

     ```cmd
     manage-bde -status C:
     ```

     Si el estado de protección indica **"Protección desactivada"** o el comando responde que BitLocker no está disponible, el disco no está cifrado y puedes pasar directo al [punto 2.2](#22-desactivar-inicio-rápido-fast-startup).
   - En ediciones **Windows 11 Pro/Enterprise**, busca en su lugar **"Administrar BitLocker"** desde el buscador de Windows, o ve al **Panel de control** → `Sistema y seguridad` → `Cifrado de unidad BitLocker`.
3. Si el cifrado sí está activo, desactívalo (interruptor de **Cifrado de dispositivo**, o "Desactivar BitLocker") y espera a que la unidad `(C:)` termine de descifrarse por completo (puede tardar varios minutos).

### 2.2 Desactivar Inicio Rápido (Fast Startup)

El Inicio Rápido deja el disco en un estado de "hibernación parcial" que puede impedir que GRUB detecte correctamente las particiones de Windows.

1. Abrir el **Panel de control** (buscarlo desde el menú Inicio) → **Sistema y seguridad** → **Opciones de energía**.
2. En el panel izquierdo, hacer clic en **"Elegir el comportamiento de los botones de inicio/apagado"**.
3. Hacer clic en **"Cambiar la configuración actualmente no disponible"** (requiere permisos de administrador).
4. En la sección **"Configuración de apagado"**, desmarcar la casilla **"Activar inicio rápido (recomendado)"**.
5. Hacer clic en **Guardar cambios**.

### 2.3 Gestión de espacio (reducir la partición de Windows)

1. Presionar `Win + X` (o clic derecho sobre el botón Inicio) y seleccionar **Administración de discos** (*Disk Management*).
2. Hacer clic derecho sobre el volumen `(C:)` y seleccionar **"Reducir volumen"** (*Shrink Volume*).
3. Windows calculará el espacio máximo disponible para reducir; ingresar la cantidad en MB (mínimo **51200 MB** ≈ 50 GB **por cada versión de Linux**, es decir, ~100 GB en total para las dos instalaciones de Ubuntu).
4. Hacer clic en **Reducir**. El espacio liberado aparecerá como **"Espacio no asignado"** (en negro) junto a la partición `(C:)` — este es el espacio que se usará durante la instalación de Ubuntu.

> ⚠️ No formatear ni asignar una letra de unidad al espacio no asignado: el instalador de Ubuntu se encargará de crear las particiones sobre él en el [Paso 4](#paso-4--instalación-manual-particionado).

---

## Paso 3 — Configuración del USB booteable (Rufus)

Para que el arranque sea reconocido por la BIOS/UEFI moderna:

| Parámetro | Valor |
|---|---|
| Esquema de partición | GPT |
| Sistema de destino | UEFI (no CSM) |
| Modo de escritura | "Escribir en modo imagen ISO (Recomendado)" |

---

## Paso 4 — Instalación manual (particionado)

Al iniciar el instalador de Ubuntu, seleccionar siempre **"Algo más"** para gestionar las particiones manualmente:

1. **Punto de montaje:** seleccionar el espacio libre y crear una partición `Ext4` con punto de montaje `/`.
2. **Partición EFI:** identificar la partición existente de ~272 MB (donde reside Windows). **Nunca formatearla.** El instalador añadirá automáticamente la entrada de GRUB ahí.
3. **Bootloader:** instalar el cargador de arranque en el disco raíz (ej. `/dev/nvme0n1`), no en una partición individual.

---

## Resolución de problemas comunes

### Error de PIN en Windows

Es normal que Windows solicite reconfigurar el PIN debido a cambios detectados por el TPM al modificar el arranque.

### Installer Crashed

Si el instalador de Ubuntu 22.04 falla, desconectarse de Internet durante la instalación y usar `GParted` para crear la partición previamente.

### Sincronización de GRUB

Para ver todos los sistemas en el menú de inicio, ejecutar en la terminal de Linux:

```bash
sudo os-prober
sudo update-grub
```

---

<div align="center">

[← Volver al README principal](../../README.md)

**Referencias:** [Ubuntu Desktop](https://ubuntu.com/download/desktop) &nbsp;|&nbsp; [Rufus](https://rufus.ie/)

</div>
