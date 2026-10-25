# P07 — Dos DS18B20 en el mismo bus 1-Wire (T_BAT + T_AMB)

**Objetivo:** Verificar que dos sensores en el mismo bus no interfieren y pueden identificarse de forma única.

**Equipamiento:**
- Dos DS18B20 distintos
- Mismo circuito de pull-up (una sola resistencia 4.7 kΩ sirve para ambos)
- GPIO 4 de Raspberry Pi

**Paso a paso:**

1. Conecta ambos sensores en paralelo al GPIO 4:
   ```
   DS18B20_1   GND ─┬─ GND Raspberry Pi
              DQ  ─┼─ GPIO 4 (con pull-up)
              VCC ─┼─ +3.3 V
              
   DS18B20_2   GND ─┤
              DQ  ─┤
              VCC ─┤
   ```

2. Lista los dispositivos 1-Wire:
   ```bash
   ls /sys/bus/w1/devices/
   # Debe mostrar DOS direcciones distintas, ej:
   # 28-000000000001
   # 28-000000000002
   ```

3. Lee ambos sensores simultáneamente:
   ```python
   import glob
   import time
   
   devices = glob.glob('/sys/bus/w1/devices/28-*/w1_slave')
   
   for device in sorted(devices):
       with open(device, 'r') as f:
           lines = f.readlines()
           if lines[0].strip()[-3:] == 'YES':
               temp = float(lines[1].split('t=')[1]) / 1000.0
               print(f"{device.split('/')[-2]}: {temp:.3f} °C")
   ```

4. Aplica calor a un sensor manteniendo el otro a temperatura ambiente, y verifica que se diferencian correctamente.

**Criterios de aceptación:**
- Ambos sensores detectados en direcciones distintas
- Lectura independiente sin crosstalk
- Diferencia de temperatura entre sensores: > 1 °C cuando uno se calienta

**Resultado esperado:**

```
28-000000000001: 22.512 °C (T_AMB ambiente)
28-000000000002: 35.156 °C (T_BAT con calor aplicado)
✓ Diferenciación correcta
```

---
