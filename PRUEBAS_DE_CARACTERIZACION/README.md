
# Pruebas de caracterización


## 1. Descripción general

Este directorio contiene los protocolos propuestos para la caracterización y verificación funcional de los componentes electrónicos y sistemas de adquisición de datos utilizados en el desarrollo de una estación de carga de corriente alterna (AC) para vehículos eléctricos, alimentada mediante generación solar fotovoltaica, almacenamiento en baterías y respaldo de la red eléctrica.

Las pruebas tienen como finalidad evaluar el funcionamiento individual de los sensores, los circuitos de acondicionamiento de señales, las interfaces de comunicación y la integración de los subsistemas de medición.

Los procedimientos se encuentran organizados en archivos Markdown independientes, con el propósito de facilitar su consulta, ejecución y posterior documentación de resultados experimentales.

---

## 2. Organización de las pruebas

## BLOQUE 1: Sensores analógicos + ADC (V_PV, I_PV, V_BAT, I_BAT)


| Código | Prueba | Documentación |
|---|---|---|
| P01 | Divisor resistivo V_PV (caracterización de la atenuación) | [Ver prueba](./P01_Divisor_Resistivo.md) |
| P02 | Sensor Hall ACS758LCB-050B (calibración I_PV) | [Ver prueba](./P02_ACS758.md) |
| P03 | Shunt FL-2 (caracterización diferencial I_BAT) | [Ver prueba](./P03_Shunt_FL2.md) |
| P04 | ADS1015 (verificación I²C y resolución) | [Ver prueba](./P04_ADS1015.md) |
| P05 | Integración ADS1015 + divisor resistivo (V_PV) | [Ver prueba](./P05_Divisor_ADS1015.md) |

## BLOQUE 2: Sensores de temperatura (DS18B20)

| Código | Prueba | Documentación |
|---|---|---|
| P06 | DS18B20 (comunicación 1-Wire y respuesta térmica) | [Ver prueba](./P06_DS18B20.md) |
| P07 | Dos DS18B20 en el mismo bus 1-Wire (T_BAT + T_AMB) | [Ver prueba](./P07_Dos_DS18B20.md) |

## BLOQUE 3: Medidor AC (PZEM-016)

| Código | Prueba | Documentación |
|---|---|---|
| P08 | PZEM-016 (Modbus-RTU básico y lecturas AC) | [Ver prueba](./P08_PZEM016_Modbus.md) |
| P09 | PZEM-016 con Transformador de Corriente (CT) | [Ver prueba](./P09_PZEM016_CT.md) |

## BLOQUE 4: Conversores de comunicación (MAX3232, SC16IS750, XY-017)

| Código | Prueba | Documentación |
|---|---|---|
| P10 | XY-017 (TTL–RS485 con control de dirección) | [Ver prueba](./P10_XY017.md) |
| P11 | MAX3232 + SC16IS750 (RS-232 para ML2440) | [Ver prueba](./P11_MAX3232_SC16IS750.md) |

## PBLOQUE 5: RUEBAS CONJUNTAS (integración de sensores antes del montaje final)

| Código | Prueba | Documentación |
|---|---|---|
| P12 | ADS1015 + ACS758 + Divisor resistivo (cadena completa de DC) | [Ver prueba](./P12_Adquisicion_DC.md) |
| P13 |PZEM-016 + DS18B20 + ADS1015 (monitoreo integrado) | [Ver prueba](./P13_Monitoreo_Integrado.md) |

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

