# P09 — PZEM-016 con Transformador de Corriente (CT)

**Objetivo:** Verificar que el transformador de corriente se acopla correctamente y el PZEM mide corriente sin romper el circuito.

**Equipamiento:**
- PZEM-016 + CT PZCT-02
- Fuente AC 110 V 60 Hz
- Carga AC variable (0–10 A)
- Pinza amperimétrica de referencia

**Paso a paso:**

1. Instala el CT en serie con el conductor de fase:
   ```
   Red AC ─[RCCB]─[CT PZCT-02]─[Carga]─ Retorno
                  ↓ (enrollado)
                  PZEM-016 (entrada de corriente)
   ```

2. Energiza sin carga: I debe leer 0.00–0.05 A.

3. Aplica cargas progresivas: 2 A, 4 A, 6 A, 8 A, 10 A.

4. Para cada carga, mide:
   - Corriente con pinza amperimétrica
   - Corriente con PZEM-016
   - Error porcentual

5. Verifica energía acumulada:
   - Déjalo funcionando 5 minutos a carga constante
   - Lee el acumulador de energía (E)
   - Compara: E esperada = P × t / 1000

**Criterios de aceptación:**
- Acoplamiento CT: instalación sin romper circuito ✓
- Linealidad: R² > 0.98 de I vs. carga
- Precisión: error ≤ 2 % en rango 2–10 A
- Energía: E medida ± 3 % de E esperada

**Resultado esperado:**

| Carga nominal (A) | I_PZEM (A) | I_pinza (A) | Error % |
|---|---|---|---|
| 0 | 0.02 | 0.01 | — |
| 2 | 1.98 | 2.00 | −1.0 |
| 4 | 3.97 | 4.00 | −0.75 |
| 6 | 5.98 | 6.00 | −0.33 |
| 8 | 8.04 | 8.00 | +0.5 |
| 10 | 9.98 | 10.00 | −0.2 |

---
