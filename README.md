# Azure Zero-Trust Cloud Infrastructure Deployment

## Descripción General
Documentación técnica del diseño, segmentación y aplicación de controles de seguridad sobre una infraestructura en Microsoft Azure, aplicando el modelo **Zero Trust** ("Nunca confiar, siempre verificar") y el Principio de Menor Privilegio (PoLP).

---

## Arquitectura del Entorno

* **Grupo de Recursos:** `RG-Seguridad-Proyecto` (Región: East US)
* **Red Virtual (VNet):** `VNet-Principal` (`10.0.0.0/16`) con segmentación de subredes aisladas.
* **Cuenta de Almacenamiento:** `stseguridadjh2026` (TLS 1.2 forzado, acceso público deshabilitado).
* **Bóveda de Claves:** `kv-seguridad-jh2026` (Custodia criptográfica de secretos con Soft-Delete habilitado).

---

## Paso a Paso de la Implementación

### 1. Gobernanza y Grupo de Recursos
Creación del grupo de recursos `RG-Seguridad-Proyecto` en la región `East US` como contenedor lógico principal para la gestión de recursos y etiquetado de control de costos.

### 2. Arquitectura de Red y Segmentación (VNet)
Despliegue de la red virtual `VNet-Principal` con espacio de direcciones `10.0.0.0/16`. Se aplicó una arquitectura de defensa en profundidad mediante subredes para aislar capas expuestas (Frontend), lógica de negocio (Backend) y datos (Database).

### 3. Control de Acceso e Identidades (RBAC & Entra ID)
Aplicación de roles específicos de Microsoft Entra ID respetando el Principio de Menor Privilegio (PoLP):
* `Key Vault Secrets Officer` para administración exclusiva de credenciales.
* `Storage Blob Data Contributor` para gestión de objetos mediante tokens autenticados sin compartir claves maestras.

### 4. Hardening de Almacenamiento (Storage Account)
Despliegue de `stseguridadjh2026` aplicando configuraciones de endurecimiento:
* Restricción de versión mínima de transferencia a **TLS 1.2**.
* Bloqueo global de acceso anónimo a blobs.
* Deshabilitación total de acceso desde redes públicas no autorizadas.

### 5. Custodia de Secretos (Azure Key Vault)
Configuración de la bóveda `kv-seguridad-jh2026` con protección contra borrado accidental (*Soft-Delete*) activa. Se centralizó la custodia cifrada de credenciales sensibles para prevenir la exposición de contraseñas en código fuente.

---

## Tecnologías Utilizadas
* **Cloud Provider:** Microsoft Azure
* **Identity & Security:** Microsoft Entra ID (RBAC), Azure Key Vault
* **Networking:** Virtual Networks (VNet), Subnets, Network Security Groups (NSG)
