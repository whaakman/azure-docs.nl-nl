---
title: Procedures voor IP-firewall voor uw Azure Cosmos-account configureren
description: Informatie over het configureren van IP-beleid voor toegangsbeheer voor firewall-ondersteuning op Azure Cosmos DB-database-accounts.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: e1253d4a43e45fc7df7efbb6d5f9dd4176957ca2
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629005"
---
# <a name="how-to-configure-ip-firewall-for-your-azure-cosmos-account"></a>Procedures voor IP-firewall voor uw Azure Cosmos-account configureren

U kunt de gegevens die zijn opgeslagen in uw Azure Cosmos-account met behulp van IP-firewalls beveiligen. Azure Cosmos DB biedt ondersteuning voor IP-gebaseerd toegangsbeheer voor firewallregels voor binnenkomend ondersteuning. U kunt een IP-firewall op de Azure Cosmos-account instellen met behulp van een van de volgende manieren:

1. Vanuit Azure portal
2. Declaratief met behulp van een Azure Resource Manager-sjabloon
3. Via een programma via Azure CLI of Azure Powershell door het bijwerken van de **ipRangeFilter** eigenschap.

## <a id="configure-ip-policy"></a> IP-firewall met behulp van Azure portal configureren

Om in te stellen de IP-beleid voor toegangsbeheer in Azure portal, gaat u naar de pagina voor Azure Cosmos DB-account, klikt u op **Firewall en virtuele netwerken** in het navigatiemenu, wijzig de **zodat toegang vanaf** waarde naar **geselecteerde netwerken**, en klik vervolgens op **opslaan**. 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure-portal openen](./media/how-to-configure-firewall/azure-portal-firewall.png)

Zodra het IP-toegangsbeheer is ingeschakeld, biedt Azure portal de mogelijkheid om op te geven van IP-adressen, IP-adresbereiken, evenals de schakelopties voor toegang tot andere Azure-services en Azure portal. In de volgende secties vindt u meer informatie over deze switches.

> [!NOTE]
> Nadat u beleid voor IP-toegangsbeheer voor uw Azure Cosmos-account hebt ingeschakeld, worden alle aanvragen naar uw Azure Cosmos-account van computers buiten de toegestane lijst met IP-adresbereiken worden geweigerd. Bladeren door de Azure Cosmosdb-resources vanaf de portal worden ook worden geblokkeerd om te controleren of de integriteit van access control.

### <a name="allow-requests-from-the-azure-portal"></a>Aanvragen van de Azure-portal toestaan 

Wanneer u een beleid voor IP-toegangsbeheer via een programma inschakelt, moet u toevoegen het IP-adres voor de Azure-portal naar de **ipRangeFilter** eigenschap om toegang te behouden. De portal IP-adressen zijn:

|Regio|IP-adres|
|------|----------|
|Duitsland|51.4.229.218|
|China|139.217.8.252|
|Amerikaanse overheid|52.244.48.71|
|Alle regio's behalve de bovenstaande drie|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

U kunt toegang tot de Azure-portal inschakelen door het kiezen van de **zodat toegang vanaf Azure portal** optie zoals wordt weergegeven in de volgende schermafbeelding: 

![Schermopname die laat zien hoe u Azure portal toegang inschakelen](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Aanvragen van de globale Azure-datacenters of andere bronnen in Azure toestaan

Als u toegang tot Azure Cosmos-account van de services die niet een statisch IP-adres, bijvoorbeeld Azure Stream analytics bieden, Azure Functions enz. u kunt nog steeds de IP-firewall gebruikt om toegang te beperken. Als u wilt toegang tot Azure Cosmos-account toestaan van dergelijke services, moet een firewallinstelling zijn ingeschakeld. Als u wilt deze firewallinstelling inschakelt, toevoegen het IP-adres-0.0.0.0 aan de lijst met toegestane IP-adressen. 0.0.0.0 beperkt aanvragen tot uw Azure Cosmos-account van Azure datacenter IP-adresbereik. Deze instelling is niet toegestaan voor toegang voor alle andere IP-adresbereiken op uw Azure Cosmos-account.

> [!NOTE]
> Deze optie configureert u de firewall voor alle aanvragen van Azure, inclusief aanvragen van de abonnementen van andere klanten geïmplementeerd in Azure. De lijst met IP-adressen toegestaan door deze optie is breed, zodat deze wordt beperkt door de effectiviteit van firewall-beleid. Deze optie moet alleen worden gebruikt als de aanvragen niet afkomstig van statische IP-adressen of subnetten in vnet's zijn. Als u deze optie kiest, wordt automatisch kan toegang vanuit de Azure-portal, omdat de Azure-portal wordt geïmplementeerd in Azure.

U kunt toegang tot de Azure-portal inschakelen door het kiezen van de **verbindingen aanvaarden van openbare Azure-datacenters** optie zoals wordt weergegeven in de volgende schermafbeelding: 

![Schermopname die laat zien hoe u de Firewall-pagina in de Azure-portal openen](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Aanvragen van uw huidige IP-adres

Als u wilt vereenvoudigen, kunt de Azure-portal u identificeren en de IP-adres van uw client-computer toevoegen aan de lijst met toegestane, zodat apps die de computer worden uitgevoerd toegang heeft tot uw Azure Cosmos-account. Het IP-adres van de client wordt automatisch gedetecteerd door de portal. De client-IP-adres van uw computer of het IP-adres van de gateway van uw netwerk kan het zijn. Zorg ervoor dat u dit IP-adres verwijderen voordat u uw workloads voor productie. 

Als uw huidige IP-adres, schakelt **mijn huidige IP toevoegen**, die uw huidige IP-adres toegevoegd aan de lijst met IP-adressen en klik vervolgens op **opslaan**.

![Schermopname die laat zien hoe u een firewall-instellingen voor huidige IP-adres configureren](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Aanvragen van cloudservices

In Azure vormen cloudservices een veelgebruikte manier voor het hosten van de middelste laag service logische met Azure Cosmos DB. Als u wilt toegang inschakelen voor uw Azure Cosmos-account van een service in de cloud, het openbare IP-adres van de cloudservice moet worden toegevoegd aan de lijst met toegestane IP-adressen die zijn gekoppeld aan uw Azure Cosmos-account door [het IP-toegangscontrolebeleidconfigureren](#configure-ip-policy). Dit zorgt ervoor dat alle rolinstanties van cloudservices die toegang tot uw Azure Cosmos-account hebben. U kunt IP-adressen voor uw cloud-services in Azure portal, ophalen, zoals wordt weergegeven in de volgende schermafbeelding:

![Schermopname van het openbare IP-adres voor een cloudservice die wordt weergegeven in de Azure portal](./media/how-to-configure-firewall/public-ip-addresses.png)

Wanneer u uw cloudservice uitbreiden door extra rol instantie (s) toe te voegen, hebben deze nieuwe exemplaren automatisch toegang tot het Azure Cosmos-account omdat ze deel van dezelfde cloudservice uitmaken.

### <a name="requests-from-virtual-machines"></a>Aanvragen van virtuele machines

[Virtuele machines](https://azure.microsoft.com/services/virtual-machines/) of [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kan ook worden gebruikt voor het hosten van de middelste laag services met Azure Cosmos DB. Voor het configureren van uw Cosmos-account voor toegang van virtuele machines, openbare IP-adressen van virtuele machine en/of virtuele-machineschaalset moeten worden geconfigureerd als een van de toegestane IP-adressen voor uw Azure Cosmos-account door [het IP-adres configureren toegangsbeheerbeleid](#configure-ip-policy). U kunt IP-adressen voor virtuele machines in Azure portal kunt ophalen, zoals wordt weergegeven in de volgende schermafbeelding:

![Schermafbeelding met een openbaar IP-adres voor een virtuele machine weergegeven in de Azure portal](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Wanneer u aanvullende VM-exemplaren aan de groep toevoegt, worden ze automatisch toegang tot uw Azure Cosmos-account opgegeven.

### <a name="requests-from-the-internet"></a>Aanvragen van internet

Wanneer u toegang uw Azure Cosmos-account van een computer op het internet tot, moet de client-IP-adres of IP-adresbereik van de machine worden toegevoegd aan de lijst met toegestane IP-adres voor uw account.

## <a id="configure-ip-firewall-arm"></a>IP-firewall met behulp van Resource Manager-sjabloon configureren

Voor het configureren van beheer van toegang tot uw Azure Cosmos-account, het Resource Manager-sjabloon moet opgeven de **ipRangeFilter** kenmerk met een lijst met IP-adresbereiken, zodat de whitelist moeten worden. Bijvoorbeeld, de volgende JSON-code toevoegen aan uw sjabloon:

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

## <a id="configure-ip-firewall-cli"></a>Configureren van IP-beleid voor toegangsbeheer met behulp van Azure CLI

De volgende opdracht laat zien hoe een Azure Cosmos-account maken met IP-toegangsbeheer: 

```azurecli-interactive
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

## <a id="troubleshoot-ip-firewall"></a>IP-access control-beleid oplossen

U kunt IP-access control beleidsproblemen kunt oplossen met behulp van de volgende opties: 

### <a name="azure-portal"></a>Azure Portal 
Door een beleid voor toegangsbeheer IP voor uw Azure Cosmos-account is ingeschakeld, worden alle aanvragen naar uw account van computers buiten de toegestane lijst met IP-adresbereiken worden geblokkeerd. Dus als u inschakelen portal gegevens vlak bewerkingen wilt, zoals surfen containers en query's voor documenten, moet u expliciet toestaan met behulp van Azure portal toegang tot de **Firewall** deelvenster in de portal.

### <a name="sdks"></a>SDK's 
Wanneer u toegang Azure Cosmos-resources tot met behulp van SDK's van de computers die zich niet in de lijst met toegestane **een algemene 404 niet gevonden-antwoord wordt geretourneerd zonder extra details**. Controleer of de lijst met toegestane IP-voor uw account en zorg ervoor dat de configuratie van het juiste beleid wordt toegepast op uw Cosmos-account. 

### <a name="check-source-ips-in-blocked-requests"></a>Bron-IP's in geblokkeerde aanvragen controleren
Schakel Diagnostische logboekregistratie voor uw Azure Cosmos-account, deze logboeken toont elke aanvraag en reactie. **De berichten met betrekking tot firewall intern worden geregistreerd met een 403 retourcode**. Door deze berichten filteren, kunt u de bron-IP-adressen voor de geblokkeerde aanvragen bekijken. Zie [Diagnostische logboekregistratie van Azure Cosmos DB](logging.md).

### <a name="requests-from-subnet-with-service-endpoint-for-azure-cosmos-database-enabled"></a>Aanvragen van subnet met de service-eindpunt voor Azure Cosmos-database is ingeschakeld
Aanvragen van een subnet in VNET waarvoor service-eindpunt voor Azure Cosmos DB ingeschakeld verzendt VNET en subnet-id voor Azure Cosmos-accounts. Deze aanvragen nog geen van de openbare IP-adres van de bron, zodat ze door IP-filters worden geweigerd. Als u wilt toestaan dat toegang vanaf specifieke subnetten in vnet's, toevoegen toegangsbeheerlijst VNET die worden beschreven in [virtueel netwerk- en toegangsbeheer op basis van een subnet voor uw Azure Cosmos-account configureren](how-to-configure-vnet-service-endpoint.md). Het kan tot 15 minuten voor firewall-regels om toe te passen duren.


## <a name="next-steps"></a>Volgende stappen

Voor het configureren van service-eindpunt voor virtueel netwerk voor uw Azure Cosmos DB-account, Zie de volgende artikelen:

* [VNET en subnet toegangsbeheer voor uw Azure Cosmos-account](vnet-service-endpoint.md)
* [Virtueel netwerk en subnet configureren op basis van toegang voor uw Azure Cosmos-account](how-to-configure-vnet-service-endpoint.md)


