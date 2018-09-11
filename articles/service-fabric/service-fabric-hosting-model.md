---
title: Azure Service Fabric-hostingmodel | Microsoft Docs
description: Beschrijving van de relatie tussen de replica's (of exemplaren) van een geïmplementeerde Service Fabric-service en het hostproces van de service.
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: 367f21c63eac3969fb19eada91eae9a8577921de
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348477"
---
# <a name="azure-service-fabric-hosting-model"></a>Azure Service Fabric-hostingmodel
In dit artikel biedt een overzicht van modellen die worden geleverd door Azure Service Fabric-toepassing en beschrijving van de verschillen tussen de **gedeeld proces** en **exclusieve proces** modellen. Hierin wordt beschreven hoe een geïmplementeerde toepassing analyseert in een Service Fabric-knooppunt en de relatie tussen de replica's (of exemplaren) van de service en het hostproces van de service.

Voordat u doorgaat, zorg ervoor dat u de verschillende concepten kennen en relaties die zijn uiteengezet in [een toepassing modelleren in Service Fabric][a1]. 

> [!NOTE]
> In dit artikel, tenzij expliciet wordt vermeld als een andere betekenis:
>
> - *Replica* verwijst naar zowel een replica van een stateful service en een exemplaar van een staatloze service.
> - *CodePackage* wordt beschouwd als gelijkwaardig zijn aan een *ServiceHost* proces dat registreert een *ServiceType*, en hosts replica's van services van die *ServiceType*.
>

Voor meer informatie over het hostingmodel, gaan we nemen een voorbeeld. Stel dat we hebben een *ApplicationType* 'MyAppType', heeft een *ServiceType* 'MyServiceType'. 'MyServiceType' wordt geleverd door de *ServicePackage* 'MyServicePackage', heeft een *CodePackage* 'MyCodePackage'. 'MyCodePackage' registreert *ServiceType* MyServiceType wanneer deze wordt uitgevoerd.

Stel dat we hebben een cluster met drie knooppunten en maken we een *toepassing* **fabric: / App1** van het type 'MyAppType'. In deze toepassing **fabric: / App1**, maken we een service **fabric: / App1/ServiceA** van het type 'MyServiceType'. Deze service heeft twee partities (bijvoorbeeld **P1** en **P2**), en drie replica's per partitie. Het volgende diagram toont de weergave van deze toepassing als dit het geval is geïmplementeerd op een knooppunt.


![Diagram van knooppuntweergave van geïmplementeerde toepassing][node-view-one]


Service Fabric geactiveerd 'MyServicePackage', die aan de slag 'MyCodePackage', die als host voor replica's van zowel de partities fungeert. Alle knooppunten in het cluster hebben dezelfde weergave, omdat we hebben gekozen voor het aantal replica's per partitie moet gelijk zijn aan het aantal knooppunten in het cluster. We gaan maken van een andere service, **fabric: / App1/ServiceB**, in de toepassing **fabric: / App1**. Deze service heeft één partitie (bijvoorbeeld **P3**), en drie replica's per partitie. Het volgende diagram toont de nieuwe weergave op het knooppunt:


![Diagram van knooppuntweergave van geïmplementeerde toepassing][node-view-two]


Service Fabric de nieuwe replica voor de partitie geplaatst **P3** van service **fabric: / App1/XPb** in de bestaande activering van 'MyServicePackage'. Nu. We gaan maken van een andere toepassing **fabric: / App2** van het type 'MyAppType'. Binnen **fabric: / App2**, een service maken **fabric: / App2/ServiceA**. Deze service heeft twee partities (**P4** en **P5**), en drie replica's per partitie. Het volgende diagram toont de nieuwe weergave van het knooppunt:


![Diagram van knooppuntweergave van geïmplementeerde toepassing][node-view-three]


Service Fabric Hiermee activeert u een nieuw exemplaar van 'MyServicePackage', waarmee een nieuw exemplaar van 'MyCodePackage' wordt gestart. Replica's van beide partities van de service **fabric: / App2/ServiceA** (**P4** en **P5**) in deze nieuwe kopie 'MyCodePackage' worden geplaatst.

## <a name="shared-process-model"></a>Gedeelde procesmodel
De voorgaande sectie beschrijft de standaard hostingmodel geleverd door de Service Fabric, aangeduid als het model gedeeld proces. In dit model voor een bepaalde toepassing slechts één exemplaar van een bepaalde *ServicePackage* is geactiveerd op een knooppunt (Hiermee start u alle de *CodePackages* erin). Alle replica's van alle services van een bepaalde *ServiceType* worden geplaatst de *CodePackage* die wordt geregistreerd dat *ServiceType*. Met andere woorden, alle replica's van alle services op een knooppunt van een bepaalde *ServiceType* delen hetzelfde proces.

## <a name="exclusive-process-model"></a>Exclusieve procesmodel
Het andere hostingmodel geleverd door de Service Fabric is de exclusieve procesmodel. In dit model, klikt u op een bepaald knooppunt, is elke replica in een eigen toegewezen proces aanwezig. Service Fabric activeert een nieuwe kopie van *ServicePackage* (Hiermee start u alle de *CodePackages* erin). Replica's worden geplaatst de *CodePackage* die geregistreerd de *ServiceType* van de service waartoe de replica behoort. 

Als u van Service Fabric versie 5.6 gebruikmaakt of hoger, kunt u de exclusieve procesmodel op het moment maakt u een service (met behulp van [PowerShell][p1], [REST] [ r1], of [FabricClient][c1]). Geef **ServicePackageActivationMode** als 'ExclusiveProcess'.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Als u een standaard-service in uw manifest van de toepassing hebt, kunt u de exclusieve procesmodel kiezen door op te geven de **ServicePackageActivationMode** kenmerk:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Nu maken we een andere service, **fabric: / App1/ServiceC**, in de toepassing **fabric: / App1**. Deze service heeft twee partities (bijvoorbeeld **P6** en **P7**), en drie replica's per partitie. U stelt **ServicePackageActivationMode** naar 'ExclusiveProcess'. Het volgende diagram toont de nieuwe weergave op het knooppunt:


![Diagram van knooppuntweergave van geïmplementeerde toepassing][node-view-four]


Zoals u ziet, Service Fabric twee nieuwe exemplaren van 'MyServicePackage' geactiveerd (één voor elke replica van partitie **P6** en **P7**). Service Fabric elke replica in een toegewezen exemplaar van geplaatst *CodePackage*. Wanneer u de exclusieve procesmodel voor een bepaalde toepassing, meerdere exemplaren van een bepaalde *ServicePackage* actief is op een knooppunt kan zijn. In het voorgaande voorbeeld, drie kopieën van 'MyServicePackage' zijn actief voor **fabric: / App1**. Elk van deze actieve exemplaren van 'MyServicePackage' heeft een **ServicePackageActivationId** die ermee verbonden zijn. Deze ID wordt aangegeven dat exemplaar in toepassing **fabric: / App1**.

Als u alleen het model gedeeld proces voor een toepassing gebruikt, en is er slechts één actieve kopie van *ServicePackage* op een knooppunt. De **ServicePackageActivationId** voor deze activering van *ServicePackage* is een lege tekenreeks. Dit is het geval is, bijvoorbeeld met **fabric: / App2**.

> [!NOTE]
>- Het gedeeld proces hostingmodel komt met overeen **ServicePackageActivationMode** gelijk is aan **SharedProcess**. Dit is de standaardinstelling hostingmodel, en **ServicePackageActivationMode** niet op het moment van het maken van de service moet worden opgegeven.
>
>- De exclusieve proces hostingmodel komt met overeen **ServicePackageActivationMode** gelijk is aan **ExclusiveProcess**. Als u wilt deze instelling gebruikt, moet u deze expliciet op het moment van het maken van de service. 
>
>- Als u het hostingmodel van een service, query de [beschrijving van de service][p2], en bekijk de waarde van **ServicePackageActivationMode**.
>
>

## <a name="work-with-a-deployed-service-package"></a>Werken met een geïmplementeerde service-pakket
Een actieve kopie van een *ServicePackage* op een knooppunt wordt aangeduid als een [geïmplementeerd servicepakket][p3]. Wanneer u de exclusieve procesmodel gebruikt voor het maken van services voor een bepaalde toepassing, is er mogelijk meerdere geïmplementeerde servicepakketten voor dezelfde *ServicePackage*. Als u bewerkingen die specifiek zijn voor een geïmplementeerde service-pakket uitvoert, moet u opgeven **ServicePackageActivationId** voor het identificeren van een specifiek geïmplementeerd servicepakket. Bijvoorbeeld, de ID opgeven als u [rapportage van de status van een geïmplementeerde service-pakket] [ p4] of [opnieuw opstarten van het codepakket van een geïmplementeerde service-pakket] [p5].

U vindt de **ServicePackageActivationId** van een geïmplementeerde service-pakket door het opvragen van de lijst met [geïmplementeerd servicepakketten] [ p3] op een knooppunt. Wanneer u query's uitvoeren voor de [geïmplementeerd servicetypen][p6], [replica's geïmplementeerd][p7], en [codepakketten geïmplementeerd ] [ p8] op een knooppunt, bevat het queryresultaat ook de **ServicePackageActivationId** van het pakket met de bovenliggende geïmplementeerde service.

> [!NOTE]
>- Onder het gedeeld proces hostingmodel, op een bepaald knooppunt, voor een bepaalde toepassing slechts één exemplaar van een *ServicePackage* is geactiveerd. Er is een **ServicePackageActivationId** gelijk zijn aan *lege tekenreeks*, en moet niet worden opgegeven tijdens het uitvoeren van bewerkingen met betrekking tot het pakket geïmplementeerde service. 
>
> - Onder de exclusieve proces die als host fungeert model op een bepaald knooppunt, voor een bepaalde toepassing, een of meer exemplaren van een *ServicePackage* actief kan zijn. Elke activering heeft een *niet-lege* **ServicePackageActivationId**opgegeven tijdens het uitvoeren van bewerkingen met betrekking tot het pakket geïmplementeerde service. 
>
> - Als **ServicePackageActivationId** wordt weggelaten, wordt standaard *lege tekenreeks*. Als u een geïmplementeerde service-pakket dat is geactiveerd onder het gedeelde procesmodel aanwezig is, wordt de bewerking erop worden uitgevoerd. Anders mislukt de bewerking.
>
> - Geen query uitvoeren voor één keer en cache de **ServicePackageActivationId**. De ID wordt dynamisch gegenereerd en kan wijzigen om verschillende redenen. Voordat u een bewerking die moet **ServicePackageActivationId**, u moet eerst de lijst met query [geïmplementeerd servicepakketten] [ p3] op een knooppunt. Gebruik vervolgens de **ServicePackageActivationId** van het queryresultaat de oorspronkelijke bewerking uit te voeren.
>
>

## <a name="guest-executable-and-container-applications"></a>Gast uitvoerbaar bestand- en container-toepassingen
Service Fabric behandelt [Gast kan worden uitgevoerd] [ a2] en [container] [ a3] toepassingen, zoals stateless services die ingesloten zijn. Er is geen Service Fabric-runtime in *ServiceHost* (een proces of container). Omdat deze services ingesloten zijn, het aantal replica's per *ServiceHost* is niet van toepassing voor deze services. De meest voorkomende configuratie gebruikt in combinatie met deze services is één partitie met [InstanceCount] [ c2] gelijk aan -1 (één exemplaar van de servicecode die wordt uitgevoerd op elk knooppunt van het cluster). 

De standaardwaarde **ServicePackageActivationMode** voor deze services is **SharedProcess**, Service Fabric activeert in dat geval slechts één exemplaar van *ServicePackage* op een knooppunt voor een bepaalde toepassing.  Dit betekent dat slechts één exemplaar van de servicecode een knooppunt wordt uitgevoerd. Als u wilt dat meerdere kopieën van uw servicecode uit te voeren op een knooppunt, geeft u **ServicePackageActivationMode** als **ExclusiveProcess** op het moment van het maken van de service. Bijvoorbeeld, u kunt dit doen wanneer u meerdere services maakt (*Service1* naar *ServiceN*) van *ServiceType* (opgegeven in *ServiceManifest*), of als uw service is meerdere partities. 

## <a name="change-the-hosting-model-of-an-existing-service"></a>Het hostingmodel van een bestaande service wijzigen
Op dit moment, kan u het hostingmodel van een bestaande service wijzigen van gedeeld proces in exclusieve proces (of omgekeerd).

## <a name="choose-between-the-hosting-models"></a>Kiezen tussen de host modellen
U moet evalueren welke hostingmodel beste past bij uw vereisten. Gedeeld proces gebruikt het model besturingssysteemresources beter, omdat minder processen zijn gestart en meerdere replica's in hetzelfde proces poorten kunnen delen. Echter, als een van de replica's een fout waar ze moeten de ServiceHost uitvallen heeft, dit invloed heeft op alle andere replica's in hetzelfde proces.

 De exclusieve procesmodel biedt betere isolatie, met elke replica in een eigen proces. Als een van de replica's een fout heeft, heeft dit geen invloed op andere replica's. Dit model is handig in situaties waarbij delen van de poort niet wordt ondersteund door het communicatieprotocol. Dit vereenvoudigt het uitvoeren van de mogelijkheid om toe te passen resourcebeheer op het niveau van de replica. De exclusieve proces verbruikt echter meer besturingssysteem-resources, zoals het Hiermee wordt een proces voor elke replica die op het knooppunt.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Overwegingen voor het model exclusieve procesmodel en toepassing
Voor de meeste toepassingen, kunt u uw toepassing in Service Fabric model doordat een *ServiceType* per *ServicePackage*. 

Voor bepaalde gevallen, Service Fabric kunt ook meer dan één *ServiceType* per *ServicePackage* (één *CodePackage* kunt registreren meer dan één  *ServiceType*). Hier volgen enkele van de scenario's waarin deze configuraties kunnen nuttig zijn:

- U wilt optimaliseren door bij het maken van minder processen en met hogere dichtheid van replica per proces.
- Replica's van verschillende *ServiceTypes* nodig hebt voor het delen van sommige algemene gegevens met een hoge initialisatie of geheugen kosten.
- U hebt een gratis service-aanbieding en u wilt een limiet van Resourcegebruik door het plaatsen van alle replica's van de service in hetzelfde proces plaatsen.

De exclusieve proces hostingmodel is niet samenhangend met een toepassingsmodel met meerdere *ServiceTypes* per *ServicePackage*. Dit komt doordat meerdere *ServiceTypes* per *ServicePackage* zijn ontworpen om te realiseren hogere resource delen tussen de replica's, en kunt u hogere dichtheid van replica per proces. De exclusieve procesmodel is ontworpen voor verschillende resultaten te bereiken.

Neem het geval van meerdere *ServiceTypes* per *ServicePackage*, met een andere *CodePackage* registreren van elke *ServiceType*. Stel dat we hebben een *ServicePackage* 'MultiTypeServicePackge', met twee *CodePackages*:

- 'MyCodePackageA', waardoor wordt geregistreerd *ServiceType* 'MyServiceTypeA'.
- 'MyCodePackageB', waardoor wordt geregistreerd *ServiceType* 'MyServiceTypeB'.

Nu, laten we zeggen dat we maken van een toepassing **fabric: / SpecialApp**. Binnen **fabric: / SpecialApp**, maken we twee services met de exclusieve procesmodel te volgen:

- Service **fabric: / SpecialApp/ServiceA** van het type 'MyServiceTypeA', met twee partities (bijvoorbeeld **P1** en **P2**), en drie replica's per partitie.
- Service **fabric: / SpecialApp/ServiceB** van het type 'MyServiceTypeB', met twee partities (**P3** en **P4**), en drie replica's per partitie.

Op een bepaald knooppunt hebben beide van de services elke twee replica's. Omdat we de exclusieve procesmodel gebruikt voor het maken van de services, wordt een nieuw exemplaar van 'MyServicePackage' voor elke replica in Service Fabric geactiveerd. Elke activering van 'MultiTypeServicePackge' Start een exemplaar van 'MyCodePackageA' en 'MyCodePackageB'. Echter slechts één van de 'MyCodePackageA' of 'MyCodePackageB' als host fungeert voor de replica waarvoor 'MultiTypeServicePackge' is geactiveerd. Het volgende diagram toont de weergave van het knooppunt:


![Diagram van de weergave van het knooppunt van de geïmplementeerde toepassing][node-view-five]


In het activeren van 'MultiTypeServicePackge' voor de replica van de partitie **P1** van service **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' als host fungeert voor de replica. 'MyCodePackageB' wordt uitgevoerd. Op deze manier in het activeren van 'MultiTypeServicePackge' voor de replica van de partitie **P3** van service **fabric: / SpecialApp/XPb**, 'MyCodePackageB' als host fungeert voor de replica. 'MyCodePackageA' wordt uitgevoerd. Dus, hoe groter het aantal *CodePackages* (registreren van verschillende *ServiceTypes*) per *ServicePackage*, hoe hoger het redundante Resourcegebruik. 
 
 Echter, als we de services maakt **fabric: / SpecialApp/ServiceA** en **fabric: / SpecialApp/XPb** met de gedeelde procesmodel, Service Fabric activeert slechts één exemplaar van ' MultiTypeServicePackge' voor de toepassing **fabric: / SpecialApp**. 'MyCodePackageA' als host fungeert voor alle replica's voor de service **fabric: / SpecialApp/ServiceA**. 'MyCodePackageB' als host fungeert voor alle replica's voor de service **fabric: / SpecialApp/XPb**. Het volgende diagram toont de weergave van het knooppunt in deze instelling: 


![Diagram van de weergave van het knooppunt van de geïmplementeerde toepassing][node-view-six]


In het voorgaande voorbeeld u denkt dat als 'MyCodePackageA' registreert zowel 'MyServiceTypeA' en 'MyServiceTypeB', en er is geen 'MyCodePackageB', dan is er geen redundante *CodePackage* uitgevoerd. Hoewel dit klopt, wordt deze toepassingsmodel komt niet overeen met de exclusieve proces hostingmodel. Als het doel is om elke replica in een eigen toegewezen proces, hoeft u niet om u te registreren beide *ServiceTypes* uit hetzelfde *CodePackage*. In plaats daarvan u plaatst u elk *ServiceType* in een eigen *ServicePackage*.

## <a name="next-steps"></a>Volgende stappen
[Een toepassing verpakken] [ a4] en bereid om te implementeren.

[Implementeren en verwijderen van toepassingen][a5]. In dit artikel wordt beschreven hoe u PowerShell gebruiken voor het beheren van exemplaren van de toepassing.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-guest-executables-introduction.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/module/servicefabric/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/module/servicefabric/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedcodepackage
