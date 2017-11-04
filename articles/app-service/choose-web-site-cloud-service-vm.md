---
title: Vergelijking van Azure App Service, virtuele Machines, Service Fabric en Cloud Services | Microsoft Docs
description: Informatie over hoe u kiest tussen Azure App Service, virtuele Machines, Service Fabric en Cloud-Services voor het hosten van webtoepassingen.
services: app-service\web, virtual-machines, cloud-services
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: edd5099d2804fdb5867b4be5b11a361004db1665
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Vergelijking van Azure App Service, virtuele Machines, Service Fabric en Cloud-Services
## <a name="overview"></a>Overzicht
Azure biedt verschillende manieren naar de websites host: [Azure App Service][Azure App Service], [virtuele Machines][Virtual Machines], [Service Fabric][Service Fabric], en [Cloudservices][Cloud Services]. In dit artikel helpt u inzicht in de opties en maak de juiste keuze voor uw webtoepassing.

Azure App Service is de beste keuze voor de meeste web-apps. Implementatie en beheer zijn geïntegreerd in het platform, sites snel kunnen worden geschaald voor intensief verkeer belasting en de ingebouwde load balancing en verkeer manager bieden hoge beschikbaarheid. U kunt bestaande sites verplaatsen naar Azure App Service eenvoudig met een [online Migratiehulpmiddel](https://www.migratetoazure.net/), een open source-app uit de galerie toepassing gebruiken of maak een nieuwe site met behulp van het framework en hulpprogramma's van uw keuze. De [WebJobs] [ WebJobs] functie kunt u gemakkelijk achtergrondverwerking taak toevoegen aan uw App Service-web-app.

Service Fabric is een goede keuze als u een nieuwe app maken of een bestaande app voor het gebruik van een microservice-architectuur opnieuw in te schrijven. Apps die worden uitgevoerd op een gedeelde groep machines, kunnen begin klein en groeien op grote schaal met honderden of duizenden machines zo nodig. Stateful services kunnen u eenvoudig consistent en betrouwbaar app status opslaan en Service Fabric beheert automatisch service partitioneren, schaalbaarheid en beschikbaarheid voor u.  Service Fabric ondersteunt ook WebAPI met Open Web Interface voor .NET (OWIN) en ASP.NET Core.  Vergeleken met de App Service, bevat Service Fabric ook meer controle over of directe toegang tot de onderliggende infrastructuur. U kunt externe in uw servers of server starten van de taken configureren. Cloud-Services is vergelijkbaar met Service Fabric in de mate van controle en eenvoudig te gebruiken, maar het is nu een verouderde service en Service Fabric wordt aanbevolen voor de ontwikkeling van nieuwe.

Als u een bestaande toepassing die moet worden uitgevoerd in App Service- of Service Fabric aanzienlijke wijzigingen hebt, kunt u virtuele Machines om te migreren naar de cloud vereenvoudigen. Correct vereist configureren, beveiligen en beheren van virtuele machines echter veel meer tijd en IT-expertise in vergelijking met Azure App Service en Service Fabric. Als u Azure Virtual Machines overweegt, moet dat u rekening houden met de voortdurende onderhoud moeite patch, bijwerken en beheren van uw VM-omgeving. Virtuele Machines in Azure is Infrastructure-as-a-Service (IaaS), terwijl de App Service en Service Fabric-Platform-as-a-Service (Paas) zijn. 

## <a name="features"></a>Vergelijking van functies
De volgende tabel vergelijkt de mogelijkheden van App Service, Cloud Services, virtuele Machines en Service Fabric voor hulp bij het maken van de beste keuze. Zie voor actuele informatie over de SLA voor elke optie [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

| Functie | App Service (web-apps) | Cloud-Services (webrollen) | Virtuele machines | Service Fabric | Opmerkingen |
| --- | --- | --- | --- | --- | --- |
| Handomdraai implementatie |X | | |X |Een toepassing of het bijwerken van een toepassing implementeren naar een Cloudservice of het maken van een virtuele machine, duurt het enkele minuten ten minste; een toepassing implementeren in een web-app duurt seconden. |
| Opschalen naar grotere machines zonder opnieuw distribueren |X | | |X | |
| Web server-exemplaren delen inhoud en configuratie, wat betekent dat u hoeft dat te implementeren of opnieuw moet configureren terwijl u schaalt. |X | | |X | |
| Meerdere implementatieomgevingen (productie en in fasering) |X |X | |X |Service Fabric kunt u meerdere omgevingen voor uw apps hebben of voor het implementeren van verschillende versies van uw app naast elkaar. |
| Automatische OS updatebeheer |X |X | | |Gedeeltelijk via Patch Orchestration toepassing (POA) en volledig in de toekomst. |
| Naadloze platform overschakelen (gemakkelijk verplaatsen tussen de 32-bits en 64-bits) |X |X | | | |
| Code implementeren in GIT, FTP |X | |X | | |
| Code implementeren met Web Deploy |X | |X | |Cloudservices ondersteunt het gebruik van Web Deploy updates kunt toepassen op afzonderlijke rolinstanties. Echter, u deze niet gebruiken voor de initiële implementatie van een rol en als u een Web Deploy voor een update gebruiken hebt u afzonderlijk implementeren op elk exemplaar van een rol. Meerdere exemplaren zijn vereist voor het in aanmerking voor de Cloud Service-SLA voor productieomgevingen. |
| Ondersteuning voor WebMatrix |X | |X | | |
| Toegang tot services zoals Service Bus-, opslag-, SQL-Database |X |X |X |X | |
| Host-web- of web services-laag van een architectuur met meerdere lagen |X |X |X |X | |
| Middelste laag van de host van een architectuur met meerdere lagen |X |X |X |X |App Service-web-apps kunnen eenvoudig een REST-API als middelste laag, hosten en de [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) functie achtergrondtaken verwerking kan hosten. U kunt WebJobs uitvoeren op een speciale website te bereiken onafhankelijke schaalbaarheid voor de laag. |
| Geïntegreerde MySQL as a service-ondersteuning |X |X |X | |MySQL-as-a-service kunnen worden geïntegreerd in cloud-Services via de offerings van ClearDB, maar niet als onderdeel van de Azure Portal-werkstroom. |
| Ondersteuning voor ASP.NET, klassiek ASP, Node.js, PHP, Python |X |X |X |X |Service Fabric ondersteunt het maken van een web-front-met [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) of kunt u elk type toepassing (Node.js, Java, enzovoort) implementeren als een [Gast uitvoerbaar bestand](../service-fabric/service-fabric-deploy-existing-app.md). |
| Uitschalen naar meerdere exemplaren zonder opnieuw distribueren |X |X |X |X |Virtuele Machines kunt uitbreiden naar meerdere exemplaren, maar de services die daarop worden uitgevoerd voor het afhandelen van deze scale-out moeten worden geschreven. U moet een load balancer te routeren aanvragen via de machines en maken van een Affiniteitsgroep om te voorkomen dat gelijktijdige opnieuw opstarten van alle exemplaren vanwege onderhoud of hardwarestoringen configureren. |
| Ondersteuning voor SSL |X |X |X |X |Voor App Service WebApps, wordt SSL voor aangepaste domeinnamen alleen ondersteund voor Basic en Standard-modus. Zie voor meer informatie over het gebruik van SSL met web-apps [configureren van een SSL-certificaat voor een Azure-Website](app-service-web-tutorial-custom-ssl.md). |
| Visual Studio-integratie |X |X |X |X | |
| Foutopsporing op afstand |X |X |X | | |
| Code implementeren in TFS |X |X |X |X | |
| Netwerkisolatie met [Azure Virtual Network](/azure/virtual-network/) |X |X |X |X |Zie ook [Azure Websites virtuele netwerkintegratie](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Ondersteuning voor [met Azure Traffic Manager](/azure/traffic-manager/) |X |X |X |X | |
| Geïntegreerde Eindpuntcontrole |X |X |X | | |
| Externe bureaublad toegang tot servers | |X |X |X | |
| Een aangepaste MSI installeren | |X |X |X |Service Fabric kunt u voor het hosten van een uitvoerbaar bestand als een [Gast uitvoerbaar bestand](../service-fabric/service-fabric-deploy-existing-app.md) of u kunt een app installeren op de virtuele machines. |
| Startfase taken definiëren/uitvoeren | |X |X |X | |
| Kan luisteren naar de ETW-gebeurtenissen | |X |X |X | |

## <a name="scenarios"></a>Scenario's en aanbevelingen
Hier volgen enkele algemene scenario's van de toepassing uit met betrekking tot welke Azure webhosting optie mogelijk meest geschikt is voor elk aanbevelingen.

* [Ik heb nodig een webfront-end met achtergrond verwerking en database back-end van zakelijke toepassingen die zijn geïntegreerd met lokale activa.](#onprem)
* [Ik moet een betrouwbare manier voor het hosten van mijn bedrijfswebsite die goed schaalbaar en aanbiedingen globale bereiken.](#corp)
* [Ik heb een IIS6-toepassing op Windows Server 2003 wordt uitgevoerd.](#iis6)
* [Ik ben eigenaar van een klein bedrijf en ik heb een goedkope manier voor het hosten van Mijn site nodig, maar met toekomstige groei in gedachten.](#smallbusiness)
* [Ik ben een web- of grafisch ontwerper en ik wil ontwerpen en bouwen van websites voor mijn klanten.](#designer)
* [Ik ben mijn toepassing met meerdere lagen met een webfront-end migreren naar de Cloud.](#multitier)
* [Mijn toepassing afhankelijk is van een zeer aangepaste Windows of Linux-omgevingen en I wilt verplaatsen naar de cloud.](#custom)
* [Mijn site maakt gebruik van open-sourcesoftware en ik wil deze hosten in Azure.](#oss)
* [Ik heb een line-of-business-toepassing die verbinding moet maken met het bedrijfsnetwerk.](#lob)
* [Ik wil een REST-API of de webservice voor mobiele clients hosten.](#mobile)

### <a id="onprem"></a>Ik heb nodig een webfront-end met achtergrond verwerking en database back-end van zakelijke toepassingen die zijn geïntegreerd met lokale activa.
Azure App Service is een uitstekende oplossing voor complexe business-toepassingen. U kunt ontwikkelen van apps die automatisch schalen op een load balanced platform, zijn beveiligd met Active Directory en verbinding maken met uw lokale bronnen. Er wordt het beheer van apps die via een hoogwaardige portal en API's eenvoudig, en kunt u meer inzicht krijgen in hoe klanten ze worden gebruikt met hulpprogramma's voor app-inzicht. De [Webjobs] [ Webjobs] functie kunt u achtergrondprocessen uitvoeren en taken als onderdeel van uw weblaag, terwijl hybride verbindingen en VNET-functies kunnen u eenvoudig verbinding weer naar lokale bronnen. Azure App Service biedt drie 9 SLA voor web-apps en kunt u:

* Uw toepassingen betrouwbaar uitvoeren op een zelfherstellende, automatisch patchen cloudplatform.
* Schalen automatisch via een wereldwijd netwerk van datacenters.
* Back-up en herstel voor herstel na noodgevallen.
* ISO, SOC2 en PCI compatibel zijn.
* Integratie met Active Directory

### <a id="corp"></a>Ik moet een betrouwbare manier voor het hosten van mijn bedrijfswebsite die goed schaalbaar en aanbiedingen globale bereiken.
Azure App Service is een uitstekende oplossing voor het hosten van zakelijke websites. Kan web-apps te schalen snel en eenvoudig om te voldoen aan de vraag over een wereldwijd netwerk van datacenters. Lokale reach fouttolerantie en intelligente verkeer management biedt. Alle op een platform waarmee hoogwaardige beheerhulpprogramma's, zodat u meer inzicht krijgen in de sitestatus en siteverkeer snel en eenvoudig. Azure App Service biedt drie 9 SLA voor web-apps en kunt u:

* Uw websites betrouwbaar uitvoeren op een zelfherstellende, automatisch patchen cloudplatform.
* Schalen automatisch via een wereldwijd netwerk van datacenters.
* Back-up en herstel voor herstel na noodgevallen.
* Beheren van Logboeken en verkeer met geïntegreerde hulpmiddelen.
* ISO, SOC2 en PCI compatibel zijn.
* Integratie met Active Directory

### <a id="iis6"></a>Ik heb een IIS6-toepassing op Windows Server 2003 wordt uitgevoerd.
Azure App Service kunt gemakkelijk te voorkomen dat de kosten van de infrastructuur die is gekoppeld aan het migreren van oudere IIS6-toepassingen. Microsoft heeft gemaakt [hulpprogramma's voor migratie eenvoudig te gebruiken en gedetailleerde migratie-richtlijnen](https://www.movemetowebsites.net/) waarmee u compatibiliteit controleren en eventuele wijzigingen beschreven die moeten worden aangebracht. Integratie met Visual Studio, TFS en algemene CMS-hulpprogramma's kunt gemakkelijk IIS6 toepassingen rechtstreeks naar de cloud implementeren. Zodra geïmplementeerd, de Azure Portal de biedt krachtige beheerprogramma's waarmee u kunt het omlaag schalen voor het beheren van kosten en maximaal voldoen aan vraag zo nodig. U kunt met het hulpprogramma voor migratie:

* Snel en eenvoudig migreren uw oude Windows Server 2003-webtoepassing met de cloud.
* Als u wilt uw gekoppelde SQL database on-premises voor het maken van een toepassing hybride laten kiezen.
* De SQL-database, samen met de oudere toepassing verplaatst automatisch.

### <a id="smallbusiness"></a>Ik ben eigenaar van een klein bedrijf en ik heb een goedkope manier voor het hosten van Mijn site nodig, maar met toekomstige groei in gedachten.
Azure App Service is een uitstekende oplossing voor dit scenario, omdat u kunt gratis gebruiken en vervolgens meer mogelijkheden toevoegen wanneer u deze nodig. Elke gratis web-app wordt geleverd met een domein dat is verstrekt door Azure (*your_company*. azurewebsites.net), en het platform omvat geïntegreerde hulpprogramma's voor implementatie en beheer, evenals een toepassingsgalerie waarmee u snel aan de slag. Er zijn veel andere services en schalingsopties waarmee de site met de toegenomen gebruikersvraag ontwikkelen. U kunt met Azure App Service:

* Beginnen met de laag gratis en schaal omhoog indien nodig.
* Gebruik de Toepassingsgalerie snel populaire webtoepassingen, zoals WordPress instellen.
* Extra Azure-services en functies toevoegen aan uw toepassing naar behoefte.
* Beveilig uw web-app met HTTPS.

### <a id="designer"></a>Ik ben een web- of grafisch ontwerper en ik wil ontwerpen en bouwen van websites voor mijn klant
Biedt ondersteuning voor Git en FTP-implementatie en nauwe integratie met hulpprogramma's en services, zoals Visual Studio en SQL-Database biedt voor webontwikkelaars- en ontwerpers Azure App Service kan eenvoudig worden geïntegreerd met tal van frameworks en hulpprogramma's. Met App Service kunt u:

* Gebruik de opdrachtregelprogramma's voor [geautomatiseerde taken][scripting].
* Werken met populaire talen zoals [.Net][dotnet], [PHP][PHP], [Node.js] [ nodejs], en [Python][Python].
* Selecteer de drie verschillende schalen niveaus voor het schalen van zeer hoge capaciteit.
* Integreren met andere Azure-services, zoals [SQL-Database][sqldatabase], [Service Bus] [ servicebus] en [opslag] [ Storage], of de partner van de offerings uit de [Azure Store][azurestore], zoals MySQL en MongoDB.
* Geïntegreerd met hulpprogramma's, zoals Visual Studio, Git WebMatrix, WebDeploy, TFS en FTP.

### <a id="multitier"></a>Ik ben mijn toepassing met meerdere lagen met een webfront-end migreren naar de Cloud
Als u een toepassing met meerdere lagen, zoals een webserver die is verbonden met een database, is Azure App Service een goede optie die nauwe integratie met Azure SQL Database biedt. En u kunt de API-functie voor het uitvoeren van back-end-processen.

Service Fabric kiezen voor een of meer van uw lagen als u meer controle over de server-omgeving, zoals de mogelijkheid op afstand in de server of server starten van de taken configureren.

Als u wilt uw eigen installatiekopie machine gebruiken of het uitvoeren van server-software of services die u op de Service Fabric configureren kunt, kunt u virtuele Machines kiezen voor een of meer van de lagen.

### <a id="custom"></a>Mijn toepassing afhankelijk is van een zeer aangepaste Windows of Linux-omgevingen en I wilt verplaatsen naar de cloud.
Als uw toepassing vereist complexe installatie of configuratie van de software en het besturingssysteem, wordt virtuele Machines is waarschijnlijk de beste oplossing. Met virtuele Machines, kunt u het volgende doen:

* Gebruik de galerie met virtuele Machine te starten met een besturingssysteem, zoals Windows of Linux, en vervolgens aanpassen voor uw toepassingsvereisten.
* Maken en uploaden van een aangepaste installatiekopie van een bestaande lokale server worden uitgevoerd op een virtuele machine in Azure.

### <a id="oss"></a>Mijn site maakt gebruik van open-sourcesoftware en ik wil deze hosten in Azure
Als uw open-source framework wordt ondersteund op App Service, de talen en frameworks die nodig is voor uw toepassing automatisch voor u geconfigureerd. App Service kunt u:

* Gebruik veel populaire open source talen, zoals [.NET][dotnet], [PHP][PHP], [Node.js] [ nodejs], en [Python][Python].
* WordPress, Drupal, Umbraco, DNN en vele andere webtoepassingen van derden instellen.
* Migreren van een bestaande toepassing of een nieuwe maken uit de galerie met toepassingen.

Als uw open-source framework wordt niet ondersteund op App Service, kunt u deze uitvoeren op een van de andere Azure-web hosting-opties. Met virtuele Machines u installeren en configureren van de software op de machine-installatiekopie, die Windows worden kan of Linux-.

### <a id="lob"></a>Ik heb een line-of-business-toepassing die verbinding moet maken met het bedrijfsnetwerk.
Als u een line-of-business-toepassing maken wilt, is uw website mogelijk directe toegang tot de services of gegevens op het bedrijfsnetwerk. Dit is mogelijk op App Service, Service Fabric en virtuele Machines met de [Azure Virtual Network service](/azure/virtual-network/). Op App Service kunt u de [VNET-integratiefunctie](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), waardoor uw Azure-toepassingen worden uitgevoerd alsof ze in uw bedrijfsnetwerk.

### <a id="mobile"></a>Ik wil een REST-API of de webservice voor mobiele clients hosten
HTTP-gebaseerde web-services kunnen u een groot aantal clients, waaronder mobiele clients ondersteunen. Frameworks zoals ASP.NET Web API worden geïntegreerd met Visual Studio om eenvoudiger maken en gebruiken van REST-services.  Deze services worden blootgesteld vanaf een webeindpunt, zodat het mogelijk een techniek op Azure voor webhosting gebruiken ter ondersteuning van dit scenario is. App Service is echter een uitstekende keuze is voor het hosten van de REST-API's. Met App Service kunt u:

* Snel maken een [mobiele app](../app-service-mobile/app-service-mobile-value-prop.md) of API-app voor het hosten van de HTTP-web-service in een Azure globaal gedistribueerd datacenters.
* Migreren van bestaande services of nieuwe maken.
* SLA voor beschikbaarheid met één exemplaar bereiken of uitschalen naar meerdere toegewezen machines.
* De gepubliceerde site gebruiken voor REST-API's op een HTTP-clients, waaronder mobiele clients.

> [!NOTE]
> Als u wilt dat aan de slag met Azure App Service voordat u zich aanmeldt voor een account, gaat u naar <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, waar u kunt direct een tijdelijke en eenvoudige app maken in Azure App Service gratis. Geen creditcard nodig en bent nergens toe verplicht.
> 
> 

## <a id="nextsteps"></a>De volgende stappen
Voor meer informatie over de drie opties host web, Zie [Introducing Azure](../fundamentals-introduction-to-azure.md).

Zie de volgende bronnen om te beginnen met de gekozen opties voor uw toepassing:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Virtuele Machines in Azure](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: app-service-web-tutorial-custom-ssl.md
[azurestore]: https://azuremarketplace.microsoft.com/en-us/marketplace/apps
[scripting]: https://azure.microsoft.com/documentation/scripts/?services=web-sites
[dotnet]: https://azure.microsoft.com/develop/net/
[nodejs]: https://azure.microsoft.com/develop/nodejs/
[PHP]: https://azure.microsoft.com/develop/php/
[Python]: https://azure.microsoft.com/develop/python/
[servicebus]: /azure/service-bus/
[sqldatabase]: /azure/sql-database/
[Storage]: /azure/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
