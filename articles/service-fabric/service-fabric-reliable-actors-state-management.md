---
title: Reliable Actors status management | Microsoft Docs
description: Hierin wordt beschreven hoe Reliable Actors status wordt beheerd, opgeslagen en gerepliceerd voor hoge beschikbaarheid.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 65dd47ab21ca4b1c50e0f17b73e7bc4eae8a96e8
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665777"
---
# <a name="reliable-actors-state-management"></a>Statusbeheer van betrouwbare actoren
Reliable Actors zijn single-threaded objecten die zowel de logica en de status kunnen inkapselen. Omdat actoren op Reliable Services worden uitgevoerd, kunnen deze status op betrouwbare wijze onderhouden met behulp van dezelfde persistentie en replicatiemechanismen voor. Op deze manier actoren dat u geen hun status na fouten bij het reactiveren na de garbagecollection, of wanneer ze worden verplaatst tussen knooppunten in een cluster vanwege de bron-balancing of upgrades worden uitgevoerd.

## <a name="state-persistence-and-replication"></a>Status persistentie en replicatie
Alle Reliable Actors worden beschouwd als *stateful* omdat elk exemplaar van de actor wordt toegewezen aan een unieke ID. Dit betekent dat herhaalde aanroepen naar dezelfde actor-ID worden gerouteerd naar hetzelfde exemplaar van de actor. In een staatloze systeem, daarentegen, zijn clientaanroepen niet noodzakelijkerwijs telkens naar dezelfde server worden gerouteerd. Actorservices zijn daarom altijd stateful services.

Hoewel actoren worden beschouwd als stateful, dat betekent niet dat de status op betrouwbare wijze moeten worden opgeslagen. Actoren kunnen ervoor kiezen het niveau van persistentie ingeschakeld en replicatie op basis van hun gegevens opslagvereisten:

* **Opgeslagen status**: Status is opgeslagen op schijf en worden gerepliceerd naar drie of meer replica's. Permanente status is de meest duurzame opslag-optie state waar status kunt behouden tot en met volledige cluster onderbreking.
* **Veranderlijke status**: De status is gerepliceerd naar drie of meer replica's en alleen in het geheugen worden bewaard. Veranderlijke status bevat tolerantie tegen knooppuntfout en actor mislukt, en tijdens upgrades en bron-balancing. Echter de status niet is opgeslagen op schijf. Dus als alle replica's verloren gaan in één keer, de status is verloren gegaan ook.
* **Er is geen permanente status**: Status niet is gerepliceerd of geschreven naar de schijf alleen gebruiken voor actors die niet hoeven te onderhouden, de status op betrouwbare wijze.

Elk niveau van persistentie is gewoon een andere *state-provider* en *replicatie* configuratie van uw service. Wel of niet de status wordt geschreven naar schijf is afhankelijk van de state-provider--het onderdeel in een betrouwbare service die wordt opgeslagen status. Replicatie is afhankelijk van het aantal replica's een-service wordt geïmplementeerd met. Net als bij betrouwbare Services, kunnen zowel de state-provider en het aantal replica's eenvoudig worden handmatig ingesteld. Het actorframework biedt een kenmerk dat, wanneer op een actor gebruikt, selecteert automatisch een standaard state-provider en instellingen voor het aantal replica's voor het bereiken van een van deze drie persistentie-instellingen worden automatisch gegenereerd. Het kenmerk StatePersistence is niet overgenomen door de afgeleide klasse, elk type Actor zijn StatePersistence niveau moet opgeven.

### <a name="persisted-state"></a>Permanente status
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Deze instelling maakt gebruik van een state-provider die gegevens worden opgeslagen op schijf en het aantal service-replica's automatisch ingesteld op 3.

### <a name="volatile-state"></a>Veranderlijke status
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Deze instelling maakt gebruik van een in-memory-only state-provider en wordt het aantal replica's op 3.

### <a name="no-persisted-state"></a>Er is geen permanente status
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Deze instelling maakt gebruik van een in-memory-only state-provider en wordt het aantal replica's ingesteld op 1.

### <a name="defaults-and-generated-settings"></a>Gegenereerde instellingen en standaardinstellingen
Wanneer u de `StatePersistence` kenmerk, een state-provider wordt automatisch geselecteerd voor u tijdens runtime wanneer de actorservice wordt gestart. Het aantal replica's echter is ingesteld bij het compileren van de hulpprogramma's voor Visual Studio actor build. Automatisch genereren van de build-hulpprogramma's een *standaardservice* voor de actorservice in ApplicationManifest.xml. Parameters worden gemaakt voor **grootte van de replicaset min** en **doelreplica grootte instellen**.

U kunt deze parameters handmatig wijzigen. Maar telkens wanneer de `StatePersistence` kenmerk wordt gewijzigd, de parameters zijn ingesteld op de replica set grootte standaardwaarden voor de geselecteerde `StatePersistence` kenmerk overschrijven van een vorige waarden. Met andere woorden, de waarden die u in ServiceManifest.xml instelt zijn *alleen* overschreven tijdens het opbouwen wanneer u wijzigt de `StatePersistence` waarde van het kenmerk.

```xml
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Status manager
Elke actor-exemplaar heeft een eigen status manager: een woordenlijst-achtige gegevensstructuur waarmee op betrouwbare wijze worden opgeslagen sleutel/waarde-paren. De status manager is een wrapper rond een state-provider. U kunt deze gebruiken voor het opslaan van gegevens, ongeacht welke persistentie instelling wordt gebruikt. Biedt geen garanties dat een actieve actorservice kan worden gewijzigd van een instelling vluchtige (in-memory-only) staat in de instelling van een permanente status via een rolling upgrade behoud van gegevens. Het is echter mogelijk aantal replica's voor een actieve service wijzigen.

Status manager sleutels moeten tekenreeksen zijn. Waarden zijn algemeen en kan elk type zijn, met inbegrip van aangepaste typen. Waarden die zijn opgeslagen in de status manager moet serializable-gegevenscontract omdat ze tijdens de replicatie via het netwerk naar andere knooppunten kunnen worden verzonden en kunnen worden geschreven naar de schijf, afhankelijk van de instelling van persistentie van een actor.

De status manager bevat algemene woordenlijst methoden voor het beheren van de status, vergelijkbaar met die in een betrouwbare Dictionary gevonden.

Lees voor meer voorbeelden van het beheer van de actorstatus [toegang, opslaan, en het verwijderen van Reliable Actors-status](service-fabric-reliable-actors-access-save-remove-state.md).

## <a name="best-practices"></a>Aanbevolen procedures
Hier volgen enkele aanbevolen procedures en tips voor probleemoplossing voor het beheren van de actorstatus.

### <a name="make-the-actor-state-as-granular-as-possible"></a>De actorstatus zo nauwkeurig mogelijk maken
Dit is essentieel voor prestaties en het Resourcegebruik van uw toepassing. Wanneer er sprake is van schrijven/updates moet uitvoeren op de 'met de naam status' van een actor, wordt de hele waarde die overeenkomt met die "naam"staat geserialiseerd en via het netwerk worden verzonden naar de secundaire replica's.  De secundaire replica's schrijven naar de lokale schijf en antwoord terug naar de primaire replica. Wanneer de primaire bevestigingen van een quorum van secundaire replica's ontvangt, worden de status van de geschreven naar de lokale schijf. Stel bijvoorbeeld dat de waarde is een klasse die 20 leden en een grootte van 1 MB is. Zelfs als u een van de klasseleden die alleen gewijzigd formaat van 1 KB, einde van het betalen van de kosten van serialisatie en netwerk- en schrijfbewerkingen voor de volledige 1 MB. Op dezelfde manier als de waarde is een verzameling (zoals een lijst, een matrix of een woordenlijst), betaalt u de kosten voor de volledige verzameling, zelfs als u een van de leden wijzigen. De StateManager-interface van de actorklasse is vergelijkbaar met een woordenlijst. U moet altijd de gegevensstructuur actorstatus boven op deze woordenlijst voor het model.
 
### <a name="correctly-manage-the-actors-life-cycle"></a>Levenscyclus van de actor correct beheren
U hebt duidelijk beleid over het beheren van de grootte van status in elke partitie van een actor-service. De actor-service moet een vast aantal actoren en ze zo veel mogelijk opnieuw te gebruiken. Als u voortdurend nieuwe actoren maakt, moet u ze verwijderen wanneer ze klaar bent met hun werk. Het actorframework slaat enkele metagegevens voor elke acteur aanwezig is. Verwijderen van de status van een actor verwijderd metagegevens over die actor niet. U moet de actor verwijderen (Zie [actoren en hun status verwijderen](service-fabric-reliable-actors-lifecycle.md#manually-deleting-actors-and-their-state)) verwijderen van alle informatie over deze opgeslagen in het systeem. Als een extra controle, moet u een query de actor-service (Zie [actoren opsommen](service-fabric-reliable-actors-enumerate.md)) en één keer te controleren of het aantal actors zijn binnen het verwachte bereik.
 
Als u ooit ziet dat de grootte van database van een Actor-Service worden steeds groter dan het verwachte, zorg ervoor dat u de voorgaande richtlijnen volgt. Als u deze richtlijnen volgt en nog steeds database problemen met de grootte van bestand zijn, moet u [open een ondersteuningsticket](service-fabric-support.md) met het productteam om hulp te krijgen.

## <a name="next-steps"></a>Volgende stappen

Status die opgeslagen in Reliable Actors moet achtereenvolgens worden uitgevoerd voordat de naar schijf worden geschreven en gerepliceerd voor hoge beschikbaarheid. Meer informatie over [Actor typeserialisatie](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Vervolgens meer informatie over [Actor diagnostische gegevens en bewaking van toepassingsprestaties](service-fabric-reliable-actors-diagnostics.md).
