---
title: Prestatiemeter netwerk configureren voor Azure ExpressRoute-circuits (Preview) | Microsoft Docs
description: NPM configureren voor Azure ExpressRoute-circuits. (Preview)
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: cherylmc
ms.openlocfilehash: 6a03986288fdb6acaf234a8abf690f728d160fd7
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="configure-network-performance-monitor-for-expressroute-preview"></a>Configureren van netwerk-Prestatiemeter voor ExpressRoute (Preview)

Netwerk Performance Monitor (NPM) is een cloud-gebaseerde netwerk bewakingsoplossing, die wordt bewaakt connectiviteit tussen Azure-cloud-implementaties en lokale locaties (filialen, enzovoort). NPM maakt deel uit van Microsoft Operations Management Suite (OMS). NPM biedt nu een extensie voor ExpressRoute waarmee u de prestaties van het netwerk via ExpressRoute-circuits die zijn geconfigureerd voor gebruik van persoonlijke Peering bewaken. Wanneer u NPM voor ExpressRoute configureert, kunt u detecteren netwerkproblemen identificeren en oplossen.

U kunt:

* Bewaken en netwerklatentie voor verschillende VNets en waarschuwingen instellen

* Bewaken alle paden (inclusief redundante paden) op het netwerk

* Problemen met tijdelijke en punt in tijd netwerk die moeilijk te repliceren

* Bepalen van een bepaald segment in het netwerk die verantwoordelijk is voor verminderde prestaties

* Doorvoer per virtueel netwerk ophalen (als u agents zijn geïnstalleerd in elke VNet hebt)

* Zie de ExpressRoute-systeemstatus van een eerder punt in tijd

## <a name="regions"></a>Ondersteunde regio 's

U kunt ExpressRoute-circuits in elk deel van de hele wereld bewaken met behulp van een werkruimte die wordt gehost in een van de volgende gebieden:

* West-Europa 
* VS - oost 
* Zuidoost-Azië 

## <a name="workflow"></a>Werkstroom

Bewaking agents zijn geïnstalleerd op meerdere servers, zowel on-premises en in Azure. De agents met elkaar communiceren, maar geen gegevens verzenden, ze TCP-handshake pakketten verzenden. De communicatie tussen de agents kan Azure de netwerktopologie en pad van het verkeer kan worden toegewezen.

1. Maken van een werkruimte NPM in een van de [ondersteunde regio's](#regions).
2. Installeren en configureren van softwareagents: 
    * Installeer monitoring-agents op de on-premises servers en de Azure VM's.
    * Instellingen configureren op de monitoring agentservers om toe te staan van de bewaking agents om te communiceren. (Open firewall-poorten, enz.)
3. Configureren (NSG) netwerkbeveiligingsgroepen zodat de monitoring agent is geïnstalleerd op Azure Virtual machines om te communiceren met on-premises monitoring-agents.
4. Verzoek voor het goedgekeurde IP-adressen de NPM-werkruimte.
5. Bewaking ingesteld: Voer automatisch ontdekken en te beheren welke netwerken zichtbaar zijn in NPM.

Als u al met een netwerk Prestatiemeter voor het bewaken van andere objecten of -services en u al werkruimte in een van de ondersteunde regio's hebt, kunt u stap 1 en 2 van de stap overslaan en beginnen met de configuratie met stap 3.

## <a name="configure"></a>Stap 1: Een werkruimte maken

1. In de [Azure-portal](https://portal.azure.com), zoek in de lijst van services in de **Marketplace** voor 'Prestatiemeter netwerk'. Klik in het retourtype op om de **netwerk Prestatiemeter** pagina.

  ![portal](.\media\how-to-npm\3.png)<br><br>
2. Aan de onderkant van de belangrijkste **netwerk Prestatiemeter** pagina, klikt u op **maken** openen **netwerk-Prestatiemeter - nieuwe oplossing maken** pagina. Klik op **OMS-werkruimte - Selecteer een werkruimte** om de werkruimten pagina te openen. Klik op **+ maken nieuwe werkruimte** om de pagina van de werkruimte te openen.
3. Op de **OMS-werkruimte** pagina **nieuw** en configureer de volgende instellingen:

  * OMS-werkruimte - Typ een naam voor uw werkruimte.
  * Abonnement - als u meerdere abonnementen, kies de variant die u wilt koppelen aan de nieuwe werkruimte hebt.
  * Resourcegroep - een resourcegroep maken of gebruik een bestaande.
  * Locatie - moet u een [regio ondersteund](#regions).
  * Prijscategorie - Selecteer vrije
  
  >[!NOTE]
  >Het ExpressRoute-circuit kan worden, overal ter wereld en hoeft niet te worden in dezelfde regio bevinden als de werkruimte.
  >


  ![Werkruimte](.\media\how-to-npm\4.png)<br><br>
4. Klik op **OK** opslaan en de sjabloon instellingen implementeren. Nadat de sjabloon wordt gevalideerd, klikt u op **maken** voor het implementeren van de werkruimte.
5. Nadat u de werkruimte is geïmplementeerd, gaat u naar de **NetworkMonitoring(name)** resource die u hebt gemaakt. Controleer de instellingen en klik vervolgens op **oplossing is aanvullende configuratie vereist**.

  ![aanvullende configuratie](.\media\how-to-npm\5.png)
6. Op de **Welkom bij de Prestatiemeter netwerk** pagina **TCP gebruik van de synthetische transacties**, klikt u vervolgens op **indienen**. De TCP-transacties worden alleen gebruikt voor maken en de verbinding verbreken. Er zijn geen gegevens worden verzonden via deze TCP-verbindingen.

  ![TCP voor synthetische transacties](.\media\how-to-npm\6.png)

## <a name="agents"></a>Stap 2: Installeren en configureren van agents

### <a name="download"></a>2.1: download het installatiebestand van de agent

1. Op de **Performance Monitor netwerkconfiguratie - TCP-installatiepagina** voor uw resource in de **OMS-Agents installeren** sectie, klikt u op de agent die overeenkomt met de processor en het downloaden van de server de Setup-bestand.

  >[!NOTE]
  >De agent moet worden geïnstalleerd op een Windows Server (2008 SP1 of hoger). Bewaking van ExpressRoute-circuits met behulp van Windows Desktop-besturingssysteem- en Linux-besturingssysteem wordt niet ondersteund. 
  >
  >
2. Kopieer de **werkruimte-ID** en **primaire sleutel** naar Kladblok.
3. In de **Agents configureren** sectie, het downloaden van het Powershell-Script. Het PowerShell-script kunt u de relevante firewallpoort voor de TCP-transacties te openen.

  ![PowerShell-script](.\media\how-to-npm\7.png)

### <a name="installagent"></a>2.2: een monitoring agent installeren op elke server controleren

Het is raadzaam dat u ten minste twee agents voor elke zijde van de ExpressRoute-verbinding (dat wil zeggen, on-premises Azure VNETs) voor redundantie installeren. Gebruik de volgende stappen uit om agents te installeren:

1. Voer **Setup** de agent te installeren op elke server die u gebruiken wilt voor het bewaken van ExpressRoute. De server die u voor het bewaken van gebruikt een virtuele machine of on-premises kan zijn en toegang tot Internet moet hebben. U moet ten minste één agent on-premises, en een agent installeren op elk netwerksegment dat u wilt bewaken in Azure.
2. Op de **Welkom** pagina, klikt u op **volgende**.
3. Op de **licentievoorwaarden** pagina, lees de licentievoorwaarden en klik vervolgens op **ik ga akkoord**.
4. Op de **doelmap** pagina, wijzigen of de standaardinstallatiemap houden en klik vervolgens op **volgende**.
5. Op de **installatieopties voor Agent** pagina kunt u de agent verbinden met Azure logboekanalyse (OMS) of Operations Manager. Of u kunt de keuzes leeg laten als u wilt dat de agent later configureren. Nadat u uw selection(s), klikt u op **volgende**.

  * Als u wilt verbinding maken met **Azure logboekanalyse (OMS)**, plak de **werkruimte-ID** en **Werkruimtesleutel** (primaire sleutel) dat u hebt gekopieerd in Kladblok in de vorige sectie. Klik op **Volgende**.

    ![ID en -sleutel](.\media\how-to-npm\8.png)
  * Als u wilt verbinding maken met **Operations Manager**op de **Beheergroepconfiguratie** pagina, typt u de **Beheergroepsnaam**, **Management Server** , en de **Beheerserverpoort**. Klik op **Volgende**.

    ![Operations Manager](.\media\how-to-npm\9.png)
  * Op de **Agentactieacount** pagina de **lokaal systeem** -account of **domein of lokale computeraccount**. Klik op **Volgende**.

    ![Account](.\media\how-to-npm\10.png)
6. Op de **gereed voor installatie** pagina, Controleer uw selecties en klik vervolgens op **installeren**.
7. Op de **configuratie voltooid** pagina, klikt u op **voltooien**.
8. Als u klaar is Microsoft Monitoring Agent wordt weergegeven in het Configuratiescherm. U kunt uw configuratie er bekijken en controleren of de agent aan Operational Insights (OMS) is verbonden. Wanneer verbonden met OMS, de agent wordt weergegeven voor een bericht weergegeven: **de Microsoft Monitoring Agent verbonden is met de Microsoft Operations Management Suite-service**.

### <a name="proxy"></a>2.3: Configureer proxy-instellingen (optioneel)

Als u van een webproxy gebruikmaakt voor internettoegang, gebruikt u de volgende stappen uit voor het configureren van proxy-instellingen voor Microsoft Monitoring Agent. Voer deze stappen uit voor elke server. Als er veel servers zijn die u moet configureren, is het wellicht eenvoudiger om een script te gebruiken om dit proces te automatiseren. Zo ja, Zie [voor het configureren van proxy-instellingen voor Microsoft Monitoring Agent met een script](../log-analytics/log-analytics-windows-agent.md).

Proxy-instellingen voor Microsoft Monitoring Agent via het Configuratiescherm configureren:

1. Open de **Configuratiescherm**.
2. Open **Microsoft Monitoring Agent**.
3. Klik op het tabblad **Proxyinstellingen**.
4. Selecteer **een proxyserver gebruiken** en typ de URL en het poortnummer, als er een nodig is. Als er voor uw proxyserver verificatie is vereist, voert u de gebruikersnaam en het wachtwoord in om de proxyserver te openen.

  ![Proxy](.\media\how-to-npm\11.png)

### <a name="verifyagent"></a>2.4: Controleer of de agent-connectiviteit

Eenvoudig kunt u controleren of uw agents communiceren.

1. Open op een server met de bewakingsagent de **Configuratiescherm**.
2. Open de **Microsoft Monitoring Agent**.
3. Klik op de **Azure logboekanalyse (OMS)** tabblad.
4. In de **Status** kolom ziet u de agent verbonden is met de Operations Management Suite-service.

  ![status](.\media\how-to-npm\12.png)

### <a name="firewall"></a>2.5: open de firewallpoorten op de monitoring agentservers

Als u wilt gebruiken het TCP-protocol, moet u de firewall-poorten om ervoor te zorgen dat de bewaking agents kunnen communiceren openen.

U kunt een PowerShell-script waarmee de registersleutels vereist voor de Prestatiemeter voor netwerk, evenals de Windows Firewall-regels om toe te staan monitoring-agents voor het maken van TCP-verbindingen met elkaar maken uitvoeren. De registersleutels gemaakt door het script ook opgeven of om de logboeken voor foutopsporing en het pad voor het bestand logboeken te leggen. Het definieert ook de agent TCP-poort gebruikt voor communicatie. De waarden voor deze sleutels worden automatisch ingesteld door het script, zodat deze sleutels niet handmatig te wijzigen.

Poort 8084 wordt standaard geopend. U kunt een aangepaste poort gebruiken door de parameter 'portNumber' aan het script. Als u doet dit, moet u dezelfde poort voor alle servers waarop u het script uitvoert opgeven.

>[!NOTE]
>Het 'EnableRules' PowerShell-script configureert Windows Firewall-regels alleen op de server waarop het script wordt uitgevoerd. Als u een netwerkfirewall hebt, moet u ervoor zorgen dat verkeer dat is bestemd voor de TCP-poort wordt gebruikt door Netwerkcontrole prestaties kunnen.
>
>

Open een PowerShell-venster met beheerdersbevoegdheden op de agentservers. Voer de [EnableRules](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) PowerShell-script (die u eerder hebt gedownload). Gebruik geen parameters.

  ![PowerShell_Script](.\media\how-to-npm\script.png)

## <a name="opennsg"></a>Stap 3: Configureer netwerkbeveiligingsgroepen

Voor het controleren van agentservers die zich in Azure, moet u configureren (NSG) netwerkbeveiligingsgroepen voor TCP-verkeer op een poort wordt gebruikt door NPM van de synthetische transacties. De standaardpoort is 8084. Hierdoor kan een monitoring agent is geïnstalleerd op virtuele machine van Azure om te communiceren met een on-premises monitoring agent.

Zie voor meer informatie over het NSG [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-create-nsg-arm-portal.md).

## <a name="whitelist"></a>Stap 4: Aanvraag voor geaccepteerde werkruimte

>[!NOTE]
>Zorg ervoor dat u de agents (zowel de lokale server-agent en de Azure-server-agent) hebt geïnstalleerd en het PowerShell-script voordat u doorgaat met deze stap hebt uitgevoerd.
>
>

Voordat u kunt met behulp van de ExpressRoute-monitorfunctie van NPM, moet u vragen om uw werkruimte wilt plaatsen. [Klik hier om te gaan naar de pagina en vul het aanvraagformulier](https://aka.ms/npmcohort). (Hint: U kunt deze koppeling opent in een nieuw venster of tabblad). Het proces whitelisting kan een werkdag of langer duren. Zodra de whitelisting voltooid is, ontvangt u een e-mailbericht.

## <a name="setupmonitor"></a>Stap 5: NPM configureren voor het bewaken van ExpressRoute

>[!WARNING]
>Pas verder verder nadat uw werkruimte goedgekeurde lijst is en u een bevestiging per e-mail ontvangt.
>
>

Nadat u de vorige secties en controleer of dat u goedgekeurde lijst zijn, kunt u controle instellen.

1. Navigeer naar de overzichttegel Prestatiemeter netwerk door te gaan naar de **alle Resources** pagina en klik op de goedgekeurde lijst NPM-werkruimte.

  ![npm-werkruimte](.\media\how-to-npm\npm.png)
2. Klik op de **netwerk Prestatiemeter** overzichttegel online zetten van het dashboard. Het dashboard bevat een pagina met ExpressRoute, waaruit blijkt dat ExpressRoute een niet-geconfigureerde status is'. Klik op **functie Setup** de configuratiepagina van de netwerk-Prestatiemeter te openen.

  ![functie-installatie](.\media\how-to-npm\npm2.png)
3. Navigeer naar de op het tabblad ExpressRoute-Peerings in het deelvenster links op de configuratiepagina. Klik op **nu detecteren**.

  ![detecteren](.\media\how-to-npm\13.png)
4. Wanneer detectie is voltooid, ziet u regels voor unieke naam van het Circuit en VNet-naam. In eerste instantie zijn deze regels uitgeschakeld. Schakelt u de regels en selecteer vervolgens de controle agents en drempelwaarden.

  ![regels](.\media\how-to-npm\14.png)
5. Na het inschakelen van de regels en selecteren van de waarden en de agents die u wilt bewaken, is een wachttijd van ongeveer 30 tot 60 minuten voor de waarden om te beginnen in te vullen en de **ExpressRoute bewaking** tegels beschikbaar. Zodra u de bewaking tegels ziet, zijn uw ExpressRoute-circuits en verbindingsbronnen worden bewaakt door NPM.

  ![tegels bewaking](.\media\how-to-npm\15.png)

## <a name="explore"></a>Stap 6: Weergave tegels bewaking

### <a name="dashboard"></a>Pagina van de Prestatiemeter netwerk

De NPM-pagina bevat een pagina voor ExpressRoute die een overzicht van de status van het ExpressRoute-circuits en -peerings geeft.

  ![Dashboard](.\media\how-to-npm\dashboard.png)

### <a name="circuits"></a>Circuits lijst

Een lijst van alle bewaakte ExpressRoute-circuits wilt bekijken, klikt u op de **ExpressRoute-circuits** tegel. U kunt een circuit selecteren en weergeven van de status, trendgrafieken voor pakketverlies, bandbreedte en latentie. De grafieken, zijn interactief. U kunt een aangepaste tijdvenster voor het uitzetten van de grafieken selecteren. Sleep de muis over een gebied op de grafiek inzoomen en fijnmazig gegevenspunten bekijken.

  ![circuit_list](.\media\how-to-npm\circuits.png)

#### <a name="trend"></a>Trend van gegevensverlies, latentie en doorvoer

De grafieken bandbreedte, latentie en gegevensverlies, zijn interactief. U kunt inzoomen in een willekeurig gedeelte van deze grafieken met behulp van besturingselementen van de muis. U ziet ook de bandbreedte, latentie en verlies van gegevens voor andere intervallen door te klikken op **datum/tijd**, dat zich bevindt onder de knop acties in de linkerbovenhoek.

![trend](.\media\how-to-npm\16.png)

### <a name="peerings"></a>Lijst van Peerings

Klik op de **persoonlijke Peerings** tegel op het dashboard wordt een lijst weergegeven van alle verbindingen met virtuele netwerken via persoonlijke peering. Hier kunt u een virtuele netwerkverbinding en bekijk de status, trendgrafieken voor pakketverlies, bandbreedte en latentie.

  ![lijst van circuit](.\media\how-to-npm\peerings.png)

### <a name="topology"></a>Circuit-topologie

Voor het circuit topologie weergeven, klikt u op de **topologie** tegel. Hiermee gaat u naar de Topologieweergave van de geselecteerde circuit of peering. Diagram van de topologie biedt de latentie voor elk segment in het netwerk en elke laag 3-hop wordt vertegenwoordigd door een knooppunt van het diagram. Op een hop te klikken, geeft meer informatie over de hop.
U kunt het niveau van zichtbaarheid lokale hops opnemen door de onderstaande schuifknop te verplaatsen verhogen **Filters**. De schuifregelaar naar links of rechts verplaatsen, verhoogt/verlaagt het aantal hops in de grafiek topologie. De latentie tussen elk segment is zichtbaar, waardoor voor snellere isolatie van hoge latentie segmenten in uw netwerk.

![filters](.\media\how-to-npm\topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>Gedetailleerde weergave van de topologie van een circuit

Deze weergave toont de VNet-verbindingen.
![gedetailleerde topologie](.\media\how-to-npm\17.png)
