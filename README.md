# PDF Parser

A comprehensive PDF parsing solution that extracts and classifies text content while maintaining document structure and footnote relationships.

## Overview

This project implements a multi-method approach to PDF parsing that combines the strengths of different parsing libraries to accurately extract and classify text content. The system specifically handles paragraphs, footnotes, and their references while excluding table and image data.

## Features
- Handles multiple PDF layouts
- Automatically classifies text
- Captures footnote and reference in text
    - Maintains linking between references and footnotes
    - Highlights footnote markers in the processed text
- Excludes table and image data to focus on textual content

## Example output
**Input PDF files**


**Parsed output**


## Approach

**Multi-Method Parsing**: Utilizes three complementary parsing approaches that bypass the limitations of their counterparts:
(1) Unstructured; (2) Docling; (3) PyMuPDF

- **Parsing by Unstructured**: 
  - Advanced document structure recognition
  - Extracts embedded URLs and the link text
*Limitations:* 
    - Misclassifies the parsed text
    - Does not capture font changes

- **Text Classification by Docling**: 
  - Automatically categorizes extracted content
  - Leveraged to rectify the misclassified text by Unstructured

- **Reference extraction by PyMuPDF**: 
  - Font-based analysis for superscript detection
  - Identifies footnote references within text

- **Content Filtering**: Applicable for Unstructured and Docling

## Architecture

The parsing system follows a structured workflow:
<img width="382" height="409" alt="image" src="https://github.com/user-attachments/assets/8950d5d1-c80e-44b1-93b8-b1cc5b52f674" />


## Output Structure

The parser returns a structured object containing the following fields as described in the Pydantic schema:

```python
{
  "properties": {
    "file_name": {
      "description": "File name",
      "title": "File Name",
      "type": "string"
    },
    "page_number": {
      "description": "Page number",
      "title": "Page Number",
      "type": "string"
    },
    "type": {
      "description": "Text type: Footer, section_header, Text, PageBreak, ListItem, NarrativeText, footnote, Title, Header, caption",
      "title": "Type",
      "type": "integer"
    },
    "text": {
      "title": "Text",
      "type": "string"
    },
    "embedded_links": {
      "items": {},
      "title": "Embedded Links",
      "type": "array"
    },
    "reference_list": {
      "description": "List of references mentioned in the text",
      "items": {},
      "title": "Reference List",
      "type": "array"
    }
  },
  "required": [
    "file_name",
    "page_number",
    "type",
    "reference_list"
  ],
  "title": "ParsedPDF",
  "type": "object"
}
```

## Acknowledgments

- Built using Unstructured for document analysis
- Docling for sentence classification
- PyMuPDF for low-level PDF processing
- Thanks to the open-source community for the foundational libraries

## Roadmap

- Support for additional document formats
- Enhanced table detection and exclusion
- Improved footnote reference matching
- Batch processing capabilities
- API endpoint development
- Performance optimization

