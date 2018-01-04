---
title: Vergelijking van Azure App Service, Virtual Machines, Service Fabric en Cloud Services | Microsoft Docs
description: Informatie over hoe u kiest tussen Azure App Service, Virtual Machines, Service Fabric en Cloud Services voor het hosten van webtoepassingen.
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
ms.openlocfilehash: 0dba36e5490af56debd3b64b20d39809cd5d5f81
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Vergelijking van Azure App Service, Virtual Machines, Service Fabric en Cloud Services
## <a name="overview"></a>Overzicht
Azure biedt verschillende manieren om websites te hosten: [Azure App Service][Azure App Service], [Virtual Machines][Virtual Machines], [Service Fabric][Service Fabric] en [Cloud Services][Cloud Services]. Dit artikel geeft u inzicht in de diverse opties en helpt u de juiste keuze te maken voor uw webtoepassing.

Azure App Service is de beste keuze voor de meeste web-apps. Implementatie en beheer zijn geïntegreerd in het platform, de schaal van sites kan snel worden aangepast bij intensief verkeer, en de ingebouwde taakverdeling en verkeersbeheer zorgen voor hoge beschikbaarheid. U kunt bestaande sites eenvoudig verplaatsen naar Azure App Service met een [online migratiehulpprogramma](https://www.migratetoazure.net/), een open source-app uit de galerie met webtoepassingen gebruiken of een nieuwe site maken met behulp van een framework en hulpprogramma's van uw keuze. Met de functie [WebJobs][WebJobs] kunt u eenvoudig achtergrondverwerking van taken toevoegen aan uw App Service-web-app.

Service Fabric is een goede keuze als u een nieuwe app maakt of een bestaande app herschrijft voor gebruik met een microservice-architectuur. Apps die worden uitgevoerd in een gedeelde groep machines, kunnen kleinschalig beginnen en afhankelijk van de behoefte opschalen tot wel honderden of duizenden machines. Stateful services maken het eenvoudig om de app-status consistent en betrouwbaar op te slaan en Service Fabric beheert automatisch de partitionering, schaalbaarheid en beschikbaarheid van de service voor u.  Service Fabric ondersteunt ook WebAPI met Open Web Interface voor .NET (OWIN) en ASP.NET Core.  Vergeleken met App Service biedt Service Fabric ook meer controle over en directe toegang tot de onderliggende infrastructuur. U hebt op afstand toegang tot uw servers en u kunt taken configureren om de server te starten. Cloud Services biedt een vergelijkbare mate van beheer en gebruiksgemak als Service Fabric, maar is een verouderde service. Er wordt aanbevolen bij de ontwikkeling van nieuwe toepassingen Service Fabric te gebruiken.

Als u een bestaande toepassing hebt waarvoor aanzienlijke wijzigingen nodig zouden zijn om deze uit te voeren in App Service of Service Fabric, kunt u Virtual Machines gebruiken om de migratie naar de cloud te vereenvoudigen. Het correct configureren, beveiligen en onderhouden van virtuele machines vereist echter veel meer tijd en IT-expertise, vergeleken met Azure App Service en Service Fabric. Als u Azure Virtual Machines overweegt, moet u rekening houden met doorlopende onderhoudswerkzaamheden voor het patchen, bijwerken en beheren van uw VM-omgeving. Azure Virtual Machines vormt een Infrastructure-as-a-Service (IaaS), terwijl App Service en Service Fabric een Platform-as-a-Service (PaaS) zijn. 

## <a name="features"></a>Vergelijking van functies
In de volgende tabel worden de mogelijkheden van App Service, Cloud Services, Virtual Machines en Service Fabric vergeleken, zodat u de beste keuze kunt maken voor uw toepassing. Raadpleeg de [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/) voor actuele informatie over de SLA voor elke optie.

| Functie | App Service (web-apps) | Cloud Services (webrollen) | Virtuele machines | Service Fabric | Opmerkingen |
| --- | --- | --- | --- | --- | --- |
| Vrijwel directe implementatie |X | | |X |Het implementeren van een toepassing of update van een toepassing naar een cloudservice, of het maken van een virtuele machine duurt minstens enkele minuten; het implementeren van een toepassing naar een web-app kost slechts enkele seconden. |
| Opschalen naar grotere machines zonder opnieuw te hoeven implementeren |X | | |X | |
| Webserver-exemplaren delen inhoud en configuratie, wat betekent dat u niet opnieuw hoeft te implementeren of te configureren als u de schaal wijzigt. |X | | |X | |
| Meerdere implementatieomgevingen (productie- en faseringsomgeving) |X |X | |X |Met Service Fabric kunt u meerdere omgevingen gebruiken voor uw apps of verschillende versies van uw app naast elkaar implementeren. |
| Automatisch beheer van updates voor het besturingssysteem |X |X | | |Gedeeltelijk via Patch Orchestration-toepassing (POA) en in de toekomst volledig. |
| Naadloos overschakelen tussen platforms (gemakkelijk overschakelen tussen 32 bits en 64 bits) |X |X | | | |
| Code implementeren via GIT, FTP |X | |X | | |
| Code implementeren met Web Deploy |X | |X | |Cloud Services ondersteunt het gebruik van Web Deploy om afzonderlijke rolinstanties bij te werken. U kunt dit echter niet gebruiken voor de initiële implementatie van een rol. Als u Web Deploy voor een update gebruikt, moet u dit ook voor elk exemplaar van die rol afzonderlijk implementeren. Meerdere exemplaren zijn vereist om in aanmerking te komen voor de Cloud Service-SLA voor productieomgevingen. |
| Ondersteuning voor WebMatrix |X | |X | | |
| Toegang tot services als Service Bus, Storage, SQL Database |X |X |X |X | |
| Laag voor web of webservices hosten van een architectuur met meerdere lagen |X |X |X |X | |
| Middelste laag hosten van een architectuur met meerdere lagen |X |X |X |X |App Service-web-apps kunnen eenvoudig de middelste laag van een REST API hosten en de functie [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) kan taken voor achtergrondverwerking hosten. U kunt WebJobs uitvoeren op een speciale website om onafhankelijke schaalbaarheid voor die laag mogelijk te maken. |
| Geïntegreerde ondersteuning voor MySQL-as-a-service |X |X | | | |
| Ondersteuning voor ASP.NET, klassieke ASP, Node.js, PHP, Python |X |X |X |X |Service Fabric ondersteunt het maken van een web-front-end met behulp van [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) en u kunt elk type toepassing (Node.js, Java enz.) implementeren als een [toepassing die door een gast kan worden uitgevoerd](../service-fabric/service-fabric-deploy-existing-app.md). |
| Uitschalen naar meerdere exemplaren zonder opnieuw te hoeven implementeren |X |X |X |X |Virtual Machines kan uitschalen naar meerdere exemplaren, maar de services die daarop worden uitgevoerd, moeten zijn geschreven om hiermee om te kunnen gaan. U moet een load balancer configureren om aanvragen tussen de machines te routeren en een affiniteitsgroep maken die voorkomt dat alle exemplaren tegelijkertijd opnieuw worden opgestart wegens onderhoud of hardwarefouten. |
| Ondersteuning voor SSL |X |X |X |X |Voor App Service-web-apps wordt SSL voor aangepaste domeinnamen alleen ondersteund voor de Basic en Standard-modus. Zie [Configuring an SSL certificate for an Azure Website](app-service-web-tutorial-custom-ssl.md) (Een SSL-certificaat voor een Azure-website configureren) voor meer informatie over het gebruik van SSL met web-apps. |
| Integratie met Visual Studio |X |X |X |X | |
| Foutopsporing op afstand |X |X |X | | |
| Code implementeren via TFS |X |X |X |X | |
| Netwerkisolatie met [Azure Virtual Network](/azure/virtual-network/) |X |X |X |X |Zie ook [Integratie van Azure Websites Virtual Network](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Ondersteuning voor [Azure Traffic Manager](/azure/traffic-manager/) |X |X |X |X | |
| Geïntegreerde eindpuntbewaking |X |X |X | | |
| Toegang tot servers via Extern bureaublad | |X |X |X | |
| Aangepaste MSI's installeren | |X |X |X |Met Service Fabric kunt u ieder uitvoerbaar bestand hosten als een [toepassing die door een gast kan worden uitgevoerd](../service-fabric/service-fabric-deploy-existing-app.md) en u kunt iedere app installeren op de virtuele machines. |
| Mogelijkheid om starttaken te definiëren en uit te voeren | |X |X |X | |
| Kan luisteren naar ETW-gebeurtenissen | |X |X |X | |

## <a name="scenarios"></a>Scenario's en aanbevelingen
Hier volgen enkele veelvoorkomende scenario's met aanbevelingen over welk soort Azure-webhosting mogelijk het meest geschikt is.

* [Ik heb een web-front-end nodig met achtergrondverwerking en een database-back-end om zakelijke toepassingen uit te voeren met integratie voor on-premises assets.](#onprem)
* [Ik heb een betrouwbare manier nodig om mijn bedrijfswebsite te hosten die goed schaalbaar is en wereldwijd bereik heeft.](#corp)
* [Ik heb een IIS6-toepassing die op Windows Server 2003 wordt uitgevoerd.](#iis6)
* [Ik ben eigenaar van een klein bedrijf en ik heb een goedkope manier nodig om mijn site te hosten, die ook ruimte biedt voor toekomstige groei.](#smallbusiness)
* [Ik ben een web- of grafisch ontwerper en ik wil websites ontwerpen en bouwen voor mijn klanten.](#designer)
* [Ik ben mijn toepassing met meerdere lagen en een web-front-end aan het migreren naar de cloud.](#multitier)
* [Mijn toepassing is afhankelijk van op maat gemaakte Windows- of Linux-omgevingen en ik wil deze naar de cloud verplaatsen.](#custom)
* [Mijn site maakt gebruik van open source-software en ik wil deze hosten in Azure.](#oss)
* [Ik heb een line-of-business-toepassing die verbinding moet maken met het bedrijfsnetwerk.](#lob)
* [Ik wil een REST API of webservice voor mobiele clients hosten.](#mobile)

### <a id="onprem"></a> Ik heb een web-front-end nodig met achtergrondverwerking en een database-back-end om zakelijke toepassingen uit te voeren met integratie voor on-premises assets.
Azure App Service is een uitstekende oplossing voor complexe zakelijke toepassingen. U kunt hiermee apps ontwikkelen die automatisch schalen op een platform met een load balancer, zijn beveiligd met behulp van Active Directory en verbinding maken met uw on-premises resources. Het beheer van deze apps wordt eenvoudiger door de hoogwaardige portal en API's, en u kunt meer inzicht krijgen in hoe klanten deze apps gebruiken met behulp van de hulpprogramma's voor app-inzicht. U kunt achtergrondprocessen en taken uitvoeren als onderdeel van uw weblaag, met behulp van de functie [Webjobs][Webjobs], en hybride verbindingen en VNET-functies zorgen ervoor dat u eenvoudig verbinding kunt maken met on-premises resources. Azure App Service biedt drie-maal-negen-SLA's voor web-apps en stelt u in staat om:

* uw toepassingen betrouwbaar uit te voeren op een zelfherstellend, automatisch bijgewerkt cloudplatform;
* automatisch te schalen in een wereldwijd netwerk van datacenters;
* back-up en herstel na noodgevallen te gebruiken;
* de naleving van ISO-, SOC2- en PCI-normen te bewaken.
* Integratie met Active Directory

### <a id="corp"></a> Ik heb een betrouwbare manier nodig om mijn bedrijfswebsite te hosten die goed schaalbaar is en wereldwijd bereik heeft.
Azure App Service is een uitstekende oplossing voor het hosten van zakelijke websites. Hiermee kan de schaal van web-apps snel en eenvoudig worden aangepast in een wereldwijd netwerk van datacenters om te voldoen aan de vraag. De service biedt lokaal bereik, fouttolerantie en intelligent verkeersbeheer. En dat allemaal op een platform dat ook hoogwaardige beheerhulpprogramma's biedt, zodat u snel en eenvoudig meer inzicht krijgt in de sitestatus en het siteverkeer. Azure App Service biedt drie-maal-negen-SLA's voor web-apps en stelt u in staat om:

* uw websites betrouwbaar uit te voeren op een zelfherstellend, automatisch bijgewerkt cloudplatform;
* automatisch te schalen in een wereldwijd netwerk van datacenters;
* back-up en herstel na noodgevallen te gebruiken;
* logboeken en verkeer te beheren met geïntegreerde hulpmiddelen;
* de naleving van ISO-, SOC2- en PCI-normen te bewaken.
* Integratie met Active Directory

### <a id="iis6"></a> Ik heb een IIS6-toepassing die in Windows Server 2003 wordt uitgevoerd.
Azure App Service maakt het eenvoudig om de infrastructuurkosten te voorkomen die gepaard gaan met het migreren van oudere IIS6-toepassingen. Microsoft heeft [gebruiksvriendelijke hulpprogramma's voor migratie en gedetailleerde migratierichtlijnen](https://www.migratetoazure.net/) gemaakt waarmee u de compatibiliteit kunt controleren en eventuele wijzigingen kunt identificeren die moeten worden aangebracht. Integratie met Visual Studio, TFS en veelgebruikte CMS-hulpprogramma's maakt het eenvoudig om IIS6-toepassingen rechtstreeks naar de cloud te implementeren. Wanneer deze is geïmplementeerd, biedt Azure Portal krachtige beheerprogramma's waarmee u omlaag kunt schalen om de kosten te beperken en omhoog kunt schalen om aan een toenemende vraag te voldoen. Met het hulpprogramma voor migratie kunt u:

* snel en eenvoudig uw oude Windows Server 2003-webtoepassing naar de cloud migreren;
* ervoor kiezen om uw gekoppelde SQL-database on-premises te houden en een hybride toepassing te maken;
* uw SQL-database automatisch samen met uw oudere toepassing te verplaatsen.

### <a id="smallbusiness"></a> Ik ben eigenaar van een klein bedrijf en ik heb een goedkope manier nodig om mijn site te hosten, die ook ruimte biedt voor toekomstige groei.
Azure App Service is een uitstekende oplossing voor dit scenario, omdat u dit gratis in gebruik kunt nemen en vervolgens meer mogelijkheden kunt toevoegen wanneer u deze nodig hebt. Elke gratis web-app wordt door Azure van een domein voorzien (*uw_bedrijf*.azurewebsites.net) en het platform biedt geïntegreerde hulpprogramma's voor implementatie en beheer, evenals een toepassingsgalerie waarmee u snel aan de slag kunt. Er zijn bovendien nog talloze andere services en schalingsopties waarmee uw site kan meegroeien met een toegenomen gebruikersvraag. Met Azure App Service kunt u:

* beginnen met de gratis laag en omhoog schalen wanneer dit nodig is;
* de toepassingsgalerie gebruiken om snel populaire webtoepassingen in te stellen, zoals WordPress;
* extra Azure-services en -functies aan uw toepassing toevoegen wanneer u deze nodig hebt;
* uw web-app beveiligen met HTTPS.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

### <a id="designer"></a> Ik ben een web- of grafisch ontwerper en ik wil websites ontwerpen en bouwen voor mijn klanten.
Voor webontwikkelaars en ontwerpers biedt Azure App Service eenvoudige integratie met tal van frameworks en hulpmiddelen, zoals ondersteuning voor implementatie via Git en FTP, en een verregaande integratie met hulpprogramma's en services als Visual Studio en SQL Database. Met App Service kunt u:

* opdrachtregelprogramma's gebruiken voor [geautomatiseerde taken][scripting];
* werken met populaire talen als [.NET][dotnet], [PHP][PHP], [Node.js][nodejs] en [Python][Python];
* drie verschillende schaalniveaus selecteren om omhoog te schalen tot een zeer hoge capaciteit;
* integreren met andere Azure-services, zoals [SQL Database][sqldatabase], [Service Bus][servicebus] en [Storage][Storage] of het aanbod aan diensten van partners in de [Azure Store][azurestore] gebruiken, zoals MySQL en MongoDB;
* integreren met hulpprogramma's als Visual Studio, Git WebMatrix, WebDeploy, TFS en FTP.

### <a id="multitier"></a>Ik ben mijn toepassing met meerdere lagen en een web-front-end aan het migreren naar de cloud
Als u een toepassing met meerdere lagen uitvoert, zoals een webserver die is verbonden met een database, is Azure App Service een goede optie die nauwe integratie met Azure SQL Database biedt. U kunt bovendien de Webjobs-functie gebruiken voor het uitvoeren van back-end-processen.

Als u bij een toepassing met een of meer lagen meer controle over de serveromgeving wilt, zoals de mogelijkheid om u op afstand aan te melden bij de server of starttaken voor de server te configureren, kunt u het beste kiezen voor Service Fabric.

Als u uw eigen installatiekopie wilt gebruiken of serversoftware of services wilt uitvoeren die u niet kunt configureren in Service Fabric, kunt u het beste kiezen voor Virtual Machines.

### <a id="custom"></a> Mijn toepassing is afhankelijk van op maat gemaakte Windows- of Linux-omgevingen en ik wil deze naar de cloud verplaatsen.
Als uw toepassing een complexe installatie of configuratie van de software en het besturingssysteem vereist, is Virtual Machines waarschijnlijk de beste oplossing. Met Virtual Machines kunt u:

* de galerie met virtuele machines gebruiken om te beginnen met een besturingssysteem, zoals Windows of Linux, en dit vervolgens aan te passen aan uw toepassingsvereisten;
* een aangepaste installatiekopie van een bestaande on-premises server maken en uploaden, zodat deze kan worden uitgevoerd op een virtuele Azure-machine.

### <a id="oss"></a>Mijn site maakt gebruik van open source-software en ik wil mijn site hosten in Azure.
Als uw open source-framework in App Service wordt ondersteund, worden de talen en frameworks die uw toepassing vereist, automatisch voor u geconfigureerd. Met App Service kunt u:

* vele populaire talen gebruiken, zoals [.NET][dotnet], [PHP][PHP], [Node.js][nodejs] en [Python][Python];
* WordPress, Drupal, Umbraco, DNN en vele andere webtoepassingen van derden instellen;
* een bestaande toepassing migreren of een nieuwe toepassing maken met behulp van de toepassingsgalerie.

Als uw open source-framework niet wordt ondersteund in App Service, kunt u het uitvoeren in een van de andere opties voor Azure-webhosting. U kunt Virtual Machines gebruiken en de software installeren en configureren op de installatiekopie, die gebaseerd kan zijn op Windows of Linux.

### <a id="lob"></a> Ik heb een line-of-business-toepassing die verbinding moet maken met het bedrijfsnetwerk.
Als u een line-of-business-toepassing wilt maken, heeft uw website mogelijk directe toegang nodig tot de services of gegevens op het bedrijfsnetwerk. Dit kunt u configureren in App Service, Service Fabric en Virtual Machines, met behulp van de [Azure Virtual Network-service](/azure/virtual-network/). In App Service kunt u de [VNET-integratiefunctie](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) gebruiken, waardoor uw Azure-toepassingen worden uitgevoerd alsof ze zich in uw bedrijfsnetwerk bevinden.

### <a id="mobile"></a> Ik wil een REST API of webservice voor mobiele clients hosten.
Met op HTTP gebaseerde web-services kunt u een verscheidenheid aan clients ondersteunen, waaronder mobiele clients. Frameworks als ASP.NET Web API bieden integratie met Visual Studio om het eenvoudiger te maken REST-services te maken en te gebruiken.  Deze services worden beschikbaar gemaakt vanaf een webeindpunt, zodat u alle technieken voor webhosting in Azure kunt gebruiken om dit scenario te ondersteunen. App Service is echter een uitstekende keuze voor het hosten van REST API's. Met App Service kunt u:

* snel een [mobiele app](../app-service-mobile/app-service-mobile-value-prop.md) of API-app maken om de HTTP-webservice te hosten in een van de wereldwijd verspreide Azure-datacenters;
* bestaande services migreren of nieuwe services maken;
* aan SLA's voor beschikbaarheid voldoen met één exemplaar of uitschalen naar meerdere toegewezen machines;
* de gepubliceerde site gebruiken om REST API's te bieden voor allerlei HTTP-clients, waaronder mobiele clients.

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een account, gaat u naar <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>. Hier kunt u direct gratis een tijdelijke app maken in Azure App Service. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a id="nextsteps"></a> Volgende stappen
Raadpleeg [Introducing Azure](../fundamentals-introduction-to-azure.md) (Kennismaken met Azure) voor meer informatie over de drie opties voor webhosting.

Raadpleeg de volgende bronnen om aan de slag te gaan met de gekozen opties voor uw toepassing:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Azure Virtual Machines](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
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
