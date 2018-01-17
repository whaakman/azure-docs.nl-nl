---
title: Wat is Azure Analysis Services | Microsoft Docs
description: Hier vindt u een algemene beschrijving van Analysis Services in Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/04/2017
ms.author: owend
ms.openlocfilehash: fe46b3c099c4fd4e04a39e2b2f127c1206711e60
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-analysis-services-overview"></a>Wat is Azure Analysis Services
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services biedt een krachtige, cloudgebaseerde oplossing voor het modelleren van gegevens. Het is een volledig beheerd Platform as a Service (PaaS), geïntegreerd met de Azure Data Platform-services. 

Met Analysis Services kunt u gegevens uit meerdere bronnen verfijnen en combineren, metrische gegevens definiëren, en de gegevens beveiligen in één vertrouwd semantisch gegevensmodel. Het gegevensmodel biedt een gemakkelijkere en snellere manier om door enorme hoeveelheden gegevens te bladeren met behulp van clienttoepassingen zoals Power BI, Excel, Reporting Services, apps van derden en aangepaste apps.

![Gegevensbronnen](./media/analysis-services-overview/aas-overview-data-sources.png)

Bekijk [deze video](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) om te zien hoe Azure Analysis Services aansluit bij de overige BI-voorzieningen van Microsoft en waarom het zinvol is om uw gegevensmodellen over te brengen naar de cloud.

## <a name="built-on-sql-server-analysis-services"></a>Gebaseerd op SQL Server Analysis Services
Azure Analysis Services is compatibel met veel geweldige functies die al deel uitmaken van SQL Server Analysis Services Enterprise Edition. Azure Analysis Services ondersteunt tabellaire modellen met het [compatibiliteitsniveau](analysis-services-compat-level.md) 1200 en 1400. Partities, beveiliging op rijniveau, bidirectionele relaties en vertalingen worden allemaal ondersteund. De in-memory en DirectQuery-modi staan garant voor razendsnelle query's in omvangrijke en complexe gegevenssets.

De tabellaire modellen kunnen snel worden ontwikkeld en zijn in hoge mate aanpasbaar. Voor ontwikkelaars bevatten de tabellaire modellen ook Tabular Object Model (TOM) om modelobjecten te beschrijven. TOM wordt in JSON weergegeven via [TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) en in de AMO Data Definition Language met behulp van de naamruimte [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx).

## <a name="better-with-azure"></a>Beter met Azure
Azure Analysis Services kan worden geïntegreerd met veel Azure-services, zodat u geavanceerde analyseoplossingen kunt creëren. Integratie met [Azure Active Directory](../active-directory/active-directory-whatis.md) biedt beveiligde, op rollen gebaseerde toegang tot kritieke gegevens. Integreer met [Azure Data Factory](../data-factory/introduction.md)-pijplijnen door een activiteit toe te voegen die gegevens in het model laadt. [Azure Automation](../automation/automation-intro.md) en [Azure Functions](../azure-functions/functions-overview.md) kunnen worden gebruikt voor de eenvoudige indeling van modellen met behulp van aangepaste code.

## <a name="get-up-and-running-quickly"></a>Snel aan de slag
In Azure Portal kunt u binnen enkele minuten [een server maken](analysis-services-create-server.md). En met behulp van Azure Resource Manager-[sjablonen](../azure-resource-manager/resource-manager-create-first-template.md) en PowerShell kunt u servers inrichten aan de hand van een declaratieve sjabloon. Met één enkele sjabloon kunt u meerdere services implementeren, samen met andere Azure-onderdelen, zoals opslagaccounts en Azure Functions. 

Nadat u een server hebt gemaakt, kunt u rechtstreeks in Azure Portal een tabellair model maken. Met de nieuwe functie [Web Designer](analysis-services-create-model-portal.md) (preview-versie) kunt u verbinding maken met een Azure SQL-database of een Azure SQL Data Warehouse-gegevensbron, maar ook een pbix-bestand uit Power BI Desktop importeren. De relaties tussen de tabellen worden automatisch aangebracht. Bovendien kunt u rechtstreeks vanuit uw browser metingen instellen en het model.bim-bestand in JSON-indeling bewerken.

## <a name="scale-to-your-needs"></a>Schalen naar uw behoeften

### <a name="the-right-tier-when-you-need-it"></a>Altijd de juiste laag

Azure Analysis Services is beschikbaar in de servicelagen Developer, Basic en Standard. Binnen elke servicelaag variëren de abonnementskosten afhankelijk van verwerkingskracht, QPU's en geheugen. Wanneer u een server maakt, selecteert u binnen een servicelaag een abonnement. U kunt een abonnement binnen dezelfde servicelaag omhoog of omlaag bijstellen en ook upgraden naar een hogere servicelaag. U kunt echter niet downgraden naar een lagere servicelaag.

U kunt uw server omhoog of omlaag schalen en zelfs onderbreken. Dit doet u via Azure Portal of, voor volledige controle op elk moment, met behulp van PowerShell. U betaalt alleen voor wat u gebruikt. Raadpleeg [Prijzen van Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/) voor meer informatie over de verschillende abonnementen en servicelagen, en om de prijscalculator te gebruiken. Hiermee bepaalt u eenvoudig welk abonnement het meest geschikt voor u is.

### <a name="scale-out-resources-for-fast-query-responses"></a>Resources uitschakelen voor snellere respons van query's

Met de uitschaalfunctie van Azure Analysis Services worden clientquery's verdeeld over meerdere *queryreplica's* in een querypool. Queryreplica's bevatten gesynchroniseerde kopieën van uw modellen in tabelvorm. Door de werkbelasting van query's te spreiden, kunnen responstijden tijdens hoge querywerkbelastingen worden verlaagd. Taken voor modelverwerking kunnen worden gescheiden van de querypool, zodat de prestaties van clientquery's niet nadelig worden beïnvloed door verwerkingstaken. U kunt een querypool maken met maximaal zeven extra queryreplica's (acht in totaal, met inbegrip van uw server). 

Net zoals bij het wijzigen van de laag, kunt u queryreplica's uitschalen als dat nodig is. Dit kan in de portal of met behulp van REST-API's. Zie [Azure Analysis Services uitschalen](analysis-services-scale-out.md) voor meer informatie.

## <a name="keep-your-data-close"></a>Gegevens in de buurt houden
U kunt een Azure Analysis Services-server maken in de volgende [Azure-regio's](https://azure.microsoft.com/regions/):

| Noord- en Zuid-Amerika | Europa | Azië en Stille Oceaan |
|----------|--------|--------------|
|  Brazilië - zuid<br> Canada - midden<br> VS - oost 2<br> Noord-centraal VS<br> Zuid-centraal VS<br> West-centraal VS<br> VS - west | Noord-Europa<br> Verenigd Koninkrijk Zuid<br> West-Europa |   Australië - zuidoost<br> Japan - oost<br> Zuidoost-Azië<br> West-India  |

Er worden voortdurend nieuwe regio's toegevoegd. Mogelijk is deze lijst daarom onvolledig. U kiest uw locatie tijdens het maken van uw server in Azure Portal of met behulp van een Azure Resource Manager-sjabloon. Voor optimale prestaties moet u een locatie kiezen die zich zo dicht mogelijk bij uw grootste gebruikersgroep bevindt. Een [hoge beschikbaarheid](analysis-services-bcdr.md) garandeert u door uw modellen op redundante servers in meerdere regio's te implementeren.

## <a name="migrate-your-existing-tabular-models"></a>Bestaande tabellaire modellen migreren
Als u een bestaand on-premises SQL Server Analysis Services-model hebt, kunt u dit zonder noemenswaardige wijzigingen naar Azure Analysis Services migreren. Bij de migratie kunt u SSDT gebruiken om het model op uw server te implementeren. In SSMS kunt u ook Back-up maken en terugzetten gebruiken, evenals TMSL.

Als u on-premises gegevensbronnen hebt, moet u een [on-premises gegevensgateway](analysis-services-gateway.md) installeren en configureren. Als u al rollen hebt geconfigureerd en toegewezen, kunt u uw rollen gewoon migreren, maar moet u de toewijzingen lezen met behulp van SSMS of PowerShell.

## <a name="connect-to-popular-data-sources"></a>Verbinding maken met populaire gegevensbronnen
Azure Analysis Services biedt ondersteuning voor [het maken van verbinding met on-premises gegevensbronnen](analysis-services-datasource.md) binnen uw organisatie en met gegevensbronnen n de cloud. Combineer gegevens uit zowel on-premises als in de cloud opgeslagen gegevensbronnen voor een hybride oplossing. 

Voor nieuwe tabellaire modellen met compatibiliteitsniveau 1400 wordt in SSDT gebruikgemaakt van de innovatieve functie Get Data, op basis van de querytaal M Formula. Met Get Data beschikt u over meer functies voor gegevenstransformatie en mashup, en over de mogelijkheid om uw eigen geavanceerde query's in M Formula te maken en te bewerken. U kunt een tabellair model met compatibiliteitsniveau 1400 bijvoorbeeld baseren op gegevensbestanden in Azure Blob Storage.

## <a name="use-the-tools-you-already-know"></a>Werken met de tools die u al kent

![Tools voor BI-ontwikkelaars](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>SQL Server Data Tools (SSDT) voor Visual Studio
Ontwikkel en implementeer modellen met de gratis [SQL Server Data Tools (SSDT) voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). SSDT bevat Analysis Services-projectsjablonen waarmee u snel aan de slag kunt gaan. Bovendien biedt SSDT voor tabellaire modellen met compatibiliteitsniveau 1400 nu ook de innovatieve queryfunctie Get Data en mashup-functionaliteit. Als u al bekend bent met Get Data in Power BI Desktop en Excel 2016, weet u hoe eenvoudig het is om voor gegevensbronnen query's te maken die naadloos aansluiten bij uw behoeften.

#### <a name="sql-server-management-studio"></a>SQL Server Management Studio
Servers en modeldatabases beheert u met behulp van [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Maak verbinding met uw servers in de cloud. Voer rechtstreeks vanuit het XMLA-queryvenster TMSL-scripts uit en automatiseer hiermee taken. Omdat er in hoog tempo nieuwe functies en mogelijkheden worden toegevoegd, wordt SSMS maandelijks bijgewerkt.

#### <a name="powershell"></a>PowerShell
Gebruik de cmdlets in de Azure Resource Manager-module (AzureRM-module) om beheertaken voor serverresources uit te voeren, zoals servers maken, serverbewerkingen onderbreken of hervatten, of het serviceniveau (de servicelaag) wijzigen. Voor andere databasebeheertaken, zoals het toevoegen of verwijderen van roltoewijzingen, het toepassen van bewerkingen of het uitvoeren van TMSL-scripts, gebruikt u de cmdlets van de SQL Server-module. De AzureRM-module en de SQL Server-module zijn beschikbaar in de [PowerShell Gallery](https://www.powershellgallery.com/).


## <a name="your-data-is-secure"></a>Gegevens zijn beveiligd
![Gegevensvisualisaties](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Verificatie
Gebruikersverificatie voor Azure Analysis services wordt afgehandeld door [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Wanneer gebruikers zich willen aanmelden bij een Azure Analysis Services-database, moeten ze de id van een organisatie-account gebruiken die toegang heeft tot de database die ze willen raadplegen. Deze gebruikers-id's moeten lid zijn van de standaard Azure Active Directory voor het abonnement waarin de Azure Analysis Services-server is ondergebracht. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).

#### <a name="data-security"></a>Gegevensbeveiliging
Azure Analysis Services maakt gebruik van Azure Blob-opslag om opslag en metagegevens voor Analysis Services-databases persistent te maken. Gegevensbestanden in Blob worden versleuteld met behulp van Azure Blob Server Side Encryption (SSE). Wanneer de Direct Query-modus wordt gebruikt, worden alleen metagegevens opgeslagen. De werkelijke gegevens worden op het moment van uitvoering van de query opgehaald uit de gegevensbron.

#### <a name="firewall"></a>Firewall

De firewall van Azure Analysis Services blokkeert alle clientverbindingen behalve de verbindingen die zijn opgegeven in regels. Configureer regels voor toegestane IP-adressen als individuele client-IP-adressen of als een bereik. Verbindingen van Power BI (service) kunnen ook worden toegestaan of geblokkeerd. 

#### <a name="on-premises-data-sources"></a>On-premises gegevensbronnen
Beveiligde toegang tot gegevens die on-premises zijn opgeslagen binnen uw organisatie, is mogelijk door installatie en configuratie van een [on-premises gegevensgateway](analysis-services-gateway.md). Gateways bieden toegang tot gegevens voor zowel de Direct Query-modus als de modus In-memory. Wanneer een Azure Analysis Services-model verbinding maakt met een on-premises gegevensbron, wordt er een query gemaakt die ook de versleutelde referenties voor de on-premises gegevensbron bevat. De gateway-cloudservice analyseert de query en verstuurt de aanvraag naar een Azure Service Bus. De on-premises gateway pollt de Azure Service Bus op aanvragen die in behandeling zijn. De gateway ontvangt vervolgens de query, ontsleutelt de referenties en maakt verbinding met de gegevensbron voor uitvoering. De resultaten worden vervolgens vanuit de gegevensbron verzonden naar de gateway en vervolgens verder naar de Azure Analysis Services-database.

Azure Analysis Services is onderhevig aan de [gebruiksvoorwaarden van Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [privacyverklaring van Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Ga naar het [Microsoft Vertrouwenscentrum](https://www.microsoft.com/trustcenter/Security/AzureSecurity) voor meer informatie over Azure-beveiliging.



## <a name="supports-the-latest-client-tools"></a>Ondersteuning voor de nieuwste clienthulpprogramma's
![Gegevensvisualisaties](./media/analysis-services-overview/aas-overview-clients.png)

Moderne hulpprogramma's voor het verkennen en visualiseren van gegevens, zoals Power BI, Excel, SQL Server 2017 Reporting Services en hulpprogramma's van derden, bieden gebruikers interactieve en visueel aantrekkelijke inzichten in de gegevens van uw model. 

Clients gebruiken MSOLAP-, AMO- of ADOMD-[clientbibliotheken](analysis-services-data-providers.md) om verbinding te maken met Analysis Services-servers. In Microsoft-clienttoepassingen als Power BI Desktop en Excel zijn alle drie deze clientbibliotheken geïnstalleerd. Afhankelijk van de versie van de toepassing of de updatefrequentie zijn dit echter mogelijk niet de clientbibliotheekversies die door Azure Analysis Services worden vereist (de meest recente versies). Dit geldt ook voor aangepaste toepassingen of andere interfaces, zoals AsCmd, TOM en ADOMD.NET. Voor deze toepassingen moeten de bibliotheken doorgaans handmatig worden geïnstalleerd als onderdeel van een pakket.


## <a name="get-help"></a>Help opvragen

#### <a name="documentation"></a>Documentatie
Azure Analysis Services is eenvoudig in te stellen en te beheren. U kunt hier alle informatie vinden die u nodig hebt om uw servergebaseerde services te maken en te beheren. Wanneer u een gegevensmodel gaat maken voor implementatie op uw server, is de procedure grotendeels hetzelfde als bij het maken van een gegevensmodel dat u implementeert op een on-premises server. Er is een uitgebreide bibliotheek met overzichtsinformatie, procedures, zelfstudies en naslagartikelen beschikbaar op [SQL Server Analysis Services help](https://docs.microsoft.com/sql/analysis-services/analysis-services).

#### <a name="videos"></a>Video's
Bekijk nuttige video's over [Azure Analysis Services op Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

#### <a name="blogs"></a>Blogs
Er veranderen nog veel dingen. U krijgt altijd de meest recente informatie in het [blog van het Analysis Services-team](https://blogs.msdn.microsoft.com/analysisservices/) en in het [Azure-blog](https://azure.microsoft.com/blog/).

#### <a name="community"></a>Community
Analysis Services heeft een zeer actieve gebruikerscommunity. Doe mee op [het Azure Analysis Services-forum](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Feedback
Hebt u suggesties of functieverzoeken? Uw [feedback voor Azure Analysis Services](https://aka.ms/azureanalysisservicesfeedback) wordt zeer op prijs gesteld.

Hebt u suggesties over de documentatie? U kunt met behulp van Livefyre onderaan elk artikel opmerkingen toevoegen.

## <a name="next-steps"></a>Volgende stappen
U weet nu in grote lijnen wat Azure Analysis Services is en dus is het tijd om echt aan de slag te gaan. Leer nu [hoe u een server maakt](analysis-services-create-server.md) in Azure. Wanneer uw server klaar is, doorloopt u de [Adventure Works-zelfstudie](tutorials/aas-adventure-works-tutorial.md) voor informatie over hoe u een volledig functioneel tabellair model maakt en dit op uw server implementeert.
