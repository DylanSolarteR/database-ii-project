# Workshop-3 – Concurrency & Distributed Design  
**Google Drive Clone – Databases II**

> *Concurrency Analysis, Parallel & Distributed Database Design*

## 📘 Sections Overview
1. **Project Scope**  
2. **Key Concurrency Scenarios**  
3. **Problem Identification & Solutions**  
4. **Parallel & Distributed Database Design**  
5. **Performance-Improvement Strategies**  
6. **References**  
7. **Authors**

---

## 1. Project Scope
Workshop 3 amplía el Google Drive Clone analizando los **riesgos de concurrencia** y proponiendo una **arquitectura de datos distribuida** capaz de escalar a ≥ 5 000 usuarios concurrentes sin sacrificar integridad ni rendimiento.

Los entregables incluyen:

* Catálogo de condiciones de carrera, interbloqueos e incidencias de aislamiento detectadas en los servicios principales (*File*, *Folder*, *Sharing*, *User*, *Analytics*, etc.).  
* Técnicas de mitigación concretas (bloqueo optimista, orden jerárquico de bloqueos, patrón Saga, indexación basada en colas, etc.).  
* Plano de alto nivel para particionar, replicar y paralelizar los almacenes de datos conforme a la arquitectura hexagonal del proyecto.

---

## 2. Key Concurrency Scenarios

| Dominio | Escenario representativo | Riesgo típico |
|---------|-------------------------|---------------|
| **FileService** | Cargas simultáneas al mismo directorio | Pérdida de incrementos en `used_storage`, nombres duplicados |
| **Folder** | Creación paralela de subcarpetas | Nombres duplicados, `children_count` incorrecto |
| **Sharing / Permissions** | Dos sesiones editan el mismo ACL | Niveles de permiso en conflicto |
| **UserManagement** | Incremento y decremento de cuota concurrentes | Valor de `used_storage` incoherente (< 0 o desfasado) |
| **LogService** | Consulta analítica mientras se insertan logs | Lecturas no repetibles / fantasmas |
| **Analytics** | ETL sobre `Activity_Log` bajo alta escritura | Lecturas sucias, métricas sesgadas |
| **Cross-Service** | Inserción de metadatos exitosa pero fallo en auditoría | Operación distribuida incompleta |

*(El catálogo completo se detalla en el documento PDF anexo).*

---

## 3. Problem Identification & Proposed Solutions

### 3.1 Operaciones de archivo y carpeta
* **Contadores atómicos** – `SET used_storage = used_storage + ?` en una sola primitiva DB.  
* **Claves compuestas únicas** – `(parent_folder_id, name)` elimina duplicados.  
* **Protocolo jerárquico de bloqueos** – bloquear siempre padre → hijos para prevenir interbloqueos.

### 3.2 Compartición y permisos
* **Upsert con campo de versión** – control de concurrencia optimista evita actualizaciones perdidas.  
* **Auditorías Repeatable-Read** – el servicio de logs captura un estado consistente.

### 3.3 Flujos distribuidos
* **Patrón Saga** – coordina metadatos, almacenamiento BLOB y logging mediante eventos; las compensaciones revierten parciales.  
* **Cola de mensajes para indexación** – desacopla escrituras de metadatos de actualizaciones en Elasticsearch, garantizando consistencia eventual.

---

## 4. Parallel & Distributed Database Design

| Capa | Tecnología sugerida | Estrategia de distribución |
|------|--------------------|---------------------------|
| **SQL transaccional** (Users, Subscriptions) | PostgreSQL cluster | Primario + réplicas de lectura por región |
| **NoSQL de metadatos** (Files, Folders) | MongoDB / DynamoDB | Sharding por `tenant_id` |
| **Blob Storage** | Buckets S3-compatibles | Tiers hot/cold conscientes de región |
| **Grafo (Permisos)** | Neo4j | Particionado por comunidad |
| **Series temporales** (Analytics) | InfluxDB / TimescaleDB | Particiones mensuales, comprimidas |

---

## 5. Performance-Improvement Strategies
1. **Escalado horizontal & balanceo de carga** – múltiples pods de API Gateway y servicios distribuyen > 5 000 sesiones concurrentes.  
2. **Sharding** – aísla inquilinos de alto tráfico, reduce hotspots de nodo único.  
3. **Replicación** – descargas de lectura para dashboards y mayor disponibilidad.  
4. **Ejecución paralela de consultas** – trabajos analíticos se dividen por rango de fechas, se procesan en paralelo y luego se consolidan.

> *Trade-off:* mayor complejidad operativa, posible retardo de réplica, elección cuidadosa de la clave de sharding.

---

## 6. References
* Amazon Web Services. *What is Data Architecture?* (2025).  
* Corporate Finance Institute. *Business Model Canvas – Examples* (2025).  
* IBM. *Data Architecture Fundamentals* (2025).  
* UNIR Revista. *Entity-Relationship Model* (2025).  
* Google Workspace. *Google Drive Product Page* (2025).

---

## 7. Authors
* **Cristian David Casas Díaz** – 20192020091  
* **Dylan Alejandro Solarte Rincón** – 20201020088  
* **Professor:** Carlos Andrés Sierra Virguez

---

*README generated for Workshop 3 – Concurrency & Distributed Design (July 2025).*
