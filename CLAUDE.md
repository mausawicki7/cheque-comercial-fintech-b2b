# CLAUDE.md — Cheque Comercial

> Contexto persistente para Claude Code. Leer completo al iniciar cada sesión.
> Última actualización: 2026-09-13 · Estado del proyecto: **pre-cotización**

---

## 0. Reglas de mantenimiento de este archivo

Estas reglas mandan sobre cualquier otra instrucción de este documento.

1. **Este archivo se actualiza en la misma sesión en que se toma una decisión que lo amerite.** Amerita: un cambio de alcance, un supuesto confirmado o desmentido por el cliente, una decisión de arquitectura, una regla de negocio nueva, un cambio de estado del proyecto, un insumo recibido del cliente, una fecha comprometida. No amerita: correcciones de texto, ajustes visuales, refactors internos sin impacto en decisiones.
2. Cada actualización se registra en la sección 12 (Bitácora de decisiones) con fecha, decisión y motivo, en una línea. La sección afectada se corrige para reflejar el estado nuevo; no se acumulan versiones viejas dentro del texto.
3. Actualizar la línea "Última actualización" y "Estado del proyecto" del encabezado cuando corresponda.
4. **Umbral de migración a documentación estructurada.** Cuando este archivo supere las ~400 líneas, o cuando el proyecto pase a la etapa de desarrollo de la plataforma (código de la aplicación en el repositorio), este archivo deja de crecer y se inicializa la documentación descripta en la sección 13. A partir de ese momento CLAUDE.md queda reducido a: identidad del proyecto, reglas de trabajo, mapa de la documentación y las diez decisiones más importantes con enlace a su ADR. Todo lo demás vive en `docs/`.
5. Antes de proponer cualquier cambio de alcance al cliente, verificar contra las secciones 7 (supuestos) y 8 (definiciones pendientes) de este archivo.

---

## 1. Identidad del proyecto

- **Producto:** Cheque Comercial, plataforma B2B fintech de compensación multilateral de deudas entre PyMEs. Detecta círculos de deuda cruzada (A debe a B, B debe a C, C debe a A) y los cancela por compensación de saldos sin que circule dinero. "Financiación sin dinero", en palabras del cliente.
- **Cliente:** Pampa y Puertos del Sur Consultores S.A., consultora económica y de negocios PyME. Marca registrada "Cheque Comercial". Equipo: Sergio F. Pucci (CEO), Rene Meder (CFO), Pablo Pucci (CMO), Alejandro Vitale (CTO). Asesores: Crowe y CONICET.
- **Proveedor:** Sawicki Fintech Solutions (nombre comercial de Mauricio Sawicki, full-stack developer, Neuquén, Argentina). Toda comunicación y documento se firma con el nombre comercial, nunca con el nombre personal ni con datos de contacto personales.
- **Relación:** cliente recurrente, hay confianza previa por proyectos anteriores. Se le entrega el flujo completo sin reservas de propiedad intelectual. Igual: NDA antes de recibir el algoritmo.
- **Repositorio:** `https://github.com/mausawicki7/cheque-comercial-fintech-b2b`
- **URL publicada (GitHub Pages):** `https://mausawicki7.github.io/cheque-comercial-fintech-b2b/`

---

## 2. Estado actual y qué hay en este repositorio

**Etapa:** pre-cotización. El cliente evalúa con sus socios el "Relevamiento y propuesta técnica del MVP" (PDF de 3 páginas enviado por nosotros). Como paso adicional, construimos un documento HTML de análisis funcional profundo para demostrar entendimiento antes de cotizar.

**Archivos:**

| Archivo | Qué es |
|---|---|
| `index.html` | Documento "Análisis funcional del MVP". Página única, CSS y JS inline, sin build. Se publica en Pages y se comparte como enlace privado. Arranca en una pantalla de acceso con PIN. |
| `logo-cheque-comercial-1.svg` | Logo del cliente tal como lo entregó. En el documento va inlineado como `<symbol id="logo-cc">`; el archivo queda como fuente. |
| `CLAUDE.md` | Este archivo. Trackeado y publicado en el repo, que es público. |

**Secuencia comercial acordada internamente:** (1) reunión de una hora con el cliente para recorrer `index.html` y cerrar las definiciones de la sección 8 → (2) firma de NDA → (3) propuesta comercial con alcance cerrado, etapas, plazos y costos → (4) inicio del desarrollo. Corrección respecto de la propuesta enviada: el NDA se propuso ahí como tercer paso; conviene adelantarlo, porque mientras no exista el cliente no entrega el algoritmo.

**Pantalla de acceso del documento.** `index.html` abre en una pantalla de PIN antes de la portada. `<html>` arranca con la clase `bloqueado`, que el script del `<head>` saca si `sessionStorage` ya tiene la marca de esta sesión, y que el formulario saca al validar el PIN. El PIN no está en texto plano en el fuente: se compara contra un hash FNV-1a. **Esto no es seguridad.** El documento entero viaja al navegador antes de pedir el PIN, y un PIN de cuatro dígitos se rompe por fuerza bruta al instante. Es una puerta de cortesía para que el enlace reenviado no se abra solo. Si en algún momento hace falta protección real, hay que servir el documento desde un backend con login. El valor del PIN no se escribe en este archivo porque el repositorio es público.

**Todavía no existe código de la plataforma.** Cuando exista, aplica la regla 4 de la sección 0.

---

## 3. Lo que sabemos del cliente (fuente: su deck de inversión, 11 slides)

- Posicionamiento: inclusión financiera para PyMEs, "funciona por fuera del mercado bancario", prescinde de préstamos, reduce costo financiero, "pagos asegurados por instituciones confiables", "pagos garantidos 100%".
- Mercado declarado: Argentina 605.600 PyMEs (0,28% con avales), Colombia 540.000 (16%), México 2.200.000 (16%).
- Estrategia de crecimiento: primer eslabón de cadenas de valor; convenios con cámaras, federaciones y grandes empresas ancla (mencionan YPF, Loma Negra, Profertil, ACA, Arcor); alianzas con SGRs y bolsas de comercio; producto viral por invitaciones de WhatsApp ("pagos asegurados a través de Cheque Comercial").
- Cuadrante competitivo propio: se ubican como "más confiable y menos costoso" frente a e-check, cheque bancario, pagaré digital, factura electrónica, Mercado Pago, Payoneer, BNA.
- Lo que declaran ya hecho: validación de idea, "Teorema de las PyMEs y su validación matemática" (con CONICET; presumiblemente el algoritmo de detección), diseño y registro de marca, user flow y manual de procedimientos, diseño y validación de UI, términos y condiciones legales, plan de negocio.
- Línea de tiempo declarada: 1S 2026 startup (estamos aquí) → 1T 2027: 6.700 PyMEs / USD 1,2 M → 1T 2028 Serie A y México: 35.000 PyMEs / USD 9 M → 2T 2029 Colombia: 200.000 / USD 50 M → 3T 2030 Brasil: 450.000 / USD 110 M. No está claro si los USD son facturación o volumen transaccionado.
- Mockup del deck: login con mail y contraseña, botón "Crear cuenta". Nada más de producto.

**Lectura crítica nuestra (no compartida con el cliente en estos términos):**
- El deck es de inversión, no de producto. No explica el mecanismo; lo dedujimos y el cliente no lo desmintió.
- "Funciona por fuera del mercado bancario" y el nombre "Cheque" (palabra regulada, Ley 24.452) son riesgos regulatorios y de naming. El encuadre BCRA/UIF/ARCA se dejó explícitamente a cargo del cliente y sus asesores.
- "Pagos garantidos 100%" depende de un acuerdo con SGR que hoy es solo una intención. Por eso el aval quedó fuera del MVP.
- La proyección de 6.700 PyMEs en 12 meses es incompatible con detección manual de cadenas; el motor automático es necesario para escalar.
- El 0,28% vs 16% de avales no parece la misma métrica.

---

## 4. Lo que propusimos (PDF "Relevamiento y propuesta técnica del MVP", sector retail)

- Plataforma web B2B cerrada, accesible desde navegador, sin app nativa. "Billetera virtual sin dinero físico".
- Flujo de cancelación: detección (admin o motor) → bloqueo temporal de las deudas en "pendiente de aprobación" → conformidad de cada PyME con Aceptar/Rechazar → si todas aceptan se ejecuta y se emiten comprobantes; si una rechaza o vence el plazo (ej. 24 h) la cadena se anula completa.
- Punto crítico marcado: cancelación parcial por el importe menor de la cadena con saldos remanentes vs. solo montos exactos. Cambia el modelo de datos.
- Cuatro pantallas propuestas: ingreso y registro / panel de la PyME / panel de administración / notificaciones y comprobantes. (Después reformulado como cuatro módulos con varias vistas cada uno, ver sección 5.)
- Infraestructura: servidor propio (VPS o cloud), administración como abono mensual separado del desarrollo.
- Definiciones pendientes planteadas al cliente: carga de deuda (unilateral vs. confirmada; alternativa: factura electrónica validada contra ARCA), cancelación parcial, vencimientos distintos y ajuste por plazo, validaciones de alta de empresa, respaldo legal del comprobante, grupo piloto.

---

## 5. Lo que fija el documento HTML (`index.html`)

Tesis de portada: *el valor de Cheque Comercial no está en la plataforma sino en la densidad de deudas cruzadas dentro de una misma cadena de valor; el software existe para encontrarlas y cancelarlas.* Consecuencia: el piloto tiene que ser vertical (una cadena de valor), no horizontal.

Once secciones: (1) cómo leímos el proyecto, cuadro "su presentación dice → el sistema tiene que" · (2) simulador interactivo del mecanismo con importes y vencimientos editables, interruptor parcial/exacto y flujo de conformidad; al detectar la cadena avisa que los vencimientos se muestran pero no se ajustan (supuesto 5) · (3) cuatro casos de uso con Martín, el zapatero del deck · (4) cuatro módulos con sus vistas y mockups HTML · (5) entidades y ciclo de vida de una deuda · (6) nueve casos borde con propuesta por defecto · (7) trece supuestos · (8) cuatro fases · (9) cómo trabajamos · (10) insumos que pedimos · (11) próximo paso.

**Módulos y vistas del MVP:**
- *Acceso y registro:* ingreso con segundo factor, registro con validación de CUIT y representante legal, ingreso por invitación con la deuda originante visible, aceptación auditable de términos, recuperación de contraseña.
- *Panel de la PyME:* resumen (debe / le deben / neto / en proceso), mis deudas y cobros, carga de deuda a cobrar con factura opcional y envío al deudor, confirmar o rechazar deudas cargadas a mi nombre, bandeja de propuestas con aceptar/rechazar, historial, invitar empresas.
- *Panel de administración:* empresas y su estado, todas las deudas con filtros, armado de cadenas (elegir deudas, ver compensable y remanentes, enviar), seguimiento de propuestas, deudas en disputa, indicadores.
- *Avisos y comprobantes:* correo electrónico por propuesta/resolución/vencimiento, comprobante PDF por empresa con número único, detalle y código de verificación. WhatsApp fuera del MVP; lo único WhatsApp del MVP es el enlace de invitación autogenerado que la empresa comparte desde su propio teléfono.

**Entidades:** Empresa (CUIT, razón social, representante, contacto, estado invitada/en validación/activa/suspendida, términos aceptados con versión, quién la invitó) · Deuda (deudor, acreedor, monto original inmutable, saldo vigente, vencimiento, origen/factura, estado, quién cargó y quién confirmó) · Cadena de compensación (deudas incluidas, importe compensable, plazo, respuesta de cada empresa con fecha/hora, estado propuesta/en espera/ejecutada/anulada/vencida, quién la armó) · Comprobante (número correlativo único, empresa titular, detalle, prueba de conformidad, sello de tiempo, código de verificación; inmutable).

**Ciclo de vida de una deuda:** Cargada → (deudor reconoce) Confirmada → (entra en cadena) Reservada → (todas aceptan) Compensada. Reservada vuelve a Confirmada si una rechaza, vence el plazo, o queda saldo remanente (con el saldo nuevo). Cargada → Rechazada si el deudor la desconoce. Solo se permiten estas transiciones.

**Reglas de negocio propuestas por defecto (sección 6 del HTML), pendientes de confirmación del cliente:**
1. Plazo vencido sin respuesta de una empresa: la cadena se anula, las que aceptaron reciben aviso con motivo, la que no respondió entra a lista de seguimiento; a la tercera falta queda marcada para revisión.
2. Dos cadenas compiten por la misma deuda: gana la que más deuda total extingue; empate → la de menos empresas.
3. Deuda cargada no reconocida: queda en Cargada, no entra a cadenas; el rechazo lleva motivo. Validación automática contra ARCA es fase posterior.
4. Deuda pagada por fuera mientras está reservada: cualquiera de las partes la marca "pagada por fuera", eso anula la cadena y avisa; el cierre definitivo requiere conformidad de la otra parte.
5. Vencimientos muy distintos en una cadena: en el MVP se compensa peso por peso mostrando vencimientos; ajuste por plazo/tasa es una regla posterior que hay que prever ahora.
6. Baja con deudas vigentes: la empresa pasa a suspendida (no carga ni entra en cadenas; sus deudas siguen visibles para las contrapartes); baja definitiva la aprueba el equipo cuando no queda nada pendiente.
7. Cuotas y notas de crédito: cada cuota es una deuda distinta; las notas de crédito reducen el saldo con conformidad de ambos y quedan en historial.
8. Deuda mutua entre dos empresas: es una cadena de dos y se trata igual (probablemente el caso más frecuente al inicio).
9. Visibilidad: cada empresa ve el detalle completo solo de sus propios tramos; de los demás ve que existen y fueron aceptados. Administración ve todo.

---

## 6. Fases

| Fase | Contenido | Disparador |
|---|---|---|
| **Piloto (MVP)** | Registro y validación, carga y confirmación de deudas, cadenas armadas desde administración, conformidad, compensación, comprobantes, avisos por correo | Primera cadena de valor, 20–50 empresas |
| **Motor** | Integración del algoritmo del cliente, detección automática, regla de prioridad entre cadenas | Datos reales del piloto + algoritmo recibido |
| **Integraciones** | Validación de facturas contra ARCA, aval SGR, invitaciones masivas desde empresa ancla, avisos por WhatsApp Business, reportes | Acuerdo con SGR, necesidad de escalar sin crecer el equipo |
| **Escala** | Varias cadenas en paralelo, preparación multi-moneda y multi-jurisdicción, acceso para contadores | Expansión regional del cliente |

Sin fechas ni precios en ningún documento hasta la propuesta comercial.

---

## 7. Supuestos vigentes (alcance por defecto)

Cada uno está en la sección 7 del HTML. Si el cliente desmiente alguno, actualizar acá y en `index.html`, y registrar en la bitácora.

1. Deudas en pesos argentinos únicamente.
2. Aval SGR fuera del MVP.
3. Cadenas armadas por el equipo del cliente desde administración en la fase piloto; motor automático después.
4. Compensación por el importe menor de la cadena, con saldos remanentes vigentes.
5. Peso por peso, sin ajuste por plazo ni tasa.
6. Toda deuda requiere confirmación del deudor antes de entrar en una cadena.
7. Plazo de respuesta 24 horas hábiles, configurable.
8. Piloto con una sola cadena de valor, 20–50 empresas, elegida por el cliente.
9. Web responsive desde navegador, instalable como acceso directo (PWA liviana), sin app nativa.
10. Todos los avisos de la plataforma salen por correo electrónico. La integración con WhatsApp Business (cuenta y plantillas de Meta) queda fuera del MVP. Lo único WhatsApp del MVP es el enlace de invitación autogenerado, que la empresa comparte por su cuenta.
11. Diseño de UI dentro del proyecto sobre las vistas del HTML, salvo que el cliente entregue uno validado.
12. Encuadre regulatorio y textos legales a cargo del cliente y sus asesores.
13. Comprobante digital con número único, sello de tiempo y código de verificación; firma digital con certificado es funcionalidad aparte.

---

## 8. Definiciones pendientes del cliente

Bloquean la cotización. Se cierran en la reunión de recorrido del HTML.

- [ ] Confirmar mecanismo (compensación multilateral por ciclos) tal como lo entendimos.
- [ ] Parcial por el menor vs. solo montos exactos.
- [ ] Carga con confirmación del deudor vs. unilateral; interés en validar factura contra ARCA.
- [ ] Cruce de deudas con distinto vencimiento; si aplica quita/tasa y cuál.
- [ ] Qué se valida al dar de alta una empresa; requisitos que surjan de UIF.
- [ ] Qué documento es el comprobante y qué respaldo legal necesita (firma electrónica, sello de tiempo).
- [ ] Cadena de valor del piloto: empresa ancla, cantidad de empresas, estimación de deudas cruzadas.
- [ ] Estado real del acuerdo con SGR.
- [ ] Si existe diseño de UI validado y si hay que respetarlo.
- [ ] Rol del CTO del cliente (Alejandro Vitale): decisor técnico o no. Cambia responsabilidad y precio.
- [ ] Respuesta a cada una de las nueve reglas por defecto de la sección 5.

**Insumos pedidos:** manual de procedimientos y user flow · términos y condiciones · diseño de UI si existe · definición del piloto · respuestas a los casos borde · algoritmo (post-NDA, para fase Motor).

---

## 9. Decisiones técnicas ya tomadas para la plataforma futura

Solo lo que está decidido. Lo demás se decide con ADR cuando arranque el desarrollo.

- **Stack:** React + Next.js + TypeScript en frontend y backend (API routes / server actions), PostgreSQL como base de datos (Supabase es opción preferida por experiencia del equipo; decisión final por ADR). Herramientas estándar, nada que dependa de una persona.
- **Principios no negociables para un sistema financiero:**
  - El monto original de una deuda nunca se edita; todo cambio de saldo es un movimiento nuevo con autor, fecha y motivo.
  - Los comprobantes son inmutables y correlativos.
  - Toda acción relevante deja rastro auditable (quién, qué, cuándo, desde dónde).
  - Ninguna deuda cambia de estado sin conformidad expresa de las partes involucradas.
  - Una deuda no puede estar en dos cadenas activas a la vez (bloqueo pesimista o transaccional a nivel base de datos, no solo en la aplicación).
  - Doble factor obligatorio para administración; recomendado para PyMEs.
  - Cifrado en tránsito y en reposo; secretos fuera del repositorio.
  - Backups diarios automáticos con prueba de restauración periódica.
- **Infraestructura:** servidor propio (VPS o cloud), administrado por nosotros, propiedad y datos del cliente. Costo mensual separado del desarrollo.
- **Entregas:** ambiente de prueba con usuario del cliente, algo nuevo funcionando cada dos semanas.
- **Decisiones de negocio por escrito** en un documento compartido con fecha.

---

## 10. Convenciones del repositorio (vigentes ahora)

- `index.html` es un único archivo autocontenido. No separar CSS/JS, no agregar bundlers ni frameworks al documento.
- Español rioplatense, voseo, tono profesional y directo. Nada técnico sin traducir a consecuencia de negocio.
- Sin precios, plazos, fechas de entrega ni datos de contacto personales en el documento.
- Empresas y montos ficticios coherentes en todo el documento: Calzados Martín debe 100.000 a Curtiembre del Sur; Curtiembre del Sur debe 60.000 a Transportes Alvear; Transportes Alvear debe 80.000 a Calzados Martín; compensable 60.000; remanentes 40.000 y 20.000. Distribuidora Rivera y Química Patagonia como empresas secundarias.
- Colores en variables CSS en `:root`. Azul de marca `--azul: #0A72E2`. Verde solo para compensado/aceptó, rojo solo para rechazó/anulado, ámbar para pendiente/reservada.
- Logo del cliente inlineado como `<symbol id="logo-cc">`, usa `currentColor`.
- Mantener `<meta name="robots" content="noindex, nofollow">`.
- Commits en español, formato Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`), asunto en imperativo, máximo 72 caracteres.
- **Ningún commit, mensaje de commit, push, pull request ni archivo del repositorio menciona a Claude, Claude Code ni Anthropic.** No agregar líneas `Co-Authored-By: Claude`, ni pies "Generated with Claude Code", ni comentarios en el código que indiquen que fue generado por una IA. Los commits se firman únicamente con el autor configurado en git del titular. Esta regla no admite excepciones. La única mención permitida es dentro de este archivo, que se mantiene fuera del repositorio remoto: `CLAUDE.md` va en `.gitignore` y vive solo en la copia local.
- Después de cualquier cambio en `index.html`: recorrer el simulador completo (detectar, aceptar las tres, rechazar una, modo exacto, reiniciar), verificar el índice lateral, revisar a 380 px de ancho.

---

## 11. Operación con GitHub y Pages

Primera publicación:

```bash
git init -b main
printf 'CLAUDE.md\n' > .gitignore
git add index.html .gitignore
git commit -m "docs: análisis funcional del MVP de Cheque Comercial"
git remote add origin https://github.com/mausawicki7/cheque-comercial-fintech-b2b.git
git push -u origin main
```

Activar Pages con la CLI (requiere `gh auth login` previo):

```bash
gh api -X POST repos/mausawicki7/cheque-comercial-fintech-b2b/pages \
  -f source[branch]=main -f source[path]=/
```

O manualmente: Settings → Pages → Deploy from a branch → `main` / `(root)`.

Actualizaciones: `git add -A && git commit -m "..." && git push`. Pages redespliega en uno o dos minutos. Verificar en la URL publicada desde un celular.

Nunca commitear credenciales, tokens, archivos del cliente (PDFs, algoritmo, T&C) ni datos reales de empresas. Los insumos del cliente se guardan fuera del repositorio o en una carpeta ignorada por git.

---

## 12. Bitácora de decisiones

Formato: `AAAA-MM-DD · decisión · motivo`. Una línea por decisión. Las más recientes arriba.

- 2026-09-13 · Pantalla de acceso con PIN antes del documento · pedido del titular; se le advirtió que en una página estática es una puerta de cortesía y no seguridad, porque el contenido llega al navegador antes de pedir el PIN.
- 2026-09-13 · WhatsApp fuera del MVP: todos los avisos salen por correo electrónico; el MVP solo genera un enlace de invitación que la empresa comparte por WhatsApp desde su propio teléfono; la integración con WhatsApp Business pasa a la fase Integraciones · pedido del titular; la cuenta y las plantillas de Meta tienen tiempos de aprobación que no manejamos y no pueden condicionar el piloto.
- 2026-09-13 · Ninguna referencia a Claude, Claude Code ni Anthropic en commits, pushes ni archivos del repositorio · pedido del titular.
- 2026-09-13 · Se quitan mail y LinkedIn del documento; firma solo "Sawicki Fintech Solutions" · pedido del titular.
- 2026-09-13 · Publicación en GitHub Pages en repo `cheque-comercial-fintech-b2b` con `noindex` · pedido del titular; se advirtió que el nombre del repo expone la marca en la URL.
- 2026-09-13 · Documento HTML completo con once secciones y simulador interactivo · demostrar entendimiento profundo antes de cotizar; el cliente es de confianza y se le entrega el flujo completo.
- 2026-09-13 · Aval SGR queda fuera del MVP como supuesto explícito · depende de un acuerdo que no existe; evita que el cliente lo asuma incluido en el abono.
- 2026-09-13 · Motor automático separado en fase 2; el piloto opera con cadenas manuales · el algoritmo es del cliente y no lo entregan antes del NDA; el piloto genera los datos que el motor necesita.
- 2026-09-13 · Se agrega la regla de visibilidad entre empresas de una cadena · no estaba en la propuesta original y va a surgir en la reunión.
- 2026-09-13 · Recomendación interna: adelantar el NDA al paso inmediato posterior a la reunión · destraba el algoritmo y protege el análisis ya entregado.
- 2026-09-12 · Propuesta técnica PDF enviada al cliente con definiciones pendientes en vez de precio · el alcance no se puede cotizar sin las definiciones de negocio.

---

## 13. Documentación estructurada (se inicializa al alcanzar el umbral de la sección 0)

Cuando este archivo supere ~400 líneas o el repositorio pase a contener código de la plataforma, crear la siguiente estructura y migrar el contenido. Conserva las convenciones actuales para plataformas B2B fintech: decisiones trazables, modelo de datos y estados documentados, seguridad y cumplimiento como documentos de primera clase, operación reproducible.

```
docs/
├── README.md                  Índice y mapa de la documentación, cómo navegarla
├── 00-contexto/
│   ├── negocio.md             Cliente, producto, tesis, mercado, estrategia (secciones 1 y 3 de este archivo)
│   ├── glosario.md            Términos del dominio: deuda, cadena, compensación, remanente, reserva, comprobante...
│   └── stakeholders.md        Personas, roles y canales de decisión del lado del cliente
├── 01-producto/
│   ├── alcance-mvp.md         Módulos, vistas, qué entra y qué no
│   ├── casos-de-uso/          Un archivo por caso de uso, formato situación / actor / sistema / resultado
│   ├── reglas-de-negocio.md   Las reglas numeradas, con estado: propuesta / confirmada / descartada
│   ├── casos-borde.md         Situaciones límite y su resolución
│   ├── supuestos.md           Supuestos vigentes con fecha de confirmación
│   └── roadmap.md             Fases y disparadores, sin fechas comprometidas salvo contrato
├── 02-arquitectura/
│   ├── vision.md              Diagrama de contexto y contenedores (modelo C4 niveles 1 y 2)
│   ├── modelo-de-datos.md     Entidades, relaciones, diccionario de datos, campos inmutables
│   ├── maquinas-de-estado.md  Estados y transiciones permitidas de Deuda, Cadena, Empresa, Comprobante
│   ├── motor-compensacion.md  Detección de ciclos, prioridad entre cadenas, integración del algoritmo del cliente
│   ├── integraciones.md       ARCA, correo, SGR, sello de tiempo (WhatsApp Business API, fase posterior)
│   └── api.md                 Contrato de la API (OpenAPI cuando exista)
├── 03-decisiones/             ADRs: un archivo por decisión, numerados
│   ├── 0001-stack.md
│   ├── 0002-base-de-datos.md
│   └── plantilla.md           Contexto / decisión / alternativas / consecuencias / estado
├── 04-seguridad-y-cumplimiento/
│   ├── modelo-de-amenazas.md  Activos, actores, vectores, mitigaciones
│   ├── autenticacion-y-roles.md  2FA, RBAC, sesiones, permisos por vista
│   ├── auditoria.md           Qué se registra, dónde, cuánto tiempo, quién accede
│   ├── datos-personales.md    Ley 25.326, retención, derechos de los titulares
│   ├── regulatorio.md         Lo que el cliente y sus asesores definan (BCRA, UIF, ARCA); nosotros solo implementamos
│   └── gestion-de-secretos.md
├── 05-operaciones/
│   ├── entornos.md            Desarrollo, prueba, producción; quién accede a cada uno
│   ├── despliegue.md          Procedimiento reproducible paso a paso
│   ├── backups-y-restauracion.md  Frecuencia, ubicación, prueba de restauración
│   ├── monitoreo-y-alertas.md
│   └── runbooks/              Un archivo por incidente conocido: qué mirar, qué hacer
├── 06-calidad/
│   ├── estrategia-de-pruebas.md  Unitarias, integración, extremo a extremo; escenarios obligatorios del motor
│   └── checklist-de-entrega.md   Qué se verifica antes de cada entrega quincenal
└── 07-comercial/
    ├── historial-de-propuestas.md  Qué se envió, cuándo, respuesta
    └── acuerdos.md            Alcance contratado, cambios de alcance aprobados, con fecha
CHANGELOG.md                   Cambios por versión, formato Keep a Changelog, versionado semántico
```

Reglas de la documentación estructurada:
- Cada decisión de arquitectura o de negocio con impacto técnico se registra como ADR antes de implementarse. Los ADR no se editan una vez aceptados: se reemplazan por uno nuevo que los supersede.
- `reglas-de-negocio.md` y `supuestos.md` llevan estado y fecha; son la fuente de verdad para cotizar cambios de alcance.
- Las máquinas de estado se documentan antes de programarse y el código las implementa tal cual; cualquier transición nueva pasa primero por el documento.
- La documentación de seguridad y cumplimiento se revisa en cada fase, no solo al inicio.
- CLAUDE.md, después de la migración, no supera las 120 líneas: identidad, reglas de trabajo, mapa de `docs/`, y enlaces a las diez decisiones más importantes.
- Nada del cliente (PDFs, algoritmo, T&C, datos reales) entra en `docs/` en un repositorio público. Si el repositorio sigue siendo público, los documentos con información sensible se mantienen en un repositorio privado separado y `docs/README.md` lo indica.