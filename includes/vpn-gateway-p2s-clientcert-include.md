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
ms.openlocfilehash: 8a49653b4083cbfd17656d701225dcb14f91f637
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. Het clientcertificaat wordt gegenereerd op basis van het basiscertificaat en wordt op elke clientcomputer geïnstalleerd. Als er geen geldig clientcertificaat is geïnstalleerd en de client probeert verbinding te maken met het VNet, mislukt de verificatie.

U kunt een uniek certificaat genereren voor elke client of hetzelfde certificaat gebruiken voor meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is dat het mogelijk is om één certificaat in te trekken. Anders moet u, als meerdere clients hetzelfde certificaat gebruiken en u het certificaat voor een van de clients wilt intrekken, nieuwe certificaten genereren en installeren voor alle clients die het certificaat voor verificatie gebruiken.

U kunt clientcertificaten genereren via de volgende methoden:

- **Commercieel certificaat**

  - Als u een commerciële certificeringsoplossing gebruikt, genereert u een clientcertificaat met de algemene waarde-indeling 'name@yourdomain.com', in plaats van de indeling 'domeinnaam\gebruikersnaam'.
  - Zorg ervoor dat het clientcertificaat dat u verleent, is gebaseerd op de certificaatsjabloon voor Gebruiker met Clientverificatie als het eerste item in de lijst, in plaats van bijvoorbeeld smartcardaanmelding. U kunt het certificaat controleren door op het clientcertificaat te dubbelklikken en **Details > Uitgebreid sleutelgebruik** weer te geven.

- **Zelfondertekend basiscertificaat:** het is belangrijk dat u de stappen in een van de onderstaande P2S-certificaatartikelen volgt. Anders zijn de clientcertificaten die u maakt niet compatibel met P2S-verbindingen en treedt er voor clients een fout op tijdens het verbinden. De stappen in de volgende artikelen zijn bedoeld om een compatibel clientcertificaat te maken: 

  * [Instructies voor Windows 10 PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): bij deze instructies zijn Windows 10 en PowerShell vereist voor het genereren van certificaten. De certificaten die worden gegenereerd, kunnen op alle ondersteunde P2S-clients worden ondersteund.
  * [MakeCert-instructies](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): gebruik MakeCert als u geen toegang hebt tot een Windows 10-computer om certificaten te genereren. MakeCert is buiten gebruik gesteld, maar u kunt het nog wel gebruiken om certificaten te genereren. De certificaten die worden gegenereerd, kunnen op alle ondersteunde P2S-clients worden ondersteund.

  Als u aan de hand van de voorgaande instructies een clientcertificaat genereert van een zelfondertekend basiscertificaat, wordt het certificaat automatisch geïnstalleerd op de computer die u hebt gebruikt om het certificaat te genereren. Als u een clientcertificaat op een andere clientcomputer wilt installeren, moet u het certificaat als .pfx exporteren, samen met de gehele certificaatketen. Hiermee maakt u een .pfx-bestand dat de gegevens van het basiscertificaat bevat die nodig zijn om de client succesvol te laten verifiëren. Zie [Certificaten - een clientcertificaat exporteren](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport) voor informatie over het exporteren van een certificaat.