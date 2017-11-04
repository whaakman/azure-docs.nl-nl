---
title: Analyse van Azure Service Fabric-gebeurtenis met OMS | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met OMS voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: f0cefab15a115719ea9c378546a7e6004bd06187
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Analyse van gebeurtenis en visualisatie met OMS

Operations Management Suite (OMS) is een verzameling van management-services die u bij de controle en diagnostische gegevens voor toepassingen helpen en services die worden gehost in de cloud. Als u een meer gedetailleerd overzicht van OMS en wat biedt, lezen [wat OMS is?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-the-oms-workspace"></a>Log Analytics en de OMS-werkruimte

Log Analytics verzamelt gegevens van beheerde bronnen, met inbegrip van de tabel van een Azure-opslag of een agent en onderhoudt dat bij een centrale opslagplaats. De gegevens kunnen vervolgens worden gebruikt voor analyse, waarschuwingen en visualisatie of meer geëxporteerd. Log Analytics ondersteunt gebeurtenissen, prestatiegegevens of andere aangepaste gegevens.

Wanneer OMS is geconfigureerd, hebt u toegang tot een specifieke *OMS-werkruimte*, uit waarbij gegevens kunnen worden opgevraagd of weergegeven in dashboards.

Nadat de gegevens worden ontvangen door logboekanalyse OMS heeft diverse *beheeroplossingen* die voorverpakte oplossingen voor het bewaken van binnenkomende gegevens, aangepast aan verschillende scenario's zijn. Deze omvatten een *Service Fabric Analytics* oplossing en een *Containers* oplossing, zijn de twee meest relevante diagnostische gegevens en controleren wanneer met behulp van Service Fabric-clusters. Er zijn verschillende evenals met waard en OMS biedt tevens mogelijkheden voor het maken van aangepaste oplossingen die u kunt meer lezen over [hier](../operations-management-suite/operations-management-suite-solutions.md). Elke oplossing die u wilt gebruiken voor een cluster kan worden geconfigureerd in de dezelfde OMS-werkruimte, samen met logboekanalyse. Werkruimten kunnen u aangepaste dashboards en visualisatie van gegevens en wijzigingen in de gegevens die u wilt verzamelen, te verwerken en analyseren.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-analytics-solution"></a>Instellen van een OMS-werkruimte met de Service Fabric Analytics-oplossing
Het is raadzaam dat u de Service Fabric-oplossing in uw werkruimte OMS opnemen-bevat een dashboard dat toont de verschillende binnenkomende logboekkanalen van het platform en toepassing niveau en biedt de mogelijkheid om query Service Fabric in specifieke Logboeken. Hier ziet u hoe een relatief eenvoudige oplossing voor Service Fabric uitziet, met één toepassing geïmplementeerd op het cluster:

![OMS SF-oplossing](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Er zijn twee manieren om te richten en te configureren van een OMS-werkruimte, via een Resource Manager-sjabloon of rechtstreeks uit Azure Marketplace. Gebruik de voormalige wanneer u een cluster implementeert en de laatste als u al een cluster dat is geïmplementeerd met diagnostische gegevens hebt ingeschakeld.

### <a name="deploying-oms-using-a-resource-management-template"></a>OMS met een Resource Management-sjabloon implementeren

Bij het implementeren van een cluster met behulp van een Resource Manager-sjabloon, het Service Fabric-oplossing aan de sjabloon kunt ook een nieuwe OMS-werkruimte maken toevoegen en configureren voor het lezen van gegevens uit de tabellen met de juiste opslag.

>[!NOTE]
>Dit werkt, diagnostische gegevens moet worden ingeschakeld om de Azure storage-tabellen bestaan voor OMS / Log Analytics om informatie in uit te lezen.

[Hier](https://azure.microsoft.com/resources/templates/service-fabric-oms/) is een voorbeeldsjabloon die u kunt gebruiken en aanpassen aan de hand van behoefte die wordt uitgevoerd boven acties. In het geval dat u meer optionaliteit wilt, er enkele meer sjablonen waarmee u verschillende opties zijn, afhankelijk van waar in het proces mogelijk voor het instellen van een OMS-werkruimte: kan worden gevonden op [Service Fabric en OMS sjablonen](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Implementatie met behulp van OMS via Azure Marketplace

Als u liever een OMS-werkruimte toevoegen nadat u een cluster hebt geïmplementeerd, Ga naar Azure Marketplace en zoekt u *'Service Fabric Analytics'*.

![OMS SF analyses in de Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

* Klik op **maken**
* Klik in het venster van het maken van Service Fabric Analytics **Selecteer een werkruimte** voor de *OMS-werkruimte* veld en vervolgens **Maak een nieuwe werkruimte**. Vul de vereiste vermeldingen - hier de enige vereiste is dat het abonnement voor de Service Fabric-cluster en de OMS-werkruimte moet gelijk zijn. Zodra uw vermeldingen zijn geverifieerd, wordt de OMS-werkruimte gaat implementeren. Dit moet alleen een paar minuten duren.
* Wanneer u klaar bent, klikt u op **maken** opnieuw op de onderkant van het Service Fabric Analytics-venster maken. Zorg ervoor dat de nieuwe werkruimte wordt weergegeven onder *OMS-werkruimte*. Hierdoor wordt de oplossing toegevoegd aan de werkruimte die u zojuist hebt gemaakt.


Hoewel de oplossing wordt toegevoegd aan de werkruimte, moet de werkruimte nog worden verbonden met de diagnostics-gegevens die afkomstig zijn van uw cluster. Navigeer naar de resourcegroep die u hebt gemaakt met de Service Fabric Analytics-oplossing in. U ziet een *ServiceFabric (\<nameOfOMSWorkspace\>)*.

* Klik op de oplossing om te navigeren naar de overzichtspagina van waar u Oplossingsinstellingen, werkruimte-instellingen wijzigen en navigeer naar de OMS-portal.
* Klik in het navigatiemenu links op **opslagaccounts logboeken**onder *werkruimte gegevensbronnen*.

    ![Service Fabric Analytics-oplossing in de Portal](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-portal.png)

* Op de *Storage account logboeken* pagina, klikt u op **toevoegen** aan de bovenkant van uw cluster logboeken toevoegen aan de werkruimte.
* Klik op **opslagaccount** de juiste account gemaakt in het cluster toe te voegen. Als u de standaardnaam gebruikt, het storage-account heet *sfdg\<resourceGroupName\>*. U kunt dit ook bevestigen door het controleren van de Azure Resource Manager-sjabloon gebruikt voor het implementeren van uw cluster, door de waarde die wordt gebruikt voor de `applicationDiagnosticsStorageAccountName`. Misschien hebt u ook Schuif naar beneden en klik op **laden meer** als de accountnaam wordt niet weergegeven. Klik op de juiste opslagaccountnaam wanneer deze wordt weergegeven om deze te selecteren.
* Vervolgens hebt u om op te geven de *gegevenstype*, die moet worden **gebeurtenissen van de Service Fabric**.
* De *bron* automatisch moet worden ingesteld op *WADServiceFabric\*EventTable*.
* Klik op **OK** verbinding maken met uw werkruimte logboeken voor uw cluster.

    ![Logboeken voor storage-account toevoegen aan OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

* Het account moet nu worden weergegeven als onderdeel van uw *Storage account logboeken* in uw werkruimte-gegevensbronnen.

U hebt nu de Service Fabric Analytics-oplossing in een logboekanalyse OMS-werkruimte nu goed met uw cluster-platform en de logboektabel van toepassing verbonden is met dit toegevoegd. U kunt aanvullende bronnen toevoegen aan de werkruimte op dezelfde manier.

## <a name="using-the-oms-agent"></a>Met behulp van de OMS-Agent

Het is aanbevolen en te gebruiken EventFlow af als aggregatieoplossingen omdat ze is toegestaan voor een meer modulaire benadering voor diagnostische gegevens en bewaking. Bijvoorbeeld: als u wilt wijzigen van de uitvoer van EventFlow, heeft deze geen wijziging in uw werkelijke instrumentation, alleen een eenvoudige wijziging aan het configuratiebestand. Als u echter besluit te investeren in met behulp van OMS en bereid bent te blijven gebruiken voor analyse van gebeurtenis (hoeft niet te zijn de enige platform dat u gebruikt in plaats dat deze worden echter ten minste één van de platforms), het is raadzaam dat u zou ook een instelling van de [OMS ag ent](../log-analytics/log-analytics-windows-agents.md). U moet ook de OMS-agent gebruiken bij het implementeren van containers met uw cluster, zoals hieronder beschreven.

Het proces om dit te doen is betrekkelijk eenvoudig, omdat u zojuist hebt voor de agent niet toevoegen als een virtuele-machineschaalset uitbreiding naar de Resource Manager-sjabloon instellen waarbij u ervoor zorgt dat deze wordt geïnstalleerd op elk van de knooppunten. Een voorbeeld Resource Manager-sjabloon die de OMS-werkruimte met de Service Fabric-oplossing (Zie boven) implementeert en de agent toegevoegd aan uw knooppunten gevonden voor clusters met [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) of [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

De voordelen hiervan zijn de volgende:

* Rijkere gegevens op de prestaties tellers en metrische gegevens aan clientzijde
* Eenvoudig te configureren van de metrische gegevens worden verzameld uit het cluster en zonder de configuratie van het cluster bij te werken. Wijzigingen in de instellingen van de agent kunnen worden gedaan vanuit de OMS-portal en de agent wordt automatisch opnieuw opgestart zodat deze overeenkomen met de vereiste configuratie. Ga naar de werkruimte voor het configureren van de OMS-agent om op te halen specifieke prestatiemeteritems **Start > Instellingen > gegevens > Windows-prestatiemeteritems** en selecteer de gegevens die u zou willen zien die worden verzameld
* Gegevens weergegeven sneller dan het hoeft te worden opgeslagen voordat deze wordt opgehaald door OMS / Log Analytics
* Bewaking van containers is veel eenvoudiger, omdat deze docker-Logboeken (stdout, stderr) en statistieken (maatstaven voor prestaties op knooppunt en container niveaus ophalen kan)

De belangrijkste overweging hier is of omdat de agent op uw cluster naast al uw toepassingen worden geïmplementeerd, kan er enige impact op de prestaties van uw toepassingen op het cluster.

## <a name="monitoring-containers"></a>Bewaking van Containers

Bij het implementeren van containers naar een Service Fabric-cluster, wordt het aanbevolen dat het cluster is ingesteld met de OMS-agent en dat de oplossing Containers is toegevoegd aan de OMS-werkruimte bewaking wilt inschakelen en diagnostische gegevens. Hier ziet u hoe de oplossing containers in een werkruimte uitziet:

![Basic OMS-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

De agent kunt het verzamelen van verschillende container-specifieke logboeken die kan worden doorzocht in OMS of gebruikt voor het gevisualiseerde prestatie-indicatoren. De logboek-typen die zijn verzameld zijn:

* ContainerInventory: geeft informatie over de containerlocatie, naam en installatiekopieën
* ContainerImageInventory: informatie over geïmplementeerde installatiekopieën, met inbegrip van id's of grootten
* ContainerLog: specifieke foutenlogboeken, docker-Logboeken (stdout, enzovoort) en andere items
* ContainerServiceLog: docker-daemon opdrachten die zijn uitgevoerd
* Prestaties: prestatiemeteritems met inbegrip van container cpu, geheugen, netwerkverkeer, schijf-i/o en aangepaste metrische gegevens van de hostmachines

In dit artikel bevat informatie over de stappen die nodig zijn voor het instellen van de bewaking van de container voor uw cluster. Bekijk voor meer informatie over de OMS-Containers oplossing hun [documentatie](../log-analytics/log-analytics-containers.md).

Als u de Container-oplossing in uw werkruimte instelt, moet u de OMS-agent die is geïmplementeerd op knooppunten in het cluster met de bovengenoemde stappen te volgen. Zodra het cluster gereed is, implementeert u een container voor het. Vergeet dat de eerste keer dat de installatiekopie van een container wordt geïmplementeerd naar een cluster duurt enkele minuten om de installatiekopie, afhankelijk van de grootte te downloaden.

Zoek in Azure Marketplace voor *Container bewaking oplossing* en maak de **Container bewaking oplossing** resultaat die moet worden geleverd, onder de bewaking en beheer categorie.

![Containers oplossing toevoegen](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

In de stap het maken van aanvraagt het een OMS-werkruimte. Selecteer de versie die is gemaakt met de bovenstaande implementatie. Deze stap voegt een oplossing Containers in de OMS-werkruimte en wordt automatisch gedetecteerd door de OMS-agent die is geïmplementeerd door de sjabloon. De agent wordt gestart met het verzamelen van gegevens over de containers processen in het cluster en minder dan 15 minuten of geval is, ziet u de oplossing lichte up met gegevens, zoals in de installatiekopie van het bovenstaande dashboard.


## <a name="next-steps"></a>Volgende stappen

Bekijk de volgende OMS-hulpprogramma's en opties voor het aanpassen van een werkruimte aan uw behoeften:

* OMS biedt voor lokale clusters, een Gateway (http-doorsturen Proxy) die kan worden gebruikt om gegevens te verzenden aan OMS. Lees meer over die in [computers zonder toegang tot het Internet verbinden met OMS met behulp van de OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md)
* Configureren van OMS om in te stellen [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) om te helpen detecteren en diagnostische gegevens
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse