---
title: Azure-toepassingen beheerder Toepassingsbieding Publicatiehandleiding voor
description: In dit artikel beschrijft de vereisten voor het publiceren van een beheerde toepassing in de Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: qianw211
manager: evansma
ms.service: marketplace
ms.topic: article
ms.date: 06/14/2018
ms.author: v-qiwe
ms.openlocfilehash: 29546b0969751a43959a55860fc22e9f3c3e225b
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154938"
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
|Geïmplementeerd op Azure-abonnement van een klant | Beheerde Apps moeten worden geïmplementeerd in het abonnement van de klant en kunnen worden beheerd door een derde partij. | 
|Facturering en betaling naar gebruik    |  De resources worden ingericht in Azure-abonnement van de klant. Betalen per gebruik (PAYGO) virtuele machines worden met de klant via Microsoft, in rekening gebracht via een van de klant Azure-abonnement (PAYGO) worden verwerkt. <br> Terwijl Microsoft brengt kosten in rekening infrastructuurkosten in rekening gebracht bij de klantabonnement, wordt u in het geval van een bring-your-own-license, de software licensing kosten voor de klant rechtstreeks handelen.        |
|Azure-compatibele virtuele harde schijf (VHD)    |   Virtuele machines moeten worden gebouwd op Windows of Linux.<ul> <ul> <li>Zie voor meer informatie over het maken van een Linux VHD [op Azure onderschreven Linux-distributies](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Zie voor meer informatie over het maken van een Windows VHD [maken van een Azure-compatibele VHD](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |

>[!NOTE]
> Beheerde apps moeten worden geïmplementeerd via Marketplace zijn. Als communicatie met klanten een probleem is, u moet contact opnemen met geïnteresseerde klanten nadat u het delen van leads hebt ingeschakeld.  

>[!Note]
>Cloud Solution Providers (CSP) partner kanaal aanmelden is nu beschikbaar.  Raadpleeg [Cloud Solution Providers](./cloud-solution-providers.md) voor meer informatie over marketingcontactpersonen van uw aanbieding via de Microsoft CSP partner kanalen.

## <a name="next-steps"></a>Volgende stappen
Als u dit nog niet hebt gedaan, 

- [Registreren](https://azuremarketplace.microsoft.com/sell) in de marketplace.

Als u bent geregistreerd en het maken van een nieuwe aanbieding of werken aan een bestaande resourcegroep

- [Meld u aan voor de Cloud Partner Portal](https://cloudpartner.azure.com) maken of uitvoeren van uw aanbieding.
