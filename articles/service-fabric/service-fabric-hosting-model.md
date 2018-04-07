---
title: Azure Service Fabric-hostingmodel | Microsoft Docs
description: Hierin wordt beschreven in de relatie tussen de replica's of exemplaren van een geïmplementeerde Service Fabric-service en het hostproces van de service.
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: b2ba019f21256ee98276ef30847c43709b9b3462
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="azure-service-fabric-hosting-model"></a>Azure Service Fabric-hostingmodel
In dit artikel biedt een overzicht van modellen die worden geleverd door de Azure Service Fabric-toepassing en beschrijft de verschillen tussen de **gedeeld proces** en **exclusieve proces** modellen. Hierin wordt beschreven hoe een geïmplementeerde toepassing van een Service Fabric-knooppunt en de relatie tussen de replica's (of exemplaren) van de service en het proces van het ServiceHost eruit ziet.

Voordat u doorgaat, zorg ervoor dat u de verschillende concepten begrijpt en relaties uitgelegd in [Model van een toepassing in Service Fabric][a1]. 

> [!NOTE]
> In dit artikel, tenzij expliciet wordt vermeld als een iets andere betekenis:
>
> - *Replica* verwijst naar zowel een replica van een stateful service en een exemplaar van een staatloze service.
> - *CodePackage* wordt behandeld als gelijkwaardig zijn aan een *ServiceHost* proces dat registreert een *ServiceType*, en hosts replica's van services van die *ServiceType*.
>

Voor informatie over het hostingmodel, kunt u een voorbeeld gaan we doorlopen. Laten we een *ApplicationType* 'MyAppType', heeft een *ServiceType* 'MyServiceType'. 'MyServiceType' wordt geleverd door de *ServicePackage* 'MyServicePackage', heeft een *CodePackage* 'MyCodePackage'. Registreert de 'MyCodePackage' *ServiceType* MyServiceType wanneer deze wordt uitgevoerd.

Stel dat we hebben een cluster met drie knooppunten en maken we een *toepassing* **fabric: / App1** van het type 'MyAppType'. In deze toepassing **fabric: / App1**, maken we een service **fabric: / App1/ServiceA** van het type 'MyServiceType'. Deze service heeft twee partities (bijvoorbeeld **P1** en **P2**), en drie replica's per partitie. Het volgende diagram toont de weergave van deze toepassing zoals deze belandt geïmplementeerd op een knooppunt.


![Diagram van knooppunt weergave van geïmplementeerde toepassing][node-view-one]


Service Fabric geactiveerd 'MyServicePackage', die 'MyCodePackage', die als host voor replica's van zowel de partities fungeert gestart. Alle knooppunten in het cluster hebben dezelfde weergave, omdat we het aantal replica's per partitie mag niet gelijk zijn aan het aantal knooppunten in het cluster hebt gekozen. We gaan maken van een andere service **fabric: / App1/ServiceB**, in de toepassing **fabric: / App1**. Deze service heeft één partitie (bijvoorbeeld **P3**), en drie replica's per partitie. Het volgende diagram toont de nieuwe weergave op het knooppunt:


![Diagram van knooppunt weergave van geïmplementeerde toepassing][node-view-two]


Service Fabric de nieuwe replica voor partitie geplaatst **P3** van service **fabric: / App1/XPb** in de bestaande 'MyServicePackage'-activering. Nu. We gaan maken van een andere toepassing **fabric: / App2** van het type 'MyAppType'. Binnen **fabric: / App2**, een service maken **fabric: / App2/ServiceA**. Deze service heeft twee partities (**P4** en **P5**), en drie replica's per partitie. Het volgende diagram toont de nieuwe weergave van knooppunt:


![Diagram van knooppunt weergave van geïmplementeerde toepassing][node-view-three]


Service Fabric Hiermee activeert u een nieuw exemplaar van 'MyServicePackage', die een nieuw exemplaar van 'MyCodePackage' begint. Replica's van beide partities van de service **fabric: / App2/ServiceA** (**P4** en **P5**) in deze nieuwe kopie 'MyCodePackage' worden geplaatst.

## <a name="shared-process-model"></a>Gedeelde procesmodel
De vorige sectie wordt de standaardwaarde geleverd door de Service Fabric, aangeduid als het gedeelde procesmodel hostingmodel beschreven. In dit model voor een bepaalde toepassing slechts één exemplaar van een bepaalde *ServicePackage* is geactiveerd op een knooppunt (dat alle begint de *CodePackages* die dit). Alle replica's van alle services van een bepaalde *ServiceType* worden geplaatst de *CodePackage* die wordt geregistreerd dat *ServiceType*. Met andere woorden, alle replica's van alle services op een knooppunt van een bepaalde *ServiceType* delen hetzelfde proces.

## <a name="exclusive-process-model"></a>Exclusieve procesmodel
Het andere geleverd door de Service Fabric hostingmodel is het exclusieve procesmodel. In dit model, klikt u op een bepaald knooppunt woont elke replica in een eigen toegewezen proces. Service Fabric Hiermee activeert u een nieuw exemplaar van *ServicePackage* (dat alle begint de *CodePackages* die dit). Replica's worden geplaatst de *CodePackage* dat geregistreerd de *ServiceType* van de service waartoe de replica behoort. 

Als u van Service Fabric versie 5.6 gebruikmaakt of hoger, kunt u de exclusieve procesmodel op het moment dat u een service maakt (met behulp van [PowerShell][p1], [REST] [ r1], of [FabricClient][c1]). Geef **ServicePackageActivationMode** als 'ExclusiveProcess'.

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

Als u een standaard-service in uw toepassingsmanifest hebt, kunt u de exclusieve procesmodel door te geven de **ServicePackageActivationMode** kenmerk:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Nu gaan we maken een andere service **fabric: / App1/ServiceC**, in toepassing **fabric: / App1**. Deze service heeft twee partities (bijvoorbeeld **P6** en **P7**), en drie replica's per partitie. U stelt **ServicePackageActivationMode** naar 'ExclusiveProcess'. Het volgende diagram toont de nieuwe weergave op het knooppunt:


![Diagram van knooppunt weergave van geïmplementeerde toepassing][node-view-four]


Zoals u ziet, Service Fabric twee nieuwe exemplaren van 'MyServicePackage' geactiveerd (één voor elke replica van partitie **P6** en **P7**). Service Fabric geplaatst elke replica in een speciale van *CodePackage*. Wanneer u de exclusieve procesmodel voor een bepaalde toepassing, meerdere exemplaren van een bepaalde *ServicePackage* actief zijn op een knooppunt. In het voorgaande voorbeeld drie kopieën van 'MyServicePackage' zijn actief voor **fabric: / App1**. Elk van deze actieve kopieën van 'MyServicePackage' heeft een **ServicePackageActivationId** gekoppeld. Deze ID identificeert die kopie in toepassing **fabric: / App1**.

Wanneer u alleen de gedeelde procesmodel voor een toepassing, er is slechts één actieve kopie van *ServicePackage* op een knooppunt. De **ServicePackageActivationId** voor deze activering van *ServicePackage* is een lege tekenreeks. Dit is het geval is, bijvoorbeeld met **fabric: / App2**.

> [!NOTE]
>- Het proces gedeeld hostingmodel komt met overeen **ServicePackageActivationMode** gelijk is aan **SharedProcess**. Dit is de standaardinstelling hostingmodel, en **ServicePackageActivationMode** niet op het moment van het maken van de service moet worden opgegeven.
>
>- De exclusieve proces hostingmodel komt met overeen **ServicePackageActivationMode** gelijk is aan **ExclusiveProcess**. Om deze instelling gebruikt, moet u deze expliciet op het moment van het maken van de service. 
>
>- Als u wilt weergeven van het hostingmodel van een service, een query de [servicebeschrijving][p2], en bekijkt u de waarde van **ServicePackageActivationMode**.
>
>

## <a name="work-with-a-deployed-service-package"></a>Werken met een geïmplementeerd servicepakket
Een actieve kopie van een *ServicePackage* op een knooppunt wordt aangeduid als een [geïmplementeerd servicepakket][p3]. Wanneer u de exclusieve procesmodel gebruikt voor het maken van services voor een bepaalde toepassing is er mogelijk meerdere geïmplementeerde servicepakketten voor dezelfde *ServicePackage*. Als u bewerkingen die specifiek zijn voor een geïmplementeerde service-pakket uitvoert, moet u opgeven **ServicePackageActivationId** voor het identificeren van een specifiek geïmplementeerd servicepakket. Bijvoorbeeld, de-ID opgeven als u [de status van een pakket geïmplementeerde service reporting] [ p4] of [opnieuw starten van het codepakket van een geïmplementeerde servicepakket] [p5].

U vindt hier de **ServicePackageActivationId** van een geïmplementeerde service-pakket door in de lijst met te [geïmplementeerde servicepakketten] [ p3] op een knooppunt. Wanneer u een query uitvoert voor de [geïmplementeerd servicetypen][p6], [replica's geïmplementeerd][p7], en [code pakketten geïmplementeerd ] [ p8] op een knooppunt, bevat het queryresultaat ook de **ServicePackageActivationId** van het pakket met de bovenliggende geïmplementeerde service.

> [!NOTE]
>- Onder de gedeelde hosting-procesmodel, op een bepaald knooppunt, voor een bepaalde toepassing slechts één exemplaar van een *ServicePackage* is geactiveerd. Er is een **ServicePackageActivationId** gelijk zijn aan *lege tekenreeks*, en niet tijdens het uitvoeren van bewerkingen die gerelateerd zijn aan het geïmplementeerde service-pakket moet worden opgegeven. 
>
> - Onder het hosten van exclusieve proces model op een bepaald knooppunt, voor een bepaalde toepassing een of meer exemplaren van een *ServicePackage* kan actief zijn. Elke activering heeft een *niet-lege* **ServicePackageActivationId**, opgegeven tijdens het uitvoeren van bewerkingen die gerelateerd zijn aan het pakket geïmplementeerde service. 
>
> - Als **ServicePackageActivationId** wordt weggelaten, wordt standaard *lege tekenreeks*. Als een geïmplementeerde service-pakket dat is geactiveerd onder het gedeelde procesmodel aanwezig is, wordt de bewerking erop worden uitgevoerd. Anders mislukt de bewerking.
>
> - Geen query uitvoeren eenmaal en cache de **ServicePackageActivationId**. De ID dynamisch wordt gegenereerd, en om verschillende redenen kan wijzigen. Voordat u een bewerking die behoeften uitvoert **ServicePackageActivationId**, u moet eerst de lijst met query [geïmplementeerde servicepakketten] [ p3] op een knooppunt. Gebruik vervolgens de **ServicePackageActivationId** van het queryresultaat is de oorspronkelijke bewerking uit te voeren.
>
>

## <a name="guest-executable-and-container-applications"></a>Het uitvoerbare bestand en de container toepassingen Gast
Service Fabric behandelt [Gast uitvoerbaar bestand] [ a2] en [container] [ a3] toepassingen als stateless services die ingesloten zijn. Er is geen Service Fabric-runtime in *ServiceHost* (een proces of container). Omdat deze services zichzelf, het aantal replica's per *ServiceHost* is niet van toepassing voor deze services. De configuratie van de meest voorkomende gebruikt in combinatie met deze services is één partitie met [InstanceCount] [ c2] gelijk aan-1 (één exemplaar van de servicecode die wordt uitgevoerd op elk knooppunt van het cluster). 

De standaardwaarde **ServicePackageActivationMode** is voor deze services **SharedProcess**, Service Fabric activeert in dat geval slechts één exemplaar van *ServicePackage* op een knooppunt voor een bepaalde toepassing.  Dit betekent dat slechts één exemplaar van de servicecode een knooppunt wordt uitgevoerd. Als u wilt dat meerdere exemplaren van uw servicecode op een knooppunt worden uitgevoerd, geeft u **ServicePackageActivationMode** als **ExclusiveProcess** op het moment van het maken van de service. Bijvoorbeeld, u kunt dit doen wanneer u meerdere services maakt (*Service1* naar *ServiceN*) van *ServiceType* (opgegeven in *ServiceManifest*), of wanneer de service zich meerdere gepartitioneerd. 

## <a name="change-the-hosting-model-of-an-existing-service"></a>Het hostingmodel van een bestaande service wijzigen
Op dit moment kan u het hostingmodel van een bestaande service gedeeld proces wijzigen in exclusieve proces (of andersom).

## <a name="choose-between-the-hosting-models"></a>Kiezen tussen de hosting modellen
U moet evalueren welke hostingmodel best past bij uw behoeften. Gedeeld proces gebruikt het model besturingssysteembronnen beter, omdat minder processen worden geïnitieerd en poorten kunnen worden gedeeld door meerdere replica's in hetzelfde proces. Echter, als een van de replica's een fout waar moet heeft worden buiten de servicehost, dit van invloed is op alle andere replica's in hetzelfde proces.

 De exclusieve procesmodel biedt betere isolatie, met elke replica in een eigen proces. Als een van de replica's een fout heeft, heeft deze geen gevolgen voor andere replica's. Dit model is handig in situaties waar delen van de poort wordt niet ondersteund door het communicatieprotocol. Dit vereenvoudigt de mogelijkheid om toe te passen resource governance op niveau van de replica. De exclusieve proces echter verbruikt meer besturingssysteembronnen, omdat deze Hiermee wordt een proces voor elke replica op het knooppunt.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Exclusieve procesmodel en toepassing modelleren overwegingen
Voor de meeste toepassingen kunt u uw toepassing in Service Fabric model doordat een *ServiceType* per *ServicePackage*. 

Voor bepaalde gevallen, Service Fabric kunt ook meer dan één *ServiceType* per *ServicePackage* (één *CodePackage* kunt registreren meer dan één  *ServiceType*). Hier volgen enkele van de scenario's waar deze configuraties nuttig kunnen zijn:

- U wilt optimaliseren Resourcegebruik door minder processen te starten en met hogere replica dichtheid per proces.
- Replica's van verschillende *ServiceTypes* nodig voor het delen van een aantal algemene gegevens met een hoge initialisatie of geheugen kosten.
- U hebt een vrije serviceaanbieding en u wilt opnemen een limiet van Resourcegebruik door de gegevens van alle replica's van de service in hetzelfde proces.

De exclusieve hostingmodel is niet samenhangende met een toepassingsmodel meerdere *ServiceTypes* per *ServicePackage*. Dit komt doordat meerdere *ServiceTypes* per *ServicePackage* voor hogere resource delen van replica's zijn ontworpen en hogere replica dichtheid per proces maakt. De exclusieve procesmodel is ontworpen voor verschillende resultaten bereiken.

Neem het geval van meerdere *ServiceTypes* per *ServicePackage*, met een andere *CodePackage* registreren van elk *ServiceType*. Laten we een *ServicePackage* 'MultiTypeServicePackge', met twee *CodePackages*:

- 'MyCodePackageA', waardoor wordt geregistreerd *ServiceType* 'MyServiceTypeA'.
- 'MyCodePackageB', waardoor wordt geregistreerd *ServiceType* 'MyServiceTypeB'.

Nu we spreken maken we een toepassing, **fabric: / SpecialApp**. Binnen **fabric: / SpecialApp**, maken we de volgende twee services met het exclusieve procesmodel:

- Service **fabric: / SpecialApp/ServiceA** van het type 'MyServiceTypeA' met twee partities (bijvoorbeeld **P1** en **P2**), en drie replica's per partitie.
- Service **fabric: / SpecialApp/ServiceB** van het type 'MyServiceTypeB' met twee partities (**P3** en **P4**), en drie replica's per partitie.

Op een bepaald knooppunt hebben beide services elke twee replica's. Omdat we het exclusieve procesmodel gebruikt voor het maken van de services, wordt een nieuw exemplaar van 'MyServicePackage' voor elke replica in Service Fabric geactiveerd. Elke activering van 'MultiTypeServicePackge' wordt een kopie van 'MyCodePackageA' en 'MyCodePackageB' gestart. Echter slechts één van de 'MyCodePackageA' of 'MyCodePackageB' als host fungeert voor de replica waarvoor 'MultiTypeServicePackge' is geactiveerd. Het volgende diagram toont de weergave van knooppunt:


![Diagram van de knooppunt-weergave van geïmplementeerde toepassing][node-view-five]


In het activeren van 'MultiTypeServicePackge' voor de replica van de partitie **P1** van service **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' als host fungeert voor de replica. 'MyCodePackageB' wordt uitgevoerd. Op deze manier in het activeren van 'MultiTypeServicePackge' voor de replica van de partitie **P3** van service **fabric: / SpecialApp/XPb**, 'MyCodePackageB' als host fungeert voor de replica. 'MyCodePackageA' wordt uitgevoerd. Daarom, hoe groter het aantal *CodePackages* (registreren verschillende *ServiceTypes*) per *ServicePackage*, hoe groter het redundante brongebruik. 
 
 Echter, als we de services maakt **fabric: / SpecialApp/ServiceA** en **fabric: / SpecialApp/XPb** met de gedeelde-procesmodel, Service Fabric Hiermee activeert u slechts één exemplaar van ' MultiTypeServicePackge' voor de toepassing **fabric: / SpecialApp**. 'MyCodePackageA' als host fungeert voor alle replica's voor de service **fabric: / SpecialApp/ServiceA**. 'MyCodePackageB' als host fungeert voor alle replica's voor de service **fabric: / SpecialApp/XPb**. Het volgende diagram toont de weergave van het knooppunt in deze instelling: 


![Diagram van de knooppunt-weergave van geïmplementeerde toepassing][node-view-six]


In het voorgaande voorbeeld u denkt dat als 'MyCodePackageA' registreert zowel 'MyServiceTypeA' en 'MyServiceTypeB' en er is geen 'MyCodePackageB', er is geen redundante *CodePackage* uitgevoerd. Hoewel dit klopt, wordt dit toepassingsmodel komt niet overeen met het exclusieve proces hostingmodel. Als het doel is om elke replica in een eigen toegewezen proces plaatsen, hoeft u niet registreren beide *ServiceTypes* van dezelfde *CodePackage*. In plaats daarvan u gewoon elk plaatsen *ServiceType* in een eigen *ServicePackage*.

## <a name="next-steps"></a>Volgende stappen
[Een toepassing van het pakket] [ a4] en voorbereidingen te implementeren.

[Implementeren en toepassingen verwijderen][a5]. Dit artikel wordt beschreven hoe u PowerShell gebruikt voor het beheren van exemplaren van een toepassing.

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

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage
