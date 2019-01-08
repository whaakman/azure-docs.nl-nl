---
title: Testen van uw VM-aanbieding voor Marketplace | Microsoft Docs
description: Lees hoe u voor het testen van uw VM-installatiekopie voor de Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: b90353dbbc5d019897735cfc05caa3ee094dfedc
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54078810"
---
# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testen van uw VM-aanbieding voor Azure Marketplace in fasering
Fasering houdt in het implementeren van uw SKU in een privé-"sandbox" waar u kunt testen en valideren van de functionaliteit ervan voorafgaand aan implementatie naar de Marketplace. De SKU wordt weergegeven in de faseringsmodus net zoals bij een klant die is geïmplementeerd. U moet uw VM-installatiekopie certificeren voordat deze wordt doorgestuurd voor fasering.

## <a name="step-1-push-your-offer-to-staging"></a>Stap 1: Push uw aanbieding voor fasering
1. Op de **publiceren** tabblad **overgaat tot fasering**.
   
    ![tekenen](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Als de Portal voor publiceren de hoogte van eventuele fouten gebracht, corrigeert u deze.
3. In de **wie toegang heeft tot uw aanbieding gefaseerde?** dialoogvenster vak, voert u de lijst van Azure-abonnementen die u gebruiken wilt om een voorbeeld van uw aanbieding in de [Azure-portal](https://portal.azure.com).
   
   > [!NOTE]
   > In het geval van virtuele Machines en sjablonen van de oplossing, neem **niet** whitelist abonnementen van het type Cryptografieprovider DreamSpark of Azure in Open.
   > 
   > 
   >
   > In het geval van virtuele Machines, wanneer u op de knop klikt **FASERING naar**, de volgende stappen uit achter de scène worden uitgevoerd. U wordt mogelijk zijn om de voortgang van elke stap onder het tabblad ' PUBLISH ' in de publicatie portal. U moet controleren op deze pagina op een vast interval (totdat de status KLAARGEZET weergegeven) voor alle informatie over de fout die correctie van uw nodig.

   > - In eerste instantie wordt uw aanvraag voor gefaseerde installatie gerouteerd naar het certificeringsinstantie-team dat de vhd te valideren. Echter, als uw aanvraag maar alleen marketing wijzigen liefst, klikt u vervolgens de certificeringsstap overgeslagen.
   > - Zodra het certificaat is voltooid, start de replicatie van de aanbieding in alle de datacenters van Azure. In het algemeen neemt 24-48hours voor de replicatie is voltooid maar duurt ongeveer een week, afhankelijk van de grootte van de vhd. Als uw aanvraag maar alleen marketing wijzigen liefst, klikt u vervolgens is de replicatie echter sneller.
   > - Wanneer de replicatie voltooid is, klikt u vervolgens de aanbieding is beschikbaar in de [Azure-portal](http:/portal.azure.com). Op die de status van de tijd worden KLAARGEZET in de publicatie portal. Een gefaseerde aanbieding wordt weergegeven in de [Azure-portal](http:/portal.azure.com) alleen met behulp van de e-mail (s) die zijn gekoppeld aan het abonnement waarmee de aanbieding tijdelijk worden opgeslagen.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een van de Azure-abonnementen die worden vermeld in de vorige stap.
2. Zoek uw aanbieding en valideren van uw VM-installatiekopie punten:
   
   * Zorg ervoor dat inhoud marketing goed in de Marketplace weergegeven.
   * De implementatie van de end-to-end van de VM-installatiekopie.
     
      ![IMG-kaart-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Uw aanbieding blijft in het faseringsproces totdat u op de hoogte bij Microsoft terechtgekomen via de Portal voor publiceren stellen [**publiceren** tabblad > Klik op de knop **'Request goedkeuring naar Push naar productie'**] bent u klaar om naar te pushen de productie. Dit is een ideaal moment om alle leden van de controle van uw team hebben ter voorbereiding vermelde zetten van uw aanbieding.
> 
> Het staging-platform is ontworpen voor het testen van de aanbieding in een preview-modus door de uitgever. We raden het sterk af met behulp van deze platformen voor commerciële doeleinden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nu dat uw aanbieding "klaargezet' en u de functionaliteit ervan hebt getest en marketing inhoud, kunt u doorgaan met de laatste fase van de publicatie **stap 4**: [Implementeren van uw aanbieding op Marketplace](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Zie ook
* [Aan de slag: Een aanbieding publiceren op Azure Marketplace](marketplace-publishing-getting-started.md)

