# 🛡️ Implementación de Arquitectura de Balanceo con Recuperación de Visibilidad IP (SNAT)

Este repositorio contiene la infraestructura técnica desarrollada para mi Trabajo de Fin de Grado:

> **"Implementación de una arquitectura de balanceo de carga con recuperación de visibilidad IP tras SNAT mediante técnicas de validación activa y Zero Trust"**

---

## 📖 Resumen del Proyecto

El objetivo principal de este proyecto es resolver el problema conocido como **"ceguera forense"** en infraestructuras que utilizan **Source NAT (SNAT)**.

En configuraciones tradicionales:
- Los servidores finales **pierden la IP real del cliente/atacante**
- Se dificulta la **trazabilidad**, auditoría y respuesta ante incidentes

### 💡 Solución propuesta

Se diseña una arquitectura basada en:
- Recuperación de IP real a nivel L7  
- Validación activa entre nodos  
- Modelo Zero Trust  
- Respuesta dinámica ante amenazas  

---

## 🏗️ Arquitectura del Sistema

La infraestructura se organiza en **dos escenarios evolutivos**:

---

### 🔬 Escenario A: Diagnóstico del Problema

**Objetivo:** demostrar la pérdida de visibilidad causada por SNAT

**Componentes:**
- **Balanceador (LB1)**
  - HAProxy en **Capa 4 (L4)**

- **SNAT con iptables**
  - Pool de IPs virtuales: `.100 - .102`
  - Simulación de múltiples orígenes

**Resultado:**
- El servidor final **solo ve la IP del balanceador**
- Se pierde la IP real del cliente

---

### 🚀 Escenario B: Solución Avanzada (Validación Activa)

**Objetivo:** recuperar trazabilidad y aplicar seguridad Zero Trust

#### 🔍 Trazabilidad en Capa 7
- Proxy Protocol v2  
- Cabeceras `X-Forwarded-For`  

Permiten transportar la IP real a través de la infraestructura.

---

#### 🛡️ WAF (Web Application Firewall)

- Nginx + ModSecurity  
- Reglas **OWASP CRS**

**Características clave:**
- Validación de tráfico HTTP  
- Configuración `map` personalizada  
- Resolución de colisiones de ACL  
- Verificación de IPs de confianza  

---

#### 🧠 Nodo de Decisión (LB2)

- Segundo balanceador (HAProxy)

**Funciones:**
- Validar **firma secreta** (`X-Token-Seguro`)  
- Detectar anomalías:
  - HTTP Request Smuggling  
  - Tráfico manipulado  

---

#### 🎯 Respuesta Dinámica

En lugar de bloquear el tráfico sospechoso:

➡️ Se redirige automáticamente a un **honeypot**

- Sistema utilizado: **T-Pot**
- Permite:
  - Observación del atacante  
  - Recolección de inteligencia  

---

## 🛠️ Tecnologías Utilizadas

| Componente        | Tecnología              | Rol |
|------------------|------------------------|-----|
| Virtualización   | VirtualBox             | Hipervisor del laboratorio |
| Balanceo         | HAProxy                | Gestión de tráfico y validación |
| WAF              | Nginx + ModSecurity    | Inspección L7 + OWASP CRS |
| Decepción        | T-Pot (Docker)         | Ecosistema de honeypots |
| Análisis         | ELK Stack (Kibana)     | SIEM y análisis forense |
| Aplicación       | DVWA                   | Servidor vulnerable de pruebas |

---

## 🔐 Características de Seguridad

- ✔️ Recuperación de IP real tras SNAT  
- ✔️ Validación de confianza entre componentes  
- ✔️ Inspección profunda de tráfico (L7)  
- ✔️ Detección de ataques avanzados  
- ✔️ Integración con honeypots  
- ✔️ Arquitectura basada en Zero Trust  

---

## 📊 Casos de Uso

- Laboratorios de ciberseguridad  
- Investigación forense de red  
- Simulación de ataques reales  
- Testing de WAF y balanceadores  
- Entornos académicos (TFG/TFM)  

---

## 🚧 Estado del Proyecto

🟢 En desarrollo / finalizado  

---

## 📌 Autor

- Nombre: *[Tu nombre]*  
- Grado: *[Tu grado]*  
- Universidad: *[Tu universidad]*  

---

## 📜 Licencia

Este proyecto se distribuye bajo la licencia *[MIT / GPL / otra]*  
