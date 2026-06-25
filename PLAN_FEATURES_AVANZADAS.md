# 🚀 PLAN FEATURES AVANZADAS — Triage Órdenes v2.0

**Objetivo:** Que cada orden generada sea profesional, respaldada en Drive, logged en Sheets, y el CRM actualizado.

---

## 📋 FEATURES A IMPLEMENTAR

### SPRINT 1: Branding del Doctor (1-2 días)

**Módulo: Upload de Logo + Firma + Timbre**

```html
En el formulario, agregar:
├─ Upload Logo (PNG/JPG)
├─ Upload Firma (PNG/JPG transparent)
├─ Upload Timbre/Sello (PNG/JPG transparent)
└─ Preview de cómo se vería

Los documentos mostrarán:
├─ Logo en header (100x100px)
├─ Firma en footer (automática)
└─ Timbre diagonal en background (watermark)
```

**Resultado:** PDFs con branding profesional

---

### SPRINT 2: Google Drive + Numeración (2-3 días)

**Automático al descargar PDF:**

```
1. Doctor autoriza acceso a su Google Drive
2. App crea carpeta: /Triage Engine/Órdenes/2026-06/
3. Guarda PDF con nombre numerado:
   ├─ 001_Presupuesto_JuanPérez_25-06-2026.pdf
   ├─ 002_Exámenes_JuanPérez_25-06-2026.pdf
   └─ 003_EKG_JuanPérez_25-06-2026.pdf

4. Retorna URL de Drive al usuario
5. Respaldo permanente + auditable
```

**Ventaja:** Respaldo legítimo, numerado, en Drive del doctor

---

### SPRINT 3: Google Sheets Logging (2 días)

**Cuando se genera orden, auto-log en Google Sheet:**

```
Sheet: "Órdenes Generadas"

Columnas:
┌─────────┬──────────┬──────────┬────────┬─────────────┬──────────┐
│ Fecha   │ Doctor   │ Paciente │ RUT    │ Cirugía     │ Drive    │
├─────────┼──────────┼──────────┼────────┼─────────────┼──────────┤
│25/06    │ Kristal  │ Juan P.  │ 15.... │ Vasectomía  │ [Link]   │
│25/06    │ Kristal  │ María G. │ 18.... │ Lipo        │ [Link]   │
└─────────┴──────────┴──────────┴────────┴─────────────┴──────────┘

Ventaja:
- Registro oficial
- Auditable
- Histórico completo
- Sincronizable con contabilidad
```

---

### SPRINT 4: Sincronización CRM (2 días)

**Cuando se genera orden en triage-ordenes, el CRM se actualiza:**

```
Flow:
1. Doctor descarga PDF en triage-ordenes.vercel.app
2. App guarda en Drive + Sheets
3. API call: PUT /api/paciente/{pacienteId}/ordenEnviada
4. CRM recibe:
   {
     pacienteId: "pac-001",
     tipo: "Presupuesto",
     fechaEnvio: "2026-06-25T01:11:00Z",
     driveLinkPresupuesto: "https://drive.google.com/...",
     driveLinkExamenes: "https://drive.google.com/...",
     driveLinkEKG: "https://drive.google.com/...",
     status: "ÓRDENES_ENVIADAS"
   }

5. CRM actualiza:
   - Paciente.etapa = "Órdenes enviadas"
   - Paciente.documentosEnviados[] = [...links]
   - Timestamp
   - Doctor que envió
```

**Resultado:** CRM siempre sincronizado

---

### SPRINT 5: Automatización WhatsApp (3-4 días - FUTURO)

```
Flujo automatizado:
1. Orden se genera → Google Sheets lo registra
2. Zapier/Make.com webhook: "Nueva orden"
3. WhatsApp message automático:
   
   "Hola Juan, 
   
   Tu pre-orden de presupuesto para Vasectomía está lista.
   
   📋 Pre-Orden: [Link Drive]
   🔬 Exámenes a realizar: [Link Drive]
   
   Por favor realiza estos exámenes antes de tu consulta.
   
   ¿Confirmás tu cita?
   [Sí] [No]"
   
4. Respuesta del paciente → Log automático en Sheets
5. Doctor ve en CRM: "Paciente confirmó" ✅
```

**Ventaja:** Cero trabajo manual, follow-up automático

---

## 🗓️ HOJA DE RUTA (REALISTA)

### Esta semana (25-28 Jun):
```
✅ Sprint 1: Branding (Logo + Firma + Timbre)
✅ Sprint 2: Google Drive + Numeración
```

### Próxima semana:
```
✅ Sprint 3: Google Sheets Logging
✅ Sprint 4: CRM Sync API
```

### Más adelante:
```
⏳ Sprint 5: WhatsApp Automation (Zapier/Make)
⏳ Sprint 6: Instagram DM Automation
```

---

## 🏗️ ARQUITECTURA FINAL

```
┌─────────────────────────────────────────────────┐
│ Doctor abre triage-ordenes.vercel.app          │
├─────────────────────────────────────────────────┤
│  1. Carga datos paciente                        │
│  2. Agrega Logo + Firma + Timbre               │
│  3. Click "Generar Órdenes"                    │
│  4. Ve 3 documentos prellenados                │
│  5. Click "📥 PDF Presupuesto"                 │
│                                                 │
│  🔄 AUTOMÁTICO:                                │
│  ├─ Guarda en Google Drive (/Órdenes/001_...) │
│  ├─ Registra en Google Sheets                  │
│  ├─ Notifica al CRM vía API                    │
│  └─ Envía WhatsApp al paciente                │
│                                                 │
│  ✅ Orden completamente trazable               │
│  ✅ Respaldo en Drive                          │
│  ✅ Dashboard CRM actualizado                  │
│  ✅ Paciente notificado automático             │
└─────────────────────────────────────────────────┘

       ↓↓↓

┌─────────────────────────────────┐
│ CRM (triage-engine-prod)        │
├─────────────────────────────────┤
│ Paciente: Juan Pérez            │
│ Estado: Órdenes enviadas ✅     │
│ Documentos:                     │
│  - 📋 Presupuesto [Ver]         │
│  - 🔬 Exámenes [Ver]            │
│  - ❤️ EKG [Ver]                 │
│ Drive Respaldo: [Ver carpeta]   │
└─────────────────────────────────┘
```

---

## 💡 BENEFICIO FINAL PARA EL DOCTOR

```
ANTES:
- Genera presupuesto en Word
- Lo envía manualmente por email/WhatsApp
- Paciente pierde el documento
- Sin registro de qué se envió
- Sin respaldo legal

AHORA (con Triage):
✅ Click 1: Orden se genera
✅ Click 2: Se guarda en Drive automático
✅ Click 3: Paciente notificado por WhatsApp
✅ Click 4: CRM actualizado
✅ Click 5: Documentos respaldados y numerados

= Doctor ahorra 20-30 minutos por paciente
= Cero duplicados
= Respaldo legal + auditable
= Perfecto para contabilidad/compliance
```

---

## 🎯 PRIORIDADES (EMPEZAR POR...)

### Semana 1 (ESTA):
1. **Branding Module** ← Fácil, alto impacto
2. **Google Drive Backup** ← Medio, crítico

### Semana 2:
3. **Google Sheets Log** ← Fácil, permite auditoría
4. **CRM API Sync** ← Crítico, integra todo

### Semana 3+:
5. **WhatsApp Automation** ← Complejo, futuro

---

## ✅ LOS 3 "MUST HAVE" INMEDIATOS

```
1️⃣ BRANDING (Logo + Firma)
   → PDFs se ven profesionales
   → Listo ESTA SEMANA

2️⃣ GOOGLE DRIVE BACKUP
   → Respaldo automático numerado
   → Listo ESTA SEMANA

3️⃣ CRM SYNC
   → Dashboard se actualiza automático
   → Listo PRÓXIMA SEMANA
```

Sin estos 3, no es solución completa. Con estos 3, es BRUTAL.

---

**¿EMPEZAMOS POR BRANDING? 🚀**

(Mañana mismo puedo hacer todo el módulo listo para Vercel)
