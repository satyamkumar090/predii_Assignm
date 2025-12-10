This project implements a Retrieval-Augmented Generation (RAG) system designed to extract structured vehicle specifications such as torque values, part numbers, and component details from an automotive Service Manual PDF.

The goal was to build a text-based RAG pipeline capable of:

  1) Extracting text from a large PDF

  2) Chunking & embedding content

  3) Performing vector retrieval

  4) Using an LLM to answer specification queries

  5) Returning clean structured JSON output

  6)This README explains the system design, tools used, and ideas for improvement.

## **Project Overview**

Automotive service manuals contain:

Long procedural instructions

Tables with torque specs

Part number lists

Images with overlaid labels

Some pages without selectable text

Thus, many specifications—especially Torque Specifications—are difficult to extract directly using plain PDF text extraction.

To solve this, a hybrid pipeline was implemented using:

PyMuPDF for PDF parsing

Sentence Transformers for embeddings

FAISS for similarity search

Gemini text model for specification extraction

OCR was attempted for image-only pages but did not fully succeed due to thin table lines, noisy backgrounds, and inconsistent resolution.
