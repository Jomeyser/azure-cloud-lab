# 🛡️ Azure Zero-Trust Cloud Infrastructure Deployment

## 📌 Descripción General
Documentación técnica del diseño, segmentación y despliegue de una infraestructura segura en **Microsoft Azure**, aplicando los principios del modelo **Zero Trust** ("Nunca confiar, siempre verificar") y el **Principio de Menor Privilegio (PoLP)** para la mitigación de vectores de ataque e hiper-segmentación de red.

---

## 📐 Arquitectura del Entorno

* **Grupo de Recursos:** `RG-Seguridad-Proyecto` (Región: `East US`)
* **Red Virtual (VNet):** `VNet-Principal` (`10.0.0.0/16`) con subredes segmentadas.
* **Cuenta de Almacenamiento:** `stseguridadjh2026` (TLS 1.2 forzado, bloqueos de acceso público).
* **Bóveda de Claves:** `kv-seguridad-jh2026` (Custodia criptográfica con Soft-Delete).

---

## 🛠️ Despliegue Paso a Paso

### 1. Gobernanza y Grupo de Recursos
Creación e inicialización del grupo de recursos `RG-Seguridad-Proyecto` como contenedor lógico de gestión y asignación de políticas de gobernanza en la región `East US`.

![Grupo de Recursos](01-resource-group.png)

---

### 2. Segmentación de Red y Control de Tráfico (VNet & Subnets)
Despliegue del direccionamiento `10.0.0.0/16` y aislamiento de capas operativas mediante subredes dedicate (`Frontend-Subnet`, `Backend-Subnet`, `DB-Subnet`). Se establecieron reglas de Network Security Groups (NSG) para restringir el tráfico inter-subnet únicamente a los puertos explícitamente autorizados.

![Segmentación de Red Virtual](02-vnet-subnets.png)

---

### 3. Hardening de Cuenta de Almacenamiento (Storage Account)
Implementación y endurecimiento de la cuenta `stseguridadjh2026`:
* **Cifrado en tránsito:** Exigencia obligatoria del protocolo **TLS 1.2**.
* **Aislamiento:** Deshabilitación de lectura pública anónima en contenedores Blob.
* **Restricción de Red:** Filtrado por firewall de almacenamiento limitando el acceso solo a IPs autorizadas.

![Configuración de Seguridad en Storage Account](03-storage-hardening.png)

---

### 4. Custodia Criptográfica y Gestión de Secretos (Azure Key Vault)
Configuración de `kv-seguridad-jh2026` para el almacenamiento de secretos, llaves de cifrado y cadenas de conexión. 
* Habilitación de la funcionalidad **Soft-Delete** y **Purge Protection** para evitar la eliminación maliciosa o accidental de activos criptográficos.

![Bóveda de Claves Key Vault](04-key-vault.png)

---

### 5. Control de Acceso Basado en Roles (RBAC & Entra ID)
Asignación granular de permisos a identidades de Microsoft Entra ID aplicando PoLP:
* `Key Vault Secrets Officer`: Administración exclusiva de secretos sin acceso a la administración del recurso Azure.
* `Storage Blob Data Contributor`: Acceso a objetos por tokens autenticados sin requerir compartir las claves primarias/secundarias del almacenamiento.

![Asignación de Roles RBAC](05-rbac-roles.png)

---

## 🚀 Tecnologías y Herramientas Utilizadas
* **Cloud Provider:** Microsoft Azure
* **Identidad & Accesos:** Microsoft Entra ID (RBAC), Azure Key Vault
* **Seguridad en Redes:** Virtual Networks (VNet), Subnets, Network Security Groups (NSG)
* **Cifrado & Custodia:** TLS 1.2/1.3, Soft-Delete, Secret Management
