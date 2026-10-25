# P12 — ADS1015 + ACS758 + Divisor resistivo (cadena completa de DC)

**Objetivo:** Verificar que juntos miden correctamente V_PV, I_PV, V_BAT, I_BAT sin interferencia.

**Equipamiento:**
- Todos los sensores del BLOQUE 1
- Fuente variable 0–30 V, corriente 0–10 A
- Script Python para leer simultáneamente todos los canales

**Paso a paso:**

1. Monta ambos ADS1015 en protoboard:
   ```
   ADS1015 Panel (0x49):
   - AIN0 ← Divisor V_PV
   - AIN1 ← ACS758 (I_PV)
   
   ADS1015 Batería (0x48):
   - AIN0 ← Divisor V_BAT
   - AIN1 ← Shunt FL-2 (I_BAT)
   ```

2. Energiza la cadena:
   - Fuente → Divisor V_PV (entrada) + ACS758 (entrada)
   - Salida ACS758 → Shunt FL-2
   - Shunt → Carga resistiva

3. Aplica corriente progresiva: 0, 2, 4, 6, 8, 10 A.

4. Lee simultáneamente los 4 canales:
   ```python
   import smbus
   
   bus = smbus.SMBus(1)
   ads_panel = 0x49
   ads_battery = 0x48
   
   for _ in range(10):
       # Lee ADS Panel
       v_pv = bus.read_word_data(ads_panel, 0x00) 
       i_pv = bus.read_word_data(ads_panel, 0x01)
       
       # Lee ADS Battery
       v_bat = bus.read_word_data(ads_battery, 0x00)
       i_bat = bus.read_word_data(ads_battery, 0x01)
       
       print(f"V_PV={v_pv*4.096/4095:.2f}V, I_PV={...}, V_BAT={...}, I_BAT={...}")
   ```

5. Verifica coherencia:
   - I_PV debe aumentar conforme aumenta la corriente
   - V_PV debe ser constante (tensión entrada)
   - V_BAT y I_BAT deben correlacionarse (P_BAT = V_BAT × I_BAT)

**Criterios de aceptación:**
- Todos los canales comunicando sin errores I²C
- Linealidad R² > 0.98 en cada uno
- Sin crosstalk entre canales (variación < 2 %)

**Resultado esperado:**

| Corriente aplicada (A) | V_PV (V) | I_PV (A) | V_BAT (V) | I_BAT (A) | P_BAT (W) |
|---|---|---|---|---|---|
| 0 | 24.0 | 0.01 | 24.0 | 0.02 | 0.5 |
| 2 | 24.0 | 2.01 | 24.0 | 2.03 | 48.7 |
| 4 | 24.0 | 4.02 | 24.0 | 4.05 | 97.2 |
| 6 | 24.0 | 6.01 | 24.0 | 6.02 | 144.5 |
| 8 | 24.0 | 8.00 | 24.0 | 8.04 | 192.96 |
| 10 | 24.0 | 10.01 | 24.0 | 10.02 | 240.48 |

---
