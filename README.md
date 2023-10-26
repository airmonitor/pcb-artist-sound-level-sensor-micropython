A MicroPython library for obtaining measurements from Plantower PTQS1005 sensor - https://www.plantower.com/en/products_36/82.html


Usage:

```python
import logging
import sys
import time

from i2c import I2CAdapter

from pcb_artist_sound_level import PCBArtistSoundLevel

logging.basicConfig(level=logging.INFO, stream=sys.stdout)


def pcb_artist_sound_level_measurements(sensor: PCBArtistSoundLevel):
    sensor_version = sensor.db_sensor_version()
    logging.debug(f"Sound level sensor version = 0x{sensor_version:02x}")
    sensor_id = sensor.db_sensor_id()
    logging.debug(f"Sound level sensor unique ID: 0x{sensor_id:02x}")

    return int.from_bytes(sensor.reg_read(), "big")


if __name__ == "__main__":
    i2c_adapter = I2CAdapter(scl=Pin(22), sda=Pin(21), freq=100000)
    sensor = PCBArtistSoundLevel(i2c=i2c_adapter)

    while True:
        sound_level = pcb_artist_sound_level_measurements(sensor)
        logging.info(f"Sound level: {sound_level} dB")
        time.sleep(2)

```
