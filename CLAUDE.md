# Cheque Comercial · Análisis funcional del MVP

Documento HTML de una sola página, preparado por Sawicki Fintech Solutions para el cliente Cheque Comercial (Pampa y Puertos del Sur Consultores S.A.). Se publica en GitHub Pages y se comparte como enlace privado con los socios del cliente, en fase de pre-cotización.

## Qué es este proyecto

- Un único archivo: `index.html`. Sin build, sin dependencias, sin framework. CSS y JS van inline.
- Audiencia: socios del cliente, perfil de consultoría económica, no técnico. Lo leen sobre todo desde el celular.
- Idioma: español rioplatense, voseo, tono profesional y directo. Nunca lenguaje técnico sin traducir a consecuencia de negocio.
- Única dependencia externa: la tipografía Source Sans 3 desde Google Fonts, con fallback al sistema.

## Estructura del documento

Portada (azul, tesis del proyecto) y once secciones numeradas:

1. Cómo leímos el proyecto — cuadro "su presentación dice → el sistema tiene que"
2. El mecanismo, en vivo — simulador interactivo de compensación (SVG + JS vanilla)
3. Martín usa Cheque Comercial — cuatro casos de uso con mockups
4. Las pantallas — cuatro módulos con sus vistas, más tres mockups
5. Cómo se guarda la información — entidades en lenguaje de negocio y ciclo de vida de una deuda
6. Preguntas que el sistema tiene que saber contestar — nueve casos borde con propuesta por defecto
7. Supuestos de trabajo — trece supuestos que definen el alcance
8. Fases — piloto, motor, integraciones, escala
9. Cómo trabajamos
10. Qué necesitamos de ustedes
11. Próximo paso

Los mockups son HTML/CSS (clases `.tel` para celular, `.escr` para escritorio), no imágenes. El logo del cliente está inlineado como `<symbol id="logo-cc">` y se usa con `<use href="#logo-cc"/>`; toma el color de `currentColor`.

## Reglas al editar

- Mantener todo en `index.html`. No separar CSS ni JS en archivos aparte, no agregar bundlers ni frameworks.
- No agregar contenido técnico extenso: el stack se nombra en una línea en la sección 9 y nada más.
- No incluir precios, plazos ni fechas de entrega. Eso va en la propuesta comercial, que es otro documento.
- No incluir datos de contacto personales. La firma es "Sawicki Fintech Solutions".
- Las empresas y montos de los ejemplos son ficticios: Calzados Martín, Curtiembre del Sur, Transportes Alvear, Distribuidora Rivera. Mantener la coherencia de nombres y montos entre el simulador, los casos de uso y los mockups (100.000 / 60.000 / 80.000, compensable 60.000).
- Los colores viven en variables CSS en `:root`. El azul de marca es `--azul: #0A72E2`. Verde solo para "compensado/aceptó", rojo solo para "rechazó/anulado", ámbar para "pendiente/reservada".
- Mantener `<meta name="robots" content="noindex, nofollow">`.
- Verificar después de cada cambio que el simulador sigue funcionando: detectar cadena, aceptar las tres empresas, rechazar una, modo "solo montos exactos", volver a empezar.

## Publicación en GitHub Pages

Repositorio: `https://github.com/mausawicki7/cheque-comercial-fintech-b2b`
URL publicada: `https://mausawicki7.github.io/cheque-comercial-fintech-b2b/`

Primera vez:

```bash
git init -b main
git add index.html CLAUDE.md
git commit -m "Análisis funcional MVP Cheque Comercial"
git remote add origin https://github.com/mausawicki7/cheque-comercial-fintech-b2b.git
git push -u origin main
```

Luego activar Pages en Settings → Pages → Deploy from a branch → `main` / `(root)`. Si la CLI `gh` está instalada y autenticada, se puede hacer desde la terminal:

```bash
gh api -X POST repos/mausawicki7/cheque-comercial-fintech-b2b/pages -f source[branch]=main -f source[path]=/
```

Actualizaciones posteriores:

```bash
git add index.html
git commit -m "Descripción del cambio"
git push
```

Pages redespliega solo en uno o dos minutos.

## Verificación antes de publicar

- Abrir `index.html` en el navegador de escritorio y en un celular (o con el modo responsive del navegador a 380px).
- Recorrer el simulador completo.
- Comprobar que el índice lateral resalta la sección activa al hacer scroll.
- Buscar que no queden textos de trabajo, placeholders ni datos de contacto.

## Contexto de negocio para tener presente

- Cheque Comercial es una plataforma B2B de compensación multilateral de deudas entre PyMEs: detecta círculos de deuda cruzada y los cancela sin que circule dinero.
- El cliente ya trabajó con nosotros; hay confianza. Este documento existe para demostrar entendimiento profundo antes de cotizar.
- Decisiones de alcance que este documento fija por defecto: aval SGR fuera del MVP, cadenas armadas manualmente desde administración en la fase piloto, compensación por el importe menor con saldos remanentes, confirmación del deudor obligatoria, avisos por correo primero y WhatsApp después.
- El algoritmo de detección es del cliente y lo entrega para la fase "Motor"; el sistema tiene que quedar preparado para recibirlo.
