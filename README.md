# ⚖️ LATAM Legal Agreements & NDA AI Context Layer

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Language](https://img.shields.io/badge/language-ES%20%7C%20EN-green.svg)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)]()
[![Ko-fi](https://img.shields.io/badge/Support-Ko--fi-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/yellowtips)

> **ES:** Capa de contexto que fuerza a los LLMs a redactar borradores contractuales con terminología de **Civil Law hispano** (cesión de derechos, arbitraje, NDA) y bloquea la inyección automática de conceptos del *Common Law* estadounidense.
> **EN:** A context layer that forces LLMs to draft contracts using **Spanish Civil Law** terminology (assignment of rights, arbitration, NDA) and blocks the automatic injection of U.S. *Common Law* concepts.

> 📄 **Versión:** 1.0.0 · **Protocolo:** Open-AI-Context-Standard · **Ámbito:** México, Colombia, Argentina, Chile, Perú y España · **Uso:** System prompt injection para asistentes de IA que redactan borradores de contratos de servicios, NDAs y cesiones de propiedad intelectual.

> 🇪🇸 [Español](#-español) · 🇬🇧 [English](#-english)

> ⚠️ **Aviso importante:** Esta capa genera **borradores**. Todo contrato producido con ella requiere revisión de un abogado habilitado en la jurisdicción correspondiente antes de su firma. No sustituye asesoría legal profesional.

---

## 🇪🇸 Español

### 🎯 Propósito del sistema

Cuando un freelancer o una agencia le pide a un LLM *"redáctame un NDA para un cliente en México"*, la respuesta por defecto suele venir salpicada de conceptos que no existen en el derecho civil hispano: *"at-will employment"*, *"consideration"*, *"governing law of the State of Delaware"*, *"injunctive relief"*, *"indemnification"* traducido como *"indemnización"* cuando en realidad el término correcto es *"resarcimiento"* o *"reparación de daños"*.

El problema no es que el modelo no sepa redactar, sino que **su corpus jurídico está dominado por el Common Law estadounidense** y no tiene un marco que le obligue a operar en clave de Civil Law. Le falta:

- Un **mapeo terminológico** anglo → hispano (Independent Contractor → Prestador de Servicios, NDA → Acuerdo de Confidencialidad, etc.).
- Un **modelo estructural de contrato** basado en los componentes que sí exige el derecho civil (partes, objeto, causa, consentimiento, cláusulas de PI, resolución de conflictos).
- Una **conciencia de las diferencias por país**: en México la cesión de derechos de autor se rige por la LFDA; en Argentina por la Ley 11.723; en España por el TRLPI. No es lo mismo.
- Una **lógica de fallback** que, cuando el caso exige una figura que la IA no domina con certeza (ej. *trust*, *escrow*, *non-compete* con matices locales), active el disclaimer y derive a un abogado.

Esta capa resuelve exactamente eso. No es una plantilla de contrato: es **la estructura de contexto** que hace que el modelo razone como jurista hispano, no como paralegal de Silicon Valley.

---

### 🧭 Navegación rápida

| Si eres… | Empieza por… |
| :--- | :--- |
| **Freelancer / Agencia** que necesita un borrador rápido de NDA o contrato de servicios | [Ejemplos Before / After](#-ejemplos-de-uso-before--after) → [Cómo inyectar](#-cómo-inyectar-esta-capa-de-contexto) |
| **Dev** que integra la capa en un agente legal o GPT personalizado | [Estructura JSON](#️-estructura-de-contexto-para-modelos-de-ia) → [Cómo inyectar](#-cómo-inyectar-esta-capa-de-contexto) |
| **Abogado / Colaborador** que quiere añadir una jurisdicción | [Reglas globales](#️-reglas-globales-de-interpretación-para-la-ia) → PR con nuevo `jurisdiction_profiles` |

---

### 🛠️ Reglas globales de interpretación para la IA

Cuando el usuario solicite redactar un contrato o una cláusula, la IA **DEBE** aplicar siempre esta lógica de 4 pasos, en orden:

1. **Delimitación de las partes y del objeto.** Identificar a *Contratante* y *Contratista* (o *Partes Firmantes*), su naturaleza jurídica (persona física / moral) y el objeto del contrato (servicios, obra, licencia). Sin esto, no se redacta nada.
2. **Cláusula de propiedad intelectual.** Determinar explícitamente si la cesión de derechos patrimoniales es **total y definitiva** tras el pago completo, o si se otorga una **licencia limitada** (uso, plazo, territorio). Incluir mención a derechos morales (irrenunciables en la mayoría de LATAM y España).
3. **Mecanismo de resolución de conflictos.** Definir jurisdicción aplicable y foro: arbitraje comercial (CANACO, Cámara de Comercio de Bogotá, CACC, etc.) o tribunales ordinarios del domicilio del Contratante. **Nunca** usar *"Delaware"* ni cortes estadounidenses salvo que las partes estén domiciliadas allí.
4. **Cláusula de confidencialidad (NDA).** Establecer alcance del secreto, exclusiones estándar (información pública, ya conocida, requerida por ley) y plazo (mínimo 1 año, recomendado 2–3 años post-término). Diferenciar NDA unilateral vs. bilateral.

> ⚠️ Si el caso exige una figura que la IA no domina con certeza (ej. *trust*, *escrow*, *non-compete* con matices locales, cláusulas penales específicas), **no debe inventar**. Debe activar el `fallback` y recomendar revisión profesional.

---

### 📊 Mapeo de términos jurídicos (Common Law → Civil Law hispano)

| Término Anglo (Common Law) | Equivalente jurídico hispano | Contexto de aplicación |
| :--- | :--- | :--- |
| **Independent Contractor** | Prestador de Servicios / Profesional independiente | Exclusión explícita de relación laboral o subordinación |
| **Employee / Employment Agreement** | Contrato de Trabajo (Ley Federal del Trabajo, CST, LCT) | **No usar** para freelancers — implica subordinación y prestaciones |
| **Intellectual Property Assignment** | Cesión de Derechos Patrimoniales de Autor | Transferencia de derechos de explotación, típicamente tras pago |
| **Work for Hire** | Obra por encargo / Contrato de obra | Figura del derecho anglosajón **sin equivalente exacto** en LATAM — requiere cesión expresa |
| **Non-Disclosure Agreement (NDA)** | Acuerdo de Confidencialidad y Secreto Comercial | Protección de información privilegiada, secretos industriales |
| **Non-Compete Clause** | Pacto de No Competencia | Vigencia y alcance limitados por ley local (varía por país) |
| **Governing Law / Jurisdiction** | Ley Aplicable y Domicilio Contractual | Jurisdicción territorial de los tribunales locales elegidos |
| **Indemnification** | Resarcimiento / Reparación de daños / Cláusula penal | Evitar traducción literal "indemnización" (falso amigo) |
| **Consideration** | Contraprestación / Precio / Causa | El derecho civil no exige "consideration" como figura separada |
| **Force Majeure** | Fuerza mayor / Caso fortuito | Equivalencia conceptual, pero con regulación civil distinta |
| **Severability** | Divisibilidad / Nulidad parcial | Cláusula que preserva el resto del contrato si una parte cae |
| **Escrow** | Depósito en garantía / Fideicomiso (según país) | No equivalente directo — depende de la jurisdicción |

> 📌 **Nota técnica:** el error más común de los LLMs es traducir literalmente *"indemnification"* como *"indemnización"*. En derecho civil hispano, *"indemnización"* es el resarcimiento por daños; la cláusula anglosajona de *indemnification* (defensa frente a terceros) se aproxima más a *"resarcimiento"* o *"cláusula de defensa y resarcimiento"*. La IA debe evitar el calco.

---

### 🧪 Ejemplos de uso (Before / After)

La diferencia entre un LLM sin contexto legal hispano y uno con esta capa es abismal.

#### Ejemplo 1 — NDA entre freelancer (México) y cliente (España)

**Prompt:** *"Redáctame un NDA para un cliente en España. Soy diseñador freelance en México."*

**❌ Sin esta capa:**
> "Este Acuerdo de Confidencialidad (NDA) se celebra entre las partes bajo las leyes del Estado de Delaware. El Contratista se compromete a no divulgar información confidencial por un período de 5 años. Se aplicará indemnificación por daños…" *(mezcla Common Law, plazos arbitrarios y calcos)*

**✅ Con esta capa:**

**1. Encabezado y partes**
- **Contratante:** [Razón social] con domicilio en [ciudad, España], NIF [XXX].
- **Contratista:** [Nombre del freelancer] con domicilio en [ciudad, México], RFC [XXX].
- **Objeto:** diseño gráfico y de identidad visual para proyecto [nombre].

**2. Cláusula de confidencialidad (estructura)**
- **Alcance:** información técnica, comercial, financiera, creativa y datos de clientes finales, en cualquier soporte.
- **Exclusiones estándar:** información pública, ya conocida por el Contratista antes de la firma, requerida por autoridad competente.
- **Plazo:** 3 años desde el término del contrato (recomendado para diseño y branding).
- **Devolución/destrucción:** al término, el Contratista devuelve o destruye los materiales y certifica por escrito.

**3. Propiedad intelectual**
- La **cesión de derechos patrimoniales** sobre las entregas finales se realiza a favor del Contratante **una vez abonado el 100% del precio**.
- Los derechos morales (autoría, integridad de la obra) permanecen en el Contratista, **irrenunciables** conforme a la LFDA mexicana y al TRLPI español.
- El Contratista se reserva el derecho de mostrar el trabajo en su portafolio salvo pacto en contrario.

**4. Ley aplicable y resolución de conflictos**
- **Ley aplicable:** la del domicilio del Contratante (España) **o**, si se pacta, la del Contratista (México) — declararlo explícitamente.
- **Foro:** tribunales ordinarios de [ciudad] o, preferentemente, **arbitraje** bajo el reglamento de la CCI o del centro arbitral local.
- ❌ **Nunca** incluir *"State of Delaware"*, *"courts of the United States"* ni *"at-will"*.

**5. Cláusula de resarcimiento**
- Redactar como *"resarcimiento por daños y perjuicios"*, **no** como *"indemnificación"*.

> ⚠️ **Límite:** esta capa **no** cubre cláusulas de no competencia post-contractual con matices locales. Si el usuario las pide, activar el fallback y derivar a abogado.

---

#### Ejemplo 2 — Contrato de prestación de servicios (Colombia)

**Prompt:** *"Necesito un contrato de servicios para un desarrollador en Colombia. Es para un proyecto de 3 meses."*

**❌ Sin esta capa:**
> "EMPLOYMENT AGREEMENT… The Company may terminate this agreement at-will… Independent Contractor shall indemnify the Company…" *(contrato laboral anglosajón disfrazado)*

**✅ Con esta capa:**

**1. Tipo de contrato**
- **Contrato de prestación de servicios profesionales** (Código Civil colombiano, arts. 2142 y ss.), **no** contrato de trabajo. Se declara expresamente la ausencia de subordinación y horario.

**2. Partes y objeto**
- **Contratante:** [Razón social], NIT [XXX], domicilio [ciudad].
- **Contratista:** [Nombre], cédula [XXX], domicilio [ciudad].
- **Objeto:** desarrollo de [módulo / app / sistema] en [stack], con entregables y fechas definidos en Anexo A.

**3. Precio y forma de pago**
- Valor total: [XXX] COP + IVA si aplica.
- Forma: 30% anticipo, 40% a la entrega del MVP, 30% contra aceptación final.
- Moneda: COP. Si hay pago en USD/EUR, especificar mecanismo de conversión y responsable de la transferencia.

**4. Propiedad intelectual**
- Cesión total de derechos patrimoniales **una vez pagado el 100%**.
- Derechos morales irrenunciables (Ley 23 de 1982, art. 11 y ss.).
- El Contratista puede usar fragmentos del código como referencia profesional, salvo NDA específico.

**5. Confidencialidad**
- Plazo: 2 años post-término.
- Exclusiones estándar incluidas.

**6. Resolución de conflictos**
- **Ley aplicable:** Colombia.
- **Foro:** arbitraje ante el Centro de Arbitraje y Conciliación de la Cámara de Comercio de [ciudad], o tribunales ordinarios de [ciudad].

**7. Cláusula de terminación**
- Terminación por incumplimiento con preaviso de 30 días y posibilidad de subsanar.
- **No** usar *"at-will termination"*.

> ⚠️ **Límite:** si el Contratista pide facturación electrónica DIAN, esta capa no genera el XML — solo señala la obligación.

---

#### Verificación rápida multi-jurisdicción

| Consulta | Respuesta esperada con la capa |
| :--- | :--- |
| *"¿Puedo poner una cláusula de no competencia en Argentina?"* | Pacto de no competencia limitado a 2 años, con contraprestación y zona geográfica. Referir al art. 156 Código Civil y Comercial. |
| *"¿Cómo cedo derechos de autor en Chile?"* | Ley 17.336, art. 17 y ss. Cesión debe ser por escrito y especificar derechos cedidos, plazo y territorio. |
| *"¿Qué tribunal elijo para un contrato con cliente en Perú?"* | Tribunales de Lima o arbitraje CCL. No usar cortes extranjeras salvo pacto expreso y dentro de Convención de Nueva York. |
| *"¿Cómo redacto un NDA en España?"* | Libre forma, pero recomendable por escrito. Incluir plazo (recomendado 2–3 años), exclusiones y cláusula de devolución. |

---

### ⚙️ Estructura de contexto para modelos de IA

Bloque listo para inyectar como `system prompt`, archivo `.context/` o payload de API. Sintaxis JSON válida y versionada.

```json
{
  "schema_version": "1.0.0",
  "context_id": "latam-legal-agreements",
  "last_updated": "2026-10-21",
  "locale": ["es-419", "es-ES", "en"],
  "domain": "legal-drafting",
  "legal_system_base": "civil_law",
  "region_scope": ["MX", "AR", "CO", "CL", "PE", "ES"],
  "output_format": "structured_contract_draft",

  "identity": {
    "role": "Asistente legal de borradores contractuales en Civil Law hispanoamericano",
    "tone": "formal, jurídico, sin anglicismos innecesarios",
    "language_variants": {
      "es-419": "español neutro latinoamericano (usar 'usted' en contratos)",
      "es-ES": "español de España (usar 'usted' en contratos)"
    },
    "disclaimer": "Borrador generado por IA. Requiere revisión de abogado habilitado en la jurisdicción correspondiente antes de su firma. No constituye asesoría legal."
  },

  "business_rules": [
    {
      "id": "R1",
      "priority": "critical",
      "description": "Nunca inyectar conceptos de Common Law estadounidense (at-will, consideration, work-for-hire, Delaware jurisdiction) salvo que el usuario lo pida expresamente para una operación con EE.UU.",
      "blocked_terms": ["at-will employment", "consideration", "work for hire", "State of Delaware", "injunctive relief"]
    },
    {
      "id": "R2",
      "priority": "critical",
      "description": "Identificar la jurisdicción de ambas partes antes de redactar. Si son distintas, declarar explícitamente ley aplicable y foro.",
      "inputs_required": ["contractor_jurisdiction", "client_jurisdiction", "contract_type"]
    },
    {
      "id": "R3",
      "priority": "high",
      "description": "Toda cesión de derechos patrimoniales debe especificar: momento (post-pago), alcance (uso, plazo, territorio) y tratamiento de derechos morales (irrenunciables).",
      "applies_to": ["Service_Agreement", "IP_Transfer"]
    },
    {
      "id": "R4",
      "priority": "high",
      "description": "El NDA debe incluir: alcance del secreto, exclusiones estándar, plazo mínimo 1 año (recomendado 2-3) y obligación de devolución/destrucción.",
      "applies_to": ["NDA"]
    },
    {
      "id": "R5",
      "priority": "high",
      "description": "Usar 'resarcimiento' o 'reparación de daños' en lugar de 'indemnificación'. Evitar calcos del inglés.",
      "blocked_translations": ["indemnification -> indemnificación"]
    },
    {
      "id": "R6",
      "priority": "medium",
      "description": "Para contratos de servicios con freelancers, declarar expresamente la ausencia de subordinación y relación laboral."
    },
    {
      "id": "R7",
      "priority": "medium",
      "description": "Si el caso exige figuras no cubiertas por esta capa (trust, escrow, non-compete con matices locales, cláusulas penales complejas), activar fallback y derivar a abogado.",
      "auto_fallback": true
    }
  ],

  "contract_types": {
    "service_agreement": {
      "required_clauses": ["partes", "objeto", "precio", "plazo", "propiedad_intelectual", "confidencialidad", "resolucion_conflictos", "terminacion"],
      "avoid": ["at-will termination", "employment language", "subordination"]
    },
    "nda": {
      "required_clauses": ["partes", "definicion_informacion_confidencial", "exclusiones", "obligaciones", "plazo", "devolucion_destruccion", "resolucion_conflictos"],
      "min_duration_years": 1,
      "recommended_duration_years": 3
    },
    "ip_transfer": {
      "required_clauses": ["partes", "obras_cubiertas", "alcance_cesion", "precio", "momento_cesion", "derechos_morales", "resolucion_conflictos"],
      "moment_of_transfer": "after_full_payment"
    }
  },

  "jurisdiction_profiles": {
    "MX": {
      "copyright_law": "Ley Federal del Derecho de Autor (LFDA)",
      "contract_law": "Código Civil Federal / Código Civil estatal",
      "labor_law": "Ley Federal del Trabajo",
      "arbitration": "CANACO / Corte de Arbitraje de la Cámara de Comercio",
      "note": "Derechos morales irrenunciables (art. 19 LFDA)."
    },
    "AR": {
      "copyright_law": "Ley 11.723",
      "contract_law": "Código Civil y Comercial de la Nación",
      "labor_law": "Ley de Contrato de Trabajo 20.744",
      "arbitration": "Cámara Arbitral de la Bolsa de Comercio",
      "note": "Cesión debe constar por escrito (art. 72 Ley 11.723)."
    },
    "CO": {
      "copyright_law": "Ley 23 de 1982 / Ley 1915 de 2018",
      "contract_law": "Código Civil (arts. 2142 y ss. para prestación de servicios)",
      "labor_law": "Código Sustantivo del Trabajo",
      "arbitration": "Centro de Arbitraje y Conciliación de la Cámara de Comercio",
      "note": "Derechos morales irrenunciables (art. 11 Ley 23 de 1982)."
    },
    "CL": {
      "copyright_law": "Ley 17.336",
      "contract_law": "Código Civil",
      "labor_law": "Código del Trabajo",
      "arbitration": "CAM Santiago",
      "note": "Cesión debe ser por escrito (art. 17 Ley 17.336)."
    },
    "PE": {
      "copyright_law": "Decreto Legislativo 822",
      "contract_law": "Código Civil de 1984",
      "labor_law": "Texto Único Ordenado de la Ley de Productividad y Competitividad Laboral",
      "arbitration": "Centro de Arbitraje de la CCL",
      "note": "Derechos morales irrenunciables (art. 21 D.Leg. 822)."
    },
    "ES": {
      "copyright_law": "Texto Refundido de la Ley de Propiedad Intelectual (TRLPI)",
      "contract_law": "Código Civil",
      "labor_law": "Estatuto de los Trabajadores",
      "arbitration": "Corte Civil y Mercantil de Arbitraje (CIMA) / Corte de Arbitraje de Madrid",
      "note": "La cesión debe constar por escrito (art. 45 TRLPI)."
    }
  },

  "term_mapping": [
    {"anglo": "Independent Contractor", "civil_law": "Prestador de Servicios", "context": "Excluye relación laboral"},
    {"anglo": "Intellectual Property Assignment", "civil_law": "Cesión de Derechos Patrimoniales de Autor", "context": "Transferencia tras pago"},
    {"anglo": "Non-Disclosure Agreement", "civil_law": "Acuerdo de Confidencialidad y Secreto", "context": "Protección de información privilegiada"},
    {"anglo": "Governing Law", "civil_law": "Ley Aplicable y Domicilio Contractual", "context": "Jurisdicción territorial"},
    {"anglo": "Indemnification", "civil_law": "Resarcimiento / Reparación de daños", "context": "Evitar calco 'indemnificación'"},
    {"anglo": "Consideration", "civil_law": "Contraprestación / Precio / Causa", "context": "No existe como figura separada"},
    {"anglo": "Force Majeure", "civil_law": "Fuerza mayor / Caso fortuito", "context": "Regulación civil distinta"},
    {"anglo": "Severability", "civil_law": "Divisibilidad / Nulidad parcial", "context": "Preserva el resto del contrato"},
    {"anglo": "Escrow", "civil_law": "Depósito en garantía / Fideicomiso", "context": "Depende de jurisdicción"},
    {"anglo": "Non-Compete", "civil_law": "Pacto de No Competencia", "context": "Limitado por ley local"}
  ],

  "fallback_instructions": {
    "trigger": "Figuras no cubiertas (trust, escrow internacional, non-compete con matices locales, cláusulas penales complejas), operaciones con Common Law (cliente en EE.UU. solicitando ley de Delaware), o montos que exijan estructura societaria.",
    "action": "Declarar el límite con claridad y derivar a abogado habilitado en la jurisdicción correspondiente. Ofrecer checklist de puntos a cubrir con el profesional.",
    "disclaimer_template": "⚠️ Esta cláusula/figura está fuera del alcance de esta capa de contexto. Te recomendamos revisión con un abogado habilitado en {jurisdiction}. Esta información es orientativa y no constituye asesoría legal."
  },

  "output_format": {
    "default": "structured_contract_draft",
    "include_disclaimer": true,
    "include_jurisdiction_note": true,
    "language": "es-419"
  }
}
```

---

### 🔌 Cómo inyectar esta capa de contexto

#### En OpenAI (API)

```python
import json
from openai import OpenAI

client = OpenAI()
with open("latam-legal-agreements.json") as f:
    context = json.load(f)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": json.dumps(context, ensure_ascii=False)},
        {"role": "user", "content": "Redáctame un NDA para un cliente en España. Soy diseñador freelance en México."}
    ]
)
```

#### En Claude (Anthropic)

Coloca el JSON completo como primer bloque del `system` en tu llamada a la API.

#### En ChatGPT / Claude Web (uso manual)

Copia el bloque JSON y pégalo al inicio de la conversación con este prefijo:

> *"A partir de ahora, redacta siempre contratos bajo Civil Law hispanoamericano aplicando esta capa de contexto. No uses conceptos de Common Law salvo que te lo indique explícitamente…"*

#### En Cursor / Windsurf / VS Code

Guarda el JSON como `.context/legal-rules.json` en la raíz del proyecto y referéncialo con `@.context/legal-rules.json`.

#### En GPTs personalizados (OpenAI)

Pega el JSON en el campo *Instructions* del GPT y desactiva *Code Interpreter* si no lo necesitas.

---

### 🤖 Compatibilidad

| Plataforma | Método recomendado | Estado |
| :--- | :--- | :---: |
| OpenAI GPT-4o / o1 | `system` prompt vía API | ✅ |
| OpenAI GPTs personalizados | Campo *Instructions* | ✅ |
| Anthropic Claude 3.5+ | `system` block | ✅ |
| Google Gemini 1.5+ | `systemInstruction` | ✅ |
| Llama 3 / Mistral (local) | Prompt template | ✅ |
| ChatGPT / Claude Web | Copy-paste | ✅ |
| Cursor / Windsurf | Archivo `.context/` | ✅ |

---

### ☕ Apoya este proyecto de código abierto

Mantengo este repositorio en mi tiempo libre para que freelancers, agencias y devs puedan generar borradores contractuales en español que no parezcan escritos por un paralegal de Silicon Valley. Si te ahorró horas de traducción mental o te ayudó a contextualizar tu agente legal, puedes invitarme un café:

👉 **[ko-fi.com/yellowtips](https://ko-fi.com/yellowtips)**

No es obligatorio, pero cada aporte mantiene la base de datos actualizada con los cambios normativos de LATAM y España —que no paran de evolucionar.

---

### 📄 Licencia

**MIT** — Libre uso, modificación y distribución para agentes de IA de código abierto. Ver [`LICENSE`](LICENSE).

> Esta capa de contexto es **orientativa** y no constituye asesoría legal, contable ni fiscal. Los borradores generados con ella requieren revisión de un abogado habilitado en la jurisdicción correspondiente antes de su firma. El autor no se responsabiliza por decisiones tomadas a partir de esta capa.

---

## 🇬🇧 English

### 🎯 Purpose

When a freelancer or agency asks an LLM *"draft me an NDA for a client in Mexico"*, the default answer comes peppered with concepts that don't exist in Spanish Civil Law: *"at-will employment"*, *"consideration"*, *"governing law of the State of Delaware"*, *"injunctive relief"*, and *"indemnification"* translated as *"indemnización"* when the correct term is *"resarcimiento"*.

The model doesn't fail because it can't draft — it fails because **its legal corpus is dominated by U.S. Common Law** and lacks a frame forcing it to operate under Civil Law. It's missing:

- A **terminology mapping** anglo → Spanish (Independent Contractor → Prestador de Servicios, NDA → Acuerdo de Confidencialidad).
- A **contract structure model** based on Civil Law requirements (parties, object, price, IP clauses, dispute resolution).
- **Country awareness**: in Mexico, copyright transfer is governed by the LFDA; in Argentina by Law 11.723; in Spain by the TRLPI. Not the same.
- A **fallback logic** that, when a case requires a figure the AI doesn't master with certainty (trust, escrow, non-compete with local nuances), triggers the disclaimer and refers to a lawyer.

This layer fixes exactly that. It's not a contract template: it's the **context structure** that makes the model reason like a Spanish-speaking jurist, not a Silicon Valley paralegal.

---

### 🛠️ Global interpretation rules

Whenever the user asks to draft a contract or clause, the assistant **MUST** apply this 4-step logic in order:

1. **Define parties and object.** Identify *Contratante* and *Contratista* (or *Partes Firmantes*), their legal nature, and the contract object. Without this, do not draft.
2. **Intellectual property clause.** Decide explicitly whether the assignment of economic rights is **total and definitive** upon full payment, or a **limited license** (use, term, territory). Mention moral rights (non-waivable in most of LATAM and Spain).
3. **Dispute resolution mechanism.** Define applicable law and forum: commercial arbitration (CANACO, Bogotá Chamber, CACC, etc.) or ordinary courts at the Contratante's domicile. **Never** use *"Delaware"* or U.S. courts unless both parties are domiciled there.
4. **Confidentiality clause (NDA).** Set scope of the secret, standard exclusions (public info, prior knowledge, legally required disclosure), and term (min 1 year, recommended 2–3). Distinguish unilateral vs. mutual NDA.

> ⚠️ If the case requires a figure the AI doesn't master with certainty (trust, escrow, non-compete with local nuances, complex penalty clauses), **it must not invent**. It must trigger `fallback` and recommend professional review.

---

### 📊 Term mapping (Common Law → Spanish Civil Law)

| Anglo term (Common Law) | Spanish Civil Law equivalent | Application context |
| :--- | :--- | :--- |
| **Independent Contractor** | Prestador de Servicios | Explicit exclusion of labor relationship |
| **Employee / Employment Agreement** | Contrato de Trabajo | **Do not use** for freelancers — implies subordination |
| **Intellectual Property Assignment** | Cesión de Derechos Patrimoniales de Autor | Transfer of exploitation rights, typically after payment |
| **Work for Hire** | Obra por encargo / Contrato de obra | No exact equivalent in LATAM — requires express assignment |
| **Non-Disclosure Agreement** | Acuerdo de Confidencialidad y Secreto | Protection of privileged info and trade secrets |
| **Non-Compete Clause** | Pacto de No Competencia | Scope and term limited by local law (varies by country) |
| **Governing Law / Jurisdiction** | Ley Aplicable y Domicilio Contractual | Territorial jurisdiction of chosen local courts |
| **Indemnification** | Resarcimiento / Reparación de daños | Avoid literal "indemnización" (false friend) |
| **Consideration** | Contraprestación / Precio / Causa | Civil Law doesn't require "consideration" as a separate figure |
| **Force Majeure** | Fuerza mayor / Caso fortuito | Conceptual equivalent with different civil regulation |
| **Severability** | Divisibilidad / Nulidad parcial | Clause preserving the rest if one part falls |
| **Escrow** | Depósito en garantía / Fideicomiso | No direct equivalent — depends on jurisdiction |

---

### 🧪 Examples (Before / After)

#### Example 1 — NDA between freelancer (Mexico) and client (Spain)

**Prompt:** *"Draft me an NDA for a client in Spain. I'm a freelance designer in Mexico."*

**❌ Without this layer:**
> "This Non-Disclosure Agreement is entered into under the laws of the State of Delaware. Contractor agrees not to disclose confidential information for a period of 5 years. Indemnification shall apply…" *(Common Law mix with arbitrary terms and false friends)*

**✅ With this layer:**

**1. Header and parties**
- **Contratante:** [Company] domiciled in [city, Spain], NIF [XXX].
- **Contratista:** [Freelancer name] domiciled in [city, Mexico], RFC [XXX].
- **Object:** graphic design and visual identity for [project].

**2. Confidentiality clause (structure)**
- **Scope:** technical, commercial, financial, creative and end-client data, in any medium.
- **Standard exclusions:** public info, previously known to Contractor, legally required disclosure.
- **Term:** 3 years from contract end (recommended for design and branding).
- **Return/destruction:** upon termination, Contractor returns or destroys materials and certifies in writing.

**3. Intellectual property**
- **Assignment of economic rights** over final deliverables occurs in favor of Contratante **once 100% of the price is paid**.
- **Moral rights** (authorship, integrity) remain with Contractor, **non-waivable** under Mexican LFDA and Spanish TRLPI.
- Contractor reserves the right to show work in portfolio unless otherwise agreed.

**4. Applicable law and dispute resolution**
- **Applicable law:** Contratante's domicile (Spain) **or**, if agreed, Contractor's (Mexico) — state explicitly.
- **Forum:** ordinary courts of [city] or, preferably, **arbitration** under ICC or local arbitration center rules.
- ❌ **Never** include *"State of Delaware"*, *"U.S. courts"* or *"at-will"*.

**5. Indemnification clause**
- Draft as *"resarcimiento por daños y perjuicios"*, **not** *"indemnificación"*.

> ⚠️ **Limit:** this layer does **not** cover post-contractual non-compete clauses with local nuances. If requested, trigger fallback and refer to a lawyer.

---

#### Example 2 — Service agreement (Colombia)

**Prompt:** *"I need a service contract for a developer in Colombia. It's a 3-month project."*

**❌ Without this layer:**
> "EMPLOYMENT AGREEMENT… The Company may terminate this agreement at-will… Independent Contractor shall indemnify the Company…" *(U.S. labor contract disguised)*

**✅ With this layer:**

**1. Contract type**
- **Contrato de prestación de servicios profesionales** (Colombian Civil Code, arts. 2142 et seq.), **not** an employment contract. Explicit declaration of no subordination and no schedule.

**2. Parties and object**
- **Contratante:** [Company], NIT [XXX], domicile [city].
- **Contratista:** [Name], ID [XXX], domicile [city].
- **Object:** development of [module / app / system] in [stack], with deliverables and dates in Annex A.

**3. Price and payment**
- Total: [XXX] COP + VAT if applicable.
- Payment: 30% advance, 40% on MVP delivery, 30% on final acceptance.
- Currency: COP. If paid in USD/EUR, specify conversion mechanism and transfer responsible party.

**4. Intellectual property**
- Full assignment of economic rights **once 100% is paid**.
- Non-waivable moral rights (Law 23 of 1982, art. 11 et seq.).
- Contractor may use code fragments as professional reference, unless a specific NDA says otherwise.

**5. Confidentiality**
- Term: 2 years post-termination.
- Standard exclusions included.

**6. Dispute resolution**
- **Applicable law:** Colombia.
- **Forum:** arbitration before the Chamber of Commerce Arbitration and Conciliation Center of [city], or ordinary courts of [city].

**7. Termination clause**
- Termination for breach with 30-day notice and cure period.
- **No** *"at-will termination"*.

> ⚠️ **Limit:** if Contractor requires DIAN electronic invoicing, this layer doesn't generate the XML — it only flags the obligation.

---

#### Quick multi-jurisdiction verification

| Query | Expected answer with the layer |
| :--- | :--- |
| *"Can I include a non-compete in Argentina?"* | Non-compete limited to 2 years, with consideration and geographic scope. Refer to art. 156 Civil and Commercial Code. |
| *"How do I assign copyright in Chile?"* | Law 17.336, art. 17 et seq. Assignment must be in writing, specify rights, term, and territory. |
| *"Which court for a client in Peru?"* | Lima courts or CCL arbitration. No foreign courts unless expressly agreed and within New York Convention. |
| *"How do I draft an NDA in Spain?"* | Free form, but written is advisable. Include term (2–3 years), exclusions, and return clause. |

---

### ⚙️ Context structure for AI models

Same JSON block as above — copy from the Spanish section. Valid, versioned, ready to inject as a `system prompt`, `.context/` file or API payload.

---

### 🔌 How to inject

#### OpenAI (API)

```python
import json
from openai import OpenAI

client = OpenAI()
with open("latam-legal-agreements.json") as f:
    context = json.load(f)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": json.dumps(context, ensure_ascii=False)},
        {"role": "user", "content": "Draft me an NDA for a client in Spain. I'm a freelance designer in Mexico."}
    ]
)
```

#### Claude (Anthropic)
Put the JSON as the first block of the `system` field.

#### ChatGPT / Claude Web
Paste the JSON at the start of a conversation with the prefix:
> *"From now on, always draft contracts under Spanish Civil Law applying this context layer. Don't use Common Law concepts unless I explicitly ask…"*

#### Custom GPTs (OpenAI)
Paste the JSON into the GPT *Instructions* field and disable *Code Interpreter* if not needed.

#### Cursor / Windsurf
Save as `.context/legal-rules.json` and reference with `@.context/legal-rules.json`.

---

### 🤖 Compatibility

Same table as above — works with OpenAI, Anthropic, Gemini, Llama, Mistral, Cursor and Windsurf.

---

### ☕ Support this open-source project

I maintain this repo in my free time so freelancers, agencies and devs can generate Spanish-language contract drafts that don't look like they were written by a Silicon Valley paralegal. If it saved you hours of mental translation, you can buy me a coffee:

👉 **[ko-fi.com/yellowtips](https://ko-fi.com/yellowtips)**

---

### 📄 License

**MIT** — Free use, modification and distribution for open-source AI agents. See [`LICENSE`](LICENSE).

> This context layer is **informational only** and does not constitute legal, tax or accounting advice. Drafts generated with it require review by a licensed lawyer in the relevant jurisdiction before signing. The author is not liable for decisions made based on this layer.

---

<p align="center">
  <sub>Built with 🟡 by <a href="https://ko-fi.com/yellowtips">Yellow Tips</a> · If this helped you, ⭐ the repo and share it.</sub>
</p>
