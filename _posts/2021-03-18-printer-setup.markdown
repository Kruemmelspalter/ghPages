---
layout: post
title:  "Using a Brother MFC-7360N with CUPS"
---

1. Setup on Debian (or derivatives like Ubuntu) with `brlaser`
2. Setup on Docker
3. Using it from another CUPS server

## Setup on Debian (or derivatives like Ubuntu) with brlaser

To use a printer, you need a driver. The appropriate driver is `brlaser` so install it with `apt-get install printer-driver-brlaser` (you might have to use [sudo](https://www.howtoforge.com/tutorial/sudo-beginners-guide/) and also [update the package list](https://itsfoss.com/apt-get-linux-guide/)). Then setup the printer in CUPS:

1. Open CUPS (`http://localhost:631`)
{% picture /_images/cups-0.png %}

2. Go to the Administration tab
{% picture /_images/cups-1.png %}

3. Click on Printers > Add Printer
{% picture /_images/cups-2.png %}

4. Click on `LPD/LPR Host or Printer` and on Continue
{% picture /_images/cups-3.png %} {% picture /_images/cups-4.png %}

5. Enter `lpd://<printerIP>/BRFAX` and click on Continue
{% picture /_images/cups-5.png %}

6. Enter a name, description (optional), location (optional) and click on Continue
{% picture /_images/cups-6.png %}

7. Select Brother and click on Continue
{% picture /_images/cups-7.png %}

8. Select your printer model and click on Add Printer
{% picture /_images/cups-8.png %}

9. Just use the default options by clicking on Set Default Options

Done!

## Setup on Docker

Unfortunately, it's not so easy on other distros like Arch so I set it up on a server with Docker so I can use the already configured printer in the local CUPS server.

Luckily, there's already a [docker container with a CUPS server](https://hub.docker.com/r/ydkn/cups). We just have to modify it to use the driver.

Dockerfile
```
ROM ydkn/cups
RUN apt-get update
RUN apt-get install -y printer-driver-brlaser
```

docker-compose.yml
```
version: '3'

services:
  cups:
    build: .
    ports:
      - 631:631
    restart: unless-stopped
```

Run the docker configuration with `docker-compose up -d`

Do the same as in the Debian part, just go to `http://<serverIP>:631` and click on `Share This Printer` in step 5.

## Using it from another CUPS server

1. Do step 1-3 from the Debian part
2. Click on `Internet Printing Protocol (ipp)` and on Continue
3. Enter `ipp://<serverIP>:631/printers/`
4. Do step 6-9

### Printers supported by brlaser
 * Brother DCP-1510
 * Brother DCP-1602
 * Brother DCP-7030
 * Brother DCP-7040
 * Brother DCP-7055
 * Brother DCP-7055W
 * Brother DCP-7060D
 * Brother DCP-7065DN
 * Brother DCP-7080
 * Brother DCP-L2500D
 * Brother DCP-L2540DW
 * Brother HL-1110 series
 * Brother HL-1200 series
 * Brother HL-L2300D series
 * Brother HL-L2320D series
 * Brother HL-L2340D series
 * Brother HL-L2360D series
 * Brother MFC-1910W
 * Brother MFC-7240
 * Brother MFC-7360N
 * Brother MFC-7365DN
 * Brother MFC-7840W
 * Brother MFC-L2710DW
 * Lenovo M7605D