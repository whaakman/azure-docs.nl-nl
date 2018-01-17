---
title: Azure API Management gebruiken met virtuele netwerken
description: Informatie over het instellen van een verbinding met een virtueel netwerk in Azure API Management en toegang tot web-services via het.
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 167a4eda4cec509a262b7e032f7629c7435beafd
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Azure API Management gebruiken met virtuele netwerken
Virtuele netwerken van Azure (vnet's) kunt u een van uw Azure-resources in een internet-routeable netwerk dat u toegang tot te plaatsen. Deze netwerken kunnen vervolgens worden verbonden met uw on-premises netwerken met behulp van verschillende VPN-technologieën. Voor meer informatie over virtuele netwerken van Azure beginnen met de informatie hier: [Azure Virtual Network-overzicht](../virtual-network/virtual-networks-overview.md).

Azure API Management kunnen worden geïmplementeerd in het virtuele netwerk (VNET), zodat deze toegang heeft tot back-end-services in het netwerk. De portal voor ontwikkelaars en API-gateway kan worden geconfigureerd om toegankelijk zijn vanaf Internet of alleen binnen het virtuele netwerk.

> [!NOTE]
> Azure API Management biedt ondersteuning voor classic en Azure Resource Manager-vnet's.
>

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een exemplaar APIM. Zie voor meer informatie [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ VNET-connectiviteit is beschikbaar in alleen de lagen Premium en ontwikkelaars. Overschakelen naar een van deze lagen volgens de aanwijzingen in de [upgraden en schalen](upgrade-and-scale.md#upgrade-and-scale) onderwerp.

## <a name="enable-vpn"></a>Inschakelen VNET-verbinding

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>VNET-connectiviteit met de Azure portal inschakelen

1. Navigeer naar uw APIM exemplaar in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **virtueel netwerk**.
3. Het exemplaar van API Management te worden geïmplementeerd in een virtueel netwerk configureren.

    ![Virtueel netwerk menu van API Management][api-management-using-vnet-menu]
4. Selecteer het gewenste toegangstype:
    
    * **Externe**: de API Management-gateway en developer-portal toegankelijk zijn vanaf het openbare internet via een externe load balancer. De gateway toegang tot bronnen in het virtuele netwerk.
    
    ![Openbare peering][api-management-vnet-public]
    
    * **Interne**: de API Management-gateway en developer-portal zijn alleen toegankelijk vanuit het virtuele netwerk via een interne load balancer. De gateway toegang tot bronnen in het virtuele netwerk.
    
    ![Persoonlijke peering][api-management-vnet-private]`

    U ziet nu een lijst met alle regio's waar uw API Management-service is ingericht. Selecteer een VNET en een subnet voor elke regio. De lijst is gevuld met zowel klassieke als Resource Manager virtuele netwerken die beschikbaar zijn in uw Azure-abonnementen die zijn ingesteld in de regio die u configureert.
    
    > [!NOTE]
    > **Service-eindpunt** in het bovenstaande diagram bevat Gateway of Proxy, Publicatieportal, Developer-Portal, GIT en het eindpunt met Direct beheer.
    > **Eindpunt** in het bovenstaande diagram wordt het eindpunt dat wordt gehost op de service voor het beheren van configuratie van Azure-portal en Powershell.
    > Ook, houd er rekening mee dat, hoewel het diagram toont de IP-adressen voor de verschillende API Management-service-eindpunten **alleen** reageert op de geconfigureerde hostnamen.
    
    > [!IMPORTANT]
    > Bij het implementeren van een Azure API Management-exemplaar naar een Resource Manager VNET, moet de service zich in een speciale subnet dat geen andere resources, met uitzondering van exemplaren van Azure API Management bevat. Als een poging is gedaan voor het implementeren van een Azure API Management-exemplaar met een Resource Manager VNET subnet die andere resources bevat, mislukt de implementatie.
    >

    ![Selecteer VPN][api-management-setup-vpn-select]

5. Klik op **opslaan** aan de bovenkant van het scherm.

> [!NOTE]
> Het VIP-adres van het exemplaar van API Management wordt gewijzigd wanneer VNET is ingeschakeld of uitgeschakeld.  
> Het VIP-adres, wijzigt u ook wanneer API Management wordt verplaatst van **externe** naar **intern** of omgekeerd
>

> [!IMPORTANT]
> Als u API Management uit een VNET verwijderen of de machine is geïmplementeerd in een wijzigt, kan de eerder gebruikte VNET tot 4 uur te blijven vergrendeld. Tijdens deze periode wordt het niet mogelijk zijn om te verwijderen van het VNET of implementeren van een nieuwe resource toe.

## <a name="enable-vnet-powershell"></a>Inschakelen VNET-verbinding met PowerShell-cmdlets
U kunt ook inschakelen met behulp van de PowerShell-cmdlets voor VNET-connectiviteit

* **Maak een API Management-service binnen een VNET**: Gebruik de cmdlet [nieuw AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) om een Azure API Management-service binnen een VNET te maken.

* **Implementeren van een bestaande API Management-service binnen een VNET**: Gebruik de cmdlet [Update AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) verplaatsen van een bestaande Azure API Management-service in een virtueel netwerk.

## <a name="connect-vnet"></a>Verbinding maken met een webservice die wordt gehost in een virtueel netwerk
Nadat uw API Management-service is verbonden met het VNET, gaat toegang tot services in het back-end niet anders zijn dan de toegang tot openbare services zijn. Typ in het lokale IP-adres of de hostnaam (als een DNS-server is geconfigureerd voor het VNET) van uw web-service in de **webservice-URL** veld bij het maken van een nieuwe API of een bestaande bewerkt.

![API via VPN-netwerk toevoegen][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Algemene configuratie netwerkproblemen
Hier volgt een lijst met veelvoorkomende configuratiefouten die optreden kunnen tijdens het implementeren van API Management-service in een virtueel netwerk.

* **Aangepaste DNS-server setup**: de API Management-service is afhankelijk van verschillende Azure-services. Wanneer API Management wordt gehost in een VNET met een aangepaste DNS-server, moet het omzetten van de hostnamen van deze Azure-services. Volg [dit](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) hulp bij het aangepaste DNS-instellingen. Zie de onderstaande tabel voor poorten en andere netwerkvereisten ter referentie.

> [!IMPORTANT]
> Het verdient aanbeveling dat, als u van een aangepaste DNS-server (s) voor het VNET gebruikmaakt, u dat instellen **voordat** een API Management-service implementeren in de App. Anders moet u de API Management-service bijwerken telkens wanneer u de DNS-Servers (s) met wijzigen de [netwerkbewerking configuratie toepassen](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/ApplyNetworkConfigurationUpdates)

* **Vereiste poorten voor API Management**: binnenkomend en uitgaand verkeer in het Subnet waarin API Management wordt geïmplementeerd, kan worden beheerd met behulp van [Netwerkbeveiligingsgroep][Network Security Group]. Als een van deze poorten niet beschikbaar zijn, wordt API Management werkt mogelijk niet correct en kan ontoegankelijk worden. Met een of meer van deze poorten geblokkeerd is een andere veelvoorkomende onjuiste configuratie probleem wanneer u API Management met een VNET.

Wanneer een exemplaar van API Management-service wordt gehost in een VNET, worden de poorten in de volgende tabel worden gebruikt.

| Bron / bestemming poort(en) | Richting | Protocol-transport | Bron / doel | Doel (*) | Type virtueel netwerk |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Inkomend |TCP |INTERNET / VIRTUAL_NETWORK|Communicatie van clients met API Management|Extern |
| * / 3443 |Inkomend |TCP |INTERNET / VIRTUAL_NETWORK|Eindpunt voor Azure-portal en Powershell |Intern |
| * / 80, 443 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET|**Afhankelijkheid van Azure Storage**, Azure Service Bus en Azure Active Directory (indien van toepassing).|Externe & interne | 
| * / 1433 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET|**Toegang tot Azure SQL-eindpunten** |Externe & interne |
| * / 5671, 5672 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET|Afhankelijkheid voor logboek Event Hub-beleid en bewakingsagent |Externe & interne |
| * / 445 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET|Afhankelijkheid van Azure-bestandsshare voor GIT |Externe & interne |
| * / 25028 |Uitgaand |TCP |VIRTUAL_NETWORK / INTERNET|Verbinding maken met de SMTP-Relay voor het verzenden van e-mailberichten |Externe & interne |
| * / 6381 - 6383 |Binnenkomend en uitgaand |TCP |VIRTUAL_NETWORK / VIRTUAL_NETWORK|Exemplaren van toegang tot Redis-Cache tussen RoleInstances |Externe & interne |
| * / * | Inkomend |TCP |AZURE_LOAD_BALANCER / VIRTUAL_NETWORK| Azure-infrastructuur Load Balancer |Externe & interne |

>[!IMPORTANT]
> * De poorten waarvoor de *doel* is **vet** zijn vereist voor API Management-service is geïmplementeerd. De andere poorten worden geblokkeerd wordt echter tot mindere de mogelijkheid om te gebruiken en controleren van de service.

* **SSL-functionaliteit**: inschakelen van SSL-certificaat-ketens en validatie van de API Management-service moet uitgaande netwerkverbinding ocsp.msocsp.com, mscrl.microsoft.com en crl.microsoft.com. Deze afhankelijkheid is niet vereist, als een certificaat dat u naar API Management uploadt de volledige keten aan de basis-CA bevat.

* **DNS-toegang**: uitgaand verkeer op poort 53 is vereist voor communicatie met de DNS-servers. Als een aangepaste DNS-server op het andere einde van een VPN-gateway bestaat, moet de DNS-server bereikbaar is vanuit het subnet voor het hosten van API Management zijn.

* **Metrische gegevens en statuscontrole**: uitgaande netwerkverbinding met de Azure Monitoring-eindpunten die onder de volgende domeinen oplossen: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Express Route-instelling**: een algemene configuratie van de klant is voor het definiëren van hun eigen standaardroute (0.0.0.0/0), waardoor uitgaand internetverkeer in plaats daarvan lokale stromen. Dit netwerkverkeer altijd verbinding met Azure API Management wordt verbroken omdat het uitgaande verkeer geblokkeerd on-premises wordt of NAT zou een onherkenbare set adressen die niet langer met verschillende Azure-eindpunten werken. De oplossing is voor het definiëren van een (of meer) gebruiker gedefinieerde routes ([udr's][UDRs]) op het subnet waarin de Azure API Management. Een UDR definieert subnet-specifieke routes die in plaats van de standaardroute gehonoreerd.
  Het verdient indien mogelijk, gebruik de volgende configuratie:
 * Configuratie van de ExpressRoute kondigt 0.0.0.0/0 en standaard force alle uitgaande verkeer lokale tunnels.
 * De UDR toegepast op het subnet met de Azure API Management definieert 0.0.0.0/0 met een volgend hoptype van Internet.
 Het gecombineerde effect van deze stappen is dat subnetniveau UDR voorrang op de ExpressRoute geforceerde tunneling heeft, zodat uitgaande toegang tot Internet vanaf de Azure API Management.

**Routering via virtuele netwerkapparaten**: configuraties waarmee een UDR met een standaardroute (0.0.0.0/0) internet bestemd verkeer vanuit het subnet van API Management routeren via een netwerk van een virtueel apparaat worden uitgevoerd in Azure kunnen volledige communicatie tussen de API Management en de vereiste services. Deze configuratie wordt niet ondersteund. 

>[!WARNING]  
>Azure API Management wordt niet ondersteund met ExpressRoute-configuraties die **onjuist cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering**. ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangen route-advertisements van Microsoft voor een groot aantal Microsoft Azure-IP-adresbereiken. Als deze adresbereiken onjuist cross aangekondigd op het pad voor persoonlijke peering, is het eindresultaat dat alle uitgaande pakketten van het Azure API Management-exemplaar subnet onjuist force via een tunnel naar een klant on-premises netwerk infrastructuur. Deze stroom van het netwerk verbreekt Azure API Management. De oplossing voor dit probleem is cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering stoppen.


## <a name="troubleshooting"></a>Probleemoplossing
* **Initiële Setup**: wanneer de eerste implementatie van API Management-service in een subnet niet slaagt, wordt u aangeraden eerst een virtuele machine implementeren in hetzelfde subnet. Extern bureaublad van de volgende in de virtuele machine en controleer of er verbinding met een van elke resource hieronder in uw azure-abonnement 
    * Azure Storage-blob
    * Azure SQL Database

 > [!IMPORTANT]
 > Nadat u de connectiviteit hebt gevalideerd, zorg er dan voor dat alle resources die zijn geïmplementeerd in het subnet voor het implementeren van API Management in het subnet te verwijderen.

* **Incrementele Updates**: wanneer u wijzigingen in uw netwerk, verwijst naar [NetworkStatus API](https://docs.microsoft.com/rest/api/apimanagement/networkstatus), om te controleren dat de API Management-service geen toegang tot de kritieke bronnen die dit is afhankelijk van is verbroken. De verbindingsstatus moet worden bijgewerkt om de 15 minuten.

* **Resourcenavigatiekoppelingen**: bij het implementeren in Resource Manager stijl vnet subnet, API Management het subnet zijn gereserveerd door het maken van een resource-navigatie koppeling. Als het subnet al een resource van een andere provider bevat, implementatie zal **mislukken**. Op dezelfde manier als u een API Management-service naar een ander subnet verplaatsen of verwijderen, wordt verwijderd die resource navigatie-koppeling. 

## <a name="routing"></a> Routering
+ Een taakverdeling openbare IP-adres (VIP) gereserveerd voor toegang tot alle service-eindpunten.
+ Een IP-adres van een subnet IP-adresbereik (DIP) wordt gebruikt voor toegang tot resources binnen het vnet en een openbare IP-adres (VIP) wordt gebruikt voor toegang tot bronnen buiten het vnet.
+ Openbare IP-adres met gelijke taakverdeling adres vindt u op de blade overzicht/Essentials in de Azure portal.

## <a name="limitations"></a>Beperkingen
* Een subnet met exemplaren van API Management kan geen andere Azure-brontypen bevatten.
* Het subnet en het API Management-service moeten zich in hetzelfde abonnement.
* Een subnet met exemplaren van API Management kan niet worden verplaatst tussen abonnementen.
* Gebruikers zijn verantwoordelijk voor het beheren van de taakverdeling tussen meerdere regio's, zoals ze eigenaar zijn van de routering voor meerdere landen/regio API Management-implementaties in de modus intern virtueel netwerk geconfigureerd.


## <a name="related-content"></a>Verwante inhoud
* [Een virtueel netwerk verbinding te maken met back-end via VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Verbinding maken met een virtueel netwerk vanuit verschillende implementatiemodellen](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Het gebruik van de API-Inspector te traceren aanroepen in Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
