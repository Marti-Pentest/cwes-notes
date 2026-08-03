# Módulo 07 — Server-side Attacks

## Sección 19/19: Skills Assessment

> [!NOTE]
> Assessment final que simula un pentest real contra el sitio de una empresa ficticia, sin indicar de antemano qué tipo de vulnerabilidad server-side está presente. Integra las cuatro familias cubiertas en el módulo.

> [!NOTE]
> **Metodología general aplicada (sin flag):**
> 1. **Reconocimiento de la superficie**: identificar parámetros que reflejan input del usuario, especialmente en funcionalidades que consultan sistemas externos (SSRF), generan contenido dinámico con datos de usuario (SSTI/SSI/XSLT), o usan archivos con extensiones reveladoras
> 2. **Confirmación con payloads de bajo impacto**: strings que rompan sintaxis sin ejecutar nada peligroso
> 3. **Identificación del sub-tipo específico**: motor de templating, versión y vendor de XSLT, etc.
> 4. **Escalada progresiva**: information disclosure → LFI → RCE, reutilizando el mecanismo de acceso descubierto en cada paso

> [!TIP]
> A diferencia de las secciones anteriores (donde el módulo ya indicaba qué vulnerabilidad buscar), este assessment refleja mejor un escenario real: hay que descubrir qué tipo de vulnerabilidad server-side está presente antes de poder explotarla.

No se documentan las flags ni la vulnerabilidad específica del assessment, respetando los términos de HTB Academy.

## 🔗 Relacionado
- [01 — Introduction](01-introduction.md)
- [03 — Identifying SSRF](03-identifying-ssrf.md)
- [09 — Identifying SSTI](09-identifying-ssti.md)
- [14 — Exploiting SSI Injection](14-exploiting-ssi-injection.md)
- [17 — Exploiting XSLT Injection](17-exploiting-xslt-injection.md)

#cwes #modulo07 #server-side-attacks #skills-assessment
