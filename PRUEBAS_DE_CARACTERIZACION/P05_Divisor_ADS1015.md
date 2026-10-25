
# P05 — Integración ADS1015 + divisor resistivo (V_PV)

**Objetivo:** Verificar que el divisor + ADS1015 forman una cadena lineal de medición sin pérdida de precisión.

**Equipamiento:**
- Resultado de Prueba 1.1 (divisor caracterizado)
- ADS1015 operativo (Prueba 1.4)
- Fuente de laboratorio 0–30 V
- Script Python con lectura de ADS1015

**Paso a paso:**

1. Conecta el divisor directamente a AIN0 del ADS1015:
   - Salida divisor → AIN0
   - GND divisor → GND ADS1015

2. Aplica tensiones en el lado de entrada del divisor: 0 V, 12 V, 24 V, 30 V.

3. Para cada tensión, lee el ADS1015 100 veces y calcula:
   - Promedio de código
   - Desviación estándar
   - Tensión en el divisor (multímetro)
   - Tensión equivalente en entrada (usando la relación del divisor)

4. Grafica: código ADC vs. tensión de entrada esperada.

**Criterios de aceptación:**
- Linealidad R² > 0.98
- Error en la medición < 3 % respecto a multímetro
- Ruido (σ) < 20 códigos

**Resultado esperado:**

| V_entrada (V) | Código ADC | V_ADC calculada | Error % |
|---|---|---|---|
| 0 | 0 | 0.000 | — |
| 12 | ? | ? | |
| 24 | ? | ? | |
| 30 | ? | ? | |

**Fórmula de conversión final:**

```
Código ADC → Tensión en divisor → Tensión en entrada
V_entrada = Código_ADC × (4.096 / 4095) × (R₁ + R₂) / R₂
```

---
