# P08 — PZEM-016 (Modbus-RTU básico y lecturas AC)

**Objetivo:** Verificar que el PZEM-016 responde a comandos Modbus-RTU y entrega mediciones coherentes.

**Equipamiento:**
- PZEM-016 con CT PZCT-02 acoplado
- Fuente AC de laboratorio 110 V / 60 Hz (o toma de pared regulada)
- Carga AC controlable (bombilla, resistor de potencia, etc.)
- Multímetro AC para validación cruzada
- Raspberry Pi con módulo XY-017 (o cable USB-RS485 si prefieres)

**Paso a paso:**

1. Arma la conexión Modbus-RTU:
   ```
   Raspberry Pi (UART0) ←[XY-017]→ RS485 ←→ PZEM-016
   GPIO 14 (TX) → XY-017 TXD
   GPIO 15 (RX) → XY-017 RXD
   GPIO 17 → XY-017 DE/RE (control de dirección)
   XY-017 A/A+ → PZEM A
   XY-017 B/B− → PZEM B
   ```

2. Instala la librería Modbus en Raspberry Pi:
   ```bash
   pip3 install pymodbus
   ```

3. Escribe un script Python simple para leer registros Modbus:
   ```python
   from pymodbus.client import ModbusSerialClient
   import time
   
   client = ModbusSerialClient(method='rtu', port='/dev/ttyAMA0', 
                                baudrate=38400, stopbits=1, bytesize=8)
   client.connect()
   
   # PZEM-016: dirección Modbus = 0x01 (por defecto)
   # Registros de entrada (función 0x04):
   # 0x0000: Voltage (V) × 10
   # 0x0001: Current (A) × 100
   # 0x0002: Power (W)
   # 0x0003: Energy (kWh) × 100
   # 0x0004: Frequency (Hz) × 10
   # 0x0005: Power Factor × 100
   
   while True:
       result = client.read_input_registers(0x0000, 6, unit=0x01)
       if result.isError():
           print("Error Modbus")
       else:
           V = result.registers[0] / 10.0
           I = result.registers[1] / 100.0
           P = result.registers[2]
           E = result.registers[3] / 100.0
           f = result.registers[4] / 10.0
           PF = result.registers[5] / 100.0
           print(f"V={V:.1f}V, I={I:.2f}A, P={P}W, E={E:.2f}kWh, f={f:.1f}Hz, PF={PF:.2f}")
       time.sleep(1)
   ```

4. Energiza el PZEM-016 sin carga:
   - Lee V_AC, I_AC, P, E, f, PF
   - Anota 5 lecturas

5. Conecta una carga conocida (ej: resistor de 1000 W, 110 V):
   - Lee nuevamente
   - Verifica: P ≈ V × I / PF (potencia activa)
   - Mide con multímetro AC en paralelo para validar V e I.

**Criterios de aceptación:**
- Comunicación Modbus-RTU: éxito en lectura sin errores
- Tensión: 110 V ± 2 V (coincide con multímetro)
- Corriente: coincide con pinza amperimétrica ± 1 %
- Potencia: P = V × I × cos(φ) ± 3 %
- Frecuencia: 60 Hz ± 0.1 Hz
- Factor de potencia: 0.98–1.00 sin carga reactiva

**Resultado esperado:**

```
Sin carga:
V = 110.2 V (multímetro: 110.1 V) ✓
I = 0.03 A
P = 0 W (o < 5 W por consumo del medidor)
f = 60.00 Hz
PF = 0.99

Con carga 1000 W:
V = 109.8 V (multímetro: 109.9 V) ✓
I = 9.09 A (multímetro: 9.10 A) ✓
P = 999 W (teórico: 110 × 9.09 = 1000 W) ✓
f = 60.00 Hz
PF = 0.99
```

---
