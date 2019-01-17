---
title: Een IP-firewall configureren voor uw Azure Cosmos DB-account
description: Informatie over het configureren van IP-beleid voor toegangsbeheer voor firewall-ondersteuning op Azure Cosmos DB-database-accounts.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: d209e1f6924e5c7d6bba7512606504b7165f0ed3
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359422"
---
# <a name="configure-an-ip-firewall-for-your-azure-cosmos-db-account"></a>Een IP-firewall configureren voor uw Azure Cosmos DB-account

U kunt de gegevens die zijn opgeslagen in uw Azure Cosmos DB-account met behulp van IP-firewalls beveiligen. Azure Cosmos DB biedt ondersteuning voor IP-gebaseerd toegangsbeheer voor firewallregels voor binnenkomend ondersteuning. U kunt een IP-firewall op de Azure Cosmos DB-account instellen met behulp van een van de volgende manieren:

* Vanuit Azure portal
* Declaratief met behulp van een Azure Resource Manager-sjabloon
* Via een programma via de Azure CLI of Azure PowerShell door het bijwerken van de **ipRangeFilter** eigenschap

## <a id="configure-ip-policy"></a> Een IP-firewall configureren met behulp van Azure portal

Om in te stellen de IP-beleid voor toegangsbeheer in Azure portal, gaat u naar de pagina voor Azure Cosmos DB-account en selecteer **Firewall en virtuele netwerken** in het navigatiemenu. Wijzig de **zodat toegang vanaf** waarde die moet worden **geselecteerde netwerken**, en selecteer vervolgens **opslaan**. 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure-portal openen](./media/how-to-configure-firewall/azure-portal-firewall.png)

Wanneer IP-toegangsbeheer is ingeschakeld, biedt de Azure-portal de mogelijkheid om op te geven van IP-adressen, IP-adresbereiken en switches. Switches inschakelen toegang tot andere Azure-services en Azure portal. De volgende secties geeft meer informatie over deze switches.

> [!NOTE]
> Nadat u een beleid voor IP-toegangsbeheer voor uw Azure Cosmos DB-account hebt ingeschakeld, worden alle aanvragen naar uw Azure Cosmos DB-account van computers buiten de toegestane lijst met IP-adresbereiken worden geweigerd. Bladeren door de Azure Cosmos DB-resources vanaf de portal is ook om te controleren of de integriteit van access control geblokkeerd.

### <a name="allow-requests-from-the-azure-portal"></a>Aanvragen van de Azure-portal toestaan 

Wanneer u een beleid voor IP-toegangsbeheer via een programma inschakelt, moet u toevoegen het IP-adres voor de Azure-portal naar de **ipRangeFilter** eigenschap om toegang te behouden. De portal IP-adressen zijn:

|Regio|IP-adres|
|------|----------|
|Duitsland|51.4.229.218|
|China|139.217.8.252|
|Amerikaanse overheid|52.244.48.71|
|Alle andere regio 's|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

U kunt toegang tot de Azure-portal inschakelen door het selecteren van de **zodat toegang vanaf Azure portal** optie, zoals wordt weergegeven in de volgende schermafbeelding: 

![Schermopname die laat zien hoe u Azure portal toegang inschakelen](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Aanvragen van de globale Azure-datacenters of andere bronnen in Azure toestaan

Als u toegang uw Azure Cosmos DB-account van de services die niet een statisch IP-adres (bijvoorbeeld Azure Stream Analytics en Azure Functions) bieden tot, kunt u de IP-firewall nog steeds gebruiken om toegang te beperken. Toevoegen als u wilt toegang tot de Azure Cosmos DB-account toestaan van dergelijke services, het IP-adres 0.0.0.0 aan de lijst met toegestane IP-adressen. De 0.0.0.0 adres aanvragen beperkt tot uw Azure Cosmos DB-account van Azure datacenter IP-adresbereik. Deze instelling is niet toegestaan voor toegang voor alle andere IP-adresbereiken op uw Azure Cosmos DB-account.

> [!NOTE]
> Deze optie configureert u de firewall voor alle aanvragen van Azure, met inbegrip van aanvragen van de abonnementen van andere klanten geïmplementeerd in Azure. De lijst met IP-adressen toegestaan door deze optie is breed, zodat deze wordt beperkt door de effectiviteit van een firewall-beleid. Gebruik deze optie alleen als de aanvragen niet afkomstig van statische IP-adressen of subnetten in virtuele netwerken zijn. Als u deze optie kiest, wordt automatisch kan toegang vanuit de Azure-portal, omdat de Azure-portal is geïmplementeerd in Azure.

U kunt toegang tot de Azure-portal inschakelen door het selecteren van de **verbindingen aanvaarden van in de openbare Azure-datacenters** optie, zoals wordt weergegeven in de volgende schermafbeelding: 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure-portal openen](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Aanvragen van uw huidige IP-adres

Als u wilt vereenvoudigen, kunt de Azure-portal u identificeren en de IP-adres van uw client-computer toevoegen aan de lijst met toegestane. Apps die de computer worden uitgevoerd, hebben vervolgens toegang tot uw Azure Cosmos DB-account. 

De portal detecteert automatisch IP-adres van de client. Het is mogelijk het client-IP-adres van uw computer of het IP-adres van de gateway van uw netwerk. Zorg ervoor dat u dit IP-adres verwijderen voordat u uw workloads voor productie. 

Selecteer om uw huidige IP-adres toe aan de lijst met IP-adressen **mijn huidige IP toevoegen**. Selecteer vervolgens **Opslaan**.

![Schermopname die laat zien hoe u een firewall-instellingen voor het huidige IP-adres configureren](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Aanvragen van cloudservices

In Azure vormen cloudservices een veelgebruikte manier voor het hosten van logica voor de middelste laag service met behulp van Azure Cosmos DB. Als u wilt toegang inschakelen voor uw Azure Cosmos DB-account van een cloudservice, moet u het openbare IP-adres van de service in de cloud toevoegen aan de lijst met toegestane IP-adressen die zijn gekoppeld aan uw Azure Cosmos DB-account door [het IP-toegangscontrolebeleidconfigureren](#configure-ip-policy). Dit zorgt ervoor dat alle rolinstanties van cloudservices die toegang tot uw Azure Cosmos DB-account hebben. 

U kunt IP-adressen voor uw cloud-services in Azure portal, ophalen, zoals wordt weergegeven in de volgende schermafbeelding:

![Schermopname van het openbare IP-adres voor een cloudservice die wordt weergegeven in de Azure portal](./media/how-to-configure-firewall/public-ip-addresses.png)

Wanneer u van uw cloudservice schalen door rolinstanties toe te voegen, hebben deze nieuwe exemplaren automatisch toegang tot het Azure Cosmos DB-account omdat ze deel uitmaken van dezelfde cloudservice.

### <a name="requests-from-virtual-machines"></a>Aanvragen van virtuele machines

U kunt ook [virtuele machines](https://azure.microsoft.com/services/virtual-machines/) of [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) op de middelste laag hosten van services met behulp van Azure Cosmos DB. Als u wilt uw Cosmos DB-account voor toegang van virtuele machines configureren, moet u het openbare IP-adres van de virtuele machine en/of de virtuele-machineschaalset als een van de toegestane IP-adressen voor uw Azure Cosmos DB-account door [ configureren van het beleid voor IP-toegangsbeheer](#configure-ip-policy). 

U kunt IP-adressen voor virtuele machines in Azure portal kunt ophalen, zoals wordt weergegeven in de volgende schermafbeelding:

![Schermafbeelding met een openbaar IP-adres voor een virtuele machine weergegeven in de Azure portal](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Wanneer u exemplaren van virtuele machines aan de groep toevoegt, krijgen ze automatisch toegang tot uw Azure Cosmos DB-account.

### <a name="requests-from-the-internet"></a>Aanvragen van internet

Wanneer u toegang uw Azure Cosmos DB-account van een computer op het internet tot, moet de client-IP-adres of IP-adresbereik van de machine worden toegevoegd aan de lijst met toegestane IP-adressen voor uw account.

## <a id="configure-ip-firewall-arm"></a>Een IP-firewall configureren met behulp van Resource Manager-sjabloon

Voor het configureren van beheer van toegang tot uw Azure Cosmos DB-account, zorg ervoor dat de Resource Manager-sjabloon bevat de **ipRangeFilter** kenmerk met een lijst met toegestane IP-adresbereiken. Bijvoorbeeld, de volgende JSON-code toevoegen aan uw sjabloon:

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
     "ipRangeFilter":"183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
   }
```

## <a id="configure-ip-firewall-cli"></a>Een beleid voor IP-toegangsbeheer configureren met behulp van de Azure CLI

De volgende opdracht laat zien hoe een Azure Cosmos DB-account maken via IP-toegangsbeheer: 

```azurecli-interactive

name="<Azure Cosmos DB account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

Voor het bijwerken van de firewall-instellingen voor een bestaand account, moet u de volgende opdracht uitvoeren:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="troubleshoot-ip-firewall"></a>Problemen oplossen met een beleid voor IP-toegangsbeheer

U kunt problemen met een beleid voor IP-toegangsbeheer oplossen met behulp van de volgende opties: 

### <a name="azure-portal"></a>Azure Portal 
Door een beleid voor toegangsbeheer IP voor uw Azure Cosmos DB-account is ingeschakeld, blokkeren u alle aanvragen naar uw account van computers buiten de toegestane lijst met IP-adresbereiken. Om in te schakelen portal data plane-bewerkingen, zoals bladeren, containers en opvragen van documenten, moet u expliciet toestaan toegang tot Azure portal met behulp van de **Firewall** deelvenster in de portal.

### <a name="sdks"></a>SDK's 
Wanneer u Azure Cosmos DB-resources opent met behulp van SDK's van de computers die zich niet in de lijst met toegestane, een generieke **403-verboden** antwoord wordt geretourneerd met geen aanvullende informatie. Controleer of de lijst met toegestane IP-voor uw account en zorg ervoor dat de configuratie van het juiste beleid wordt toegepast op uw Azure Cosmos DB-account. 

### <a name="source-ips-in-blocked-requests"></a>Bron-IP's in geblokkeerde aanvragen
Diagnostische logboekregistratie inschakelen op uw Azure Cosmos DB-account. Deze logboeken weergegeven voor elke aanvraag en reactie. De firewall-berichten worden geregistreerd met een 403 retourcode. Door deze berichten filteren, kunt u de bron-IP-adressen voor de geblokkeerde aanvragen bekijken. Zie [Diagnostische logboekregistratie van Azure Cosmos DB](logging.md).

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Aanvragen van een subnet met een service-eindpunt voor Azure Cosmos DB ingeschakeld
Aanvragen van een subnet in een virtueel netwerk met een service-eindpunt voor Azure Cosmos DB is ingeschakeld, verzendt de identiteit van de virtuele netwerk en subnet met Azure Cosmos DB-accounts. Deze aanvragen beschikt niet over de openbare IP-adres van de bron, zodat het IP-filters weigeren. Zodat toegang vanaf specifieke subnetten in virtuele netwerken toevoegen een access control list, zoals wordt beschreven in [virtueel netwerk- en toegangsbeheer op basis van een subnet voor uw Azure Cosmos DB-account configureren](how-to-configure-vnet-service-endpoint.md). Het kan tot 15 minuten voor firewall-regels om toe te passen duren.


## <a name="next-steps"></a>Volgende stappen

Voor het configureren van een service-eindpunt voor virtueel netwerk voor uw Azure Cosmos DB-account, Zie de volgende artikelen:

* [Virtueel netwerk en subnet toegangsbeheer voor uw Azure Cosmos DB-account](vnet-service-endpoint.md)
* [Virtueel netwerk- en toegangsbeheer op basis van een subnet voor uw Azure Cosmos DB-account configureren](how-to-configure-vnet-service-endpoint.md)


