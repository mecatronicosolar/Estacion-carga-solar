# P01 — Divisor resistivo V_PV (caracterización de la atenuación)

**Objetivo:** Verificar que el divisor reduce la tensión nominal del sistema a un rango compatible con el ADS1015 (0–3.3 V).

**Equipamiento:**
- Fuente de laboratorio variable (0–30 V)
- Divisor resistivo armado en protoboard
- Multímetro digital
- Osciloscopio (para verificar ruido)

**Paso a paso:**

1. Monta el divisor en el protoboard: R₁ serie de la fuente, R₂ a tierra.
2. Alimenta con tensiones de prueba:
   - 0 V → medida en el divisor: ~0 V (referencia)
   - 12 V → medida esperada: V_ADC = 12 × R₂/(R₁+R₂)
   - 24 V → medida esperada: V_ADC = 24 × R₂/(R₁+R₂)
   - 30 V (máximo) → medida: debe ser ≤ 3.2 V para evitar daño al ADS1015
3. Con el osciloscopio: mide ripple AC y ruido en la salida del divisor.
4. Repite con 10 muestras por tensión para caracterizar linealidad.

**Criterios de aceptación:**
- Linealidad: desviación ≤ 2 % de la ecuación teórica
- Ruido: ripple AC < 50 mV pico a pico
- Rango: V_ADC máximo a 30 V < 3.2 V

**Resultado esperado:**

| V_entrada (V) | V_ADC medida (V) | V_ADC teórica (V) | Error % |
|---|---|---|---|
| 0 | 0.00 | 0.00 | — |
| 12 | ? | ? | |
| 24 | ? | ? | |
| 30 | ? | ? | |

**Nota:** Los valores exactos de R₁ y R₂ dependen del diseño; si aún no los tienes, usa resistencias de prueba y luego escala.

---
