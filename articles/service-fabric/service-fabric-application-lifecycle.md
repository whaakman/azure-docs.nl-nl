---
title: Levensduur van toepassingen in Service Fabric | Microsoft Docs
description: Hierin wordt beschreven ontwikkelen, implementeren, testen, upgraden, onderhouden en verwijderen van Service Fabric-toepassingen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 08837cca-5aa7-40da-b087-2b657224a097
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/19/2018
ms.author: ryanwi
ms.openlocfilehash: 65104503af2e177f1898d8509c2d82bd9b58c266
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57446959"
---
# <a name="service-fabric-application-lifecycle"></a>Service Fabric-toepassingslevenscyclus
Wanneer een toepassing in Azure Service Fabric met andere platforms, meestal niet via de volgende fasen: ontwerp, ontwikkeling, testen, implementeren, upgraden, onderhoud en verwijderen. Service Fabric biedt uitstekende ondersteuning voor de volledige levenscyclus van cloudtoepassingen, van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud en uiteindelijk buiten gebruik stellen. Het service-model kunt meerdere verschillende rollen om deel te nemen onafhankelijk van elkaar in de levensduur van toepassingen. Dit artikel bevat een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen in de fasen van de levensduur van de Service Fabric-toepassingen.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-sfctl.md)]

## <a name="service-model-roles"></a>Service model-rollen
De service model-rollen zijn:

* **Service-ontwikkelaar**: Modulaire en algemene services die kunnen worden nieuwe bestemming krijgen en gebruikt in meerdere aanvragen van hetzelfde type of verschillende typen ontwikkelt. Bijvoorbeeld, kan een queue-service worden gebruikt voor het maken van een ticketsysteem toepassing (helpdesk) of een e-commerce-toepassing (winkelwagen).
* **Toepassingsontwikkelaar**: Hiermee maakt u toepassingen door de integratie van een verzameling van services om te voldoen aan bepaalde specifieke vereisten of scenario's. Bijvoorbeeld in een e-commerce-website kan worden geïntegreerd met 'JSON staatloze Front-End Service', 'Aanbieding Stateful Service' en 'Wachtrij Stateful Service' om een auctioning oplossing te bouwen.
* **Toepassingsbeheerder**: Beslissingen over de configuratie van de toepassing (invullen in de sjabloonparameters configuratie), implementatie (toewijzing met de beschikbare bronnen) en quality of service. Bijvoorbeeld, besluit een beheerder van de toepassing de taalinstelling van de toepassing (in het Engels voor de Verenigde Staten) of voor Japan, bijvoorbeeld de Japanse versie. Een andere geïmplementeerde toepassing kan verschillende instellingen hebben.
* **Operator**: Toepassingen op basis van de configuratie van de toepassing en die zijn opgegeven door de beheerder van de toepassing implementeert. Bijvoorbeeld, een operator bepalingen en implementeert de toepassing en zorgt ervoor dat deze wordt uitgevoerd in Azure. Operators informatie over de status en prestaties van toepassingen bewaken en onderhouden van de fysieke infrastructuur, indien nodig.

## <a name="develop"></a>Ontwikkelen
1. Een *service developer* ontwikkelt verschillende soorten services met behulp van de [Reliable Actors](service-fabric-reliable-actors-introduction.md) of [Reliable Services](service-fabric-reliable-services-introduction.md) programmeermodel.
2. Een *service developer* declaratief wordt beschreven welke servicetypen ontwikkelde in een servicemanifest die bestaan uit een of meer code, configuratie en gegevens-pakketten.
3. Een *toepassingsontwikkelaar* bouwt vervolgens een toepassing met behulp van verschillende servicetypen.
4. Een *toepassingsontwikkelaar* declaratief beschrijving van het toepassingstype in manifest van de toepassing door te verwijzen naar de servicemanifesten van de bijbehorende services en op de juiste wijze te overschrijven en parametriseren verschillende configuratie en implementatie-instellingen van de bijbehorende services.

Zie [aan de slag met Reliable Actors](service-fabric-reliable-actors-get-started.md) en [aan de slag met Reliable Services](service-fabric-reliable-services-quick-start.md) voor voorbeelden.

## <a name="deploy"></a>Implementeren
1. Een *toepassingsbeheerder* aangepast, zodat deze het toepassingstype naar een specifieke toepassing worden geïmplementeerd voor een Service Fabric-cluster door op te geven de juiste parameters van de **ApplicationType** element in het toepassingsmanifest.
2. Een *operator* wordt het toepassingspakket geüpload naar de installatiekopieopslag van het cluster met behulp van de [ **CopyApplicationPackage** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) of de [  **Kopiëren-ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Het toepassingspakket bevat het toepassingsmanifest en het verzamelen van service-pakketten. Service Fabric implementeert-toepassingen van het toepassingspakket dat is opgeslagen in de installatiekopieopslag, dit kan een Azure blob-archief of de Service Fabric service.
3. De *operator* vervolgens richt het toepassingstype in de doelcluster op basis van de geüploade toepassingspakket met de [ **ProvisionApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), wordt de  [ **Registreren ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/register-servicefabricapplicationtype), of de [ **inrichten van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
4. Na het inrichten van de toepassing, een *operator* de toepassing wordt gestart met de parameters die is opgegeven door de *toepassingsbeheerder* met behulp van de [  **CreateApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), wordt de [ **New-ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication), of de [ **toepassing maken**  REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/create-an-application).
5. Nadat de toepassing is geïmplementeerd, een *operator* maakt gebruik van de [ **CreateServiceAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient), wordt de [ **New-ServiceFabricService**  cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice), of de [ **maken van een Service** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/create-a-service) te maken van nieuwe service-exemplaren voor de toepassing op basis van de beschikbare servicetypen.
6. De toepassing wordt nu uitgevoerd in de Service Fabric-cluster.

Zie [implementeren van een toepassing](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="test"></a>Testen
1. Na de implementatie van het lokale ontwikkelcluster of een testcluster, een *service developer* wordt de ingebouwde failoverscenario voor de test uitgevoerd met behulp van de [ **FailoverTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenarioparameters) en [ **FailoverTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenario) klassen, of de [ **Invoke-ServiceFabricFailoverTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricfailovertestscenario?view=azureservicefabricps). Het scenario voor het testen van failover wordt uitgevoerd van een opgegeven service via belangrijk overgangen en failovers om ervoor te zorgen dat het is nog steeds beschikbaar zijn en werken.
2. De *service developer* voert vervolgens de ingebouwde chaos test scenario met behulp van de [ **ChaosTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenarioparameters) en [  **ChaosTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenario) klassen, of de [ **Invoke-ServiceFabricChaosTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricchaostestscenario?view=azureservicefabricps). Het scenario voor het testen van chaos induceert willekeurig meerdere knooppunten, codepakket en replica-fouten in het cluster.
3. De *service developer* [service-naar-servicecommunicatie testen](service-fabric-testability-scenarios-service-communication.md) door het ontwerpen van testscenario's voor die primaire replica om het cluster worden verplaatst.

Zie [Inleiding tot de Fault Analysis Service](service-fabric-testability-overview.md) voor meer informatie.

## <a name="upgrade"></a>Upgraden
1. Een *service developer* updates van het onderdeel services van de toepassing geïnstantieerd en/of corrigeert bugs en biedt een nieuwe versie van het servicemanifest.
2. Een *toepassingsontwikkelaar* onderdrukkingen en de configuratie en implementatie-instellingen van de consistente services parameterizes en biedt een nieuwe versie van het toepassingsmanifest. Ontwikkelaar van de toepassing wordt vervolgens de nieuwe versies van de servicemanifesten opgenomen in de toepassing en biedt een nieuwe versie van het toepassingstype in een bijgewerkte App-pakket.
3. Een *toepassingsbeheerder* bevat de nieuwe versie van het toepassingstype in de doeltoepassing door bij te werken van de juiste parameters.
4. Een *operator* uploadt het bijgewerkte App-pakket met het cluster installatiekopie store via de [ **CopyApplicationPackage** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) of de [ **Kopiëren ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Het toepassingspakket bevat het toepassingsmanifest en het verzamelen van service-pakketten.
5. Een *operator* bepalingen van de nieuwe versie van de toepassing in het doelcluster met behulp van de [ **ProvisionApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), wordt de [  **Register-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/register-servicefabricapplicationtype), of de [ **inrichten van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
6. Een *operator* upgrades van de doeltoepassing met de nieuwe versie via de [ **UpgradeApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), wordt de [  **Start-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade), of de [ **upgraden van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/upgrade-an-application).
7. Een *operator* controleert de voortgang van het gebruik van de upgrade de [ **GetApplicationUpgradeProgressAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), wordt de [  **Get-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricapplicationupgrade), of de [ **Application Upgrade wordt uitgevoerd** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/get-the-progress-of-an-application-upgrade1).
8. Indien nodig, de *operator* wijzigt en de parameters van de huidige toepassing site upgraden met de [ **UpdateApplicationUpgradeAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), de [ **Update ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade), of de [ **Update Application Upgrade** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/update-an-application-upgrade).
9. Indien nodig, de *operator* teruggedraaid van de huidige toepassing upgraden met de [ **RollbackApplicationUpgradeAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), wordt de [ **Start ServiceFabricApplicationRollback** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback), of de [ **terugdraaien van de toepassing upgraden** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/rollback-an-application-upgrade).
10. Service Fabric voert u een upgrade van de doeltoepassing zonder verlies van de beschikbaarheid van de services in het cluster wordt uitgevoerd.

Zie de [Application upgrade zelfstudie](service-fabric-application-upgrade-tutorial.md) voor voorbeelden.

## <a name="maintain"></a>Onderhouden
1. Voor het besturingssysteemupgrades en patches, Service Fabric-interfaces met de Azure-infrastructuur om te waarborgen van beschikbaarheid van alle toepassingen die worden uitgevoerd in het cluster.
2. Voor upgrades en patches voor het Service Fabric-platform, Service Fabric voert een upgrade zelf zonder verlies van beschikbaarheid van een van de toepassingen die worden uitgevoerd op het cluster.
3. Een *toepassingsbeheerder* goedkeurt, het toevoegen of verwijderen van knooppunten uit een cluster nadat het analyseren van gebruiksgegevens historische capaciteit en de verwachte toekomstige vraag.
4. Een *operator* toegevoegd en verwijderd van knooppunten die zijn opgegeven door de *toepassingsbeheerder*.
5. Wanneer nieuwe knooppunten worden toegevoegd aan of de bestaande knooppunten zijn verwijderd uit het cluster, Service Fabric automatisch taakverdelingen van de actieve toepassingen voor alle knooppunten in het cluster te bereiken van optimale prestaties.

## <a name="remove"></a>Verwijderen
1. Een *operator* kunt een specifiek exemplaar van een actieve service in het cluster verwijderen zonder te verwijderen van de volledige reeks toepassingen met behulp van de [ **DeleteServiceAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient), wordt de [ **Remove-ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice), of de [ **verwijderen van een Service** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/delete-a-service).  
2. Een *operator* kunt ook verwijderen exemplaar van een toepassing en alle bijbehorende services met behulp van de [ **DeleteApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), wordt de [  **Remove-ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricapplication), of de [ **Delete Application** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/delete-an-application).
3. Nadat de toepassing en services zijn gestopt, de *operator* kunt inrichting verwijderen van de toepassing met behulp van de [ **UnprovisionApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), de [ **Unregister-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/unregister-servicefabricapplicationtype), of de [ **inrichting verwijderen van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/unprovision-an-application). Het toepassingstype, wordt het toepassingspakket van de ImageStore niet verwijderd. Het toepassingspakket moet u handmatig verwijderen.
4. Een *operator* Hiermee verwijdert u het toepassingspakket uit de ImageStore met behulp van de [ **RemoveApplicationPackage** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) of de [  **Remove-ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps).

Zie [implementeren van een toepassing](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over het ontwikkelen, Zie testen en beheren van Service Fabric-toepassingen en services:

* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)
* [Toepassingsupgrade](service-fabric-application-upgrade.md)
* [Overzicht testbaarheid](service-fabric-testability-overview.md)
