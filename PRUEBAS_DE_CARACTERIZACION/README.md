
# Pruebas de caracterización


## 1. Descripción general

Este directorio contiene los protocolos propuestos para la caracterización y verificación funcional de los componentes electrónicos y sistemas de adquisición de datos utilizados en el desarrollo de una estación de carga de corriente alterna (AC) para vehículos eléctricos, alimentada mediante generación solar fotovoltaica, almacenamiento en baterías y respaldo de la red eléctrica.

Las pruebas tienen como finalidad evaluar el funcionamiento individual de los sensores, los circuitos de acondicionamiento de señales, las interfaces de comunicación y la integración de los subsistemas de medición.

Los procedimientos se encuentran organizados en archivos Markdown independientes, con el propósito de facilitar su consulta, ejecución y posterior documentación de resultados experimentales.

---

## 2. Organización de las pruebas

## BLOQUE 1: Sensores analógicos + ADC (V_PV, I_PV, V_BAT, I_BAT)

### 2.1. Sensores analógicos y conversión analógico-digital

| Código | Prueba | Documentación |
|---|---|---|
| P01 | Caracterización del divisor resistivo V_PV | [Ver prueba](./P01_Divisor_Resistivo.md) |
| P02 | Calibración del sensor de corriente ACS758 | [Ver prueba](./P02_ACS758.md) |
| P03 | Caracterización del shunt FL-2 | [Ver prueba](./P03_Shunt_FL2.md) |
| P04 | Verificación del conversor ADS1015 | [Ver prueba](./P04_ADS1015.md) |
| P05 | Integración del divisor resistivo y ADS1015 | [Ver prueba](./P05_Divisor_ADS1015.md) |

### 2.2. Sensores de temperatura

| Código | Prueba | Documentación |
|---|---|---|
| P06 | Caracterización individual del sensor DS18B20 | [Ver prueba](./P06_DS18B20.md) |
| P07 | Verificación de dos sensores DS18B20 en un mismo bus 1-Wire | [Ver prueba](./P07_Dos_DS18B20.md) |

### 2.3. Medición de variables eléctricas AC

| Código | Prueba | Documentación |
|---|---|---|
| P08 | Comunicación Modbus-RTU y mediciones del PZEM-016 | [Ver prueba](./P08_PZEM016_Modbus.md) |
| P09 | Caracterización del PZEM-016 con transformador de corriente | [Ver prueba](./P09_PZEM016_CT.md) |

### 2.4. Interfaces de comunicación

| Código | Prueba | Documentación |
|---|---|---|
| P10 | Verificación del módulo RS-485 XY-017 | [Ver prueba](./P10_XY017.md) |
| P11 | Verificación de la interfaz SC16IS750 y MAX3232 | [Ver prueba](./P11_MAX3232_SC16IS750.md) |

### 2.5. Pruebas de integración

| Código | Prueba | Documentación |
|---|---|---|
| P12 | Integración de la cadena de adquisición DC | [Ver prueba](./P12_Adquisicion_DC.md) |
| P13 | Integración de mediciones AC, DC y temperatura | [Ver prueba](./P13_Monitoreo_Integrado.md) |

---

## 3. Metodología general

Las pruebas se organizan en tres fases:

**Fase 1. Caracterización individual**

Verificación del funcionamiento de sensores, circuitos de acondicionamiento y dispositivos de medición.

**Fase 2. Verificación de comunicaciones**

Evaluación de las interfaces utilizadas para el intercambio de información entre la unidad central de control y los equipos externos.

**Fase 3. Integración de subsistemas**

Evaluación del funcionamiento conjunto de los dispositivos de adquisición y comunicación.

Cada protocolo contiene el objetivo, los equipos requeridos, la metodología propuesta, los criterios de aceptación y los resultados esperados.

---

## 4. Estado de la documentación

Los archivos de este directorio corresponden a protocolos de caracterización propuestos.

Los resultados esperados son referencias para las verificaciones experimentales y no representan mediciones obtenidas durante la ejecución de las pruebas.

Los procedimientos y criterios de aceptación deberán revisarse frente a las hojas de datos de los fabricantes, el diseño electrónico definitivo y las condiciones de seguridad aplicables antes de su ejecución.

---

## 5. Consideraciones

- Las pruebas deberán desarrollarse utilizando instrumentos de medición adecuados.
- Las conexiones eléctricas deben verificarse antes de cualquier ensayo.
- Las pruebas con circuitos de potencia o tensión de red requieren instalaciones protegidas y supervisión técnica.
- Las mediciones deberán registrarse para permitir su análisis y comparación con los resultados esperados.
- Los procedimientos podrán actualizarse durante el desarrollo experimental del proyecto, conservando la trazabilidad de las modificaciones.

---

**Trabajo de grado para optar por el título de Ingeniero Mecatrónico**

**Pontificia Universidad Javeriana**

**Facultad de Ingeniería — Ingeniería Mecatrónica**

