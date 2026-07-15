# Defensa · KYC de cédula colombiana

Presentación de defensa de tesis sobre un sistema de **KYC** (*Know Your Customer*) que,
a partir de la foto de una cédula colombiana, **clasifica** el tipo/lado del documento,
**rechaza** lo no soportado y **extrae** sus campos como JSON validado — todo corriendo
**100% local, sin nube ni APIs de terceros** (requisito de *Habeas Data*).

### ▶️ Ver la presentación

**https://ilukas28.github.io/kyc-defense/**

> Navega con las flechas ← → · tecla **P** para alternar la paleta noche/día (útil al
> proyectar con luz) · tecla **A** para saltar al apéndice. También hay un **QR** en la
> portada y en el cierre para abrirla desde el teléfono.

---

### De qué trata

Monet amplió su portafolio (de adelantos de salario y crédito a pago de facturas y
servicios) y con ello creció su exposición al **fraude por suplantación**. La verificación
de identidad se resolvía enviando cada documento a un modelo en la nube vía API, lo que
implica **costo por llamada, dependencia de un tercero y fuga de datos personales**.

Este trabajo propone un pipeline **end-to-end on-premise**:

```
Imagen
  │  Detección + corrección de perspectiva (OpenCV)
  ▼
Clasificador CNN — MobileNetV3-Small (transfer learning)
  │
  ├── Energy Score (Open Set Recognition) ──► rechaza lo desconocido (OOD)
  │
  └── Clase válida ──► LLM de visión local (qwen2.5-VL) lee la imagen
                         ──► JSON validado (Pydantic)
```

### Claves del enfoque

- **Clasificación + rechazo (OSR).** MobileNetV3-Small con **Energy Score** por clase para
  descartar documentos no soportados (pasaportes, licencias, look-alikes como la cédula
  ecuatoriana), en vez de confiar en el softmax.
- **Extracción con visión local.** Un LLM multimodal (qwen2.5-VL) lee la imagen directamente
  y devuelve los campos en JSON — más preciso que el OCR clásico y sin fuga de datos.
- **Privacidad por diseño.** Nada sale de la máquina: sin nube, sin API de terceros, sin
  costo por documento.

### Contenido de este repositorio

| Archivo | Descripción |
|---|---|
| `index.html` | Presentación autónoma (estilos, JS, logos y QR embebidos; sin dependencias externas). |

> Nota: el dataset y las imágenes de cédulas **no** se incluyen por tratarse de datos
> personales reales protegidos por la ley de *Habeas Data*.

---

**Autor:** Luis Guerrero · **Tutor:** Felipe Grijalva, Ph.D.
Universidad San Francisco de Quito · Maestría en Inteligencia Artificial · 2026
