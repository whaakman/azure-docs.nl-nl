---
title: Upgrade van service Fabric-App met behulp van PowerShell | Microsoft Docs
description: Dit artikel helpt bij de ervaring van een Service Fabric-toepassing implementeren, de code te wijzigen en implementeren van een upgrade met behulp van PowerShell.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 9bc75748-96b0-49ca-8d8a-41fe08398f25
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 54ca664a29ed8c6337bb27fe1fa17276e480c911
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>Upgrade van de service Fabric-toepassing met behulp van PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

De meestgebruikte en aanbevolen aanpak van de upgrade is de bewaakte rolling upgrade.  Azure Service Fabric controleert de status van de toepassing wordt bijgewerkt op basis van een reeks statusbeleid. Nadat de upgrade van een updatedomein (UD) wordt uitgevoerd, Service Fabric evalueert de toepassingsstatus en verloopt in het volgende updatedomein of de upgrade, afhankelijk van het statusbeleid is mislukt.

Een upgrade van de bewaakte toepassing kan worden uitgevoerd met behulp van de beheerde systeemeigen API's, PowerShell of REST. Zie voor instructies voor het uitvoeren van een upgrade met Visual Studio, [bijwerken van uw toepassing met Visual Studio](service-fabric-application-upgrade-tutorial.md).

Met Service Fabric bewaakt rollende upgrades, kan de beheerder van de toepassing de evaluatie van statusbeleid Service Fabric gebruikt om te bepalen of de toepassing in orde configureren. De beheerder configureren bovendien de actie moet worden gehouden bij de statusevaluatie mislukt (bijvoorbeeld, voeren een automatische terugdraaien.) Deze sectie helpt bij de upgrade van een bewaakte voor een van de SDK-voorbeelden die PowerShell gebruikt. De video volgende Microsoft Virtual Academy wordt u begeleid bij een upgrade van de app:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=OrHJH66yC_6406218965">
<img src="./media/service-fabric-application-upgrade-tutorial-powershell/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Stap 1: Bouwen en implementeren van de steekproef visuele objecten
Bouwen en de toepassing publiceren door met de rechtermuisknop op het toepassingsproject **VisualObjectsApplication,** en het selecteren van de **publiceren** opdracht.  Zie voor meer informatie [upgrade zelfstudie over Service Fabric](service-fabric-application-upgrade-tutorial.md).  U kunt ook PowerShell gebruiken om uw toepassing te implementeren.

> [!NOTE]
> Voordat een van de Service Fabric-opdrachten kunnen worden gebruikt in PowerShell, moet u eerst verbinding met het cluster met behulp van de `Connect-ServiceFabricCluster` cmdlet. Op deze manier wordt ervan uitgegaan dat het Cluster heeft al is ingesteld op uw lokale machine. Zie het artikel op [instellen van uw ontwikkelomgeving Service Fabric](service-fabric-get-started.md).
> 
> 

Nadat u het project in Visual Studio maakt, kunt u de PowerShell-opdracht [kopie ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) het toepassingspakket kopiëren naar de Installatiekopieopslag. Als u controleren of het app-pakket lokaal wilt, gebruikt u de [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) cmdlet. De volgende stap is het registreren van de toepassing met de Service Fabric-runtime via de [registreren ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) cmdlet. De volgende stap is het een exemplaar van de toepassing wordt gestart via de [nieuw ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.  Deze drie stappen zijn vergelijkbaar met behulp van de **implementeren** menu-item in Visual Studio.  Nadat het inrichten is voltooid, moet u het gekopieerde toepassingspakket uit het installatiekopiearchief opschonen om te reduceren van de resources verbruikt.  Als een toepassingstype niet langer vereist is, moet zijn geregistreerd om dezelfde reden. Zie [implementeren en verwijderen van toepassingen via PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) voor meer informatie.

U kunt nu [Service Fabric Explorer om het cluster en de toepassing weer te geven](service-fabric-visualizing-your-cluster.md). De toepassing heeft een webservice die kan worden genavigeerd in Internet Explorer door te typen [http://localhost: 8081/visualobjects](http://localhost:8081/visualobjects) in de adresbalk.  Hier ziet u enkele zwevende visual objecten navigeren in het scherm.  Bovendien kunt u [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) om de toepassingsstatus te controleren.

## <a name="step-2-update-the-visual-objects-sample"></a>Stap 2: Het voorbeeld visuele objecten bijwerken
U wellicht opgevallen dat door de versie die is geïmplementeerd in stap 1, visuele objecten niet draaien. Deze toepassing op een waar de visuele objecten ook draaien gaat upgraden.

Selecteer het project VisualObjects.ActorService binnen de oplossing VisualObjects en open het bestand StatefulVisualObjectActor.cs. In dat bestand navigeren naar de methode `MoveObject`, uitcommentariëren `this.State.Move()`, en verwijder het commentaarteken `this.State.Move(true)`. Deze wijziging worden de objecten na de upgrade van de service.

Er moet ook bijwerken de *ServiceManifest.xml* -bestand (onder PackageRoot) van het project **VisualObjects.ActorService**. Update de *CodePackage* en de serviceversie 2.0 en de bijbehorende regels in de *ServiceManifest.xml* bestand.
U kunt de Visual Studio *Manifest bestanden bewerken* optie nadat u met de rechtermuisknop op de oplossing om de wijzigingen van het manifestbestand.

Nadat de wijzigingen zijn aangebracht, het manifest moet eruitzien als in de volgende (de wijzigingen gemarkeerde gedeelten weergeven):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Nu de *ApplicationManifest.xml* bestand (gevonden onder de **VisualObjects** project onder de **VisualObjects** oplossing) wordt bijgewerkt naar versie 2.0 van het **VisualObjects.ActorService** project. Bovendien is versie van de toepassing bijgewerkt naar 2.0.0.0 van 1.0.0.0. De *ApplicationManifest.xml* moet eruitzien als in het volgende fragment:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```

Nu bouw het project door te selecteren alleen de **ActorService** project, en vervolgens met de rechtermuisknop op en selecteert de **bouwen** optie in Visual Studio. Als u selecteert **alles opnieuw opbouwen**, moet u de versies voor alle projecten, bijwerken, omdat de code zou is gewijzigd. Daarna laten we de bijgewerkte toepassing door met de rechtermuisknop op het pakket ***VisualObjectsApplication***, het Service Fabric-Menu selecteren en te kiezen **pakket**. Deze actie wordt gemaakt van een toepassingspakket dat kan worden geïmplementeerd.  De bijgewerkte toepassing is gereed om te worden geïmplementeerd.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Stap 3: Bepaal statusbeleid en parameters bijwerken
Vertrouwd raken met de [parameters voor het bijwerken van toepassing](service-fabric-application-upgrade-parameters.md) en de [upgradeproces](service-fabric-application-upgrade.md) ophalen van een goed begrip van de verschillende parameters voor het bijwerken, time-outs en health criterium toegepast. Voor dit scenario moet het criterium service health evaluatie is ingesteld op de standaardwaarde (en aanbevolen) waarden, wat betekent dat alle services en -exemplaren moeten *orde* na de upgrade.  

We vergroot echter de *HealthCheckStableDuration* tot 60 seconden (zodat de services zijn in orde voor ten minste 20 seconden voordat de upgrade wordt uitgevoerd aan de volgende updatedomein).  Hiermee kunt u ook instellen de *UpgradeDomainTimeout* 1200 seconden worden en de *UpgradeTimeout* 3000 seconden zijn.

Tot slot gaan we ook stelt de *UpgradeFailureAction* terug te draaien. Deze optie vereist Service Fabric ongedaan te maken de toepassing naar de vorige versie als er problemen tijdens de upgrade aangetroffen. Dus bij het starten van de upgrade (in stap 4), zijn de volgende parameters opgeven:

FailureAction = terugdraaien

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec 1200 =

UpgradeTimeout = 3000

## <a name="step-4-prepare-application-for-upgrade"></a>Stap 4: De toepassing voor een upgrade voorbereiden
De toepassing is nu gemaakt en gereed voor upgrade worden uitgevoerd. Als u het openen van een PowerShell-venster als beheerder en typ [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), het laat u weten dat het toepassingstype 1.0.0.0 van **VisualObjects** die zijn geïmplementeerd.  

Het toepassingspakket is opgeslagen in het volgende relatieve pad waar u de Service Fabric SDK - ongecomprimeerde *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. U moet een pakketmap '' vinden in die map waar het toepassingspakket is opgeslagen. Controleer de tijdstempels om ervoor te zorgen dat deze de laatste build (mogelijk moet u de paden op de juiste wijze ook wijzigen).

Nu gaan we de bijgewerkte toepassing-pakket kopiëren naar de Service Fabric-Installatiekopieopslag (waar de toepassingspakketten zijn opgeslagen door de Fabric-Service). De parameter *ApplicationPackagePathInImageStore* informeert Service Fabric waar deze het toepassingspakket kan vinden. We hebben de bijgewerkte toepassing plaatsen ' VisualObjects\_V2 "met de volgende opdracht (mogelijk moet u opnieuw op de juiste wijze paden wijzigen).

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

De volgende stap is deze toepassing registreren met Service Fabric, die kan worden uitgevoerd met behulp van de [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) opdracht:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Als de voorgaande opdracht niet lukt, is het waarschijnlijk dat u het opbouwen van alle services moet. Zoals vermeld in stap 2, moet u wellicht uw WebService-versie bijwerken.

Het verdient aanbeveling dat u het toepassingspakket verwijderen nadat de toepassing is geregistreerd.  Toepassingspakketten verwijderen uit de image store vrijgemaakt systeembronnen.  Schijfopslag verbruikt en leidt tot een toepassing prestatieproblemen in ongebruikte toepassingspakketten houden.

```powershell
Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore "VisualObjects\_V2" -ImageStoreConnectionString fabric:ImageStore
```

## <a name="step-5-start-the-application-upgrade"></a>Stap 5: Start de upgrade van de toepassing
Nu we alles is voorbereid voor de upgrade van de toepassing wordt gestart via de [Start ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) opdracht:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Naam van de toepassing is hetzelfde als het is beschreven in de *ApplicationManifest.xml* bestand. Deze naam wordt gebruikt om te bepalen welke toepassing getting upgrade van service Fabric. Als u de time-outs te kort worden ingesteld, kunt u een foutbericht waarin wordt aangegeven van het probleem tegenkomen. Raadpleeg het gedeelte voor probleemoplossing of vergroot de time-outs.

Nu als de upgrade opbrengst toepassing controleren met behulp van Service Fabric Explorer of met behulp van de [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) PowerShell-opdracht: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

In een paar minuten, die u hebt verkregen met behulp van de voorgaande PowerShell-opdracht, zou de status dat alle domeinen van de update zijn bijgewerkt (voltooid). En u ziet dat de visuele objecten in uw browservenster hebt gestart roteren!

Een upgrade van versie 2 naar versie 3 of van versie 2 naar versie 1 wijze van oefening maakt, kunt u proberen. Verplaatsen van versie 2 van versie 1 wordt ook beschouwd als een upgrade. Afspelen met time-outs en statusbeleid voor uzelf bekend mee te maken. Wanneer u een Azure-cluster implementeert, moeten de parameters op de juiste wijze worden ingesteld. Is het raadzaam om de time-outs conservatieve ingesteld.

## <a name="next-steps"></a>Volgende stappen
[Bijwerken van uw toepassing met Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.

Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [parameters upgrade](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibel maken door te leren hoe u [serialisatie van gegevens](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [probleemoplossing toepassingsupgrades](service-fabric-application-upgrade-troubleshooting.md).

