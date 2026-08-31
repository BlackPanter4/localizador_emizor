O# ZEEK - Localizador Emizor PRO 🚀

Rastreador GPS casero con ESP32, NEO-6M y SIM800L. Proyecto de BlackPanter4 desde Luchanas, Coah.

### ¿Qué hace?
1. El NEO-6M saca latitud y longitud real.
2. El ESP32 la procesa.
3. El SIM800L con chip TELCEL la envía por HTTP a tu servidor `Localizador.py`
4. Tu página web muestra el punto en el mapa.

### Hardware Necesario
- ESP32 DevKit v1
- Módulo GPS NEO-6M
- Módulo GSM SIM800L + Chip TELCEL con datos
- Buck Converter 12V a 5V 3A
- Batería 12V (de moto/carro) + Fusible 3A
- Capacitor 1000uF para el SIM800L

### Conexión de los 3 Módulos
> ¡TODOS los GND juntos o no jala!

**NEO-6M -> ESP32**
- VCC -> 5V
- GND -> GND
- TX -> GPIO 17 (RX2)
- RX -> GPIO 16 (TX2)

**SIM800L -> ESP32**
- VCC -> 5V (del Buck)
- GND -> GND
- TX -> GPIO 27 (RX1)
- RX -> GPIO 26 (TX1)

**Alimentación**
- Batería 12V+ -> Fusible -> IN+ Buck
- Batería 12V- -> IN- Buck
- OUT+ 5V Buck -> 5V ESP32, NEO-6M, SIM800L
- OUT- GND Buck -> GND ESP32, NEO-6M, SIM800L

### Instalación

1. **En el ESP32 (con Thonny):**
   - Flashea MicroPython
   - Sube `main.py` (código con librería `machine`)

2. **En tu PC / Servidor:**
   ```bash
   python Localizador.py
   # Escuchando en 0.0.0.0:8000
## Estado Actual
- [x] Código Python servidor (Localizador.py)
- [x] Diagrama de conexión
- [ ] Hardware comprado
- [ ] Prueba en campo Luchanas