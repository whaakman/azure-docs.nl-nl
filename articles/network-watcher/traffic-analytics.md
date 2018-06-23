---
title: Azure traffic analytics | Microsoft Docs
description: Informatie over het analyseren van Azure-netwerk groep stroom beveiligingslogboeken met traffic analytics.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2018
ms.author: yagup;jdial
ms.openlocfilehash: ad26772650cf052926a2534d343f64765f47b78f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333391"
---
# <a name="traffic-analytics"></a>Verkeersanalyse

Verkeer analytics is een cloud-gebaseerde oplossing die inzicht in gebruikers- en activiteit in de cloud-netwerken biedt. Verkeer analytics analyseert netwerk-Watcher netwerk groep (NSG) stroom beveiligingslogboeken om op te geven inzicht in het netwerkverkeer in uw Azure-cloud. Met traffic analytics, kunt u het volgende doen:

- Netwerkactiviteit visualiseren via uw Azure-abonnementen en hotspots identificeren.
- Beveiligingsrisico's te identificeren en beveiligen van uw netwerk, met informatie zoals open-poorten, toepassingen die proberen toegang tot internet en virtuele machines (VM) netwerken rogue verbinding maakt.
- Begrijpen stroom verkeerspatronen over Azure-regio's en het internet om de netwerkimplementatie van uw voor de prestaties en capaciteit te optimaliseren.
- Speldenpunt netwerk door middel van configuratiefouten leidt tot mislukte verbindingen in uw netwerk.

## <a name="why-traffic-analytics"></a>Waarom traffic analytics?

Het is essentieel om te controleren, beheren en weten van uw eigen netwerk voor ongeschonden beveiliging, naleving en prestaties. Uw eigen omgeving weten is van groot belang om te beveiligen en te optimaliseren. Vaak wilt u weten van de huidige status van het netwerk die verbinding maakt, waar ze verbinding maakt vanaf, welke poorten zijn geopend met het internet, verwachte netwerkverkeer, onregelmatige netwerk gedrag, en plotselinge stijgingen in het verkeer.

Cloud-netwerken zijn anders dan enterprise voor on-premises netwerken, waarin u netflow of gelijkwaardige protocol kunnen routers en switches, die de mogelijkheid bieden voor het verzamelen van IP-netwerkverkeer, zoals het binnengaat of een netwerkinterface verlaat hebben. Door het verkeer stroomgegevens analyseren, kunt u een analyse van netwerkverkeer en het volume.

Virtuele netwerken van Azure hebben NSG stroom Logboeken, waarin u informatie over binnenkomende en uitgaande IP-verkeer via een netwerk-beveiligingsgroep die is gekoppeld aan afzonderlijke netwerkinterfaces, virtuele machines of subnetten. Door onbewerkte stroom NSG-logboeken te analyseren en invoegen intelligence van beveiliging, de topologie en Geografie verkeer kunt analytics u voorzien van inzicht in het netwerkverkeer in uw omgeving. Verkeer Analytics biedt informatie zoals meest communicerende hosts, meest communicerende toepassingsprotocollen, meest conversatie host paren, verkeer toegestaan/geblokkeerd, binnenkomend en uitgaand verkeer, open internet-poorten, meest blokkerende regels, verkeer distributiepunten per Azure-datacenter, virtueel netwerk, subnetten, of rogue netwerken.

## <a name="key-components"></a>Belangrijkste onderdelen

- **Netwerkbeveiligingsgroep (NSG)**: bevat een lijst met regels voor toestaan of weigeren van netwerkverkeer naar resources die zijn verbonden met een virtueel netwerk van Azure. NSG's kunnen worden gekoppeld aan subnetten, afzonderlijke virtuele machines (klassiek) of afzonderlijke netwerkinterfaces (NIC) die zijn gekoppeld aan VM’s (Resource Manager). Zie voor meer informatie [netwerk groep beveiligingsoverzicht](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Netwerk beveiligingslogboeken groep (NSG) stroom**: kunt u informatie bekijken over inkomende en uitgaande IP-verkeer via een netwerkbeveiligingsgroep. NSG-stroom logboeken zijn geschreven in json-indeling en weergeven van binnenkomende en uitgaande stromen op basis van per regel dat de NIC de stroom is van toepassing op, 5-tuple informatie over de stroom (bron/het doel-IP-adres, bron/het doel-poort en protocol) en als het verkeer is toegestaan of geweigerd. Zie voor meer informatie over het NSG stroom logboeken [NSG stroom logboeken](network-watcher-nsg-flow-logging-overview.md).
- **Meld u Analytics**: een Azure-service die u verzamelt bewakingsgegevens en slaat de gegevens in een centrale opslagplaats. Deze gegevens kunnen gebeurtenissen, prestatiegegevens of aangepaste worden geleverd via de Azure-API bevatten. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export. Bewaking van toepassingen, zoals netwerk prestaties bewaken en traffic analytics zijn gebouwd met behulp van logboekanalyse als basis. Zie voor meer informatie [Meld analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Meld u werkruimte**: een exemplaar van logboekanalyse, waar de gegevens die betrekking hebben op een Azure-account is opgeslagen. Zie voor meer informatie over log analytics-werkruimten [maken van een werkruimte voor logboekanalyse](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Netwerk-Watcher**: een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op niveau van een scenario in Azure. U kunt NSG stroom Logboeken in- en uitschakelen met de netwerk-Watcher inschakelen. Zie voor meer informatie [netwerk-Watcher](network-watcher-monitoring-overview.md).

## <a name="how-traffic-analytics-works"></a>De werking van traffic analytics

Verkeer analytics onderzoekt het NSG-logboeken voor onbewerkte stroom en verminderde Logboeken door het samenvoegen van algemene stromen tussen de dezelfde bron-IP-adres, IP-doeladres, doelpoort en -protocol wordt vastgelegd. Bijvoorbeeld:-Host 1 (IP-adres: 10.10.10.10) naar Host 2 communicatie (IP-adres: 10.10.20.10), 100 keer gedurende een periode van 1 uur met behulp van poort (bijvoorbeeld: 80) en het protocol (bijvoorbeeld http). De verminderde logboek heeft een vermelding, die Host 1 & Host 2 uitgewisseld 100 keer gedurende een periode van 1 uur met behulp van poort *80* en protocol *HTTP*, in plaats van 100 vermeldingen. Verminderde logboeken zijn uitgebreid met Geografie, beveiliging en gegevens van de netwerktopologie en vervolgens wordt opgeslagen in een log analytics-werkruimte. De volgende afbeelding ziet de gegevensstroom:

![De gegevensstroom voor NSG stroom logboeken verwerken](./media/traffic-analytics/data-flow-for-nsg-flow-log-processing.png)

## <a name="supported-regions"></a>Ondersteunde regio’s

U kunt traffic analytics gebruiken voor het nsg's in een van de volgende gebieden: West-Centraal VS, VS-Oost, VS-Oost 2, Noordelijk Centraal, VS, Zuid-centraal VS, VS-midden, VS-West, VS-West-2, West-Europa, Noord-Europa, West VK, Zuid VK, Australië-Oost, Australië-Zuidoost, en Zuidoost-Azië. Log analytics-werkruimte moet bestaan in de West-Centraal VS, VS-Oost, West-Europa, Zuid VK, Australië-Zuidoost of de regio Zuidoost-Azië.

## <a name="prerequisites"></a>Vereisten

### <a name="user-access-requirements"></a>Vereisten voor toegang voor gebruiker

Uw account moet lid zijn van een van de volgende Azure [ingebouwde rollen](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json):

|Implementatiemodel   | Functie                   |
|---------          |---------               |
|Resource Manager   | Eigenaar                  |
|                   | Inzender            |
|                   | Lezer                 |
|                   | Inzender voor netwerken    |
|Klassiek            | Accountbeheerder  |
|                   | Servicebeheerder  |
|                   | Co-beheerder       |

Als uw account niet aan een van de ingebouwde rollen toegewezen is, moet deze worden toegewezen aan een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) die de volgende acties uit, op het abonnementsniveau is toegewezen:

- 'Microsoft.Network/applicationGateways/read'
- 'Microsoft.Network/connections/read'
- 'Microsoft.Network/loadBalancers/read'
- 'Microsoft.Network/localNetworkGateways/read'
- 'Microsoft.Network/networkInterfaces/read'
- 'Microsoft.Network/networkSecurityGroups/read'
- 'Microsoft.Network/publicIPAddresses/read'
- 'Microsoft.Network/routeTables/read'
- 'Microsoft.Network/virtualNetworkGateways/read'
- 'Microsoft.Network/virtualNetworks/read'

Zie voor meer informatie over het controleren van machtigingen voor gebruikerstoegang [Traffic analytics Veelgestelde vragen over](traffic-analytics-faq.md).

### <a name="enable-network-watcher"></a>Network Watcher inschakelen

Voor het analyseren van verkeer, moet u beschikken over een bestaande netwerk-watcher of [inschakelen van een netwerk-watcher](network-watcher-create.md) verkeer voor elke regio waarin u nsg's die u wilt analyseren hebt. Analytics voor verkeer kan worden ingeschakeld voor het nsg's die worden gehost in een van de [ondersteunde regio's](#supported-regions).

### <a name="re-register-the-network-resource-provider"></a>De netwerk-resourceprovider opnieuw geregistreerd

Voordat u verkeer analytics gebruiken kunt, moet u uw netwerkresourceprovider opnieuw registreren. Klik op **probeert het** in de volgende code in de Azure-Cloud-Shell openen. De Cloud-Shell registreert u in automatisch in uw Azure-abonnement. Nadat de Cloud-Shell geopend is, voer de volgende opdracht om de netwerk-resourceprovider opnieuw geregistreerd:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Selecteer een netwerkbeveiligingsgroep

Voordat u NSG stroom logboekregistratie inschakelt, moet u een netwerkbeveiligingsgroep aan te melden stromen voor hebben. Als u een netwerkbeveiligingsgroep geen hebt, raadpleegt u [maken van een netwerkbeveiligingsgroep](../virtual-network/manage-network-security-group.md#create-a-network-security-group) een maken.

Aan de linkerkant van de Azure portal, selecteer **Monitor**, klikt u vervolgens **netwerk-watcher**, en selecteer vervolgens **NSG stroom logboeken**. Selecteer de netwerkbeveiligingsgroep die u inschakelen, een NSG stroom logboek, wilt zoals wordt weergegeven in de volgende afbeelding:

![Selectie van het nsg's waarvoor het inschakelen van het NSG stroom logboek](./media/traffic-analytics/selection-of-nsgs-that-require-enablement-of-nsg-flow-logging.png)

Als u probeert te traffic analytics inschakelen voor een NSG die wordt gehost in elke regio dan het [ondersteunde regio's](#supported-regions), foutbericht 'Niet gevonden'.

## <a name="enable-flow-log-settings"></a>Logboekinstellingen stroom inschakelen

Voordat u de stroom logboekinstellingen inschakelt, moet u de volgende taken uitvoeren:

Registreer de provider Azure inzicht als deze nog niet geregistreerd voor uw abonnement:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

Als u nog geen hebt een Azure Storage-account voor het opslaan van NSG-stroom zich aanmeldt, moet u een opslagaccount maken. U kunt een opslagaccount maken met de volgende opdracht. Voordat u de opdracht uitvoert, vervangt u `<replace-with-your-unique-storage-account-name>` met een naam die is uniek voor alle Azure locaties tussen 3 tot 24 tekens lang zijn, met behulp van alleen cijfers en kleine letters. U kunt ook de naam van de resourcegroep, indien nodig wijzigen.

```azurepowershell-interactive
New-AzureRmStorageAccount `
  -Location westcentralus `
  -Name <replace-with-your-unique-storage-account-name> `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Selecteer de volgende opties, zoals wordt weergegeven in de afbeelding:

1. Selecteer *op* voor **Status**
2. Selecteer een bestaand opslagaccount voor het opslaan van de stroom-Logboeken in. Als u opslaan van de gegevens permanent wilt, stel de waarde op *0*. U Azure Storage-kosten voor het opslagaccount hebt gemaakt.
3. Stel **bewaren** aan het aantal dagen dat u gegevens wilt opslaan voor.
4. Selecteer *op* voor **Traffic Analytics Status**.
5. Selecteer een bestaande werkruimte voor logboekanalyse (OMS) of selecteer **nieuwe werkruimte maken** naar een nieuwe maken. Een werkruimte voor logboekanalyse wordt gebruikt door Traffic Analytics voor het opslaan van de geaggregeerde en geïndexeerde gegevens die vervolgens wordt gebruikt voor het genereren van de analyses. Als u een bestaande werkruimte selecteert, moet bestaan in een van de [ondersteunde regio's](#traffic-analytics-supported-regions) en bijgewerkt naar de nieuwe querytaal. Als u niet wilt bijwerken van een bestaande werkruimte of geen een werkruimte in een ondersteunde regio, kunt u een nieuwe maken. Zie voor meer informatie over querytalen [Azure-logboekanalyse bijwerken naar de nieuwe logboek zoekopdracht](../log-analytics/log-analytics-log-search-upgrade.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

    De log analytics-werkruimte die als host fungeert voor het analytics-oplossing van verkeer en het nsg's hoeven niet te zijn in dezelfde regio. Bijvoorbeeld, wellicht u traffic analytics in een werkruimte in de regio West-Europa, terwijl er nsg's in VS-Oost en VS-West. Meerdere nsg's kunnen worden geconfigureerd in dezelfde werkruimte.
6. Selecteer **Opslaan**.

    ![Selectie van het opslagaccount, de werkruimte voor logboekanalyse en Traffic Analytics inschakelen](./media/traffic-analytics/selection-of-storage-account-log-analytics-workspace-and-traffic-analytics-enablement.png)

Herhaal de vorige stappen voor alle andere nsg's waarvoor u wilt inschakelen van traffic analytics voor. Gegevens uit logboeken van de stroom is verzonden naar de werkruimte, dus zorg ervoor dat de lokale wet- en regelgeving in uw land toestaat dat de opslag van gegevens in de regio waar de werkruimte bestaat.

U kunt ook configureren voor verkeer analytics met behulp van de [Set AzureRmNetworkWatcherConfigFlowLog](/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog) PowerShell-cmdlet in AzureRm PowerShell moduleversie 6.2.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="view-traffic-analytics"></a>Analytische gegevens bekijken verkeer

Selecteer op de aan de linkerkant van de portal, **alle services**, voer dan *Monitor* in de **Filter** vak. Wanneer **Monitor** wordt weergegeven in de zoekresultaten, selecteer deze. Selecteer om te verkennen traffic analytics en de mogelijkheden ervan, **netwerk-watcher**, klikt u vervolgens **Traffic Analytics**.

![Toegang tot het dashboard met analytische verkeer](./media/traffic-analytics/accessing-the-traffic-analytics-dashboard.png)

Het dashboard kan de eerste keer wordt weergegeven omdat Traffic Analytics moet voldoende gegevens voor het afleiden van betekenisvolle inzichten voordat deze rapporten kunt genereren eerst samenvoegen tot 30 minuten duren.

## <a name="usage-scenarios"></a>Gebruiksscenario's

Sommige van de inzichten te krijgen nadat Traffic Analytics is volledig geconfigureerd, kunt u zijn als volgt:

### <a name="find-traffic-hotspots"></a>Verkeer hotspots vinden

**Zoeken**

- Welke hosts, subnetten en virtuele netwerken zijn verzenden of ontvangen van de meeste verkeer, passeert maximale schadelijk verkeer en aanzienlijke stromen blokkeren?
    - Controleer vergelijkende grafiek voor de host, subnet en het virtuele netwerk. Inzicht in welke hosts, subnetten en virtuele netwerken om berichten te verzenden of ontvangen van de meeste verkeer, kunt u identificeren van de hosts die het meeste verkeer wordt verwerkt en of de distributie van verkeer juist wordt uitgevoerd.
    - U kunt evalueren of het volume van verkeer dat geschikt is voor een host. De hoeveelheid verkeer normaal gedrag is of het verdient verder onderzoek?
- Hoeveel binnenkomend en uitgaand verkeer is?
    -   De host naar verwachting ontvangen meer binnenkomend verkeer dan uitgaande, of omgekeerd?
- De statistieken van geblokkeerd verkeer.
    - Waarom is een host een aanzienlijke hoeveelheid onschadelijk verkeer blokkeren? Dit gedrag vereist verder onderzoek en waarschijnlijk optimalisatie van configuratie
- De statistieken van schadelijk verkeer toegestaan/geblokkeerd
    - Waarom is een host ontvangen van schadelijk verkeer en waarom stromen van schadelijke bron is toegestaan? Dit gedrag vereist verder onderzoek en waarschijnlijk optimalisatie van de configuratie.

    Selecteer **alle**onder **Host**, zoals wordt weergegeven in de volgende afbeelding:

    ![Dashboard met host met de meeste verkeer details](media/traffic-analytics/dashboard-showcasing-host-with-most-traffic-details.png)

- De volgende afbeelding ziet tijd trends voor de bovenste vijf sprekende hosts en de stroom-gerelateerde details (toegestane – binnenkomend en uitgaand en geweigerde - binnenkomend en uitgaand flows) voor een host:

    ![Top vijf meest praten host trend](media/traffic-analytics/top-five-most-talking-host-trend.png)

**Zoeken**

- Zijn de meest conversing host paren?
    - Verwacht gedrag zoals front-end of back-end communicatie of afwijkend gedrag, zoals back-end internetverkeer.
- De statistieken van verkeer toegestaan/geblokkeerd
    - Waarom een host is toe te staan of aanzienlijke netwerkverkeer blokkeren
- De meestgebruikte toepassingsprotocol tussen de meeste conversing host paren:
    - Zijn deze toepassingen toegestaan voor dit netwerk?
    - De toepassingen juist zijn geconfigureerd? Gebruik ze het juiste protocol voor communicatie? Selecteer **alle** onder **conversatie regelmatige**, zoals weergegeven in de volgende afbeelding:

        ![Meest voorkomende conversatie met Dashboard](./media/traffic-analytics/dashboard-showcasing-most-frequent-conversation.png)

- De volgende afbeelding ziet u tijd voor de bovenste vijf conversaties trends en de stroom-gerelateerde details, zoals toegestane en geweigerde items binnenkomende en uitgaande stromen voor een paar conversatie:

    ![Top 5 chatty conversatie details en trend](./media/traffic-analytics/top-five-chatty-conversation-details-and-trend.png)

**Zoeken**

- Welke toepassingsprotocol meest in uw omgeving wordt gebruikt, en welke conversing host paren het toepassingsprotocol het meest gebruikt?
    - Zijn deze toepassingen toegestaan voor dit netwerk?
    - De toepassingen juist zijn geconfigureerd? Gebruik ze het juiste protocol voor communicatie? Verwacht gedrag is een algemene poorten zoals 80 en 443. Voor communicatie gebruikt standaard, als een ongebruikelijke poorten worden weergegeven, ze mogelijk een configuratiewijziging. Selecteer **alle** onder **toepassing poort**, in de volgende afbeelding:

        ![Dashboard met bovenste toepassingsprotocollen](./media/traffic-analytics/dashboard-showcasing-top-application-protocols.png)

- De volgende afbeeldingen tijd weergeven voor de top vijf N7-protocollen en de stroom-gerelateerde details (bijvoorbeeld toegestaan en stromen geweigerd) voor een protocol N7 trends:

    ![Bovenste vijf laag 7 protocollen details en trend](./media/traffic-analytics/top-five-layer-seven-protocols-details-and-trend.png)

    ![Details voor toepassingsprotocol in logboek zoekopdracht stromen](./media/traffic-analytics/flow-details-for-application-protocol-in-log-search.png)

**Zoeken**

- Capaciteit gebruikstrends van een VPN-gateway in uw omgeving.
    - Elke SKU VPN kunt een bepaalde hoeveelheid bandbreedte. Zijn de VPN-gateways onderbenutte?
    - Zijn uw gateways capaciteit bereikt? Moet u upgraden naar de volgende hogere SKU?
- Zijn de meest conversing hosts, via welke VPN-gateway, via welke poort?
    - Is dit patroon normaal? Selecteer **alle** onder **VPN-gateway**, zoals wordt weergegeven in de volgende afbeelding:

        ![Dashboard met bovenste actieve VPN-verbindingen](./media/traffic-analytics/dashboard-showcasing-top-active-vpn-connections.png)

- De volgende afbeelding ziet tijd trends voor gebruik van de capaciteit van een Azure VPN-Gateway en de stroom-gerelateerde details (zoals toegestane stromen en poorten):

    ![VPN-gateway trend en stroom details van tapegebruik](./media/traffic-analytics/vpn-gateway-utilization-trend-and-flow-details.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Distributie van verkeer per Geografie visualiseren

**Zoeken**

- Distributie van verkeer per datacentrum zoals bovenste bronnen van verkeer naar een datacenter bovenste rogue netwerken conversatie met het datacenter en top conversatie toepassingsprotocollen.
    - Als u meer belasting van een datacenter ziet, kunt u plannen voor de distributie van verkeer efficiënt.
    - Als rogue netwerken zijn conversatie in het datacenter, klikt u vervolgens NSG-regels voor het blokkeren van ze te corrigeren.

    Selecteer **kaart bekijken** onder **uw omgeving**, zoals wordt weergegeven in de volgende afbeelding:

    ![Dashboard rouleren verkeer distributie](./media/traffic-analytics/dashboard-showcasing-traffic-distribution.png)

- De geo-kaart toont het bovenste lint voor de selectie van parameters zoals datacenters (Nee/geïmplementeerd-implementatie/niet-actief/actief/Traffic Analytics ingeschakeld/Traffic Analytics niet ingeschakeld) en landen Benign/kwaadaardig verkeer naar de actieve bijdragen implementatie:

    ![Geo-overzichtsweergave met actieve implementatie](./media/traffic-analytics/geo-map-view-showcasing-active-deployment.png)

- De geo-kaart toont de distributie van verkeer naar een datacenter uit andere landen en continenten communicatie met deze in blauw (onschadelijk verkeer) en rood (schadelijk verkeer) gekleurd regels:

    ![Geo-overzichtsweergave verkeer distributie naar landen en continenten met](./media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png)

    ![Details voor de distributie van verkeer in logboek zoekopdracht stromen](./media/traffic-analytics/flow-details-for-traffic-distribution-in-log-search.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Distributie van verkeer te visualiseren door virtuele netwerken

**Zoeken**

- Distributie van verkeer per virtueel netwerk, topologie, bovenste bronnen van verkeer naar het virtuele netwerk conversatie met het virtuele netwerk en de conversatie toepassingsprotocollen boven hoogste rogue-netwerken.
    - Weten welke virtuele netwerk is conversatie met welke virtuele netwerk. Als de conversatie niet verwacht wordt, kan worden gecorrigeerd.
    - Als een rogue-netwerken zijn conversatie met een virtueel netwerk, kunt u NSG-regels voor het blokkeren van de netwerken rogue corrigeren.
 
    Selecteer **weergave VNets** onder **uw omgeving**, zoals wordt weergegeven in de volgende afbeelding:

    ![Dashboard met virtueel netwerk distributie](./media/traffic-analytics/dashboard-showcasing-virtual-network-distribution.png)

- De virtuele netwerktopologie toont het bovenste lint voor de keuze van de parameters zoals een virtueel netwerk van (Inter virtueel netwerk verbindingen/actief/inactief), externe verbindingen, actieve gegevensoverdrachten en schadelijke stromen van het virtuele netwerk.
- De virtuele netwerktopologie toont de distributie van verkeer met een virtueel netwerk met betrekking tot stromen (toegestaan/geblokkeerd/Inkomend/uitgaand/Benign/kwaadaardig), het toepassingsprotocol en netwerkbeveiligingsgroepen, bijvoorbeeld:

    ![Virtuele netwerktopologie met verkeer distributie en stroom details](./media/traffic-analytics/virtual-network-topology-showcasing-traffic-distribution-and-flow-details.png)

    ![Details voor virtueel netwerkverkeer distributie in logboek zoekopdracht stromen](./media/traffic-analytics/flow-details-for-virtual-network-traffic-distribution-in-log-search.png)

**Zoeken**

- Verkeer distributie per subnet, topologie, bovenste bronnen van het verkeer naar het subnet van de conversatie aan het subnet en top toepassingsprotocollen conversatie bovenste rogue-netwerken.
    - Weten welke subnet is conversatie met welk subnet. Als er onverwachte uitwisselingen, kunt u uw configuratie corrigeren.
    - Als een rogue-netwerken zijn conversatie met een subnet, bent u kunnen corrigeren door het NSG-regels voor het blokkeren van de rogue-netwerken configureren.
- De topologie subnetten bevat het bovenste lint voor selectie van parameters zoals actief/inactief subnet, externe verbindingen, actieve gegevensoverdrachten en schadelijke stromen van het subnet.
- De topologie Subnet toont de distributie van verkeer met een virtueel netwerk met betrekking tot stromen (toegestaan/geblokkeerd/Inkomend/uitgaand/Benign/kwaadaardig), het toepassingsprotocol en nsg's, bijvoorbeeld:

    ![Distributie van verkeer met een virtueel netwerksubnet met betrekking tot stromen subnet-topologie](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-virtual-subnet-with-regards-to-flows.png)

**Zoeken**

Distributie van verkeer per toepassingsgateway & Load Balancer, topologie, bovenste bronnen van verkeer, bovenste rogue netwerken conversatie aan de toepassingsgateway & Load Balancer en top conversatie toepassingsprotocollen. 
    
 - Weten welke subnet is conversatie waarvan de toepassingsgateway of Load Balancer. Als u onverwachte conversaties ziet, kunt u uw configuratie corrigeren.
 - Als een rogue-netwerken zijn conversatie met een toepassingsgateway of een Load Balancer, bent u kunnen corrigeren door het NSG-regels voor het blokkeren van de rogue-netwerken configureren. 

    ![subnet-Topology-showcasing-Traffic-Distribution-to-a-Application-Gateway-subnet-with-regards-to-flows](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-application-gateway-subnet-with-regards-to-flows.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Poorten en virtuele machines die zijn ontvangen van verkeer van internet weergeven

**Zoeken**

- Welke poorten open zijn conversatie via internet?
    - Als er onverwachte poorten open zijn gevonden, kunt u uw configuratie corrigeren:

    ![Dashboard rouleren poorten ontvangen en verzenden van verkeer naar internet](./media/traffic-analytics/dashboard-showcasing-ports-receiving-and-sending-traffic-to-the-internet.png)

    ![Details van Azure doelpoorten en hosts](./media/traffic-analytics/details-of-azure-destination-ports-and-hosts.png)

**Zoeken**

Hebt u schadelijk verkeer in uw omgeving? Waar wordt deze die afkomstig zijn van? Waar wordt deze is bestemd voor?

![Details van de schadelijk verkeer stroomt in logboek zoeken](./media/traffic-analytics/malicious-traffic-flows-detail-in-log-search.png)


### <a name="visualize-the-trends-in-nsgnsg-rules-hits"></a>De trends in het NSG/NSG-regels treffers visualiseren

**Zoeken**

- Welke NSG/NSG-regels hebben de meeste treffers in vergelijkende grafiek met stromen verdeling?
- Wat zijn de belangrijkste bron- en doelserver conversatie paren per NSG/NSG-regels?

    ![Dashboard met NSG treffers statistieken](./media/traffic-analytics/dashboard-showcasing-nsg-hits-statistics.png)

- De volgende afbeeldingen tonen tijd trends voor treffers van NSG-regels en details van de bron-doel stroom voor een netwerkbeveiligingsgroep:

    - Snel detecteren welke nsg's en het NSG regels schadelijke stromen worden doorlopen en toegang tot uw cloudomgeving die zijn de belangrijkste schadelijke IP-adressen
    - Identificeren welke NSG/NSG-regels zijn zodat/netwerkverkeer wordt geblokkeerd aanzienlijke
    - Selecteer boven filters voor gedetailleerde inspectie van een NSG of het NSG-regels

    ![Rouleren tijd trends voor het NSG regel treffers en bovenste NSG-regels](./media/traffic-analytics/showcasing-time-trending-for-nsg-rule-hits-and-top-nsg-rules.png)

    ![Bovenste NSG regels statistieken details in het logboek zoeken](./media/traffic-analytics/top-nsg-rules-statistics-details-in-log-search.png)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Als u antwoorden op veelgestelde vragen, Zie [Traffic analytics Veelgestelde vragen over](traffic-analytics-faq.md).
