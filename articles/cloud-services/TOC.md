# [Documentatie voor Cloud Services](index.md)

# Overzicht
## [Wat is Cloud Services?](cloud-services-choose-me.md)
## [Configuratiebestanden en verpakking van een cloudservice](cloud-services-model-and-package.md)

# Aan de slag
## [Voorbeeld van .NET-cloudservice](cloud-services-dotnet-get-started.md)
## [Voorbeeld van Python voor Visual Studio-cloudservice](cloud-services-python-ptvs.md)
## [Een hybride HPC-cluster configureren met Microsoft HPC Pack](cloud-services-setup-hybrid-hpcpack-cluster.md)

# Procedures
## Plannen
### [Grootte van virtuele machines](cloud-services-sizes-specs.md)
### [Updates](cloud-services-update-azure-service.md)

## Ontwikkelen
### [PHP-web- en -werkrollen maken](../cloud-services-php-create-web-role.md)
### [Een Node.js-toepassing bouwen en implementeren](cloud-services-nodejs-develop-deploy-app.md)
### [Een Node.js-webtoepassing bouwen met Express](cloud-services-nodejs-develop-deploy-express-app.md)
### Storage en Visual Studio
#### [Blob Storage en verbonden services](../visual-studio/vs-storage-cloud-services-getting-started-blobs.md)
#### [Queue Storage en verbonden services](../visual-studio/vs-storage-cloud-services-getting-started-queues.md)
#### [Table Storage en verbonden services](../visual-studio/vs-storage-cloud-services-getting-started-tables.md)
### [Verkeersregels voor een rol configureren](cloud-services-enable-communication-role-instances.md)
### [Levenscyclusgebeurtenissen van cloudservices verwerken](cloud-services-role-lifecycle-dotnet.md)
### [Socket.io (Node.js)](cloud-services-nodejs-chat-app-socketio.md)
### [Bellen met behulp van Twilio (.NET)](../partner-twilio-cloud-services-dotnet-phone-call-web-role.md)

### Opstarttaken configureren
#### [Opstarttaken maken](cloud-services-startup-tasks.md)
#### [Veelvoorkomende opstarttaken](cloud-services-startup-tasks-common.md)
#### [Een taak gebruiken om .NET op een cloudservicerol te installeren](cloud-services-dotnet-install-dotnet.md)

### Extern bureaublad configureren
#### [Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
#### [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
#### [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

## Implementeren
### [Een cloudservice maken en implementeren in de portal](cloud-services-how-to-create-deploy-portal.md)
### [Een lege cloudservicecontainer maken in PowerShell](cloud-services-powershell-create-cloud-container.md)
### [Een aangepaste domeinnaam configureren](cloud-services-custom-domain-name-portal.md)
### [Verbinding maken met een aangepaste domeincontroller](cloud-services-connect-to-custom-domain.md)

## Services beheren
### [Veelvoorkomende beheertaken](cloud-services-how-to-manage-portal.md)
### [Een cloudservice configureren](cloud-services-how-to-configure-portal.md)
### [Een cloudservice beheren met Azure Automation](automation-manage-cloud-services.md)
### [Automatisch schalen configureren](cloud-services-how-to-scale-portal.md)
### [Python gebruiken om Azure-resources te beheren](cloud-services-python-how-to-use-service-management.md)
### [Speculatieve uitvoering beperken](mitigate-se.md)

### [Patches voor gastbesturingssysteem](cloud-services-guestos-msrc-releases.md)
### Buitengebruikstelling van gastbesturingssysteem
#### [Stopzetting van beleid](cloud-services-guestos-retirement-policy.md)
#### [Kennisgeving van buitengebruikstelling van serie 1](cloud-services-guestos-family1-retirement.md)
### [Releasenieuws voor gastbesturingssysteem](cloud-services-guestos-update-matrix.md)
### [Overzichtskaart van configuratie van Cloud Services-rollen voor XPath](cloud-services-role-config-xpath.md)

## Certificaten beheren
### [Cloud Services en beheercertificaten](cloud-services-certs-create.md)
### [SSL configureren](cloud-services-configure-ssl-certificate-portal.md)

## Controleren
### [Cloud Services controleren](cloud-services-how-to-monitor.md)
### [Prestatiemeteritems gebruiken](diagnostics-performance-counters.md)
### [Prestaties testen](../vs-azure-tools-performance-profiling-cloud-services.md)
#### [Testen met Visual Studio Profiler](cloud-services-performance-testing-visual-studio-profiler.md)
### Diagnostische gegevens inschakelen
#### [Azure PowerShell](cloud-services-diagnostics-powershell.md)
#### [.NET](cloud-services-dotnet-diagnostics.md)
#### [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
### [Diagnostische gegevens opslaan en weergeven in Azure Storage](../monitoring-and-diagnostics/azure-diagnostics-storage.md?toc=%2fazure%2fcloud-services%2ftoc.json )
### [Een cloudservice volgen met de diagnosefunctie](cloud-services-dotnet-diagnostics-trace-flow.md)

## Problemen oplossen
### Fouten opsporen 
#### [Opties voor een cloudservice](../vs-azure-tools-debugging-cloud-services-overview.md)
#### [Lokale cloudservice met Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md)
#### [Gepubliceerde cloudservice met Visual Studio](../vs-azure-tools-intellitrace-debug-published-cloud-services.md)
### [Toewijzing van cloudservice is mislukt](cloud-services-allocation-failures.md)
### [Veelvoorkomende oorzaken voor het recyclen van cloudservicerollen](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)
### [Standaard TEMP-map is te klein voor de rol](cloud-services-troubleshoot-default-temp-folder-size-too-small-web-worker-role.md)
### [Veelvoorkomende implementatieproblemen](cloud-services-troubleshoot-deployment-problems.md)
### [Rol is niet gestart](cloud-services-troubleshoot-roles-that-fail-start.md)
### [Richtlijnen voor herstel](cloud-services-disaster-recovery-guidance.md)
### Veelgestelde vragen over Cloud Services
#### [Veelgestelde vragen over beschikbaarheid van toepassingen en services](cloud-services-application-and-service-availability-faq.md)
#### [Veelgestelde vragen over configuratie en beheer](cloud-services-configuration-and-management-faq.md)
#### [Veelgestelde vragen over connectiviteit en netwerken](cloud-services-connectivity-and-networking-faq.md)
#### [Veelgestelde vragen over implementatie](cloud-services-deployment-faq.md)

# Naslaginformatie
## [Codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=cloud-services)
## [.csdef XML Schema](schema-csdef-file.md)
### [LoadBalancerProbe Schema](schema-csdef-loadbalancerprobe.md)
### [WebRole Schema](schema-csdef-webrole.md)
### [WorkerRole Schema](schema-csdef-workerrole.md)
### [NetworkTrafficRules Schema](schema-csdef-networktrafficrules.md)
## [.cscfg XML Schema](schema-cscfg-file.md)
### [Role Schema](schema-cscfg-role.md)
### [NetworkConfiguration Schema](schema-cscfg-networkconfiguration.md)
## [REST](/rest/api/compute/cloudservices/)

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=compute)
## [Leertraject](https://azure.microsoft.com/documentation/learning-paths/cloud-services/)
## [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-us/home?forum=windowsazuredevelopment)
## [Prijzen](https://azure.microsoft.com/pricing/details/cloud-services/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Service-updates](https://azure.microsoft.com/updates/?product=cloud-services&updatetype=&platform=)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=cloud-services)
