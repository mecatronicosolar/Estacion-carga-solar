
# P03 — Shunt FL-2 (caracterización diferencial I_BAT)

**Objetivo:** Verificar que la caída diferencial del shunt sea proporcional a la corriente y sin asimetrías.

**Equipamiento:**
- Fuente de corriente de laboratorio (0–50 A DC) O módulo de carga resistiva
- Pinza amperimétrica de referencia
- Multímetro digital en modo de diferencial (si lo permite) o dos multímetros
- Osciloscopio (para medir la diferencia de tensión)

**Paso a paso:**

1. Arma el shunt en serie en el protoboard:
   - Terminal de entrada: desde la fuente
   - Terminal de salida: hacia la carga
   - Conecta dos puntas del osciloscopio a cada lado del shunt en modo diferencial (CH1 − CH2).

2. Aplica corrientes crecientes: 5 A, 10 A, 15 A, 20 A, 25 A, 30 A (hasta donde permita tu fuente).

3. Para cada corriente, mide:
   - Tensión diferencial V_shunt (multímetro o osciloscopio en modo diferencial)
   - Corriente real (pinza amperimétrica)
   - Temperatura del shunt (termómetro infrarrojo, si es posible)

4. Repite en ambas direcciones si tu fuente lo permite (corriente positiva y negativa).

**Criterios de aceptación:**
- Relación lineal: ΔV = I × 1.5 mV/A (caída de 75 mV a 50 A)
- Linealidad R² > 0.99
- Asimetría: diferencia < 2 % entre corriente positiva y negativa
- Temperatura: no debe exceder 60 °C a 30 A continuo

**Resultado esperado:**

| I (A) | V_shunt (mV) | V_shunt teórica (mV) | Error % |
|---|---|---|---|
| 0 | 0 | 0 | — |
| 10 | ? | 15 | |
| 20 | ? | 30 | |
| 30 | ? | 45 | |

---
