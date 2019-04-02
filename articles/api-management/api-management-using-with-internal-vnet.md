---
title: Over het gebruik van Azure API Management met interne virtuele netwerken | Microsoft Docs
description: Meer informatie over het instellen en configureren van Azure API Management op een intern virtueel netwerk
services: api-management
documentationcenter: ''
author: vladvino
manager: kjoshi
editor: ''
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: apimpm
ms.openlocfilehash: da27c772a0650a923068b3c519ef39494573f96a
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793115"
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Met behulp van Azure API Management-service met een intern virtueel netwerk
Met virtuele netwerken van Azure beheren Azure API Management API's die niet toegankelijk is via internet. Een aantal VPN-technologieën zijn beschikbaar voor het maken van de verbinding. API Management kan worden geïmplementeerd in twee belangrijke modi binnen een virtueel netwerk:
* Extern
* Intern

Wanneer er wordt een API Management geïmplementeerd in de modus voor intern virtueel netwerk, zijn alle service-eindpunten (gateway, de portal voor ontwikkelaars, de Azure portal, direct beheer en Git) alleen zichtbaar binnen een virtueel netwerk dat u de toegang tot beheert. Geen van de service-eindpunten zijn geregistreerd op de openbare DNS-server.

U kunt de volgende scenario's met behulp van API Management in de modus voor interne, bereiken:

* API's die worden gehost in uw privéverbindingen voor datacenters veilig toegankelijk worden gemaakt door derde partijen buiten deze met behulp van site-naar-site of Azure ExpressRoute-VPN-verbindingen maken
* Hybride cloud-scenario's inschakelen bij het blootstellen van uw cloud-gebaseerde API's en on-premises API's via een gateway.
* Beheer uw API's die worden gehost in meerdere geografische locaties met behulp van een enkele gateway-eindpunt.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ **Een actief Azure-abonnement**.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Een Azure API Management-exemplaar**. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).

## <a name="enable-vpn"> </a>Het maken van een API Management in een intern virtueel netwerk
De API Management-service in een intern virtueel netwerk wordt gehost achter een [interne load balancer (klassiek)](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-ilb-classic-cloud). Dit is de enige beschikbare optie en kan niet worden gewijzigd.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>De verbinding van een virtueel netwerk met behulp van de Azure-portal inschakelen

1. Blader naar de Azure API Management-instantie in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **Virtueel netwerk**.
3. De API Management-exemplaar om te worden geïmplementeerd in het virtuele netwerk configureren.

    ![Menu voor het instellen van een Azure API Management in een intern virtueel netwerk][api-management-using-internal-vnet-menu]

4. Selecteer **Opslaan**.

Nadat de implementatie is voltooid, ziet u **persoonlijke** virtuele IP-adres en **openbare** virtuele IP-adres van uw API Management-service op de overzichtsblade. De **persoonlijke** virtuele IP-adres is een load balanced IP-adres van de API Management overgedragen subnet waarover `gateway`, `portal`, `management` en `scm` eindpunten kunnen worden geopend. De **openbare** virtuele IP-adres wordt gebruikt **alleen** voor het besturingselement vlak van het verkeer naar `management` eindpunt meer dan poort 3443 en kan worden vergrendeld omlaag naar de [ApiManagement] [ ServiceTags] servicetag.

![API Management-dashboard met een intern virtueel netwerk geconfigureerd][api-management-internal-vnet-dashboard]

> [!NOTE]
> De Test-console beschikbaar zijn in de Azure Portal werkt niet voor **intern** VNET service hebt geïmplementeerd als de Url van de Gateway is niet geregistreerd op de openbare DNS-server. U moet in plaats daarvan gebruikt u de Test-Console die is opgegeven op de **ontwikkelaarsportal**.

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>Inschakelen van de verbinding van een virtueel netwerk met behulp van PowerShell-cmdlets

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt ook verbinding met het virtuele netwerk inschakelen met behulp van PowerShell-cmdlets.

* Een API Management-service binnen een virtueel netwerk maken: Gebruik de cmdlet [New-AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) naar een Azure API Management-service binnen een virtueel netwerk maken en configureren voor het gebruik van het type intern virtueel netwerk.

* Een bestaande implementatie van een API Management-service binnen een virtueel netwerk bijwerken: Gebruik de cmdlet [Update AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) te verplaatsen van een bestaande API Management-service binnen een virtueel netwerk configureren voor het gebruik van het type intern virtueel netwerk.

## <a name="apim-dns-configuration"></a>DNS-configuratie
Wanneer er is een API Management in de modus voor extern virtueel netwerk, worden de DNS-server wordt beheerd door Azure. U hebt voor het beheren van uw eigen routering voor de modus intern virtueel netwerk.

> [!NOTE]
> API Management-service niet luisteren naar aanvragen die afkomstig zijn van IP-adressen. Alleen reageert op aanvragen voor de hostnaam die is geconfigureerd op de service-eindpunten. Deze eindpunten zijn gateway, de Azure-portal en de portal voor ontwikkelaars, direct beheereindpunt en Git.

### <a name="access-on-default-host-names"></a>Toegang op standaard hostnamen
Bij het maken van een API Management-service, bijvoorbeeld met de naam "contosointernalvnet" worden standaard de volgende service-eindpunten geconfigureerd:

   * Gateway of proxy: contosointernalvnet.azure-api.net

   * De Azure-portal en de portal voor ontwikkelaars: contosointernalvnet.portal.azure-api.net

   * Direct beheereindpunt: contosointernalvnet.management.azure-api.net

   * GIT: contosointernalvnet.scm.azure-api.net

Voor toegang tot deze API Management service-eindpunten, kunt u een virtuele machine maken in een subnet dat is verbonden met het virtuele netwerk waarin API Management is geïmplementeerd. Ervan uitgaande dat de interne virtuele IP-adres voor uw service 10.1.0.5 is, kunt u toewijzen het hosts-bestand % SystemDrive%\drivers\etc\hosts, als volgt:

   * 10.1.0.5     contosointernalvnet.azure-api.net

   * 10.1.0.5     contosointernalvnet.portal.azure-api.net

   * 10.1.0.5 contosointernalvnet.management.azure-api.net

   * 10.1.0.5 contosointernalvnet.scm.azure-api.net

U kunt vervolgens toegang tot alle service-eindpunten van de virtuele machine die u hebt gemaakt.
Als u een aangepaste DNS-server in een virtueel netwerk gebruikt, kunt u ook een DNS-records maken en toegang hebben tot deze eindpunten vanaf elke locatie in uw virtuele netwerk.

### <a name="access-on-custom-domain-names"></a>Toegang tot aangepaste domeinnamen

1. Als u toegang tot de API Management-service met de standaard-hostnamen niet wilt, kunt u aangepaste domeinnamen voor alle uw service-eindpunten zoals wordt weergegeven in de volgende afbeelding kunt instellen:

   ![Instellen van een aangepast domein voor API Management][api-management-custom-domain-name]

2. Vervolgens maakt u records in de DNS-server toegang hebben tot de eindpunten die alleen toegankelijk vanuit uw virtuele netwerk zijn.

## <a name="routing"> </a> Routing
+ Een gelijke persoonlijke virtuele IP-adres uit het subnetbereik wordt gereserveerd en gebruikt voor toegang tot de API Management service-eindpunten van binnen het vnet.
+ Een gelijke openbare IP-adres (VIP) wordt ook worden gereserveerd voor toegang tot het beheer van service-eindpunt alleen via poort 3443.
+ Een IP-adres van een subnet-IP-adresbereik (DIP) wordt gebruikt voor toegang tot resources binnen het vnet en een openbare IP-adres (VIP) wordt gebruikt voor toegang tot bronnen buiten het vnet.
+ Met Load Balancing openbare en privé-IP-adressen kunnen worden gevonden op de blade overzicht/Essentials in Azure portal.

## <a name="related-content"> </a>Gerelateerde inhoud
Zie de volgende artikelen voor meer informatie:
* [Veelvoorkomende problemen met het netwerk configuratie tijdens het instellen van Azure API Management in een virtueel netwerk][Common network configuration problems]
* [Virtueel netwerk Veelgestelde vragen](../virtual-network/virtual-networks-faq.md)
* [Het maken van een record in DNS](/previous-versions/windows/it-pro/windows-2000-server/bb727018(v=technet.10))

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-using-with-internal-vnet.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

[ServiceTags]: ../virtual-network/security-overview.md#service-tags

