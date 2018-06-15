---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4ae4cfb91fb3a746c73d6b098a1adc9e4dee8698
ms.sourcegitcommit: caebf2bb2fc6574aeee1b46d694a61f8b9243198
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/12/2018
ms.locfileid: "35414702"
---
Exporteer de openbare sleutel .cer bestand voor het basiscertificaat (niet de persoonlijke sleutel) na het maken van een zelfondertekend basiscertificaat. U zult dit bestand later uploaden naar Azure. De volgende stappen kunt u het cer-bestand voor uw zelfondertekende basiscertificaat exporteren:

1. Als u een CER-bestand wilt genereren van het certificaat, opent u **Gebruikerscertificaten beheren**. Zoek het zelfondertekende basiscertificaat, meestal in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten', en klik er met de rechtermuisknop op. Klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**. Als u het huidige gebruiker\Persoonlijk\Certificaten-certificaat niet vinden kan het zijn dat u certificaten Manager geopend voor de lokale computer-certificaten (titel zijn 'Certificaten – lokale Computer' als 'Certificaten – huidige gebruiker' geboden). Certificaten Manager openen in huidige gebruiker bereik starten vanaf de dezelfde PowerShell waar de certificaten zijn gemaakt door te typen ```certmgr```.

  ![Exporteren](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. Klik in de wizard op **Volgende**.

  ![Certificaat exporteren](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. Selecteer **Nee, de persoonlijke sleutel niet exporteren** en klik vervolgens op **Volgende**.

  ![De persoonlijke sleutel niet exporteren](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. Selecteer op de pagina **Bestandsindeling voor export** de optie **Met Base64 gecodeerde X.509 (.CER)** en klik op **Volgende**.

  ![Base 64-codering](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. Voor **te exporteren bestand**, **Bladeren** naar de locatie waarnaar u wilt exporteren van het certificaat. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.

  ![Bladeren](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Klik op **Voltooien** om het certificaat te exporteren.

  ![Voltooien](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. Uw certificaat is geëxporteerd.

  ![Geslaagd](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. Het geëxporteerde certificaat ziet er ongeveer als volgt uit:

  ![Geëxporteerd](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Als u het geëxporteerde certificaat met Kladblok opent, ziet u iets soortgelijks als in dit voorbeeld. De sectie in blauw bevat de informatie die is geüpload naar Azure. Als u uw certificaat open met Kladblok en deze niet ziet er ongeveer als volgt, meestal dit betekent dat u hebt niet het exporteren met behulp van de Base-64 gecodeerde X.509 (. De indeling van de CER). Als u een andere teksteditor gebruiken wilt, moet u bovendien begrijpen dat sommige editors kunnen leiden tot onverwachte opmaak op de achtergrond. Dit kan leiden tot problemen wanneer de tekst van dit certificaat op Azure geüpload.

  ![Open met Kladblok](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)
