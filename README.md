# Hybrid RAG Engine ⚡

Motor de Retrieval-Augmented Generation (RAG) híbrido con re-ranking semántico, observabilidad en tiempo real y suite de evaluación automatizada para documentación técnica.

---

## 📌 ¿Qué hace el proyecto?

- **Ingesta estructurada:** Carga, limpia y divide documentos técnicos (Markdown/PDFs) preservando contexto jerárquico.
- **Búsqueda híbrida:** Combina búsqueda vectorial densa (embeddings) con filtrado y matching léxico dentro de **Qdrant**.
- **Re-ranking semántico:** Refina los $N$ documentos recuperados usando un modelo de re-ranking (Cross-Encoder / Cohere) antes de armar el prompt final.
- **Generación fundamentada:** Responde consultas complejas citando fuentes exactas y reduciendo alucinaciones mediante Gemini / LLMs modernos.
- **Evaluación y observabilidad:** Mide latencia, consumo de tokens y métricas de *faithfulness* / *answer relevancy* integrando **Langfuse** y tests automatizados.

---

## 🛠️ Stack y Herramientas

| Componente | Tecnología |
|---|---|
| **Lenguaje y entorno** | Python 3.12+, `uv` (Fast package manager) |
| **API Backend** | FastAPI, Pydantic Settings |
| **Vector Database** | Qdrant (desplegado localmente con Docker Compose) |
| **Modelos & LLMs** | Google Gemini API (`google-genai`), `sentence-transformers` |
| **Observabilidad** | Langfuse |

---

## 🧠 Conceptos clave que cubre

1. **RAG Híbrido vs. Vanilla RAG:** Por qué la búsqueda vectorial pura no alcanza y cómo combinarla con re-ranking semántico.
2. **Estrategias de Chunking:** Fragmentación adaptada al tipo de archivo para no cortar código ni definiciones críticas.
3. **LLMOps y Trazabilidad:** Monitoreo de llamadas, cadenas y costos en producción con Langfuse.
4. **Evals sintéticos:** Medición continua de calidad para evitar regresiones antes de desplegar cambios de prompts o modelos.

---

## 🚀 Inicio Rápido

### Prerrequisitos
- Docker Desktop
- `uv` (`curl -LsSf https://astral.sh/uv/install.sh | sh`)

### Instalación

```bash
# 1. Clonar el repositorio
git clone [https://github.com/diegodazpeitia/hybrid-rag-engine.git](https://github.com/diegodazpeitia/hybrid-rag-engine.git)
cd hybrid-rag-engine

# 2. Levantar la base de datos vectorial
docker compose up -d

# 3. Instalar dependencias
uv sync

# 4. Configurar variables de entorno
cp .env.example .env # Completar con GEMINI_API_KEY y credenciales de Langfuse

# 5. Ejecutar servidor
uv run uvicorn app.main:app --reload