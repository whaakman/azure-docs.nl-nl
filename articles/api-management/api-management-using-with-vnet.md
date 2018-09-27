---
title: Azure API Management gebruiken met virtuele netwerken
description: Informatie over het instellen van een verbinding met een virtueel netwerk in Azure API Management en toegang tot web-services via deze.
services: api-management
documentationcenter: ''
author: vlvinogr
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 1a02fd604d08e87c84a73657b7204ecb42b3498b
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393176"
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Azure API Management gebruiken met virtuele netwerken
Azure-netwerken (VNETs) kunt u een van uw Azure-resources in een niet-internet routeerbare netwerk dat u toegang tot te plaatsen. Deze netwerken kunnen vervolgens worden verbonden met uw on-premises netwerken met behulp van verschillende VPN-technologieën. Voor meer informatie over Azure Virtual Networks beginnen met de informatie hier: [Azure Virtual Network Overview](../virtual-network/virtual-networks-overview.md).

Met Azure API Management kunnen worden geïmplementeerd in het virtuele netwerk (VNET), zodat deze toegang heeft tot de back-endservices binnen het netwerk. De developer-portal en API-gateway, kunnen worden geconfigureerd zodat ze toegankelijk zijn vanaf Internet of alleen binnen het virtuele netwerk.

> [!NOTE]
> Met Azure API Management biedt ondersteuning voor zowel klassieke als Azure Resource Manager VNets.
>

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een APIM-instantie. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).
+ VNET-connectiviteit is beschikbaar in de Premium- en Developer alleen-lagen. Overstappen op een van deze lagen door de aanwijzingen in de [bijwerken en schalen](upgrade-and-scale.md#upgrade-and-scale) onderwerp.

## <a name="enable-vpn"> </a>VNET-verbinding inschakelen

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>VNET-connectiviteit met de Azure-portal inschakelen

1. Navigeer naar de APIM-instantie in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **virtueel netwerk**.
3. De API Management-exemplaar om te worden geïmplementeerd in een virtueel netwerk configureren.

    ![Menu van het virtuele netwerk van API Management][api-management-using-vnet-menu]
4. Selecteer de gewenste toegangstype:

    * **Externe**: de API Management-gateway en developer-portal zijn toegankelijk via het openbare internet via een externe load balancer. De gateway hebben toegang tot resources binnen het virtuele netwerk.

    ![Openbare peering][api-management-vnet-public]

    * **Interne**: de API Management-gateway en developer-portal zijn alleen toegankelijk vanuit het virtuele netwerk via een interne load balancer. De gateway hebben toegang tot resources binnen het virtuele netwerk.

    ![Persoonlijke peering][api-management-vnet-private]`

    U ziet nu een lijst met alle regio's waar uw API Management-service is ingericht. Selecteer een VNET en subnet voor elke regio. De lijst is gevuld met zowel klassieke als Resource Manager virtuele netwerken die beschikbaar zijn in uw Azure-abonnementen die zijn ingesteld in de regio die u wilt configureren.

    > [!NOTE]
    > **Service-eindpunt** in het bovenstaande diagram bevat Gateway/Proxy, de Azure-portal, de portal voor ontwikkelaars, GIT en het eindpunt voor Direct.
    > **Beheereindpunt** in het bovenstaande diagram wordt het eindpunt dat wordt gehost op de service voor het beheer van configuratie via Azure portal en Powershell.
    > Ook, houd er rekening mee dat, zelfs als het diagram ziet u IP-adressen voor de verschillende API Management-service-eindpunten **alleen** reageert op de geconfigureerde hostnamen.

    > [!IMPORTANT]
    > Wanneer u een Azure API Management-exemplaar met een Resource Manager VNET implementeert, moet de service zich in een toegewezen subnet die geen andere resources, met uitzondering van exemplaren van Azure API Management bevat. Als een poging wordt gedaan om het exemplaar van Azure API Management implementeren met een Resource Manager-VNET-subnet die andere resources bevat, mislukt de implementatie.
    >

    ![Selecteer VPN][api-management-setup-vpn-select]

5. Klik op **opslaan** aan de bovenkant van het scherm.

> [!NOTE]
> Het VIP-adres van exemplaar van API Management wordt gewijzigd wanneer VNET is ingeschakeld of uitgeschakeld.
> Het VIP-adres verandert ook wanneer API Management wordt verplaatst van **externe** naar **intern** of omgekeerd
>

> [!IMPORTANT]
> Als u API Management te van een VNET verwijderen of deze is geïmplementeerd in een wijzigt, kan de eerder gebruikte VNET voor maximaal twee uur te blijven vergrendeld. Tijdens deze periode wordt het niet mogelijk zijn om te verwijderen van het VNET of implementeren van een nieuwe resource toe.

## <a name="enable-vnet-powershell"> </a>Inschakelen van VNET-verbinding met behulp van PowerShell-cmdlets
U kunt ook inschakelen met VNET-connectiviteit met de PowerShell-cmdlets

* **Een API Management-service binnen een VNET maken**: Gebruik de cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) om een Azure API Management-service binnen een VNET te maken.

* **Implementeren van een bestaande API Management-service binnen een VNET**: Gebruik de cmdlet [Update AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) te verplaatsen van een bestaande Azure API Management-service binnen een Virtueelnetwerk.

## <a name="connect-vnet"> </a>Verbinding maken met een webservice die worden gehost in een virtueel netwerk
Nadat uw API Management-service is verbonden met het VNET, is het niet anders dan toegang tot openbare services om toegang tot endservices in het back. Typ in het lokale IP-adres of de hostnaam (als een DNS-server is geconfigureerd voor het VNET) van de webservice in de **webservice-URL** veld bij het maken van een nieuwe API of een bestaande bewerkt.

![API toevoegen uit VPN][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"> </a>Veelvoorkomende problemen van Network Configuration
Hieronder volgt een lijst met veelvoorkomende configuratiefouten die optreden kunnen tijdens het implementeren van API Management-service in een Virtueelnetwerk.

* **Aangepaste DNS-server-instellingen**: de API Management-service is afhankelijk van verschillende Azure-services. Wanneer de API Management wordt gehost in een VNET met een aangepaste DNS-server, moet deze op te lossen de hostnamen van deze Azure-services. Volg [dit](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) richtlijnen voor aangepaste DNS-instellingen. Zie de onderstaande tabel voor poorten en andere vereisten voor verwijzing.

> [!IMPORTANT]
> Als u van plan bent te gebruiken een aangepaste DNS-server (s) voor het VNET, moet u het instellen van **voordat** implementeren van een API Management-service erin. Anders moet u de API Management-service telkens bijgewerkt wanneer u de DNS-server (s) door het uitvoeren van wijzigen de [netwerkactiviteiten configuratie toepassen](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/ApplyNetworkConfigurationUpdates)

* **Poorten die nodig zijn voor API Management**: binnenkomend en uitgaand verkeer in het Subnet waarin API Management is geïmplementeerd, kan worden beheerd met behulp van [Network Security Group][Network Security Group]. Als een van deze poorten niet beschikbaar zijn, wordt API Management werken mogelijk niet correct en kan niet meer toegankelijk. Met een of meer van deze poorten geblokkeerd, is dat een andere veelvoorkomende onjuiste configuratie probleem bij het gebruik van API Management met een VNET.

Wanneer een exemplaar van API Management-service wordt gehost in een VNET, worden de poorten in de volgende tabel worden gebruikt.

| Bron / doelpoort(en) | Richting | Transportprotocol | Bron / bestemming | Doel (*) | Type virtuele netwerk |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Inkomend |TCP |INTERNET / VIRTUAL_NETWORK|Communicatie van clients met API Management|Extern |
| * / 3443 |Inkomend |TCP |APIMANAGEMENT / VIRTUAL_NETWORK|Beheereindpunt voor Azure-portal en Powershell |Externe en interne |
| * / 80, 443 |Uitgaand |TCP |VIRTUAL_NETWORK / opslag|**Afhankelijkheid van Azure Storage**|Externe en interne |
| * / 80, 443 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET| Azure Active Directory (indien van toepassing)|Externe en interne |
| * / 1433 |Uitgaand |TCP |VIRTUAL_NETWORK / SQL|**Toegang tot Azure SQL-eindpunten** |Externe en interne |
| * / 5672 |Uitgaand |TCP |VIRTUAL_NETWORK / EventHub |Afhankelijkheid voor logboek naar Event Hub-beleid en de monitoring agent |Externe en interne |
| * / 445 |Uitgaand |TCP |VIRTUAL_NETWORK / opslag |Afhankelijkheid van Azure-bestandsshare voor GIT |Externe en interne |
| * / 1886 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET|Die nodig zijn voor het publiceren van de Integriteitsstatus van de op Resource Health |Externe en interne |
| * / 25028 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET|Verbinding maken met de SMTP-Relay voor het verzenden van e-mailberichten |Externe en interne |
| * / 6381 - 6383 |Inkomende en uitgaande |TCP |VIRTUAL_NETWORK / VIRTUAL_NETWORK|Toegang tot Redis Cache-exemplaren tussen RoleInstances |Externe en interne |
| * / * | Inkomend |TCP |AZURE_LOAD_BALANCER / VIRTUAL_NETWORK| Infrastructuur van Azure Load Balancer |Externe en interne |

>[!IMPORTANT]
> De poorten waarvoor de *doel* is **vet** vereist zijn voor API Management-service is geïmplementeerd. De andere poorten te blokkeren echter zorgt ervoor dat vermindering in de mogelijkheid om te gebruiken en controleren van de service.

* **SSL-functionaliteit**: om in te schakelen SSL-certificaatketen gebouw en validatie van de API Management-service moet uitgaande netwerkconnectiviteit ocsp.msocsp.com, mscrl.microsoft.com en crl.microsoft.com. Met deze afhankelijkheid is niet vereist, als een certificaat dat u naar API Management uploadt de volledige keten aan de basis-CA bevat.

* **DNS-toegang**: uitgaand verkeer op poort 53 is vereist voor communicatie met DNS-servers. Als een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat, moet de DNS-server bereikbaar is vanaf het subnet die als host fungeert voor API Management zijn.

* **Metrische gegevens en statuscontrole**: uitgaande netwerkverbinding naar Azure Monitoring-eindpunten die onder de volgende domeinen oplossen: 

    | Azure-omgeving | Eindpunten |
    | --- | --- |
    | Azure openbaar | <ul><li>Prod.warmpath.msftcloudes.com</li><li>shoebox2.metrics.nsatc.NET</li><li>prod3.metrics.nsatc.NET</li><li>prod3 black.prod3.metrics.nsatc.net</li><li>prod3 red.prod3.metrics.nsatc.net</li><li>Prod.warm.ingestion.msftcloudes.com</li><li>`azure region`. warm.ingestion.msftcloudes.com waar `East US 2` eastus2.warm.ingestion.msftcloudes.com is</li></ul> |
    | Azure Government | <ul><li>fairfax.warmpath.usgovcloudapi.NET</li><li>shoebox2.metrics.nsatc.NET</li><li>prod3.metrics.nsatc.NET</li></ul> |
    | Azure China | <ul><li>mooncake.warmpath.chinacloudapi.CN</li><li>shoebox2.metrics.nsatc.NET</li><li>prod3.metrics.nsatc.NET</li></ul> |

* **Azure-portal Diagnostics**: de stroom van diagnostische logboeken van Azure portal inschakelen bij het gebruik van de API Management-extensie van binnen een virtueel netwerk, uitgaande toegang tot `dc.services.visualstudio.com` op poort 443 is vereist. Dit helpt bij het oplossen van problemen die u tegenkomen kan bij het gebruik van extensie.

* **Snelle installatie van de Route**: een algemene configuratie van de klant is voor het definiëren van hun eigen standaardroute (0.0.0.0/0) die ervoor zorgt uitgaand internetverkeer dat naar in plaats daarvan flow on-premises. Connectiviteit met Azure API Management verbroken met deze verkeersstroom altijd omdat het uitgaande verkeer geblokkeerd on-premises wordt en NAT wilt een onherkenbare set met adressen die niet meer met verschillende Azure-eindpunten werken. De oplossing is voor het definiëren van een (of meer) gebruiker gedefinieerde routes ([udr's][UDRs]) op het subnet waarin de Azure API Management. Een UDR definieert de subnet-specifieke routes die worden gebruikt in plaats van de standaardroute.
  Het verdient indien mogelijk, gebruik de volgende configuratie:
 * De ExpressRoute-configuratie adverteert 0.0.0.0/0 en standaard geforceerde tunnels al het uitgaande verkeer on-premises.
 * De UDR toegepast op het subnet met de Azure API Management definieert 0.0.0.0/0 met een volgend hoptype van Internet.
 Het gecombineerde effect van deze stappen is dat het subnetniveau UDR voorrang boven de ExpressRoute geforceerde tunneling, dus ervoor te zorgen dat uitgaande toegang tot Internet vanaf de Azure API Management.

* **Routering via virtuele netwerkapparaten**: configuraties die gebruikmaken van een UDR met een standaardroute (0.0.0.0/0) voor het routeren van internet bestemd verkeer vanuit het subnet API Management via een virtueel netwerkapparaat uitvoeren in Azure wordt geblokkeerd beheer van verkeer die afkomstig zijn van Internet naar de API Management-service-exemplaar geïmplementeerd in een subnet van het virtuele netwerk. Deze configuratie wordt niet ondersteund.

>[!WARNING]
>Met Azure API Management wordt niet ondersteund met ExpressRoute-configuraties die **onjuist advertentieoverschrijdende routes van de openbare-peeringpad naar het pad voor persoonlijke peering**. ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangen routeadvertenties van Microsoft voor een groot aantal Microsoft Azure IP-adresbereiken. Als deze adresbereiken onjuist advertentieoverschrijdend op het pad voor persoonlijke peering zijn, is het eindresultaat dat alle uitgaande netwerkpakketten vanuit het subnet van de Azure API Management-instantie onjuist geforceerde aan van een klant on-premises netwerk infrastructuur. Deze netwerkstroom wordt verbroken Azure API Management. De oplossing voor dit probleem is om te stoppen advertentieoverschrijdende routes van het pad voor openbare peering naar het pad voor persoonlijke peering.


## <a name="troubleshooting"> </a>Problemen oplossen
* **Instellingen voor de eerste**: wanneer de eerste implementatie van API Management-service in een subnet niet gelukt is, is het raadzaam eerst een virtuele machine implementeren in hetzelfde subnet bevinden. Extern bureaublad van de volgende bij de virtuele machine en controleren of er verbinding met een van elke resource hieronder in uw azure-abonnement
    * Azure Storage-blob
    * Azure SQL Database
    * Azure Storage-tabel

 > [!IMPORTANT]
 > Nadat u de connectiviteit hebt gevalideerd, zorg ervoor dat alle resources die zijn geïmplementeerd in het subnet voor het implementeren van API Management in het subnet te verwijderen.

* **Incrementele Updates**: als u wijzigingen aanbrengt aan uw netwerk, verwijzen naar [NetworkStatus API](https://docs.microsoft.com/rest/api/apimanagement/networkstatus), om te verifiëren dat de API Management-service niet tot een van de kritieke resources die dit is afhankelijk van is verbroken. De verbindingsstatus van de moet worden bijgewerkt om de 15 minuten.

* **Resourcenavigatiekoppelingen**: wanneer u deze implementeert in Resource Manager stijl vnet-subnet, API Management het subnet gereserveerd door het maken van een koppeling-resourcenavigatie. Als het subnet al een resource in een andere provider bevat, implementatie zal **mislukken**. Wanneer u een API Management-service naar een ander subnet verplaatsen of verwijderen, zullen we op dezelfde manier dat resourcenavigatiekoppeling verwijderen.

## <a name="subnet-size"> </a> De vereiste grootte subnet
Azure reserveert bepaalde IP-adressen binnen elk subnet, en deze adressen kunnen niet worden gebruikt. De eerste en laatste IP-adressen van de subnetten zijn gereserveerd voor protocolconformiteit en drie adressen voor Azure-services. Zie voor meer informatie, [gelden er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Naast de IP-adressen die worden gebruikt door de Azure-VNET-infrastructuur, gebruikt elk exemplaar van Api Management in het subnet twee IP-adressen per eenheid van de Premium-SKU of een IP-adres voor de Developer-SKU. Elk exemplaar een extra IP-adres voor de externe load balancer gereserveerd. Wanneer u deze implementeert in interne vnet, is een extra IP-adres voor de interne load balancer vereist.

Gezien de berekening boven de minimale grootte van het subnet waarin API Management kunnen worden geïmplementeerd is het /29 waarmee 3 IP-adressen.

## <a name="routing"> </a> Routering
+ Een gelijke openbare IP-adres (VIP) wordt gereserveerd voor toegang tot alle service-eindpunten.
+ Een IP-adres van een subnet-IP-adresbereik (DIP) wordt gebruikt voor toegang tot resources binnen het vnet en een openbare IP-adres (VIP) wordt gebruikt voor toegang tot bronnen buiten het vnet.
+ Openbaar IP-adres met gelijke taakverdeling adres kan worden gevonden op de blade overzicht/Essentials in Azure portal.

## <a name="limitations"> </a>Beperkingen
* Een subnet met exemplaren van API Management kan geen andere typen van Azure-resources bevatten.
* Het subnet en de API Management-service moeten zich in hetzelfde abonnement.
* Een subnet met exemplaren van API Management kan niet worden verplaatst tussen abonnementen.
* Voor meerdere regio's API Management implementaties die zijn geconfigureerd in de modus voor intern virtueel netwerk, zijn gebruikers die verantwoordelijk is voor het beheren van de load balancer in meerdere regio's waarvan ze eigenaar de routering.
* Verbinding van een resource in een wereldwijd gekoppelde VNET in een andere regio met API Management-service in de modus voor interne werkt niet vanwege een platformbeperking. Zie voor meer informatie, [Resources in een virtueel netwerk kunnen niet communiceren met interne load balancer van Azure in gekoppelde virtuele netwerk](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints)


## <a name="related-content"> </a>Gerelateerde inhoud
* [Een Virtueelnetwerk verbinden met back-end met behulp van Vpn-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Het verbinden van een Virtueelnetwerk van verschillende implementatiemodellen](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Het gebruik van de API-Inspector te traceren aanroepen in Azure API Management](api-management-howto-api-inspector.md)
* [Veelgestelde vragen over Virtual Network](../virtual-network/virtual-networks-faq.md)
* [Servicetags](../virtual-network/security-overview.md#service-tags)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-internal.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-external.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/security-overview.md
