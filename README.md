This project implements a Retrieval-Augmented Generation (RAG) system designed to extract structured vehicle specifications such as torque values, part numbers, and component details from an automotive Service Manual PDF.

The goal was to build a text-based RAG pipeline capable of:

  1) Extracting text from a large PDF

  2) Chunking & embedding content

  3) Performing vector retrieval

  4) Using an LLM to answer specification queries

  5) Returning clean structured JSON output

  6) This README explains the system design, tools used, and ideas for improvement.

## **1) Project Overview**

Automotive service manuals contain:

   1) Long procedural instructions

   2) Tables with torque specs

   3) Part number lists

   4) Images with overlaid labels

   5) Some pages without selectable text

Thus, many specifications—especially Torque Specifications—are difficult to extract directly using plain PDF text extraction.

To solve this, a hybrid pipeline was implemented using:

   1) PyMuPDF for PDF parsing

   2) Sentence Transformers for embeddings
    
   3) FAISS for similarity search

   4) Gemini text model for specification extraction
   
  


**1) OCR was attempted for image-only pages but did not fully succeed due to thin table lines, noisy backgrounds.I have implemented the OCR in the notebook.Needed a bit of croppng on the tables to identify lb-ft,Nm,lb-in.
  2)Torque tables inside images do not appear in extracted text, so an LLM cannot see them unless OCR or a Vision model is applied
  3) Some of the part number and torque tables were in html document format which was difficult to extract**

## **2. System Architecture**

The RAG pipeline contains four main stages:

  2.1 PDF Text Extraction 

        1) Using PyMuPDF (fitz)

        2) Each page is read and converted to raw text

        3) Some pages contain only partial text because torque tables are actually embedded images

        4) A fallback OCR method using pytesseract was attempted but accuracy was low due to:
              a)Thin table lines
              c)Inconsistent font rendering
              c)Multi-column layouts

  2.2 Text Chunking 

      1) Manual text is split into Maximum 250 words per chunk

      2) Table rows remain grouped where possible

      3) This improves contextual retrieval during querying

  2.3 Embeddings & Vector Store

    1) SentenceTransformer: all-MiniLM-L6-v2

    2) FAISS IndexFlatIP for similarity search

 Embeddings allow retrieval even when the query wording differs from the manual text. 

 2.4 Retrieval-Augmented LLM Query

  The RAG query flow:

   1) User → “Torque for brake caliper bolts”

   2 )Embed query and retrieve top-k relevant chunks

   3) Send retrieved chunks + system prompt to Gemini text model

   4) LLM outputs structured JSON


| Purpose            Tool                   |
| PDF Parsing     | PyMuPDF (fitz)          |
| OCR (attempted) | pytesseract             |
| Embeddings      | Sentence-Transformers   |
| Vector Search   | FAISS                   |
| LLM             | Gemini 2.5 Flash (text) |
| Environment     | Google Colab            |



   
## **2) Future Improvements**
 
6.1 Integrate Gemini Vision (Important Upgrade) 

    Many torque tables exist ONLY as images.
    Gemini Vision can:

      1 )Detect tables visually

      2) Extract rows and numerical columns

      3) Preserve structure

      4) Improve accuracy over OCR

      5) This would completely solve the problem of thin-line tables.


     6.2 Use Deep Table Extractors 

       Examples:
        1) Google DocAI
        2) LayoutLMv3

     6.3 Using Heirarchial Chunking on basis of section and their heading which appears on evry page and nearly 40-50 pages have same headings.
      
       

