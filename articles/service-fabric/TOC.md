# Overzicht
## [Wat is Service Fabric?](service-fabric-overview.md)
## [Uitleg over microservices](service-fabric-overview-microservices.md)
## [Toepassingsscenario's](service-fabric-application-scenarios.md)
## [Architectuur](service-fabric-architecture.md)
## [Terminologie](service-fabric-technical-overview.md)
## [Inhoudsschema](service-fabric-content-roadmap.md)

# Aan de slag
## De ontwikkelomgeving instellen
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## Uw eerste toepassing maken
### [C# op Windows](service-fabric-create-your-first-application-in-visual-studio.md)
### [Java op Linux](service-fabric-create-your-first-linux-application-with-java.md)
### [C# op Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Implementeren van apps op een lokale cluster](service-fabric-get-started-with-a-local-cluster.md)

# Procedures
## Een toepassing bouwen
### [Patronen en scenario's](service-fabric-patterns-and-scenarios.md)
### Basisbeginselen
#### [Toepassingsmodel](service-fabric-application-model.md)
#### [Ondersteund programmeermodel](service-fabric-choose-framework.md)
#### [Servicestatus](service-fabric-concepts-state.md)
#### [Servicecommunicatie](service-fabric-connect-and-communicate-with-services.md)
#### [Een webfront-end toevoegen](service-fabric-add-a-web-frontend.md)
#### [Servicemanifest-resources](service-fabric-service-manifest-resources.md)
#### [Apps beheren in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
#### [Beveiligde verbindingen configureren in Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
#### Fouten opsporen
##### [Fouten opsporen in een C#-service in Visual Studio](service-fabric-debugging-your-application.md)
##### [Fouten opsporen in een Java-service in Eclipse](service-fabric-debugging-your-application-java.md)
#### Bewaken en diagnosticeren
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Toepassingsgeheimen beheren](service-fabric-application-secret-management.md)  
#### [Beveiligingsbeleid configureren voor uw toepassing](service-fabric-application-runas-security.md)  
#### [Uw toepassing configureren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md)  
#### [Veelvoorkomende fouten en uitzonderingen](service-fabric-errors-and-exceptions.md) 

### Door gast uitvoerbare toepassing
#### [Een toepassing implementeren die door een gast kan worden uitgevoerd](service-fabric-deploy-existing-app.md)
#### [Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd](service-fabric-deploy-multiple-apps.md)

### Containertoepassing
#### [Overzicht](service-fabric-containers-overview.md)
#### [Windows-container implementeren](service-fabric-deploy-container.md)
#### [Docker-container implementeren](service-fabric-deploy-container-linux.md)

### Een Reliable Services-toepassing
#### [Overzicht](service-fabric-reliable-services-introduction.md)
#### Aan de slag
##### [C# op Windows](service-fabric-reliable-services-quick-start.md)
##### [Java op Linux](service-fabric-reliable-services-quick-start-java.md)
#### [Architectuur](service-fabric-reliable-services-platform-architecture.md)
#### [Levenscyclus van Reliable Services](service-fabric-reliable-services-lifecycle.md)
#### [Betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)
#### [Betrouwbare verzamelingen gebruiken](service-fabric-work-with-reliable-collections.md)
#### [Configureren](service-fabric-reliable-services-configuration.md)
#### [Meldingen](service-fabric-reliable-services-notifications.md)
#### [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)
#### [Communiceren via Betrouwbare services](service-fabric-reliable-services-communication.md)
#### [Veilige communicatie via Reliable Services](service-fabric-reliable-services-secure-communication.md)
##### [ASP.NET](service-fabric-reliable-services-communication-webapi.md)
##### [Service voor externe toegang](service-fabric-reliable-services-communication-remoting.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Omgekeerde proxy](service-fabric-reverseproxy.md)
#### [Geavanceerd gebruik](service-fabric-reliable-services-advanced-usage.md)

### Toepassing Reliable Actor
#### [Overzicht](service-fabric-reliable-actors-introduction.md)
#### Aan de slag
##### [C# op Windows](service-fabric-reliable-actors-get-started.md)
##### [Java op Linux](service-fabric-reliable-actors-get-started-java.md)
#### [Architectuur](service-fabric-reliable-actors-platform.md)
#### [Levenscyclus en garbagecollection](service-fabric-reliable-actors-lifecycle.md)
#### [Polymorfisme](service-fabric-reliable-actors-polymorphism.md)
#### [Herintreding](service-fabric-reliable-actors-reentrancy.md)
#### [Timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
#### [Gebeurtenissen](service-fabric-reliable-actors-events.md)
#### [Statusbeheer](service-fabric-reliable-actors-state-management.md)
#### [State-provider configureren](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Typeserialisatie](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)
#### [Communicatie-instellingen configureren](service-fabric-reliable-actors-fabrictransportsettings.md) 

## Migreren uit Cloud Services
### [Cloud Services vergelijken met Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migreren naar Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

## Clusters maken en beheren

### Basisbeginselen
#### [Overzicht](service-fabric-deploy-anywhere.md)
#### [Een cluster beschrijven](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Capaciteitsplanning](service-fabric-cluster-capacity.md)
#### [Een cluster visualiseren](service-fabric-visualizing-your-cluster.md)
#### [Verbinding maken met een beveiligde cluster](service-fabric-connect-to-secure-cluster.md)
#### [Een cluster beheren met Azure CLI](service-fabric-azure-cli.md) 
#### [Een cluster beveiligen](service-fabric-cluster-security.md)
#### [Herstel na noodgevallen](service-fabric-disaster-recovery.md)

### Clusters op Azure
#### Een cluster maken op Azure
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
##### [Visual Studio en Azure Resource Manager](service-fabric-cluster-creation-via-visual-studio.md)
#### [Knooppunttypen en VM-schaalsets](service-fabric-cluster-nodetypes.md)
#### [Een cluster schalen](service-fabric-cluster-scale-up-down.md)
#### [Een cluster upgraden](service-fabric-cluster-upgrade.md)
#### [Een cluster verwijderen](service-fabric-cluster-delete.md)
#### [Toegangsbeheer](service-fabric-cluster-security-roles.md)
#### [Een cluster configureren](service-fabric-cluster-fabric-settings.md)
#### [Een cluster beveiligen met certificaten](service-fabric-windows-cluster-x509-security.md)
#### [Clustercertificaten toevoegen of overdragen](service-fabric-cluster-security-update-certs-azure.md) 
#### [Een Party Cluster gratis proberen](http://aka.ms/tryservicefabric)

### Zelfstandige clusters
#### [Implementatie plannen en voorbereiden](service-fabric-cluster-standalone-deployment-preparation.md)
#### [Inhoud van het zelfstandige pakket voor Service Fabric](service-fabric-cluster-standalone-package-contents.md)
#### [Een zelfstandige cluster maken](service-fabric-cluster-creation-for-windows-server.md)
#### [Een zelfstandige cluster maken in Azure Virtual Machines](service-fabric-cluster-creation-with-windows-azure-vms.md)
#### [Een cluster schalen](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Een cluster upgraden](service-fabric-cluster-upgrade-windows-server.md)
#### [Toegangsbeheer](service-fabric-cluster-security-roles.md)
#### [Een cluster configureren](service-fabric-cluster-manifest.md)
#### [Een cluster beveiligen met certificaten](service-fabric-windows-cluster-x509-security.md)  
#### [Een cluster beveiligen met Windows-beveiliging](service-fabric-windows-cluster-windows-security.md) 

## Toepassingslevenscyclus beheren
### [Overzicht](service-fabric-application-lifecycle.md)
### [Continue integratie instellen](service-fabric-set-up-continuous-integration.md)
### [Meer informatie over de instelling ImageStoreConnectionString](service-fabric-image-store-connection-string.md)
### Toepassingen implementeren of verwijderen
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
### [Overzicht toepassingsupgrade](service-fabric-application-upgrade.md)
### [Toepassingsupgrade configureren](service-fabric-visualstudio-configure-upgrade.md)
### [Parameters toepassingsupgrade](service-fabric-application-upgrade-parameters.md)
### Een upgrade van een app uitvoeren
#### [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Visual Studio](service-fabric-application-upgrade-tutorial.md)
### [Problemen met toepassingsupgrades oplossen](service-fabric-application-upgrade-troubleshooting.md)
### [Gegevensserialisatie in toepassingsupgrades](service-fabric-application-upgrade-data-serialization.md)
### [Toepassingsupgrade: geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md)

## Status van toepassing en cluster inspecteren
### [Service Fabric-status bewaken](service-fabric-health-introduction.md)
### [Servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Aangepaste statusrapporten toevoegen](service-fabric-report-health.md)
### [Problemen met systeemstatusrapporten oplossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

## Bewaken en diagnoses uitvoeren
### [Toepassingen bewaken en er diagnoses op uitvoeren](service-fabric-diagnostics-overview.md)
### Services lokaal bewaken en er diagnoses op uitvoeren
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Diagnostische logboeken van Azure
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Logboeken verzamelen van een serviceproces](service-fabric-diagnostic-collect-logs-without-an-agent.md)
### [Diagnose in stateful Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Diagnose in Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Problemen met uw lokale cluster oplossen](service-fabric-troubleshoot-local-cluster-setup.md)
### [Algemene problemen oplossen](service-fabric-diagnostics-troubleshoot-common-scenarios.md)

## Schaaltoepassingen
### [Reliable Services partitioneren](service-fabric-concepts-partitioning.md)
### [Beschikbaarheid van services](service-fabric-availability-services.md)
### [Schaaltoepassingen](service-fabric-concepts-scalability.md)
### [Capaciteit van toepassingen plannen](service-fabric-capacity-planning.md)

## Testtoepassingen en -services
### [Overzicht foutanalyse](service-fabric-testability-overview.md)
### [Service-naar-servicecommunicatie testen](service-fabric-testability-scenarios-service-communication.md)
### Fouten simuleren
#### [Gecontroleerde chaos gebruiken](service-fabric-controlled-chaos.md)
#### [Testacties gebruiken](service-fabric-testability-actions.md)
#### [Tijdens workloads](service-fabric-testability-workload-tests.md)
#### [Door gegevensverlies aan te roepen](service-fabric-use-data-loss-api.md)
#### [Testscenario's gebruiken](service-fabric-testability-scenarios.md)
#### [De API's voor knooppuntovergang gebruiken](service-fabric-node-transition-apis.md)
### [Uw testtoepassing laden](service-fabric-vso-load-test.md)

## Clusterbronnen beheren en organiseren
### [Overzicht van Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
### [Cluster Resource Manager-architectuur](service-fabric-cluster-resource-manager-architecture.md)
### [Een cluster beschrijven](service-fabric-cluster-resource-manager-cluster-description.md)
### [Overzicht toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md)
### [Instellingen van Cluster Resource Manager configureren](service-fabric-cluster-resource-manager-configure-services.md)
### [Bron metrische verbruiksgegevens](service-fabric-cluster-resource-manager-metrics.md)
### [Serviceaffiniteit gebruiken](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
### [Beleid voor serviceplaatsing](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
### [Een cluster beheren](service-fabric-cluster-resource-manager-management-integration.md)
### [Clusterdefragmentatie](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
### [Een cluster in balans brengen](service-fabric-cluster-resource-manager-balancing.md)
### [Beperking](service-fabric-cluster-resource-manager-advanced-throttling.md)
### [Servicebeweging](service-fabric-cluster-resource-manager-movement-cost.md)

# Naslaginformatie
## [PowerShell](//powershell/servicefabric/vlatest/servicefabric)
## [Java-API](/java/api/microsoft.servicefabric.services)
## [.NET](/dotnet/api/microsoft.servicefabric.services)
## [REST](/rest/api/servicefabric)

# Resources
## [Veelgestelde vragen over Service Fabric](service-fabric-common-questions.md)
## [Ondersteuningsopties voor Service Fabric](service-fabric-support.md)
## [Voorbeeldcode](http://aka.ms/servicefabricsamples)
## [Leertraject](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Prijzen](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Service-updates](https://azure.microsoft.com/updates/?product=service-fabric)
## [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)
