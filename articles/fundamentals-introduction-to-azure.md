---
title: Inleiding tot Microsoft Azure | Microsoft Docs
description: Nieuw in Microsoft Azure? Ophalen van een eenvoudig overzicht van de services biedt met voorbeelden van hoe ze handig zijn.
services: " "
documentationcenter: .net
author: rboucher
manager: carolz
editor: 
ms.assetid: 6f47f711-2208-4c21-8c1d-826a54c05c29
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2015
ms.author: robb
ms.openlocfilehash: efcafa40c3d47ebfdd4520f98b65ad2c05b03c57
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="introducing-microsoft-azure"></a>Inleiding tot Microsoft Azure
Microsoft Azure is Microsoft toepassingsplatform voor de openbare cloud.  Het doel van dit artikel is het bieden u een basis voor informatie over de grondbeginselen van Azure, zelfs als u niet over cloud weet computing.

**In dit artikel lezen**

Azure wordt voortdurend groeiende zodat u gemakkelijk te overbelast raakt.  Beginnen met de basisservices die eerst in dit artikel worden vermeld, en ga vervolgens verder met extra services. Dat betekent niet dat u zojuist de extra services niet gebruiken op zichzelf. Maar de basisservices vormen de kern van een toepassing die wordt uitgevoerd in Azure.

**Feedback geven**

Uw feedback is van belang. In dit artikel geeft u een effectieve overzicht van Azure. Als dit niet het geval is, Geef in het gedeelte met opmerkingen onder aan de pagina. Sommige informatie over wat u verwacht te zien en het verbeteren van het artikel geven.  

## <a name="the-components-of-azure"></a>De onderdelen van Azure
Azure services worden gegroepeerd in categorieën die in de Azure-portal en op verschillende visuele hulpmiddelen zoals de [Wat Is Azure Infographic](https://azure.microsoft.com/documentation/infographics/azure/). De [Azure-portal](http://portal.azure.com) wordt gebruikt voor het beheren van services in Azure.

Dit artikel wordt een **andere organisatie** praten over services op basis van vergelijkbare functie en aan te roepen belangrijke onderliggende services die deel van grotere die zijn uitmaken.  

![Azure-onderdelen](./media/fundamentals-introduction-to-azure/AzureComponentsIntroNew780.png)   
 *Afbeelding: Azure biedt Internet toegankelijke toepassingsservices in Azure-datacenters wordt uitgevoerd.*

## <a name="azure-portal"></a>Azure Portal
Azure heeft een webinterface die is aangeroepen de [Azure-portal](http://portal.azure.com) waarmee beheerders voor toegang tot en beheer van Azure-functies en services.  

## <a name="compute"></a>Compute
Een van de meest eenvoudige dingen die een cloudplatform biedt is voor het uitvoeren van toepassingen. Elk van de Azure compute-modellen heeft een eigen rol spelen.

U kunt deze technologieën afzonderlijk gebruiken of ze combineren zo nodig de juiste basis voor uw toepassing te maken. De methode die u kiest is afhankelijk van welke problemen u probeert om op te lossen.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines
![Virtuele Machines in Azure ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_VirtualMachinesIntroNew_12345.png)   
*Afbeelding: Virtuele Machines in Azure hebt u volledige controle over virtuele machine-exemplaren in de cloud.*

De mogelijkheid te maken van een virtuele machine op aanvraag of van een standaardinstallatiekopie of van een die u opgeeft, kan zeer nuttig zijn. Deze benadering gewoonlijk bekend als infrastructuur als een Service (IaaS), is wat Azure Virtual Machines biedt. Afbeelding 2 ziet u een combinatie van hoe een virtuele Machine (VM) wordt uitgevoerd en hoe u een vanaf een VHD te maken.  

U opgeven welke VHD moet gebruiken en de VM-grootte voor het maken van een virtuele machine.  U betaalt vervolgens voor de tijd dat de VM wordt uitgevoerd. U betaalt per minuut en alleen terwijl deze wordt uitgevoerd, maar er een minimale opslag kosten is voor het bewaren van de VHD beschikbaar. Azure biedt tal van stock VHD's ('afbeeldingen' genoemd) die een besturingssysteem te starten vanaf bevatten. Deze omvatten Microsoft en haar partners opties, zoals Windows Server- en Linux-, SQL Server-, Oracle en nog veel meer. U kunt gratis VHD's en installatiekopieën maken en vervolgens uploaden zelf. U kunt zelfs uploaden VHD's die alleen gegevens bevatten en deze vervolgens kunt openen vanaf de actieve virtuele machines.

Waar de VHD afkomstig zijn van, kunt u alle wijzigingen terwijl een virtuele machine wordt uitgevoerd permanent opslaan. De volgende keer dat u een virtuele machine van die VHD maken dingen worden opgepikt waar u gebleven was. De virtuele harde schijven die back-de virtuele Machines worden opgeslagen in Azure Storage-blobs, die we later over hebben.  Dat betekent dat u redundantie om te controleren of dat uw virtuele machines won't verdwijnen vanwege hardware- en fouten. Het is ook mogelijk om te kopiëren van de gewijzigde VHD buiten Azure en vervolgens het lokaal uitvoeren.

Uw toepassing wordt uitgevoerd binnen een of meer virtuele Machines, afhankelijk van hoe u het eerder gemaakt of besluit nu vanaf het begin te maken.

Deze aanpak vrij algemeen cloud computing kan worden gebruikt voor het oplossen van veel verschillende problemen.

**Scenario's voor virtuele Machine**

1. **Ontwikkelen en testen** -u kunt ze gebruiken voor het maken van een goedkope ontwikkel- en -platform die u afsluiten kunt wanneer u klaar bent met het gebruik van maken. U kunt ook maken en toepassingen die gebruikmaken van welke talen en bibliotheken die u wilt uitvoeren. Deze toepassingen kunnen u elk van de opties voor het beheer van gegevens die Azure biedt, en u kunt er ook voor kiezen om SQL Server of een ander DBMS uitgevoerd op een of meer virtuele machines te gebruiken.
2. **Verplaats toepassingen naar Azure (Lift-en-shift)** -"Lift-en-shift" verwijst naar het verplaatsen van uw toepassing veel zoals wanneer u een vorkheftruck om een groot object te verplaatsen.  U "lift-" de VHD van uw lokale datacentrum en 'verschuiven' deze naar Azure en er uitvoeren.  Normaal gesproken moet u stappen ondernemen om afhankelijkheden op andere systemen te verwijderen. Als er te veel, kunt u in plaats daarvan optie 3.  
3. **Uw Datacenter uitbreiden** -gebruik Azure VM's als een uitbreiding van uw on-premises datacentrum met SharePoint of andere toepassingen. Ter ondersteuning hiervan, is het mogelijk te maken van Windows-domeinen in de cloud met behulp van Active Directory in Azure Virtual machines. Azure Virtual Network (later wordt vermeld) kunt u uw lokale netwerk en het netwerk in Azure met elkaar verbinden.

### <a name="web-apps"></a>Web Apps
![Azure-Web-Apps ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_AzureWebsitesIntroNew_12345.png)   
 *Afbeelding: Azure Web Apps een websitetoepassing uitvoert in de cloud zonder de onderliggende webserver beheren.*

Een van de meest voorkomende dingen die mensen doen in de cloud wordt websites en webtoepassingen uitgevoerd. Virtuele Machines in Azure dit toestaat, maar nog steeds laat u de verantwoordelijkheid voor het beheren van een of meer virtuele machines en de onderliggende besturingssystemen van. Cloud services-web-functies kunnen dit doen, maar implementeren en onderhouden van ze nog steeds beheertaken duurt.  Wat gebeurt er als u alleen wilt een website waar iemand anders zorgt voor de administratieve werk voor u?

Dit is precies wat Web Apps biedt. Dit model compute biedt een beheerde omgeving met behulp van de Azure-portal, alsmede de API's. U kunt een bestaande websitetoepassing naar ongewijzigd Web-Apps verplaatsen of kunt u een nieuwe rechtstreeks in de cloud. Wanneer een website wordt uitgevoerd, kunt u toevoegen of exemplaren dynamisch verwijderen vertrouwen op Azure Web Apps laden van aanvragen verdelen over ertussen. Apps van Azure biedt zowel een gedeelde optie, waar uw website wordt uitgevoerd in een virtuele machine met andere sites, en een standaard optie waarmee u een site in een eigen virtuele machine uit te voeren. De standaard optie kunt u de grootte (rekenkracht) van uw exemplaren vergroten.

Voor ontwikkeling ondersteunt Web-Apps .NET, PHP, Node.js, Java en Python samen met de SQL-Database en de Azure-Database voor MySQL voor relationele opslag. Het biedt ook ingebouwde ondersteuning voor verschillende veelgebruikte toepassingen, zoals WordPress, Joomla en Drupal. Het doel is het bieden van een goedkope, schaalbare en veelzijdige, handige platform voor het maken van websites en webtoepassingen in de openbare cloud.

**Web-Apps-scenario 's**

Web-Apps is bedoeld om het nuttig zijn voor ondernemingen, ontwikkelaars en instanties van web-ontwerp. Voor ondernemingen is het een gemakkelijk te beheren, schaalbare, maximaal beveiligde en maximaal beschikbare oplossing voor het uitvoeren van de aanwezigheid websites. Wanneer u voor het instellen van een Website, is het raadzaam te starten met Azure-Web-Apps en doorgaan met Cloud-Services wanneer u moet een functie die niet beschikbaar. Zie het einde van de sectie 'Compute' voor meer koppelingen kunt u kiezen tussen de opties.

### <a name="cloud-services"></a>Cloud Services
![Azure-Cloudservice](./media/fundamentals-introduction-to-azure/CloudServicesIntroNew.png)   
*Afbeelding: Azure Cloud Services hier kunt u zeer schaalbare aangepaste code uitvoeren op een Platform als een Service (PaaS)-omgeving*

Stel dat u wilt een cloudtoepassing bouwt die veel gelijktijdige gebruikers ondersteunen, hoeft niet veel beheer en nooit uitvalt. Bent u mogelijk een tot stand gebrachte softwareleverancier bijvoorbeeld die heeft besloten te spelen op Software als een Service (SaaS) door het maken van een versie van een van uw toepassingen in de cloud. Of u mogelijk een beginnend bedrijf bent het maken van een consumer-toepassing die u verwacht te snel groeien. Als u in Azure maakt, welk model uitvoeren moet u gebruiken?

Azure-Web-Apps kunnen maken van dit soort webtoepassing, maar er zijn enkele beperkingen. U hebt geen beheerderstoegang, bijvoorbeeld, wat betekent dat u kunt willekeurige software niet installeren. Virtuele Machines van Azure kunt u veel flexibiliteit, met inbegrip van beheerderstoegang. U kunt deze gebruiken voor het bouwen van een toepassing zeer schaalbare. Maar u hebt voor het afhandelen van veel aspecten van betrouwbaarheid en -beheer. Wat u wilt dat is een optie waarmee u het besturingselement dat u nodig hebt, maar ook voert het grootste deel van het werk dat nodig is voor de betrouwbaarheid en beheer.

Dit is precies wat wordt verstrekt door Azure Cloud Services. Deze technologie uitdrukkelijk is ontworpen ter ondersteuning van schaalbare, betrouwbare en lage admin-toepassingen en van een voorbeeld van wat is genoemd Platform als een Service (PaaS). Als u wilt gebruiken, moet u een toepassing met de technologie die u, zoals C#, Java, PHP, Python, Node.js, of iets anders kiest maken. Uw code vervolgens in virtuele machines (aangeduid als exemplaren) wordt uitgevoerd met een versie van Windows Server.

Maar deze virtuele machines zijn hetzelfde als degene die u met Azure Virtual Machines maken. Voor één ding, Azure zelf beheert ze doen, zoals het installeren van patches voor besturingssysteem en installatiekopieën automatisch nieuwe rolt hersteld. Dit betekent dat de toepassing mag niet worden gebruikt voor het onderhouden van de status in web- of worker-rolexemplaren; Deze moet in plaats daarvan worden opgeslagen in een van de Azure data management-opties in de volgende sectie beschreven. Azure bewaakt ook deze virtuele machines, opnieuw starten van ieder die mislukken. U kunt instellen dat cloudservices voor het automatisch maken van meer of minder exemplaren in reactie op de vraag. Hiermee kunt u afhandelen van toegenomen gebruik en daarna geschaald terug zodat u zo veel mogelijk worden niet betaalt wanneer er minder informatie over het gebruik.

U hebt twee rollen waaruit u kunt kiezen wanneer u een exemplaar maakt, zowel op basis van Windows Server. Het belangrijkste verschil tussen de twee is dat een exemplaar van een Webrol IIS uitvoert, maar niet door een exemplaar van een werkrol. Beide op dezelfde manier worden beheerd, en is het gebruikelijk dat voor een toepassing moet u beide gebruiken. Een exemplaar van de rol web kan bijvoorbeeld accepteer aanvragen van gebruikers en aan een exemplaar van de rol worker voor verwerking door te geven. Als u wilt uw toepassing omhoog of omlaag schalen, kunt u dat Azure meer exemplaren van de rol maken of bestaande exemplaren afgesloten vragen. En vergelijkbare op Azure Virtual Machines, u bent in rekening gebracht alleen voor de tijd dat elk exemplaar van de functie web- of worker wordt uitgevoerd.

**Cloud Services-scenario 's**

Cloudservices zijn ideaal voor de ondersteuning van grootschalige uit wanneer u moet meer controle over het platform dan door Azure Web Apps, maar geen controle over het onderliggende besturingssysteem hoeft.

#### <a name="choosing-a-compute-model"></a>Een Compute-Model kiezen
De pagina [vergelijking van Azure Web Apps, Cloudservices en virtuele Machines](app-service/choose-web-site-cloud-service-vm.md) voorziet in gedetailleerde informatie over het kiezen van een Compute-model.

## <a name="data-management"></a>Gegevensbeheer
Toepassingen gegevens nodig en verschillende soorten toepassingen moeten verschillende soorten gegevens. Als gevolg hiervan biedt Azure verschillende manieren opslaan en beheren van gegevens. Azure biedt veel opslagopties, maar alle zijn ontworpen voor zeer duurzaam opslag.  Met een van deze opties zijn altijd 3 kopieën van uw gegevens gesynchroniseerd tussen een Azure-datacenter--6 als u Azure back-up naar een ander datacenter ten minste 300 mijl weg met geografische redundantie toestaat.     

### <a name="in-virtual-machines"></a>In de virtuele Machines
De mogelijkheid voor het uitvoeren van SQL Server of een ander DBMS in een virtuele machine gemaakt met Azure Virtual Machines heeft al genoemde. Houd er rekening mee dat deze optie is niet beperkt tot relationele systemen; u kunt ook gratis NoSQL-technologieën zoals MongoDB en Cassandra uitvoeren. Eenvoudige it repliceren voor het uitvoeren van uw eigen databasesysteem is wat we gebruikt voor de eigen datacenters- maar het ook het beheer van die DBMS verwerking vereist.  In andere opties verwerkt Azure meer of alle voor het beheer van u.

Weer de status van de virtuele Machine en eventuele aanvullende gegevensschijf u maakt of uploadt worden ondersteund door de blob-opslag (die we later over hebben).  

### <a name="azure-sql-database"></a>Azure SQL Database
![Azure Storage SQL-Database](./media/fundamentals-introduction-to-azure/StorageAzureSQLDatabaseIntroNew.png)   

*Afbeelding: Azure SQL Database biedt een beheerde relationele databaseservice in de cloud.*

Azure biedt voor relationele opslag, de functie SQL-Database. Laat niet aan de naamgeving u te misleiden. Dit is anders dan een typische SQL-Database geleverd door SQL Server wordt uitgevoerd boven op Windows Server.  

Azure SQL Database biedt voorheen SQL Azure, alle van de belangrijkste functies van een relationele databasebeheersysteem, met inbegrip van atomische transacties gelijktijdige toegang door meerdere gebruikers met integriteit van gegevens, ANSI SQL-query's en een vertrouwde programmeermodel. Zoals SQL Server, SQL-Database toegankelijk is met behulp van Entity Framework, access ADO.NET, JDBC en andere bekende data-technologieën. Het ondersteunt ook de meeste van de T-SQL-taal, samen met SQL Server-hulpprogramma's, zoals SQL Server Management Studio. Voor iedereen bekend zijn met SQL Server (of een andere relationele database) met behulp van SQL-Database is eenvoudig.

Maar SQL-Database is niet alleen een DBMS in de cloud-it de een PaaS-service. U uw gegevens nog steeds bepalen en wie toegang heeft tot het, maar SQL-Database zorgt voor het werk dat administratieve knorvis, zoals het beheren van de hardware-infrastructuur en automatisch de software-database en het besturingssysteem up-to-date houden. SQL-Database biedt ook hoge beschikbaarheid, automatische back-ups, punt in tijd herstelmogelijkheden en exemplaren kunnen repliceren tussen de geografische regio's.  

**Scenario's voor SQL-Database**

Als u een Azure-toepassing (met een van de compute-modellen) die relationele archief behoeften maakt, zijn SQL-Database een goede optie. Toepassingen die worden uitgevoerd buiten de cloud kunnen deze service ook echter gebruiken zodat er tal van andere scenario's zijn. Bijvoorbeeld: gegevens die zijn opgeslagen in SQL-Database toegankelijk zijn vanuit andere clientsystemen, met inbegrip van desktops, laptops, tablets en telefoons. En omdat het biedt ingebouwde hoge beschikbaarheid door middel van replicatie, met behulp van SQL-Database kunt uitvaltijd te minimaliseren.

### <a name="tables"></a>Tabellen
![Azure Storage-tabellen](./media/fundamentals-introduction-to-azure/StorageTablesIntroNew.png)  

*Afbeelding: Azure-tabellen biedt een platte NoSQL-manier voor het opslaan van gegevens.*

Deze functie wordt soms verschillende condities als onderdeel van een grotere functie met de naam 'Azure Storage' genoemd. Als u 'tabellen', 'Azure-tabellen' of 'opslagtabellen' ziet, is hetzelfde is.  

En niet te verwarren met de naam: deze technologie niet relationele opslag bieden. Het is in feite een voorbeeld van een NoSQL-methode een sleutel/waarde-archief genoemd. Azure-tabellen kunnen een toepassing voor het opslaan van eigenschappen van verschillende typen, zoals tekenreeksen, gehele getallen en datums. Een toepassing kunt vervolgens een groep met eigenschappen ophalen door een unieke sleutel voor de groep. Tijdens het complexe bewerkingen zoals joins worden niet ondersteund, tabellen snelle toegang tot de ingevoerde gegevens bieden. Ze zijn ook zeer schaalbaar, met één tabel kunnen veel een terabyte die gegevens bevatten. En die overeenkomt met hun eenvoud, tabellen zijn meestal goedkoper dan relationele SQL-Database-opslag gebruiken.

**Scenario's voor tabellen**

Stel dat u wilt maken een Azure-toepassing die snelle toegang tot nodig hebt getypt gegevens, mogelijk veel ervan, maar hoeven niet te complex SQL-query's uitvoeren op deze gegevens. Stel dat u maakt een consumer-toepassing die moet worden opgeslagen profiel klantgegevens voor elke gebruiker. Uw app is het verstandig om zeer populair is, zodat u wilt toestaan voor grote hoeveelheden gegevens, maar u u niet veel met deze gegevens na het opslaan hebt, klikt u vervolgens bij het ophalen van het op eenvoudige manieren. Dit is precies het soort scenario waarbij Azure-tabellen zinvol is.

### <a name="blobs"></a>Blobs
![Azure Storage-Blobs](./media/fundamentals-introduction-to-azure/StorageBlobsIntroNew.png)    
*Afbeelding: Azure Blobs bevat niet-gestructureerde binaire gegevens.*  

Azure-Blobs (opnieuw 'Blob Storage' en alleen 'Storage-Blobs' zijn hetzelfde) is ontworpen voor het opslaan van ongestructureerde binaire gegevens. Zoals tabellen, kan Blobs biedt goedkope opslag, en één blob even groot zijn als 1TB (één terabyte). Azure-toepassingen kunnen ook gebruiken voor Azure stations waarmee blobs permanente opslag bieden voor een Windows-bestandssysteem gekoppeld in een Azure-instantie. De toepassing ziet gewone Windows-bestanden, maar de gegevens daadwerkelijk worden opgeslagen in een blob.

BLOB-opslag wordt gebruikt door veel andere Azure-functies (inclusief virtuele Machines), zodat uw workloads kan zeker te verwerken.

**Scenario's voor Blobs**

Een toepassing die wordt opgeslagen video, grote bestanden of andere binaire gegevens kunt blobs gebruiken voor eenvoudige, goedkope opslag. BLOB's worden ook vaak gebruikt in combinatie met andere services, zoals Content Delivery Network, die we later over zal hebben.  

### <a name="import--export"></a>Import/export
![Azure Import Export-Service](./media/fundamentals-introduction-to-azure/ImportExportIntroNew.png)  

*Afbeelding: Azure Import / Export biedt de mogelijkheid voor het verzenden van een fysieke harde schijf of van Azure voor sneller en goedkoper bulksgewijs gegevens importeren of exporteren.*  

Soms wilt u grote hoeveelheden gegevens naar Azure verplaatst. Die zou lang duren, mogelijk dagen en veel bandbreedte gebruiken. In dergelijke gevallen kunt u Azure Import/Export, zodat u kunt Bitlocker-versleuteling 3.5" SATA harde schijven rechtstreeks naar de Azure-datacenters, waarbij Microsoft de gegevens wordt overgezet naar de blobopslag voor u worden geleverd.  Nadat het uploaden is voltooid, wordt de stations aan u geleverd door Microsoft.  U kunt ook aanvragen dat grote hoeveelheden gegevens uit Blob Storage worden geëxporteerd naar harde schijven en verzonden naar u via e-mail.

**Scenario's voor Import / Export**

* **Grote gegevensmigratie** -telkens wanneer u grote hoeveelheden gegevens (Terabytes) die u wilt uploaden naar Azure hebt, de Import/Export-service is vaak sneller en mogelijk goedkoper dan overdragen via internet. Nadat de gegevens blobs is, kunt u deze kunt verwerken in andere formulieren zoals Table storage of een SQL-Database.
* **Gegevensherstel gearchiveerd** -kunt u voor importeren/exporteren om Microsoft-overdracht die grote hoeveelheden gegevens opgeslagen in Azure Blob-opslag met een opslagapparaat die u verzendt en vervolgens hebt dat apparaat geleverd terug naar een locatie die u wenst. Omdat dit het enige tijd duurt, is het niet een goede optie voor noodherstel. Het is raadzaam voor gearchiveerde gegevens die u snel toegang tot niet nodig.

### <a name="file-service"></a>File-Service
![Azure File-Service](./media/fundamentals-introduction-to-azure/FileServiceIntroNew.png)    
*Afbeelding: Azure Bestandsservices biedt SMB \\ \\server\share paden voor toepassingen die worden uitgevoerd in de cloud.*

On-premises is het gebruikelijk om grote hoeveelheden toegankelijk via de Server Message Block (SMB) protocol met behulp van bestandsopslag een \\ \\Server\share-indeling. Azure heeft nu een service waarmee u met dit protocol in de cloud. Toepassingen die worden uitgevoerd in Azure kunnen deze gebruiken voor het delen van bestanden tussen virtuele machines met bekend bestandssysteem API's zoals ReadFile en WriteFile. Bovendien zijn de bestanden ook toegankelijk op hetzelfde moment via een REST-interface waarmee u toegang tot de shares on-premises wanneer u ook een virtueel netwerk instellen. Azure Files is ingebouwd in de blob-service zodat de dezelfde beschikbaarheid, duurzaamheid, schaalbaarheid en geografische redundantie ingebouwd in Azure Storage worden overgenomen.

**Scenario's voor Azure Files**

* **Migreren van bestaande apps naar de cloud** -het gemakkelijker voor het migreren van on-premises toepassingen naar de cloud die bestandsshares gebruiken voor het delen van gegevens tussen onderdelen van de toepassing. Elke virtuele machine maakt verbinding met de bestandsshare en vervolgens het kan bestanden lezen en schrijven net zoals het zou tegen een lokaal bestand delen.
* **Gedeelde toepassingsinstellingen** -een algemene patroon voor gedistribueerde toepassingen is dat configuratiebestanden op een centrale locatie waar ze toegankelijk zijn vanuit vele verschillende virtuele machines. Deze configuratiebestanden kunnen worden opgeslagen in een Azure-bestandsshare en worden gelezen door alle exemplaren van een toepassing. De instellingen kunnen ook worden beheerd via de REST-interface waarmee u wereldwijd toegang krijgt tot de configuratiebestanden.
* **Diagnostische Share** : U kunt bestanden opslaan en delen diagnostische zoals Logboeken, metrische gegevens, en crashdumps. Met deze bestanden kunt beschikbaar via het SMB- en de REST-interface toepassingen kunnen gebruikmaken van tal van hulpprogramma's voor analyse voor het verwerken en analyseren van de diagnostische gegevens.
* **Dev/testen/Debug** : wanneer ontwikkelaars of beheerders op virtuele machines in de cloud werkt, moeten vaak een set hulpprogramma's of hulpprogramma's. Installatie en distributie van deze hulpprogramma's op elke virtuele machine is tijdrovend. Met Azure-bestanden, kan een ontwikkelaar of beheerder hun favoriete tools opslaan op een bestandsshare en verbinding mee te maken van een virtuele machine.

## <a name="networking"></a>Netwerken
Azure voert vandaag veel datacenters verdeeld over de hele wereld. Als u een toepassing uitvoeren of opslaan van gegevens, selecteert u een of meer van deze datacenters te gebruiken. U kunt ook verbinding maken met deze datacenters op verschillende manieren met behulp van de services die hieronder.

### <a name="virtual-network"></a>Virtueel netwerk
![VirtualNetwork](./media/fundamentals-introduction-to-azure/VirtualNetworkIntroNew.png)   

*Afbeelding: Virtuele netwerken bevat een particulier netwerk in de cloud, zodat verschillende services met elkaar kunnen communiceren, of tot lokale bronnen als u een VPN cross-premises verbinding.*  

Er is een handige manier om te gebruiken van een openbare cloud te behandelen als een uitbreiding van uw eigen datacenter.

Omdat u virtuele machines op aanvraag maken kunt, klikt u vervolgens verwijder ze (en betaalt stoppen) wanneer ze niet meer nodig zijn, kunt u rekenkracht alleen als u wilt dat deze te laten. En omdat de virtuele Machines van Azure kunt u virtuele machines met SharePoint, Active Directory en andere vertrouwde lokale software maken, deze benadering kunt werken met de toepassingen die u al hebt.

Om dit heel handig, echter zou uw gebruikers kunnen deze toepassingen worden behandeld alsof ze in uw eigen datacenter uitvoert. Dit is precies wat Azure Virtual Network is toegestaan. Met een VPN-gateway-apparaat, kunt een beheerder u een virtueel particulier netwerk (VPN) tussen uw lokale netwerk en uw virtuele machines die zijn geïmplementeerd met een virtueel netwerk in Azure instellen. Omdat u uw eigen IP-v4-adressen aan de cloud VM's toewijst, worden deze weergegeven op uw eigen netwerk. Gebruikers in uw organisatie toegang tot de toepassingen die virtuele machines bevatten alsof ze lokaal uitvoert.

Zie voor meer informatie over het plannen en maken van een virtueel netwerk dat voor u geschikt [virtueel netwerk](virtual-network/virtual-networks-overview.md).

### <a name="express-route"></a>ExpressRoute
![ExpressRoute](./media/fundamentals-introduction-to-azure/ExpressRouteIntroNew.png)   

*Afbeelding: ExpressRoute maakt gebruik van een Azure-netwerk, maar verbindingen via sneller speciale regels in plaats van het openbare Internet van routes.*  

Als u moet meer bandbreedte of beveiliging dan een Azure-netwerk verbinding kan bieden, kunt u zoeken naar ExpressRoute. In sommige gevallen kunt ExpressRoute ook u geld besparen. U moet nog steeds een virtueel netwerk in Azure, maar de koppeling tussen Azure en uw site gebruikt een speciale verbinding die u niet via het openbare Internet gaat. Om deze service gebruikt, moet u een overeenkomst met een netwerkserviceprovider of een exchange-provider hebt.

Het instellen van een ExpressRoute verbinding meer tijd vereist plannen, zodat u wilt starten met een site-naar-site-VPN en migreren naar een ExpressRoute-verbinding.

Zie voor meer informatie over ExpressRoute [technisch overzicht van ExpressRoute](expressroute/expressroute-introduction.md).

### <a name="traffic-manager"></a>Traffic Manager
![TrafficManager](./media/fundamentals-introduction-to-azure/TrafficManagerIntroNew.png)   

*Afbeelding: Azure Traffic Manager kunt u algemene verkeer naar uw service op basis van regels intelligent sturen.*

Als uw Azure-toepassing wordt uitgevoerd in meerdere datacenters, kunt u Azure Traffic Manager gebruiken voor het aanvragen van gebruikers op intelligente wijze te routeren over exemplaren van de toepassing. Ook kunt u verkeer routeren voor services worden niet uitgevoerd in Azure, zolang ze toegankelijk vanaf internet zijn.  

Een Azure-toepassing met gebruikers in slechts één deel van de wereld in slechts één Azure-datacenter mogelijk uitgevoerd. Een toepassing met gebruikers verspreid over de hele wereld, is echter vaker uitvoeren in meerdere datacenters mogelijk zelfs alle. In dit tweede geval kunt u een probleem wordt geconfronteerd: hoe u op intelligente wijze gebruikers verwijzen naar exemplaren van een toepassing? De meeste gevallen, wilt u waarschijnlijk elke gebruiker toegang heeft tot het dichtst bij hen datacenter omdat het krijgen ze de beste reactietijd. Maar wat gebeurt er als dat exemplaar van de toepassing is overbelast of niet beschikbaar? In dit geval zou het niet handig hun aanvraag automatisch naar een ander datacenter. Dit is precies wat door Azure Traffic Manager wordt uitgevoerd.

De eigenaar van een toepassing definieert de regels die specificeren hoe aanvragen van gebruikers moeten worden omgeleid naar datacentra en is gebaseerd op Traffic Manager bij het uitvoeren van deze regels. Bijvoorbeeld gebruikers normaal kunnen worden omgeleid naar het dichtstbijzijnde Azure-datacentrum, maar naar een andere wordt verzonden wanneer de reactietijd van hun datacenter standaard de reactietijd van andere datacentra overschrijdt. Voor globaal gedistribueerde toepassingen met veel gebruikers is hebben een ingebouwde service voor het afhandelen van problemen zoals dit handig.

Het Traffic manager Directory Name Service (DNS) gebruikt voor gebruikers van de route voor service-eindpunten, maar meer verkeer gaat niet via het Traffic Manager wanneer deze verbinding wordt gemaakt. Zo voorkomt Traffic Manager wordt een knelpunt dat uw service-communicatie mogelijk vertragen.

## <a name="developer-services"></a>Ontwikkelaarsservices
Azure biedt een aantal hulpprogramma's waarmee ontwikkelaars en IT-Professionals maken en onderhouden van toepassingen in de cloud.  

### <a name="azure-sdk"></a>Azure SDK
Terug in 2008 ondersteund de allereerste voorlopige versie van Azure .NET-ontwikkeling. Vandaag de dag, kunt u Azure-toepassingen in vrijwel elke taal. Microsoft biedt momenteel taalspecifieke SDK voor .NET, Java, PHP, Node.js, Ruby en Python. Er is een algemene Azure-SDK die basic ondersteuning voor een andere taal, zoals C++ biedt.  

Deze SDK's te maken, implementeren en beheren van Azure-toepassingen. Ze zijn beschikbaar ofwel van [www.microsoftazure.com](https://azure.microsoft.com/downloads/) of GitHub, en ze kunnen worden gebruikt met Visual Studio en Eclipse. Azure biedt ook opdrachtregelprogramma's waarmee ontwikkelaars kunnen met een editor of ontwikkelings-omgeving, waaronder hulpprogramma's voor het implementeren van toepassingen naar Azure van Linux- en Macintosh-systemen.

Samen met bijdragen aan Azure-toepassingen te ontwikkelen, bieden deze SDK's ook het clientbibliotheken waarmee u software maken die gebruikmaakt van Azure-services. U kan bijvoorbeeld een toepassing bouwt die leest en schrijft Azure blobs of maken van een hulpprogramma dat Azure-toepassingen via de Azure-interface implementeert.

### <a name="visual-studio-team-services"></a>Visual Studio Team Services
Visual Studio Team Services is een marketing naam die betrekking hebben op een aantal services die helpen bij het ontwikkelen van toepassingen in Azure.

Voorkom verwarring - biedt geen een gehoste of Web gebaseerde versie van Visual Studio. U moet nog steeds op uw lokale kopie van de Visual Studio die wordt uitgevoerd. Maar het biedt veel andere hulpprogramma's die handig kunnen zijn.

Dit omvat een gehoste bronbeheersysteem aangeroepen Team Foundation-Service versiebeheer en work item bijhouden biedt.  U kunt zelfs Git voor versiebeheer gebruiken als u liever die. En u kunt het bronbeheersysteem die u door project gebruikt variëren. U kunt maken onbeperkte persoonlijke teamprojecten toegankelijk is vanaf overal ter wereld.  

Visual Studio Team Services biedt een service voor het testen van belasting. U kunt de load-tests die zijn gemaakt in Visual Studio op virtuele machines in de cloud uitvoeren. U geeft het totale aantal gebruikers die u wilt testen met en Visual Studio Team Services automatisch laden bepaalt hoeveel agents nodig zijn, ronddraaien van de vereiste virtuele machines en voer uw tests load. Als u een MSDN-abonnee bent, krijgt u duizenden gebruikersminuten belast testen van elke maand gratis.

Visual Studio Team Services biedt ook ondersteuning voor het flexibel ontwikkelen met functies zoals continue integratie bouwt, kanbanborden en virtueel team ruimten.

**Visual Studio Team Services-scenario 's**

Visual Studio Team Services is een goede optie voor bedrijven die moeten samenwerken overal ter wereld en niet de infrastructuur hebben opgesteld om dit te doen. U kunt setup ophalen in minuten, kiest u een bronbeheersysteem en beginnen met het schrijven van code en bouwen van die dag.  De team hulpprogramma's bieden een plaats voor coördinatie en samenwerking en de aanvullende hulpprogramma's bieden de analyse nodig om te testen en snel afstemmen van uw toepassing.

Maar organisaties die al een on-premises systeem nieuwe projecten in Visual Studio Team Services om te zien als het is efficiënter kunnen testen.   

### <a name="application-insights"></a>Application Insights
![Application Insights](./media/fundamentals-introduction-to-azure/ApplicationInsights.png)  

*Afbeelding: Application Insights monitors prestaties en het gebruik van uw live web- of app.*

Wanneer u uw app - hebt gepubliceerd of deze wordt uitgevoerd op mobiele apparaten, bureaubladen of webbrowsers - Application Insights geeft aan hoe deze wordt uitgevoerd en wat gebruikers doen met het. Een aantal crashes en trage reactie blijven, waarschuwing of de afbeeldingen cross onaanvaardbaar drempelwaarden en kunnen u eventuele problemen wilt onderzoeken.

Wanneer u een nieuwe functie ontwikkelt, wilt u het succes meten met gebruikers. Door het analyseren van gebruikspatronen begrijpen wat het beste werkt voor uw klanten en uw app in elke ontwikkelingscyclus verbeteren.

Hoewel deze wordt gehost in Azure, werkt de Application Insights voor een breed en groeien scala aan apps, zowel in en uit Azure. J2EE- en ASP.NET web apps worden beschreven, evenals iOS, Android, OS x en Windows-toepassingen. Telemetrie is verzonden via een SDK al ingebouwd met de app worden geanalyseerd en weergegeven in de Application Insights-service in Azure.

Als u meer gespecialiseerde analytics wilt, de telemetriestroom met een database of exporteren naar Power BI of andere hulpprogramma's.

**Application Insights-scenario 's**

U ontwikkelt een app. Het is mogelijk een web-app of een apparaat-app of een apparaat-app met een web-back-end.

* De prestaties van uw app optimaliseren nadat deze is gepubliceerd of terwijl het laden testen.  Application Insights cumuleert de telemetrie van de geïnstalleerde exemplaren en biedt u de grafieken van reactietijden, aanvraag en aantal uitzonderingen is afhankelijkheid reactietijden en andere prestatie-indicatoren. Dit kunnen u uw app-prestaties afstemmen. U kunt de code voor het rapporteren van meer invoegen specifieke gegevens als u deze nodig hebt.
* Detecteren en onderzoeken van problemen in uw app live. Ontvang waarschuwingen per e-mail als prestatie-indicatoren cross acceptabele drempelwaarden. Specifieke gebruikerssessies, bijvoorbeeld om te zien van de aanvraag die een uitzondering veroorzaakt, kunt u onderzoeken.
* Gebruik voor het evalueren van het succes van elke nieuwe functie bijhouden. Wanneer u een nieuwe gebruiker-artikel ontwerpt, moet u wilt meten hoeveel wordt gebruikt en of gebruikers hun verwachte doelstellingen te realiseren. Application Insights biedt u algemene gebruiksgegevens zoals Webpaginaweergaven en kunt u code voor het bijhouden van de gebruikerservaring in meer detail invoegen.

### <a name="automation"></a>Automatisering
Niemand graag verspillen tijd steeds dezelfde handmatige processen uitvoeren. Azure Automation biedt een manier om u te maken, bewaken, beheren en implementeren van resources in uw Azure-omgeving.  

Automation maakt gebruik van 'runbooks', dat gebruikmaakt van Windows PowerShell-werkstromen (versus NET reguliere PowerShell) onder de behandelt. Runbooks zijn bedoeld om te worden uitgevoerd zonder dat tussenkomst van de gebruiker. PowerShell-werkstromen kunnen de status van een script dat momenteel controlepunten langs de manier worden opgeslagen. Als er een fout optreedt, hoeft u een script te starten vanaf het begin. U kunt deze opnieuw starten op het laatste controlepunt. Dit bespaart u veel werk wilt maken van het script verwerkt elke mislukken.

**Automatiseringsscenario 's**

Azure Automation is een goede keuze voor het automatiseren van de handmatige, langlopende, foutgevoelige en regelmatig herhaalde taken in Azure.

### <a name="api-management"></a>API Management
Is een veelgebruikte manier voor het leveren van services aan toepassingen maken en publiceren van Application programmeurs Interfaces (API's) op het internet. Als deze services zijn resellable (bijvoorbeeld weergegevens), kan een organisatie andere partijen voor toegang tot deze services dezelfde voor een vast bedrag toestaan. Terwijl u naar meer partners schaalt, moet u doorgaans te optimaliseren en toegangsbeheer.  Sommige partners moet mogelijk ook de gegevens in een andere indeling.

Azure API Management eenvoudig organisaties API's publiceren naar partners, werknemers en ontwikkelaars van derden veilig en op schaal. Het biedt een andere API-eindpunt en fungeert als proxy echt eindpunt niet aanroepen terwijl er services zoals caching, transformatie, beperking, toegangsbeheer en analytics aggregatie.

**API Management-scenario 's**

Stel dat uw bedrijf beschikt over een reeks apparaten dat alle moet teruggebeld naar een centrale service om op te halen gegevens--bijvoorbeeld een back-ups van bedrijf dat apparaten in elke vrachtwagen onderweg is.  Het bedrijf wil zeker voor het instellen van een systeem voor het bijhouden van eigen vrachtwagens zodat betrouwbaar kunnen voorspellen en leveringstijden bijwerken. Het kan weet hoeveel vrachtwagens heeft en op de juiste wijze plannen.  Elke vrachtwagen moet een apparaat dat terug naar een centrale locatie met de plaatsing en snelheid gegevens en mogelijk meer belt.

Een klant van het bedrijf back-ups wilt waarschijnlijk ook profiteren van deze plaatsing gegevens ophalen.  De klant kunt gebruiken om te weten hoe ver producten kunnen reizen, waar ze hangen, hoeveel betaalt langs bepaalde routes (indien dit wordt gecombineerd met ze betaald verzenden) ze hebben. Als het bedrijf back-upfunctie al deze gegevens aggregeert, mogelijk veel klanten betalen voor.  Maar vervolgens back-ups moet het bedrijf bieden een manier om klanten de gegevens. Nadat ze toegang aan klanten bieden, ze mogelijk geen controle over hoe vaak de gegevens wordt opgevraagd. Ze hebben regels die bepalen wie toegang heeft tot welke gegevens bevatten. Deze regels zou moeten zijn ingebouwd in hun externe API. Dit is waar API Management kan helpen.  

## <a name="identity-and-access"></a>Identiteit en toegang
Werken met de identiteit is onderdeel van de meeste toepassingen. Weten wie een gebruiker is, kunt een toepassing die bepalen hoe het met die gebruiker moet werken. Azure biedt services voor het bijhouden van identiteit ook integreren met identiteitsopslag die kunt u al gebruikt.

### <a name="active-directory"></a>Active Directory
Zoals de meeste directoryservices, Azure Active Directory informatie opgeslagen over gebruikers en de organisaties waartoe ze behoren. Hiermee kunnen gebruikers zich kunnen aanmelden en vervolgens verstrekt ze met tokens die ze tot toepassingen om hun identiteit te bewijzen kunnen opleveren. Daarnaast kunt u gebruikersgegevens synchroniseren met Windows Server Active Directory met on-premises in uw lokale netwerk. Terwijl de mechanismen en gegevensopmaak die wordt gebruikt door Azure Active Directory niet identiek zijn met die worden gebruikt in Windows Server Active Directory, zijn de functies die worden uitgevoerd lijkt veel.

Het is belangrijk te weten dat Azure Active Directory voornamelijk voor gebruik door cloudtoepassingen ontworpen is. Het kan worden gebruikt door toepassingen die worden uitgevoerd op Azure, bijvoorbeeld of op andere cloudplatforms. Dit wordt ook gebruikt door de Microsoft cloud-toepassingen, zoals die in Office 365. Als u uw datacenter uitbreiden naar de cloud met Azure Virtual Machines en virtuele netwerken van Azure wilt, echter Azure Active Directory is niet de juiste keuze. In plaats daarvan moet u Windows Server Active Directory uitgevoerd op virtuele Machines.

Azure Active Directory biedt zodat toepassingen toegang tot de informatie die deze bevat een RESTful-API aangeroepen Azure Active Directory Graph. Deze API kan toepassingen die op elk platform toegang directory-objecten en de relaties tussen deze uitgevoerd.  Een geautoriseerde toepassing kan bijvoorbeeld deze API gebruiken voor meer informatie over een gebruiker, de groepen die hij tot deel uitmaakt en andere informatie. Toepassingen relaties tussen gebruikers hun sociale graph-waarin ze meer op intelligente wijze werken met de verbindingen tussen mensen ook worden weergegeven.

Een andere functionaliteit van deze service, Azure Active Directory-toegangsbeheer gemakkelijker voor een toepassing te accepteren van identiteitsgegevens van Facebook, Google, Windows Live ID en andere populaire id-providers. In plaats van de toepassing om te begrijpen van de van diverse gegevensindelingen en protocollen die worden gebruikt door elk van deze providers vereist, zet toegangsbeheer allemaal in één algemene indeling. Ook kunt u een toepassing accepteren aanmeldingen vanaf een of meer Active Directory-domeinen. Een leverancier bieden een SaaS-toepassing kan bijvoorbeeld Azure Active Directory-toegangsbeheer gebruiken om gebruikers in elk van de klanten eenmalige aanmelding tot de toepassing.

Directory-services worden een core basis van de lokale computer. Mag niet het verrassend dat ze zich ook belangrijk in de cloud.

### <a name="multi-factor-authentication"></a>Meervoudige verificatie
![Azure Multi-Factor Authentication](./media/fundamentals-introduction-to-azure/MFAIntroNew.png)   

*Afbeelding: Multi-Factor Authentication biedt de functionaliteit voor uw toepassing om te controleren of meer dan één vorm van identificatie*

Beveiliging is altijd belangrijk. Multi-factor authentication (MFA) kunt u ervoor zorgen dat alleen gebruikers zelf toegang hebben tot hun account. MFA (ook wel bekend als tweeledige verificatie of '2FA') moet gebruikers twee van deze drie methoden voor verificatie van de identiteit voor gebruikersaanmeldingen en transacties.

* Iets u weet (doorgaans een wachtwoord)
* Iets er (een vertrouwd apparaat die niet eenvoudig wordt gedupliceerd, zoals een telefoon)
* Iets dat u (biometrie)

Dus wanneer een gebruiker zich aanmeldt, kunt u vereisen ze ook hun identiteit verifiëren met een mobiele app, een telefonische oproep of een tekstbericht in combinatie met hun wachtwoord. Azure Active Directory ondersteunt standaard, het gebruik van wachtwoorden als verificatiemethode voor gebruikersaanmelding. Met behulp van de MFA-SDK kunt u MFA samen met Azure AD of met aangepaste toepassingen en mappen. U kunt deze ook gebruiken met on-premises toepassingen met behulp van multi-factor Authentication-Server.

**MFA-scenario 's**

De beveiliging van de aanmelding op gevoelige accounts zoals bank aanmeldingen en code toegang tot de gegevensbron waarop onbevoegde toegang kan er met een hoge financiële of intellectueel-eigenschap kosten.   

## <a name="mobile"></a>Mobiele telefoon
Als u een app voor een mobiel apparaat maakt, is Azure kunt opslaan van gegevens in de cloud, gebruikers verifiëren en om pushmeldingen te verzenden zonder dat u hoeft een grote hoeveelheid aangepaste code schrijven.

Terwijl u gewoon de back-end voor een mobiele app met virtuele Machines, Cloud Services of Web-Apps bouwen kan, kunt u besteden veel minder tijd de onderliggende serviceonderdelen schrijven met behulp van Azure services.

### <a name="mobile-apps"></a>Mobile Apps
![Mobile Apps](./media/fundamentals-introduction-to-azure/MobileServicesIntroNew.png)

*Afbeelding: Mobiele Apps biedt functionaliteit die zijn doorgaans vereist voor toepassingen die de interface met mobiele apparaten.*

Mobiele Apps van Azure biedt veel handige functies waarmee u bespaart tijd bij het bouwen van een back-end van een mobiele toepassing. Hiermee kunt u doen eenvoudige inrichting en beheer van gegevens die zijn opgeslagen in een SQL-Database. U kunt opties voor het opslaan van aanvullende gegevens zoals blob-opslag of MongoDB gebruiken met servercode. Mobiele Apps biedt ondersteuning voor meldingen, hoewel in bepaalde gevallen u in plaats daarvan Notification Hubs gebruiken kunt zoals hieronder wordt beschreven.  De service heeft ook een REST-API die uw mobiele App aanroepen kunt voor werk. Mobiele Apps biedt ook de mogelijkheid om te verifiëren van gebruikers via Microsoft- en Active Directory en andere bekende id-providers zoals Facebook, Twitter en Google.   

U kunt andere Azure-Services zoals Service Bus- en werkrollen gebruiken en verbinding maken met on-premises systemen. U kunt zelfs 3e partij invoegtoepassingen van de Azure Store (zoals SendGrid voor e-mailadres) verbruiken om aanvullende functionaliteit te bieden.

Native clientbibliotheken voor Android-, iOS-, HTML/JavaScript-, Windows Phone- en Windows Store makkelijker te ontwikkelen voor apps op alle veelgebruikte mobiele platforms. Een REST-API kunt u met Mobile Services-gegevens en verificatie functionaliteit met apps op verschillende platforms. Een mobiele service kunt back-meerdere client-apps zodat u een consistente gebruikerservaring verschillende apparaten bieden kunt.

Omdat Azure al grootschalige ondersteunt, kunt u het verkeer verwerken als uw app steeds populairder wordt.  Controle en logboekregistratie worden ondersteund voor het oplossen van problemen en prestaties beheren.

### <a name="notification-hubs"></a>Notification Hubs
![NotificationHubs](./media/fundamentals-introduction-to-azure/NotificationHubsIntroNew.png)  

*Afbeelding: Notification Hubs bevat functies die nodig zijn vaak door toepassingen die de interface met mobiele apparaten.*

Terwijl u de code hiervoor meldingen in Azure Mobile Apps schrijven kunt, wordt Notification Hubs geoptimaliseerd voor uitzending miljoenen sterk gepersonaliseerde pushmeldingen binnen enkele minuten.  U hoeft niet te hoeven maken over de gegevens, zoals mobiele operator of de fabrikant van apparaat. U kunt richt de persoon die of het miljoenen gebruikers met één API-aanroep.

Notification Hubs is ontworpen voor gebruik met elke back-end. U kunt Azure Mobile Apps, een aangepaste back-end in de cloud die wordt uitgevoerd op elke provider of een lokale back-end.

**Notification Hub-scenario's** als u was bezig met het schrijven van een mobiele spel waar spelers schakelt heeft, moet u mogelijk speler 2 waarschuwen dat speler 1 haar inschakelen is voltooid. Als dit is alles wat die u moet doen, kunt u alleen mobiele Apps gebruiken. Maar als u had 100.000 gebruikers spelen uw en u wilt verzenden van een tijdstip gevoelige gratis aanbieding wilt maken voor iedereen met Notification Hubs een betere keuze is.

U kunt belangrijk nieuws, sport gebeurtenissen en meldingen van de product-aankondiging naar miljoenen gebruikers met een lage latentie verzenden. Ondernemingen kunnen melden bij hun werknemers over nieuwe tijd gevoelige communicatie, zoals verkoopkansen, zodat werknemers niet voortdurend controleren van e-mailadres of andere toepassingen op de hoogte blijven. U kunt ook een eenmalige wachtwoorden vereist voor multi-factor authentication verzenden.

## <a name="back-up"></a>Back-ups maken
Elke enterprise moet back-up en herstellen van gegevens. U kunt Azure back-up en herstellen van uw toepassing in de cloud of on-premises gebruiken. Azure biedt verschillende opties om u te helpen afhankelijk van het type back-up.

### <a name="site-recovery"></a>Site Recovery
Azure Site Recovery (voorheen Hyper-V Recovery Manager) kunt u belangrijke toepassingen beveiligen door een coördinatie van de replicatie en herstel tussen sites. Site Recovery biedt de mogelijkheid toepassingen op basis van de Hyper-v, VMWare of SAN naar uw eigen secundaire site, een hoster site of Azure beveiligen en te voorkomen dat de kosten en complexiteit van het maken en beheren van uw eigen secundaire locatie. Azure versleutelt gegevens en communicatie en u hebt de optie versleuteling voor gegevens in rust te schakelen.

Het continu bewaakt de status van uw services en helpt om het normaal herstel van services in het geval van een onderbreking van de site op het primaire datacenter te automatiseren. Virtuele machines kunnen op een gecoördineerde manier worden geactiveerd om de dienstverlening snel te herstellen, zelfs binnen complexe werkvolumes met meerdere lagen.

Site Recovery werkt met bestaande technologieën zoals Hyper-V Replica, System Center en SQL Server Always On. Bekijk [Azure Site Recovery-overzicht](site-recovery/site-recovery-overview.md) voor meer informatie.

### <a name="azure-backup"></a>Azure Backup
![Azure Backup](./media/fundamentals-introduction-to-azure/AzureBackupIntroNew.png)  

*Afbeelding: Azure back-up waarin gegevens van lokale Windows-Servers in de cloud.*  

Azure Backup back-ups van gegevens van lokale servers met Windows Server naar de cloud. U kunt uw back-ups rechtstreeks vanuit de back-uphulpprogramma's in Windows Server 2012, Windows Server 2012 Essentials of System Center 2012 - Data Protection Manager beheren. U kunt ook een gespecialiseerde back-upagent gebruiken.

Gegevens is veiliger omdat back-ups worden versleuteld voor verzending en versleuteld in Azure opgeslagen en beveiligd door een certificaat dat u uploadt. De service gebruikt dezelfde redundante en maximaal beschikbare gegevensbeveiliging gevonden in Azure Storage.  U kunt back-up bestanden en mappen op een regelmatige of direct volledige of incrementele back-ups uitgevoerd. Nadat de back-up naar de cloud, kunnen geautoriseerde gebruikers eenvoudig back-ups op elke server herstellen. Biedt ook bewaarbeleid voor gegevens kunnen worden geconfigureerd, compressie van gegevens en gegevens overbrengen beperking zodat u de kosten voor het opslaan en gegevens overbrengen kunt beheren.

**Scenario's voor Azure Backup**

Als u al Windows Server of System Center gebruikt, is Azure backup een natuurlijke oplossing voor back-ups van uw bestandssysteem voor servers, virtuele machines en SQL Server-databases.  Dit proces werkt met versleutelde, sparse en gecomprimeerde bestanden. Er zijn enkele beperkingen, moet u [Controleer de vereisten voor Azure Backup](http://technet.microsoft.com/library/dn296608.aspx) eerste.

## <a name="messaging-and-integration"></a>Berichten en integratie
Ongeacht wat het doet, moet de code vaak communiceren met andere code.  In sommige gevallen is alles wat nodig is in de wachtrij basisberichten. In andere gevallen zijn meer complexe interacties vereist. Azure biedt een aantal verschillende manieren voor het oplossen van deze problemen. Afbeelding 5 ziet u de opties.

### <a name="queues"></a>Wachtrijen
![Azure Service Bus Relay](./media/fundamentals-introduction-to-azure/QueuesIntroNew.png)

*Afbeelding: Wachtrijen toestaan of koppeling tussen de onderdelen van een toepassing en vergemakkelijken schalen.*  

Queuing is een eenvoudige idee: bericht uiteindelijk wordt gelezen door een andere toepassing en één toepassing een bericht in een wachtrij geplaatst. Als uw toepassing alleen deze eenvoudige service moet, zijn wachtrijen Azure de beste keuze.

Bieden vergelijkbare Queuing-services vanwege de manier waarop die de Azure klein gedurende een bepaalde periode, Azure Storage-wachtrijen en Service Bus-wachtrijen. De redenen waarom u zou een wilt gebruiken dan de andere worden behandeld in het redelijk technische document [Azure wachtrijen en Service Bus-wachtrijen - vergeleken en tegenstelling tot](http://msdn.microsoft.com/library/azure/hh767287.aspx).  In veel scenario's, ofwel werkt.

**Wachtrij-scenario 's**

Wachtrijen worden vaak gebruikt is vandaag de dag dat een web-rolinstantie communiceren met een worker-rolinstantie in dezelfde Cloud Services-toepassing.

Stel bijvoorbeeld dat u maakt een Azure-toepassing voor het delen van video. De toepassing bestaat uit PHP code die wordt uitgevoerd in een Webrol waarmee gebruikers uploaden en bekijk video's, samen met een werkrol die is geïmplementeerd in C# die geüploade video in verschillende indelingen zet.

Wanneer een exemplaar van de rol web ontvangt een nieuwe video van een gebruiker, kan het opslaan van de video in een blob en vervolgens een bericht verzenden naar een werkrol via een wachtrij geeft u op waar vind ik deze nieuwe video. Een exemplaar worker-rol-en niet terzake welke één zal en vervolgens het bericht uit de wachtrij lezen en uitvoeren van de vereiste video vertalingen op de achtergrond.

Asynchrone verwerking structureren van een toepassing op deze manier kunt en ook kan de toepassing gemakkelijker te schalen, omdat het aantal rolinstanties web en rolinstanties worker onafhankelijk kan worden gewijzigd. U kunt ook de grootte van de wachtrij gebruiken als een trigger het aantal werkrollen omhoog en omlaag schalen. Te hoog is, en het toevoegen van meer functies. Bij het ophalen van lagere, kunt u het aantal rollen geld besparen beperken.  

Zelfs als ze geen web-en werkrollen, kunt u dit patroon tussen veel verschillende onderdelen van uw toepassing.  Hiermee kunt u de onderdelen aan beide zijden van de wachtrij omhoog en omlaag als vraag schalen en verwerkingstijd vereist.

### <a name="service-bus"></a>Service Bus
Of ze worden uitgevoerd in de cloud, in uw datacenter, op een mobiel apparaat of ergens anders, moeten toepassingen communiceren. Het doel van Azure Service Bus is te laten servertoepassingen vrijwel overal uitwisselen van gegevens.

Naast de wachtrijen (-op-een) die eerder zijn beschreven, biedt Service Bus ook op andere wijze communicatie.

#### <a name="service-bus-relay"></a>Service Bus Relay
![Azure Service Bus Relay](./media/fundamentals-introduction-to-azure/ServiceBusRelayIntroNew.png)

*Afbeelding: Service Bus Relay kunt communicatie tussen toepassingen op verschillende kanten van een firewall.*

Service Bus kunt rechtstreekse communicatie via de relay-service biedt een veilige manier om te communiceren via firewalls. Service Bus relays kunnen toepassingen om te communiceren met het uitwisselen van berichten via een gehost in de cloud in plaats van lokaal eindpunt.

**Service Bus Relay-scenario 's**

Toepassingen die via Service Bus communiceren mogelijk op Azure-toepassingen of software die wordt uitgevoerd op een andere cloud-platform. Ze kunnen ook toepassingen die worden uitgevoerd buiten de cloud, maar worden. Een luchtvaartmaatschappij dat reserveringsinformatie op computers in een eigen datacenter implementeert bijvoorbeeld zien. De luchtvaartmaatschappij moet deze services te veel clients, waaronder incheckbalies op luchthavens, reservering agent aansluitingen, beschikbaar stellen en mogelijk zelfs klanten telefoons. Deze mogelijk gebruik van Service Bus hiervoor losse interacties tussen de verschillende toepassingen maken.

#### <a name="service-bus-topics-and-subscriptions"></a>Service Bus-onderwerpen en -abonnementen
![Azure Service Bus-onderwerpen](./media/fundamentals-introduction-to-azure/ServiceBusTopicsSubsIntroNew.png)   
 *Afbeelding: Service Bus-onderwerpen kunnen meerdere apps berichten en andere toepassingen voor een abonnement voor het ontvangen van berichten die aan een bepaald criterium voldoen.*

Service Bus biedt een mechanisme voor publiceren en abonneren aangeroepen onderwerpen en abonnementen. Met publish-subscribe, kunt een toepassing berichten verzenden naar een onderwerp, terwijl andere toepassingen abonnementen naar dit onderwerp kunnen maken. Hierdoor kan een-op-veel-communicatie tussen een set met toepassingen, waardoor hetzelfde bericht worden gelezen door meerdere ontvangers.

**Service Bus-onderwerpen en abonnementen scenario 's**

Op elk gewenst moment u instelt waarbij er zijn veel berichten die alle belangrijke zijn, maar verschillende downstream systemen hoeft alleen te luisteren naar verschillende subsets van deze communicatie, Service Bus-onderwerp en abonnementen zijn een goede optie.

### <a name="biztalk-services"></a>BizTalk Services
![BizTalk Services](./media/fundamentals-introduction-to-azure/BizTalkServicesIntroNew.png)   
 *Afbeelding: BizTalk Services biedt de mogelijkheid voor het transformeren van XML-berichten indelingen in de cloud.*

Soms moet u verbinding maken met systemen die communiceren met behulp van verschillende-indelingen. Het is gebruikelijk voor bedrijven hebben andere database schema's en XML-messaging-indelingen, zelfs als een algemene standaard beschikbaar. In plaats van een groot aantal aangepaste code schrijven, kunt u BizTalk Server on-premises, voor het integreren van verschillende systemen.  Azure BizTalk Services geeft hetzelfde type van de service, maar in de cloud. U kunt alleen wat u gebruikt en u geen zorgen over schalen zoals u tot on-premises hebben zou betalen.

**BizTalk Services-scenario 's**

Dit type vertaling voor bedrijven vereisen meestal interacties Business-to-Business (B2B).  Bijvoorbeeld, een bedrijf bouwen vliegtuigen moet volgorde-onderdelen van de verschillende onderdelen leveranciers. Er is veel leveranciers van onderdelen.  Deze orders moeten worden geautomatiseerd om rechtstreeks vanuit de vliegtuig opbouwfuncties systemen van de leveranciers-systemen.  Geen van beide bedrijven wil wijzigen van hun core systemen en berichtindelingen en is zeer onwaarschijnlijk dat deze indelingen hetzelfde zijn. BizTalk Services kan duren voordat de berichten en tussen de nieuwe indelingen beide manieren vertalen. Het werk te vertalen kunt doen door de leverancier van het vliegtuig of de verschillende leveranciers kunnen, afhankelijk van die wil meer controle en de hoeveelheid vertaling nodig.     

## <a name="compute-assistance"></a>Rekenuren voor hulp
Azure biedt ondersteuning voor services die niet hoeven te worden uitgevoerd.  

### <a name="scheduler"></a>Scheduler
![Azure Scheduler](./media/fundamentals-introduction-to-azure/SchedulerIntroNew.png)   
*Afbeelding: Azure Scheduler biedt een manier om taken te plannen op een bepaald tijdstip gedurende een bepaalde periode.*

Soms moeten toepassingen alleen uitvoeren op een bepaald moment. In Azure, kunt u geld met dit type app in plaats van een toepassing alleen 24 x 7 wachten op gegevens verwerken blijven uitvoeren laten opslaan. Azure Scheduler kunt u plannen wanneer een toepassing moet worden uitgevoerd op basis van een interval van tijd of een kalender. Het is betrouwbare en controleert of dat een proces wordt uitgevoerd zelfs als er netwerk, machine en data center-fouten. De REST-API van Scheduler kunt u deze acties beheren.

Wanneer een geplande waarschuwing wordt Scheduler HTTP of HTTPS-berichten verzendt naar een specifieke eindpunt of een bericht in een wachtrij opslag kunt plaatsen.  Daarom moet u uw toepassing op een toegankelijk eindpunt hebt of laat het bewaken van een opslagwachtrij hebt. Zodra het bericht krijgt, kunt het ongeacht deze geprogrammeerd actie uitvoeren.

**Scheduler-scenario 's**

* Terugkerende acties van toepassingen: als u bijvoorbeeld een service mogelijk periodiek gegevens ophalen uit twitter en verzamelen van de gegevens in een reguliere feed.
* Dagelijks onderhoud: logboek verwerken of weghalen, presterende back-ups en andere afwisselend taken plannen.
* Taken die 's nachts uitvoeren.
* Web applications taken zoals het dagelijks verwijderen van Logboeken, het uitvoeren van back-ups en overige onderhoudstaken. Een beheerder kiezen voor een back-up haar database om 1 uur, dagelijks voor de volgende negen maanden, bijvoorbeeld.

De Scheduler-API kunt u maken, bijwerken, verwijderen, weergeven en jobverzamelingen en geplande taken beheren via een programma.

## <a name="performance"></a>Prestaties
Prestaties is altijd belangrijk voor een toepassing. Toepassingen vaak steeds toegang krijgen tot dezelfde gegevens. Een manier om prestaties te verbeteren is om een kopie van die gegevens dichter tot de toepassing voor het minimaliseren van de tijd die nodig is voor het ophalen van het te beperken. Azure biedt verschillende services om dit te doen.

### <a name="azure-caching"></a>Azure Caching
![Azure Caching](./media/fundamentals-introduction-to-azure/AzureCacheIntroNew.png)   
 **Afbeelding: Een Azure-toepassing kunt gegevens in het geheugen in de cache en zelfs van splitsen over veel werkrollen**

Toegang tot gegevens die zijn opgeslagen in een Azure gegevens management services-SQL-Database, tabellen of BLOB's-erg snel. Nog toegang tot gegevens die zijn opgeslagen in het geheugen is zelfs sneller. Bewaren van een kopie in het geheugen van veelgebruikte gegevens verbeteren de prestaties van toepassingen als gevolg hiervan. U kunt Azure in-memory Caching gebruiken om dit te doen.

Een Cloud Services-toepassing kunt gegevens in de cache opslaan en vervolgens rechtstreeks zonder toegang tot permanente opslag worden opgehaald. De cache kan worden beheerd in virtuele machines van uw toepassing of door virtuele machines die specifiek is voor het opslaan in cache worden opgegeven. In beide gevallen moet de cache kan worden verdeeld, met de gegevens erin verspreid over meerdere virtuele machines in een Azure-datacenter.

Azure heeft een aantal verschillende cache-technologieën die gedurende een bepaalde periode hebt verplaatst. In de volgorde waarin ze zijn geïntroduceerd, er is een gedeelde in de rol, beheerd en Redis-cache. Het gedeelde opslaan in cache is een oudere technologie en u nieuwe implementaties aan mag niet maken. De Cache beheerd moet dezelfde functies van de In-Role cache, maar als beheerde service buiten de Azure-portal. De Redis-implementatie is het grootste aantal onderdelen en wordt aanbevolen als u nieuwe cache code schrijft.

**Azure-Cache-scenario 's**

Een toepassing die herhaaldelijk een productcatalogus leest kan profiteren van het gebruik van dit soort caching, bijvoorbeeld omdat de gegevens het moet beschikbaar sneller. De technologie ondersteunt ook vergrendelen, zodat deze worden gebruikt met zowel lezen/schrijven als alleen-lezen gegevens. En ASP.NET-toepassingen kunnen de service gebruiken voor het opslaan van sessiegegevens met alleen een configuratiewijziging.

### <a name="content-delivery-network"></a>CDN (Content Delivery Network)
![Azure CDN](./media/fundamentals-introduction-to-azure/CDNIntroNew.png)   
 **Afbeelding: Kopieën van een blob kunnen worden opgeslagen in de cache op sites overal ter wereld.**

Stel dat u nodig hebt voor het opslaan van blob-gegevens die door gebruikers over de hele wereld wordt geopend. Het is mogelijk een video van de meest recente WK overeenkomen, bijvoorbeeld of updates voor stuurprogramma's, of een populaire e-book. Helpt bij het opslaan van een kopie van de gegevens in meerdere Azure-datacenters, maar als er veel gebruikers, het is mogelijk niet voldoende. U kunt Azure CDN gebruiken voor zelfs betere prestaties.

De CDN heeft tientallen sites overal ter wereld, elke geschikt is voor het opslaan van kopieën van Azure blobs. De eerste keer dat een gebruiker in een bepaald deel van de hele wereld naar een bepaalde blob wordt de inhoud ervan gekopieerd van een Azure-datacenter naar de lokale opslag van CDN in die Geografie. Hierna moeten toegang uit het deel van de hele wereld gebruikt het blob-exemplaar in de cache opgeslagen in de CDN-ze hoeft niet helemaal naar de dichtstbijzijnde Azure-datacenter. Het resultaat is sneller toegang tot veelgebruikte gegevens door gebruikers overal ter wereld.

**CDN-scenario 's**

Het is gebruikelijk om CDN met Media Services gebruiken om video over de hele wereld. Video is gewoonlijk lang en vereist een grote bandbreedte.  Media Services is elders besproken op deze pagina.

## <a name="big-data-and-big-compute"></a>BIG Data en Big Compute
### <a name="hdinsight-hadoop"></a>HDInsight (Hadoop)
![HDInsight](./media/fundamentals-introduction-to-azure/HDInsightIntroNew.png)   
 **Afbeelding: HDInsight helpt bij de bulkverwerking van grote hoeveelheden gegevens**

Jaren, is het grootste deel van de analyse van gegevens op relationele gegevens die zijn opgeslagen in een datawarehouse gebouwd met een relationele DBMS uitgevoerd. Dit soort business analytics is nog steeds belangrijk zal, en deze gedurende een lange periode afkomstig zijn. Maar wat gebeurt er als de gegevens die u wilt analyseren zo groot dat relationele databases NET kunnen niet; het verwerkt is gemaakt? En stel dat de gegevens niet relationele? Server worden vastgelegd in een datacenter, bijvoorbeeld of historische gebeurtenisgegevens van sensoren of iets anders kan zijn. In gevallen als volgt hebt u wel een big data-probleem. U moet een andere benadering.

De verwerkingsflexibiliteit vandaag voor analyse van big data-technologie is Hadoop. Een Apache open source-project, deze technologie slaat van gegevens met Hadoop Distributed File System (HDFS) en vervolgens kan ontwikkelaars MapReduce-taken voor het analyseren van gegevens maken. HDFS verspreid gegevens over meerdere servers, en vervolgens voert segmenten van de MapReduce-taak op elkaar, zodat de big data parallel worden verwerkt.

HDInsight is de naam van de Azure-service op basis van Apache Hadoop. HDInsight kunt HDFS gegevens worden opgeslagen op het cluster en het verdelen over meerdere virtuele machines. Deze ook verspreid de logica van een MapReduce-taak over de VM's. Net zoals met lokale Hadoop, gegevens verwerkte lokaal de logica en gegevens die werkt op in dezelfde virtuele machine zijn- en parallel voor betere prestaties. HDInsight kan ook opgeslagen in Azure Storage kluis (ASV), dat gebruikmaakt van blobs.  Met behulp van ASV, kunt u geld besparen omdat u kunt uw HDInsight-cluster als deze niet in gebruik verwijderen, maar nog steeds uw gegevens in de cloud blijven.

HDinsight biedt ondersteuning voor andere onderdelen van het Hadoop-ecosysteem, waaronder Hive en Pig. Microsoft heeft ook onderdelen waarmee het eenvoudiger om te werken met gegevens die door HDInsight worden geproduceerd met traditionele BI-hulpprogramma's, zoals de HiveODBC-adapter en Data Explorer die werken met Excel gemaakt.

### <a name="high-performance-computing-big-compute"></a>High-Performance Computing (Big Compute)
Een van de meest aantrekkelijk manieren om een cloudplatform is high performance computing (HPC) en andere toepassingen 'Big Compute'. Voorbeelden zijn gespecialiseerde engineering toepassingen die zijn ontworpen om te gebruiken voor de industriestandaard Message Passing Interface (MPI), evenals de zogenaamde perfect parallelle toepassingen, zoals de modellen van financiële risico's.

De essentie van Big Compute wordt code uitgevoerd op meerdere computers tegelijkertijd. In Azure machines dit betekent dat met veel virtuele tegelijk, alle werkt parallel sommige probleem op te lossen. Hiervoor moet een bepaalde manier tot bronnen en toepassingen, dat wil zeggen plannen, naar hun werk te verdelen over deze exemplaren. Het gratis HPC Pack van Microsoft en andere compute clusteroplossingen kunnen uitvoeren in Azure, profiteert van Azure services voor de berekenings- en infrastructuur capaciteit op aanvraag toevoegen aan een lokale compute cluster of voer Big Compute-toepassingen volledig in de cloud.

Azure biedt een bereik van de virtuele machine exemplaar grootten met verschillende configuraties van CPU-kernen, geheugen, schijfcapaciteit en andere kenmerken om te voldoen aan de vereisten van verschillende toepassingen. De onlangs geïntroduceerd A8 en A9 exemplaren werken goed bij veel compute intensieve werkbelastingen en parallelle MPI-toepassingen met name omdat ze hoge snelheid, multicore processors en grote hoeveelheden geheugen. In bepaalde configuraties profiteren de exemplaren van een netwerk met lage latentie en hoge gegevensdoorvoer toepassing in de cloud met remote direct memory access (RDMA)-technologie voor maximale efficiëntie van parallelle MPI-toepassingen.

Azure biedt ook Big Compute ontwikkelaars en partners een volledige set van compute-mogelijkheden, services, architectuurkeuzen en ontwikkelingsprogramma's. Azure ondersteunt aangepaste Big Compute-werkstromen met betrekking tot gespecialiseerde gegevens werkstromen en taak en planning patronen die kunnen worden geschaald naar duizenden kernen berekenen.

## <a name="media"></a>Media
![Azure Media Services](./media/fundamentals-introduction-to-azure/MediaServicesIntroNew.png)   
 **Afbeelding: Mediaservices is een platform voor toepassingen die video en andere media aan clients over de hele wereld leveren.**

Video maakt een groot deel van het internetverkeer vandaag de dag en dat percentage morgen nog groter worden. Nog waarbij video op de webserver is niet eenvoudig. Er zijn veel variabelen, zoals de codering algoritme en de schermresolutie van het scherm van de gebruiker. Video doorgaans ook bursts hebben in de vraag, zoals een piek zaterdag nacht wanneer veel mensen beslist dat ze graag willen een online film te bekijken.

Gezien de populariteit, is een kluis relevante treffers dat veel nieuwe toepassingen die video gebruik wordt gemaakt. Maar deze moet voor het oplossen van enkele van de problemen en het aanbrengen van elk criterium oplossen van deze problemen op een eigen zorgt ervoor dat geen zin. Er is een betere benadering voor het maken van een platform waarmee u algemene oplossingen voor veel toepassingen gebruiken. En bouwen van dit platform in de cloud heeft enkele voordelen wissen. Kan algemeen beschikbaar is op basis van betalen naar gebruik, en ook de verscheidenheid van de vraag die vaak met video toepassingen dezelfde kan verwerken.

Azure Media Services lost dit probleem. Dit biedt een set van cloud-onderdelen die leven eenvoudiger voor gebruikers maken en uitvoeren van toepassingen met media video en andere.

Zoals in de afbeelding toont, biedt Media Services een reeks onderdelen voor toepassingen die met video's en andere media werken. Het bevat bijvoorbeeld een medium voor het opnemen van onderdeel voor het uploaden van video in Media Services (waar deze wordt opgeslagen in Azure Blobs), een onderdeel van het codering die ondersteuning biedt voor verschillende video en audio-indelingen, een onderdeel van de beveiliging van inhoud waarmee beheer van digitale rechten, een onderdeel voor het invoegen van advertenties in een videostream, onderdelen voor streaming en meer. Microsoft-partners kunnen ook onderdelen opgeven voor het platform en vervolgens dat Microsoft deze onderdelen te distribueren en namens hen in rekening brengen.

Toepassingen die gebruikmaken van dit platform kunnen uitvoeren op Azure of ergens anders. Bijvoorbeeld, een bureaubladtoepassing voor een video productie huis kunt mogelijk de video uploaden met Media Services, gebruikers vervolgens verwerken op verschillende manieren. U kunt ook een inhoudsbeheer cloud-gebaseerde service uitgevoerd op Azure mogelijk afhankelijk van de Media Services om te verwerken en distribueren van video. Waar deze wordt uitgevoerd en wat het doet, elke toepassing geeft de voorkeur welke onderdelen moet worden gebruikt, toegang hebben tot deze via RESTful-interfaces.

Voor het distribueren van wat is het resultaat, een toepassing gebruik van Azure CDN, een ander CDN of net bits rechtstreeks naar gebruikers verzenden. Bij het ophalen van er, kan er echter video gemaakt met behulp van Media Services worden gebruikt door verschillende clientsystemen, met inbegrip van Windows, Macintosh HTML 5, iOS, Android, Windows Phone, Flash en Silverlight. Het doel is om het gemakkelijker om voor moderne mediatoepassingen te maken.

**Verwijzingen**

Voor een meer visuele weergave van de werking van Media Services, downloaden de [Azure Media Services Poster][Azure Media Services Poster].

## <a name="commerce"></a>Commerce
De toename van de Software als een Service worden getransformeerd hoe we toepassingen maken. Dit wordt ook transformeren hoe we toepassingen verkopen. Aangezien er zich een SaaS-toepassing in de cloud, is het zinvol die de potentiële klanten er voor online-oplossingen ziet. En deze wijziging geldt voor gegevens over de toepassingen. Waarom mag niet mensen zoekt u naar de cloud verkrijgbare gegevenssets? Microsoft houdt zowel deze problemen met de [Azure Marketplace](https://azure.microsoft.com/marketplace/).

![Azure-Commerce](./media/fundamentals-introduction-to-azure/CommerceIntroNew.png)   
 **Afbeelding: Azure Marketplace en Azure Store kunt u vinden en kopen van Azure-toepassingen en in de commerciële gegevenssets en ze als onderdeel van uw Azure-toepassingen gebruiken.**

Het verschil tussen de twee is dat Marketplace buiten de Azure-portal valt, maar de Store toegankelijk zijn vanuit binnen de portal. Azure-toepassingen die hun behoeften kunnen potentiële klanten zoeken. Klanten kunnen zoeken voor commerciële gegevenssets, met inbegrip van demografische gegevens, financiële gegevens en geografische gegevens. Wanneer ze iets dat die ze graag wilt vinden, toegang te krijgen tot deze een van de leverancier rechtstreeks via de Marketplace of winkel weblocaties of in sommige gevallen uit de Azure portal. Toepassingen kunnen ook de Bing zoeken-API gebruiken via de Marketplace, zodat ze toegang hebben op de resultaten van zoekacties op het web.

**Commerce scenario 's**

SendGrid is een toepassing in de Azure Store waarmee u e-mail verzenden. Biedt aanvullende functionaliteit, zoals de levering van betrouwbare en statistieken.  U kunt deze toepassing en verwante services kopen plaats probeert te maken van een dergelijke infrastructuur zelf.  

## <a name="getting-started"></a>Aan de slag
Nu u hebt de grote afbeelding, de volgende stap is het schrijven van uw eerste Azure-toepassing. Kies uw taal [Download de juiste SDK](https://azure.microsoft.com/en-us/downloads/), en Ga daarvoor. Cloud computing is de nieuwe standaard--nu aan de slag.

[Azure Media Services Poster]: http://azure.microsoft.com/documentation/infographics/media-services/
