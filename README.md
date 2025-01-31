# Introducción al sistema operativo Linux 2

## **1. Introducción**

En bioinformática, el análisis de datos biológicos requiere herramientas que permitan la reproducibilidad, la escalabilidad y la portabilidad. A continuación, se describen tres herramientas esenciales:

1. **Nextflow**: Un motor de flujo de trabajo que permite crear pipelines reproducibles y escalables.
2. **Docker**: Una plataforma de contenedores que empaqueta aplicaciones y sus dependencias para garantizar consistencia en diferentes entornos.
3. **Singularity**: Una herramienta de contenedores diseñada específicamente para entornos de alto rendimiento (HPC) y sistemas donde Docker no es viable.

---

## **2. Instalación de las herramientas**

### **2.1. Instalación de Nextflow**

**Requisitos previos:**
- Java 8 o superior.

**Pasos de instalación:**
1. Descarga Nextflow:
   ```bash
   curl -s https://get.nextflow.io | bash
   ```
2. Mueve Nextflow a un directorio en tu PATH:
   ```bash
   sudo mv nextflow /usr/local/bin/
   ```
3. Verifica la instalación:
   ```bash
   nextflow -version
   ```

---

### **2.2. Instalación de Docker**

**Requisitos previos:**
- Un sistema Linux con acceso de superusuario (sudo).

**Pasos de instalación:**
1. Actualiza los paquetes del sistema:
   ```bash
   sudo apt-get update
   ```
2. Instala dependencias para usar repositorios HTTPS:
   ```bash
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```
3. Agrega la clave GPG oficial de Docker:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```
4. Agrega el repositorio de Docker:
   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```
5. Instala Docker:
   ```bash
   sudo apt-get update
   sudo apt-get install docker-ce
   ```
6. Verifica la instalación:
   ```bash
   sudo docker --version
   ```
7. Agregar tu usuario al grupo
   ```bash
   grep docker /etc/group
   sudo usermod -aG docker ins_user
   docker --version
   ```
   
---

### **2.3. Instalación de Singularity**

**Requisitos previos:**
- Un sistema Linux con acceso de superusuario (sudo).
- Go (versión 1.17 o superior).
- Dependencias adicionales: `cryptsetup`, `glib-2.0`, etc.

**Pasos de instalación:**

1. **Instala dependencias adicionales:**
   - Instala `cryptsetup` y `glib-2.0`:
     ```bash
     sudo apt-get update
     sudo apt-get install -y cryptsetup-bin libglib2.0-dev
     ```
   - Verifica que `cryptsetup` esté instalado:
     ```bash
     cryptsetup --version
     ```
   - Verifica que `glib-2.0` esté instalado:
     ```bash
     pkg-config --cflags glib-2.0
     ```

2. **Instala Go (Golang):**
   - Descarga Go:
     ```bash
     wget https://go.dev/dl/go1.21.3.linux-amd64.tar.gz
     ```
   - Extrae e instala Go:
     ```bash
     sudo tar -C /usr/local -xzf go1.21.3.linux-amd64.tar.gz
     ```
   - Configura las variables de entorno:
     ```bash
     export PATH=$PATH:/usr/local/go/bin
     export GOPATH=$HOME/go
     export PATH=$PATH:$GOPATH/bin
     ```
   - Verifica la instalación:
     ```bash
     go version
     ```

3. **Descarga y compila Singularity:**
   - Descarga Singularity:
     ```bash
     wget https://github.com/sylabs/singularity/releases/download/v3.10.0/singularity-ce-3.10.0.tar.gz
     ```
   - Extrae el archivo:
     ```bash
     tar -xzf singularity-ce-3.10.0.tar.gz
     cd singularity-ce-3.10.0
     ```
   - Configura y compila:
     ```bash
     ./mconfig
     make -C builddir
     sudo make -C builddir install
     ```
4. **Verifica la instalación:**
   ```bash
   singularity --version
   ```
