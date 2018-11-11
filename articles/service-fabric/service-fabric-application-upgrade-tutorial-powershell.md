---
title: Upgrade van service Fabric-toepassing met behulp van PowerShell | Microsoft Docs
description: Dit artikel helpt bij de ervaring van de implementatie van een Service Fabric-toepassing, de code te wijzigen en implementeren van een upgrade met behulp van PowerShell.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: 9bc75748-96b0-49ca-8d8a-41fe08398f25
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 0f134bdb4f77034dd124027fc960d172d25db721
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515315"
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>Upgrade van de service Fabric-toepassing met behulp van PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

De meestgebruikte en aanbevolen upgrade benadering is van de bewaakte rolling upgrade.  Azure Service Fabric controleert de status van de toepassing een upgrade wordt uitgevoerd op basis van een set statusbeleid. Zodra een updatedomein (UD) is geüpgraded, Service Fabric wordt geëvalueerd als de status van de toepassing en gaat deze naar het volgende updatedomein of de upgrade, afhankelijk van het statusbeleid is mislukt.

Een upgrade van de bewaakte toepassing kan worden uitgevoerd met behulp van de beheerde of systeemeigen API's, PowerShell, Azure CLI, Java of REST. Zie voor instructies over het uitvoeren van een upgrade met Visual Studio, [bijwerken van uw toepassing met Visual Studio](service-fabric-application-upgrade-tutorial.md).

Met Service Fabric bewaakt rolling upgrades, kan de beheerder van de toepassing het statusbeleid voor evaluatie die gebruikmaakt van Service Fabric om te bepalen of de toepassing in orde configureren. Bovendien configureren de beheerder de actie die moet worden uitgevoerd wanneer de status evaluatie is mislukt (bijvoorbeeld, voeren een automatische terugdraaien.) Dit gedeelte doorloopt samen door middel van een bewaakte upgrade voor een van de SDK-voorbeelden die gebruikmaakt van PowerShell. De video volgende Microsoft Virtual Academy leidt u door een app-upgrade: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=OrHJH66yC_6406218965">
<img src="./media/service-fabric-application-upgrade-tutorial-powershell/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Stap 1: Bouw en implementeer een voorbeeld van de Visual objecten
Bouwt en publiceert u de toepassing met de rechtermuisknop op het toepassingsproject **VisualObjectsApplication,** en selecteren van de **publiceren** opdracht.  Zie voor meer informatie, [Service Fabric application upgrade zelfstudie](service-fabric-application-upgrade-tutorial.md).  U kunt ook PowerShell gebruiken om uw toepassing te implementeren.

> [!NOTE]
> Voordat een van de Service Fabric-opdrachten kunnen worden gebruikt in PowerShell, moet u eerst verbinding met het cluster met behulp van de `Connect-ServiceFabricCluster` cmdlet. Op deze manier wordt ervan uitgegaan dat het Cluster is al ingesteld op uw lokale computer. Zie het artikel op [instellen van uw Service Fabric-ontwikkelomgeving](service-fabric-get-started.md).
> 
> 

Na het maken van het project in Visual Studio, kunt u de PowerShell-opdracht [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage) het toepassingspakket kopiëren naar de ImageStore. Als u controleren of het app-pakket lokaal wilt, gebruikt u de [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage) cmdlet. De volgende stap is het registreren van de toepassing met de Service Fabric-runtime via de [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype) cmdlet. De volgende stap is het starten van een exemplaar van de toepassing met behulp van de [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.  Deze drie stappen zijn vergelijkbaar met behulp van de **implementeren** menu-item in Visual Studio.  Nadat het inrichten is voltooid, moet u het gekopieerde toepassingspakket uit het installatiekopiearchief opschonen om te reduceren van de resources die nodig zijn.  Als een toepassingstype niet langer vereist is, moet dit ongedaan gemaakt zijn om dezelfde reden. Zie [implementeren en remove-toepassingen met behulp van PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) voor meer informatie.

Nu kunt u [Service Fabric Explorer om het cluster en de toepassing weer te geven](service-fabric-visualizing-your-cluster.md). De toepassing heeft een webservice die kan worden genavigeerd in Internet Explorer door te typen [ http://localhost:8081/visualobjects ](http://localhost:8081/visualobjects) in de adresbalk.  Hier ziet u enkele zwevende visual objecten in het scherm bladeren.  Bovendien kunt u [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) om de toepassingsstatus te controleren.

## <a name="step-2-update-the-visual-objects-sample"></a>Stap 2: Het voorbeeld Visual objecten bijwerken
U zult merken dat met de versie die is geïmplementeerd in stap 1, de visual objecten niet draaien. Laten we deze toepassing op een waar de visual objecten ook draaien upgraden.

Selecteer het project VisualObjects.ActorService binnen de oplossing VisualObjects en open het bestand StatefulVisualObjectActor.cs. In dat bestand navigeren naar de methode `MoveObject`, commentaar `this.State.Move()`, en verwijder het commentaarteken `this.State.Move(true)`. Deze wijziging worden de objecten na de upgrade van de service.

We ook nodig hebt om bij te werken de *ServiceManifest.xml* bestand (onder PackageRoot) van het project **VisualObjects.ActorService**. Update de *CodePackage* en de serviceversie 2.0 en de overeenkomende regels in de *ServiceManifest.xml* bestand.
U kunt de Visual Studio *Manifest van de bestanden bewerken* optie nadat u met de rechtermuisknop op de oplossing voor het aanbrengen van de manifest-bestand.

Nadat de wijzigingen zijn aangebracht, het manifest moet eruitzien als de volgende (gemarkeerde gedeelten weergeven van de wijzigingen):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Nu de *ApplicationManifest.xml* bestand (te vinden onder de **VisualObjects** project onder de **VisualObjects** oplossing) is bijgewerkt naar versie 2.0 van de **VisualObjects.ActorService** project. Bovendien is de versie van de toepassing bijgewerkt naar 2.0.0.0 van 1.0.0.0. De *ApplicationManifest.xml* moet eruitzien als in het volgende codefragment:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```

Nu, bouw het project door te selecteren alleen de **ActorService** project, en vervolgens met de rechtermuisknop op en selecteer de **bouwen** optie in Visual Studio. Als u selecteert **alles opnieuw opbouwen**, moet u de versies voor alle projecten, bijwerken, omdat de code zou zijn gewijzigd. Vervolgens gaan we de bijgewerkte toepassing verpakken met de rechtermuisknop op ***VisualObjectsApplication***, het selecteren van het Service Fabric-Menu en kiezen **pakket**. Deze actie wordt gemaakt van een toepassingspakket dat kan worden geïmplementeerd.  Uw bijgewerkte toepassing is gereed om te worden geïmplementeerd.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Stap 3: Statusbeleid hebt bepaald en Upgradeparameters
Maak uzelf vertrouwd met de [parameters toepassingsupgrade](service-fabric-application-upgrade-parameters.md) en de [upgradeproces](service-fabric-application-upgrade.md) om op te halen van een goed begrip van de verschillende parameters voor het bijwerken, time-outs en health criterium toegepast. Voor dit scenario is het criterium voor evaluatie van health service ingesteld op de standaardwaarde (en aanbevolen) waarden, wat betekent dat alle services en -exemplaren moeten zijn *in orde* na de upgrade.  

Maar laten we vergroten de *HealthCheckStableDuration* 180 seconden (zodat de services van ten minste 120 seconden voordat de upgrade wordt uitgevoerd op het volgende updatedomein in orde zijn).  We gaan ook instellen de *UpgradeDomainTimeout* moet 1200 seconden en de *UpgradeTimeout* 3000 seconden zijn.

Tot slot gaan we ook stelt de *UpgradeFailureAction* terug te draaien. Deze optie is vereist voor Service Fabric om terug te draaien de toepassing naar de vorige versie als er problemen ondervindt tijdens de upgrade. Dus bij het starten van de upgrade (in stap 4), zijn de volgende parameters opgegeven:

FailureAction = terugdraaien

HealthCheckStableDurationSec 180 =

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000

## <a name="step-4-prepare-application-for-upgrade"></a>Stap 4: Een toepassing voorbereiden voor upgrade
De toepassing is nu gebouwd en klaar om te worden bijgewerkt. Als u het openen van een PowerShell-venster als beheerder en typ [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), dit laat u weten dat het toepassingstype 1.0.0.0 van **VisualObjects** die zijn geïmplementeerd.  

Het toepassingspakket worden opgeslagen onder het volgende relatieve pad waar u de Service Fabric-SDK - ongecomprimeerd *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Een map 'Package' vindt u in die map waar het toepassingspakket is opgeslagen. Controleer de tijdstempels om ervoor te zorgen dat deze de meest recente build (mogelijk moet u de paden op de juiste wijze ook wijzigen).

Nu gaan we de bijgewerkte toepassing-pakket kopiëren naar de Service Fabric-ImageStore (waar de toepassingspakketten zijn opgeslagen door de Service Fabric). De parameter *ApplicationPackagePathInImageStore* informeert van Service Fabric, waar deze het toepassingspakket kan vinden. We hebben de bijgewerkte toepassing plaatsen ' VisualObjects\_V2 ' met de volgende opdracht (mogelijk moet u opnieuw op de juiste wijze paden wijzigen).

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

De volgende stap bestaat uit het registreren van deze toepassing met Service Fabric, die kunnen worden uitgevoerd met behulp van de [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) opdracht:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Als de voorgaande opdracht niet lukt, is het waarschijnlijk dat u samenstellen van alle services moet. Zoals vermeld in stap 2, is het wellicht ook de versie van uw WebService bij te werken.

Het verdient aanbeveling dat u het toepassingspakket verwijderen nadat de registratie van de toepassing is geslaagd.  Toepassingspakketten verwijderen uit de installatiekopieopslag vrijgemaakt systeemresources.  Niet-gebruikte toepassingspakketten disk-opslag verbruikt en leidt tot prestatieproblemen van toepassing.

```powershell
Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore "VisualObjects\_V2" -ImageStoreConnectionString fabric:ImageStore
```

## <a name="step-5-start-the-application-upgrade"></a>Stap 5: Start de upgrade van de toepassing
Nu kunnen we gaan de upgrade van de toepassing met behulp van de [Start ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) opdracht:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Naam van de toepassing is hetzelfde als deze is beschreven in de *ApplicationManifest.xml* bestand. Deze naam wordt gebruikt om te bepalen welke toepassing is bijgewerkt van service Fabric. Als u de time-outs te kort zijn ingesteld, kunt u een foutbericht dat het probleem tegenkomen. Raadpleeg de sectie probleemoplossing of vergroot de time-outs.

Nu als de upgrade opbrengst van toepassing, kunt u controleren met behulp van Service Fabric Explorer of met behulp van de [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) PowerShell-opdracht: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

In een paar minuten, die u hebt verkregen met behulp van de vorige PowerShell-opdracht, zou de status dat alle updatedomeinen zijn bijgewerkt (voltooid). En u ziet dat de visuele objecten in het browservenster hebt gestart draaien!

U kunt een upgrade van versie 2 naar versie 3 of van versie 2 op versie 1 bij wijze van oefening. Verplaatsen van versie 2 op versie 1 wordt ook beschouwd als een upgrade. Speel met time-outs en statusbeleid om uzelf bekend met hen. Wanneer u een Azure-cluster implementeert, moeten de parameters op de juiste wijze worden ingesteld. Het is goed om in te stellen de time-outs voorzichtig.

## <a name="next-steps"></a>Volgende stappen
[Bijwerken van uw toepassing met Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij de upgrade van een toepassing met Visual Studio.

Bepalen hoe uw toepassing met behulp van upgrades [Upgradeparameters](service-fabric-application-upgrade-parameters.md).

Upgrades van uw toepassingen compatibel maken door te leren hoe u [gegevensserialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Oplossen van veelvoorkomende problemen in upgrades van toepassingen door te verwijzen naar de stappen in [problemen met toepassingsupgrades oplossen](service-fabric-application-upgrade-troubleshooting.md).

