# [Documentatie voor Service Fabric](/azure/service-fabric)
# Overzicht
## [Wat is Service Fabric?](service-fabric-overview.md)

# Snelstartgidsen
## [Een .NET-toepassing maken](service-fabric-quickstart-dotnet.md)
## [Een Linux-containertoepassing implementeren](service-fabric-quickstart-containers-linux.md)
## [Een Windows-containertoepassing implementeren](service-fabric-quickstart-containers.md)
## Java-snelstartgidsen
### [Een Spring Boot-toepassing implementeren](service-fabric-quickstart-java-spring-boot.md)
### [Een betrouwbare servicetoepassing implementeren](service-fabric-quickstart-java-reliable-services.md)

# Zelfstudies
## Een .NET-toepassing implementeren
### [1 - Een .NET-toepassing maken](service-fabric-tutorial-create-dotnet-app.md)
### [2 - De toepassing implementeren](service-fabric-tutorial-deploy-app-to-party-cluster.md)
### [3 - CI/CD configureren](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
### [4- Bewaken en diagnoses uitvoeren](service-fabric-tutorial-monitoring-aspnet.md)

## Een bestaande .NET-app in een container plaatsen
### [1- Een .NET-app implementeren met behulp van Docker Compose](service-fabric-host-app-in-a-container.md)
### [2- De container bewaken](service-fabric-tutorial-monitoring-wincontainers.md)

## Een Linux-container-app maken
### [1 - Containerinstallatiekopieën maken](service-fabric-tutorial-create-container-images.md)
### [2 - Pakket maken en containers implementeren](service-fabric-tutorial-package-containers.md)
### [3 - Failover uitvoeren en schalen](service-fabric-tutorial-containers-failover.md)

## Een cluster maken en beheren
### 1 - Een cluster maken in Azure
#### [1a: Een Windows-cluster maken](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
#### [1b: Een Linux-cluster maken](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
### [2: Het cluster schalen](service-fabric-tutorial-scale-cluster.md)
### [3: De cluster-runtime upgraden](service-fabric-tutorial-upgrade-cluster.md)
### [4: API Management met Service Fabric implementeren](service-fabric-tutorial-deploy-api-management.md)



# Voorbeelden
## [Codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=service-fabric)
## [Azure PowerShell](service-fabric-powershell-samples.md)
## [Service Fabric-CLI](samples-cli.md)

# Concepten
## [Uitleg over microservices](service-fabric-overview-microservices.md)
## [Grote afbeelding](service-fabric-content-roadmap.md)
## [Toepassingsscenario's](service-fabric-application-scenarios.md)
## [Patronen en scenario's](service-fabric-patterns-and-scenarios.md)
## [Architectuur](service-fabric-architecture.md)
## [Terminologie](service-fabric-technical-overview.md)

## Toepassingen en services bouwen
### Ondersteunde programmeermodellen
#### [Overzicht](service-fabric-choose-framework.md)
#### Containers
##### [Overzicht](service-fabric-containers-overview.md)
##### [Docker Compose (preview)](service-fabric-docker-compose.md)
##### [Resourcebeheer](service-fabric-resource-governance.md)
#### Reliable Services
##### [Overzicht](service-fabric-reliable-services-introduction.md)
##### [Levenscyclus van Reliable Services - C#](service-fabric-reliable-services-lifecycle.md)
##### [Levenscyclus van Reliable Services - Java](service-fabric-reliable-services-lifecycle-java.md)
##### [Betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)
##### [Richtlijnen en aanbevelingen voor betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections-guidelines.md)
##### [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
##### [Transacties en vergrendelingen](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
##### [Betrouwbare gelijktijdige wachtrij](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Betrouwbare verzamelingserialisatie](service-fabric-reliable-services-reliable-collections-serialization.md)
##### [Beheer van betrouwbare status en interne werking van betrouwbare verzameling](service-fabric-reliable-services-reliable-collections-internals.md)
##### [Geavanceerd gebruik](service-fabric-reliable-services-advanced-usage.md)

#### Reliable Actors
##### [Overzicht](service-fabric-reliable-actors-introduction.md)
##### [Architectuur](service-fabric-reliable-actors-platform.md)
##### [Levenscyclus en garbagecollection](service-fabric-reliable-actors-lifecycle.md)
##### [Statusbeheer](service-fabric-reliable-actors-state-management.md)
##### [Polymorfisme](service-fabric-reliable-actors-polymorphism.md)
##### [Herintreding](service-fabric-reliable-actors-reentrancy.md)
##### [Typeserialisatie](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

### [Toepassingsmodel](service-fabric-application-model.md)
### [Hostingmodel](service-fabric-hosting-model.md)

### Services
#### [Serviceresources](service-fabric-service-manifest-resources.md)
#### [Servicestatus](service-fabric-concepts-state.md)
#### [Service partitioneren](service-fabric-concepts-partitioning.md)
#### [Beschikbaarheid van services](service-fabric-availability-services.md)
#### [Replica's en exemplaren](service-fabric-concepts-replica-lifecycle.md)
#### [Herconfiguratie](service-fabric-concepts-reconfiguration.md)
#### Servicecommunicatie
##### [Overzicht](service-fabric-connect-and-communicate-with-services.md)
##### [DNS-service](service-fabric-dnsservice.md)
##### [Omgekeerde proxy](service-fabric-reverseproxy.md)
##### [Omgekeerde proxy voor veilige communicatie configureren](service-fabric-reverseproxy-configure-secure-communication.md)
##### [Diagnostische gegevens voor omgekeerde proxy](service-fabric-reverse-proxy-diagnostics.md)
### [Schaalbaarheid van toepassingen](service-fabric-concepts-scalability.md)
### [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)

### [Capaciteit van een toepassing plannen](service-fabric-capacity-planning.md)

## Toepassingen beheren
### [Overzicht](service-fabric-application-lifecycle.md)
### [De instelling ImageStoreConnectionString](service-fabric-image-store-connection-string.md)
### Toepassingsupgrade
#### [Overzicht](service-fabric-application-upgrade.md)
#### [Configuratie](service-fabric-visualstudio-configure-upgrade.md)
#### [Parameters toepassingsupgrade](service-fabric-application-upgrade-parameters.md)
#### [Gegevensserialisatie in toepassingsupgrades](service-fabric-application-upgrade-data-serialization.md)
#### [Toepassingsupgrade: geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md)
### [Overzicht foutanalyse](service-fabric-testability-overview.md)

## Clusters maken en beheren
### [Overzicht](service-fabric-deploy-anywhere.md)
### Plannen en voorbereiden
#### [Capaciteitsplanning](service-fabric-cluster-capacity.md)
#### [Herstel na noodgevallen](service-fabric-disaster-recovery.md)
### [Een cluster beschrijven](service-fabric-cluster-resource-manager-cluster-description.md)
### [Clusterbeveiliging](service-fabric-cluster-security.md)
### [Functieverschillen tussen Linux en Windows](service-fabric-linux-windows-differences.md)
### Clusters op Azure
#### [Knooppunttypen en VM-schaalsets](service-fabric-cluster-nodetypes.md)
#### [Clusternetwerkpatronen](service-fabric-patterns-networking.md)
### Cluster Resource Manager
#### [Overzicht](service-fabric-cluster-resource-manager-introduction.md)
#### [Architectuur](service-fabric-cluster-resource-manager-architecture.md)
#### [Een cluster beschrijven](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Overzicht toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md)
#### [Instellingen van Cluster Resource Manager configureren](service-fabric-cluster-resource-manager-configure-services.md)
#### [Bron metrische verbruiksgegevens](service-fabric-cluster-resource-manager-metrics.md)
#### [Serviceaffiniteit gebruiken](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
#### [Beleid voor serviceplaatsing](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
#### [Een cluster beheren](service-fabric-cluster-resource-manager-management-integration.md)
#### [Clusterdefragmentatie](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
#### [Een cluster in balans brengen](service-fabric-cluster-resource-manager-balancing.md)
#### [Beperking](service-fabric-cluster-resource-manager-advanced-throttling.md)
#### [Servicebeweging](service-fabric-cluster-resource-manager-movement-cost.md)

## [Integreren met API Management](service-fabric-api-management-overview.md)

## Bewaken en diagnoses uitvoeren
### [Toepassingen bewaken en er diagnoses op uitvoeren](service-fabric-diagnostics-overview.md)
### Gebeurtenissen genereren
#### [Gebeurtenissen genereren op platformniveau](service-fabric-diagnostics-event-generation-infra.md)
##### [Operationele kanaal](service-fabric-diagnostics-event-generation-operational.md)
##### [Reliable Services-gebeurtenissen](service-fabric-reliable-services-diagnostics.md)
##### [Reliable Actors-gebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
##### [Metrische gegevens voor prestaties](service-fabric-diagnostics-event-generation-perf.md)
##### [Externe toegang tot service bewaken](service-fabric-reliable-serviceremoting-diagnostics.md)
#### [Gebeurtenissen op toepassingsniveau genereren](service-fabric-diagnostics-event-generation-app.md)
### Status van toepassing en cluster inspecteren
#### [Service Fabric-status bewaken](service-fabric-health-introduction.md)
#### [Servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
#### [Aangepaste statusrapporten toevoegen](service-fabric-report-health.md)
#### [Problemen met systeemstatusrapporten oplossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
#### [Statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)
### Gebeurtenissen samenstellen
#### [Gebeurtenissen samenstellen met EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)
#### Gebeurtenissen samenstellen met Azure Diagnostics
##### [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
##### [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
### Gebeurtenissen analyseren
#### [Gebeurtenissen analyseren met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
#### [Gebeurtenissen analyseren met OMS](service-fabric-diagnostics-event-analysis-oms.md)
### [Problemen met uw lokale cluster oplossen](service-fabric-troubleshoot-local-cluster-setup.md)

# Handleidingen
## De ontwikkelomgeving instellen
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)

## [De Service Fabric-CLI instellen](service-fabric-cli.md)

## Een toepassing bouwen
### [Uw eerste C#-app in Visual Studio maken](service-fabric-create-your-first-application-in-visual-studio.md)
### Een service bouwen die door een gast kan worden uitgevoerd
#### [Een Node.js-toepassing in Windows hosten](quickstart-guest-app.md)
#### [Een toepassing implementeren die door een gast kan worden uitgevoerd](service-fabric-deploy-existing-app.md)
#### [Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd](service-fabric-deploy-multiple-apps.md)
### Een containerservice bouwen
#### [Een Windows-containertoepassing maken](service-fabric-get-started-containers.md)
#### [Een Linux-containertoepassing maken](service-fabric-get-started-containers-linux.md)
#### [Beveiliging van de container](service-fabric-securing-containers.md)
#### [Docker Compose (preview)](service-fabric-docker-compose.md)
#### [Resourcebeheer voor containers en services](service-fabric-resource-governance.md)
#### [Volume en stuurprogramma's voor logboekregistratie](service-fabric-containers-volume-logging-drivers.md)
#### [Services in containers](service-fabric-services-inside-containers.md)
#### [Netwerkmodi voor containers](service-fabric-networking-modes.md)

### Een Reliable Services-service bouwen
#### Betrouwbare verzamelingen
##### [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
##### [Betrouwbare gelijktijdige wachtrij](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Betrouwbare verzamelingserialisatie](service-fabric-reliable-services-reliable-collections-serialization.md)

#### Aan de slag
##### [C# op Windows](service-fabric-reliable-services-quick-start.md)
##### [Java op Linux](service-fabric-reliable-services-quick-start-java.md)
##### [Een C#-toepassing maken in Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
#### Communiceren met services
##### [Communiceren via Betrouwbare services](service-fabric-reliable-services-communication.md)

##### [Service voor externe toegang - C#](service-fabric-reliable-services-communication-remoting.md)
##### [Service voor externe toegang - Java](service-fabric-reliable-services-communication-remoting-java.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Beveiligde communicatie - C#](service-fabric-reliable-services-secure-communication.md)
##### [Beveiligde communicatie - Java](service-fabric-reliable-services-secure-communication-java.md)

#### [Configureren](service-fabric-reliable-services-configuration.md)
#### [Meldingen verzenden](service-fabric-reliable-services-notifications.md)
#### [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)

### Een Reliable Actors-service bouwen
#### Aan de slag
##### [C# op Windows](service-fabric-reliable-actors-get-started.md)
##### [Java Actor op Linux](service-fabric-create-your-first-linux-application-with-java.md)
#### [Meldingen verzenden](service-fabric-reliable-actors-events.md)
#### [Timers en herinneringen instellen](service-fabric-reliable-actors-timers-reminders.md)
#### [KvsActorStateProvider configureren](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Communicatie-instellingen configureren](service-fabric-reliable-actors-fabrictransportsettings.md)
#### [ReliableDictionaryActorStateProvider configureren](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

### [Oude Java-toepassing migreren voor ondersteuning van Maven](service-fabric-migrate-old-javaapp-to-use-maven.md)

### [Omgekeerde proxy voor veilige communicatie configureren](service-fabric-reverseproxy-configure-secure-communication.md)

### [Een ASP.NET Core-service bouwen](service-fabric-add-a-web-frontend.md)

### Beveiliging configureren
#### [Toepassingsgeheimen beheren](service-fabric-application-secret-management.md)  
#### [Beveiligingsbeleid configureren voor uw toepassing](service-fabric-application-runas-security.md)

## Werken in een Windows-ontwikkelomgeving
### [Toepassingen beheren in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
### [Beveiligde verbindingen configureren in Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
### [Uw toepassing configureren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md)
### [Fouten opsporen in een .NET-service in Visual Studio](service-fabric-debugging-your-application.md)
### [Veelvoorkomende fouten en uitzonderingen](service-fabric-errors-and-exceptions.md)
### [Lokaal bewaken en diagnoses uitvoeren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
### [Een Linux-cluster instellen in Windows](service-fabric-local-linux-cluster-windows.md)

## Werken in een Linux-ontwikkelomgeving
### [Aan de slag met Eclipse-invoegtoepassing voor Java-ontwikkeling](service-fabric-get-started-eclipse.md)
### [Fouten opsporen in een Java-service in Eclipse](service-fabric-debugging-your-application-java.md)
### [Lokaal bewaken en diagnoses uitvoeren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## Migreren uit Cloud Services
### [Cloud Services vergelijken met Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migreren naar Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
### [Aanbevolen procedures](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Toepassingslevenscyclus beheren
### [Toepassingspakket maken](service-fabric-package-apps.md)

### Toepassingen implementeren of verwijderen
#### [Toepassingen implementeren in een lokaal cluster](service-fabric-get-started-with-a-local-cluster.md)
#### [Azure Resource Manager](service-fabric-application-arm-resource.md)
#### [Azure PowerShell](service-fabric-deploy-remove-applications.md)
#### [Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md)
#### [FabricClient-API's](service-fabric-deploy-remove-applications-fabricclient.md)

### Toepassingen upgraden
#### [Upgraden met Azure PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Upgraden met Visual Studio](service-fabric-application-upgrade-tutorial.md)
#### [Problemen met toepassingsupgrades oplossen](service-fabric-application-upgrade-troubleshooting.md)

### Testtoepassingen en -services
#### [Service-naar-servicecommunicatie testen](service-fabric-testability-scenarios-service-communication.md)
#### Fouten simuleren
##### [Gecontroleerde chaos gebruiken](service-fabric-controlled-chaos.md)
##### [Testacties gebruiken](service-fabric-testability-actions.md)
##### [Tijdens workloads](service-fabric-testability-workload-tests.md)
##### [Testscenario's gebruiken](service-fabric-testability-scenarios.md)
##### [De API's voor knooppuntovergang gebruiken](service-fabric-node-transition-apis.md)

### Continue integratie instellen
#### [Continue integratie met VSTS instellen](service-fabric-set-up-continuous-integration.md)
#### [Uw Linux-toepassingen implementeren met behulp van Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)

## Clusters maken en beheren
### Clusters op Azure
#### Maken
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### Schalen
##### [Handmatig](service-fabric-cluster-scale-up-down.md)
##### [Programmatisch](service-fabric-cluster-programmatic-scaling.md)
#### [Upgraden](service-fabric-cluster-upgrade.md)
#### [Toegangsbeheer instellen](service-fabric-cluster-security-roles.md)
#### [Configureren](service-fabric-cluster-fabric-settings.md)
#### [Een poort in de load balancer openen](create-load-balancer-rule.md)
#### [Clustercertificaten beheren](service-fabric-cluster-security-update-certs-azure.md)
#### [Verwijderen](service-fabric-cluster-delete.md)

### Zelfstandige clusters
#### [Implementatie plannen en voorbereiden](service-fabric-cluster-standalone-deployment-preparation.md)
#### Maken
##### [Uw eerste zelfstandige -cluster maken](service-fabric-get-started-standalone-cluster.md)
##### [On-premises maken](service-fabric-cluster-creation-for-windows-server.md)
##### [Beveiligen met behulp van certificaten](service-fabric-windows-cluster-x509-security.md)  
##### [Beveiligen met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)
##### [Inhoud van het zelfstandige pakket](service-fabric-cluster-standalone-package-contents.md)
#### [Schalen](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Toegangsbeheer instellen](service-fabric-cluster-security-roles.md)
#### [Configureren](service-fabric-cluster-manifest.md)
#### [Upgraden](service-fabric-cluster-upgrade-windows-server.md)

### [Een cluster visualiseren](service-fabric-visualizing-your-cluster.md)
### [Verbinding maken met een beveiligde cluster](service-fabric-connect-to-secure-cluster.md)
### [Patch uitvoeren voor clusterknooppunten](service-fabric-patch-orchestration-application.md)

## Bewaken en diagnoses uitvoeren
### OMS
#### [OMS Log Analytics instellen](service-fabric-diagnostics-oms-setup.md)
#### [De OMS-agent toevoegen](service-fabric-diagnostics-oms-agent.md)
#### [Containers bewaken](service-fabric-diagnostics-oms-containers.md)
### Prestatiebewaking
#### [Prestatiebewaking met WAD](service-fabric-diagnostics-perf-wad.md)

# Naslaginformatie
## [Azure PowerShell](/powershell/module/azurerm.servicefabric/)
## [PowerShell](/powershell/module/servicefabric/?view=azureservicefabricps)
## [Azure CLI (az sf)](/cli/azure/sf)
## [Service Fabric CLI (sfctl)](service-fabric-sfctl.md)
### [sfctl application](service-fabric-sfctl-application.md)
### [sfctl chaos](service-fabric-sfctl-chaos.md)
### [sfctl cluster](service-fabric-sfctl-cluster.md)
### [sfctl compose](service-fabric-sfctl-compose.md)
### [sfctl is](service-fabric-sfctl-is.md)
### [sfctl node](service-fabric-sfctl-node.md)
### [sfctl partition](service-fabric-sfctl-partition.md)
### [sfctl replica](service-fabric-sfctl-replica.md)
### [sfctl rpm](service-fabric-sfctl-rpm.md)
### [sfctl service](service-fabric-sfctl-service.md)
### [sfctl store](service-fabric-sfctl-store.md)
## [Java-API](/java/api/overview/azure/servicefabric)
## [.NET](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet)
## [REST](/rest/api/servicefabric)
## [XML-schema van servicemodel](service-fabric-service-model-schema.md)

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/)
## [Veelgestelde vragen](service-fabric-common-questions.md)
## [Leertraject](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Prijzen](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Voorbeeldcode](http://aka.ms/servicefabricsamples)
## [Ondersteuningsopties](service-fabric-support.md)
## [Service-updates](https://azure.microsoft.com/updates/?product=service-fabric)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)