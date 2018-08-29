---
title: Publicatiehandleiding voor voor Azure Marketplace Virtual Machine-aanbieding
description: Dit artikel beschrijft de vereisten voor het publiceren van een virtuele machine en een gratis proefversie van de software te worden geïmplementeerd vanaf de Marketplace.
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
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: b8caeab7f08ffeee81492b01750cbb255e172872
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144519"
---
# <a name="virtual-machine-offer-publishing-guide"></a>Publicatiehandleiding voor virtuele Machine-aanbieding

Installatiekopieën van virtuele machines vormen een van de manieren voor het publiceren van een oplossing op Azure Marketplace. Deze handleiding gebruiken om informatie over de systeemvereisten voor deze aanbieding. 

Dit zijn de transactie-aanbiedingen die zijn geïmplementeerd en worden gefactureerd via de Marketplace. De aanroep naar de actie die door een gebruiker is "Nu downloaden."

## <a name="free-trial"></a>Gratis proefversie 

U kunt ervoor zorgen dat gebruikers voor het testen van uw aanbieding door het openen van softwarelicenties beperkte termijn bij het gebruik van het model van de blling doen om uw eigen licentie (BYOL). Hieronder vindt u de vereisten voor het implementeren van deze aanbieding. 

|Vereisten  |Details  |
|---------|---------|
|Gratis proefperiode is afgelopen en evaluatiesoftware     |   De klant kan uw app gratis uitproberen gedurende een beperkte periode. De klant is niet vereist een licentie of abonnement om kosten te betalen voor uw aanbieding. Uw klanten zijn niet vereist om te betalen voor de onderliggende Microsoft eigen product of service. Alle opties voor proefversie worden geïmplementeerd naar uw Azure-abonnement. Hebt u enige controle van de kosten optimaliseren en het beheer. U kunt een gratis proefversie of interactieve demo. Ongeacht wat u kiest dient uw gratis proefversie de klant een vooraf bepaalde hoeveelheid tijd om te proberen uw aanbieding zonder extra kosten.|
|Eenvoudig kunnen worden geconfigureerd, kant-en-klare oplossing    |  Uw app moet zijn eenvoudig en snel te configureren en instellen.       |
|Beschikbaarheid / uptime    |    Uw SaaS-app of -platform moet een uptime van ten minste 99,9% hebben.     |
|Azure Active Directory     |    Uw aanbieding moet toestaan dat Azure Active Directory (Azure AD) voor federatieve eenmalige aanmelding (SSO) (Azure AD federatieve SSO) met toestemming ingeschakeld.     |

## <a name="test-drive"></a>Test Drive

Een of meer virtuele machines via infrastructure-as-a-service(IaaS) of SaaS-apps te implementeren. Een voordeel van test drive publicatieoptie is de geautomatiseerde inrichting van een virtuele machine of de gehele oplossing onder leiding van een partner gehoste rondleiding. Een test uit biedt een evaluatie zonder extra kosten voor uw klant. De klant hoeft niet te worden van een bestaande Azure-klant contact opnemen met de proefversie. 

Neem contact op met ons op [amp testdrive](mailto:amp-testdrive@microsoft.com) aan de slag. 

|Vereisten  |Details |
|---------|---------|
| U hebt een Marketplace-app   |    Een of meer virtuele machines via IaaS of SaaS.      |

## <a name="interactive-demo"></a>Interactieve demo

U kunt een begeleide ervaring van uw oplossing opgeven voor uw klanten met behulp van een interactieve demonstratie. Het voordeel van interactieve demo optie publiceren is dat u een proefversie ervaring bieden zonder ingewikkelde inrichting van uw complexe oplossing. 

## <a name="virtual-machine-offer"></a>Virtuele Machine-aanbieding

Het type van de aanbieding virtuele machine gebruiken wanneer u een virtueel apparaat voor het abonnement is gekoppeld aan uw klant implementeert. VM's zijn volledig ingeschakeld met behulp van betalen per gebruik of bring-your-own-license (BYOL)-licentiemodellen commerce. Microsoft als host fungeert voor de commercetransactie en uw klant namens u kosten in rekening gebracht. U profiteren van de voordelen van het gebruik van de gewenste betaling relatie tussen de klant en Microsoft, zoals een Enterprise-overeenkomsten.

>[!NOTE]
>Op dit moment kunnen de monetaire toezeggingen die zijn gekoppeld aan een Enterprise Agreement worden gebruikt voor het gebruik van uw virtuele machine van Azure, maar niet op basis van uw kosten voor softwarelicenties.  

>[!NOTE]
>U zijn kunt de detectie en implementatie van uw virtuele machine naar een specifieke set van klanten beperken door de installatiekopie van het publiceren en prijzen als een persoonlijke aanbieding. Persoonlijke aanbiedingen Ontgrendel de mogelijkheid voor u exclusieve aanbiedingen maken voor uw dichtstbijzijnde klanten en bieden van aangepaste software en de voorwaarden. De aangepaste voorwaarden kunnen u een verscheidenheid aan scenario's, waaronder deals onder leiding van een veld met speciale prijzen en voorwaarden, evenals vroege toegang tot beperkte versie van de software te markeren. Persoonlijke aanbiedingen inschakelen u kunt specifieke prijzen bieden of producten voor een beperkt aantal klanten door het maken van een nieuwe SKU met de gegevens.  
*   Voor meer informatie over het privé-aanbiedingen gaat u naar de persoonlijke aanbiedingen op Azure Marketplace-pagina te vinden op [azure.microsoft.com/blog/private-offers-on-azure-marketplace](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace).  

| Vereiste | Details |  
|:--- |:--- | 
| Facturering en betaling naar gebruik | Uw virtuele machine moet ondersteunen BYOL of betalen per gebruik maandelijkse facturering. |  
| Azure-compatibele virtuele harde schijf (VHD) | Virtuele machines moeten worden gebouwd op Windows of Linux.<ul> <li>Voor meer informatie over het maken van een VHD met Linux-, gaat u naar de maken een sectie (Linux-indeling) met Azure compatibele VHD zich bevindt op [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2- Create-an-Azure-compatible-VHD-Linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Voor meer informatie over het maken van een Windows VHD, gaat u naar de maken een sectie (Windows-indeling) met Azure compatibele VHD zich bevindt op [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3- Create-an-Azure-compatible-VHD-Windows-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul> |  

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan, 

- [Registreren](https://azuremarketplace.microsoft.com/sell) in de marketplace

Als u bent geregistreerd en het maken van een nieuwe aanbieding of werken aan een bestaande resourcegroep

- [Meld u aan bij de Cloud Partner-Portal](https://cloudpartner.azure.com) maken of uw aanbieding voltooien
