---
title: Statuscontrole in Service Fabric | Microsoft Docs
description: Een inleiding tot de Azure Service Fabric voor health monitoring model biedt bewaking van het cluster en de toepassingen en services.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 1d979210-b1eb-4022-be24-799fd9d8e003
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: oanapl
ms.openlocfilehash: 271d02bf5793ccb4ca8cbc4eeb8a6c5cfdd74f03
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-service-fabric-health-monitoring"></a>Inleiding tot de statuscontrole Service Fabric
Azure Service Fabric introduceert een statusmodel waarmee de uitgebreide, flexibele en uitbreidbare statusevaluatie en rapportage. Het model kunt near-realtime bewaking van de status van het cluster en de services die erin worden uitgevoerd. Kunt u eenvoudig verkrijgen van informatie en corrigeer eventuele problemen voordat ze trapsgewijs en grote storingen veroorzaken. In het typische model services verzenden rapporten op basis van hun lokale weergaven en dat er gegevens worden samengevoegd om op te geven van een algemene-clusterniveau weergeven.

Dit model uitgebreide health service Fabric-onderdelen gebruiken voor het rapporteren van hun huidige status. U kunt hetzelfde mechanisme rapport status van uw toepassingen. Als u investeert in de status van hoge kwaliteit rapportage die uw aangepaste voorwaarden worden vastgelegd, kunt u detecteren en veel gemakkelijker problemen oplossen voor uw toepassing uitgevoerd.

De volgende video voor Microsoft Virtual Academy beschrijft ook het statusmodel van de Service Fabric en hoe deze wordt gebruikt:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-health-introduction/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

> [!NOTE]
> We begonnen met het subsysteem health een behoefte voor bewaakte upgrades. Service Fabric bevat bewaakte toepassing en de cluster-upgrades die volledige beschikbaarheid, zonder uitvaltijd te garanderen en minimale naar niets tussenkomst van de gebruiker. Als u wilt bereiken deze doelstellingen, controleert de upgrade op basis van geconfigureerde Upgradebeleid health. Een upgrade kunt doorgaan alleen wanneer de status respecteert de gewenste drempelwaarden. Anders wordt is de upgrade automatisch teruggedraaid of om beheerders te geven een kans om op te lossen problemen met de onderbroken. Zie voor meer informatie over toepassingsupgrades, [in dit artikel](service-fabric-application-upgrade.md).
> 
> 

## <a name="health-store"></a>Health store
De health store houdt health-gerelateerde informatie over entiteiten in het cluster voor eenvoudig ophalen en evalueren. Deze is geïmplementeerd als een Service Fabric stateful service persistent om ervoor te zorgen voor hoge beschikbaarheid en schaalbaarheid. De health store maakt deel uit van de **fabric: / System** toepassing, en is beschikbaar wanneer het cluster actief is en wordt uitgevoerd.

## <a name="health-entities-and-hierarchy"></a>De statusentiteiten en -hiërarchie
De health-entiteiten zijn georganiseerd in een logische hiërarchie waarmee interacties en afhankelijkheden tussen verschillende entiteiten wordt vastgelegd. De health store worden automatisch de statusentiteiten en hiërarchie gebaseerd op rapporten ontvangen van Service Fabric-onderdelen bouwt.

De health-entiteiten spiegelen van de Service Fabric-entiteiten. (Bijvoorbeeld **health Toepassingsentiteit** overeenkomt met een toepassingsexemplaar geïmplementeerd in het cluster, terwijl **health knooppunt entiteit** overeenkomt met het knooppunt van een Service Fabric-cluster.) De health-hiërarchie bevat de interactie van de systeementiteiten en is de basis voor geavanceerde de statusevaluatie. U kunt meer informatie over belangrijke concepten voor Service Fabric in [technisch overzicht van Service Fabric](service-fabric-technical-overview.md). Zie voor meer informatie over de toepassing [Service Fabric-toepassingsmodel](service-fabric-application-model.md).

De health-entiteiten en een hiërarchie maken voor de cluster en toepassingen worden effectief gerapporteerd, foutopsporing, en bewaakt. Het statusmodel biedt een nauwkeurige *gedetailleerde* weergave van de status van de vele bewegende softwareonderdelen in het cluster.

![Health-entiteiten.][1]
De health-entiteiten, geordend in een hiërarchie op basis van relaties bovenliggend-onderliggend.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

De health-entiteiten zijn:

* **Cluster**. Vertegenwoordigt de status van een Service Fabric-cluster. Statusrapporten cluster beschrijven voorwaarden die invloed hebben op het hele cluster. Deze voorwaarden invloed hebben op meerdere entiteiten in het cluster of het cluster zelf. Op basis van de voorwaarde, beperken niet de Rapportagefout het probleem naar beneden op een of meer beschadigde onderliggende. Voorbeelden hiervan zijn de brain van het cluster verdelen vanwege netwerkproblemen partitioneren of communicatie.
* **Knooppunt**. Vertegenwoordigt de status van een Service Fabric-knooppunt. Statusrapporten knooppunt beschreven voorwaarden die invloed hebben op de functionaliteit van het knooppunt. Ze doorgaans invloed op de geïmplementeerde entiteiten uitgevoerd. Voorbeelden zijn knooppunt geen schijfruimte meer (of andere eigenschappen van alle computers, zoals geheugen, verbindingen) en wanneer een knooppunt niet actief is. De entiteit van het knooppunt wordt aangeduid met de naam van het knooppunt (tekenreeks).
* **Toepassing**. Vertegenwoordigt de status van de instantie van een toepassing in het cluster wordt uitgevoerd. Statusrapporten toepassing beschrijven voorwaarden die invloed hebben op de algemene status van de toepassing. Ze kunnen niet worden teruggebracht naar afzonderlijke onderliggende elementen (services of geïmplementeerde toepassingen). Voorbeelden zijn de end-to-end interactie tussen verschillende services in de toepassing. De Toepassingsentiteit wordt geïdentificeerd door de naam van de toepassing (URI).
* **Service**. Vertegenwoordigt de status van een service in het cluster wordt uitgevoerd. Rapporten van de health service beschrijven voorwaarden die invloed hebben op de algemene status van de service. De Rapportagefout kan niet naar een slechte status partitie of replica interpreteren. Voorbeelden zijn een serviceconfiguratie (zoals poort of een externe bestandsshare) die wordt veroorzaakt door problemen voor alle partities. De service-entiteit wordt geïdentificeerd door de naam van de service (URI).
* **Partitie**. Vertegenwoordigt de status van een service-partitie. Statusrapporten partitie beschrijven voorwaarden die invloed hebben op de hele replicaset. Voorbeelden zijn wanneer het aantal replica's lager dan het aantal doel is, waarbij een partitie is sprake van quorumverlies. De entiteit partitie wordt geïdentificeerd door de partitie-ID (GUID).
* **Replica**. Vertegenwoordigt de status van een replica stateful service of een staatloze service-exemplaar. De replica is de kleinste eenheid die watchdogs en onderdelen van het systeem kunnen worden weergegeven voor een toepassing. Voorbeelden zijn voor stateful services, een primaire replica die bewerkingen secundaire replica's te traag replicatie kan niet worden gerepliceerd. Een stateless exemplaar kan ook rapporteren wanneer deze wordt uitgevoerd onvoldoende resources of problemen met de netwerkverbinding heeft. De replica-entiteit wordt geïdentificeerd door de partitie-ID (GUID) en de replica of het exemplaar-ID (lang).
* **DeployedApplication**. Vertegenwoordigt de status van een *toepassing die wordt uitgevoerd op een knooppunt*. Statusrapporten geïmplementeerde toepassing worden voorwaarden die specifiek zijn voor de toepassing op het knooppunt dat niet kan worden teruggebracht op servicepakketten die zijn geïmplementeerd op hetzelfde knooppunt beschreven. Voorbeelden zijn fouten bij het toepassingspakket kan niet worden gedownload op dat knooppunt en problemen met het instellen van beveiligings-principals van toepassing op het knooppunt. De gedistribueerde toepassing wordt geïdentificeerd door de naam van de toepassing (URI) en de naam van het knooppunt (tekenreeks).
* **DeployedServicePackage**. Vertegenwoordigt de status van een servicepakket uitgevoerd op een knooppunt in het cluster. Hierin zijn specifiek voor een servicepakket voorwaarden die niet van invloed op de andere servicepakketten op hetzelfde knooppunt voor dezelfde toepassing. Voorbeelden zijn een codepakket in het servicepakket kan niet worden gestart en een configuratiepakket dat kan niet worden gelezen. Het pakket geïmplementeerde service wordt geïdentificeerd door de naam van de toepassing (URI), knooppuntnaam (tekenreeks), service manifest-naam (tekenreeks) en service pakket activerings-ID (tekenreeks).

De granulatie van het statusmodel kunt gemakkelijk detecteren en oplossen van problemen. Bijvoorbeeld, als een service niet reageert, is het mogelijk voor het rapporteren van het toepassingsexemplaar is beschadigd. Rapportage op dat niveau niet ideaal, echter is, omdat het probleem zonder de services binnen die toepassing dat mogelijk. Het rapport moet worden toegepast met de slechte service of met een partitie specifieke onderliggende als u meer informatie naar de betreffende partitie verwijst. De gegevens automatisch verwerkingsinformatie door middel van de hiërarchie en een slechte status partitie is zichtbaar gemaakt op niveau van de service en toepassing. De aggregatie kunt identificeren en hoe sneller de hoofdoorzaak van het probleem oplossen.

De health-hiërarchie is samengesteld van relaties bovenliggend-onderliggend. Een cluster bestaat uit de knooppunten en toepassingen. Toepassingen hebben services en toepassingen geïmplementeerd. Geïmplementeerde toepassingen heeft servicepakketten geïmplementeerd. Services hebben partities en elke partitie heeft een of meer replica's. Er is een speciale relatie tussen knooppunten en geïmplementeerde entiteiten. Een knooppunt slecht zoals gemeld door onderdeel van de instantie, de Failover Manager-service is van invloed op de geïmplementeerde toepassingen, servicepakketten en geïmplementeerd op deze replica's.

De health-hiërarchie vertegenwoordigt de meest recente toestand van het systeem op basis van de meest recente statusrapporten dat is bijna realtime informatie.
Interne en externe watchdogs kunnen op dezelfde diensten op basis van toepassingsspecifieke logica of aangepaste bewaakte voorwaarden rapporteren. Gebruikersrapporten worden gecombineerd met de systeemrapporten.

Plan te investeren in het rapport en reageren op health bij het ontwerp van een grote cloudservice. Deze vooraf investement vergemakkelijkt de service voor foutopsporing, bewaken en gebruiken.

## <a name="health-states"></a>Statussen
Service Fabric bevat drie statussen die laten zien of een entiteit al dan niet in orde is: OK, waarschuwing en fout. Een rapport wordt verzonden naar de store health moet een van deze statussen opgeven. Resultaat van evaluatie van de status is een van deze statussen.

De mogelijke [statussen](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) zijn:

* **OK**. De entiteit is in orde. Er zijn geen bekende problemen die zijn gerapporteerd bij het of het onderliggende (indien van toepassing).
* **Waarschuwing**. De entiteit heeft enkele problemen, maar deze nog steeds correct kan werken. Bijvoorbeeld, treedt vertraging, maar ze zorgen niet voor eventuele problemen met de functionele nog. In sommige gevallen kan waarschuwingsvoorwaarde voor de zelf oplossen zonder tussenkomst van de externe. In dergelijke gevallen statusrapporten bewustzijn en bieden inzicht in wat er gebeurt. In andere gevallen kan de waarschuwingsvoorwaarde voor de prestaties in een ernstig probleem zonder tussenkomst van de gebruiker.
* **Fout**. De entiteit is beschadigd. Actie moet worden uitgevoerd om het herstel van de status van de entiteit omdat deze niet juist werken.
* **Onbekende**. De entiteit bestaat niet in de health store. Dit resultaat kan worden verkregen van de gedistribueerde query's die samen de resultaten van meerdere onderdelen. Bijvoorbeeld, de lijstquery voor get-knooppunt overschakelt naar de **FailoverManager**, **ClusterManager**, en **HealthManager**; ophalen van de toepassing lijstquery overschakelt naar de  **ClusterManager** en **HealthManager**. Deze query's samenvoegen resultaten van meerdere onderdelen van het systeem. Als onderdeel van een andere een entiteit die niet aanwezig zijn in health store retourneert, het samengevoegde resultaat bevat een onbekende status. Een entiteit is niet in de store omdat statusrapporten nog niet zijn verwerkt of de entiteit is opgeschoond na verwijdering.

## <a name="health-policies"></a>Statusbeleid
De health store geldt statusbeleid om te bepalen of een entiteit in orde op basis van de rapporten ervan en de onderliggende items.

> [!NOTE]
> Statusbeleid kunnen worden opgegeven in het clustermanifest (voor cluster en het knooppunt statusevaluatie) of in het toepassingsmanifest (voor toepassingsevaluatie en alle onderliggende). Health evaluatie aanvragen kunnen ook worden doorgegeven in aangepaste evaluatie statusbeleid alleen voor deze evaluatie gebruikt worden.
> 
> 

Standaard geldt Service Fabric strikte regels (alles wat u moet zich in orde) voor de hiërarchische relatie bovenliggend-onderliggend. Zelfs een van de onderliggende items heeft een slechte gebeurtenis, de bovenliggende worden beschouwd als niet in orde.

### <a name="cluster-health-policy"></a>Cluster statusbeleid
De [cluster statusbeleid](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) wordt gebruikt om de status van de cluster en het knooppunt statussen beoordelen. Het beleid kan worden gedefinieerd in het clustermanifest. Als dit niet aanwezig is, wordt het standaardbeleid (nul verdragen fouten) gebruikt.
Het statusbeleid cluster bevat:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Hiermee geeft u op of op dezelfde manier behandelen waarschuwing health als fouten tijdens de statusevaluatie. Standaard: false.
* [MaxPercentUnhealthyApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Hiermee geeft u het maximumpercentage verdragen van toepassingen die beschadigd worden kunnen voordat het cluster wordt beschouwd als fout.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Hiermee geeft u het maximumpercentage verdragen van knooppunten die niet in orde zijn mag voordat het cluster wordt beschouwd als fout. In grote clusters sommige knooppunten zijn altijd niet actief of out-reparaties, zodat dit percentage moet worden geconfigureerd voor die tolereren.
* [Applicationtypehealthpolicymap; deze](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). De toepassing type health beleid kaart kan worden gebruikt tijdens de evaluatie van de cluster-status te beschrijven van speciale toepassingstypen. Standaard worden alle toepassingen in een pool geplaatst en geëvalueerd, MaxPercentUnhealthyApplications. Als sommige toepassingtypen anders moeten worden behandeld, kunnen ze worden uitgevoerd buiten de globale groep. Ze worden geëvalueerd op basis van de percentages die zijn gekoppeld aan de naam van het toepassingstype in de kaart. Bijvoorbeeld in een cluster zijn er duizenden toepassingen met verschillende typen en enkele exemplaren van een besturingselement toepassing van een speciale toepassingstype. De toepassingen van het besturingselement moet nooit fout. U kunt globale MaxPercentUnhealthyApplications tot 20% tolereren in sommige gevallen, maar voor het type 'ControlApplicationType' de MaxPercentUnhealthyApplications ingesteld op 0 opgeven. Op deze manier als sommige van de vele toepassingen niet in orde zijn, maar onder de globale slecht percentage, zou het cluster worden geëvalueerd voor de waarschuwing. Upgraden van cluster is niet van invloed op een waarschuwingsstatus of andere bewaking geactiveerd op basis van de status fout. Maar zelfs één besturingselement toepassing in fout zouden cluster slechte, die terugdraaifase activeert of het upgraden van het cluster, afhankelijk van de configuratie van de upgrade wordt onderbroken.
  Voor de toepassingstypen gedefinieerd in de kaart, worden alle exemplaren van een toepassing genomen buiten de algemene groep van toepassingen. Ze worden geëvalueerd op basis van het totale aantal aanvragen van het toepassingstype, met behulp van de specifieke MaxPercentUnhealthyApplications van de kaart. De rest van de toepassingen blijven in de algemene groep en met MaxPercentUnhealthyApplications worden geëvalueerd.

Het volgende voorbeeld is een fragment uit een clustermanifest van de. Als u wilt definiëren vermeldingen in de toepassing type-kaart, het voorvoegsel dat de parameternaam van de met 'ApplicationTypeMaxPercentUnhealthyApplications-', gevolgd door de naam van het toepassingstype.

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
De [statusbeleid voor de toepassing](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) wordt beschreven hoe de evaluatie van gebeurtenissen en onderliggende statussen aggregatie wordt uitgevoerd voor toepassingen en onderliggende items. Het kan worden gedefinieerd in het toepassingsmanifest **ApplicationManifest.xml**, in het toepassingspakket. Als er geen beleidsregels zijn opgegeven, Service Fabric wordt ervan uitgegaan dat de entiteit slecht is wanneer er een statusrapport of een onderliggend element op de status waarschuwing of fout.
De configureerbare beleidsregels zijn:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Hiermee geeft u op of op dezelfde manier behandelen waarschuwing health als fouten tijdens de statusevaluatie. Standaard: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Hiermee geeft u het maximale percentage verdragen geïmplementeerde toepassingen die beschadigd worden kunnen voordat de toepassing wordt beschouwd als fout. Dit percentage wordt berekend door het aantal beschadigde geïmplementeerde toepassingen delen via het aantal knooppunten dat de toepassingen die momenteel zijn geïmplementeerd op in het cluster. De berekening rondt af naar boven op een storing op een klein aantal knooppunten tolereren. Percentage standaard: nul.
* [DefaultServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Hiermee geeft u het standaard service type statusbeleid, die het statusbeleid standaard voor alle servicetypen in de toepassing vervangt.
* [Servicetypehealthpolicymap; deze](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Geeft een overzicht van service statusbeleid per servicetype. Deze beleidsregels vervangen door de service type health standaardbeleidsregels voor elk type opgegeven service. Bijvoorbeeld, als een toepassing een servicetype staatloze gateway en een servicetype stateful-engine heeft, kunt u het statusbeleid voor de evaluatie anders. Wanneer u beleid per servicetype opgeeft, krijgt u gedetailleerde controle over de status van de service.

### <a name="service-type-health-policy"></a>Statusbeleid voor service type
De [service type statusbeleid](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) geeft aan hoe evalueren en samenvoegen van de services en de onderliggende services. Het beleid bevat:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Hiermee geeft u het maximale percentage verdragen slecht partities voordat een service wordt beschouwd als niet in orde. Percentage standaard: nul.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Hiermee geeft u het maximale percentage verdragen slecht replica's voordat u een partitie is in orde beschouwd. Percentage standaard: nul.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Hiermee geeft u het maximale percentage verdragen slecht services voordat de toepassing is in orde beschouwd. Percentage standaard: nul.

Het volgende voorbeeld is een fragment uit een manifest van de toepassing:

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

## <a name="health-evaluation"></a>De statusevaluatie
Gebruikers en geautomatiseerde services kunnen de status voor een entiteit op elk gewenst moment evalueren. Een entiteit om status te evalueren, het health store statistische functies alle health rapporten voor de entiteit en evalueert alle onderliggende (indien van toepassing). De health-aggregatie-algoritme maakt gebruik van statusbeleid die opgeeft hoe statusrapporten evalueren en hoe voor het cumuleren van onderliggende statussen (indien van toepassing).

### <a name="health-report-aggregation"></a>Aggregatie van Health-rapport
Een entiteit kan meerdere statusrapporten verzonden door verschillende rapporteurs (onderdelen van het systeem of watchdogs) op verschillende eigenschappen hebben. De aggregatie maakt gebruik van de bijbehorende statusbeleid, met name het ConsiderWarningAsError lid van het statusbeleid voor de toepassing of het cluster. ConsiderWarningAsError geeft aan hoe waarschuwingen evalueren.

De geaggregeerde status wordt geactiveerd door de *slechtste* systeemstatusrapporten op de entiteit. Als er ten minste één fout statusrapport, wordt de geaggregeerde status is een fout opgetreden.

![Aggregatie voor Health rapport met een foutenrapport.][2]

Een health-entiteit die een of meer fout statusrapporten heeft wordt geëvalueerd als de fout. Hetzelfde geldt voor een verlopen statusrapport, ongeacht de status.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Als er geen foutenrapporten en een of meer waarschuwingen, wordt de geaggregeerde status waarschuwing of fout, afhankelijk van de vlag ConsiderWarningAsError-beleid.

![Aggregatie voor Health rapport met een rapport van de waarschuwing en ConsiderWarningAsError false.][3]

Aggregatie voor het rapport met een rapport van de waarschuwing en ConsiderWarningAsError Health ingesteld op false (standaard).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Onderliggende health aggregatie
De cumulatieve status van een entiteit weerspiegelt de statussen van onderliggende (indien van toepassing). De algoritme voor het verzamelen van statussen onderliggende gebruikt het statusbeleid van toepassing op basis van het entiteitstype.

![Onderliggende entiteiten health aggregatie.][4]

Onderliggende aggregatie op basis van het statusbeleid.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Nadat de health store heeft alle onderliggende objecten geëvalueerd, aggregeert het hun statussen op basis van het geconfigureerde maximumpercentage van slecht onderliggende items. Dit percentage wordt overgenomen uit het beleid op basis van het type entiteit en onderliggende.

* Als alle onderliggende items OK statussen hebt, is de status van onderliggende geaggregeerd OK.
* Als de onderliggende elementen OK en statussen van de waarschuwing hebt, wordt de status van onderliggende geaggregeerd waarschuwing.
* Als er onderliggende elementen met fout statussen die bieden geen ondersteuning voor de maximaal toegestane percentage van slecht onderliggende items, wordt de geaggregeerde status is een fout opgetreden.
* Als de onderliggende elementen met fout statussen inachtneming van het maximaal toegestane percentage van slecht kinderen, de geaggregeerde status is een waarschuwing.

## <a name="health-reporting"></a>Health rapportage
Onderdelen van het systeem, systeem-Fabric-toepassingen en interne of externe watchdogs kunnen rapporteren Service Fabric-entiteiten. Controleer de rapporteurs *lokale* bepalingen van de status van de bewaakte entiteiten, op basis van de voorwaarden die ze bewaken. Ze nodig niet om te kijken naar alle globale status of samenvoegen van gegevens. Het gewenste gedrag is te eenvoudig rapporteurs en geen complexe organismen die nodig zijn om te kijken naar veel zaken die u moet het afleiden van welke gegevens worden verzonden.

Health om gegevens te verzenden naar de health store, moet een Rapportagefout bepalen van de betrokken entiteit en een statusrapport maken. Gebruik voor het verzenden van het rapport, de [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API, rapport health API's worden weergegeven op de `Partition` of `CodePackageActivationContext` objecten, PowerShell-cmdlets of REST.

### <a name="health-reports"></a>Statusrapporten
De [statusrapporten](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) voor elk van de entiteiten in het cluster de volgende informatie bevatten:

* **SourceId**. Een tekenreeks die een unieke identificatie van de Rapportagefout van de health-gebeurtenis.
* **Entiteit-id**. Identificeert de entiteit waarop het rapport wordt toegepast. Het verschil op basis van de [entiteitstype](service-fabric-health-introduction.md#health-entities-and-hierarchy):
  
  * Het cluster. Geen.
  * Knooppunt. Naam van knooppunt (tekenreeks).
  * de toepassing. Toepassingsnaam (URI). Vertegenwoordigt de naam van het toepassingsexemplaar geïmplementeerd in het cluster.
  * De service. Service-naam (URI). Vertegenwoordigt de naam van het service-exemplaar dat is geïmplementeerd in het cluster.
  * Partitie. Partitie-ID (GUID). Hiermee geeft u de unieke id van partitie.
  * De replica. De stateful-replica-ID of de staatloze service-exemplaar-ID (INT64).
  * DeployedApplication. Toepassingsnaam (URI) en de naam van het knooppunt (tekenreeks).
  * DeployedServicePackage. Toepassingsnaam (URI), de naam van het knooppunt (tekenreeks) en de service manifest naam (tekenreeks).
* **De eigenschap**. Een *tekenreeks* (geen vaste opsomming) waarmee de Rapportagefout om de statusgebeurtenis voor een bepaalde eigenschap van de entiteit te categoriseren. Bijvoorbeeld, de status van de Node01 kan rapporteren dat Rapportagefout A ' opslageigenschap ' en Rapportagefout B kunnen de status van de Node01 rapporteren 'Verbinding'-eigenschap. Deze rapporten worden in het archief health behandeld als afzonderlijke health-gebeurtenissen voor de entiteit Node01.
* **Beschrijving**. Een tekenreeks waarmee een Rapportagefout te bieden gedetailleerde informatie over de statusgebeurtenis. **SourceId**, **eigenschap**, en **HealthState** het rapport moet volledig worden beschreven. De beschrijving van de leesbare informatie over het rapport toegevoegd. De tekst gemakkelijker voor beheerders en gebruikers om te begrijpen van het statusrapport.
* **HealthState**. Een [opsomming](service-fabric-health-introduction.md#health-states) die de status van het rapport beschrijft. De toegestane waarden zijn OK, waarschuwing en fout.
* **TimeToLive**. Een TimeSpan-waarde die aangeeft hoe lang het statusrapport geldig is. In combinatie met **RemoveWhenExpired**, wordt de health store verlopen gebeurtenissen evalueren. Standaard de waarde is oneindig en het rapport is geldig permanent verloren.
* **RemoveWhenExpired**. Een Booleaanse waarde. Als is ingesteld op true, wordt het verlopen statusrapport wordt automatisch verwijderd uit het archief van de gezondheid en het rapport heeft geen invloed op entiteit de statusevaluatie. Gebruikt wanneer het rapport voor een opgegeven periode alleen geldig is en de Rapportagefout hoeft niet volledig te wissen uit. Dit wordt ook gebruikt voor rapporten verwijderen uit de store health (bijvoorbeeld een watchdog is gewijzigd en stopt met het verzenden van rapporten met vorige bron en de eigenschap). Het verzenden van een rapport met een korte TimeToLive samen met RemoveWhenExpired om te wissen van een eerdere status van de health store. Als de waarde is ingesteld op false, wordt het rapport verlopen beschouwd als een fout van de health-evaluatie. De waarde false geeft aan de health store dat de bron voor deze eigenschap periodiek moet rapporteren. Als dit niet het geval is, moet er iets mis met de watchdog. Status van de watchdog wordt op basis van de gebeurtenis als een fout vastgelegd.
* **SequenceNumber**. Een positief geheel getal dat moet kunnen worden steeds groter wordende, is de volgorde van de rapporten. Deze wordt gebruikt door de health store voor het detecteren van verouderde rapporten die vertraagd vanwege vertragingen in het netwerk of andere problemen worden ontvangen. Een rapport wordt geweigerd als het volgnummer is dat kleiner dan of gelijk zijn aan de meest voor de dezelfde entiteit, de bron en de eigenschap onlangs zijn toegepast. Als deze niet is opgegeven, wordt het volgnummer automatisch gegenereerd. U hoeft te plaatsen in het volgnummer alleen tijdens het rapporteren van verloopgebeurtenissen bij statuswijzigingen. In dit geval kan moet de bron onthouden welke rapporten deze verzonden en de gegevens voor herstel op failover behouden.

Deze vier onderdelen van informatie--SourceId, entiteits-id-eigenschap en HealthState--zijn vereist voor elke statusrapport. De SourceId-tekenreeks mag niet beginnen met het voorvoegsel '**System.**', die is gereserveerd voor systeemrapporten. Er is slechts één rapport voor de dezelfde bron en de eigenschap voor dezelfde entiteit. Meerdere rapporten voor de bron en de eigenschap overschrijven elkaar, aan de clientzijde health (als ze in batch worden opgenomen) of op de status aan clientzijde opslaan. De vervanging is gebaseerd op volgnummers; nieuwere rapporten (met hogere volgnummers) vervangt oudere rapporten.

### <a name="health-events"></a>Health-gebeurtenissen
Intern maakt de health store houdt [gebeurtenissen health](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent), die de informatie in de rapporten en aanvullende metagegevens bevatten. De metagegevens omvatten de tijd die het rapport is opgegeven voor de health-client en de tijd die het aan de serverzijde is gewijzigd. De health-gebeurtenissen worden geretourneerd door [statusquery's](service-fabric-view-entities-aggregated-health.md#health-queries).

De metagegevens van de toegevoegde bevatten:

* **SourceUtcTimestamp**. De tijd het rapport is besteed aan de client health (Coordinated Universal Time).
* **LastModifiedUtcTimestamp**. De tijd die het rapport voor het laatst is gewijzigd op de server (Coordinated Universal Time).
* **IsExpired**. Een markering om aan te geven of het rapport is verlopen wanneer de query is uitgevoerd door de health store. Een gebeurtenis kan zijn verlopen alleen als RemoveWhenExpired ingesteld op false is. Anders wordt de gebeurtenis wordt niet geretourneerd door de query en wordt verwijderd uit de store.
* **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. De laatste keer voor OK/waarschuwing/fout overgangen. Deze velden geeft de geschiedenis van de status van statusovergangen voor de gebeurtenis.

De velden van de overgang status kunnen worden gebruikt voor slimmer waarschuwingen of 'historische' health gebeurtenisgegevens. Deze inschakelen scenario's zoals:

* Een melding wanneer een eigenschap is al op waarschuwing/fout voor meer dan X minuten. Controleren of de voorwaarde voor een bepaalde periode, voorkomt waarschuwingen op tijdelijke omstandigheden. Bijvoorbeeld, een waarschuwing als de status heeft meer dan vijf minuten is waarschuwing kan worden vertaald naar (HealthState waarschuwings- en nu - LastWarningTransitionTime == > 5 minuten).
* Waarschuwingen alleen op de voorwaarden die zijn gewijzigd in de laatste X minuten. Als een rapport al bij fout voordat u de opgegeven tijd is, kan worden genegeerd omdat deze al eerder is gesignaleerd.
* Als een eigenschap is schakelen tussen waarschuwingen en fouten, moet u bepalen hoe lang het is niet in orde (dat wil zeggen, niet OK). Bijvoorbeeld, een waarschuwing als de eigenschap niet in orde meer dan vijf minuten is kan worden vertaald naar (HealthState! = Ok en nu - LastOkTransitionTime > 5 minuten).

## <a name="example-report-and-evaluate-application-health"></a>Voorbeeld: Rapporteren en evalueren van de toepassingsstatus
Het volgende voorbeeld stuurt een statusrapport via PowerShell over de toepassing **fabric: / WordCount** van de bron **MyWatchdog**. Het statusrapport bevat informatie over de health-eigenschap 'beschikbaarheid' in een foutstatus met oneindige TimeToLive. Vervolgens wordt de toepassingsstatus, welke retourneert geaggregeerd health status fouten en de gerapporteerde status gebeurtenissen in de lijst met gebeurtenissen health opgevraagd.

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
Het statusmodel kunt cloudservices en de onderliggende Service Fabric-platform wilt schalen, omdat de controle en health bepalingen zijn verdeeld over de verschillende monitors binnen het cluster.
Andere systemen hebben een gecentraliseerde service op het clusterniveau van het waarop alle parseert de *mogelijk* nuttige informatie verzonden door de services. Deze aanpak is het lastig hun schaalbaarheid. Deze ook kan geen specifieke informatie om u te helpen bij het identificeren van problemen en potentiële problemen als dicht bij de hoofdoorzaak mogelijk te verzamelen.

Het statusmodel wordt intensief gebruikt voor controle en diagnose, voor het evalueren van de gezondheid van cluster en de toepassing en bewaakte upgrades. Andere statusgegevens gebruikt om automatische reparaties uitvoeren, de geschiedenis van cluster bouwen en waarschuwingen op bepaalde voorwaarden uitgeeft.

## <a name="next-steps"></a>Volgende stappen
[Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

[Systeemstatusrapporten gebruiken voor het oplossen van problemen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Het rapport en controleer de servicestatus van de](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Aangepaste Service Fabric-statusrapporten toevoegen](service-fabric-report-health.md)

[Controle en diagnose van lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade van de service Fabric-toepassing](service-fabric-application-upgrade.md)

