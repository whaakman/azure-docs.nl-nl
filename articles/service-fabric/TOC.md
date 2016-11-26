# Overzicht
## [Wat is Service Fabric?](service-fabric-overview.md)
## [Uitleg over microservices](service-fabric-overview-microservices.md)
## [Toepassingsscenario's](service-fabric-application-scenarios.md)
## [Architectuur](service-fabric-architecture.md)

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
### Basisbeginselen
#### [Programmeermodel](service-fabric-choose-framework.md)
#### [Toepassingsmodel](service-fabric-application-model.md)
#### [Servicecommunicatie](service-fabric-connect-and-communicate-with-services.md)
#### [Hulpprogramma's](service-fabric-manage-application-in-visual-studio.md)
#### [Foutopsporing](service-fabric-debugging-your-application.md)
#### Bewaken en diagnosticeren
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Beveiligingsbeleid configureren voor uw toepassing](service-fabric-application-runas-security.md)
#### [Uw toepassing configureren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md)

### Een Reliable Services-toepassing
#### [Overzicht](service-fabric-reliable-services-introduction.md)
#### Aan de slag
##### [C# op Windows](service-fabric-reliable-services-quick-start.md)
##### [Java op Linux](service-fabric-reliable-services-quick-start-java.md)
#### [Architectuur](service-fabric-reliable-services-platform-architecture.md)
#### [Betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)
#### [Betrouwbare verzamelingen gebruiken](service-fabric-work-with-reliable-collections.md)
#### [Configureren](service-fabric-reliable-services-configuration.md)
#### [Meldingen](service-fabric-reliable-services-notifications.md)
#### [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)
#### [Communiceren via Betrouwbare services](service-fabric-reliable-services-communication.md)
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

### Door gast uitvoerbare toepassing
#### [Een toepassing implementeren die door een gast kan worden uitgevoerd](service-fabric-deploy-existing-app.md)
#### [Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd](service-fabric-deploy-multiple-apps.md)

### Containertoepassing
#### [Overzicht](service-fabric-containers-overview.md)
#### [Windows-container implementeren](service-fabric-deploy-container.md)
#### [Docker-container implementeren](service-fabric-deploy-container-linux.md)

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
#### [Beveiliging](service-fabric-cluster-security.md)
#### [Herstel na noodgevallen](service-fabric-disaster-recovery.md)

### Clusters op Azure
#### Een cluster maken op Azure
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### [Knooppunttypen en VM-schaalsets](service-fabric-cluster-nodetypes.md)
#### [Een cluster schalen](service-fabric-cluster-scale-up-down.md)
#### [Een cluster upgraden](service-fabric-cluster-upgrade.md)
#### [Een cluster verwijderen](service-fabric-cluster-delete.md)
#### [Toegangsbeheer](service-fabric-cluster-security-roles.md)
#### [Een cluster configureren](service-fabric-cluster-fabric-settings.md)
#### [Een Party Cluster gratis proberen](http://aka.ms/tryservicefabric)

### Zelfstandige clusters
#### [Een zelfstandige cluster maken](service-fabric-cluster-creation-for-windows-server.md)
#### [Een cluster schalen](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Een cluster upgraden](service-fabric-cluster-upgrade-windows-server.md)
#### [Een cluster beveiligen](service-fabric-windows-cluster-x509-security.md)
#### [Toegangsbeheer](service-fabric-cluster-security-roles.md)
#### [Een cluster configureren](service-fabric-cluster-manifest.md)

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

## Toepassingslevenscyclus beheren
### [Overzicht](service-fabric-application-lifecycle.md)
### [Continue integratie instellen](service-fabric-set-up-continuous-integration.md)
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
### [Voorbeeld van levenscyclus van op REST-gebaseerde toepassing](service-fabric-rest-based-application-lifecycle-sample.md)

## Status van toepassing en cluster inspecteren
### [Service Fabric-status controleren](service-fabric-health-introduction.md)
### [Servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Aangepaste statusrapporten toevoegen](service-fabric-report-health.md)
### [Problemen met systeemstatusrapporten oplossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

## Bewaken en diagnosticeren
### Services lokaal controleren en een diagnose uitvoeren
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Diagnostische logboeken van Azure
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Service Fabric-toepassingstracering](service-fabric-diagnostic-how-to-use-elasticsearch.md)
### [Diagnose in Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Diagnose in stateful Reliable Services](service-fabric-reliable-services-diagnostics.md)
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
#### [Testscenario’s gebruiken](service-fabric-testability-scenarios.md)
### [Uw testtoepassing laden](service-fabric-vso-load-test.md)

# Naslaginformatie
## [Terminologie](service-fabric-technical-overview.md)
## [Reliable Actors](https://go.microsoft.com/fwlink/p/?linkid=833398)
## [Reliable Actors WCF](https://go.microsoft.com/fwlink/p/?linkid=833401)
## [Reliable Services](https://go.microsoft.com/fwlink/p/?linkid=833402)
## [Reliable Services WCF](https://go.microsoft.com/fwlink/p/?linkid=833403)
## [Gegevens](https://go.microsoft.com/fwlink/p/?linkid=833404)
## [Gegevensinterfaces](https://go.microsoft.com/fwlink/p/?linkid=833406)
## [Systeem](https://go.microsoft.com/fwlink/p/?linkid=833407)
## [Voorbeeldcode](http://aka.ms/servicefabricsamples)
## [PowerShell](/powershell/servicefabric/vlatest/servicefabric)
## [Java-API](https://go.microsoft.com/fwlink/p/?linkid=833410)
## [.NET](/dotnet/api)
## [REST](/rest/api/servicefabric/)

# Bronnen
## [Leertraject](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Prijzen](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Service-updates](https://azure.microsoft.com/updates/?product=service-fabric)
## [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)


<!--HONumber=Nov16_HO4-->


