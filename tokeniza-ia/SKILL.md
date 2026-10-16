---
name: tokeniza-ia
description: Mide tokens verificables y estima, escala y compara costos de API de modelos de IA. Úsalo para presupuestos de prompts, respuestas y automatizaciones; no para precios de suscripciones.
metadata:
  short-description: Tokenización y costos de API verificables
---

# Tokeniza-IA | Estimación y escalamiento de costos de modelos de IA

Calcula y compara el costo de una interacción de IA y su escalamiento. El resultado debe ser auditable: cada total debe distinguir datos medidos, estimaciones, supuestos y límites.

## Cuándo usarlo

Úsalo cuando se soliciten conteos de tokens, costos por llamada, proyecciones de volumen, comparación de modelos o sensibilidad de costos. No lo uses para evaluar la calidad de un modelo únicamente por su precio, ni para precios de planes ChatGPT/Codex o suscripciones.

## Prevuelo obligatorio

Antes de calcular, inventaría estas entradas:

- Texto de entrada exacto y, para una respuesta prevista, texto de salida exacto o una muestra representativa.
- Escenarios solicitados. Para cuatro combinaciones corta/larga, deben existir y estar identificados los cuatro textos: pregunta corta, pregunta larga, respuesta corta y respuesta larga. No clasifiques ni redactes variantes sin declararlo como supuesto y obtener aceptación cuando cambie el alcance.
- Modelos y modalidades de precio: tokens de entrada, salida, entrada en caché y cualquier cargo fijo por solicitud.
- Fuente de tarifa, fecha de consulta, moneda/unidad y volumen de llamadas.
- Método de tokenización compatible con cada modelo y si el contexto compartido puede usar caché.

Si falta un precio, un conteo de tokens, la conversión entre unidades, o una definición que altere materialmente el resultado, no completes el cálculo con una cifra inventada. Explica qué falta y solicita el dato o autorización para usar una alternativa verificable.

## Fuentes y medición

1. Usa el tokenizer del proveedor o una codificación explícitamente compatible con el modelo. Registra el nombre del tokenizer/codificación y el texto medido.
2. Cuando el usuario indique fuentes concretas, consúltalas. Para precios actuales, verifica la vigencia antes de calcular; registra URL, fecha y tarifa usada.
3. No transfieras automáticamente el conteo de un tokenizer de OpenAI a modelos de Anthropic, Google u otros proveedores. Si se usa como aproximación para comparar, etiquétalo como **estimación no facturable**.
4. No conviertas USD, créditos u otras unidades sin una equivalencia proporcionada o publicada por una fuente verificable. Conserva la unidad original si no existe conversión.
5. No mezcles precio de API con créditos de producto, suscripciones, herramientas de terceros ni precios por asiento.

Lee [la guía de cálculo](references/calculo-y-reporte.md) antes de construir tablas o conclusiones.

## Flujo de trabajo

1. Identifica propósito, alcance, modelos, escenarios, moneda y volúmenes.
2. Mide o recopila los tokens de entrada y salida de cada escenario. Separa tokens reales de una salida de prueba de una proyección de salida.
3. Obtén y valida las tarifas por millón de tokens y los cargos por solicitud. Verifica unidades, fecha y si la tarifa de caché aplica.
4. Calcula el costo unitario, después escala a los volúmenes solicitados. Mantén precisión interna y redondea sólo la presentación.
5. Compara modelos con una base porcentual declarada y calcula la contribución de cada componente del costo.
6. Realiza comprobaciones de consistencia y reporta fuentes, supuestos, advertencias y límites antes de recomendar.

## Salida mínima

Entrega, como mínimo:

- tabla de entradas verificadas: texto/escenario, tokens, tarifa, fuente, fecha, unidad y estado de evidencia;
- tabla de costo por llamada y de escala para 1,000, 10,000 y 1,000,000 llamadas, salvo que el usuario pida otros volúmenes;
- fórmula sustituida con los valores de cada escenario;
- diferencias absolutas y porcentuales entre modelos, con denominador explícito;
- variable dominante calculada por contribución monetaria, no por intuición;
- supuestos, información por verificar y límites;
- recomendación de automatización y controles de costo que distinga el ahorro de precio de la idoneidad de calidad.

Cuando las tarifas no permitan una tabla completa, entrega una plantilla con celdas pendientes y detente antes de declarar totales o una recomendación numérica.

## Control de calidad

Antes de finalizar, confirma que cada fila usa la misma moneda/unidad, que los millones de tokens no se trataron como tokens individuales, que no se duplicó la entrada en caché, que los volúmenes multiplican el costo unitario correcto y que toda cifra externa tiene fuente y fecha. Distingue siempre **dato**, **inferencia**, **supuesto** y **límite**.
