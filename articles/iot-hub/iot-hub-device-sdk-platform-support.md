---
title: Azure IoT SDK's ondersteuning voor apparaatplatforms | Microsoft Docs
description: Concepten - lijst met platforms die worden ondersteund door de Azure IoT Device-SDK 's
author: yzhong94
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: yizhon
ms.openlocfilehash: cf3c80424c4626b62317bda537f9491cafc8198c
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043776"
---
# <a name="azure-iot-sdks-platform-support"></a>Platformondersteuning voor Azure IoT SDK 's

[Azure IoT SDK's](iot-hub-devguide-sdks.md) zijn een set van bibliotheken om te communiceren met IoT Hub en de Device Provisioning Service met brede taal en platformondersteuning.  De SDK's worden uitgevoerd op de meest voorkomende platformen en ontwikkelaars kunnen de C-SDK voor specifieke platform poort door de [overzetten richtlijnen](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md). 

Microsoft ondersteunt een groot aantal besturingssystemen/platforms/frameworks en kan worden uitgebreid met de Azure IoT C-SDK.  Sommige worden officieel ondersteund door het team, gegroepeerd in lagen, waarbij het niveau van ondersteuning voor gebruikers kunnen verwachten.  Volledig ondersteunde platforms middelen die door Microsoft:
    - Doorlopend ondersteund bouwen en end-to-end-test op basis van model en de LTS versie (s)
    - Installatiehandleiding of pakketten bieden, indien van toepassing
    - Volledige ondersteuning op GitHub

Bovendien een lijst met partners is overgezet onze C-SDK naar meer platformen en ze de platform abstraction layer (PAL) worden onderhouden.  [Azure Certified voor IoT-Apparaatcatalogus](https://catalog.azureiotsolutions.com/) ook functies een lijst van de verschillende SDK's van de OS-platformen zijn getest op basis van.  Bovendien de SDK's ook regelmatig bouwen op deze platforms met beperkte testen en ondersteunen:
- MBED2
- Arduino
- Windows CE 2013 (afschaffen in oktober 2018)
- .NET standard 1.3 en 2.1 van .NET Core en .NET Framework 4.7
- Xamarin iOS, Android, UWP
- Android met behulp van Java

## <a name="supported-platforms"></a>Ondersteunde platforms
### <a name="c-sdk"></a>C-SDK
| OS                  | Arch | Compiler             | TLS-bibliotheek       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | GCC-5.4.0            | openssl - 1.0.2g |
| 18.04 Ubuntu LTS    | X64  | GCC-7.3              | WolfSSL – 1.13    |
| 18.04 Ubuntu LTS    | X64  | CLang 6.0.X          | Openssl-1.1.0g  |
| OSX 10.13.4         | x64  | XCode 9.4.1          | Systeemeigen OSX        |
| Windows Server 2016 | x64  | Visual Studio 14.0.X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0.X | SChannel          |
| Debian 9 Stretch    | x64  | GCC-7.3              | Openssl-1.1.0f  |

### <a name="python-sdk"></a>Python-SDK
| OS                  | Arch | Compiler   | TLS-bibliotheek |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2.7 | openssl     |
| Windows Server 2016 | x64  | Python 2.7 | openssl     |
| Windows Server 2016 | x86  | Python 3.5 | openssl     |
| Windows Server 2016 | x64  | Python 3.5 | openssl     |
| 18.04 Ubuntu LTS    | x86  | Python 2.7 | openssl     |
| 18.04 Ubuntu LTS    | x86  | Python 3.4 | openssl     |
| MacOS High Sierra   | x64  | Python 2.7 | openssl     |

### <a name="net-sdk"></a>.NET SDK
| OS                  | Arch | Framework            | Standard          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET framework 4.7   | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET framework 4.5.1 | N/A               |

### <a name="nodejs-sdk"></a>Node.js SDK
| OS                                           | Arch | De versie van knooppunt |
|----------------------------------------------|------|--------------|
| Ubuntu 16.04 LTS (met behulp van docker-installatiekopie van knooppunt 6) | X64  | Knooppunt 6       |
| Windows Server 2016                          | X64  | Knooppunt 6       |

### <a name="java-sdk"></a>Java-SDK
| OS                  | Arch | Java-versie |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |

## <a name="partner-supported-platforms"></a>Door partners ondersteund platforms
| Partner             | Apparaten                            | Koppeling                     | Ondersteuning |
|---------------------|------------------------------------|--------------------------|---------|
| Qualcomm            | Qualcomm MDM9206 LTE IoT Modem     | [Qualcomm LTE voor IoT-SDK](https://developer.qualcomm.com/software/lte-iot-sdk) | [Forum](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | STM32L4 serie STM32F4-serie      | [X-KUBUS-CLOUD](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html)             | [Ondersteuning](https://www.st.com/content/st_com/en/support/support-home.html) |
|                     | STM32F7-serie                     | [X-KUBUS-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html)             |         |
|                     | STM32L4 detectie Kit voor IoT-knooppunt | [P-NUCLEO-AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html)          |         |
|                     |                                    | [FP-CLD-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            |         |
| Espressif           | ESP32                              | [ESP-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  |

## <a name="next-steps"></a>Volgende stappen
- [Apparaat- en service-SDK's](iot-hub-devguide-sdks.md)
- [Poortrichtlijnen](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)