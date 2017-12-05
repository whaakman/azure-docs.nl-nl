---
title: Het gebruik van Azure API Management met interne virtuele netwerken | Microsoft Docs
description: Meer informatie over het instellen en configureren van Azure API Management op een intern virtueel netwerk
services: api-management
documentationcenter: 
author: vladvino
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apimpm
ms.openlocfilehash: df2ebb6ee8b1f108c751226188556ced907314e1
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Gebruik van Azure API Management-service met een intern virtueel netwerk
Met virtuele netwerken van Azure beheren Azure API Management API's die niet toegankelijk op het internet. Een aantal VPN-technologieën zijn beschikbaar voor het maken van de verbinding. API Management kan worden geïmplementeerd in twee belangrijke modi binnen een virtueel netwerk:
* Extern
* Intern


Wanneer API Management wordt geïmplementeerd in de modus intern virtueel netwerk, zijn alle service-eindpunten (gateway, developer-portal, publicatieportal, direct beheer en Git) alleen zichtbaar zijn in een virtueel netwerk dat u de toegang tot beheren. Geen van de service-eindpunten zijn geregistreerd op de openbare DNS-server.

U kunt de volgende scenario's met behulp van API Management in de interne modus, bereiken:
* Controleer de API's die worden gehost in uw persoonlijke datacenter veilig toegankelijk door derden buiten via site-naar-site of Azure ExpressRoute VPN-verbindingen.
* Hybride cloud-scenario's mogelijk bij het blootstellen van uw cloud-gebaseerde API's en API's voor lokale via een gemeenschappelijke gateway.
* Beheren van uw API's die worden gehost in meerdere geografische locaties met behulp van een enkele gateway-eindpunt. 


## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ **Een actief Azure-abonnement**.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Exemplaar van Azure API Management**. Zie voor meer informatie [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

## <a name="enable-vpn"></a>Maken van een API Management in een intern virtueel netwerk
De API Management-service in een intern virtueel netwerk wordt achter een interne load balancer (ILB) gehost.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Een virtueel netwerkverbinding met de Azure portal inschakelen

1. Blader naar uw Azure API Management-exemplaar in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **virtueel netwerk**.
3. Het exemplaar van API Management te worden geïmplementeerd in het virtuele netwerk configureren.

    ![Menu voor het instellen van een Azure API Management in een intern virtueel netwerk][api-management-using-internal-vnet-menu]

4. Selecteer **Opslaan**.

Nadat de implementatie is geslaagd, ziet u de interne virtuele IP-adres van uw service op het dashboard.

![API Management dashboard met een intern virtueel netwerk geconfigureerd][api-management-internal-vnet-dashboard]

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>Een virtueel netwerkverbinding inschakelen met behulp van PowerShell-cmdlets
U kunt ook virtuele netwerkverbindingen inschakelen met behulp van PowerShell-cmdlets.

* Een API Management-service in een virtueel netwerk maken: Gebruik de cmdlet [nieuw AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) een Azure API Management-service in een virtueel netwerk maken en configureren voor het gebruik van het type intern virtueel netwerk.

* Een bestaande API Management-service in een virtueel netwerk hebt geïmplementeerd: Gebruik de cmdlet [Update AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) verplaatsen van een bestaande API Management-service in een virtueel netwerk en configureert voor het gebruik van de interne type virtueel netwerk.

## <a name="apim-dns-configuration"></a>DNS-configuratie
Wanneer u API Management in de modus voor extern virtueel netwerk, worden de DNS-server wordt beheerd door Azure. U hebt voor de modus van een intern virtueel netwerk voor het beheren van uw eigen routering.

> [!NOTE]
> API Management-service luistert niet op aanvragen die afkomstig zijn van IP-adressen. Alleen reageert op aanvragen voor de hostnaam die is geconfigureerd op de service-eindpunten. Deze eindpunten zijn gateway, developer-portal, publicatieportal, direct beheer eindpunt en Git.

### <a name="access-on-default-host-names"></a>Toegang op standaard hostnamen
Wanneer u een API Management-service, bijvoorbeeld de naam 'contoso' maakt worden standaard de volgende service-eindpunten geconfigureerd:

   * Gateway of proxy: contoso.azure api.net

   * Publicatieportal en -portal voor ontwikkelaars: contoso.portal.azure api.net

   * Direct beheer eindpunt: contoso.management.azure api.net

   * GIT: contoso.scm.azure-api.net

Voor toegang tot deze API Management-service-eindpunten, kunt u een virtuele machine in een subnet is verbonden met het virtuele netwerk waarin API Management wordt geïmplementeerd. Ervan uitgaande dat de interne virtuele IP-adres voor uw service 10.0.0.5 is, kunt u toewijzen het hosts-bestand % SystemDrive%\drivers\etc\hosts, als volgt:

   * 10.0.0.5 contoso.azure-api.net

   * 10.0.0.5 contoso.portal.azure-api.net

   * 10.0.0.5 contoso.management.azure-api.net

   * 10.0.0.5 contoso.scm.azure-api.net

U kunt vervolgens toegang tot alle service-eindpunten van de virtuele machine die u hebt gemaakt. Als u een aangepaste DNS-server in een virtueel netwerk gebruikt, u kunt ook een DNS-records maken en toegang tot deze eindpunten vanaf elke locatie in het virtuele netwerk. 

### <a name="access-on-custom-domain-names"></a>Toegang op aangepaste domeinnamen

   1. Als u toegang tot de API Management-service met de standaard hostnamen niet wilt, kunt u aangepaste domeinnamen voor alle uw service-eindpunten zoals weergegeven in de volgende afbeelding kunt instellen: 

   ![Instellen van een aangepast domein voor API Management][api-management-custom-domain-name]

   2. Vervolgens kunt u records maken in uw DNS-server voor toegang tot de eindpunten die alleen toegankelijk vanuit het virtuele netwerk.

## <a name="routing"></a> Routering
+ Een taakverdeling persoonlijke virtuele IP-adres uit het subnetbereik wordt gereserveerd en wordt gebruikt voor toegang tot de API Management service-eindpunten van binnen het vnet.
+ Een taakverdeling openbare IP-adres (VIP) ook gereserveerd om toegang te bieden met de management service-eindpunt alleen via poort 3443.
+ Een IP-adres van een subnet IP-adresbereik (DIP) wordt gebruikt voor toegang tot resources binnen het vnet en een openbare IP-adres (VIP) wordt gebruikt voor toegang tot bronnen buiten het vnet.
+ Met gelijke taakverdeling openbare en particuliere IP-adressen kunnen worden gevonden op de blade overzicht/Essentials in de Azure portal.

## <a name="related-content"></a>Verwante inhoud
Zie voor meer informatie de volgende artikelen:
* [Algemene problemen met het netwerk configuratie tijdens het instellen van Azure API Management in een virtueel netwerk][Common network configuration problems]
* [Virtueel netwerk Veelgestelde vragen](../virtual-network/virtual-networks-faq.md)
* [Maken van een record in DNS](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

