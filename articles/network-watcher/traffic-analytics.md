---
title: Azure traffic analytics | Microsoft Docs
description: Meer informatie over het analyseren van Azure network security group-stroomlogboeken met traffic analytics.
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
ms.openlocfilehash: 120b97f69c8fad2daf3090441e8d0326e80115c3
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338580"
---
# <a name="traffic-analytics"></a>Verkeersanalyse

Traffic analytics is een cloud-gebaseerde oplossing waarmee u inzicht in gebruikers en toepassingen in cloudnetwerken. Traffic analytics analyseert Network Watcher network security group (NSG)-stroomlogboeken voor inzicht in de verkeersstroom in uw Azure-cloud. Met traffic analytics, kunt u het volgende doen:

- Netwerkactiviteit visualiseren voor uw Azure-abonnementen en hotspots identificeren.
- Beveiligingsrisico's te identificeren en Beveilig uw netwerk, met informatie zoals open-poorten, toepassingen die toegang tot internet en virtuele machines (VM) maken van verbinding met netwerken rogue proberen.
- Patronen van verkeersstromen begrijpen tussen Azure-regio's en het internet naar het optimaliseren van uw netwerkimplementatie voor de prestaties en capaciteit.
- Identificeer netwerk configuratiefouten leiden tot mislukte verbindingen in uw netwerk.

## <a name="why-traffic-analytics"></a>Waarom traffic analytics?

Het is essentieel om te controleren, beheren en weten van uw eigen netwerk voor ongeschonden beveiliging, naleving en prestaties. Uw eigen omgeving te weten is van cruciaal belang om te beveiligen en te optimaliseren. Vaak wilt u weten wat de huidige toestand van het netwerk, die verbinding maakt, waar ze verbinding maakt, welke poorten zijn geopend, het internet, verwachte netwerkverkeer, onregelmatig netwerk gedrag, en plotselinge toename van verkeer.

Cloudnetwerken zijn anders dan on-premises bedrijfsnetwerken, waarbij u netflow of gelijkwaardige protocol kunnen routers en switches, die de mogelijkheid bieden voor het verzamelen van IP-netwerkverkeer als deze binnengaat of een netwerkinterface verlaat hebben. Verkeer stroom door gegevens te analyseren, kunt u een analyse van netwerkverkeer en het volume maken.

Virtuele netwerken van Azure hebben NSG-stroomlogboeken, waarin u informatie over binnenkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep die is gekoppeld aan afzonderlijke netwerkinterfaces, virtuele machines of subnetten. Door onbewerkte NSG-stroomlogboeken analyseren en het invoegen van intelligence van beveiliging, topologie en Geografie, traffic kunt analytics u voorzien van inzicht in de verkeersstroom in uw omgeving. Traffic Analytics voorziet in informatie zoals meest communicerende hosts, meest communicerende toepassingsprotocollen, meest converserende host-paren, verkeer toegestaan/geblokkeerd, binnenkomend en uitgaand verkeer, open poorten met internet, meest blokkerende regels, verkeer distributie per Azure-datacenter, virtueel netwerk, subnetten, of rogue netwerken.

## <a name="key-components"></a>Belangrijkste onderdelen

- **Netwerkbeveiligingsgroep (NSG)**: Bevat een lijst met regels voor toestaan of weigeren van netwerkverkeer naar resources die zijn verbonden met een Azure-netwerk. NSG's kunnen worden gekoppeld aan subnetten, afzonderlijke virtuele machines (klassiek) of afzonderlijke netwerkinterfaces (NIC) die zijn gekoppeld aan VM’s (Resource Manager). Zie voor meer informatie, [overzicht van netwerkbeveiligingsgroepen](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Network security group (NSG) stroomlogboeken**: Kunt u informatie bekijken over inkomende en uitgaande IP-verkeer via een netwerkbeveiligingsgroep. NSG-stroom logboeken zijn geschreven in json-indeling en weergeven van binnenkomende en uitgaande stromen op basis van per regel, dat de NIC van de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP-adres, bron-/ doel-poort en protocol) en als het verkeer is toegestaan of is geweigerd. Zie voor meer informatie over NSG-stroomlogboeken [NSG-stroomlogboeken](network-watcher-nsg-flow-logging-overview.md).
- **Log Analytics**: Een Azure-service die gegevens verzamelt en de gegevens worden opgeslagen in een centrale opslagplaats. Deze gegevens kunnen gebeurtenissen, prestatiegegevens of aangepaste gegevens die worden geleverd via de API van Azure bevatten. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export. Bewaking van toepassingen, zoals network performance monitor en verkeer analytics zijn gebouwd met behulp van Log Analytics als basis. Zie voor meer informatie, [Log analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Log analytics-werkruimte**: Een instantie van log analytics, waar de gegevens die betrekking hebben op een Azure-account is opgeslagen. Zie voor meer informatie over log analytics-werkruimten, [een Log Analytics-werkruimte maken](../azure-monitor/learn/quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Network Watcher**: Een regionale service waarmee u kunt bewaken en diagnosticeren op netwerkscenarioniveau in Azure. U kunt NSG-stroomlogboeken in- en uitschakelen met Network Watcher inschakelen. Zie voor meer informatie, [Network Watcher](network-watcher-monitoring-overview.md).

## <a name="how-traffic-analytics-works"></a>De werking van traffic analytics

Verkeersanalyse onderzoekt de onbewerkte NSG-stroomlogboeken en verminderde Logboeken vastgelegd door samenvoeging van veelvoorkomende stromen tussen de dezelfde bron-IP-adres, de doel-IP-adres, de doelpoort en het protocol. Bijvoorbeeld-Host 1 (IP-adres: 10.10.10.10) communicatie met de Host 2 (IP-adres: 10.10.20.10), 100 keer gebruikt in een periode van 1 uur (bijvoorbeeld: 80)-poort en protocol (bijvoorbeeld http). De verminderde logboek heeft een vermelding, die-Host 1 en 2 van de Host gecommuniceerd 100 keer gedurende een periode van 1 uur via poort *80* en protocol *HTTP*, in plaats van 100 berichten. Verminderde logboeken zijn uitgebreid met Geografie, beveiliging en informatie over de topologie en vervolgens opgeslagen in een log analytics-werkruimte. De volgende afbeelding ziet u de gegevensstroom:

![De gegevensstroom voor verwerking van NSG-stroomlogboeken](./media/traffic-analytics/data-flow-for-nsg-flow-log-processing.png)

## <a name="supported-regions"></a>Ondersteunde regio’s

U kunt traffic analytics gebruiken voor nsg's in een van de volgende ondersteunde regio's:

* Canada - midden
* US - west-centraal
* US - oost
* US - oost 2
* US - noord-centraal
* US - zuid-centraal
* US - centraal
* US - west
* US - west 2
* Europa -west
* Europa - noord
* Brazilië - zuid
* Verenigd Koninkrijk West
* Verenigd Koninkrijk Zuid
* Australië - oost
* Australië - zuidoost
* Azië - zuidoost
* India - centraal
* India - zuid
* Japan - oost 

De Log Analytics-werkruimte moet bestaan in de volgende regio's:
* Canada - midden
* US - west-centraal
* US - oost
* Europa -west
* Verenigd Koninkrijk Zuid
* Australië - zuidoost
* Azië - zuidoost
* India - centraal
* Japan - oost

## <a name="prerequisites"></a>Vereisten

### <a name="user-access-requirements"></a>Toegangsvereisten van gebruiker

Uw account moet lid zijn van een van de volgende Azure [ingebouwde rollen](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json):

|Implementatiemodel   | Rol                   |
|---------          |---------               |
|Resource Manager   | Eigenaar                  |
|                   | Inzender            |
|                   | Lezer                 |
|                   | Inzender voor netwerken    |
|Klassiek            | Accountbeheerder  |
|                   | Servicebeheerder  |
|                   | Co-beheerder       |

Als uw account niet aan een van de ingebouwde rollen toegewezen is, moet deze worden toegewezen aan een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) die de volgende acties uit, op het abonnementsniveau van het is toegewezen:

- "Microsoft.Network/applicationGateways/read"
- "Microsoft.Network/connections/read"
- "Microsoft.Network/loadBalancers/read"
- "Microsoft.Network/localNetworkGateways/read"
- "Microsoft.Network/networkInterfaces/read"
- "Microsoft.Network/networkSecurityGroups/read"
- "Microsoft.Network/publicIPAddresses/read"
- "Microsoft.Network/routeTables/read"
- "Microsoft.Network/virtualNetworkGateways/read"
- "Microsoft.Network/virtualNetworks/read"

Zie voor meer informatie over het controleren van machtigingen voor gebruikerstoegang [Traffic analytics Veelgestelde vragen over](traffic-analytics-faq.md).

### <a name="enable-network-watcher"></a>Network Watcher inschakelen

Voor het analyseren van netwerkverkeer, moet u beschikken over een bestaande netwerk-watcher of [een network watcher inschakelen](network-watcher-create.md) verkeer in elke regio waarvoor u nsg's die u wilt analyseren voor. Traffic analytics kan worden ingeschakeld voor nsg's die worden gehost in een van de [ondersteunde regio's](#supported-regions).

### <a name="re-register-the-network-resource-provider"></a>De netwerkresourceprovider opnieuw te registreren

Voordat u traffic analytics gebruiken kunt, moet u uw netwerkresourceprovider opnieuw registreren. Klik op **uitproberen** in de volgende code in de Azure Cloud Shell openen. De Cloud Shell legt u in automatisch in uw Azure-abonnement. Nadat de Cloud Shell geopend is, voert u de volgende opdracht om de netwerkresourceprovider opnieuw te registreren:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Selecteer een netwerkbeveiligingsgroep

Voordat u NSG-stroom logboekregistratie inschakelt, moet u een netwerkbeveiligingsgroep om aan te melden stromen voor hebben. Als u geen een netwerkbeveiligingsgroep, Zie [maken van een netwerkbeveiligingsgroep](../virtual-network/manage-network-security-group.md#create-a-network-security-group) een te maken.

Aan de linkerkant van de Azure-portal, selecteert u **Monitor**, klikt u vervolgens **Network watcher**, en selecteer vervolgens **NSG-stroomlogboeken**. Selecteer de netwerkbeveiligingsgroep die u inschakelen van een NSG-stroom-logboek voor, wilt zoals wordt weergegeven in de volgende afbeelding:

![Selectie van nsg's waarvoor de activering van het logboek van NSG-stroom](./media/traffic-analytics/selection-of-nsgs-that-require-enablement-of-nsg-flow-logging.png)

Als u probeert te verkeersanalyse inschakelen voor een NSG die wordt gehost in andere regio's dan de [ondersteunde regio's](#supported-regions), ontvangt u een 'Niet gevonden'-fout.

## <a name="enable-flow-log-settings"></a>Logboekinstellingen stroom inschakelen

Voordat u de instellingen van het flow inschakelt, moet u de volgende taken uitvoeren:

Registreer de provider Azure Insights als deze nog niet geregistreerd voor uw abonnement:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

Als u nog niet hebt een Azure Storage-account voor het opslaan van NSG-stroom zich aanmeldt, moet u een opslagaccount maken. U kunt een storage-account maken met de opdracht die volgt. Voordat u de opdracht uitvoert, Vervang `<replace-with-your-unique-storage-account-name>` met een naam die uniek is voor alle Azure locaties, tussen 3 en 24 tekens lang, met behulp van alleen cijfers en kleine letters. U kunt ook de naam van de resourcegroep, indien nodig wijzigen.

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
2. Selecteer een bestaand opslagaccount voor het opslaan van de logboeken van de stroom in. Als u wilt voor het opslaan van de gegevens altijd, stel de waarde op *0*. Worden er kosten voor Azure Storage voor het opslagaccount.
3. Stel **retentie** aan het aantal dagen dat u wilt voor het opslaan van gegevens voor.
4. Selecteer *op* voor **Traffic Analytics Status**.
5. Selecteer een bestaande Log Analytics-werkruimte of selecteer **nieuwe werkruimte maken** naar een nieuwe maken. Een Log Analytics-werkruimte wordt gebruikt door Traffic Analytics voor het opslaan van de geaggregeerde en geïndexeerde gegevens die vervolgens gebruikt wordt voor het genereren van de analyse. Als u een bestaande werkruimte selecteert, moet bestaan in een van de [ondersteunde regio's](#traffic-analytics-supported-regions) en bijgewerkt naar de nieuwe querytaal. Als u niet wilt upgraden van een bestaande werkruimte of geen een werkruimte in een ondersteunde regio, kunt u een nieuwe maken. Zie voor meer informatie over querytalen [Azure Log Analytics upgraden voor nieuwe zoekopdrachten](../log-analytics/log-analytics-log-search-upgrade.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

    De log analytics-werkruimte die als host fungeert voor de oplossing voor traffic analytics en de nsg's hoeven niet te zijn in dezelfde regio. Bijvoorbeeld, u mogelijk traffic analytics in een werkruimte in de regio West-Europa, terwijl er nsg's in VS-Oost en VS-West. Meerdere nsg's kunnen worden geconfigureerd in dezelfde werkruimte.
6. Selecteer **Opslaan**.

    ![Selectie van het opslagaccount, Log Analytics-werkruimte en inschakelen voor Traffic Analytics](./media/traffic-analytics/selection-of-storage-account-log-analytics-workspace-and-traffic-analytics-enablement.png)

Herhaal de vorige stappen voor alle andere nsg's waarvoor u wilt inschakelen van traffic analytics voor. Gegevens uit logboeken van de stroom wordt verzonden naar de werkruimte, dus zorg ervoor dat de lokale wetten en regelgeving in uw land/regio toestaan dat de opslag van gegevens in de regio waar de werkruimte zich bevindt.

U kunt ook configureren voor traffic analytics met behulp van de [Set AzureRmNetworkWatcherConfigFlowLog](/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog) PowerShell-cmdlet in AzureRm PowerShell-moduleversie 6.2.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` te vinden van de geïnstalleerde versie. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="view-traffic-analytics"></a>Verkeersanalyse weergeven

Selecteer op de aan de linkerkant van de portal, **alle services**, voert u *Monitor* in de **Filter** vak. Wanneer **Monitor** wordt weergegeven in de zoekresultaten, selecteert u dit. Selecteer eerst verkennen traffic analytics en de mogelijkheden ervan **Network watcher**, klikt u vervolgens **Traffic Analytics**.

![Toegang tot het dashboard Traffic Analytics](./media/traffic-analytics/accessing-the-traffic-analytics-dashboard.png)

Het dashboard duurt maximaal 30 minuten tot de eerste keer worden weergegeven omdat Traffic Analytics moet eerst Verzamel voldoende gegevens voor het afleiden van betekenisvolle inzichten, voordat deze rapporten kunt genereren.

## <a name="usage-scenarios"></a>Gebruiksscenario's

Sommige van de inzichten te krijgen nadat Traffic Analytics is volledig geconfigureerd, kunt u zijn als volgt:

### <a name="find-traffic-hotspots"></a>Verkeer hotspots zoeken

**Zoeken**

- Welke hosts, subnetten en virtuele netwerken verzenden of ontvangen het meeste verkeer, maximale schadelijk verkeer doorlopen en aanzienlijke stromen blokkeren?
    - Controleer vergelijkende grafieken voor de host, subnet en virtueel netwerk. Inzicht in welke hosts, subnetten en virtuele netwerken worden verzonden of ontvangen van het meeste verkeer, kunt u identificeren van de hosts die het meeste verkeer wordt verwerkt en of de distributie van verkeer correct wordt uitgevoerd.
    - U kunt evalueren of het volume verkeer geschikt voor een host is. Is het volume van de normale werking van verkeer van of deze verdient verder onderzoek?
- Hoeveel binnenkomend en uitgaand verkeer is?
    -   De host naar verwachting ontvangen meer binnenkomend verkeer dan uitgaande of vice versa?
- Statistieken van het verkeer geblokkeerd.
    - Waarom is een host een aanzienlijke hoeveelheid goedaardig verkeer blokkeren? Dit probleem vereist verder onderzoek en waarschijnlijk optimalisatie van de configuratie
- Statistieken van schadelijk verkeer toegestaan/geblokkeerd
    - Waarom is er een host ontvangen schadelijk verkeer en waarom stromen van schadelijke bron is toegestaan? Dit gedrag vereist verder onderzoek en waarschijnlijk optimalisatie van de configuratie.

    Selecteer **alle**onder **Host**, zoals wordt weergegeven in de volgende afbeelding:

    ![Dashboard met daarop host met details van de meeste verkeer](media/traffic-analytics/dashboard-showcasing-host-with-most-traffic-details.png)

- De volgende afbeelding ziet u tijd trends voor de top vijf praten hosts en de details met betrekking tot flow (toegestane: binnenkomend en uitgaand en geweigerde - inkomende/uitgaande stromen) voor een host:

    ![Trend van vijf host de meeste communicatie](media/traffic-analytics/top-five-most-talking-host-trend.png)

**Zoeken**

- De meeste converserende host-paren zijn?
    - Verwacht gedrag net als de front-end en back-end communicatie of afwijkend gedrag, zoals back-end internetverkeer.
- Statistieken van het verkeer toegestaan/geblokkeerd
    - Waarom een host is toestaan of blokkeren van aanzienlijke verkeersvolume
- Meest gebruikte toepassingsprotocol tussen de meeste converserende host paren:
    - Mogen deze toepassingen op dit netwerk?
    - De toepassingen juist zijn geconfigureerd? Zijn ze het juiste protocol gebruikt voor communicatie? Selecteer **alle** onder **frequente conversatie**, zoals weergegeven in de volgende afbeelding:

        ![Dashboard met daarop de meest voorkomende gesprek](./media/traffic-analytics/dashboard-showcasing-most-frequent-conversation.png)

- De volgende afbeelding ziet u tijd voor de top vijf gesprekken trends en de details van de flow-gerelateerde zoals toegestaan en binnenkomende en uitgaande stromen voor een paar conversatie geweigerd:

    ![Top vijf intensieve conversatie details en trend](./media/traffic-analytics/top-five-chatty-conversation-details-and-trend.png)

**Zoeken**

- Welke toepassingsprotocol meest in uw omgeving wordt gebruikt, en welke converserende paren van de host het toepassingsprotocol het meest gebruikt?
    - Mogen deze toepassingen op dit netwerk?
    - De toepassingen juist zijn geconfigureerd? Zijn ze het juiste protocol gebruikt voor communicatie? Normaal is algemene poorten, zoals 80 en 443. Voor communicatie met de standaard als een ongebruikelijke poorten worden weergegeven, ze mogelijk een configuratiewijziging. Selecteer **alle** onder **toepassingspoort**, in de volgende afbeelding:

        ![Dashboard met daarop de meest gebruikte toepassingsprotocollen](./media/traffic-analytics/dashboard-showcasing-top-application-protocols.png)

- De volgende afbeeldingen tonen tijd trends voor de top vijf L7-protocollen en de details met betrekking tot flow (bijvoorbeeld toegestaan en stromen geweigerd) voor een L7-protocol:

    ![Top vijf layer 7 protocollen details en trend](./media/traffic-analytics/top-five-layer-seven-protocols-details-and-trend.png)

    ![Stroomgegevens voor toepassingsprotocol in zoeken in Logboeken](./media/traffic-analytics/flow-details-for-application-protocol-in-log-search.png)

**Zoeken**

- Capaciteitsverbruik trends van een VPN-gateway in uw omgeving.
    - Elke SKU VPN kunt een bepaalde hoeveelheid bandbreedte. Zijn de VPN-gateways gebruikt?
    - Zijn uw gateways capaciteit bereikt? Moet u een upgrade naar de volgende hogere SKU?
- Welke het meest converserende hosts, via welke VPN-gateway, via welke poort zijn?
    - Is dit patroon normaal? Selecteer **alle** onder **VPN-gateway**, zoals wordt weergegeven in de volgende afbeelding:

        ![Dashboard met daarop bovenste actieve VPN-verbindingen](./media/traffic-analytics/dashboard-showcasing-top-active-vpn-connections.png)

- De volgende afbeelding ziet u tijd trends voor gebruik van de capaciteit van een Azure VPN-Gateway en de details met betrekking tot flow (zoals toegestane stromen en poorten):

    ![VPN-gateway gebruik trend en flow details](./media/traffic-analytics/vpn-gateway-utilization-trend-and-flow-details.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Distributie van verkeer op basis van Geografie visualiseren

**Zoeken**

- Distributie van verkeer per datacentrum, zoals de meest gebruikte bronnen van verkeer naar een datacenter, top rogue netwerken discussiëren met het datacenter en boven toepassingsprotocollen bespreken.
    - Als u meer belasting van een datacenter geobserveerd, kunt u van plan bent voor efficiënte verkeersdistributie.
    - Als rogue-netwerken in het datacenter bespreken worden, klikt u vervolgens NSG-regels voor het blokkeren van ze te corrigeren.

    Selecteer **kaart weergeven** onder **uw omgeving**, zoals wordt weergegeven in de volgende afbeelding:

    ![Dashboard weergeven distributie van verkeer](./media/traffic-analytics/dashboard-showcasing-traffic-distribution.png)

- De geografische gebieden toegewezen bevat de bovenste lint voor de selectie van parameters zoals datacenters (geen/geïmplementeerd-implementatie/niet-actief/actief/Traffic Analytics ingeschakeld/Traffic Analytics niet ingeschakeld) en die bijdragen aan Benign/schadelijk verkeer naar de actieve landen implementatie:

    ![Geografische kaartweergave actieve implementatie weergeven](./media/traffic-analytics/geo-map-view-showcasing-active-deployment.png)

- De geo-kaart toont de distributie van verkeer naar een datacenter uit andere landen en continenten communicatie met deze in het blauw (goedaardig verkeer) en rode (schadelijk verkeer) gekleurd regels:

    ![Geografische kaartweergave distributie van verkeer in landen/regio's en continenten weergeven](./media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png)

    ![Stroomgegevens voor de distributie van verkeer in zoeken in Logboeken](./media/traffic-analytics/flow-details-for-traffic-distribution-in-log-search.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Visualiseer de distributie van verkeer door virtuele netwerken

**Zoeken**

- Distributie van verkeer per virtueel netwerk, topologie, meest gebruikte bronnen van verkeer naar het virtuele netwerk, top rogue netwerken bespreken met het virtuele netwerk en boven toepassingsprotocollen bespreken.
    - Weten welk virtuele netwerk is bespreken met welke virtuele netwerk. Als de conversatie niet verwacht wordt, kan worden gecorrigeerd.
    - Als een rogue-netwerken zijn discussiëren met een virtueel netwerk, kunt u NSG-regels voor het blokkeren van de rogue-netwerken kunt oplossen.
 
    Selecteer **weergave VNets** onder **uw omgeving**, zoals wordt weergegeven in de volgende afbeelding:

    ![Dashboard met daarop virtuele netwerkdistributie](./media/traffic-analytics/dashboard-showcasing-virtual-network-distribution.png)

- De topologie van het virtuele netwerk bevat de bovenste lint voor selectie van parameters, zoals een virtueel netwerk van (Inter virtueel netwerk verbindingen/actief/inactief), externe verbindingen, actieve stromen en schadelijke stromen van het virtuele netwerk.
- U kunt de virtuele netwerktopologie op basis van abonnementen, werkruimten, resourcegroepen en tijdsinterval filteren. Extra filters waarmee u inzicht in de stroom zijn: Flow Type (InterVNet, IntraVNET enzovoort), Flow richting (inkomend, uitgaand), de Status Flow (toegestaan, geblokkeerd) vnet's (doelgroepen en verbonden), verbindingstype (Peering of Gateway - P2S- en S2S) en in de Netwerkbeveiligingsgroep. Deze filters gebruiken om zich te richten op VNets die u wilt onderzoeken in detail.
- De topologie van het virtuele netwerk bevat de distributie van verkeer naar een virtueel netwerk met betrekking tot stromen (toegestaan/geblokkeerd/Inkomend/uitgaand/Benign/kwaadaardig), toepassingsprotocol en netwerkbeveiligingsgroepen, bijvoorbeeld:

    ![Virtuele netwerktopologie verkeer distributie en flow details weergeven](./media/traffic-analytics/virtual-network-topology-showcasing-traffic-distribution-and-flow-details.png)
    
    ![Virtueel netwerk-topologie die laat zien op het hoogste niveau en meer filters](./media/traffic-analytics/virtual-network-filters.png)

    ![Stroomgegevens voor de distributie van verkeer voor virtueel netwerk in zoeken in Logboeken](./media/traffic-analytics/flow-details-for-virtual-network-traffic-distribution-in-log-search.png)

**Zoeken**

- Verkeer distributie per subnet, topologie, meest gebruikte bronnen van verkeer naar het subnet bovenste rogue-netwerken voor het subnet, en bespreken toepassingsprotocollen boven bespreken.
    - Weten welk subnet is bespreken aan welk subnet. Als u onverwachte gesprekken ziet, kunt u uw configuratie kunt corrigeren.
    - Als een rogue-netwerken zijn discussiëren met een subnet, bent u kunnen pas dit door het configureren van NSG-regels voor het blokkeren van de rogue-netwerken.
- De topologie subnetten bevat de bovenste lint voor de selectie van parameters zoals actief/inactief subnet, externe verbindingen, actieve stromen en schadelijke stromen van het subnet.
- De topologie van het Subnet bevat de distributie van verkeer naar een virtueel netwerk met betrekking tot stromen (toegestaan/geblokkeerd/Inkomend/uitgaand/Benign/kwaadaardig), toepassingsprotocol en nsg's, bijvoorbeeld:

    ![Subnettopologie distributie van verkeer een subnet van een virtueel netwerk met betrekking tot stromen weergeven](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-virtual-subnet-with-regards-to-flows.png)

**Zoeken**

Distributie van verkeer per toepassingsgateway & Load Balancer, topologie, meest gebruikte bronnen van het verkeer, top rogue netwerken bespreken naar de Application gateway & Load Balancer, en van boven toepassingsprotocollen bespreken. 
    
 - Weten welk subnet is bespreken waarvan de Load Balancer of Application gateway. Als u onverwachte gesprekken geobserveerd, kunt u uw configuratie corrigeren.
 - Als een rogue-netwerken zijn discussiëren met een Application gateway of een Load Balancer, bent u kunnen pas dit door het configureren van NSG-regels voor het blokkeren van de rogue-netwerken. 

    ![subnet-Topology-showcasing-Traffic-Distribution-to-a-Application-Gateway-subnet-with-regards-to-flows](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-application-gateway-subnet-with-regards-to-flows.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Poorten en virtuele machines die verkeer ontvangen van de internet bekijken

**Zoeken**

- Welke poorten openen via het internet zijn bespreken?
    - Als er onverwachte poorten open zijn gevonden, kunt u uw configuratie corrigeren:

    ![Dashboard weergeven poorten ontvangen en verzenden van verkeer naar internet](./media/traffic-analytics/dashboard-showcasing-ports-receiving-and-sending-traffic-to-the-internet.png)

    ![Details van de Azure-doelpoorten en hosts](./media/traffic-analytics/details-of-azure-destination-ports-and-hosts.png)

**Zoeken**

Hebt u schadelijk verkeer in uw omgeving? Waar wordt deze die afkomstig zijn van? Waar is het dat is bestemd voor?

![Detail van schadelijke stromen in zoeken in Logboeken](./media/traffic-analytics/malicious-traffic-flows-detail-in-log-search.png)


### <a name="visualize-the-trends-in-nsgnsg-rules-hits"></a>Visualiseer de trends in treffers van NSG/NSG-regels

**Zoeken**

- Welke NSG/NSG-regels bevatten de meeste treffers vergelijkende grafieken met stromen distributie?
- Wat zijn de belangrijkste bron- en doelserver gespreksparen per NSG/NSG-regels?

    ![Dashboard met daarop NSG treffers statistieken](./media/traffic-analytics/dashboard-showcasing-nsg-hits-statistics.png)

- De volgende afbeeldingen tonen tijd trends voor treffers van NSG-regels en de details van de bron-doel productiestroom voor een netwerkbeveiligingsgroep:

    - Snel te detecteren welke nsg's en de NSG regels schadelijke stromen zijn doorlopen en zijn de belangrijkste schadelijke IP-adressen toegang tot uw cloudomgeving
    - Identificeren welke NSG/NSG-regels toestaan/blokkeren significant netwerkverkeer
    - Selecteer boven filters voor gedetailleerde controle van een NSG of de NSG regels

    ![Die laat zien tijd trends voor treffers van NSG-regel en belangrijkste NSG-regels](./media/traffic-analytics/showcasing-time-trending-for-nsg-rule-hits-and-top-nsg-rules.png)

    ![Belangrijkste NSG regels voor statistieken in zoeken in Logboeken](./media/traffic-analytics/top-nsg-rules-statistics-details-in-log-search.png)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Vindt u antwoorden op veelgestelde vragen, [Traffic analytics Veelgestelde vragen over](traffic-analytics-faq.md).
