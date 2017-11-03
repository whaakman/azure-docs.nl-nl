---
title: Testen van uw VM-aanbieding voor de Marketplace | Microsoft Docs
description: Begrijpen hoe u voor het testen van uw VM-installatiekopie voor Azure Marketplace.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: 26f856059b381be91b9cdd1f98a11dc90813c0c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testen van uw VM-aanbieding voor Azure Marketplace in fasering
Fasering betekent dat uw SKU in een particulier 'sandbox"waar u kunt testen en valideren van de functionaliteit ervan voordat u deze implementeert in de Marketplace te implementeren. De SKU wordt weergegeven in de faseringsmodus net zoals voor een klant die is geïmplementeerd. Uw VM-installatiekopie moet worden doorgegeven voor fasering worden gecertificeerd.

## <a name="step-1-push-your-offer-to-staging"></a>Stap 1: Uw aanbieding voor fasering Push
1. Op de **publiceren** tabblad **Push voor fasering**.
   
    ![tekenen](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Als de Publishing Portal een melding van fouten, corrigeer deze.
3. In de **wie toegang heeft tot uw voorbereide aanbieding?** dialoogvenster en voer de lijst met Azure-abonnementen die u gebruiken wilt voor de preview van uw aanbieding in de [Azure preview-portal](https://portal.azure.com).
   
   > [!NOTE]
   > In geval van virtuele Machines en oplossingssjablonen, neem **niet** geaccepteerde abonnementen van het type Cryptografieprovider DreamSpark of Azure in Open.
   > 
   > 

    > In geval van virtuele Machines, als u op de knop **PUSH FASERING**, de volgende stappen achter de scene worden uitgevoerd. U mag de voortgang van elke stap op het tabblad publiceren weergeven bij het publiceren portal. U moet controleren op deze pagina op een vast interval (totdat de status tijdelijke geeft) voor alle foutinformatie die correctie van uw nodig.

    > - In eerste instantie gaat uw staging aanvraag naar het certificeringsinstantie-team dat de vhd te valideren. Echter, als uw aanvraag is alleen marketing wijzigen, klikt u vervolgens de certificeringsinstantie stap overgeslagen.
    > - Zodra de certificeringsinstantie is voltooid, start de replicatie van de aanbieding in de Azure datacenters. In het algemeen neemt 24-48hours voor de replicatie is voltooid maar een week, afhankelijk van de grootte van de vhd kan duren. Als uw aanvraag is alleen marketing wijzigen, klikt u vervolgens is de replicatie echter sneller.
    > - Wanneer de replicatie voltooid is, klikt u vervolgens de aanbieding is beschikbaar in de [Azure-portal](http:/portal.azure.com). AT die als de status in de publicatie worden voorbereid portal. Een gefaseerde aanbieding is zichtbaar in de [Azure-portal](http:/portal.azure.com) alleen met behulp van de e-id('s) gekoppeld aan het abonnement waarmee de aanbieding tijdelijk worden opgeslagen.

1. Aanmelden bij de [Azure preview-portal](https://portal.azure.com) via een van de Azure-abonnementen die worden vermeld in de vorige stap.
2. Zoek uw aanbieding en valideren van de VM-installatiekopie punten:
   
   * Zorg ervoor dat inhoud marketing correct in de Marketplace weergegeven.
   * End-to-end-implementatie van de VM-installatiekopie.
     
      ![IMG-kaart-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Uw aanbieding blijft in de faseringsmodus totdat u Microsoft via het Publishing Portal waarschuwen [**publiceren** tabblad > Klik op de knop **'Aanvragen goedkeuring naar Push naar productie'**] bent u klaar om naar te pushen productie. Dit is een ideale tijd dat alle leden van de controle van uw team via alles ter voorbereiding op uw aanbieding gaat van vermelde.
> 
> De test platform is ontworpen voor het testen van de aanbieding in een preview-modus door de uitgever. We voorkomen raden dat deze platofrm gebruiken voor commerciële doeleinden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nu dat uw aanbieding 'gefaseerde"en u de functionaliteit ervan hebt getest en marketing inhoud, kunt u doorgaan met de laatste fase van de publicatie **stap 4**: [uw aanbieding implementeren op de Marketplace](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Zie ook
* [Aan de slag: hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)

