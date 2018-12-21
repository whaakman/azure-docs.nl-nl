---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.openlocfilehash: 7ae3886db6391836cd8d281e44c95c5253cc8dd5
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323692"
---
Op elke clientcomputer die u verbindt met een VNet via een punt-naar-site-verbinding, moet een clientcertificaat zijn geïnstalleerd. U genereert het clientcertificaat op basis van het basiscertificaat en installeert het clientcertificaat op elke clientcomputer. Als u geen geldig clientcertificaat hebt geïnstalleerd en de client probeert verbinding te maken met het VNet, mislukt de verificatie.

U kunt een uniek certificaat genereren voor elke client of hetzelfde certificaat gebruiken voor meerdere clients. Het voordeel van het genereren van unieke clientcertificaten is dat het mogelijk is om één certificaat in te trekken. Anders moet u, als meerdere clients hetzelfde certificaat voor verificatie gebruiken en u het certificaat intrekt, nieuwe certificaten genereren en installeren voor elke client die dat certificaat voor verificatie gebruikt.

U kunt clientcertificaten genereren op de volgende manieren:

- **Commercieel certificaat**

  - Als u een commerciële certificeringsoplossing gebruikt, genereert u een clientcertificaat met de algemene waarde-indeling *name@yourdomain.com*. Gebruik deze indeling in plaats van de indeling *domeinnaam\gebruikersnaam*.
  - Zorg ervoor dat het clientcertificaat dat u verleent, is gebaseerd op de certificaatsjabloon voor *Gebruiker met Clientverificatie* als het eerste item in de lijst. U kunt het certificaat controleren door op het clientcertificaat te dubbelklikken en **Enhanced Key Usage**  (Uitgebreid sleutelgebruik) weer te geven op het tabblad **Details**.

- **Zelfondertekend basiscertificaat**: Volg de stappen in een van de volgende artikelen over P2S-certificaten zodat de clientcertificaten die u maakt compatibel zijn met P2S-verbindingen. De stappen in deze artikelen zijn bedoeld om een compatibel clientcertificaat te maken: 

  * [Instructies voor Windows 10 PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): Bij deze instructies zijn Windows 10 en PowerShell vereist voor het genereren van certificaten. De gegenereerde certificaten kunnen worden geïnstalleerd op alle ondersteunde P2S-clients.
  * [Instructies voor MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Gebruik MakeCert als u geen toegang hebt tot een Windows 10-computer voor het genereren van certificaten. MakeCert is inmiddels afgeschaft, maar u kunt het nog wel gebruiken om certificaten te genereren. U kunt de gegenereerde certificaten installeren op alle ondersteunde P2S-clients.
  * [Instructies voor Linux](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)

  Als u een clientcertificaat genereert op basis van een zelfondertekend basiscertificaat, wordt het certificaat automatisch geïnstalleerd op de computer die u hebt gebruikt om het certificaat te genereren. Als u een clientcertificaat op een andere clientcomputer wilt installeren, moet u het certificaat samen met de gehele certificaatketen exporteren als PFX-bestand. Hiermee maakt u een PFX-bestand met alle gegevens van het basiscertificaat die nodig zijn voor de verificatie van de client. Als u een certificaat wilt exporteren, raadpleegt u de stappen voor het [genereren en exporteren van certificaten via een punt-naar-site-verbinding met behulp van PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).