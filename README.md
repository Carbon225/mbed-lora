# Mbed LoRa

[![Build Status](https://travis-ci.org/sandeepmistry/arduino-LoRa.svg?branch=master)](https://travis-ci.org/sandeepmistry/arduino-LoRa)

An [Mbed](https://www.mbed.com/) library for sending and receiving data using [LoRa](https://www.lora-alliance.org/) radios.

# Port notes
Library was tested on SX1278 module. The main difference is that now you have to create the `LoRaClass` object yourself. I removed the MKR WAN shield code. Also I am not sure what will happen if the DIO0 interrupt happend during SPI operation since the Lora library will try to lock the bus. It might wait for the bus to become free but I do not know exactly how mbed handles SPI.

## Compatible Hardware

 * [Semtech SX1276/77/78/79](http://www.semtech.com/apps/product.php?pn=SX1276) based boards including:
   * [Dragino Lora Shield](http://www.dragino.com/products/module/item/102-lora-shield.html)
   * [HopeRF](http://www.hoperf.com/rf_transceiver/lora/) [RFM95W](http://www.hoperf.com/rf_transceiver/lora/RFM95W.html), [RFM96W](http://www.hoperf.com/rf_transceiver/lora/RFM96W.html), and [RFM98W](http://www.hoperf.com/rf_transceiver/lora/RFM98W.html)
   * [Modtronix](http://modtronix.com/) [inAir4](http://modtronix.com/inair4.html), [inAir9](http://modtronix.com/inair9.html), and [inAir9B](http://modtronix.com/inair9b.html)
 
### Semtech SX1276/77/78/79 wiring

| Semtech SX1276/77/78/79 | Arduino |
| :---------------------: | :------:|
| VCC | 3.3V |
| GND | GND |
| SCK | SCK |
| MISO | MISO |
| MOSI | MOSI |
| NSS | 10 |
| NRESET | 9 |
| DIO0 | 2 |

**NOTES**:
 * Some boards (like the Arduino Nano), cannot supply enough current for the SX127x in TX mode. This will cause lockups when sending, be sure to use an external 3.3V supply that can provide at least 120mA's when using these boards.
 * If your Arduino board operates at 5V, like the Arduino Uno, Leonardo or Mega, you will need to use a level converter for the wiring to the Semtech SX127x module. Most Semtech SX127x breakout boards do not have logic level converters built-in.

## Installation

### Using Git

```sh
cd /path/to/mbed/project/
git clone https://github.com/Carbon225/mbed-lora
```

## API

See [API.md](API.md).

## Examples

See [examples](examples) folder (outdated).

## FAQ

**1) Initilizating the LoRa radio is failing**

Please check the wiring you are using matches what's listed in [Semtech SX1276/77/78/79 wiring](#semtech-sx1276777879-wiring). Some logic level converters cannot operate at 8 MHz, you can call `LoRa.setSPIFrequency(frequency)` to lower the SPI frequency used by the library. Both API's must be called before `LoRa.begin(...)`.

**2) Can other radios see the packets I'm sending?**

Yes, any LoRa radio that are configured with the same radio parameters and in range can see the packets you send.

**3) Is the data I'm sending encrypted?**

No, all data is sent unencrypted. If want your packet data to be encrypted, you must encrypt it before passing it into this library, followed by decrypting on the receiving end.

**4) How does this library differ from LoRaWAN libraries?**

This library exposes the LoRa radio directly, and allows you to send data to any radios in range with same radio parameters. All data is broadcasted and there is no addressing. LoRaWAN builds on top of LoRA, but adds addressing, encryption, and additional layers. It also requires a LoRaWAN gateway and LoRaWAN network and application server.

**5) Does this library honor duty cycles?**

No, you have to manage it by your self.

**6) Which frequencies can I use?**

You can use [this table](https://www.thethingsnetwork.org/wiki/LoRaWAN/Frequencies/By-Country) to lookup the available frequencies by your country. The selectable frequency also depends on your hardware. You can lookup the data sheet or ask your supplier.

Please also notice the frequency dependent duty cycles for legal reasons!

## License

This libary is [licensed](LICENSE) under the [MIT Licence](https://en.wikipedia.org/wiki/MIT_License).
