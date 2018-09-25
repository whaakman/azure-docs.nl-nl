---
title: Azure-woordenlijst - Azure-woordenlijst | Microsoft Docs
description: Gebruik de Azure-woordenlijst om informatie over cloud-terminologie op het Azure-platform. Deze korte Azure woordenlijst bevat definities voor de algemene voorwaarden van de cloud voor Azure.
keywords: Woordenlijst voor Azure, terminologie van de cloud, Azure-woordenlijst, overzicht van termen die, cloud-voorwaarden
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: ''
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: monicar
ms.openlocfilehash: e62910cc760a200abba6d56e4fa1eb87973285ec
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963591"
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Verklarende woordenlijst voor Microsoft Azure: een woordenlijst met cloud-terminologie op het Azure-platform

De verklarende woordenlijst van Microsoft Azure is een korte woordenlijst met cloud-terminologie voor de Azure-platform. Zie ook:

* [Microsoft Azure en Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) -definities aan Azure-services en hun tegenhangers AWS.<!-- I propose to link to https://azure.microsoft.com/services/ instead of this -->
* [Cloud betrekking heeft op cloudcomputing](https://azure.microsoft.com/overview/cloud-computing-dictionary/) -algemene branche cloud voorwaarden.

## <a name="account"></a>account
Een account dat wordt gebruikt voor toegang tot en beheer van een Azure-abonnement. Dit wordt vaak aangeduid als een Azure-account Hoewel een account kan een van deze zijn: een bestaand werk, school, of persoonlijke Microsoft-account, of een Office 365-gebruikersnaam en wachtwoord. U kunt ook maken met een account voor het beheren van een Azure-abonnement wanneer u zich registreren voor de [gratis proefversie](https://azure.microsoft.com).  
Zie [zich aanmelden voor een Azure-abonnement met uw Office 365-account](billing/billing-use-existing-office-365-account-azure-subscription.md) en [Accounts kunt u aan te melden bij](active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="api-app"></a>API-app
Een andere naam voor [App Service-app](#app-service-app).

## <a name="app-service-app"></a>App Service-app
De rekenresources die [Azure App Service](app-service/app-service-web-overview.md) biedt voor het hosten van een website of web-toepassing, web-API of [mobiele back-end](app-service-mobile/app-service-mobile-value-prop.md). App Service-apps worden ook aangeduid als *App Services*, *web-apps*, *API apps*, en *mobiele apps*.

## <a name="availability-set"></a>Beschikbaarheidsset
Een verzameling van virtuele machines die samen worden beheerd voor toepassing redundantie en betrouwbaarheid. Het gebruik van een beschikbaarheidsset zorgt ervoor dat tijdens een geplande of onvoorziene onderhoudsgebeurtenis ten minste één virtuele machine beschikbaar is.  
Zie [de beschikbaarheid van virtuele machines van Windows beheren](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [de beschikbaarheid van virtuele Linux-machines beheren](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="classic-model"></a>Model van de klassieke Azure-implementatie
Een van de twee [implementatiemodellen](resource-manager-deployment-model.md) gebruikt voor het implementeren van resources in Azure (het nieuwe model is Azure Resource Manager). Sommige Azure-services ondersteunt alleen het Resource Manager-implementatiemodel en sommige ondersteunen zowel sommige ondersteunen alleen het klassieke implementatiemodel. De documentatie voor elke Azure-service geeft aan welke modellen die worden ondersteund.

## <a name="cli"></a>Azure-opdrachtregelinterface (CLI)
Een opdrachtregelinterface die kunnen worden gebruikt voor het beheren van Azure-services met Windows, macOS en Linux.  Sommige services of de service-functies kunnen alleen via PowerShell of de CLI worden beheerd. Zie [Azure CLI](/cli/azure)

## <a name="powershell"></a>Azure PowerShell
Een opdrachtregelinterface voor het beheren van Azure-services via een opdrachtregel van Windows-pc's. Sommige services of de service-functies kunnen alleen via PowerShell of de CLI worden beheerd.
Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview)

## <a name="arm-model"></a>Azure Resource Manager-implementatiemodel
Een van de twee [implementatiemodellen](resource-manager-deployment-model.md) gebruikt voor het implementeren van resources in Microsoft Azure (de andere is het klassieke implementatiemodel). Sommige Azure-services ondersteunt alleen het Resource Manager-implementatiemodel en sommige ondersteunen zowel sommige ondersteunen alleen het klassieke implementatiemodel. De documentatie voor elke Azure-service geeft aan welke modellen die worden ondersteund.

## <a name="fault-domain"></a>Foutdomein
De verzameling van virtuele machines in een beschikbaarheidsset die mogelijk op hetzelfde moment kan mislukken. Een voorbeeld is een groep van virtuele machines in een rek die een gemeenschappelijke voedingsbron en netwerkswitch delen. In Azure, de virtuele machines in een beschikbaarheidsset automatisch verdeeld over meerdere foutdomeinen.  
Zie [de beschikbaarheid van virtuele machines van Windows beheren](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [de beschikbaarheid van virtuele Linux-machines beheren](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>Geo
De grens van een gedefinieerde voor gegevensresidentie die normaal gesproken twee of meer regio's bevat. De grenzen mogelijk binnen of buiten de grenzen voor nationale en worden beïnvloed door het btw-instelling. Elke geo heeft ten minste één regio. Voorbeelden van geografische gebieden zijn Azië en Stille Oceaan en Japan. Ook wel genoemd *Geografie*.  
Zie [Azure-regio's](best-practices-availability-paired-regions.md)

## <a name="geo-replication"></a>Geo-replicatie
Het proces van de inhoud, zoals blobs, tabellen en wachtrijen in een regionaal paar automatisch wordt gerepliceerd.  
Zie [actieve Geo-replicatie voor Azure SQL-Database](sql-database/sql-database-geo-replication-overview.md)
<!-- The meaning of "geo" in this term seems to be different than the meaning provided in the "geo" entry -->

## <a name="image"></a>image
Een bestand met het besturingssysteem en de configuratie van de toepassing die kan worden gebruikt voor het maken van een willekeurig aantal virtuele machines. In Azure zijn twee soorten afbeeldingen: VM-installatiekopie en de installatiekopie van het besturingssysteem. Een VM-installatiekopie bevat een besturingssysteem en alle schijven die zijn gekoppeld aan een virtuele machine, wanneer de installatiekopie is gemaakt. Een installatiekopie van het besturingssysteem bevat alleen een gegeneraliseerde besturingssysteem met schijfconfiguraties die geen gegevens.  
Zie [navigeren door en selecteren installatiekopieën van Windows-machines in Azure met PowerShell of CLI](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>Limieten
Het aantal resources dat kunnen worden gemaakt of de benchmark-prestaties die kan worden bereikt. Grenzen zijn meestal gekoppeld aan abonnementen, services en aanbiedingen.  
Zie [Azure-abonnement en Servicelimieten, quotums en beperkingen](azure-subscription-service-limits.md)

## <a name="load-balancer"></a>Load balancer
Een resource die inkomend verkeer tussen de computers in een netwerk distribueert. In Azure verdeelt een load balancer het verkeer naar virtuele machines die zijn gedefinieerd in een load balancer-set. Een [netwerktaakverdeler](load-balancer/load-balancer-overview.md) kan internet gerichte of interne kan zijn.  

## <a name="mobile-app"></a>mobiele app
Een andere naam voor [App Service-App](#app-service-app).

## <a name="offer"></a>aanbieding
De prijzen, tegoeden en bijbehorende voorwaarden van toepassing op een Azure-abonnement.  
Zie de [pagina met details van Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/)

## <a name="portal"></a>portal
De beveiligde web-portal gebruikt om te implementeren en beheren van Azure-services.

## <a name="region"></a>regio
Een gebied in een geografisch gebied dat niet overschrijdend nationale randen en bevat een of meer datacenters. Prijzen, regionale services en typen worden weergegeven op het regioniveau van de. Een regio is doorgaans gekoppeld aan een andere regio gekoppeld, dit mag maximaal enkele honderden mijl weg. Regioparen kunnen worden gebruikt als een mechanisme voor herstel na noodgevallen en scenario's voor hoge beschikbaarheid. Ook wel *locatie*.  
Zie [Azure-regio's](best-practices-availability-paired-regions.md)

## <a name="resource"></a>Bron
Een item die deel uitmaakt van uw Azure-oplossing. Elke Azure-service kunt u verschillende soorten resources, zoals databases of virtuele machines te implementeren.   
Zie [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="resource-group"></a>resourcegroep
Een container in de Resource-Manager die verwante resources voor een toepassing bevat. De resourcegroep kan alle resources voor een toepassing of alleen de resources die logisch zijn gegroepeerd bevatten. U kunt bepalen hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is.  
Zie [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="arm-template"></a>Resource Manager-sjabloon
Een JSON-bestand waarmee een of meer Azure-resources declaratief worden gedefinieerd en die afhankelijkheden tussen de geïmplementeerde resources definieert. De sjabloon kan worden gebruikt om de resources consistent en herhaaldelijk te implementeren.  
Zie [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>resourceprovider
Een service die de resources levert die u kunt implementeren en beheren via Resource Manager. Elke resourceprovider biedt bewerkingen voor het werken met de resources die zijn geïmplementeerd. Resourceproviders is toegankelijk via de Azure-portal, Azure PowerShell en diverse programming SDK's.  
Zie [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="role"></a>rol
Een manier voor het beheren van toegang die kan worden toegewezen aan gebruikers, groepen en services. Rollen zijn bewerkingen zoals het maken, beheren, en meer over Azure-resources kunt uitvoeren.  
Zie [RBAC: ingebouwde rollen](role-based-access-control/built-in-roles.md)

## <a name="sla"></a>service level agreement (SLA)
De overeenkomst die worden toezeggingen van Microsoft voor actieve tijdsduur en connectiviteit beschreven. Elke Azure-service heeft een specifieke SLA.  
Zie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/)

## <a name="sas"></a>Shared access signature (SAS)
Een handtekening waarmee u beperkt toegang verlenen tot een resource, zonder dat uw accountsleutel. Bijvoorbeeld, [Azure Storage maakt gebruik van SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) clienttoegang verlenen tot objecten, zoals blobs. [IoT Hub gebruikt SAS](iot-hub/iot-hub-devguide-security.md#security-tokens) apparaten om machtiging te verlenen om telemetrie te verzenden.

## <a name="storage-account"></a>opslagaccount
Een account waarmee u beschikt over de toegang tot de Azure-Blob, wachtrij, tabel en bestand services in Azure Storage. Naam van het opslagaccount definieert de unieke naamruimte voor Azure Storage-gegevensobjecten.  
Zie [over Azure storage-accounts](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>abonnement
Een klant agreement van Microsoft die zorgt ervoor dat ze gebruikmaken van Azure services. De abonnementsprijzen en de bijbehorende voorwaarden zijn onderworpen aan de aanbieding gekozen voor het abonnement.
Zie [Microsoft Online Subscription overeenkomst](https://azure.microsoft.com/support/legal/subscription-agreement/) en [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory](active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>tag
Een indexering term waarmee u voor het categoriseren van resources op basis van uw vereisten voor het beheer of facturering. Wanneer u een complexe verzameling resources hebt, kunt u kunt tags gebruiken voor het visualiseren van deze assets op de manier die het meest zinvol. U kunt bijvoorbeeld resources taggen die een vergelijkbare rol hebben in uw organisatie of bij dezelfde afdeling horen.  
Zie [met tags voor het ordenen van uw Azure-resources](resource-group-using-tags.md)

## <a name="update-domain"></a>Domein bijwerken
De verzameling van virtuele machines in een beschikbaarheidsset die op hetzelfde moment worden bijgewerkt. Virtuele machines in hetzelfde updatedomein worden tegelijk opnieuw opgestart tijdens gepland onderhoud. Azure start nooit meer dan één updatedomein tegelijk. Ook aangeduid als een upgradedomein.  
Zie [de beschikbaarheid van virtuele machines van Windows beheren](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [de beschikbaarheid van virtuele Linux-machines beheren](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vm"></a>Virtuele machine
De software-implementatie van een fysieke computer die een besturingssysteem wordt uitgevoerd. Meerdere virtuele machines kunnen tegelijkertijd worden uitgevoerd op dezelfde hardware. In Azure zijn virtuele machines beschikbaar in verschillende grootten.  
Zie [documentatie voor virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="vm-extension"></a>extensie van virtuele machine
Een resource die gedrag of functies die een andere programma's werkt of bieden de mogelijkheid om te communiceren met een actieve computer implementeert. U kunt bijvoorbeeld de extensie voor VM-toegang opnieuw instellen of wijzigen van de waarden van externe toegang op een Azure-machine.
<!-- This definition seems obscure to me; maybe a list of examples would work better than a conceptual definition? -->
Zie [over extensies voor virtuele machines en -functies (Windows)](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [over extensies voor virtuele machines en -functies (Linux)](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vnet"></a>Virtueel netwerk
Een netwerk dat biedt connectiviteit tussen uw Azure-resources die is geïsoleerd van alle andere Azure-tenants. Een [Azure VPN-Gateway](vpn-gateway/vpn-gateway-about-vpngateways.md) kunt u verbindingen kunt opzetten tussen virtuele netwerken en [tussen een virtueel netwerk en een on-premises netwerk](vpn-gateway/vpn-gateway-plan-design.md). U hebt volledige controle de IP-adresblokken, DNS-instellingen, beveiligingsbeleid en routetabellen binnen een virtueel netwerk.  
Zie [overzicht van Virtual Network](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Web-app
Een andere naam voor [App Service-App](#app-service-app).

## <a name="see-also"></a>Zie ook

* [Aan de slag met Azure](https://azure.microsoft.com/get-started/)
* [Cloud resource center](https://azure.microsoft.com/resources/)  
* [Azure voor uw business-toepassing](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Azure in uw datacenter](https://azure.microsoft.com/overview/business-apps-on-azure/)

