---
title: Azure Cosmos DB-firewallondersteuning & IP-toegangsbeheer | Microsoft Docs
description: Informatie over het gebruik van beleid voor IP-toegangsbeheer voor firewallondersteuning op de Azure Cosmos DB-database-accounts.
keywords: IP-toegangsbeheer, firewallondersteuning
services: cosmos-db
author: kanshiG
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: govindk
ms.openlocfilehash: 7c9367cccf8d59d60dfa474f02567d59b9c8c8c2
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054038"
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure Cosmos DB-firewallondersteuning
Als u wilt beveiligen gegevens die zijn opgeslagen in een Azure Cosmos DB-databaseaccount, Azure Cosmos DB heeft biedt ondersteuning voor een geheim op basis van [autorisatiemodel](https://msdn.microsoft.com/library/azure/dn783368.aspx) die gebruikmaakt van een sterke Hash-based message authentication code (HMAC). Azure Cosmos DB ondersteunt nu, naast het geheim op basis van autorisatiemodel, beleid voor IP-gebaseerd toegangsbeheer voor ondersteuning van de firewallregels voor binnenkomend aangestuurd. Dit model is vergelijkbaar met de firewall-regels van een traditionele database-systeem en biedt een extra beveiligingsniveau bij de Azure Cosmos DB-databaseaccount. Met dit model kunt u nu een Azure Cosmos DB-databaseaccount om te worden alleen toegankelijk vanuit een goedgekeurde set computers en/of cloud services configureren. Toegang tot Azure Cosmos DB-resources uit deze goedgekeurde sets van machines en services is nog steeds vereist voor de oproepende functie om weer te geven van een geldige Autorisatietoken.

> [!NOTE]
> Firewall-ondersteuning is momenteel beschikbaar voor Azure Cosmos DB SQL API en de Mongo-API-accounts. Mogelijkheid voor het configureren van firewalls voor andere API's en onafhankelijke clouds, zoals Azure Duitsland of Azure Government is binnenkort beschikbaar. Als u van plan bent om te configureren van service-eindpunt ACL voor uw Azure Cosmos DB-account met een bestaande IP-firewall is geconfigureerd, houd er rekening mee de firewallconfiguratie, verwijderen van de IP-firewall en configureer vervolgens de eindpunt-ACL van de service. Nadat u service-eindpunt configureert, kunt u de IP-firewall opnieuw inschakelen indien nodig.

## <a name="ip-access-control-overview"></a>IP-Toegangsbeheer-overzicht
Een Azure Cosmos DB-databaseaccount is standaard toegankelijk via openbaar internet zolang de aanvraag wordt vergezeld van een geldige autorisatietoken. Als u IP-toegangsbeheer op basis van beleid wilt configureren, moet u de set IP-adressen of IP-adresbereiken in CIDR-notatie opgeven om op te nemen als toegestane lijst van client-IP's voor een bepaald databaseaccount. Wanneer deze configuratie is toegepast, worden alle aanvragen die afkomstig zijn van computers buiten deze toegestane lijst geblokkeerd door de server.  De verwerking van de stroom voor het besturingselement op basis van de IP-verbinding wordt beschreven in het volgende diagram:

![Diagram van het verbindingsproces voor toegangsbeheer op basis van IP](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> Het beleid voor IP-toegangsbeheer configureren
Het beleid voor IP-toegangsbeheer kan worden ingesteld in Azure portal of programmatisch via [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), of de [REST-API](/rest/api/cosmos-db/) door bij te werken van de **ipRangeFilter** eigenschap. 

Om in te stellen de IP-beleid voor toegangsbeheer in Azure portal, gaat u naar de pagina voor Azure Cosmos DB-account, klikt u op **Firewall en virtuele netwerken** in het navigatiemenu, wijzig de **zodat toegang vanaf** waarde naar **geselecteerde netwerken**, en klik vervolgens op **opslaan**. 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure-portal openen](./media/firewall-support/azure-portal-firewall.png)

Zodra het IP-toegangsbeheer is ingeschakeld, wordt in de portal de mogelijkheid om op te geven van IP-adressen en -bereiken, evenals switches voor toegang tot andere Azure-services en Azure portal biedt. Meer informatie over de schakelopties is opgegeven in de volgende secties.

> [!NOTE]
> Door een beleid voor toegangsbeheer IP voor uw Azure Cosmos DB-databaseaccount is ingeschakeld, alle toegang tot uw Azure Cosmos DB-databaseaccount van computers buiten de geconfigureerde toegestane lijst met IP-adresbereiken worden geblokkeerd. Omdat dit model worden bladeren door de bewerking voor het vlak van gegevens vanuit de portal ook kunnen om te controleren of de integriteit van access control.

## <a name="connections-from-the-azure-portal"></a>Verbindingen van Azure portal 

Wanneer u een beleid voor IP-toegangsbeheer via een programma inschakelt, moet u toevoegen het IP-adres voor de Azure-portal naar de **ipRangeFilter** eigenschap om toegang te behouden. De portal IP-adressen zijn:

|Regio|IP-adres|
|------|----------|
|Alle regio's behalve die zijn opgegeven onder|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Duitsland|51.4.229.218|
|China|139.217.8.252|
|Amerikaanse overheid|52.244.48.71|

Toegang tot de Azure-portal is standaard ingeschakeld wanneer u de Firewall-instelling te wijzigen **netwerken geselecteerd** in Azure portal. 

![Schermopname die laat zien hoe u Azure portal toegang inschakelen](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-global-azure-datacenters-or-azure-paas-services"></a>Verbindingen van de globale Azure-datacenters of Azure PaaS-services
In Azure, worden PaaS-services zoals Azure Stream analytics, Azure Functions en Azure App Service gebruikt in combinatie met Azure Cosmos DB. Voor toegang tot Azure Cosmos DB-databaseaccount van deze services met IP-adressen niet beschikbaar zijn het IP-adres 0.0.0.0 toevoegen aan de lijst met toegestane IP-adressen die via een programma is gekoppeld aan uw Azure Cosmos DB-databaseaccount. 

Toegang tot de verbindingen van in internationale datacenters van Azure is standaard ingeschakeld wanneer u de Firewall-instelling te wijzigen **netwerken geselecteerd** in Azure portal. 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure-portal openen](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Verbindingen van uw huidige IP-adres

Als u wilt vereenvoudigen, kunt de Azure-portal u identificeren en de IP-adres van uw client-computer toevoegen aan de lijst met toegestane, zodat apps die de computer worden uitgevoerd toegang heeft tot de Azure Cosmos DB-account. Het hier client IP-adres wordt gedetecteerd, zoals gezien door de portal. Kan de client-IP-adres van uw computer zijn, maar dit kan ook worden de IP-adres van de gateway van uw netwerk. Vergeet niet om deze te verwijderen voordat u verdergaat met de productie.

Als uw huidige IP-adres, schakelt **mijn huidige IP toevoegen**, die uw huidige IP-adres toegevoegd aan de lijst met IP-adressen en klik vervolgens op **opslaan**.

![Schermopname die laat zien hoe u een firewall-instellingen voor huidige IP-adres configureren](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Verbindingen van cloudservices
In Azure vormen cloudservices een veelgebruikte manier voor het hosten van de middelste laag service logische met Azure Cosmos DB. Als u wilt toegang inschakelen voor een Azure Cosmos DB-databaseaccount van een service in de cloud, het openbare IP-adres van de cloudservice moet worden toegevoegd aan de lijst met toegestane IP-adressen die zijn gekoppeld aan uw Azure Cosmos DB-databaseaccount door [configureren van de IP-toegang beleid beheren](#configure-ip-policy). Dit zorgt ervoor dat alle rolinstanties van cloudservices die toegang tot uw Azure Cosmos DB-databaseaccount hebben. U kunt IP-adressen voor uw cloud-services in Azure portal, ophalen, zoals wordt weergegeven in de volgende schermafbeelding:

![Schermopname van het openbare IP-adres voor een cloudservice die wordt weergegeven in de Azure portal](./media/firewall-support/public-ip-addresses.png)

Wanneer u uw cloudservice uitbreiden door extra rol instantie (s) toe te voegen, hebben deze nieuwe exemplaren automatisch toegang tot de Azure Cosmos DB-databaseaccount omdat ze deel van dezelfde cloudservice uitmaken.

## <a name="connections-from-virtual-machines"></a>Verbindingen van virtuele machines
[Virtuele machines](https://azure.microsoft.com/services/virtual-machines/) of [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kan ook worden gebruikt voor het hosten van de middelste laag services met Azure Cosmos DB.  Als u wilt configureren van de Azure Cosmos DB-databaseaccount zodat toegang vanaf virtuele machines, openbare IP-adressen van virtuele machine en/of de virtuele machine moet schaalset worden geconfigureerd als een van de toegestane IP-adressen voor uw Azure Cosmos DB-databaseaccount door [configureren van het beleid voor IP-toegangsbeheer](#configure-ip-policy). U kunt IP-adressen voor virtuele machines in Azure portal kunt ophalen, zoals wordt weergegeven in de volgende schermafbeelding.

![Schermafbeelding met een openbaar IP-adres voor een virtuele machine weergegeven in de Azure portal](./media/firewall-support/public-ip-addresses-dns.png)

Wanneer u aanvullende VM-exemplaren aan de groep toevoegt, worden ze automatisch toegang tot uw Azure Cosmos DB-databaseaccount opgegeven.

## <a name="connections-from-the-internet"></a>Verbindingen via internet
Wanneer u een Azure Cosmos DB-databaseaccount vanuit een computer op het internet opent, moet de client-IP-adres of IP-adresbereik van de machine worden toegevoegd aan de lijst met toegestane IP-adres voor het account van de Azure Cosmos DB-database. 

## <a name="using-azure-resource-manager-template-to-set-up-the-ip-access-control"></a>Met behulp van Azure Resource Manager-sjabloon voor het instellen van de IP-toegangsbeheer

De volgende JSON toevoegen aan uw sjabloon voor het instellen van IP-toegangsbeheer. Resource Manager-sjabloon voor een account heeft ipRangeFilter kenmerk lijst met IP-adresbereiken, zodat de whitelist moeten worden.

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
     "databaseAccountOfferType": "Standard",
     "name": "[parameters('databaseAccountName')]",
     "ipRangeFilter":"10.0.0.1,10.0.0.2,183.240.196.255"
   }
   }
```

## <a name="troubleshooting-the-ip-access-control-policy"></a>Het oplossen van het beleid voor IP-toegangsbeheer
### <a name="portal-operations"></a>Portal-bewerkingen
Door een beleid voor toegangsbeheer IP voor uw Azure Cosmos DB-databaseaccount is ingeschakeld, alle toegang tot uw Azure Cosmos DB-databaseaccount van computers buiten de geconfigureerde toegestane lijst met IP-adresbereiken worden geblokkeerd. Dus als u inschakelen portal gegevens vlak bewerkingen wilt, zoals surfen containers en query's voor documenten, moet u expliciet toestaan met behulp van Azure portal toegang tot de **Firewall** pagina in de portal. 

### <a name="sdk--rest-api"></a>SDK en Rest-API
Voor toegang via SDK of REST-API van de computers niet in de lijst met toegestane uit veiligheidsoverwegingen wordt een antwoord algemene 404 niet gevonden met geen aanvullende informatie. Controleer of het IP-adres toegestaan lijst die is geconfigureerd voor het account van uw Azure Cosmos DB-database om te controleren of de dat configuratie van het juiste beleid wordt toegepast op uw Azure Cosmos DB-databaseaccount.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tips voor betere prestaties netwerkgerelateerde [tips voor betere prestaties](performance-tips.md).

