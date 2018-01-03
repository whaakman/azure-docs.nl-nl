---
title: Azure DB Cosmos-firewallondersteuning & IP-toegangsbeheer | Microsoft Docs
description: Informatie over het gebruik van IP-beleid voor toegangsbeheer voor firewallondersteuning op de accounts van Azure DB die Cosmos-database.
keywords: Toegangsbeheer voor IP-, firewallondersteuning
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: ankshah
ms.openlocfilehash: 85f5e0e076f92afc79ed8f4ce652bb0f31923113
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-cosmos-db-firewall-support"></a>Ondersteuning van Azure DB Cosmos-firewall
Als u wilt beveiligen gegevens die zijn opgeslagen in een databaseaccount Azure Cosmos DB, Azure Cosmos DB heeft biedt ondersteuning voor een geheim op basis van [autorisatie model](https://msdn.microsoft.com/library/azure/dn783368.aspx) die gebruikmaakt van een sterke Hash-based message authentication code (HMAC). Azure Cosmos DB ondersteunt nu, naast het model geheime gebaseerde autorisatie beleid IP gebaseerd toegangsbeheer voor binnenkomende firewallondersteuning aangestuurd. Dit model is vergelijkbaar met de firewallregels van een systeem van traditionele databases en biedt een extra beveiligingsniveau voor de account van de Azure DB die Cosmos-database. Met dit model, kunt u nu een databaseaccount Azure Cosmos DB om te worden alleen toegankelijk vanuit een goedgekeurde set machines en/of cloud services configureren. Toegang tot Azure Cosmos DB resources uit deze goedgekeurde sets met machines en services vereisen nog steeds de aanroeper om weer te geven van een geldige verificatietoken.

## <a name="ip-access-control-overview"></a>IP-Toegangsbeheer-overzicht
Een Azure DB die Cosmos-databaseaccount is standaard toegankelijk is vanaf het openbare internet, zolang de aanvraag vergezeld van een geldige verificatietoken gaat. De gebruiker moet de verzameling van IP-adressen of IP-adresbereiken in CIDR-formulier moeten worden opgenomen als de lijst met toegestane van client-IP-adressen voor een bepaalde database-account opgeven voor het configureren van IP-toegangsbeheer op basis van beleid. Zodra deze configuratie wordt toegepast, worden alle aanvragen die afkomstig zijn van computers buiten deze lijst met toegestane geblokkeerd door de server.  De verwerking van de stroom voor het besturingselement toegang op basis van IP-verbinding wordt beschreven in het volgende diagram:

![Diagram van het verbindingsproces voor toegangsbeheer op basis van IP](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a>Het IP-beleid voor toegangsbeheer configureren
Het IP-beleid voor toegangsbeheer kan worden ingesteld in de Azure portal of programmatisch via [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), of de [REST-API](/rest/api/documentdb/) door het bijwerken van de **ipRangeFilter** eigenschap. 

Om in te stellen de IP-beleid voor toegangsbeheer in Azure portal, gaat u naar de pagina Azure DB die Cosmos-account, klikt u op **Firewall** in het navigatiemenu, wijzigt u vervolgens de **IP-toegangsbeheer inschakelen** waarde **ON**. 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure portal openen](./media/firewall-support/azure-portal-firewall.png)

Wanneer IP-toegangsbeheer is op, biedt de portal switches voor toegang tot de Azure-portal, andere Azure-services en het huidige IP-adres. Meer informatie over de schakelopties is opgegeven in de volgende secties.

![Schermopname die laat zien hoe u een firewall-instellingen configureren in de Azure portal](./media/firewall-support/azure-portal-firewall-configure.png)

> [!NOTE]
> Als u een IP-toegangsbeleid voor het account van uw Azure DB die Cosmos-database, alle toegang tot uw Azure DB die Cosmos-databaseaccount vanaf computers buiten de geconfigureerde toegestaan lijst met IP-adresbereiken worden geblokkeerd. Doordat dit model wordt bladeren door de bewerking voor het vlak van gegevens vanuit de portal ook geblokkeerd om te controleren of de integriteit van toegangsbeheer.

## <a name="connections-from-the-azure-portal"></a>Verbindingen van de Azure-portal 

Wanneer u een IP-beleid voor toegangsbeheer programmatisch inschakelt, moet u het IP-adres voor de Azure-portal toevoegen de **ipRangeFilter** eigenschap toegankelijk. De portal IP-adressen zijn:

|Regio|IP-adres|
|------|----------|
|Alle regio's die zijn opgegeven behalve hieronder|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Duitsland|51.4.229.218|
|China|139.217.8.252|
|Amerikaanse overheid|52.244.48.71|

Instellen voor toegang tot de Azure-portal via de Azure portal, de **toegang geven tot de Azure Portal** van waarde naar **ON** in de Azure portal (de **IP-toegangsbeheer inschakelen** waarde moet worden ingesteld op **ON** bekijken en wijzigen de **toegang geven tot de Azure Portal** waarde).

![Schermopname die laat zien hoe u Azure portal-toegang](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-other-azure-paas-services"></a>Verbindingen van andere Azure-PaaS-services 
In Azure, PaaS-services zoals Azure Stream analytics, Azure Functions en Azure App Service gebruikt in combinatie met Azure Cosmos DB. Voor toegang tot Azure Cosmos DB databaseaccount van deze services met IP-adressen niet beschikbaar zijn het IP-adres 0.0.0.0 toevoegen aan de lijst met toegestane IP-adressen die zijn gekoppeld aan uw Azure DB die Cosmos-databaseaccount programmatisch of instellen de **Toegang tot Azure-Services toestaan** waarde op ON in de Azure-portal (de **IP-toegangsbeheer inschakelen** waarde moet worden ingesteld op **ON** bekijken en wijzigen de **toestaan Toegang tot Azure-Services** waarde). Dit zorgt ervoor dat Azure PaaS-services toegang hebben tot de Azure DB die Cosmos-account. 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure portal openen](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Verbindingen van uw huidige IP

Als u wilt vereenvoudigen, de Azure-portal helpt u te identificeren en het IP-adres van uw client-computer toevoegen aan de lijst met toegestane zodat apps die worden uitgevoerd van de computer toegang heeft tot de Azure DB die Cosmos-account. IP-adres van de client hier is gedetecteerd in de portal. Kan de client-IP-adres van uw machine zijn, maar dit kan ook worden de IP-adres van de gateway van uw netwerk. Vergeet niet om deze te verwijderen voordat u doorgaat naar productie.

![Schermopname die laat zien hoe u een firewall-instellingen voor huidige IP-adres configureren](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Verbindingen met cloudservices
Cloudservices zijn in Azure, een veelgebruikte manier voor het hosten van de middelste laag service logica met behulp van Azure Cosmos DB. Als u wilt toegang inschakelen voor een Azure DB die Cosmos-databaseaccount van een cloudservice, het openbare IP-adres van de cloudservice moet worden toegevoegd aan de lijst met toegestane IP-adressen die zijn gekoppeld aan uw Azure DB die Cosmos-databaseaccount door [configureren van de IP-beleid voor toegangsbeheer](#configure-ip-policy).  Dit zorgt ervoor dat alle rolexemplaren van cloud-services toegang tot uw databaseaccount Azure Cosmos DB hebben. U kunt IP-adressen ophalen voor uw cloudservices in de Azure portal, zoals wordt weergegeven in de volgende schermafbeelding:

![Schermopname van het openbare IP-adres voor een cloudservice die wordt weergegeven in de Azure-portal](./media/firewall-support/public-ip-addresses.png)

Wanneer u uw cloudservice uitbreiden door aanvullende rol exemplaren toe te voegen, hebben deze nieuwe exemplaren automatisch toegang tot het account van de Azure DB die Cosmos-database omdat ze deel van dezelfde cloudservice uitmaken.

## <a name="connections-from-virtual-machines"></a>Verbindingen van virtuele machines
[Virtuele machines](https://azure.microsoft.com/services/virtual-machines/) of [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kan ook worden gebruikt voor het hosten van de middelste laag services met behulp van Azure Cosmos DB.  Voor het configureren van de account van de Azure DB die Cosmos-database voor toegang van virtuele machines, openbare IP-adressen van virtuele machine en/of de virtuele-machineschaalset moeten worden geconfigureerd als een van de toegestane IP-adressen voor het account van uw Azure DB die Cosmos-database door [configureren van de IP-beleid voor toegangsbeheer](#configure-ip-policy). U kunt IP-adressen voor virtuele machines in de Azure-portal ophalen, zoals wordt weergegeven in de volgende schermafbeelding.

![Schermopname van een openbaar IP-adres voor een virtuele machine weergegeven in de Azure-portal](./media/firewall-support/public-ip-addresses-dns.png)

Wanneer u aanvullende virtuele machines aan de groep toevoegt, worden ze automatisch toegang tot uw Azure DB die Cosmos-databaseaccount opgegeven.

## <a name="connections-from-the-internet"></a>Clientverbindingen van het internet
Wanneer u een databaseaccount Azure Cosmos DB vanaf een computer op het internet openen, moet de client-IP-adres of IP-adresbereik van de machine worden toegevoegd aan de lijst met toegestane IP-adres voor het account van de Azure DB die Cosmos-database. 

## <a name="troubleshooting-the-ip-access-control-policy"></a>Het oplossen van het IP-beleid voor toegangsbeheer
### <a name="portal-operations"></a>Portal-bewerkingen
Als u een IP-toegangsbeleid voor het account van uw Azure DB die Cosmos-database, alle toegang tot uw Azure DB die Cosmos-databaseaccount vanaf computers buiten de geconfigureerde toegestaan lijst met IP-adresbereiken worden geblokkeerd. Dus als u inschakelen portal gegevens vlak bewerkingen wilt zoals het bladeren door verzamelingen en query-documenten, moet u expliciet toestaan toegang tot de Azure portal met behulp van de **Firewall** pagina in de portal. 

![Schermopname die laat zien hoe u een voor toegang tot de Azure-portal](./media/firewall-support/enable-azure-portal.png)

### <a name="sdk--rest-api"></a>SDK & Rest-API
Voor toegang via SDK of REST-API van computers niet in de lijst met toegestane uit veiligheidsoverwegingen het resultaat van algemene 404 niet gevonden zonder extra details. Controleer of het IP-adres toegestaan lijst die zijn geconfigureerd voor het account van uw Azure DB die Cosmos-database om te controleren of de dat configuratie van het juiste beleid wordt toegepast op uw databaseaccount Azure Cosmos DB.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tips voor betere prestaties netwerkgerelateerde [tips voor betere prestaties](performance-tips.md).

