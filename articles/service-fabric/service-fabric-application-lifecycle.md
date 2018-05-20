---
title: De levenscyclus van de toepassing in Service Fabric | Microsoft Docs
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
ms.openlocfilehash: e7dddfca3640615cb851fb6dce9eaa80260ccbf6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-application-lifecycle"></a>De levenscyclus van de service Fabric-toepassing
Zoals met andere platforms wordt een toepassing op Azure Service Fabric meestal de volgende fasen doorloopt: ontwerpen, ontwikkelen, testen, implementatie, upgraden, onderhoud en verwijderen. Service Fabric biedt uitstekende ondersteuning voor de levenscyclus van de volledige toepassing van cloud-toepassingen, van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud voor uiteindelijke buiten gebruik stellen. Het service-model kan verschillende verschillende rollen onafhankelijk deelnemen aan de levenscyclus van de toepassing. Dit artikel bevat een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen in de fasen van de levenscyclus van de Service Fabric-toepassing.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-sfctl.md)]

De volgende video voor Microsoft Virtual Academy wordt beschreven hoe voor het beheren van de levenscyclus van uw toepassing: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-application-lifecycle/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="service-model-roles"></a>Service model-rollen
De service model-rollen zijn:

* **Service-ontwikkelaar**: modulaire en algemene services die kunnen worden opnieuw toegewezen en gebruikt in meerdere aanvragen van hetzelfde type of verschillende typen ontwikkelt. Queue-service kan bijvoorbeeld worden gebruikt voor het maken van een ticketsysteem toepassing (helpdesk) of een e-commerce-toepassing (winkelwagen).
* **Toepassingsontwikkelaar**: maakt toepassingen door de integratie van een verzameling services om te voldoen aan bepaalde specifieke vereisten of scenario's. Bijvoorbeeld 'JSON Stateless Front-End-Service,' mogelijk integreren van een webwinkel 'Aanbieding Stateful Service' en 'Wachtrij Stateful Service' om een auctioning oplossing te bouwen.
* **Toepassingsbeheerder**: beslissingen over de configuratie van de toepassing (invullen van de sjabloon configuratieparameters), implementatie (toewijzing met de beschikbare bronnen) en quality of service. Bijvoorbeeld, besluit een beheerder van de toepassing de taalinstelling (Engels voor de Verenigde Staten) of Japanse voor Japan, bijvoorbeeld van de toepassing. Een andere geïmplementeerde toepassing kan verschillende instellingen hebben.
* **Operator**: toepassingen op basis van de configuratie van de toepassing en de vereisten die zijn opgegeven door de beheerder van de toepassing wordt geïmplementeerd. Bijvoorbeeld, een operator inricht en implementeert de toepassing en zorgt ervoor dat deze wordt uitgevoerd in Azure. Operators informatie over de status en prestaties van de toepassing te bewaken en onderhouden van de fysieke infrastructuur zo nodig.

## <a name="develop"></a>Ontwikkelen
1. Een *service developer* ontwikkelt verschillende soorten services met behulp van de [Reliable Actors](service-fabric-reliable-actors-introduction.md) of [Reliable Services](service-fabric-reliable-services-introduction.md) programmeermodel.
2. Een *service developer* declaratief beschrijft de ontwikkelde servicetypen in een service-manifestbestand die bestaan uit een of meer code, configuratie en gegevens pakketten.
3. Een *toepassingsontwikkelaar* maakt vervolgens een toepassing met behulp van verschillende typen.
4. Een *toepassingsontwikkelaar* declaratief beschrijving van het toepassingstype in het manifest van de toepassing door die verwijzen naar de service manifesten samenstellende Services en op de juiste wijze te overschrijven en parameters voorzien van verschillende instellingen voor de configuratie en implementatie van het onderdeel services.

Zie [aan de slag met Reliable Actors](service-fabric-reliable-actors-get-started.md) en [aan de slag met Reliable Services](service-fabric-reliable-services-quick-start.md) voor voorbeelden.

## <a name="deploy"></a>Implementeren
1. Een *toepassingsbeheerder* aangepast, zodat deze het toepassingstype naar een specifieke toepassing worden geïmplementeerd voor een Service Fabric-cluster door te geven van de juiste parameters van de **ApplicationType** element in het toepassingsmanifest.
2. Een *operator* het toepassingspakket uploadt naar het archief van de cluster-installatiekopie met behulp van de [ **CopyApplicationPackage** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) of de [ **kopie ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Het toepassingspakket bevat manifest voor de toepassing en de verzameling van servicepakketten. Service Fabric worden toepassingen van het toepassingspakket dat is opgeslagen in de image store, die een Azure blob-store of de Service Fabric-systeemservice kan zijn geïmplementeerd.
3. De *operator* voorziet in het toepassingstype in het doelcluster van de geüploade toepassingspakket met de [ **ProvisionApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **registreren ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), of de [ **inrichten van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
4. Na het inrichten van de toepassing een *operator* de toepassing wordt gestart met de parameters die worden geleverd door de *toepassingsbeheerder* met behulp van de [ **CreateApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CreateApplicationAsync_System_Fabric_Description_ApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **nieuw ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricapplication), of de [ **toepassing maken** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/create-an-application).
5. Nadat de toepassing is geïmplementeerd, een *operator* maakt gebruik van de [ **CreateServiceAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_CreateServiceAsync_System_Fabric_Description_ServiceDescription_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **nieuw ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice), of de [ **maken van een Service** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/create-a-service) voor het maken van nieuwe service-exemplaren voor de toepassing op basis van beschikbare service-typen.
6. De toepassing wordt nu uitgevoerd in het Service Fabric-cluster.

Zie [implementeren van een toepassing](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="test"></a>Testen
1. Na implementatie van op de lokaal ontwikkelcluster of een testcluster, een *service developer* de Testscenario ingebouwde failover wordt uitgevoerd met de [ **FailoverTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenarioparameters#System_Fabric_Testability_Scenario_FailoverTestScenarioParameters) en [ **FailoverTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenario#System_Fabric_Testability_Scenario_FailoverTestScenario) klassen, of de [ **Invoke-ServiceFabricFailoverTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricfailovertestscenario?view=azureservicefabricps). Het scenario voor het testen van failover wordt uitgevoerd een opgegeven service via belangrijk overgangen en failovers om ervoor te zorgen dat het is nog steeds beschikbaar zijn en werken.
2. De *service developer* voert de ingebouwde chaos test scenario met behulp van de [ **ChaosTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenarioparameters#System_Fabric_Testability_Scenario_ChaosTestScenarioParameters) en [ **ChaosTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenario#System_Fabric_Testability_Scenario_ChaosTestScenario) klassen, of de [ **Invoke-ServiceFabricChaosTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricchaostestscenario?view=azureservicefabricps). De Testscenario chaos induceert willekeurig meerdere knooppunt codepakket en replica-fouten in het cluster.
3. De *service developer* [communicatie van de service-naar-service wordt getest](service-fabric-testability-scenarios-service-communication.md) door het ontwerpen van testscenario's voor die primaire replica's om het cluster verplaatsen.

Zie [Inleiding tot de fout Analysis Services](service-fabric-testability-overview.md) voor meer informatie.

## <a name="upgrade"></a>Upgraden
1. Een *service developer* updates van het onderdeel services van de geactiveerde toepassing en/of corrigeert bugs en biedt een nieuwe versie van het servicemanifest.
2. Een *toepassingsontwikkelaar* overschreven en de configuratie en implementatie-instellingen van de consistente services parameterizes en biedt een nieuwe versie van het toepassingsmanifest. Ontwikkelaar van de toepassing wordt vervolgens de nieuwe versies van de manifesten voor de service opgenomen in de toepassing en biedt een nieuwe versie van het toepassingstype in een bijgewerkte toepassingspakket.
3. Een *toepassingsbeheerder* opgenomen met de nieuwe versie van het toepassingstype in de doeltoepassing door het bijwerken van de juiste parameters.
4. Een *operator* uploadt het bijgewerkte toepassingspakket met het cluster image store via de [ **CopyApplicationPackage** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) of de [ **kopie ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Het toepassingspakket bevat manifest voor de toepassing en de verzameling van servicepakketten.
5. Een *operator* bepalingen van de nieuwe versie van de toepassing in het doelcluster met behulp van de [ **ProvisionApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **registreren ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), of de [ **inrichten van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
6. Een *operator* upgrades van de doeltoepassing met de nieuwe versie gebruiken de [ **UpgradeApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpgradeApplicationAsync_System_Fabric_Description_ApplicationUpgradeDescription_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **Start ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationupgrade), of de [ **upgraden van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/upgrade-an-application).
7. Een *operator* controleert de voortgang van het gebruik van de upgrade de [ **GetApplicationUpgradeProgressAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_GetApplicationUpgradeProgressAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **Get-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricapplicationupgrade), of de [ **voortgang voor toepassing bijwerken ophalen** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/get-the-progress-of-an-application-upgrade1).
8. Indien nodig, de *operator* wijzigt en past de parameters van de huidige toepassing upgraden met behulp van de [ **UpdateApplicationUpgradeAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpdateApplicationUpgradeAsync_System_Fabric_Description_ApplicationUpgradeUpdateDescription_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **Update ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/update-servicefabricapplicationupgrade), of de [ **Update toepassing upgraden** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/update-an-application-upgrade).
9. Indien nodig, de *operator* teruggedraaid, de huidige toepassing upgraden met behulp van de [ **RollbackApplicationUpgradeAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RollbackApplicationUpgradeAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **Start ServiceFabricApplicationRollback** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationrollback), of de [ **terugdraaien van de toepassing bijwerken** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/rollback-an-application-upgrade).
10. Service Fabric worden bijgewerkt voor de doeltoepassing zonder verlies van de beschikbaarheid van een van de samenstellende services in het cluster wordt uitgevoerd.

Zie de [upgrade zelfstudie](service-fabric-application-upgrade-tutorial.md) voor voorbeelden.

## <a name="maintain"></a>Onderhouden
1. Voor het besturingssysteem is bijgewerkt en patches, Service Fabric-interfaces met de Azure-infrastructuur om te waarborgen van de beschikbaarheid van alle toepassingen die worden uitgevoerd in het cluster.
2. Voor upgrades en patches voor het Service Fabric-platform, Service Fabric wordt bijgewerkt zelf zonder verlies van beschikbaarheid van een van de toepassingen die worden uitgevoerd op het cluster.
3. Een *toepassingsbeheerder* het toevoegen of verwijderen van knooppunten van een cluster na het analyseren van gebruiksgegevens historische capaciteit en de verwachte toekomstige aanvraag goedkeurt.
4. Een *operator* toegevoegd en verwijderd van knooppunten die zijn opgegeven door de *toepassingsbeheerder*.
5. Wanneer nieuwe knooppunten worden toegevoegd aan of de bestaande knooppunten zijn verwijderd uit het cluster, Service Fabric automatisch taakverdelingen van de actieve toepassingen op alle knooppunten in het cluster om de optimale prestaties.

## <a name="remove"></a>Verwijderen
1. Een *operator* kunt een specifiek exemplaar van een actieve service in het cluster verwijderen zonder te verwijderen van de gehele toepassing met behulp van de [ **DeleteServiceAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_DeleteServiceAsync_System_Fabric_Description_DeleteServiceDescription_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **verwijderen ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricservice), of de [ **verwijderen van een Service** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/delete-a-service).  
2. Een *operator* kunt ook verwijderen instantie van een toepassing en alle bijbehorende services met behulp van de [ **DeleteApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_DeleteApplicationAsync_System_Fabric_Description_DeleteApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **verwijderen ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricapplication), of de [ **toepassing verwijderen** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/delete-an-application).
3. Nadat de toepassingen en services zijn gestopt, de *operator* kunt inrichting de toepassing gebruikt de [ **UnprovisionApplicationAsync** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UnprovisionApplicationAsync_System_String_System_String_System_TimeSpan_System_Threading_CancellationToken_), wordt de [ **Unregister ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype), of de [ **inrichting van een toepassing** REST-bewerking](https://docs.microsoft.com/rest/api/servicefabric/unprovision-an-application). Hierbij van het toepassingstype, wordt het toepassingspakket niet verwijderd uit de Installatiekopieopslag. Het toepassingspakket moet u handmatig verwijderen.
4. Een *operator* Hiermee verwijdert u het toepassingspakket uit de Installatiekopieopslag met behulp van de [ **RemoveApplicationPackage** methode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RemoveApplicationPackage_System_String_System_String_) of de [ **verwijderen ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps).

Zie [implementeren van een toepassing](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over het ontwikkelen, Zie testen en het beheren van Service Fabric-toepassingen en services:

* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)
* [Toepassingsupgrade](service-fabric-application-upgrade.md)
* [Overzicht van testbaarheid](service-fabric-testability-overview.md)
