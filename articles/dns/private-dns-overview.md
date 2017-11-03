---
title: Azure DNS gebruiken voor persoonlijke domeinen | Microsoft Docs
description: Overzicht van persoonlijke DNS hostingservice op Microsoft Azure.
services: dns
documentationcenter: na
author: garbrad
manager: 
editor: 
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2017
ms.author: garbrad
ms.openlocfilehash: ed47a9d850995aaf9e78bfde8b6a5fd80dc36918
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-azure-dns-for-private-domains"></a>Azure DNS gebruiken voor persoonlijke domeinen
Het Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een service het IP-adres. Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van de Microsoft Azure-infrastructuur.  Naast internet gerichte DNS-domeinen ondersteunt Azure DNS ook nu persoonlijke DNS-domeinen als preview-functie.  
 
Azure DNS biedt een betrouwbare, veilige DNS-service wilt beheren en oplossen van domeinnamen in een virtueel netwerk zonder te hoeven toevoegen van een aangepaste DNS-oplossing. Persoonlijke DNS-zones kunnen u uw eigen aangepaste domeinnamen in plaats van de beschikbare vandaag nog Azure verschafte namen gebruiken.  Aangepaste domeinnamen helpt u bij het aanpassen van de virtuele netwerkarchitectuur zelf behoeften van uw organisatie. Het biedt naamomzetting voor virtuele machines binnen een virtueel netwerk en tussen virtuele netwerken. U kunt bovendien de namen van de zones configureren met een weergave split-horizon - zodat een persoonlijke en een openbare DNS-zone voor het delen van dezelfde naam.

![DNS-overzicht](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="benefits"></a>Voordelen

* **Verwijdert de behoefte voor aangepaste DNS-oplossingen.** Eerder gemaakt veel klanten aangepaste DNS-oplossingen voor het beheren van DNS-zones in het virtuele netwerk.  DNS-zone management kan nu worden gedaan met behulp van de systeemeigen infrastructuur van Azure, waardoor de last van maken en beheren van aangepaste DNS-oplossingen.

* **Alle algemene DNS-records typen gebruiken.**  Azure DNS ondersteunt A, AAAA, CNAME, MX, NS, PTR, SOA, SRV en TXT-records.

* **Beheer van automatische hostname-record.** Samen met de aangepaste DNS-records hosten, bewaart Azure automatisch hostnaam records voor de virtuele machines in de opgegeven virtuele netwerken.  Hiermee kunt u voor het optimaliseren van de domeinnamen gebruiken zonder te maken van aangepaste DNS-oplossingen of toepassing wijzigen.

* **Hostnaamomzetting tussen virtuele netwerken.** In tegenstelling tot Azure verschafte hostnamen, kunnen de persoonlijke DNS-zones worden gedeeld tussen virtuele netwerken.  Deze mogelijkheid vereenvoudigt de detectie van cross-netwerk en service-scenario's zoals het virtuele netwerk peering.

* **Vertrouwde hulpprogramma's en gebruikerservaring.** Als u het leerproces, deze nieuwe aanbieding gebruikt de al goed tot stand gebrachte Azure DNS-hulpprogramma's (PowerShell Resource Manager-sjablonen, REST-API) en ondersteuning wordt toegevoegd aan de CLI en de portal zo snel mogelijk.

* **Ondersteuning van split-horizon DNS.** Azure DNS kunt u zones maken met dezelfde naam omzetten in verschillende antwoorden van vanuit een virtueel netwerk en het openbare Internet.  Er is een typisch scenario voor split-horizon DNS voor een specifieke versie van een service voor gebruik in uw virtuele netwerk.


## <a name="pricing"></a>Prijzen

Persoonlijke DNS-zones worden kosteloos tijdens de beheerde preview. Deze functie gebruikt een op gebruik gebaseerde prijsmodel vergelijkbaar met de bestaande Azure DNS-server biedt op aankondiging van de algemene beschikbaarheid. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [een persoonlijke DNS-zone maken](./private-dns-getstarted-powershell.md) in Azure DNS.

Meer informatie over DNS-zones en records in via: [DNS-zones en registreert overzicht](dns-zones-records.md).

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.

