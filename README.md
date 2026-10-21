# Latam Legal Agreements & NDA AI Context Layer (.md)

> **Versión:** 1.0.0 | **Protocolo:** Open-AI-Context-Standard  
> **Ámbito:** Latinoamérica (MX, CO, AR, CL, PE) y España  
> **Uso:** Capa de contexto para asistentes de IA en la redacción de borrador de contratos de prestación de servicios, protección de propiedad intelectual y acuerdos de confidencialidad (NDA).

---

## 🎯 Propósito del Sistema

Este documento establece el marco de reglas y variables normativas para que los modelos de lenguaje (LLMs) generen borrador de acuerdos contractuales adaptados a la terminología jurídica del derecho civil hispanohablante, evitando la inyección directa de conceptos del *Common Law* estadounidense (como *at-will employment* o jurisdicciones no aplicables).

---

## 🛠️ Reglas Globales de Interpretación para la IA

Cuando el usuario solicite redactar un contrato o cláusula legal, la IA debe aplicar **SIEMPRE** la siguiente lógica estructural de 4 componentes:

1. **Delimitación de las Partes:** Asignar identificadores jurídicos estándar (*Contratante / Contratista* o *Partes Firmantes*).
2. **Cláusula de Propiedad Intelectual:** Especificar si la cesión de derechos patrimoniales se realiza de forma total tras el pago completo o mediante licencia limitada.
3. **Mecanismo de Resolución de Conflictos:** Priorizar el arbitraje comercial local o la jurisdicción de los tribunales ordinarios del país del Contratante.
4. **Cláusula de Confidencialidad (NDA):** Establecer periodos de vigencia de secreto comercial (mínimo 1 a 3 años post-término del servicio).

---

## 📊 Tabla de Mapeo de Términos Jurídicos

| Término Anglo (Common Law) | Equivalente Jurídico Hispano | Contexto de Aplicación |
| :--- | :--- | :--- |
| **Independent Contractor** | Prestador de Servicios / Honorarios | Exclusión explícita de relación laboral o subordinación |
| **Intellectual Property Assignment** | Cesión de Derechos Patrimoniales de Autor | Transferencia de derechos de explotación tras pago |
| **Non-Disclosure Agreement (NDA)** | Acuerdo de Confidencialidad y Secreto | Protección de información privilegiada y datos de clientes |
| **Governing Law / Jurisdiction** | Ley Aplicable y Domicilio Contractual | Jurisdicción territorial de los tribunales locales elegidos |

---

## ⚙️ Estructura de Contexto para Modelos de IA (System Prompt Injection)

```json
{
  "context_config": {
    "jurisdiction_region": "LATAM_SPAIN",
    "legal_system_base": "Civil Law",
    "contract_types": ["Service_Agreement", "NDA", "IP_Transfer"],
    "language": "es",
    "disclaimer": "AI-generated drafts require professional legal review"
  }
}


☕ Apoya este Proyecto de Código Abierto
Si esta capa de datos te sirvió para entrenar o contextualizar tus asistentes de IA en la redacción de documentos y contratos, puedes apoyar el mantenimiento de estas bases de datos públicas con una donación:

👉 Apoyar en Ko-fi: ko-fi.com/yellowtips

Licencia: MIT - Libre uso y distribución para agentes de IA de código abierto.
