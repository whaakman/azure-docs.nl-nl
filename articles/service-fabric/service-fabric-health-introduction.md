---
title: Statuscontrole in Service Fabric | Microsoft Docs
description: Een inleiding tot de Azure Service Fabric-model, deze biedt bewaking van het cluster en de toepassingen en services voor statuscontrole.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: ''
ms.assetid: 1d979210-b1eb-4022-be24-799fd9d8e003
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: 6cba4e1fd9c9fe5fdaa7ff4513218a606a4eace9
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215227"
---
# <a name="introduction-to-service-fabric-health-monitoring"></a>Inleiding tot de statuscontrole Service Fabric
Azure Service Fabric introduceert een statusmodel dat u uitgebreide, flexibele en uitbreidbare evalueren en rapportage kunt. Het model kunt bijna-realtime bewaking van de status van het cluster en de services die erin worden uitgevoerd. U kunt eenvoudig ophalen van gegevens over de servicestatus en corrigeren mogelijke problemen voordat ze zich opstapelen en enorme storingen veroorzaken. In het model typische services rapporten op basis van hun lokale weergaven verzenden, en dat gegevens worden samengevoegd voor een algemene-niveau cluster weergeven.

Dit model uitgebreide health service Fabric-onderdelen gebruiken voor het rapporteren van de huidige status. U kunt hetzelfde mechanisme gebruiken tot de status van het rapport uit uw toepassingen. Als u investeert in de gezondheid van hoge kwaliteit reporting waarmee uw aangepaste voorwaarden wordt vastgelegd, kunt u detecteren en oplossen van problemen gemakkelijker voor de toepassing wordt uitgevoerd.

De volgende video van Microsoft Virtual Academy beschrijft ook het statusmodel van de Service Fabric en hoe deze worden gebruikt: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-health-introduction/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

> [!NOTE]
> We begonnen met het subsysteem health om nodig voor bewaakte upgrades op te lossen. Service Fabric biedt bewaakte upgrades van toepassing en cluster die ervoor zorgen dat de volledige beschikbaarheid, zonder uitvaltijd en minimale aan geen tussenkomst van de gebruiker. Als u wilt deze doelen kan bereiken, controleert de upgrade status op basis van geconfigureerde upgrade beleid. Een upgrade kan worden voortgezet wanneer health respecteert de gewenste drempelwaarden. Anders wordt is de upgrade automatisch teruggedraaid of onderbroken zodat beheerders een kans de problemen op te lossen. Zie voor meer informatie over upgrades van toepassingen, [in dit artikel](service-fabric-application-upgrade.md).
> 
> 

## <a name="health-store"></a>Health store
De health store blijft health-gerelateerde informatie over entiteiten in het cluster voor eenvoudig op te halen en de evaluatie. Deze is geïmplementeerd als een Service Fabric persistent stateful service om ervoor te zorgen voor hoge beschikbaarheid en schaalbaarheid. De health store maakt deel uit van de **fabric: / System** toepassing, en is beschikbaar wanneer het cluster actief is en worden uitgevoerd.

## <a name="health-entities-and-hierarchy"></a>De gezondheid van entiteiten en de hiërarchie
De health-entiteiten zijn ingedeeld in een logische hiërarchie waarmee interacties en afhankelijkheden tussen verschillende entiteiten wordt vastgelegd. De health store worden automatisch de statusentiteiten en hiërarchie op basis van rapporten die zijn ontvangen van Service Fabric-onderdelen bouwt.

De health-entiteiten mirror van de Service Fabric-entiteiten. (Bijvoorbeeld **health Toepassingsentiteit** komt overeen met een exemplaar van de toepassing geïmplementeerd in het cluster, terwijl **health knooppunt entiteit** komt overeen met het knooppunt van een Service Fabric-cluster.) De health-hiërarchie legt de interactie van het systeementiteiten en het is de basis voor geavanceerde evalueren. U kunt meer informatie over belangrijke concepten voor Service Fabric in [technisch overzicht van Service Fabric](service-fabric-technical-overview.md). Zie voor meer informatie over de toepassing [Service Fabric-toepassingsmodel](service-fabric-application-model.md).

De health-entiteiten en de hiërarchie kunnen het cluster en de toepassingen worden effectief gerapporteerd, foutopsporing en bewaakt. Het statusmodel biedt een nauwkeurige *gedetailleerde* weergave van de status van de veel bewegende onderdelen in het cluster.

![De gezondheid van entiteiten.][1]
De health-entiteiten, ingedeeld in een hiërarchie op basis van de relaties tussen bovenliggende en onderliggende.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

De health-entiteiten zijn:

* **Cluster**. Hiermee geeft u de status van een Service Fabric-cluster. Statusrapporten cluster beschrijven voorwaarden die invloed hebben op het hele cluster. Deze voorwaarden van invloed zijn op meerdere entiteiten in het cluster of het cluster zelf. Op basis van de voorwaarde, beperken niet de journalist het probleem op een of meer beschadigde onderliggende. Voorbeelden zijn onder meer het brein van het cluster door netwerkproblemen partitioneren of communicatie te splitsen.
* **Knooppunt**. Hiermee geeft u de status van een Service Fabric-knooppunt. Knooppunt statusrapporten beschrijven voorwaarden die van invloed op de functionaliteit van het knooppunt. Ze doorgaans invloed op de geïmplementeerde entiteiten die erop worden uitgevoerd. Voorbeelden zijn onder meer onvoldoende schijfruimte-knooppunt (of andere eigenschappen van alle computers, zoals geheugen, verbindingen) en wanneer een knooppunt niet actief is. De entiteit knooppunt wordt aangeduid met de naam van het knooppunt (tekenreeks).
* **Toepassing**. Hiermee geeft u de status van het exemplaar van een toepassing die wordt uitgevoerd in het cluster. Statusrapporten van de toepassing beschrijven voorwaarden die invloed hebben op de algemene status van de toepassing. Ze kunnen niet worden verkleind naar afzonderlijke items (services of toepassingen). Voorbeelden zijn de end-to-end-interactie tussen verschillende services in de toepassing. De Toepassingsentiteit wordt aangeduid met de naam van de toepassing (URI).
* **Service**. Hiermee geeft u de status van een service die wordt uitgevoerd in het cluster. Statusrapporten van service omschrijven voorwaarden die invloed hebben op de algemene status van de service. Het probleem met een beschadigde partitie of replica kan niet de journalist beperken. Voorbeelden zijn onder meer een serviceconfiguratie (zoals poort of een externe bestandsshare) die wordt veroorzaakt door problemen voor alle partities. De service-entiteit wordt aangeduid met de naam van de service (URI).
* **Partitie**. Hiermee geeft u de status van een servicepartitie. Partitie statusrapporten beschrijven voorwaarden die invloed hebben op de hele replicaset. Voorbeelden zijn wanneer het aantal replica's lager dan het aantal doel is en wanneer een partitie sprake van quorumverlies is. De partitie-entiteit wordt aangeduid met de partitie-ID (GUID).
* **Replica**. Hiermee geeft u de status van een stateful service-replica of een stateless service-exemplaar. De replica is de kleinste eenheid die watchdogs en onderdelen van het systeem kunnen worden weergegeven voor een toepassing. Voorbeelden zijn voor stateful services, een primaire replica die bewerkingen tot de secundaire replica's en trage replicatie kan niet worden gerepliceerd. Een stateless exemplaar kan ook rapporteren wanneer deze is onvoldoende resources of problemen met de netwerkverbinding heeft. De replica-entiteit wordt aangeduid met de partitie-ID (GUID) en de replica of het exemplaar-ID (lang).
* **DeployedApplication**. Hiermee geeft u de status van een *toepassing die wordt uitgevoerd op een knooppunt*. Statusrapporten van gedistribueerde toepassing beschrijven voorwaarden die specifiek zijn voor de toepassing op het knooppunt dat naar service-pakketten die zijn geïmplementeerd op hetzelfde knooppunt kan niet worden verkleind. Voorbeelden zijn fouten bij het toepassingspakket kan niet worden gedownload op dat knooppunt en problemen met het instellen van beveiligings-principals van toepassing op het knooppunt. De geïmplementeerde toepassing wordt aangeduid met de naam van de toepassing (URI) en de naam van knooppunt (tekenreeks).
* **DeployedServicePackage**. Hiermee geeft u de status van een servicepakket op een knooppunt in het cluster uitgevoerd. Hierin wordt beschreven specifiek is voor een servicepakket voorwaarden die niet van invloed op de andere servicepakketten op hetzelfde knooppunt voor dezelfde toepassing. Voorbeelden zijn een codepakket in het servicepakket dat kan niet worden gestart en een configuratiepakket die niet kan worden gelezen. De geïmplementeerde service-pakket wordt aangeduid met de toepassingsnaam (URI), naam van het knooppunt (tekenreeks), manifest servicenaam (tekenreeks) en service-pakket activerings-ID (tekenreeks).

De granulatie van het statusmodel kunt u eenvoudig is om te detecteren en oplossen van problemen. Bijvoorbeeld, als een service niet reageert, is het mogelijk om te rapporteren dat exemplaar van de toepassing niet in orde is. Rapportage op dat niveau niet ideaal zijn, maar is omdat het probleem kan niet worden die betrekking hebben op de services binnen die toepassing. Het rapport moet worden toegepast met de service niet in orde of naar een specifieke onderliggende partitie, als u meer informatie naar de betreffende partitie verwijst. De gegevens automatisch oppervlakken via de hiërarchie en een beschadigde partitie zichtbaar op het niveau van service en toepassing. Met deze aggregatie helpt bij het identificeren en de hoofdoorzaak van het probleem sneller oplossen.

De health-hiërarchie is samengesteld uit de relaties tussen bovenliggende en onderliggende. Een cluster bestaat uit knooppunten en toepassingen. Toepassingen services en geïmplementeerde toepassingen. Geïmplementeerde toepassingen hebt servicepakketten geïmplementeerd. Services partities hebben en elke partitie heeft een of meer replica's. Er is een speciale relatie tussen de knooppunten en geïmplementeerde entiteiten. Een beschadigd knooppunt zoals gemeld door onderdeel van de instantie, de Failover Manager-service, is van invloed op de geïmplementeerde toepassingen, service-pakketten en geïmplementeerd op deze replica's.

De health-hiërarchie staat voor de meest recente status van het systeem op basis van de meest recente statusrapporten dat is bijna real-time informatie.
Interne en externe watchdogs kunnen rapporteren dat op de dezelfde entiteiten op basis van toepassingsspecifieke logica of aangepaste bewaakte voorwaarden. Gebruikersrapporten worden gecombineerd met de systeemrapporten.

Plan te investeren in het rapport en reageren op status tijdens het ontwerpen van een grote cloudservice. Deze vooraf investement vergemakkelijkt de service om foutopsporing, bewaken en beheren.

## <a name="health-states"></a>Statussen
Service Fabric maakt gebruik van drie statussen voor het beschrijven van of een entiteit in orde of niet is: OK, waarschuwingen en fouten. Een rapport wordt verzonden naar de store health moet een van deze statussen opgeven. Het resultaat van evaluatie van health is een van deze statussen.

De mogelijke [statussen](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) zijn:

* **OK**. De entiteit is in orde. Er zijn geen bekende problemen die worden gerapporteerd of de onderliggende objecten (indien van toepassing).
* **Waarschuwing**. De entiteit heeft enkele problemen, maar deze nog steeds correct kan werken. Bijvoorbeeld, er vertragingen zijn, maar ze zorgen niet functioneel problemen nog. In sommige gevallen kan de waarschuwingsvoorwaarde zelf oplossen zonder tussenkomst van de externe. In dergelijke gevallen statusrapporten bewustzijn en bieden inzicht in wat er gebeurt. In andere gevallen kan de waarschuwingsvoorwaarde afnemen in een ernstig probleem zonder tussenkomst van de gebruiker.
* **Fout**. De entiteit is beschadigd. Actie moet worden genomen om op te lossen van de status van de entiteit, omdat deze niet naar behoren.
* **Onbekende**. De entiteit bestaat niet in de health-store. Dit resultaat kan worden verkregen van de gedistribueerde query's die worden samengevoegd resultaten uit meerdere onderdelen. Bijvoorbeeld, de lijstquery voor get-knooppunt wordt gerouteerd naar **FailoverManager**, **ClusterManager**, en **HealthManager**; toepassing lijstquery gaat naar  **ClusterManager** en **HealthManager**. Deze query's samenvoegen resultaten uit meerdere onderdelen van het systeem. Als onderdeel van een andere resulteert in een entiteit die niet aanwezig zijn in health store, het samengevoegde resultaat heeft een onbekende status. Een entiteit is niet in de store omdat statusrapporten nog niet zijn verwerkt of de entiteit zijn opgeschoond na verwijdering.

## <a name="health-policies"></a>Statusbeleid
De health store geldt statusbeleid om te bepalen of een entiteit in orde op basis van de rapporten en onderliggende items.

> [!NOTE]
> Beleid kunnen worden opgegeven in het clustermanifest (voor cluster- en knooppunt evalueren) of in het toepassingsmanifest (voor toepassingsevaluatie en alle onderliggende items). Status evaluatie aanvragen kunnen ook doorgeven in aangepaste evaluatie statusbeleid, die alleen voor die evaluatie worden gebruikt.
> 
> 

Service Fabric geldt standaard strikte regels (Alles moet zich in orde) voor de hiërarchische relatie bovenliggend-onderliggend. Als een van de onderliggende objecten een slechte gebeurtenis heeft, wordt de bovenliggende als slecht beschouwd.

### <a name="cluster-health-policy"></a>Beleid voor cluster-status
De [cluster statusbeleid](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) wordt gebruikt voor het evalueren van de clusterstatus en knooppunt-statussen. Het beleid kan worden gedefinieerd in het clustermanifest. Als dit niet aanwezig is, wordt het standaardbeleid (nul verdragen fouten) gebruikt.
Het statusbeleid cluster bevat:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Hiermee geeft u op of voor het behandelen van waarschuwing status als fouten tijdens de evaluatie van de status rapporteert. Standaard: false.
* [MaxPercentUnhealthyApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Hiermee geeft u het maximumpercentage verdragen van toepassingen die niet in orde zijn mag voordat het cluster wordt beschouwd als fout.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Hiermee geeft u het maximumpercentage verdragen van knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. In grote clusters sommige knooppunten zijn altijd in of uit voor herstellingen, zodat dit percentage moet worden geconfigureerd om te tolereren die.
* [ApplicationTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). Het toepassingsoverzicht type health beleid kan worden gebruikt tijdens de evaluatie van cluster health om te beschrijven van speciale typen. Standaard worden alle toepassingen in een pool plaatsen en geëvalueerd met MaxPercentUnhealthyApplications. Als sommige toepassingtypen anders moeten worden behandeld, kunnen ze worden uitgevoerd buiten de globale groep. In plaats daarvan worden deze geëvalueerd op basis van de percentages die zijn gekoppeld aan de naam van het toepassingstype op de kaart. Bijvoorbeeld in een cluster zijn er duizenden toepassingen van verschillende typen en een paar exemplaren van een besturingselement toepassing van een speciale toepassingstype. De toepassingen moeten geen fout zijn. U kunt globale MaxPercentUnhealthyApplications 20% te tolereren fouten opgetreden, maar dan voor het type 'ControlApplicationType' de MaxPercentUnhealthyApplications ingesteld op 0. Op deze manier als enkele van de vele toepassingen niet in orde zijn, maar onder het globale niet in orde percentage uitkomen, zou het cluster worden geëvalueerd voor de waarschuwing. Een waarschuwingsstatus heeft geen invloed op het cluster upgraden of andere bewakings geactiveerd op basis van de status fout. Maar zelfs één besturingselement toepassing fout zouden cluster niet in orde, die wordt geactiveerd terugdraaien of onderbreken van de upgrade van het cluster, afhankelijk van de configuratie van de upgrade.
  Bij de toepassingstypen die zijn gedefinieerd in de kaart, worden alle exemplaren van een toepassing uitgevoerd buiten de algemene groep van toepassingen. Ze worden geëvalueerd op basis van het totale aantal aanvragen van het toepassingstype, met behulp van de specifieke MaxPercentUnhealthyApplications van de kaart. De rest van de toepassingen blijven in de algemene groep en met MaxPercentUnhealthyApplications worden geëvalueerd.

Het volgende voorbeeld wordt een fragment uit een clustermanifest van het. Voorvoegsel voor het definiëren van vermeldingen in de overzichtsweergave van het type toepassing, de parameternaam van de met 'ApplicationTypeMaxPercentUnhealthyApplications-', gevolgd door de naam van toepassing.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Statusbeleid voor de toepassing
De [statusbeleid voor de toepassing](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) wordt beschreven hoe de evaluatie van gebeurtenissen en onderliggende Staten aggregatie wordt uitgevoerd voor toepassingen en hun kinderen. Deze kan worden gedefinieerd in het toepassingsmanifest **ApplicationManifest.xml**, in het toepassingspakket. Als er geen beleidsregels zijn opgegeven, Service Fabric wordt ervan uitgegaan dat de entiteit niet in orde als er een statusrapport of een onderliggend element op de status waarschuwing of fout.
De configureerbare beleidsregels zijn:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Hiermee geeft u op of voor het behandelen van waarschuwing status als fouten tijdens de evaluatie van de status rapporteert. Standaard: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Hiermee geeft u het maximumpercentage verdragen van gedistribueerde toepassingen die niet in orde zijn mag voordat de toepassing wordt beschouwd als fout. Dit percentage wordt berekend door het aantal beschadigde geïmplementeerde toepassingen delen via het aantal knooppunten dat de toepassingen die momenteel zijn geïmplementeerd op in het cluster. De berekening rondt af naar één tolereren op kleine aantallen knooppunten. Percentage standaard: 0.
* [DefaultServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Hiermee geeft u het service type health standaardbeleid, dat het standaardbeleid voor de status voor alle servicetypen in de toepassing vervangt.
* [ServiceTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Geeft een overzicht van service health beleidsregels per servicetype. Deze beleidsregels vervangen door de service type health standaardbeleidsregels voor elk type opgegeven service. Bijvoorbeeld, als een toepassing een Gatewaytype stateless service en een servicetype stateful-engine heeft, kunt u het statusbeleid voor de evaluatie anders. Wanneer u het beleid per servicetype opgeeft, kunt u gedetailleerde controle over de status van de service krijgen.

### <a name="service-type-health-policy"></a>Beleid voor de status van service type
De [service type statusbeleid](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) geeft aan hoe om te evalueren en samenvoegen van de services en de onderliggende objecten van services. Het beleid bevat:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Hiermee geeft u het maximumpercentage verdragen van beschadigde partities voordat een service als slecht beschouwd. Percentage standaard: 0.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Hiermee geeft u het maximumpercentage verdragen van replica's beschadigd voordat een partitie als slecht beschouwd. Percentage standaard: 0.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Hiermee geeft u het maximumpercentage verdragen van beschadigde services voordat de toepassing als slecht beschouwd. Percentage standaard: 0.

Het volgende voorbeeld wordt een fragment uit een manifest van de toepassing:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Evalueren
Gebruikers en geautomatiseerde services kunnen de status voor een entiteit op elk gewenst moment evalueren. Om te evalueren de status van een entiteit, het health store statistische functies status van alle rapporten weergeven over de entiteit en evalueert alle onderliggende items (indien van toepassing). De status van aggregatie-algoritme maakt gebruik van statusbeleid dat statusrapporten evalueren en de te aggregeren onderliggende statussen (indien van toepassing) opgeven.

### <a name="health-report-aggregation"></a>Aggregatie van de Health-rapport
Een entiteit kan meerdere statusrapporten die zijn verzonden door verschillende rapporteurs (onderdelen van het systeem of watchdogs) op verschillende eigenschappen hebben. De aggregatie gebruikt beleidsregels voor de bijbehorende status, in het bijzonder de ConsiderWarningAsError lid is van toepassing of het cluster statusbeleid. ConsiderWarningAsError geeft aan hoe waarschuwingen te evalueren.

De geaggregeerde status wordt geactiveerd door de *slechtste* systeemstatusrapporten over de entiteit. Als er ten minste één fout health-rapport, wordt de geaggregeerde status van een fout.

![Aggregatie voor Health rapport met een foutrapport kan worden opgenomen.][2]

Een health-entiteit met een of meer fout statusrapporten geëvalueerd als de fout. Hetzelfde geldt voor een verlopen statusrapport, ongeacht de status.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Als er geen foutenrapporten en een of meer waarschuwingen, wordt de geaggregeerde status waarschuwing of fout, afhankelijk van de vlag ConsiderWarningAsError-beleid.

![Aggregatie van de Health rapport waarschuwing rapport en een ConsiderWarningAsError false.][3]

Aggregatie van de Health-rapport waarschuwing rapport en een ConsiderWarningAsError ingesteld op false (standaard).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Aggregatie van onderliggende health
De samengevoegde status van een entiteit is inclusief de statussen van onderliggende (indien van toepassing). Het algoritme voor het verzamelen van statussen van de onderliggende maakt gebruik van het beleid van toepassing op basis van het entiteitstype.

![Onderliggende entiteiten health aggregatie.][4]

Onderliggende aggregatie op basis van de gezondheid van beleid.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Nadat de health store heeft vastgesteld dat alle onderliggende objecten, worden hun statussen op basis van het geconfigureerde maximale percentage van beschadigde onderliggende items. Dit percentage is afkomstig uit het beleid op basis van het type entiteit en onderliggende.

* Als alle onderliggende items OK Staten hebt, wordt de status van de onderliggende samengevoegd OK.
* Als onderliggende items OK en statussen van de waarschuwing hebt, wordt de status van de onderliggende samengevoegd waarschuwing.
* Als er onderliggende items met foutstatussen die niet aansluiten bij de maximaal toegestane percentage van slechte kinderen, wordt de status van de samengevoegde bovenliggende een fout.
* Als de onderliggende objecten met foutstatussen met inachtneming van de maximaal toegestane percentage van de beschadigde onderliggende items, de status van de samengevoegde bovenliggende is een waarschuwing.

## <a name="health-reporting"></a>Status rapporteren
Onderdelen van het systeem, systeem-Fabric-toepassingen en interne of externe watchdogs kunnen rapporteren op basis van Service Fabric-entiteiten. Controleer de verslaggevers *lokale* bepalingen van de status van de bewaakte entiteiten, op basis van de voorwaarden die ze bewaken. Ze nodig niet om te kijken naar alle globale status- of statistische gegevens. Het gewenste gedrag is dat eenvoudige rapporteurs en geen complexe organismen die nodig hebt om te kijken naar veel dingen die u moet het afleiden van welke gegevens te verzenden.

Health om gegevens te verzenden naar de health-store, moet een Rapportagefout de betrokken entiteit identificeert en maakt u een statusrapport. Voor het verzenden van het rapport, gebruikt u de [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API, rapport health API's beschikbaar gesteld op de `Partition` of `CodePackageActivationContext` objecten, PowerShell-cmdlets en REST.

### <a name="health-reports"></a>Statusrapporten
De [statusrapporten](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) voor elk van de entiteiten in het cluster de volgende informatie bevatten:

* **SourceId**. Een tekenreeks is die de journalist van de health-gebeurtenis wordt aangeduid.
* **Id van de entiteit**. Hiermee geeft u de entiteit wanneer het rapport wordt toegepast. Het verschilt op basis van de [entiteitstype](service-fabric-health-introduction.md#health-entities-and-hierarchy):
  
  * Het cluster. Geen.
  * Knooppunt. Naam van knooppunt (tekenreeks).
  * de toepassing. Toepassingsnaam (URI). Hiermee geeft u de naam van het exemplaar van de toepassing geïmplementeerd in het cluster.
  * De service. Servicenaam (URI). Hiermee geeft u de naam van de service-exemplaar geïmplementeerd in het cluster.
  * De partitie. Partitie-ID (GUID). Hiermee geeft u de unieke id van de partitie.
  * De replica. De stateful service-replica-ID of de stateless service-exemplaar-ID (INT64).
  * DeployedApplication. Toepassingsnaam (URI) en de naam van knooppunt (tekenreeks).
  * DeployedServicePackage. Toepassingsnaam (URI), naam van het knooppunt (tekenreeks) en service manifest van de naam (tekenreeks).
* **De eigenschap**. Een *tekenreeks* (niet een vaste opsomming) waarmee de journalist voor het categoriseren van de statusgebeurtenis voor een bepaalde eigenschap van de entiteit. Bijvoorbeeld, de status van de Node01 kan rapporteren dat journalist A 'Opslag'-eigenschap en journalist B kunnen de status van de Node01 rapporteren 'Connectiviteit'-eigenschap. In de winkel de gezondheid van worden deze rapporten behandeld als afzonderlijke statusgebeurtenissen voor de entiteit Node01.
* **Beschrijving**. Een tekenreeks waarmee een Rapportagefout gedetailleerde informatie over de statusgebeurtenis opgeven. **SourceId**, **eigenschap**, en **HealthState** moet volledig worden beschreven het rapport. De beschrijving van de leesbare informatie over het rapport toegevoegd. De tekst gemakkelijker voor beheerders en gebruikers te begrijpen van het statusrapport.
* **HealthState**. Een [opsomming](service-fabric-health-introduction.md#health-states) met de beschrijving van de status van het rapport. De geaccepteerde waarden zijn OK, waarschuwing en fout.
* **TimeToLive**. Een TimeSpan-waarde die aangeeft hoe lang het statusrapport is ongeldig. In combinatie met **RemoveWhenExpired**, wordt de health store verlopen gebeurtenissen evalueren. Standaard de waarde is oneindig en het rapport altijd geldig is.
* **RemoveWhenExpired**. Een Booleaanse waarde. Indien ingesteld op true, het verlopen statusrapport wordt automatisch verwijderd uit de store status en het rapport niet invloed hebben op evalueren entiteit. Gebruikt wanneer het rapport voor een opgegeven periode alleen geldig is en de journalist hoeft niet te deze expliciet wissen. Ook wordt gebruikt voor het verwijderen van rapporten uit de store health (bijvoorbeeld een watchdog is gewijzigd en stopt met het verzenden van rapporten met vorige bron en de eigenschap). Een rapport met een korte TimeToLive samen met RemoveWhenExpired om te wissen van een vorige status van de health store kan verzenden. Als de waarde is ingesteld op false, wordt het verlopen rapport behandeld als een fout opgetreden in de health-evaluatie. De waarde false geeft aan de health store dat de bron voor deze eigenschap regelmatig moet rapporteren. Als dat niet het geval is, moet er iets mis is met de watchdog. Status van de watchdog wordt op basis van de gebeurtenis als een fout vastgelegd.
* **SequenceNumber**. Een positief geheel getal dat moet worden toenemende, staat de volgorde van de rapporten. Deze wordt gebruikt door de health store voor het detecteren van verouderde rapporten die worden ontvangen, laat vanwege vertragingen in het netwerk of andere problemen. Een rapport wordt geweigerd als het volgnummer is dat kleiner dan of gelijk zijn aan de meest recent nummer voor de dezelfde entiteit, de bron en de eigenschap toegepast. Als deze niet is opgegeven, wordt het volgnummer automatisch gegenereerd. Het is nodig om te zetten in het volgnummer alleen tijdens het rapporteren van verloopgebeurtenissen bij statuswijzigingen. In dit geval wordt de bron moet onthouden welke rapporten, verzonden en blijven de gegevens voor herstel bij failover.

Deze vier onderdelen van informatie, SourceId, entiteits-id, eigenschap en HealthState--zijn vereist voor elke statusrapport. De SourceId tekenreeks mag niet beginnen met het voorvoegsel "**systeem.**', die is gereserveerd voor systeemrapporten. Er is slechts één rapport voor de dezelfde bron en de eigenschap voor de dezelfde entiteit. Meerdere rapporten voor de bron en de eigenschap overschrijven elkaar en aan de clientzijde health (als ze worden batchgewijs) of op de status aan clientzijde opslaan. De vervanging is gebaseerd op volgnummers; nieuwere rapporten (met hogere volgnummers) vervangt oudere rapporten.

### <a name="health-events"></a>Statusgebeurtenissen
Intern, houdt u de health store [statusgebeurtenissen](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent), die alle gegevens van de rapporten en aanvullende metagegevens bevatten. De metagegevens bevatten de tijd die het rapport is gegeven aan de health-client en de tijd die het op de server is gewijzigd. De health-gebeurtenissen worden geretourneerd door [statusquery's](service-fabric-view-entities-aggregated-health.md#health-queries).

De metagegevens van de toegevoegde bevat:

* **SourceUtcTimestamp**. De tijd die het rapport is gegeven aan de health-client (Coordinated Universal Time).
* **LastModifiedUtcTimestamp**. De tijd die het rapport het laatst is gewijzigd op de server (Coordinated Universal Time).
* **IsExpired**. Een vlag die aangeeft of het rapport is verlopen wanneer de query is uitgevoerd door de health-store. Een gebeurtenis kan zijn verlopen alleen als RemoveWhenExpired ingesteld op false is. Anders wordt de gebeurtenis is niet geretourneerd door de query en wordt verwijderd uit de store.
* **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. De laatste keer voor OK/waarschuwing/fout overgangen. Deze velden geeft de geschiedenis van de status van statusovergangen voor de gebeurtenis.

De velden van de overgang status kunnen worden gebruikt voor slimmer waarschuwingen of informatie over 'historische' health-gebeurtenissen. Deze inschakelen scenario's zoals:

* Waarschuwen wanneer een eigenschap op de waarschuwing/fout voor meer dan X minuten is. Controle van de voorwaarde voor een bepaalde periode, voorkomt u waarschuwingen op tijdelijke omstandigheden. Bijvoorbeeld, een waarschuwing als de status van de is zijn waarschuwing voor meer dan vijf minuten kan worden vertaald naar (HealthState waarschuwings- en nu - LastWarningTransitionTime == > 5 minuten).
* Waarschuwing is alleen van de voorwaarden die zijn gewijzigd in de laatste X minuten. Als een rapport al bij fout voordat u de opgegeven periode is, kan worden genegeerd omdat deze al eerder is gesignaleerd.
* Als een eigenschap tussen de waarschuwings- en uitschakelen is, moet u bepalen hoe lang het is niet in orde (dat wil zeggen, niet OK). Bijvoorbeeld, een waarschuwing als de eigenschap niet in orde meer dan vijf minuten is kan worden vertaald naar (HealthState! = Ok en nu - LastOkTransitionTime > 5 minuten).

## <a name="example-report-and-evaluate-application-health"></a>Voorbeeld: Rapporteren en evalueren van de status van de toepassing
Het volgende voorbeeld verzendt een statusrapport via PowerShell op de toepassing **fabric: / WordCount** van de bron **MyWatchdog**. Het statusrapport bevat informatie over de health-eigenschap 'beschikbaarheid' in een foutstatus, met onbeperkte TimeToLive. Vervolgens hiervoor wordt de status van de toepassing deze retourneert fouten voor de status en de gerapporteerde statusgebeurtenissen in de lijst met statusgebeurtenissen samengevoegde.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Gebruik van de gezondheid van modellen
Het statusmodel kunt cloudservices en de onderliggende Service Fabric-platform om te schalen, omdat de controle en statusrapporten bepalingen zijn verdeeld over de verschillende monitors binnen het cluster.
Andere systemen een gecentraliseerde service hebben op het niveau van het cluster waarop alle parseert de *mogelijk* nuttige informatie verzonden door de services. Deze benadering, kunnen hun schaalbaarheid. Deze ook kan geen specifieke informatie verzamelen om te helpen bij het identificeren van problemen en potentiële problemen als dicht bij de hoofdoorzaak mogelijk.

Het statusmodel wordt intensief gebruikt voor controle en diagnose van, voor het evalueren van de gezondheid van cluster en de toepassing en voor bewaakte upgrades. Statusgegevens andere services gebruiken voor het uitvoeren van automatische reparaties, statusgeschiedenis cluster maken en uitgeven van waarschuwingen van bepaalde voorwaarden.

## <a name="next-steps"></a>Volgende stappen
[Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

[Systeemstatusrapporten gebruiken voor het oplossen van problemen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Het servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Aangepaste statusrapporten van Service Fabric toevoegen](service-fabric-report-health.md)

[Controle en diagnose van services lokaal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md)

