# Workshop-1 – Google Drive Clone

This repository contains the deliverables for **Workshop 1** of the course project, which outlines the initial design and business model for a cloud-based file management platform inspired by Google Drive.

## 📘 Sections Overview

### 1. Project Overview
This section includes a general introduction to the project, highlighting the goal of developing a secure, scalable, and user-friendly Google Drive clone. The solution focuses on features such as file sharing, synchronization, mobile access, and fine-grained permissions.

### 2. User Stories
A detailed list of 20+ user stories is provided, each with priority, time estimate, and acceptance criteria. Key functionalities include:
- Secure file upload
- Folder organization
- File sharing with permissions
- Drag-and-drop upload
- File synchronization
- Mobile access
- Activity logs
- Two-factor authentication
- Analytics dashboard

### 3. Business Model Canvas
An adapted Business Model Canvas identifies the project's value propositions, key partners, customer relationships, cost structure, revenue streams, and core resources.  
📎 View the diagram: [Business Model Canva](https://www.canva.com/design/DAGiAAyqO_M/fNvX-0e8GAlQERt1i3x6VA/view?utm_content=DAGiAAyqO_M&utm_campaign=designshare&utm_medium=link&utm_source=viewer)

### 4. Functional Requirements
Specifies all platform capabilities grouped in categories such as:
- User Management
- File Management
- Folder Organization
- Content Search
- Sharing and Collaboration
- Mobile Support
- Plans and Quotas
- File Security
- Integration with external services
- Analytics and Reporting

### 5. Non-Functional Requirements
Describes system-level characteristics, including:
- Performance and responsiveness
- Scalability (up to 5,000 concurrent users)
- Security standards (AES encryption, brute-force protection)
- Usability and responsiveness
- Compatibility with modern browsers
- Maintainability and modular architecture
- Internationalization (English and Spanish support)

### 6. Initial Architecture
A modular, hexagonal architecture is proposed, composed of:
- **API Gateway**
- **Domain Layer** (FileService, SharingService, etc.)
- **Adapters Layer** (MongoDB, S3, Redis, Elasticsearch, Neo4j)
- **Analytics & Monitoring** (InfluxDB, Prometheus, SendGrid)
- **External Integrations** (Auth0, cloud storage, email)
📎 Architecture Diagram: [Initial Architecture](https://drive.google.com/file/d/1DLr7pwfCojS48Jxpm57vCV-NHuYOqhi6/view?usp=sharing)

### 7. Entity-Relationship (ER) Diagram
Defines the key entities and relationships in the system:
- USER, FILE, FOLDER, TAG, ACTIVITY_LOG, SHARING, SHARED_LINK, etc.
- Includes support for versioning, favorites, tagging, and permission models.
📎 ER Diagram: [ER Model](https://drive.google.com/file/d/1MD3f5MO3aawOB-vF6Bol_tBEoIcMpYv5/view?usp=sharing)

### 8. Data Flow and Storage Design
- Explains separation of metadata and binary content.
- Describes use of hot vs. cold storage strategies.
- Covers chunked uploads, caching, encryption, and deduplication.
- Details security, analytics, and monitoring pipelines.

---

## 📚 References

- Amazon Web Services. (n.d.). *What is data architecture?* AWS. Retrieved May 13, 2025, from https://aws.amazon.com/es/what-is/data-architecture/

- Business model canvas examples. (n.d.). *Corporate Finance Institute*. Retrieved March 17, 2025, from https://corporatefinanceinstitute.com/resources/management/business-model-canvas-examples/

- IBM. (n.d.). *What is data architecture?* IBM. Retrieved May 13, 2025, from https://www.ibm.com/es-es/topics/data-architecture

- UNIR. (n.d.). *Entity-relationship model*. UNIR Revista. Retrieved May 13, 2025, from https://www.unir.net/revista/ingenieria/modelo-entidad-relacion/

- Workspace, G. (n.d.). *Google Drive: Share files online with secure cloud storage*. Google Workspace. Retrieved March 17, 2025, from https://workspace.google.com/intl/es-419/products/drive/

---

### 👥 Authors

- Cristian David Casas Díaz – 20192020091  
- Dylan Alejandro Solarte Rincón – 20201020088  
Professor: Carlos Andrés Sierra Virguez

---

> This document serves as a reference for project planning and initial implementation of the Google Drive Clone application.
