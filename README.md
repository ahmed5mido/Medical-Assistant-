# Ai Medical Assistant
An AI-powered medical assistant that provides safe, source-grounded health information using Retrieval-Augmented Generation (RAG), Google Gemini, and a dedicated multi-layer safety system. 

⚠️ Disclaimer: For educational purposes only. Not a substitute for professional medical advice, diagnosis, or treatment.

# Overview
Instead of letting an LLM answer medical questions purely from its own internal knowledge (risking hallucination and outdated information), this system:

Retrieves relevant medical context from a trusted dataset (MedQuAD) via semantic vector search.
Generates a response with Google Gemini, strictly grounded in the retrieved sources.
Reviews and filters the response through a safety layer — preventing dosage recommendations, confirmed diagnoses, and unverified claims.
An emergency detection layer short-circuits the pipeline immediately for potentially life-threatening symptoms, bypassing the LLM entirely.

# Architecture

User Input

 Emergency Check : If detected: return static emergency message immediately
    
 RAG Retrieval (FAISS + sentence-transformers)

 Prompt Construction (rules + retrieved sources)

 Gemini Generation + Self-Review (single API call)
   
 Safety Disclaimer & Warning Injection
   
Final Response



# Tech Stack
Python, Google Gemini API, FAISS, sentence-transformers (all-MiniLM-L6-v2), LangChain (chunking), Streamlit, MedQuAD dataset

# Safety Design
Three independent layers, not a single point of control:

Prompt-level constraints — explicit rules against inventing information, confirming diagnoses, or giving dosages.
LLM self-review — Gemini flags its own response for dosage mentions, confirmed-diagnosis language, and unsupported claims, in the same API call as generation.
Rule-based post-processing — deterministic emergency keyword detection and disclaimer injection, independent of the LLM.

#Known Limitations
Embedding model (all-MiniLM-L6-v2) retrieves English queries well but Arabic queries weakly. Planned: multilingual embedding model.
Emergency detection covers formal Arabic but may miss colloquial phrasing.
MedQuAD is English-only; multilingual support relies on the embedding layer bridging the gap at retrieval time.

