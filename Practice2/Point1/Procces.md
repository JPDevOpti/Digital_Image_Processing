# 🧠 Conversión de DICOM a BIDS con BIDScoin

Este proyecto documenta el proceso para convertir imágenes médicas en formato **DICOM** a la estructura estándar **BIDS (Brain Imaging Data Structure)** utilizando **BIDScoin**.

---

## 📁 Estructura inicial de los datos

Los datos originales están organizados así:
├── raw/
│ ├── 0001/
│ ├── 0002/
│ ├── 0003/
│ └── 0004/
├── bids/ ← Aquí se almacenará la salida en formato BIDS

Cambiamos los nombres de los archivos para evitar errores y poner formato sub-xxx
├── raw/
│ ├── sub-001/
│ ├── sub-002/
│ ├── sub-003/
│ └── sub-004/
├── bids/ ← Aquí se almacenará la salida en formato BIDS

---

## 🛠️ Paso 1: Crear el mapa BIDS

Ejecutamos `bidsmapper` para escanear los datos DICOM y generar un mapeo automático inicial.

``` bash
bidsmapper raw bids
```
Esto genera una carpeta bids/bidsmap/ con el archivo bidsmap.yaml.

![Estructura inicial](/Practice2/Point1/Plots/3.png) 
---

## 🖼️ Paso 2: Editar el archivo de mapeo

Usamos el editor gráfico de BIDScoin para revisar y corregir la clasificación de cada imagen:

``` bash
bidseditor
```
![Estructura inicial](/Practice2/Point1/Plots/4.png) 

| #   | BIDS output path                            | BIDS `datatype` | BIDS `suffix` |
| --- | ------------------------------------------- | --------------- | ------------- |
| 001 | `sub-001_mod-SAG3DT1ACCELERATED.*`          | `anat`          | `T1w`         |
| 002 | `sub-001_mod-FLAIRSAG3D.*`                  | `anat`          | `FLAIR`       |
| 003 | `sub-003_acq-HEADEC2_mod-SAGT1_echo-1_SE.*` | `anat`          | `T1w`         |
| 004 | `sub-004_mod-AXT2.*`                        | `anat`          | `T2w`         |
| 005 | `sub-005_mod-T2COR.*`                       | `anat`          | `T2w`         |

---

## ⚙️ Paso 3: Ejecutar la conversión

Con el mapeo definido, ejecutamos el proceso de conversión:

``` bash
bidscoiner raw bids
```

![Estructura inicial](/Practice2/Point1/Plots/5.png) 

---

## 📂 Estructura BIDS generada

Obtenemos finalmente los archivos en estrcutura bids con los nombres correspondientes.

bids/
├── dataset_description.json
├── participants.tsv
├── sub-001/
│   └── anat/
│       └── sub-001_T1w.nii.gz
│       └── sub-001_T1w.json
│       └── sub-001_FLAIR.nii.gz
│       └── sub-001_FLAIR.json
├── sub-003/
│   └── anat/
│       └── sub-003_T1w.nii.gz
...
![Estructura inicial](/Practice2/Point1/Plots/6.png) 

---
## ✅ Paso 4: Validar con bids-validator

Se utiliza la aplicación web de bids validator

![Estructura inicial](/Practice2/Point1/Plots/7.png) 

Se encontraron algunos "warings" pero estos no son problemas graves y estan enfocados a sugerencias, por tanto se puede decir que la validación ha sido exitosa

---