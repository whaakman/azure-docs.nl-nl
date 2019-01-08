---
title: Azure-toepassingen beheerder Toepassingsbieding Publicatiehandleiding voor
description: In dit artikel beschrijft de vereisten voor het publiceren van een beheerde toepassing in de Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 12/19/2018
ms.author: ellacroi
ms.openlocfilehash: ae50562a9354b9c1e5b2711e5bbeec7b1e0fbbb8
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54078793"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Azure-toepassingen: Beheerder Toepassingsbieding Publicatiehandleiding voor

Een beheerde toepassing is een van de manieren voor het publiceren van een oplossing in de Marketplace. Deze handleiding gebruiken om informatie over de systeemvereisten voor deze aanbieding. 

Dit zijn de transactie-aanbiedingen die zijn geïmplementeerd en worden gefactureerd via de Marketplace. De aanroep naar de actie die door een gebruiker is "Nu downloaden."

Gebruik de Azure-app: app-aanbieding voor type beheerd wanneer de volgende voorwaarden vereist zijn:
- U kunt ofwel een oplossing implementeren voor uw klanten met behulp van een virtuele machine of een volledige IaaS-oplossing.
- U of uw klant vereisen dat de oplossing wordt beheerd door een partner.

>[!NOTE]
>Een partner zijn mogelijk een SI of beheerde-serviceprovider (MSP).  

## <a name="managed-application-offer"></a>Beheerde toepassing aanbieding

|Vereisten |Details  |
|---------|---------|
|Geïmplementeerd op Azure-abonnement van een klant | Beheerde Apps moeten worden geïmplementeerd in het abonnement van de klant en kunnen worden beheerd door een 3e partij | 
|Facturering en betaling naar gebruik    |  De resources worden ingericht in Azure-abonnement van de klant. Betalen per gebruik (PAYGO) virtuele machines zullen worden verwerkt met de klant via Microsoft, in rekening gebracht via een van de klant Azure-abonnement (PAYGO) 
In het geval van een bring-your-own-license, terwijl Microsoft brengt kosten in rekening infrastructuurkosten in rekening gebracht bij de klantabonnement, wordt u transact uw softwarelicenties kosten voor de klant rechtstreeks        |
|Azure-compatibele virtuele harde schijf (VHD)    |   Virtuele machines moeten worden gebouwd op Windows of Linux.<ul> <ul> <li>Zie voor meer informatie over het maken van een Linux VHD [op Azure onderschreven Linux-distributies](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Zie voor meer informatie over het maken van een Windows VHD [maken van een Azure-compatibele VHD](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |

>[!NOTE]
> Beheerde apps moeten worden geïmplementeerd via Marketplace zijn. Als communicatie met klanten een probleem is, u moet contact opnemen met geïnteresseerde klanten nadat u het delen van leads hebt ingeschakeld.  


## <a name="next-steps"></a>Volgende stappen
Als u dit nog niet hebt gedaan, 

- [Registreren](https://azuremarketplace.microsoft.com/sell) in de marketplace.

Als u bent geregistreerd en het maken van een nieuwe aanbieding of werken aan een bestaande resourcegroep

- [Meld u aan bij de Cloud Partner-Portal](https://cloudpartner.azure.com) maken of uitvoeren van uw aanbieding.
