---
title: Standard Contract | Azure
description: Standard-Contract in Azure Marketplace en AppSource
services: Azure, Marketplace, Compute, Storage, Networking
author: qianw211
ms.service: marketplace
ms.topic: article
ms.date: 07/05/2019
ms.author: ellacroi
ms.openlocfilehash: 80c157423572d356026f257e81d52650ce01d3e8
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620394"
---
# <a name="standard-contract"></a>Standaardcontract

Voor het vereenvoudigen van het aankoopproces voor klanten en juridische vanwege de complexiteit van softwareleveranciers, biedt Microsoft een Standard Contract-sjabloon om een transactie in de marketplace te vergemakkelijken. In plaats van het samenstellen van aangepaste voorwaarden en bepalingen, kunt Azure Marketplace-uitgevers bieden van hun software onder het standaardcontract, klanten alleen hoeven om te screenen en één keer te accepteren. Het standaardcontract vindt u hier: [ https://go.microsoft.com/fwlink/?linkid=2041178 ](https://go.microsoft.com/fwlink/?linkid=2041178). 

De voorwaarden en bepalingen voor een aanbieding zijn gedefinieerd op het tabblad Marketplace bij het maken van een aanbieding in de Cloud Partner-Portal. De optie standaardcontract wordt ingeschakeld door de instelling wijzigen op Ja.

![De optie standaardcontract inschakelen](media/marketplace-publishers-guide/standard-contract.png)

>[!Note] 
>Als u gebruiken de standaardcontract wilt, afzonderlijke voorwaarden en bepalingen zijn nog steeds vereist zijn voor de [Cloud Solution Provider](./cloud-solution-providers.md) kanaal.

## <a name="standard-contract-amendments"></a>Standard Contract wijzigingen

Standard Contract wijzigingen kunnen uitgevers om te selecteren van het standaard contract voor het gemak en aangepaste voorwaarden voor hun product of bedrijf.  Klanten moeten alleen de wijzigingen van het contract, bekijken als ze al hebt gecontroleerd en geaccepteerd van de standaard Microsoft-overeenkomst.

Er zijn twee soorten wijzigingen beschikbaar voor Azure Marketplace-uitgevers:

* Universele wijzigingen: Deze wijzigingen worden universeel toegepast op het standaard contract voor alle klanten. Universele wijzigingen worden weergegeven voor elke klant van het product in de stroom aankoop.

![Universele wijzigingen](media/marketplace-publishers-guide/universal-amendaments.png)

* Aangepaste wijzigingen: Azure Marketplace bevat ook een voorziening voor aangepaste wijzigingen die zijn gericht aan tenants. Ze zijn speciale wijzigingen in de standard-overeenkomst die bedoeld zijn voor bepaalde klanten alleen. Uitgevers kunnen de tenant die ze willen als doel kiezen. Klanten van deze tenant zou het product op Standard Contract en de wijzigingen van de betreffende kopen.

![Aangepaste wijzigingen](media/marketplace-publishers-guide/custom-amendaments.png)

>[!Note] 
>Klanten met aangepaste wijzigingen wordt benaderd krijgen ook de universele wijziging van de voorwaarden van de standaard tijdens de aankoop.

>[!Note]
>De volgende typen ondersteunen standaard wijzigingen van overeenkomst: Azure-toepassingen (Oplossingssjablonen en beheerde toepassingen), virtuele Machines, Containers, containertoepassingen.

### <a name="customer-experience"></a>Ervaring van de klant

Klanten, worden de voorwaarden die zijn gekoppeld aan het product als het standaard Microsoft-Contract en de wijzigingen zien tijdens het aankoopproces in Azure portal.

![De klant van Azure portal-ervaring.](media/marketplace-publishers-guide/ibiza-customer-experience.png)

### <a name="api"></a>API

Klanten kunnen gebruiken `Get-AzureRmMarketplaceTerms` aan de voorwaarden van een aanbieding ophalen en accepteren. De standaard contract en de bijbehorende wijzigingen worden geretourneerd in de uitvoer van de cmdlet.

---
