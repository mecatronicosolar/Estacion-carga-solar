# Pruebas de Caracterización - Estación de Carga Solar

## BLOQUE 1: Sensores analógicos + ADC (V_PV, I_PV, V_BAT, I_BAT)

### Prueba 1.1 — Divisor resistivo V_PV (caracterización de la atenuación)

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

### Prueba 1.2 — Sensor Hall ACS758LCB-050B (calibración I_PV)

**Objetivo:** Verificar la sensibilidad y offset del sensor, y confirmar que su salida se ajusta a la ecuación teórica.

**Equipamiento:**
- Fuente de corriente de laboratorio (0–10 A DC) O carga resistiva controlable + fuente de voltaje
- Pinza amperimétrica (para medir corriente real de referencia)
- Multímetro digital
- Osciloscopio (para medir V_out en tiempo real)

**Paso a paso:**

1. Arma el ACS758 en protoboard:
   - Pin 1 (IP+): hacia la fuente
   - Pin 2 (IP−): hacia la carga
   - Pin 3 (GND): a masa
   - Pin 4 (VOUT): a un canal del osciloscopio y a un canal del multímetro
   - Pin 5 (VCC): a +5 V

2. Mide el offset (sin corriente):
   - Apaga la fuente de corriente → I = 0 A
   - Lee V_out en reposo → debe ser ~2.5 V (referencia del sensor)
   - Anota: V_offset = _____ V

3. Aplica corrientes crecientes en pasos de 2 A:
   - I = 0 A → V_out = ? V
   - I = 2 A → V_out = ? V (debe ser ~2.54 V = 2.5 + 2×0.02)
   - I = 4 A → V_out = ? V
   - I = 6 A → V_out = ? V
   - I = 8 A → V_out = ? V
   - I = 10 A → V_out = ? V (máximo sensor = 2.7 V)

4. Mide también con corriente negativa (si es posible invertir polaridad):
   - I = −2 A → V_out = ? V (debe ser ~2.46 V = 2.5 − 2×0.02)
   - I = −4 A, etc.

5. Con el osciloscopio: observa la forma de onda de V_out; debe ser una DC limpia sin ripple.

**Criterios de aceptación:**
- Offset: 2.5 V ± 0.1 V (típicamente 2.48–2.52 V)
- Sensibilidad: 20 mV/A ± 1 % (rango de aceptación 19.8–20.2 mV/A)
- Linealidad: R² > 0.99 en la regresión V_out vs. I
- Ruido: ripple < 20 mV pico a pico

**Resultado esperado:**

```
V_out (V)
   2.7 |                    ●
   2.6 |                  ●
   2.5 |         ● (referencia)
   2.4 |       ●
   2.3 |     ●
       |_____________________________ I (A)
       -10  -5   0   5  10
```

**Fórmula de calibración hallada:**

```
V_out = 2.5 + 0.020 × I_PV  (cambiar 0.020 por tu sensibilidad medida)
I_PV = (V_out − 2.5) / 0.020
```

---

### Prueba 1.3 — Shunt FL-2 (caracterización diferencial I_BAT)

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

### Prueba 1.4 — ADS1015 (verificación I²C y resolución)

**Objetivo:** Confirmar que el ADS1015 comunica correctamente por I²C y que su resolución y rango coinciden con la especificación.

**Equipamiento:**
- Raspberry Pi con acceso GPIO (I²C habilitado)
- ADS1015 soldado en protoboard
- Resistencia pull-up de 2.2 kΩ en SDA y SCL (si no están integradas)
- Fuente de referencia conocida (0–3.3 V)
- Multímetro para medir la referencia

**Paso a paso:**

1. Conecta el ADS1015:
   - VCC → +3.3 V
   - GND → GND Raspberry Pi
   - SDA → GPIO 2 (Raspberry Pi)
   - SCL → GPIO 3 (Raspberry Pi)
   - ADDR → GND (para dirección 0x48) o VCC (para 0x49)

2. En la Raspberry Pi, instala las herramientas de I²C:
   ```bash
   sudo apt install i2c-tools python3-smbus
   ```

3. Detecta el dispositivo:
   ```bash
   i2cdetect -y 1
   ```
   Debe mostrar el ADS1015 en la dirección 0x48 o 0x49.

4. Escribe un script Python simple para leer los canales:
   ```python
   import smbus
   bus = smbus.SMBus(1)
   ads_addr = 0x48
   
   # Configura ADS1015 (0x01 = Config Register)
   # Bits: rango ±4.096V, muestreo 1600 SPS, canal 0
   config = [0xC3, 0x83]  # valores típicos
   bus.write_i2c_block_data(ads_addr, 0x01, config)
   
   # Lee resultado (0x00 = Result Register)
   result = bus.read_i2c_block_data(ads_addr, 0x00, 2)
   voltage = (result[0] << 8 | result[1]) >> 4  # desplaza para 12 bits
   voltage_volts = voltage * 4.096 / 2048
   print(f"Lectura: {voltage} → {voltage_volts:.3f} V")
   ```

5. Aplica tensiones conocidas en AIN0:
   - 0 V → debe leer ~0 código
   - 1.65 V (mitad de 3.3 V) → debe leer ~2048 código (mitad de 4096)
   - 3.3 V → debe leer ~4095 código (máximo)

6. Mide ruido y estabilidad:
   - Lee 100 veces una misma tensión y calcula desviación estándar.
   - Debe ser < 10 códigos (< 20 mV en rango ±4.096 V).

**Criterios de aceptación:**
- Comunicación I²C: éxito en i2cdetect
- Linealidad: R² > 0.99 de código vs. voltaje aplicado
- Resolución: 4.096 V / 2048 códigos = 2 mV/código ✓
- Ruido: σ < 10 códigos en lecturas repetidas

**Resultado esperado:**

```
Dirección I²C: 0x48 (detectado ✓)
Lectura V = 0.000 V → código = 0
Lectura V = 1.650 V → código = 2048
Lectura V = 3.300 V → código = 4095
Ruido (σ): 5.2 códigos → 10.4 mV (aceptable)
```

---

### Prueba 1.5 — Integración ADS1015 + divisor resistivo (V_PV)

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

## BLOQUE 2: Sensores de temperatura (DS18B20)

### Prueba 2.1 — DS18B20 (comunicación 1-Wire y respuesta térmica)

**Objetivo:** Verificar que el sensor comunica correctamente por 1-Wire, mide temperatura con precisión, y responde adecuadamente a cambios térmicos.

**Equipamiento:**
- Raspberry Pi con GPIO4 disponible
- DS18B20 en protoboard
- Resistencia pull-up 4.7 kΩ (entre DQ y +3.3 V)
- Fuente de calor (secador de aire, placa caliente, hielo)
- Termómetro de referencia (termómetro de mercurio o digital confiable)

**Paso a paso:**

1. Conecta el DS18B20:
   - Pin 1 (GND) → GND Raspberry Pi
   - Pin 2 (DQ) → GPIO 4 + pull-up 4.7 kΩ a +3.3 V
   - Pin 3 (VCC) → +3.3 V

2. En la Raspberry Pi, habilita 1-Wire:
   ```bash
   sudo nano /boot/firmware/config.txt
   # Agrega esta línea:
   dtoverlay=w1-gpio,gpiopin=4
   # Reinicia
   sudo reboot
   ```

3. Detecta el sensor:
   ```bash
   ls /sys/bus/w1/devices/
   # Debe mostrar algo como "28-000000000000"
   ```

4. Lee la temperatura en reposo (ambiente ~20–25 °C):
   ```bash
   cat /sys/bus/w1/devices/28-*/w1_slave
   # Salida típica:
   # 95 01 4b 46 7f ff 0e 10 6b : crc=6b YES
   # 95 01 4b 46 7f ff 0e 10 6b t=25000
   # t=25000 → 25.000 °C
   ```

5. Aplica calor gradualmente (con un secador en modo bajo):
   - Lee temperatura cada 5 segundos
   - Anota 10–15 lecturas mientras calientas
   - Mide con termómetro de referencia en paralelo.

6. Repite con hielo (descenso de temperatura):
   - Coloca el sensor en una bolsa de hielo
   - Lee temperatura cada 10 segundos durante 1–2 minutos

**Criterios de aceptación:**
- Comunicación 1-Wire: sensor detectado en /sys/bus/w1/devices/
- Rango de lectura: debe medir entre −10 °C y +85 °C
- Precisión: ±0.5 °C respecto al termómetro de referencia
- Respuesta térmica: cambio visible en la lectura dentro de 1–2 segundos de aplicar calor
- Resolución: lecturas reportan al menos 0.001 °C (12 bits)

**Resultado esperado:**

```
Temperatura ambiente inicial: 22.5 °C (multímetro) ↔ 22.512 °C (DS18B20) ✓

Calentamiento con secador:
Tiempo (s) | T_ref (°C) | T_DS18B20 (°C) | Δ (°C) |
0          | 22.5       | 22.512         | +0.012 |
30         | 35.2       | 35.156         | −0.044 |
60         | 42.8       | 42.781         | −0.019 |
Promedio error: 0.025 °C (aceptable ✓)

Congelación:
Tiempo (s) | T_ref (°C) | T_DS18B20 (°C) |
0          | 22.5       | 22.512         |
30         | 15.3       | 15.281         |
60         | 8.1        | 8.094          |
Respuesta térmica: OK ✓
```

---

### Prueba 2.2 — Dos DS18B20 en el mismo bus 1-Wire (T_BAT + T_AMB)

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

## BLOQUE 3: Medidor AC (PZEM-016)

### Prueba 3.1 — PZEM-016 (Modbus-RTU básico y lecturas AC)

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

### Prueba 3.2 — PZEM-016 con Transformador de Corriente (CT)

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

## BLOQUE 4: Conversores de comunicación (MAX3232, SC16IS750, XY-017)

### Prueba 4.1 — XY-017 (TTL–RS485 con control de dirección)

**Objetivo:** Verificar que el módulo convierte correctamente los niveles TTL a RS485 y que el control de dirección (GPIO 17) funciona.

**Equipamiento:**
- Raspberry Pi
- Módulo XY-017
- Dos resistores 120 Ω (terminación de línea RS485)
- Osciloscopio dual (para ver TTL y RS485 en simultáneo)
- Cable de prueba RS485

**Paso a paso:**

1. Arma el XY-017:
   ```
   Raspberry Pi    XY-017       RS485
   GPIO 14 (TX) → TXD
   GPIO 15 (RX) → RXD
   GPIO 17      → DE/RE
   GND          → GND          ─ GND
                                A(+) ─[120Ω]─ (carga o endpoint)
                                B(−) ─[120Ω]─
   ```

2. En la Raspberry Pi, configura GPIO 17 como salida:
   ```python
   import RPi.GPIO as GPIO
   import serial
   import time
   
   GPIO.setmode(GPIO.BCM)
   GPIO.setup(17, GPIO.OUT)
   
   # UART0
   ser = serial.Serial('/dev/ttyAMA0', baudrate=38400, timeout=0.1)
   
   # Test 1: Envío (GPIO 17 = HIGH)
   GPIO.output(17, GPIO.HIGH)
   time.sleep(0.1)
   ser.write(b'HELLO')
   print("Enviado HELLO en modo TX")
   
   time.sleep(1)
   
   # Test 2: Recepción (GPIO 17 = LOW)
   GPIO.output(17, GPIO.LOW)
   time.sleep(0.1)
   data = ser.read(10)
   print(f"Recibido: {data}")
   ```

3. Con el osciloscopio:
   - Mide TTL en GPIO 14 (TX) durante envío
   - Mide RS485 diferencial durante envío
   - Verifica que el nivel TTL (0–3.3 V) se convierte a RS485 (±12 V aprox)

4. Verifica control de dirección:
   - GPIO 17 = HIGH → línea DE/RE activa en transmisión
   - GPIO 17 = LOW → línea DE/RE inactiva, modo recepción

**Criterios de aceptación:**
- Comunicación TTL–RS485: datos legibles en línea RS485
- Niveles: TTL ~3.3 V se convierte a RS485 ~±12 V
- Control de dirección: cambio entre TX/RX funciona sin errores
- Velocidad: 38400 baud sin pérdida de datos

**Resultado esperado:**

```
TTL (GPIO 14): 0V → 3.3V (transiciones claras)
RS485 diferencial: −12V → +12V (proporcional a TTL)
Datos recibidos: HELLO (coincide con lo enviado)
Control DE/RE: funciona correctamente ✓
```

---

### Prueba 4.2 — MAX3232 + SC16IS750 (RS-232 para ML2440)

**Objetivo:** Verificar que la cadena TTL → I²C → UART → RS-232 funciona para comunicar con el controlador MPPT.

**Equipamiento:**
- Raspberry Pi
- SC16IS750 en protoboard
- MAX3232 en protoboard
- Cable RS-232 de prueba (9 pines D-Sub)
- Osciloscopio (para RS-232)

**Paso a paso:**

1. Arma el SC16IS750 en I²C:
   ```
   SC16IS750      Raspberry Pi
   VCC     → +3.3V
   GND     → GND
   SDA     → GPIO 2
   SCL     → GPIO 3
   ADDR    → GND (dirección 0x48) o VCC (0x49)
   GND     → para configurar dirección a 0x4D [PD]
   TXD     → MAX3232 RIN1
   RXD     → MAX3232 DOUT1
   ```

2. Arma el MAX3232:
   ```
   MAX3232        SC16IS750 / RS-232
   T1IN   → SC16IS750 TXD
   R1OUT  → SC16IS750 RXD
   T1OUT  → RS-232 RXD (pin 2)
   R1IN   → RS-232 TXD (pin 3)
   GND    → GND
   VCC    → +5V
   ```

3. Escribe un script Python para enviar datos:
   ```python
   import smbus
   import time
   
   bus = smbus.SMBus(1)
   sc16_addr = 0x4D  # (o según tu configuración)
   
   # Configurar registro UART (consulta datasheet SC16IS750)
   # Registro 0x00: RX FIFO / TX FIFO
   # Registro 0x01: IER (habilitación de interrupciones)
   # Registro 0x03: LCR (8 bits, 1 stop, sin paridad)
   
   # Configura el puerto serie
   bus.write_byte_data(sc16_addr, 0x03, 0x03)  # 8N1
   
   # Envía "TEST\r\n"
   data = b"TEST\r\n"
   for byte in data:
       bus.write_byte_data(sc16_addr, 0x00, byte)
       time.sleep(0.01)
   
   print("Enviado 'TEST\\r\\n'")
   ```

4. Con el osciloscopio, mide RS-232:
   - Debe ver transiciones entre −12 V (lógica 0) y +12 V (lógica 1)
   - Velocidad: según baudrate configurado (típicamente 2400–9600 para ML2440)

**Criterios de aceptación:**
- Comunicación I²C con SC16IS750: detectado en dirección 0x4D (o la tuya)
- Envío de datos: salida RS-232 válida a ±12 V
- Velocidad: sin pérdida de datos

**Resultado esperado:**

```
SC16IS750 detectado en dirección 0x4D ✓
Datos enviados por I²C → UART → RS-232
RS-232 salida: −12 V / +12 V (válido)
Baudrate: 2400 baud (según ML2440)
```

---

## PRUEBAS CONJUNTAS (integración de sensores antes del montaje final)

### Prueba 5.1 — ADS1015 + ACS758 + Divisor resistivo (cadena completa de DC)

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

### Prueba 5.2 — PZEM-016 + DS18B20 + ADS1015 (monitoreo integrado)

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

## RESUMEN: orden de ejecución recomendado

### Fase 1: Sensores individuales (bloques 1–3)
- Prueba 1.1: Divisor resistivo
- Prueba 1.2: ACS758
- Prueba 1.3: Shunt FL-2
- Prueba 1.4: ADS1015
- Prueba 2.1: DS18B20 simple
- Prueba 2.2: DS18B20 × 2
- Prueba 3.1–3.2: PZEM-016

### Fase 2: Conversores (BLOQUE 4)
- Prueba 4.1: XY-017
- Prueba 4.2: MAX3232 + SC16IS750

### Fase 3: Integración (BLOQUE 5)
- Prueba 5.1: Cadena DC completa
- Prueba 5.2: Sistema integrado

Esto te permite identificar componentes defectuosos temprano y caracterizar el comportamiento de cada uno antes de enfrentar debugging en el sistema completo.
