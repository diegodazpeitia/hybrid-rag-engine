# 🧠 Conceptos y Fundamentos Técnicos: Hybrid RAG Engine

Este documento recopila las bases teóricas, conceptos de Inteligencia Artificial y decisiones arquitectónicas aplicadas en este proyecto.

---

## 1. Fundamentos de IA y LLMs

* **RAG (Retrieval-Augmented Generation):** Patrón arquitectónico que resuelve dos debilidades críticas de los LLMs: la falta de conocimiento privado/reciente y las alucinaciones. Consiste en recuperar fragmentos de información relevantes desde una base de datos externa e inyectarlos dinámicamente en el contexto del prompt antes de generar la respuesta.
* **Embeddings:** Representaciones vectoriales densas de texto en un espacio multidimensional ($n$ dimensiones). Los textos con proximidad semántica tienen distancias matemáticas reducidas (medidas típicamente por similitud coseno o producto escalar), permitiendo buscar por significado en lugar de coincidencia exacta de palabras.
* **Chunking Semántico:** Proceso de fragmentación de documentos largos en bloques coherentes de tamaño controlado. Previene la saturación de la ventana de contexto del LLM y asegura que los vectores contengan una densidad de información uniforme.
* **Búsqueda Híbrida (Hybrid Search):** Combinación en paralelo de dos paradigmas de recuperación:
  1. *Búsqueda léxica (Sparse/BM25):* Basada en frecuencia de términos exactos (nombres de funciones, errores, IDs).
  2. *Búsqueda densa (Dense/Embeddings):* Basada en significado contextual y sinónimos.
* **Re-ranking Semántico:** Técnica que utiliza modelos Cross-Encoder (como Cohere Rerank o BGE-Reranker) para revaluar los mejores $N$ resultados devueltos por la búsqueda híbrida, reordenándolos según su relevancia real respecto a la pregunta antes de pasárselos al modelo generador.

---

## 2. LLMOps y Calidad

* **Observabilidad de LLMs:** Monitoreo y trazabilidad de cada interacción del pipeline (latencia por etapa, consumo de tokens de entrada/salida, costo monetario y registro de inputs/outputs para auditoría).
* **Evals (Evaluación Automatizada):** Frameworks de métricas que usan modelos de evaluación para medir sistemáticamente:
  * *Faithfulness (Fidelidad):* Si la respuesta se fundamenta exclusivamente en el contexto recuperado (evita alucinaciones).
  * *Answer Relevance (Relevancia):* Si la respuesta atiende directamente la duda planteada por el usuario.
  * *Context Precision:* Si los chunks recuperados contienen la información necesaria sin ruido extra.

---

## 3. Decisiones Arquitectónicas del Stack

* **Qdrant (Vector Database):** Motor de base de datos vectorial de alto rendimiento escrito en Rust. Se seleccionó por su capacidad nativa de búsqueda híbrida, filtrado de metadatos (payloads) en tiempo real y facilidad de despliegue mediante contenedores.
* **Google Gemini API (`google-genai`):** LLM utilizado para razonamiento y generación final, aprovechando ventanas de contexto amplias y soporte para salidas estructuradas (JSON Schema / Pydantic).
* **Sentence-Transformers:** Modelos de embeddings compactos y eficientes para transformar texto a vectores localmente o vía inferencia optimizada.
* **Langfuse:** Plataforma de observabilidad open-source para registrar trazas completas de las cadenas RAG.
* **FastAPI & uv:** Backend asíncrono con tipado estricto combinado con el gestor de paquetes en Rust más veloz del ecosistema actual de Python.
