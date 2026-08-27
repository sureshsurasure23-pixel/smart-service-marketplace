# Smart Service Marketplace — System Architecture

## 1. Architecture Overview

The Smart Service Marketplace follows a client-server architecture.

The React frontend communicates with the Django REST API backend using HTTP/HTTPS requests.

The Django backend handles business logic, authentication, authorization, validation, and database operations.

MySQL stores application data.

The optional AI module provides service recommendations.

---

## 2. High-Level Architecture

```text
+----------------------+
|      CUSTOMER        |
+----------+-----------+
           |
           v
+----------------------+
|    React Frontend    |
+----------+-----------+
           |
           | REST API
           v
+----------------------+
| Django REST Backend  |
+----------+-----------+
           |
     +-----+-----+
     |           |
     v           v
+---------+  +-----------+
|  MySQL  |  | AI Module |
|Database |  | Python ML |
+---------+  +-----------+