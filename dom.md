
***
title: dom
date: 2020-04-29 16:36:29+01:00
author: Gaxpert
***


# DOM
## Node Object Types
* DOCUMENT_NODE (e.g., window.document)
* ELEMENT_NODE (e.g., <body>, <a>, <p>, <script>, <style>, <html>, <h1>)
* ATTRIBUTE_NODE (e.g., class="funEdges")
* TEXT_NODE (e.g., text characters in an HTML document including carriage returns
and whitespace)
* DOCUMENT_FRAGMENT_NODE (e.g., document.createDocumentFragment())
* DOCUMENT_TYPE_NODE (e.g., <!DOCTYPE html>)

## Node interfaces/constructors
|Interface/constructor | nodeType (returned from .nodeType)|
|----------------------|-------------------------------|
|HTML*Element [e.g., HTMLBodyElement]| 1 (i.e., ELEMENT_NODE)|
|Text | 3 (i.e., TEXT_NODE)|
|Attr | 2 (i.e., ATTRIBUTE_NODE)|
|HTMLDocument | 9 (i.e., DOCUMENT_NODE)|
|DocumentFragment | 11 (i.e., DOCUMENT_FRAGMENT_NODE)|
|DocumentType | 10 (i.e., DOCUMENT_TYPE_NODE)|



