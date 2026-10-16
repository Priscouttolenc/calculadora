# Guía de cálculo y reporte

## Variables y fórmulas

Para un escenario `s` y modelo `m`:

| Símbolo | Significado | Unidad |
|---|---|---|
| `I` | Tokens de entrada no almacenados en caché | tokens |
| `C` | Tokens de entrada en caché facturables | tokens |
| `O` | Tokens de salida | tokens |
| `Pi`, `Pc`, `Po` | Precio de entrada, caché y salida | moneda por 1M tokens |
| `F` | Cargo fijo por solicitud, si existe | moneda por llamada |
| `N` | Número de llamadas | llamadas |

`Costo_por_llamada = (I × Pi + C × Pc + O × Po) / 1,000,000 + F`

`Costo_total(N) = Costo_por_llamada × N`

Aplica la fórmula sólo a los componentes cobrados. Si la tarifa publicada ya desglosa la entrada en caché, no sumes los mismos tokens como entrada normal. Si no hay tarifa de caché publicada o no se conoce el uso de caché, usa `C = 0` únicamente cuando se declare que no se está modelando caché; de lo contrario deja el resultado pendiente.

## Cuatro escenarios de texto

Cuando se pidan combinaciones de longitud, nómbralas sin ambigüedad:

| Escenario | Entrada | Salida |
|---|---|---|
| S1 | Pregunta corta | Respuesta corta |
| S2 | Pregunta corta | Respuesta larga |
| S3 | Pregunta larga | Respuesta corta |
| S4 | Pregunta larga | Respuesta larga |

Mide cada texto exacto por separado. Si falta una de las cuatro piezas, no derives su tokenización a partir de la etiqueta “corta” o “larga”. Solicita el texto o presenta una plantilla pendiente.

## Estructura de reporte sugerida

### 1. Evidencia de entrada

| Escenario | Texto/versionado | Tokens de entrada | Tokens de salida | Método de medición | Estado |
|---|---|---:|---:|---|---|

Usa `medido`, `estimado`, `pendiente` o `no comparable`. Un texto de salida generado por una prueba es un dato de esa prueba, no un pronóstico universal.

### 2. Tarifas verificadas

| Modelo | Entrada/1M | Caché/1M | Salida/1M | Cargo fijo | Moneda | Fuente | Fecha de consulta |
|---|---:|---:|---:|---:|---|---|---|

Si se solicita una tabla de modelos de distintos proveedores, conserva la tarifa API de cada proveedor y marca cualquier equivalencia de tokens como estimación. Nunca presentes créditos de una plataforma como USD de API sin una conversión verificable.

### 3. Costos y escalamiento

| Modelo | Escenario | Costo por llamada | × 1,000 | × 10,000 | × 1,000,000 |
|---|---|---:|---:|---:|---:|

Muestra al menos un cálculo sustituido, por ejemplo:

`((I × Pi) + (C × Pc) + (O × Po)) / 1,000,000 + F = costo por llamada`

Presenta suficientes decimales para reproducir el cálculo; puedes redondear la tabla ejecutiva, indicando la regla de redondeo.

### 4. Comparación y sensibilidad

Para un modelo A frente a una referencia B:

`Diferencia_absoluta = Costo_A − Costo_B`

`Diferencia_%_vs_B = (Costo_A − Costo_B) / Costo_B × 100`

Si `Costo_B` es cero, no calcules porcentaje; informa que el denominador no permite comparación porcentual.

Calcula las contribuciones monetarias:

- Entrada: `I × Pi / 1,000,000`
- Caché: `C × Pc / 1,000,000`
- Salida: `O × Po / 1,000,000`
- Cargo fijo: `F`

La variable dominante es el componente con la mayor contribución al costo por llamada. Si se comparan escenarios, reporta si la dominancia cambia con la longitud de entrada o salida.

## Recomendación de automatización y controles

La recomendación debe ser proporcional a la evidencia:

- Estandariza prompts y limita la salida con esquemas, longitud máxima o respuestas estructuradas cuando la salida domine el costo.
- Reutiliza contexto/caché sólo si el proveedor, el flujo y la tarifa lo soportan; mide la tasa real de aciertos de caché.
- Enruta tareas de baja complejidad a un modelo más económico sólo tras validar calidad, tasa de errores y escalamiento.
- Configura presupuesto, alertas por gasto, cuotas, etiquetas por flujo y muestreo de uso real. Recalibra con tokens facturados, no sólo con muestras.

No afirmes que el modelo más barato es “mejor” si no existen métricas de calidad, latencia, seguridad y tasa de éxito para el caso de uso.

## Lista de validación

- [ ] Texto y escenario identificados sin ambigüedad.
- [ ] Tokenizer/codificación compatible y método declarado.
- [ ] Precios de entrada, salida, caché y cargos fijos verificados o marcados pendientes.
- [ ] Moneda/unidad consistente; conversión documentada si aplica.
- [ ] Fórmula, volumen y redondeo reproducibles.
- [ ] Comparación porcentual con referencia indicada.
- [ ] Afirmaciones separadas en dato, inferencia, supuesto y límite.
