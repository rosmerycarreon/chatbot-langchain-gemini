# 🤖 Proyecto Chatbot LangChain

Un proyecto completo de chatbot conversacional con soporte para procesamiento de documentos PDF usando LangChain, Hugging Face y Gradio.

**Tabla de contenidos**
- [Características](#características)
- [Requisitos previos](#requisitos-previos)
- [Instalación](#instalación)
- [Configuración](#configuración)
- [Uso](#uso)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Componentes principales](#componentes-principales)
- [Solución de problemas](#solución-de-problemas)
- [Licencia](#licencia)

---

## 🎯 Características

✅ **Conversación natural** - Chatbot conversacional con capacidades de IA  
✅ **Procesamiento de PDF** - Carga y analiza documentos PDF  
✅ **Búsqueda semántica** - Encuentra respuestas relevantes en tus documentos  
✅ **RAG (Retrieval Augmented Generation)** - Combina búsqueda de documentos con generación de texto  
✅ **Interfaz web** - UI moderna con Gradio  
✅ **API REST** - Endpoints FastAPI para integración  
✅ **Tokens de Hugging Face** - Soporte para modelos remotos  

---

## 📋 Requisitos previos

- **Python 3.10+** (recomendado 3.12)
- **pip** o **conda** para gestión de paquetes
- **Token de Hugging Face** (opcional, pero recomendado para embeddings)
- **Conexión a internet** para descargar modelos

---

## 🚀 Instalación

### 1. Clonar o descargar el repositorio

```bash
cd c:\Users\ROSS\PYTHON\Proyecto\ chatbot\ LangChain
```

### 2. Crear un entorno virtual (recomendado)

```powershell
# Windows PowerShell
python -m venv venv
.\venv\Scripts\Activate.ps1

# O en cmd
python -m venv venv
venv\Scripts\activate.bat
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

Para la aplicación Gradio en `michatbotRGB/`:

```bash
pip install gradio sentence-transformers langchain-huggingface
```

---

## ⚙️ Configuración

### Configurar token de Hugging Face

**Opción 1: Variable de entorno**

```powershell
# Windows PowerShell
$env:HF_TOKEN="hf_YOUR_TOKEN_HERE"

# O agrégalo a tu archivo .env
HF_TOKEN=hf_YOUR_TOKEN_HERE
HF_API_TOKEN=hf_YOUR_TOKEN_HERE
```

**Opción 2: Crear archivo `.env`**

Crea un archivo `.env` en la raíz del proyecto:

```env
HF_TOKEN=hf_YOUR_TOKEN_HERE
HF_API_TOKEN=hf_YOUR_TOKEN_HERE
HUGGINGFACE_HUB_TOKEN=hf_YOUR_TOKEN_HERE
GRADIO_SERVER_PORT=7860
```

> 📌 **Obtén tu token en**: https://huggingface.co/settings/tokens

### Obtener token de Hugging Face

1. Crea una cuenta en [huggingface.co](https://huggingface.co/)
2. Ve a [Settings → Access Tokens](https://huggingface.co/settings/tokens)
3. Crea un nuevo token (el token debe tener permiso de lectura)
4. Cópialo y úsalo en tu configuración

---

## 📖 Uso

### 1. Ejecutar la aplicación Gradio (recomendado)

```powershell
$env:HF_TOKEN="tu_token_aqui"
.\venv\Scripts\python.exe michatbotRGB\app.py
```

La aplicación estará disponible en: **http://127.0.0.1:7860**

#### Características de la interfaz

- **Sube tu PDF**: Haz clic en el área de carga para seleccionar un documento
- **Procesar PDF**: Extrae texto, crea embeddings y construye la base de conocimiento
- **Chat**: Escribe tu pregunta y obtén respuestas basadas en el PDF (si está cargado) o conversación general

### 2. Ejecutar con FastAPI (backend)

```powershell
$env:HF_TOKEN="tu_token_aqui"
.\venv\Scripts\python.exe app.py
```

API disponible en: **http://127.0.0.1:8000**

#### Endpoints disponibles

- `POST /chat` - Envía un mensaje y obtén respuesta
- `POST /upload-pdf` - Sube un PDF para procesamiento
- `GET /health` - Verifica el estado de la API

---

## 📁 Estructura del proyecto

```
Proyecto chatbot LangChain/
│
├── app.py                          # API FastAPI principal
├── requirements.txt                # Dependencias Python
├── .env                            # Variables de entorno (no incluido en git)
├── README.md                       # Este archivo
│
├── michatbotRGB/                   # Aplicación Gradio
│   ├── app.py                      # Interfaz Gradio del chatbot
│   ├── requirements.txt            # Dependencias específicas
│   └── README.md                   # Documentación de Gradio
│
├── templates/                      # Plantillas HTML (si aplica)
│   └── index.html
│
├── uploads/                        # PDFs cargados temporalmente
│   └── [archivos_temporales]
│
├── pdf_base/                       # Base vectorial Chroma (se crea al procesar PDFs)
│   ├── chroma.sqlite3
│   └── data/
│
└── venv312/                        # Entorno virtual Python
    ├── Lib/
    ├── Scripts/
    └── pyvenv.cfg
```

---

## 🔧 Componentes principales

### 1. **Chatbot Gradio** (`michatbotRGB/app.py`)

Interface web moderna para interacción con el chatbot.

**Características:**
- Subida de archivos PDF
- Procesamiento con `PyPDF2` y `RecursiveCharacterTextSplitter`
- Embeddings con `HuggingFaceEmbeddings` (all-MiniLM-L6-v2)
- Base vectorial `Chroma`
- Chat con `InferenceClient` de Hugging Face

**Flujo:**
1. Usuario sube PDF → se extrae texto
2. Texto se divide en fragmentos → se crean embeddings
3. Fragmentos se almacenan en `Chroma`
4. Preguntas buscan fragmentos relevantes
5. Respuesta se genera usando el contexto + historial

### 2. **API FastAPI** (`app.py`)

Backend REST para integración con otros servicios.

**Responsabilidades:**
- Manejo de rutas HTTP
- Procesamiento de consultas
- Gestión de sesiones
- Integración con modelos de LangChain

### 3. **Modelos y librerías utilizadas**

| Componente | Librería | Propósito |
|-----------|----------|-----------|
| **Embeddings** | `sentence-transformers/all-MiniLM-L6-v2` | Convertir texto a vectores |
| **Vector Store** | `Chroma` | Almacenar y recuperar embeddings |
| **Chat** | `huggingface_hub.InferenceClient` | Generar respuestas |
| **Splits de texto** | `RecursiveCharacterTextSplitter` | Dividir documentos en fragmentos |
| **PDF** | `PyPDF2` | Leer archivos PDF |
| **UI** | `Gradio` | Interfaz web |
| **API** | `FastAPI` | Backend REST |

---

## 🐛 Solución de problemas

### ❌ Error: `401 Unauthorized` al descargar embeddings

**Causa**: Token de Hugging Face inválido, expirado o sin permisos.

**Solución:**
```powershell
# 1. Verifica tu token en https://huggingface.co/settings/tokens
# 2. Crea uno nuevo si es necesario
# 3. Establécelo en la sesión actual

$env:HF_TOKEN="tu_nuevo_token"
.\venv\Scripts\python.exe michatbotRGB\app.py
```

### ❌ Error: `Port 7860 already in use`

**Causa**: El puerto está siendo usado por otro proceso.

**Solución:**
```powershell
# Opción 1: Cambiar puerto en .env
$env:GRADIO_SERVER_PORT=7861

# Opción 2: Matar el proceso en el puerto
Get-Process | Where-Object {$_.Listening -eq $true} | Where-Object {$_.LocalPort -eq 7860} | Stop-Process
```

### ❌ Error: `sentence-transformers` no se puede importar

**Causa**: Librería no instalada.

**Solución:**
```bash
pip install sentence-transformers
# o especificar la versión
pip install "sentence-transformers>=2.2.0"
```

### ❌ Error: `Data incompatible with messages format` en Gradio

**Causa**: El historial de chat no está en el formato correcto.

**Solución**: Verificar que la función `respond()` devuelve:
```python
[
    {"role": "user", "content": "..."},
    {"role": "assistant", "content": "..."}
]
```

### ❌ Error: PDF no se carga o está vacío

**Causa:** El PDF puede estar protegido o no contener texto extraíble.

**Solución:**
- Verifica que el PDF no esté protegido
- Intenta con otro PDF
- Revisa el log para mensajes de error específicos

---

## 📦 Dependencias principales

```
fastapi             # Framework REST
uvicorn             # Servidor ASGI
gradio              # UI web
langchain           # Framework NLP
langchain-core      # Core de LangChain
langchain-community # Integraciones comunitarias
langchain-text-splitters  # División de texto
langchain-huggingface     # Integraciones HF
sentence-transformers     # Embeddings locales
chromadb            # Base vectorial
PyPDF2              # Lectura de PDFs
huggingface_hub     # Cliente HF Inference
python-dotenv       # Manejo de .env
```

Ver `requirements.txt` para la lista completa con versiones.

---

## 🎓 Arquitectura RAG (Retrieval Augmented Generation)

```
PDF Documento
    ↓
[Lectura con PyPDF2]
    ↓
Texto completo
    ↓
[RecursiveCharacterTextSplitter]
    ↓
Fragmentos (chunks)
    ↓
[HuggingFaceEmbeddings]
    ↓
Vectores de embeddings
    ↓
[Chroma Vector Store]
    ↓
Base de conocimiento persistente
    ↑
    │
Pregunta del usuario
    ↓
[Similarity Search en Chroma]
    ↓
Fragmentos relevantes
    ↓
[Contexto + Historial de chat]
    ↓
[InferenceClient Hugging Face]
    ↓
Respuesta generada
```

---

## 🚀 Mejoras futuras

- [ ] Soporte para más formatos (DOCX, TXT, markdown)
- [ ] Múltiples PDFs simultáneamente
- [ ] Diferentes modelos de chat (GPT, Claude, etc.)
- [ ] Base de datos persistente para historial
- [ ] Autenticación de usuarios
- [ ] Deploy en Hugging Face Spaces
- [ ] Caché de embeddings
- [ ] Análisis de sentimientos
- [ ] Feedback del usuario para mejorar respuestas

---

## 📞 Soporte

Para reportar problemas o sugerir mejoras, consulta:
- El archivo de logs de la aplicación
- La documentación oficial de [LangChain](https://python.langchain.com/)
- La documentación de [Gradio](https://www.gradio.app/)
- La documentación de [Hugging Face](https://huggingface.co/docs)

---

## 📄 Licencia

Este proyecto es de uso libre para fines educativos y personales.

---

**Última actualización**: Mayo 2026  
**Versión**: 1.0.0  
**Autor**: Tu nombre/Equipo

