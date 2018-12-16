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
ms.openlocfilehash: 3c6485406c67bf84b9e0fdfb9f4683abe5062d6c
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444075"
---
Na het maken van een zelfondertekend basiscertificaat, exporteer de openbare sleutel cer-basiscertificaatbestand (niet de persoonlijke sleutel). U wordt dit bestand later uploaden naar Azure. De volgende stappen kunt u het cer-bestand voor uw zelfondertekend basiscertificaat exporteren:

1. Als u een CER-bestand wilt genereren van het certificaat, opent u **Gebruikerscertificaten beheren**. Zoek het zelfondertekende basiscertificaat, meestal in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten', en klik er met de rechtermuisknop op. Klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**. Als u het certificaat in de huidige gebruiker\Persoonlijk\Certificaten niet vinden kan het zijn dat u certificaten Manager geopend voor de lokale computer-certificaten (titel zijn 'Certificaten - lokale Computer' als plaats van 'Certificaten - huidige gebruiker'). Certificaten Manager openen in huidige gebruikersbereik van starten vanuit de dezelfde PowerShell waar de certificaten zijn gemaakt door te typen ```certmgr```.

   ![Exporteren](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. Klik in de wizard op **Volgende**.

   ![Certificaat exporteren](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. Selecteer **Nee, de persoonlijke sleutel niet exporteren** en klik vervolgens op **Volgende**.

   ![De persoonlijke sleutel niet exporteren](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. Selecteer op de pagina **Bestandsindeling voor export** de optie **Met Base64 gecodeerde X.509 (.CER)** en klik op **Volgende**.

   ![Base-64 gecodeerd](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. Voor **te exporteren bestand**, **Bladeren** naar de locatie waarnaar u wilt dat het certificaat te exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.

   ![Bladeren](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Klik op **Voltooien** om het certificaat te exporteren.

   ![Voltooien](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. Uw certificaat is geëxporteerd.

   ![Geslaagd](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. Het geëxporteerde certificaat ziet er ongeveer als volgt uit:

   ![Geëxporteerd](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Als u het geëxporteerde certificaat met behulp van Kladblok opent, ziet er ongeveer uitzien als in dit voorbeeld. De sectie in het blauw bevat de informatie die is geüpload naar Azure. Als u uw certificaat open met Kladblok en deze niet ziet er ongeveer als volgt, doorgaans dit betekent dat u hebt niet het exporteren met behulp van de Base-64 encoded X.509 (. CER)-indeling. Als u een andere teksteditor gebruiken wilt, moet u bovendien begrijpen dat sommige editors kunnen leiden tot onverwachte opmaak op de achtergrond. Dit kan leiden tot problemen bij het uploaden van de tekst van dit certificaat naar Azure.

   ![Open met Kladblok](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)
