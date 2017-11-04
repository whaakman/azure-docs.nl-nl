---
title: Azure Service Fabric hostingmodel | Microsoft Docs
description: "Beschrijft de relatie tussen de replica's (of exemplaren) van een geïmplementeerde Servic Fabric-service en de ServiceHost proces."
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
ms.openlocfilehash: ecc9038cf895ddaeb06dd0e4e9852d5ef4a4513a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="service-fabric-hosting-model"></a>Service Fabric-hostingmodel
In dit artikel biedt een overzicht van modellen die worden geleverd door de Service Fabric-toepassing en beschrijft de verschillen tussen de **gedeeld proces** en **exclusieve proces** modellen. Hierin wordt beschreven hoe een geïmplementeerde toepassing eruitziet op een Service Fabric-knooppunt en de relatie tussen de replica's (of exemplaren) van de service en het hostproces van de service.

Voordat u doorgaat, zorg ervoor dat u bekend met bent [Service Fabric-toepassingsmodel] [ a1] en verschillende concepten en relatie ertussen begrijpen. 

> [!NOTE]
> In dit artikel, ter vereenvoudiging tenzij expliciet wordt vermeld:
>
> - Al het gebruik van word *replica* verwijst naar zowel een replica van een stateful service of een exemplaar van een staatloze service.
> - *CodePackage* wordt behandeld is gelijk aan *ServiceHost* proces dat registreert een *ServiceType* en hosts replica's van services van die *ServiceType*.
>

Voor informatie over het hostingmodel, kunt u een voorbeeld laat het ons doorlopen. Laat het ons spreken we hebben een *ApplicationType* MyAppType' met een *ServiceType* MyServiceType' die wordt geleverd door *ServicePackage* MyServicePackage' met een *CodePackage* MyCodePackage, waardoor wordt geregistreerd *ServiceType* MyServiceType wanneer deze wordt uitgevoerd.

Stel dat we hebben een cluster met 3 knooppunten en maken we een *toepassing* **fabric: / App1** van het type 'MyAppType'. In dit *toepassing* **fabric: / App1** maken we een service **fabric: / App1/ServiceA** van het type MyServiceType' dat 2 partities is (spreken **P1** & **P2**) en 3 replica's per partitie. Het volgende diagram toont de weergave van deze toepassing zoals deze belandt geïmplementeerd op een knooppunt.

<center>
![Knooppunt-weergave van geïmplementeerde toepassing][node-view-one]
</center>

Service Fabric geactiveerd MyServicePackage' die gestart MyCodePackage' die als host voor replica's van zowel de partities dat wil zeggen fungeert **P1** & **P2**. Houd er rekening mee dat alle knooppunten in het cluster wordt dezelfde weergave omdat we hebt gekozen voor het aantal replica's per partitie gelijk zijn aan het aantal knooppunten in het cluster. We gaan maken van een andere service **fabric: / App1/ServiceB** in toepassing **fabric: / App1** die 1 partitie heeft (spreken **P3**) en 3 replica's per partitie. Volgende diagram toont de nieuwe weergave op het knooppunt:

<center>
![Knooppunt-weergave van geïmplementeerde toepassing][node-view-two]
</center>

Zoals we zien Service Fabric de nieuwe replica voor partitie geplaatst **P3** van service **fabric: / App1/XPb** in de bestaande 'MyServicePackage'-activering. Nu kunt maken van een andere *toepassing* **fabric: / App2** van het type 'MyAppType' en binnen **fabric: / App2** service maken **fabric: / App2/ServiceA** die 2 partities heeft (spreken **P4** & **P5**) en 3 replica's per partitie. Diagrammen, ziet u het nieuwe knooppunt weergave in:

<center>
![Knooppunt-weergave van geïmplementeerde toepassing][node-view-three]
</center>

Deze tijd Service Fabric is geactiveerd op een nieuw exemplaar van MyServicePackage' die wordt gestart van een nieuw exemplaar van 'MyCodePackage' en replica's van beide partities van de service **fabric: / App2/ServiceA** (dat wil zeggen **P4** & **P5**) in deze nieuwe kopie 'MyCodePackage' worden geplaatst.

## <a name="shared-process-model"></a>Gedeelde procesmodel
We zagen hierboven is de standaard hostingmodel geleverd door de Service Fabric en wordt aangeduid als **gedeeld proces** model. In dit model voor een gegeven *toepassing*, slechts één kopiëren van een bepaalde *ServicePackage* is geactiveerd op een *knooppunt* (Hiermee start u alle de *CodePackages* die dit) en alle replica's van alle services van een opgegeven *ServiceType* worden geplaatst de *CodePackage* die wordt geregistreerd dat *ServiceType*. Met andere woorden, alle replica's van alle services op een knooppunt van een bepaalde *ServiceType* delen hetzelfde proces.

## <a name="exclusive-process-model"></a>Exclusieve procesmodel
Het andere geleverd door de Service Fabric hostingmodel is **exclusieve proces** model. In dit model op een gegeven *knooppunt*, voor de plaatsing van elke replica Service Fabric Hiermee activeert u een nieuw exemplaar van *ServicePackage* (dat alle begint de *CodePackages* die dit) en replica wordt geplaatst in de *CodePackage* dat geregistreerd de *ServiceType* van de service welke replica behoort. Met andere woorden, woont elke replica in een eigen toegewezen proces. 

Dit model wordt ondersteund vanaf versie 5.6 van Service Fabric. **Exclusieve proces** model kan worden gekozen op het moment van de service maakt (met behulp van [PowerShell][p1], [REST] [ r1] of [FabricClient][c1]) door te geven **ServicePackageActivationMode** als 'ExclusiveProcess'.

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

Als u een standaardservice in het manifest van uw toepassing hebt, kunt u kiezen **exclusieve proces** model door op te geven **ServicePackageActivationMode** kenmerk zoals hieronder wordt weergegeven:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
U kunt doorgaan met het bovenstaande voorbeeld, kunt maken van een andere service **fabric: / App1/ServiceC** in toepassing **fabric: / App1** die 2 partities heeft (spreken **P6** & **P7**) en 3 replica's per partitie met **ServicePackageActivationMode** ingesteld op 'ExclusiveProcess'. Volgende diagram toont de nieuwe weergave op het knooppunt:

<center>
![Knooppunt-weergave van geïmplementeerde toepassing][node-view-four]
</center>

Zoals u ziet, Service Fabric twee nieuwe exemplaren van 'MyServicePackage' geactiveerd (één voor elke replica van partitie **P6** & **P7**) en elke replica geplaatst in een speciale van *CodePackage*. Iets anders te worden hier, wanneer **exclusieve proces** model wordt gebruikt, voor een bepaalde *toepassing*, meerdere exemplaren van een bepaalde *ServicePackage* actief zijn op een *knooppunt*. In bovenstaande voorbeeld zien we drie kopieën van 'MyServicePackage' zijn actief voor **fabric: / App1**. Elk van deze actieve kopieën van 'MyServicePackage' heeft een **ServicePackageActivationId** gekoppeld waarmee wordt aangegeven dat exemplaar binnen *toepassing* **fabric: / App1**.

Wanneer alleen **gedeeld proces** model wordt gebruikt voor een *toepassing*, bijvoorbeeld **fabric: / App2** in bovenstaande voorbeeld is slechts één actieve kopie van *ServicePackage*  op een *knooppunt* en **ServicePackageActivationId** voor deze activering van *ServicePackage* ' lege tekenreeks '.

> [!NOTE]
>- **Gedeeld proces** hostingmodel komt overeen met **ServicePackageActivationMode** gelijk **SharedProcess**. Dit is de standaardinstelling hostingmodel en **ServicePackageActivationMode** niet op het moment van het maken van de service moet worden opgegeven.
>
>- **Exclusieve proces** hostingmodel komt overeen met **ServicePackageActivationMode** gelijk **ExclusiveProcess** en moeten expliciet worden opgegeven op het moment van het maken van de service. 
>
>- HostingModel van een service kan worden herkend door het uitvoeren van query's de [servicebeschrijving] [ p2] en waarde van kijken **ServicePackageActivationMode**.
>
>

## <a name="working-with-deployed-service-package"></a>Werken met geïmplementeerd servicepakket
Een actieve kopie van een *ServicePackage* op een knooppunt wordt aangeduid als [geïmplementeerd servicepakket][p3]. Zoals eerder opgemerkt hierboven, wanneer **exclusieve proces** model wordt gebruikt voor het maken van services voor een bepaalde *toepassing*, er zijn meerdere geïmplementeerde servicepakketten voor dezelfde *ServicePackage*. Tijdens het uitvoeren van bewerkingen die specifiek zijn voor geïmplementeerd servicepakket zoals [status van een pakket geïmplementeerde service reporting] [ p4] of [codepakket van een geïmplementeerde servicepakket opnieuw te starten] [ p5] enz., **ServicePackageActivationId** moet worden opgegeven voor het identificeren van een specifiek geïmplementeerd servicepakket.

 **ServicePackageActivationId** van een geïmplementeerde service pakket kan worden verkregen door het opvragen van de lijst met [geïmplementeerde servicepakketten] [ p3] op een knooppunt. Tijdens het opvragen van [geïmplementeerd servicetypen][p6], [replica's geïmplementeerd] [ p7] en [code pakketten geïmplementeerd] [ p8] op een knooppunt, bevat het queryresultaat ook de **ServicePackageActivationId** van pakket met de bovenliggende geïmplementeerde service.

> [!NOTE]
>- Onder **gedeeld proces** hostingmodel, op een gegeven *knooppunt*, voor een bepaalde *toepassing*, slechts één exemplaar van een *ServicePackage* is geactiveerd. Deze heeft **ServicePackageActivationId** gelijk zijn aan *lege tekenreeks* en moet niet worden opgegeven tijdens bewerkingen met betrekking tot presterende geïmplementeerd servicepakket. 
>
> - Onder **exclusieve proces** hostingmodel, op een gegeven *knooppunt*, voor een bepaalde *toepassing*, een of meer exemplaren van een *ServicePackage* kan actief zijn. Elke activering heeft een *niet-lege* **ServicePackageActivationId** en moet worden opgegeven tijdens het uitvoeren van de geïmplementeerde service pakket gekoppelde bewerkingen. 
>
> - Als **ServicePackageActivationId** wordt weggelaten wordt standaard ingesteld op 'lege string'. Als u een geïmplementeerde service die het pakket is geactiveerd onder **gedeeld proces** model aanwezig is, wordt de bewerking wordt uitgevoerd op het, anders de bewerking mislukt.
>
> - Het wordt niet aangeraden eenmaal doorzoeken en cache **ServicePackageActivationId** naar dynamisch wordt gegenereerd en om verschillende redenen kunt wijzigen. Voordat u een bewerking die behoeften uitvoert **ServicePackageActivationId**, u moet eerst de lijst met query [geïmplementeerde servicepakketten] [ p3] op een knooppunt en gebruik vervolgens  **ServicePackageActivationId** uit het queryresultaat de oorspronkelijke bewerking uit te voeren.
>
>

## <a name="guest-executable-and-container-applications"></a>Het uitvoerbare bestand en de container toepassingen Gast
Service Fabric behandelt [Gast uitvoerbaar bestand] [ a2] en [container] [ a3] toepassingen als statless services die ingesloten zijn dus er is geen Service Fabric-runtime in *ServiceHost* (een proces of container). Aangezien deze services zelfstandig zijn, aantal replica's per *ServiceHost* is niet van toepassing voor deze services. De configuratie van de meest voorkomende gebruikt in combinatie met deze services is één partitie met [InstanceCount] [ c2] gelijk aan-1 (dat wil zeggen een kopie van de servicecode die wordt uitgevoerd op elk knooppunt van het cluster). 

De standaardwaarde **ServicePackageActivationMode** is voor deze services **SharedProcess** Service Fabric activeert in dat geval slechts één exemplaar van *ServicePackage* op een *knooppunt* voor een bepaalde *toepassing* wat betekent dat slechts één exemplaar van de servicecode wordt uitgevoerd een *knooppunt*. Als u wilt dat meerdere exemplaren van uw servicecode uit te voeren op een *knooppunt* wanneer u meerdere services maakt (*Service1* naar *ServiceN*) van *ServiceType* (opgegeven in *ServiceManifest*) of als uw service multi gepartitioneerd, moet u **ServicePackageActivationMode** als **ExclusiveProcess** op het moment van het maken van de service.

## <a name="changing-hosting-model-of-an-existing-service"></a>HostingModel van een bestaande service wijzigen
HostingModel van een bestaande service vanuit wijzigen **gedeeld proces** naar **exclusieve proces** en vice versa via upgraden of bijwerken van mechanisme (of specificatie van het in het toepassingsmanifest in service) wordt momenteel niet ondersteund. Ondersteuning voor deze functie wordt geleverd in toekomstige versies.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Kiezen tussen gedeeld proces en exclusieve procesmodel
Deze hosting modellen hebben de voor- en nadelen en gebruiker nodig om te beoordelen welke beste hun behoeften past. **Gedeeld proces** model stelt beter gebruik van de OS-bronnen omdat er minder processen worden geïnitieerd, meerdere replica's in hetzelfde proces kunnen delen poorten, enzovoort. Echter, als een van de replica's bij een fout waar moet worden buiten de ServiceHost aankomt, dat invloed op alle andere replica's in hetzelfde proces.

 **Exclusieve proces** model voorziet in betere isolatie met elke replica in een eigen proces en andere replica's heeft geen invloed op een replica zich niet normaal gedraagt. Deze goed van pas komt voor gevallen waarin delen van de poort wordt niet ondersteund door het communicatieprotocol. Dit vereenvoudigt de mogelijkheid om toe te passen resource governance op niveau van de replica. Aan de andere kant **exclusieve proces** verbruikt meer OS-bronnen omdat deze Hiermee wordt een proces voor elke replica op het knooppunt.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Exclusieve procesmodel en toepassing modelleren overwegingen
De aanbevolen manier om het model van uw toepassing in Service Fabric is dat een *ServiceType* per *ServicePackage* en dit model geschikt is voor de meeste van de toepassingen. 

Die bestemd zijn voor bepaalde gebruiksvoorbeelden, Service Fabric kunt ook meer dan één *ServiceType* per *ServicePackage* (één *CodePackage* meer dan één kuntregistreren *ServiceType*). Hier volgen enkele van de scenario's waar deze configuraties nuttig kunnen zijn:

- U wilt optimaliseren OS Resourcegebruik door minder processen te starten en met hogere replica dichtheid per proces.
- Replica's van verschillende ServiceTypes moeten een aantal algemene gegevens met een hoge initialisatie of geheugen kosten delen.
- U hebt een gratis serviceaanbieding en u wilt opnemen een limiet van Resourcegebruik door de gegevens van alle replica's van de service in hetzelfde proces.

**Exclusieve proces** hostingmodel is geen samenhangende met meerdere toepassingsmodel *ServiceTypes* per *ServicePackage*. Dit komt doordat meerdere *ServiceTypes* per *ServicePackage* is ontworpen om u te bereiken hogere resource delen van replica's en hogere replica dichtheid per proces maakt. Dit is in strijd is met wat **exclusieve proces** model is ontworpen om te bereiken.

Neem het geval van meerdere *ServiceTypes* per *ServicePackage* met andere *CodePackage* registreren van elk *ServiceType*. Laten we een *ServicePackage* MultiTypeServicePackge' met twee *CodePackages*:

- MyCodePackageA, waardoor wordt geregistreerd *ServiceType* 'MyServiceTypeA'.
- MyCodePackageB, waardoor wordt geregistreerd *ServiceType* 'MyServiceTypeB'.

Nu kunt spreken, maken we een *toepassing* **fabric: / SpecialApp** en binnen **fabric: / SpecialApp** maken we de volgende twee services met **exclusieve proces** model:

- Service **fabric: / SpecialApp/ServiceA** van het type 'MyServiceTypeA' met twee partities (spreken **P1** en **P2**) en 3 replica's per partitie.
- Service **fabric: / SpecialApp/ServiceB** van het type 'MyServiceTypeB' met twee partities (spreken **P3** en **P4**) en 3 replica's per partitie.

Beide de services wordt elke twee replica's hebben op een bepaald knooppunt. Aangezien we gebruikt **exclusieve proces** model voor het maken van de services, Service Fabric een nieuw exemplaar van 'MyServicePackge' voor elke replica worden geactiveerd. Elke activering van 'MultiTypeServicePackge' wordt een kopie van 'MyCodePackageA' en 'MyCodePackageB' gestart. Slechts één van de 'MyCodePackageA' of 'MyCodePackageB' host echter de replica waarvoor 'MultiTypeServicePackge' is geactiveerd. Volgende diagram toont de weergave van knooppunt:

<center>
![Knooppunt-weergave van geïmplementeerde toepassing][node-view-five]
</center>

 Zoals we in het activeren van 'MultiTypeServicePackge' voor de replica van partitie zien kunt **P1** van service **fabric: / SpecialApp/ServiceA**, de replica wordt gehost door 'MyCodePackageA' en 'MyCodePackageB' net actief is. Op deze manier bij activering van 'MultiTypeServicePackge' voor de replica van partitie **P3** van service **fabric: / SpecialApp/XPb**, 'MyCodePackageB' als host fungeert voor de replica en 'MyCodePackageA' is zojuist lucht enzovoort. Daarom meer het aantal *CodePackages* (registreren verschillende *ServiceTypes*) per *ServicePackage*, hogere worden redundante Resourcegebruik. 
 
 Anderzijds als we services maakt **fabric: / SpecialApp/ServiceA** en **fabric: / SpecialApp/ServiceB** met **gedeeld proces** model, Service Fabric wordt slechts één exemplaar van 'MultiTypeServicePackge' activeren voor *toepassing* **fabric: / SpecialApp** (zoals u hebt gezien eerder). Alle replica's voor service fungeert als host voor 'MyCodePackageA' **fabric: / SpecialApp/ServiceA** (of een service van het type 'MyServiceTypeA' nauwkeuriger) en 'MyCodePackageB' fungeert als host voor alle replica's voor service **fabric: / SpecialApp/XPb** (of een service van het type 'MyServiceTypeB' nauwkeuriger). Volgende diagram toont de weergave van het knooppunt in deze instelling: 

<center>
![Knooppunt-weergave van geïmplementeerde toepassing][node-view-six]
</center>

In het bovenstaande voorbeeld kunt u overwegen als 'MyCodePackageA' zowel 'MyServiceTypeA' en 'MyServiceTypeB registreert' en er is geen 'MyCodePackageB', dan zal er geen redundante *CodePackage* uitgevoerd. Dit klopt, echter, zoals eerder vermeld, dit toepassingsmodel komt niet overeen met **exclusieve proces** hostingmodel. Als het doel is om elke replica in een eigen toegewezen verwerkt, registreren, zowel *ServiceTypes* van dezelfde *CodePackage* is niet nodig en stellen elke *ServiceType* in een eigen *ServicePacakge* is een natuurlijke keuze.

## <a name="next-steps"></a>Volgende stappen
[Een toepassing van het pakket] [ a4] en voorbereidingen te implementeren.

[Implementeren en toepassingen verwijderen] [ a5] wordt beschreven hoe u PowerShell gebruiken om exemplaren van een toepassing beheren.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
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
