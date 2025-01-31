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

---

## **3. Ejemplo práctico en bioinformática**

**Viralrecon** es un pipeline de Nextflow diseñado para el análisis de datos de secuenciación de virus, como SARS-CoV-2. Este pipeline integra múltiples herramientas bioinformáticas para realizar el ensamblaje del genoma viral, la identificación de variantes y la generación de reportes.

**Illumina**

![illumina](https://raw.githubusercontent.com/nf-core/viralrecon/2.6.0//docs/images/nf-core-viralrecon_metro_map_illumina.png)

**Nanopore**

[nanopore](https://raw.githubusercontent.com/nf-core/viralrecon/2.6.0//docs/images/nf-core-viralrecon_metro_map_nanopore.png)

### Decarga del pipeline

```
nextflow run nf-core/viralrecon -profile test,docker --outdir test_ins
```

Comandos típicos para análisis de lecturas provenientes de tecnología Illumina:

```
# Shotgun
nextflow run nf-core/viralrecon \
    --input samplesheet.csv \
    --outdir <OUTDIR> \
    --platform illumina \
    --protocol metagenomic \
    --genome 'MN908947.3' \
    -profile <docker/singularity/podman/conda/institute>
```

```
# amplicón
nextflow run nf-core/viralrecon \
    --input samplesheet.csv \
    --outdir <OUTDIR> \
    --platform illumina \
    --protocol amplicon \
    --genome 'MN908947.3' \
    --primer_set artic \
    --primer_set_version 3 \
    --skip_assembly \
    -profile <docker/singularity/podman/conda/institute>
```

## Nanopor

```
# amplicón
nextflow run nf-core/viralrecon \
    --input samplesheet.csv \
    --outdir <OUTDIR> \
    --platform nanopore \
    --genome 'MN908947.3' \
    --primer_set_version 3 \
    --fastq_dir fastq_pass/ \
    --fast5_dir fast5_pass/ \
    --sequencing_summary sequencing_summary.txt \
    -profile <docker/singularity/podman/conda/institute>
```

Vamos a requerir de un script en Python [fastq_dir_to_samplesheet.py]([wget -L https://raw.githubusercontent.com/nf-core/viralrecon/master/bin/fastq_dir_to_samplesheet.py](https://github.com/nf-core/viralrecon/blob/master/bin/fastq_dir_to_samplesheet.py))


```
wget -L https://raw.githubusercontent.com/nf-core/viralrecon/master/bin/fastq_dir_to_samplesheet.py
./fastq_dir_to_samplesheet.py raw_data samplesheet.csv

```

---

A continuación, te proporciono un ejemplo práctico de cómo usar **Viralrecon** con Nextflow, Docker y Singularity.

### Análisis de datos de SARS-CoV-2 con Viralrecon**

#### **1. Requisitos previos**

- **Nextflow** instalado (ver sección 2.1 del manual).
- **Docker** o **Singularity** instalado (ver secciones 2.2 y 2.3 del manual).
- Datos de secuenciación (archivos FASTQ) de SARS-CoV-2.
- Un genoma de referencia de SARS-CoV-2 (por ejemplo, `NC_045512.2`).

---

#### **2. Configuración del entorno**

##### **2.1. Crear un directorio de trabajo**
1. Crea un directorio para el proyecto:
   ```bash
   conda create -n nf-core 
   conda activate nf-core
   mkdir viralrecon_analysis
   cd viralrecon_analysis
   ```

##### **2.2. Preparar los datos**

1. Coloca tus archivos FASTQ en un directorio llamado `fastq`:
   ```bash
   mkdir fastq
   cp /ruta/a/tus/archivos/*.fastq.gz fastq/
   ```

---

#### **3. Ejecución de Viralrecon**

##### **3.1. Usar Viralrecon con Docker**
1. Ejecuta el pipeline de Viralrecon con Docker:
   ```bash
   nextflow run nf-core/viralrecon \
       --input samplesheet.csv \
       --genome 'MN908947.3' \
       --platform illumina \
       --protocol amplicon \
       -profile docker
   ```

   - **`samplesheet.csv`**: Un archivo CSV que describe las muestras y los archivos FASTQ. Ejemplo:
     ```csv
     sample,fastq_1,fastq_2
     sample1,fastq/sample1_R1.fastq.gz,fastq/sample1_R2.fastq.gz
     sample2,fastq/sample2_R1.fastq.gz,fastq/sample2_R2.fastq.gz
     ```
   - **`--genome 'MN908947.3'`**: Especifica la versión del genoma de referencia.
   - **`--platform illumina`**: Indica la plataforma de secuenciación.
   - **`--protocol amplicon`**: Especifica el protocolo de secuenciación (amplicon, metagenómico, etc.).
   - **`-profile docker`**: Usa Docker para gestionar las dependencias.

2. Los resultados se guardarán en el directorio `results`.

---

##### **3.2. Usar Viralrecon con Singularity**
1. Ejecuta el pipeline de Viralrecon con Singularity:
   ```bash
   nextflow run nf-core/viralrecon \
       --input samplesheet.csv \
       --genome 'MN908947.3' \
       --platform illumina \
       --protocol amplicon \
       -profile singularity
   ```

   - **`-profile singularity`**: Usa Singularity para gestionar las dependencias.

2. Los resultados se guardarán en el directorio `results`.

---

#### **4. Resultados esperados**

El pipeline de Viralrecon generará varios resultados, entre los que se incluyen:

1. **Ensamblaje del genoma viral**:
   - Archivos de consenso en formato FASTA (`results/assembly/consensus/`).
   - Gráficos de cobertura (`results/assembly/coverage/`).

2. **Identificación de variantes**:
   - Archivos VCF con variantes detectadas (`results/variants/ivar/` o `results/variants/bcftools/`).
   - Reportes de variantes en formato HTML (`results/variants/ivar/report/`).

3. **Control de calidad**:
   - Reportes de calidad de las lecturas (`results/qc/fastp/`).
   - Gráficos de calidad (`results/qc/multiqc/`).

4. **Reporte final**:
   - Un reporte HTML resumiendo todos los análisis (`results/multiqc/`).

---

#### **5. Ejemplo de archivo `samplesheet.csv`**

Aquí tienes un ejemplo de cómo estructurar el archivo `samplesheet.csv` para dos muestras:

```csv
sample,fastq_1,fastq_2
sample1,fastq/sample1_R1.fastq.gz,fastq/sample1_R2.fastq.gz
sample2,fastq/sample2_R1.fastq.gz,fastq/sample2_R2.fastq.gz
```

---

#### **6. Conclusión**

**Viralrecon** es una herramienta poderosa para el análisis de datos de secuenciación de virus, como SARS-CoV-2. Al utilizar Nextflow junto con Docker o Singularity, puedes garantizar que el pipeline sea reproducible y escalable en diferentes entornos.

Este ejemplo práctico te permitirá:
- Ensamblar genomas virales.
- Identificar variantes genómicas.
- Generar reportes de calidad y resultados visuales.
