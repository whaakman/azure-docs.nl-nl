---
title: Azure Traffic Analytics | Microsoft Docs
description: Informatie over het analyseren van Azure-netwerk groep stroom beveiligingslogboeken met Traffic Analytics.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: jdial
ms.openlocfilehash: c113bbe646a54813a2885b3a9087a0171220f271
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="traffic-analytics"></a>Verkeer Analytics

Verkeer Analytics is een cloud-gebaseerde oplossing die inzicht in gebruikers- en activiteit in de cloud-netwerken biedt. Verkeer Analytics analyseert netwerk-Watcher netwerk groep (NSG) stroom beveiligingslogboeken om op te geven inzicht in het netwerkverkeer in uw Azure-cloud. Met Traffic Analytics, kunt u het volgende doen:

- Netwerkactiviteit visualiseren via uw Azure-abonnementen en hotspots identificeren.
- Beveiligingsrisico's te identificeren en beveiligen van uw netwerk, met informatie zoals open-poorten, toepassingen die proberen toegang tot internet en virtuele machines (VM) netwerken rogue verbinding maakt.
- Begrijpen stroom verkeerspatronen over Azure-regio's en het internet om de netwerkimplementatie van uw voor de prestaties en capaciteit te optimaliseren.
- Speldenpunt netwerk door middel van configuratiefouten leidt tot mislukte verbindingen in uw netwerk.

## <a name="why-traffic-analytics"></a>Waarom Traffic Analytics?

Het is essentieel om te controleren, beheren en weten van uw eigen netwerk voor ongeschonden beveiliging, naleving en prestaties. Uw eigen omgeving weten is van groot belang om te beveiligen en te optimaliseren. Vaak wilt u weten de huidige status van het netwerk die verbinding maakt wanneer welke poorten geopend met het internet zijn, verwacht gedrag voor netwerk, onregelmatige netwerk gedrag, en plotselinge stijgingen in het verkeer.

Cloud-netwerken zijn anders dan enterprise voor on-premises netwerken, waarin u Netflow of gelijkwaardige protocol kunnen routers en switches, die de mogelijkheid bieden voor het verzamelen van IP-netwerkverkeer, zoals het binnengaat of een netwerkinterface verlaat hebben. Door het verkeer stroomgegevens analyseren, kunt u een analyse van netwerkverkeer en het volume.

Virtuele netwerken van Azure hebben NSG stroom Logboeken, waarin u informatie over binnenkomende en uitgaande IP-verkeer via een netwerk-beveiligingsgroep die is gekoppeld aan afzonderlijke netwerkinterfaces, virtuele machines of subnetten. Door het analyseren van onbewerkte NSG vloeien logboeken en invoegen intelligence van beveiliging, de topologie en Geografie, Traffic Analytics kunt u voorzien van inzicht in het netwerkverkeer in uw omgeving. Verkeer Analytics biedt informatie zoals meest communicerende hosts, meest communicerende toepassingsprotocollen, meest conversatie host paren, verkeer toegestaan/geblokkeerd, binnenkomend en uitgaand verkeer, open internet-poorten, meest blokkerende regels, verkeer distributiepunten per Azure-datacenter, virtueel netwerk, subnetten, of rogue netwerken.

## <a name="key-components"></a>Belangrijkste onderdelen 

- **Netwerkbeveiligingsgroep (NSG)**: bevat een lijst met regels voor toestaan of weigeren van netwerkverkeer naar resources die zijn verbonden met een virtueel netwerk van Azure. NSG's kunnen worden gekoppeld aan subnetten, afzonderlijke virtuele machines (klassiek) of afzonderlijke netwerkinterfaces (NIC) die zijn gekoppeld aan VM’s (Resource Manager). Zie voor meer informatie [netwerk groep beveiligingsoverzicht](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Netwerkbeveiligingsgroep (NSG) stroom logboeken netwerk**: kunt u informatie bekijken over inkomende en uitgaande IP-verkeer via een netwerkbeveiligingsgroep. NSG-stroom logboeken zijn geschreven in json-indeling en weergeven uitgaande en inkomende stromen per regel op basis van een de NIC de stroom is van toepassing op 5-tuple informatie over de stroom (bron/het doel-IP-adres, bron/het doel-poort en protocol), en als het verkeer is toegestaan of geweigerd. Zie voor meer informatie over het NSG stroom logboeken [NSG stroom logboeken](network-watcher-nsg-flow-logging-overview.md).
- **Meld u Analytics**: een Azure-service die u verzamelt bewakingsgegevens en slaat de gegevens in een centrale opslagplaats. Deze gegevens kunnen gebeurtenissen, prestatiegegevens of aangepaste worden geleverd via de Azure-API bevatten. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export. Bewaking van toepassingen, zoals netwerk-Prestatiemeter en Traffic Analytics zijn gebouwd met logboekanalyse als basis. Zie voor meer informatie [Meld analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Meld u werkruimte**: een exemplaar van logboekanalyse, waar de gegevens die betrekking hebben op een Azure-account is opgeslagen. Zie voor meer informatie over Log Analytics-werkruimten [maken van een werkruimte voor logboekanalyse](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Netwerk-Watcher**: een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op niveau van een scenario in Azure. U kunt NSG stroom Logboeken in- en uitschakelen met de netwerk-Watcher inschakelen. Zie voor meer informatie [netwerk-Watcher](network-watcher-monitoring-overview.md#network-watcher).

## <a name="how-traffic-analytics-works"></a>De werking van Traffic Analytics 

Verkeer Analytics onderzoekt het NSG-logboeken voor onbewerkte stroom en verminderde Logboeken door het samenvoegen van algemene stromen tussen de dezelfde bron-IP-adres, IP-doeladres, doelpoort en -protocol wordt vastgelegd. Bijvoorbeeld:-Host 1 (IP-adres: 10.10.10.10) naar Host 2 communicatie (IP-adres: 10.10.20.10), 100 keer gedurende een periode van 1 uur met behulp van poort (bijvoorbeeld: 80) en het protocol (bijvoorbeeld http). De verminderde logboek heeft een vermelding, die Host 1 & Host 2 uitgewisseld 100 keer gedurende een periode van 1 uur met behulp van poort *80* en protocol *HTTP*, in plaats van 100 vermeldingen. Verminderde logboeken worden opgeslagen in de werkruimte voor logboekanalyse en uitgebreid met Geografie-, beveiligings-en-topologie. De uitgebreide logboeken zijn verdere geanalyseerd om te analytics worden afgeleid. De volgende afbeelding ziet de gegevensstroom:

![De werking van Traffic Analytics](media/traffic-analytics/1.png)

## <a name="supported-regions"></a>Ondersteunde regio’s

Verkeer Analytics is beschikbaar in preview. Functies in de preview-versie hoeft niet dezelfde mate van beschikbaarheid en betrouwbaarheid als functies in het algemeen release.  In de preview-versie, kunt u verkeer Analytics voor nsg's in een van de volgende gebieden: West-Centraal VS, VS-Oost, VS-Oost 2, Noordelijk Centraal, VS, Zuid-centraal VS, VS-midden, VS-West, VS-West-2, West-Europa, Noord-Europa, West VK, Zuid VK, Australië-Oost , en Australië-Zuidoost. De werkruimte voor logboekanalyse moet bestaan in de West-Centraal VS, VS-Oost, West-Europa, Australië-Zuidoost of Zuid VK regio.

## <a name="prerequisites"></a>Vereisten

### <a name="enable-network-watcher"></a>Inschakelen van netwerk-Watcher 

Voor het analyseren van verkeer, moet u beschikken over een bestaande netwerk-Watcher of [inschakelen van een Azure-netwerk-watcher](network-watcher-create.md) verkeer voor elke regio waarin u nsg's die u wilt analyseren hebt. Analytics voor verkeer kan worden ingeschakeld voor het nsg's die worden gehost in een van de [ondersteunde regio's](#supported-regions).

### <a name="re-register-the-network-resource-provider"></a>De netwerk-resourceprovider opnieuw geregistreerd 

Voordat u verkeer Analytics tijdens de preview gebruiken kunt, moet u uw netwerkresourceprovider opnieuw registreren. Klik op **probeert het** in de volgende code in de Azure-Cloud-Shell openen. De Cloud-Shell registreert u in automatisch in uw Azure-abonnement. Nadat de Cloud-Shell geopend is, voer de volgende opdracht om de netwerk-resourceprovider opnieuw geregistreerd:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Selecteer een netwerkbeveiligingsgroep 

Voordat u NSG stroom logboekregistratie inschakelt, moet u een netwerkbeveiligingsgroep aan te melden stromen voor hebben. Als u een netwerkbeveiligingsgroep geen hebt, raadpleegt u [maken van een netwerkbeveiligingsgroep](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) een maken.

Aan de linkerkant van de Azure portal, selecteer **Monitor**, klikt u vervolgens **netwerk-watcher**, en selecteer vervolgens **NSG stroom logboeken**. Selecteer de netwerkbeveiligingsgroep die u inschakelen, een NSG stroom logboek, wilt zoals wordt weergegeven in de volgende afbeelding:

![Selecteer een NSG](media/traffic-analytics/2.png)

Als u probeert te Traffic Analytics inschakelen voor een NSG die wordt gehost in elke regio dan het [ondersteunde regio's](#supported-regions), foutbericht 'Niet gevonden'. 

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

    De logboekanalyse (OMS)-werkruimte die als host fungeert voor de oplossing Traffic Analytics en het nsg's hoeven niet te zijn in dezelfde regio. Bijvoorbeeld, wellicht u Traffic Analytics in een werkruimte in de regio West-Europa, terwijl er nsg's in VS-Oost en VS-West. Meerdere nsg's kunnen worden geconfigureerd in dezelfde werkruimte.
6. Selecteer **Opslaan**.

    ![Verkeer analytics inschakelen](media/traffic-analytics/3.png)

Herhaal de vorige stappen voor alle andere nsg's waarvoor u wilt inschakelen van Traffic Analytics voor. Gegevens uit logboeken van de stroom is verzonden naar de werkruimte, dus zorg ervoor dat de lokale wet- en regelgeving in uw land toestaat dat de opslag van gegevens in de regio waar de werkruimte bestaat.

## <a name="view-traffic-analytics"></a>Analytische gegevens bekijken verkeer

Selecteer op de aan de linkerkant van de portal, **alle services**, voer dan *Monitor* in de **Filter** vak. Wanneer **Monitor** wordt weergegeven in de zoekresultaten, selecteer deze. Selecteer om te verkennen Traffic Analytics en de mogelijkheden ervan, **netwerk-watcher**, klikt u vervolgens **Traffic Analytics (Preview)**.

![Analytische gegevens bekijken verkeer](media/traffic-analytics/4.png)

Het dashboard kan de eerste keer wordt weergegeven omdat Traffic Analytics moet voldoende gegevens voor het afleiden van betekenisvolle inzichten voordat deze rapporten kunt genereren eerst samenvoegen tot 30 minuten duren.

## <a name="usage-scenarios"></a>Gebruiksscenario's

Sommige van de inzichten te krijgen nadat Traffic Analytics is volledig geconfigureerd, kunt u zijn als volgt:

### <a name="find-traffic-hotspots"></a>Verkeer hotspots vinden

**Zoeken**

- Welke hosts zijn verzenden of ontvangen van de meeste verkeer?
    - Inzicht in welke hosts zijn verzenden of ontvangen van het meeste verkeer, kunt u identificeren van de hosts die het meeste verkeer wordt verwerkt.
    - U kunt evalueren of het volume van verkeer dat geschikt is voor een host. De hoeveelheid verkeer normaal gedrag is of het verdient verder onderzoek?
- Hoeveel binnenkomend en uitgaand verkeer is?
    -   De host naar verwachting ontvangen meer binnenkomend verkeer dan uitgaande, of omgekeerd?
- De statistieken van verkeer toegestaan/geblokkeerd.
    - Waarom is een host toe te staan of een aanzienlijke hoeveelheid verkeer blokkeren?

    Selecteer **meer details**onder **Hosts met de meeste verkeer**, zoals wordt weergegeven in de volgende afbeelding:

    ![Verkeer analytics inschakelen](media/traffic-analytics/5.png)

- De volgende afbeelding ziet tijd trends voor de bovenste vijf sprekende hosts en de stroom-gerelateerde details (toegestane – binnenkomend en uitgaand en geweigerde - binnenkomend en uitgaand flows) voor een virtuele machine:

    ![](media/traffic-analytics/6.png)

**Zoeken**

- Zijn de meest conversing host paren?
    - Verwacht gedrag front-end-back-end communicatie of onregelmatig gedrag, zoals back-end-internetverkeer.
- De statistieken van verkeer toegestaan/geblokkeerd
    - Waarom een host is toe te staan of aanzienlijke netwerkverkeer blokkeren
- De meestgebruikte toepassingsprotocol tussen de meeste conversing host paren:
    - Zijn deze toepassingen toegestaan voor dit netwerk?
    - De toepassingen juist zijn geconfigureerd? Gebruik ze het juiste protocol voor communicatie? Selecteer **meer details** onder **meest frequent conversaties**, zoals weergegeven in de volgende afbeelding:

    ![](media/traffic-analytics/7.png)

- De volgende afbeelding ziet u tijd voor de bovenste vijf conversaties trends en de stroom-gerelateerde details, zoals toegestane en geweigerde items binnenkomende en uitgaande stromen voor een paar conversatie:

    ![](media/traffic-analytics/8.png)

**Zoeken**

- Welke toepassingsprotocol meest in uw omgeving wordt gebruikt, en welke conversing host paren het toepassingsprotocol het meest gebruikt?
    - Zijn deze toepassingen toegestaan voor dit netwerk?
    - De toepassingen juist zijn geconfigureerd? Gebruik ze het juiste protocol voor communicatie? Verwacht gedrag is een algemene poorten zoals 80 en 443. Voor communicatie gebruikt standaard, als een ongebruikelijke poorten worden weergegeven, ze mogelijk een configuratiewijziging. Selecteer **meer details** onder **Top toepassingsprotocollen**, in de volgende afbeelding:

    ![](media/traffic-analytics/9.png)

- De volgende afbeeldingen tijd weergeven voor de top vijf N7-protocollen en de stroom-gerelateerde details (bijvoorbeeld toegestaan en stromen geweigerd) voor een protocol N7 trends:

    ![](media/traffic-analytics/10.png)

    ![](media/traffic-analytics/11.png)

**Zoeken**

- Capaciteit gebruikstrends van een VPN-gateway in uw omgeving.
    - Elke SKU VPN kunt een bepaalde hoeveelheid bandbreedte. Zijn de VPN-gateways onderbenutte?
    - Zijn uw gateways capaciteit bereikt? Moet u upgraden naar de volgende hogere SKU?
- Zijn de meest conversing hosts, via welke VPN-gateway, via welke poort?
    - Is dit patroon normaal? Selecteer **meer details** onder **Top actieve VPN-verbindingen**, zoals wordt weergegeven in de volgende afbeelding:

    ![](media/traffic-analytics/12.png)

- De volgende afbeelding ziet tijd trends voor gebruik van de capaciteit van een Azure VPN-Gateway en de stroom-gerelateerde details (zoals toegestane stromen en poorten):

    ![](media/traffic-analytics/13.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Distributie van verkeer per Geografie visualiseren

**Zoeken**

- Distributie van verkeer per datacentrum zoals bovenste bronnen van verkeer naar een datacenter bovenste rogue netwerken conversatie met het datacenter en top conversatie toepassingsprotocollen.
    - Als u meer belasting van een datacenter ziet, kunt u plannen voor de distributie van verkeer efficiënt.
    - Als rogue netwerken zijn conversatie in het datacenter, klikt u vervolgens NSG-regels voor het blokkeren van ze te corrigeren.

    Selecteer **Klik hier voor live geomap** onder **distributie van verkeer tussen Azure-datacenters**, zoals wordt weergegeven in de volgende afbeelding:

    ![](media/traffic-analytics/14.png)

- De geo-kaart toont het bovenste lint voor de selectie van parameters zoals datacenters (Nee/geïmplementeerd-implementatie/niet-actief/actief/Traffic Analytics ingeschakeld/Traffic Analytics niet ingeschakeld) en landen Benign/kwaadaardig verkeer naar de actieve bijdragen implementatie:

    ![](media/traffic-analytics/15.png)

- De geo-kaart toont de distributie van verkeer naar een datacenter uit andere landen en continenten communicatie met deze in blauw (onschadelijk verkeer) en rood (schadelijk verkeer) gekleurd regels:

    ![](media/traffic-analytics/16.png)

    ![](media/traffic-analytics/17.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Distributie van verkeer te visualiseren door virtuele netwerken

**Zoeken**

- Distributie van verkeer per virtueel netwerk, topologie, bovenste bronnen van verkeer naar het virtuele netwerk conversatie met het virtuele netwerk en de conversatie toepassingsprotocollen boven hoogste rogue-netwerken.
    - Weten welke virtuele netwerk is conversatie met welke virtuele netwerk. Als de conversatie niet verwacht wordt, kan worden gecorrigeerd.
    - Als een rogue-netwerken zijn conversatie met een virtueel netwerk, kunt u NSG-regels voor het blokkeren van de netwerken rogue corrigeren.
 
    Selecteer **Klik hier voor VNet-verkeer topologie** onder **virtuele netwerk distributie**, zoals wordt weergegeven in de volgende afbeelding: 

        ![](media/traffic-analytics/18.png)

- De virtuele netwerktopologie toont het bovenste lint voor de keuze van de parameters zoals een virtueel netwerk van (Inter virtueel netwerk verbindingen/actief/inactief), externe verbindingen, actieve gegevensoverdrachten en schadelijke stromen van het virtuele netwerk.
- De virtuele netwerktopologie toont de distributie van verkeer met een virtueel netwerk met betrekking tot stromen (toegestaan/geblokkeerd/Inkomend/uitgaand/Benign/kwaadaardig), het toepassingsprotocol en netwerkbeveiligingsgroepen, bijvoorbeeld:

    ![](media/traffic-analytics/19.png)

    ![](media/traffic-analytics/20.png)

**Zoeken**

- Distributie van verkeer per subnet, topologie, bovenste bronnen van het verkeer naar het subnet van de bovenste rouge netwerken conversatie aan het subnet en top toepassingsprotocollen conversatie.
    - Weten welke subnet is conversatie met welk subnet. Als er onverwachte uitwisselingen, kunt u uw configuratie corrigeren.
    - Als rouge netwerken zijn conversatie met een subnet, bent u kunnen corrigeren door NSG regels voor het blokkeren van de netwerken rogue zo te configureren.
- De topologie subnetten bevat het bovenste lint voor selectie van parameters zoals actief/inactief subnet, externe verbindingen, actieve gegevensoverdrachten en schadelijke stromen van het subnet.
- De topologie Subnet toont de distributie van verkeer met een virtueel netwerk met betrekking tot stromen (toegestaan/geblokkeerd/Inkomend/uitgaand/Benign/kwaadaardig), het toepassingsprotocol en nsg's, bijvoorbeeld:

    ![](media/traffic-analytics/21.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Poorten en virtuele machines die zijn ontvangen van verkeer van internet weergeven

**Zoeken**

- Welke poorten open zijn conversatie via internet?
    - Als er onverwachte poorten open zijn gevonden, kunt u uw configuratie corrigeren:

        ![](media/traffic-analytics/22.png)

        ![](media/traffic-analytics/23.png)

**Zoeken**

Hebt u schadelijk verkeer in uw omgeving? Waar wordt deze die afkomstig zijn van? Waar wordt deze is bestemd voor?

![](media/traffic-analytics/24.png)


### <a name="visualize-the-trends-in-nsg-rule-hits"></a>De trends in het NSG regel treffers visualiseren

**Zoeken**

- Welke NSG/regel heeft de meeste treffers?
- Wat zijn de belangrijkste bron- en doelserver conversatie paren per NSG?

    ![](media/traffic-analytics/25.png)

- De volgende afbeeldingen tonen tijd trends voor treffers van NSG-regels en details van de bron-doel stroom voor een netwerkbeveiligingsgroep:

    ![](media/traffic-analytics/26.png)

    ![](media/traffic-analytics/27.png)