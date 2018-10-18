---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/06/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5ceb2d737083e2a218fc624c4e1a2f6d8fd0db1d
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49312447"
---
U kunt een basiscertificaat gebruiken dat is gegenereerd met een commerciële oplossing (aanbevolen) of u kunt een zelfondertekend certificaat genereren. Nadat u het basiscertificaat hebt gemaakt, exporteert u de gegevens van het openbare certificaat (niet de persoonlijke sleutel) als een X.509 CER-bestand dat is gecodeerd met Base64 en uploadt u de gegevens van het openbare certificaat naar Azure.

* **Commercieel certificaat:** als u gebruikmaakt van een commerciële oplossing kunt u de bestaande certificaatketen gebruiken. Verkrijg het CER-bestand dat u wilt gebruiken voor het basiscertificaat.
* **Zelfondertekend basiscertificaat:** als u geen commerciële certificeringsoplossing gebruikt, moet u een zelfondertekend basiscertificaat maken. Het is belangrijk dat u de stappen in een van de onderstaande P2S-certificaatartikelen volgt. Anders zijn de certificaten die u maakt niet compatibel met P2S-verbindingen en treedt er voor clients een verbindingsfout op tijdens het verbinden. U kunt Azure PowerShell, MakeCert of OpenSSL gebruiken. De stappen in de opgegeven artikelen zijn bedoeld om een compatibel certificaat te maken:

  * [Instructies voor Windows 10 PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): bij deze instructies zijn Windows 10 en PowerShell vereist voor het genereren van certificaten. Clientcertificaten die worden gegenereerd op basis van het basiscertificaat kunnen op alle ondersteunde P2S-clients worden ondersteund.
  * [MakeCert-instructies](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): gebruik MakeCert als u geen toegang hebt tot een Windows 10-computer om certificaten te genereren. MakeCert is buiten gebruik gesteld, maar u kunt het nog wel gebruiken om certificaten te genereren. Clientcertificaten die worden gegenereerd op basis van het basiscertificaat kunnen op alle ondersteunde P2S-clients worden ondersteund.
  * [Instructies voor Linux](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)
