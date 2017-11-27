---
title: Verklarende woordenlijst voor Azure - Azure woordenlijst | Microsoft Docs
description: De verklarende woordenlijst voor Azure gebruiken om te begrijpen cloud terminologie op de Azure-platform. Deze korte Azure woordenlijst bevat definities voor algemene voorwaarden van de cloud voor Azure.
keywords: Woordenlijst voor Azure, cloud terminologie, Azure verklarende woordenlijst, termen die, cloud-voorwaarden
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: 
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: monicar
ms.openlocfilehash: cbc4b8cdb0ff9255d0be02b998e67686921921ea
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Verklarende woordenlijst voor Microsoft Azure: een dictionary van cloud-terminologie op de Azure-platform

De verklarende woordenlijst voor Microsoft Azure is een korte dictionary van cloud-terminologie voor Azure-platform. Zie ook:

* [Microsoft Azure en Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) -definities van Azure-services en hun AWS-equivalenten.<!-- I propose to link to https://azure.microsoft.com/en-us/services/ instead of this -->
* [Cloud computing voorwaarden](https://azure.microsoft.com/overview/cloud-computing-dictionary/) -algemeen bedrijfstak cloud voorwaarden.

## <a name="account"></a>Account
Een account dat wordt gebruikt voor toegang tot en beheer van een Azure-abonnement. Dit wordt vaak aangeduid als een Azure-account Hoewel een account kunt elk van deze waarden zijn: een bestaand werk, school, of persoonlijke Microsoft-account of een Office 365-gebruikersnaam en wachtwoord. U kunt ook maken met een account voor het beheren van een Azure-abonnement als u zich registreert voor de [gratis proefversie](https://azure.microsoft.com).  
Zie [aanmelden voor een Azure-abonnement met uw Office 365-account](billing/billing-use-existing-office-365-account-azure-subscription.md) en [Accounts kunt u aan te melden](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="api-app"></a>API-app
Een andere naam voor [App Service-app](#app-service-app).

## <a name="app-service-app"></a>App Service-app
De rekenresources die [Azure App Service](app-service/app-service-web-overview.md) biedt voor het hosten van een website of web-toepassing, web-API of [back-end voor mobiele app](app-service-mobile/app-service-mobile-value-prop.md). App Service-apps worden ook aangeduid als *App Services*, *web-apps*, *API-apps*, en *mobiele apps*.

## <a name="availability-set"></a>Beschikbaarheidsset
Een verzameling van virtuele machines die samen worden beheerd voor redundantie van de toepassing en betrouwbaarheid. Het gebruik van een beschikbaarheidsset zorgt ervoor dat tijdens beide gepland of ongepland onderhoud ten minste één virtuele machine beschikbaar is.  
Zie [beheren van de beschikbaarheid van virtuele machines van Windows](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [de beschikbaarheid van Linux virtuele machines beheren](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="classic-model"></a>Azure klassieke implementatiemodel
Twee [implementatiemodellen](resource-manager-deployment-model.md) gebruikt voor het implementeren van resources in Azure (het nieuwe model is Azure Resource Manager). Sommige Azure-services ondersteunt alleen het Resource Manager-implementatiemodel, sommige browsers ondersteunen het klassieke implementatiemodel en sommige ondersteunen beide. De documentatie voor elke Azure-service geeft aan welke modellen ondersteunen.

## <a name="cli"></a>Azure-opdrachtregelinterface (CLI)
Een opdrachtregelinterface die kunnen worden gebruikt voor het beheren van Azure-services van Windows, Mac OS- en Linux.  Bepaalde services of functies van de service kunnen alleen via PowerShell of de CLI worden beheerd. Zie [Azure CLI 2.0](/cli/azure/overview)

## <a name="powershell"></a>Azure PowerShell
Een opdrachtregelinterface voor het beheren van Azure-services via een opdrachtregel van Windows-pc's. Bepaalde services of functies van de service kunnen alleen via PowerShell of de CLI worden beheerd.
Zie [hoe Azure PowerShell installeren en configureren](/powershell/azure/overview)

## <a name="arm-model"></a>Azure Resource Manager-implementatiemodel
Twee [implementatiemodellen](resource-manager-deployment-model.md) gebruikt voor het implementeren van resources in Microsoft Azure (de andere is het klassieke implementatiemodel). Sommige Azure-services ondersteunt alleen het Resource Manager-implementatiemodel, sommige browsers ondersteunen het klassieke implementatiemodel en sommige ondersteunen beide. De documentatie voor elke Azure-service geeft aan welke modellen ondersteunen.

## <a name="fault-domain"></a>Foutdomein
De verzameling van virtuele machines in een beschikbaarheidsset dat mogelijk op hetzelfde moment kunnen mislukken. Een voorbeeld is een groep van virtuele machines in een rek die een gemeenschappelijk power-bron- en switch delen. In Azure gescheiden zijn automatisch virtuele machines in een beschikbaarheidsset in meerdere domeinen met fouten.  
Zie [beheren van de beschikbaarheid van virtuele machines van Windows](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [de beschikbaarheid van Linux virtuele machines beheren](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>geografisch
De grens van een gedefinieerd voor de hand van gegevens vestigingsplaats waarin doorgaans twee of meer regio's. De grenzen mogelijk binnen of buiten de grenzen voor nationale en zijn beïnvloed door BTW-instelling. Elke geo heeft ten minste één regio. Voorbeelden van geografische gebieden zijn Azië en Stille Oceaan en Japan. Ook wel *Geografie*.  
Zie [Azure-regio's](best-practices-availability-paired-regions.md)

## <a name="geo-replication"></a>geo-replicatie
Het proces voor het repliceren van inhoud zoals blobs, tabellen en wachtrijen in een combinatie van regionale automatisch.  
Zie [actieve Geo-replicatie voor Azure SQL-Database](sql-database/sql-database-geo-replication-overview.md)
<!-- The meaning of "geo" in this term seems to be different than the meaning provided in the "geo" entry -->

## <a name="image"></a>Afbeelding
Een bestand met het besturingssysteem en de configuratie van de toepassing die kan worden gebruikt voor het maken van een willekeurig aantal virtuele machines. In Azure zijn twee typen installatiekopieën: VM installatiekopie en de installatiekopie van het besturingssysteem. Een VM-installatiekopie bevat een besturingssysteem en alle schijven die zijn gekoppeld aan een virtuele machine wanneer de installatiekopie wordt gemaakt. Een installatiekopie van het besturingssysteem bevat alleen een gegeneraliseerde besturingssysteem met schijfconfiguraties die geen gegevens.  
Zie [navigeren door en selecteer installatiekopieën van Windows virtuele machines in Azure met PowerShell of de CLI](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>Limieten
Het aantal bronnen die kunnen worden gemaakt of de benchmark-prestaties die kan worden bereikt. Grenzen zijn meestal gekoppeld aan abonnementen, services en aanbiedingen.  
Zie [Azure-abonnement en Servicelimieten, quota's en beperkingen](azure-subscription-service-limits.md)

## <a name="load-balancer"></a>De load balancer
Een bron die binnenkomend verkeer op de computers in een netwerk distribueert. In Azure wordt een load balancer verkeer naar virtuele machines die zijn gedefinieerd in een load balancer-set. Een [netwerktaakverdeler](load-balancer/load-balancer-overview.md) kan internetgerichte of interne kan zijn.  

## <a name="mobile-app"></a>mobiele app
Een andere naam voor [App Service-App](#app-service-app).

## <a name="offer"></a>Aanbieding
De prijzen, tegoeden, en de bijbehorende voorwaarden van toepassing op een Azure-abonnement.  
Zie de [detailpagina aanbieding voor Azure](https://azure.microsoft.com/support/legal/offer-details/)

## <a name="portal"></a>portal
De beveiligde webportal te implementeren en beheren van Azure-services.

## <a name="region"></a>regio
Een gebied binnen een geografisch die niet snijpunt nationale grenzen en bevat een of meer datacenters. Prijzen regionale diensten en aanbiedingstypen worden weergegeven op het regioniveau van de. Een regio is meestal gekoppeld aan een andere regio, wat kan maximaal enkele honderden mijl verwijderd. Regionale paren kunnen worden gebruikt als een mechanisme voor herstel na noodgevallen en scenario's voor hoge beschikbaarheid. Ook wel *locatie*.  
Zie [Azure-regio's](best-practices-availability-paired-regions.md)

## <a name="resource"></a>Bron
Een item die deel uitmaakt van uw Azure-oplossing. Elke Azure-service kunt u verschillende soorten resources, zoals databases of virtuele machines te implementeren.   
Zie [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="resource-group"></a>resourcegroep
Een container in de Resource-Manager met verwante resources voor een toepassing. De resourcegroep kan bevatten alle resources voor een toepassing of alleen de resources die logisch zijn gegroepeerd. U kunt bepalen hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is.  
Zie [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="arm-template"></a>Resource Manager-sjabloon
Een JSON-bestand declaratief definiëren een of meer Azure-resources en de afhankelijkheden tussen de geïmplementeerde resources te definiëren. De sjabloon kan worden gebruikt om de resources consistent en herhaaldelijk te implementeren.  
Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>Resourceprovider
Een service die de resources levert die u kunt implementeren en beheren via Resource Manager. Elke resourceprovider biedt bewerkingen voor het werken met de resources die zijn geïmplementeerd. Resourceproviders kunnen worden geopend via de Azure-portal, Azure PowerShell en diverse programming SDK's.  
Zie [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="role"></a>Rol
Een methode voor het beheren van toegang die kan worden toegewezen aan gebruikers, groepen en services. Rollen zijn uitvoeren van acties zoals maken, beheren, en gelezen op Azure-resources.  
Zie [RBAC: ingebouwde rollen](active-directory/role-based-access-built-in-roles.md)

## <a name="sla"></a>serviceovereenkomst (SLA)
De overeenkomst die worden beschreven van Microsoft verbintenissen voor bedrijfstijd en connectiviteit. Elke Azure-service heeft een specifieke SLA.  
Zie [serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)

## <a name="sas"></a>Shared access signature (SAS)
Een handtekening waarmee u beperkte om toegang te verlenen tot een resource, zonder dat de sleutel van uw account. Bijvoorbeeld: [SAS maakt gebruik van Azure Storage](storage/common/storage-dotnet-shared-access-signature-part-1.md) clienttoegang verlenen tot objecten, zoals blobs. [IoT Hub gebruikt SAS](iot-hub/iot-hub-devguide-security.md#security-tokens) apparaten machtiging verlenen om te verzenden van telemetrie.

## <a name="storage-account"></a>Storage-account
Een account waarmee u toegang tot de Azure Blob, Queue, Table en bestand services in Azure Storage. Naam van het opslagaccount definieert een unieke naamruimte voor Azure Storage-gegevensobjecten.  
Zie [over Azure storage-accounts](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>abonnement
Een klant overeenkomst met Microsoft waarmee deze Azure-services krijgen. De abonnement-prijzen en de bijbehorende voorwaarden worden bepaald door de aanbieding voor het abonnement gekozen.
Zie [Microsoft Online Subscription overeenkomst](https://azure.microsoft.com/support/legal/subscription-agreement/) en [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>Label
Een indexering term waarmee u resources volgens uw vereisten voor beheer of facturering indelen. Wanneer u een complexe verzameling resources hebt, kunt u codes gebruiken voor het visualiseren van deze assets op de manier die het meest zinvol. U kunt bijvoorbeeld resources taggen die een vergelijkbare rol hebben in uw organisatie of bij dezelfde afdeling horen.  
Zie [met labels om uw Azure-resources te organiseren](resource-group-using-tags.md)

## <a name="update-domain"></a>Bijwerken van domein
De verzameling van virtuele machines in een beschikbaarheidsset die op hetzelfde moment worden bijgewerkt. Virtuele machines in hetzelfde updatedomein worden samen opnieuw gestart tijdens gepland onderhoud. Azure start nooit meer dan één updatedomein tegelijk. Ook aangeduid als een upgradedomein.  
Zie [beheren van de beschikbaarheid van virtuele machines van Windows](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [de beschikbaarheid van Linux virtuele machines beheren](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vm"></a>virtuele machine
De software-implementatie van een fysieke computer waarop een besturingssysteem wordt uitgevoerd. Meerdere virtuele machines kan tegelijkertijd worden uitgevoerd op dezelfde hardware. In Azure zijn virtuele machines in een aantal verschillende grootten beschikbaar.  
Zie [documentatie Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="vm-extension"></a>extensie van virtuele machine
Een bron die gedrag of functies waarmee een andere programma's een werk- of bieden de mogelijkheid om te communiceren met een actieve computer implementeert. U kunt bijvoorbeeld de uitbreiding toegang tot de virtuele machine opnieuw instellen of wijzigen van waarden voor externe toegang op een virtuele machine van Azure.
<!-- This definition seems obscure to me; maybe a list of examples would work better than a conceptual definition? -->
Zie [over uitbreidingen van de virtuele machine en -functies (Windows)](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [over uitbreidingen van de virtuele machine en -functies (Linux)](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vnet"></a>virtueel netwerk
Een netwerk dat biedt connectiviteit tussen uw Azure-resources die is geïsoleerd van andere Azure tenants. Een [Azure VPN-Gateway](vpn-gateway/vpn-gateway-about-vpngateways.md) kunt u verbindingen maken tussen virtuele netwerken en [tussen een virtueel netwerk en een on-premises netwerk](vpn-gateway/vpn-gateway-plan-design.md). U kunt volledig de IP-adresblokken, DNS-instellingen, beveiligingsbeleidsregels en routetabellen binnen een virtueel netwerk beheren.  
Zie [Virtual Network-overzicht](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Web-app
Een andere naam voor [App Service-App](#app-service-app).

## <a name="see-also"></a>Zie ook

* [Aan de slag met Azure](https://azure.microsoft.com/get-started/)
* [Cloud-resources](https://azure.microsoft.com/resources/)  
* [Azure voor business-toepassing](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Azure in uw datacenter](https://azure.microsoft.com/overview/business-apps-on-azure/)

