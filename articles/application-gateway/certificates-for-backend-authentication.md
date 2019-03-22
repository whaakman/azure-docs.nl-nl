---
title: Certificaten die vereist zijn voor opname in de whitelist back-ends via Azure Application Gateway
description: In dit artikel vindt u voorbeelden van hoe een SSL-certificaat kan worden geconverteerd naar het certificaat voor clientverificatie en vertrouwd basiscertificaat die nodig voor opname in de whitelist back-end-exemplaren in Azure Application Gateway zijn
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/14/2019
ms.author: absha
ms.openlocfilehash: 72ee9123ad959c0c7240d4f7a906adc1a4dd1a93
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260499"
---
# <a name="create-certificates-for-whitelisting-backend-with-azure-application-gateway"></a>Certificaten voor opname in de whitelist back-end met Azure Application Gateway maken

Als u end-to-end SSL, vereist application-gateway dat de back-end-exemplaren in de whitelist opgenomen door de verificatie/vertrouwde basiscertificaten uploaden. In het geval van v1-SKU, certificaten voor serververificatie zijn vereist dat de vertrouwde basiscertificaten zijn vereist voor opname in de whitelist in het geval van v2-SKU, de certificaten

In dit artikel leert u het volgende:

> [!div class="checklist"]
>
> - Certificaat voor serververificatie exporteren vanuit een back-end-certificaat (voor v1-SKU)
> - Vertrouwd basiscertificaat exporteren vanuit een back-end-certificaat (voor v2 SKU)

## <a name="prerequisites"></a>Vereisten

Hebt u nodig een bestaand back-end-certificaat voor het genereren van de certificaten voor serververificatie of de vertrouwde basiscertificaten vereist zijn voor opname in de whitelist back-end-exemplaren met application gateway. Het back-end-certificaat kan zijn hetzelfde als het SSL-certificaat of een ander voor extra beveiliging. Application gateway biedt u geen een mechanisme voor het maken of een SSL-certificaat kopen. U kunt een zelfondertekend certificaat maken voor testdoeleinden, maar moet u deze niet gebruiken voor productieworkloads. 

## <a name="export-authentication-certificate-for-v1-sku"></a>Certificaat voor serververificatie exporteren (voor v1-SKU)

Certificaat voor clientverificatie is vereist voor het goedgekeurde back-end-exemplaren in application gateway v1 SKU. Certificaat voor clientverificatie is de openbare sleutel van de back-in Base-64 encoded X.509 (. CER)-indeling. In dit voorbeeld, we gebruiken een SSL-certificaat voor de back-end-certificaat en exporteer de openbare sleutel moet worden gebruikt als de verificatie-certificering. Ook in dit voorbeeld gebruiken we het hulpprogramma Windows Certificate Manager om de vereiste certificaten te exporteren. U kunt een ander hulpprogramma aan de hand van uw gemak gebruiken.

Exporteer de openbare sleutel cer-bestand (niet de persoonlijke sleutel) van uw SSL-certificaat. De volgende stappen uit voor hulp bij het exporteren van het cer-bestand in de Base-64 encoded X.509 (. CER)-indeling voor het certificaat:

1. Als u een CER-bestand wilt genereren van het certificaat, opent u **Gebruikerscertificaten beheren**. Zoek het certificaat, meestal in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten', en met de rechtermuisknop op. Klik op **Alle taken** en vervolgens op **Exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**. Als u het certificaat in de huidige gebruiker\Persoonlijk\Certificaten niet vinden, mogelijk hebt u per ongeluk geopend 'Certificaten - lokale Computer, is' in plaats van 'Certificaten - huidige gebruiker'). Als u Certificate Manager openen in de huidige gebruikersbereik met behulp van PowerShell wilt, u typt *certmgr* in het consolevenster.

   ![Exporteren](./media/certificates-for-backend-authentication/export.png)

2. Klik in de wizard op **Volgende**.

   ![Certificaat exporteren](./media/certificates-for-backend-authentication/exportwizard.png)

3. Selecteer **Nee, de persoonlijke sleutel niet exporteren** en klik vervolgens op **Volgende**.

   ![De persoonlijke sleutel niet exporteren](./media/certificates-for-backend-authentication/notprivatekey.png)

4. Selecteer op de pagina **Bestandsindeling voor export** de optie **Met Base64 gecodeerde X.509 (.CER)** en klik op **Volgende**.

   ![Base-64 gecodeerd](./media/certificates-for-backend-authentication/base64.png)

5. Voor **te exporteren bestand**, **Bladeren** naar de locatie waarnaar u wilt dat het certificaat te exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.

   ![Bladeren](./media/certificates-for-backend-authentication/browse.png)

6. Klik op **Voltooien** om het certificaat te exporteren.

   ![Voltooien](./media/certificates-for-backend-authentication/finish.png)

7. Uw certificaat is geëxporteerd.

   ![Geslaagd](./media/certificates-for-backend-authentication/success.png)

   Het geëxporteerde certificaat ziet er ongeveer als volgt uit:

   ![Geëxporteerd](./media/certificates-for-backend-authentication/exported.png)

8. Als u het geëxporteerde certificaat met behulp van Kladblok opent, ziet er ongeveer uitzien als in dit voorbeeld. De sectie in het blauw bevat de informatie die wordt geüpload naar de toepassingsgateway. Als u uw certificaat open met Kladblok en deze niet ziet er ongeveer als volgt, doorgaans dit betekent dat u hebt niet het exporteren met behulp van de Base-64 encoded X.509 (. CER)-indeling. Als u een andere teksteditor gebruiken wilt, moet u bovendien begrijpen dat sommige editors kunnen leiden tot onverwachte opmaak op de achtergrond. Dit kan leiden tot problemen bij het uploaden van de tekst van dit certificaat naar Azure.

   ![Open met Kladblok](./media/certificates-for-backend-authentication/format.png)

## <a name="export-trusted-root-certificate-for-v2-sku"></a>Vertrouwd basiscertificaat exporteren (voor v2 SKU)

Vertrouwd basiscertificaat is vereist voor goedgekeurde back-end-exemplaren in application gateway v2 SKU. Het basiscertificaat is een met Base-64 encoded X.509 (. Basiscertificaat van CER)-indeling van de back-end-servercertificaten. In dit voorbeeld wordt we een SSL-certificaat gebruiken voor het back-end-certificaat, de openbare sleutel exporteren en vervolgens het basiscertificaat van de vertrouwde Certificeringsinstantie uit de openbare sleutel in base64-indeling om op te halen van het vertrouwde basiscertificaat te exporteren. 

De volgende stappen kunt u het cer-bestand voor het certificaat te exporteren:

1. Gebruik de stappen 1-9 vermeld in de sectie **verificatiecertificaat exporteren vanuit een back-end-certificaat (voor v1-SKU)** hierboven voor de openbare sleutel van uw back-end-certificaat exporteren.

2. Nadat de openbare sleutel is geëxporteerd, open het bestand.

   ![Open autorisatie-certificaat](./media/certificates-for-backend-authentication/openAuthcert.png)

   ![over het certificaat](./media/certificates-for-backend-authentication/general.png)

3. Verplaatsen naar het certificeringspad weer te geven de certificeringsinstantie (CA).

   ![details van certificaat](./media/certificates-for-backend-authentication/certdetails.png)

4. Selecteer het basiscertificaat en klik op **certificaat weergeven**.

   ![certificaat-pad](./media/certificates-for-backend-authentication/rootcert.png)

   U zou het mogelijk om de details van het root-certificaat weer te geven.

   ![certificaat-info](./media/certificates-for-backend-authentication/rootcertdetails.png)

5. Verplaatsen naar de **Details** bekijken en klik op **kopiëren naar bestand...**

   ![basiscertificaat kopiëren](./media/certificates-for-backend-authentication/rootcertcopytofile.png)

6. Op dit moment hebt u de details van het basiscertificaat opgehaald uit het back-end-certificaat. U ziet de **Wizard Certificaat exporteren**. Nu gebruik de stappen 2-9 vermeld in de sectie **verificatiecertificaat exporteren vanuit een back-end-certificaat (voor v1-SKU)** hierboven voor het exporteren van de vertrouwde basis-certificaat in de Base-64 encoded X.509 (. CER)-indeling.

## <a name="next-steps"></a>Volgende stappen

Nu kunt u de verificatie/certificaat wordt vertrouwd basiscertificaat in een Base-64 encoded X.509 (. CER)-indeling. U kunt deze toevoegen aan de toepassingsgateway aan lijst met geaccepteerde uw back-endservers voor end-to-end SSL-versleuteling Zie [end to end SSL-versleuteling configureren](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).