---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/13/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: afd4836229c60ebef1536d4fa1ca4206a492e56d
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418106"
---
Na het maken van een zelfondertekend basiscertificaat, exporteer de openbare sleutel cer-basiscertificaatbestand (niet de persoonlijke sleutel). U wordt dit bestand later uploaden naar Azure. De volgende stappen kunt u het cer-bestand voor uw zelfondertekend basiscertificaat exporteren:

1. Als u een CER-bestand wilt genereren van het certificaat, opent u **Gebruikerscertificaten beheren**. Zoek het zelfondertekende basiscertificaat, meestal in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten', en klik er met de rechtermuisknop op. Klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**. Als u het certificaat in de huidige gebruiker\Persoonlijk\Certificaten niet vinden, mogelijk hebt u per ongeluk geopend 'Certificaten - lokale Computer, is' in plaats van 'Certificaten - huidige gebruiker'). Als u Certificate Manager openen in de huidige gebruikersbereik met behulp van PowerShell wilt, u typt *certmgr* in het consolevenster.

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
