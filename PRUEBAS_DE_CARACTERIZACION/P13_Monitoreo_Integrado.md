# P13 - PZEM-016 + DS18B20 + ADS1015 (monitoreo integrado)

**Objetivo:** Verificar que los tres sistemas de medición (AC, temperatura, DC) funcionen simultaneamente sin interferencia EMI.

**Equipamiento:**
- PZEM-016 con CT
- Dos DS18B20 (T_BAT, T_AMB)
- ADS1015 Battery
- Fuente AC 110 V y DC 24 V

**Paso a paso:**

1. Arma la integración:
   ```
   Raspberry Pi
   ├─ I²C: ADS1015 (0x48) + cualquier sensor
   ├─ 1-Wire: DS18B20 × 2
   └─ UART0 (RS485 XY-017): PZEM-016
   ```

2. Corre un script que lea los tres sistemas:
   ```python
   import smbus
   import serial
   import glob
   
   bus = smbus.SMBus(1)
   ser = serial.Serial('/dev/ttyAMA0', 38400, timeout=0.1)
   
   while True:
       # Lee ADS1015
       v_bat = bus.read_word_data(0x48, 0x00)
       
       # Lee DS18B20
       temps = []
       for device in glob.glob('/sys/bus/w1/devices/28-*/w1_slave'):
           with open(device) as f:
               temps.append(float(f.readlines()[1].split('t=')[1]) / 1000)
       
       # Lee PZEM-016 (Modbus)
       # ... comando Modbus...
       
       print(f"V_BAT={v_bat*4.096/4095:.2f}V, T1={temps[0]:.2f}°C, T2={temps[1]:.2f}°C, PZEM=...")
   ```

3. Aplica calor a un DS18B20 mientras habilitas carga AC.

4. Verifica que:
   - No hay errores I²C/1-Wire/Modbus
   - Lecturas estables (ruido < 2 %)
   - Cambios en T se ven reflejados sin demora

**Criterios de aceptación:**
- Tres buses funcionan sin conflicto
- Latencia de lectura total < 500 ms
- Ruido en cada canal < 2 % de escala

---
