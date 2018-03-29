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
ms.openlocfilehash: b2a96aad793d956b3ea3de06fba74bcf68e50786
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
U kunt een basiscertificaat gebruiken dat is gegenereerd met een commerciële oplossing (aanbevolen) of u kunt een zelfondertekend certificaat genereren. Nadat u het basiscertificaat hebt gemaakt, exporteert u de gegevens van het openbare certificaat (niet de persoonlijke sleutel) als een X.509 CER-bestand dat is gecodeerd met Base64 en uploadt u de gegevens van het openbare certificaat naar Azure.

* **Commercieel certificaat:** als u gebruikmaakt van een commerciële oplossing kunt u de bestaande certificaatketen gebruiken. Verkrijg het CER-bestand dat u wilt gebruiken voor het basiscertificaat.
* **Zelfondertekend basiscertificaat:** als u geen commerciële certificeringsoplossing gebruikt, moet u een zelfondertekend basiscertificaat maken. Het is belangrijk dat u de stappen in een van de onderstaande P2S-certificaatartikelen volgt. Anders zijn de certificaten die u maakt niet compatibel met P2S-verbindingen en treedt er voor clients een verbindingsfout op tijdens het verbinden. U kunt Azure PowerShell, MakeCert of OpenSSL gebruiken. De stappen in de opgegeven artikelen zijn bedoeld om een compatibel certificaat te maken:

  * [Instructies voor Windows 10 PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): bij deze instructies zijn Windows 10 en PowerShell vereist voor het genereren van certificaten. Clientcertificaten die worden gegenereerd op basis van het basiscertificaat kunnen op alle ondersteunde P2S-clients worden ondersteund.
  * [MakeCert-instructies](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): gebruik MakeCert als u geen toegang hebt tot een Windows 10-computer om certificaten te genereren. MakeCert is buiten gebruik gesteld, maar u kunt het nog wel gebruiken om certificaten te genereren. Clientcertificaten die worden gegenereerd op basis van het basiscertificaat kunnen op alle ondersteunde P2S-clients worden ondersteund.
