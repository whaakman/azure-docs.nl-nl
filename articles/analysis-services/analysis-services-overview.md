---
title: Wat is Azure Analysis Services | Microsoft Docs
description: Hier vindt u een algemene beschrijving van Analysis Services in Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/03/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 625baa2df8b137779ed846c584a138cc15e89a3f
ms.contentlocale: nl-nl
ms.lasthandoff: 07/26/2017

---
# <a name="what-is-azure-analysis-services"></a>Wat is Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services biedt een krachtige, cloudgebaseerde oplossing voor het modelleren van gegevens. Het is een volledig beheerd Platform as a Service (PaaS), hecht geïntegreerd met de Azure Data Platform-services. Met de uiterst effectieve OLAP-engine voor gegevensanalyse brengt Analysis Services een uitgebreide semantische modellaag aan tussen grote, complexe en vaak totaal verschillende gegevensbronnen. U kunt het semantische gegevensmodel definiëren door gegevens te combineren. Zo ontstaat een krachtige, naadloos in uw omgeving passende analysetool waarmee u geavanceerde, interactieve analyses uitvoert in state-of-the-art clienthulpprogramma's.

![Gegevensbronnen](./media/analysis-services-overview/aas-overview-data-sources.png)


Bekijk [deze video](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) om te zien hoe Azure Analysis Services aansluit bij de overige BI-voorzieningen van Microsoft en waarom het zinvol is om uw gegevensmodellen over te brengen naar de cloud.

<!--
>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>
-->

## <a name="built-on-sql-server-analysis-services"></a>Gebaseerd op SQL Server Analysis Services
Azure Analysis Services is compatibel met de voor u bekende SQL Server Analysis Services Enterprise Edition. Azure Analysis Services ondersteunt tabellaire modellen met het compatibiliteitsniveau 1200 en 1400. Partities, beveiliging op rijniveau, bidirectionele relaties en vertalingen worden allemaal ondersteund. De in-memory en DirectQuery-modi staan garant voor razendsnelle query's in omvangrijke en complexe gegevenssets.

De tabellaire modellen kunnen snel worden ontwikkeld en zijn in hoge mate aanpasbaar. Voor ontwikkelaars bevatten de tabellaire modellen ook Tabular Object Model (TOM) om modelobjecten te beschrijven. TOM wordt in JSON weergegeven via [TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) en in de AMO Data Definition Language met behulp van de naamruimte [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx).

Nieuwe functies in tabellaire modellen met compatibiliteitsniveau 1400 bieden ondersteuning voor detailrijen, beveiliging op objectniveau, een onregelmatige hiërarchie, een moderne Get Data-ervaring in SSDT voor gegevens en talloze andere verbeteringen. En omdat de onderliggende metagegevens van de modellen identiek zijn, kunnen bestaande tabellaire on-premises modellen eenvoudig worden gemigreerd naar de cloud.


## <a name="better-with-azure"></a>Beter met Azure
Azure Analysis Services kan worden geïntegreerd met veel Azure-gegevensservices, zodat u geavanceerde analyseoplossingen kunt bouwen.

Azure Analysis Services kan gegevens betrekken uit Azure SQL Database, Azure SQL Data Warehouse en Azure Blob Storage. U kunt in Azure grootschalige datawarehouse-oplossingen bouwen met behulp van een hub-and-spoke-model, waarbij zich in het midden een SQL-datawarehouse bevindt met daaromheen meerdere BI-modellen voor verschillende bedrijfsonderdelen of aandachtsgebieden.

Met Azure Data Factory kunt u de verplaatsing en transformatie van gegevens regelen, onmisbare functionaliteit in elke grote BI-/analyseoplossing. Azure Analysis Services kan worden geïntegreerd in elke Azure Data Factory-pijplijn door een activiteit toe te voegen die gegevens in het model laadt. Voor eenvoudige indelingsgerelateerde taken in modellen met behulp van aangepaste code kunnen bovendien ook Azure Automation en Azure Functions worden gebruikt.

Azure Analysis Services is daarnaast sterk geïntegreerd met Azure Active Directory, zodat u kunt profiteren van veilige, op rollen gebaseerde toegang tot uw belangrijkste gegevens.

## <a name="pricing"></a>Prijzen
Azure Analysis Services is beschikbaar in de servicelagen Developer, Basic en Standard. Binnen elke servicelaag variëren de abonnementskosten afhankelijk van verwerkingskracht, QPU's en geheugen. Wanneer u een server maakt, selecteert u binnen een servicelaag een abonnement. U kunt een abonnement binnen dezelfde servicelaag omhoog of omlaag bijstellen en ook upgraden naar een hogere servicelaag. U kunt echter niet downgraden naar een lagere servicelaag.

![Servicelaag upgraden](./media/analysis-services-overview/aas-overview-tier-up.png)

U kunt de servicelaag direct upgraden in Azure Portal of met de PowerShell-cmdlet Set-AzureRmAnalysisServicesServer. Raadpleeg [Prijzen van Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/) voor meer informatie over de verschillende abonnementen en servicelagen, en om de prijscalculator te gebruiken. Hiermee bepaalt u eenvoudig welk abonnement het meest geschikt voor u is.

## <a name="scale-resources"></a>Resources omhoog/omlaag schalen
U kunt uw server omhoog of omlaag schalen en zelfs onderbreken. Dit doet u via Azure Portal of, voor volledige controle op elk moment, met behulp van PowerShell. U betaalt alleen voor wat u gebruikt.

Gebruik bij het maken van een nieuwe server de cmdlet [New-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) om uw abonnement in te stellen. Als u voor een bestaande server het abonnement wilt wijzigen, gebruikt u de cmdlet [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). Gebruikt u de service niet continu? Dan kunt u de service onderbreken via de portal of met de cmdlet [Suspend-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver). U hervat de service met de cmdlet [Resume-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver). U betaalt alleen voor de perioden waarin uw server actief is.


## <a name="regions"></a>Regio's
U kunt een Azure Analysis Services-server maken in de volgende [Azure-regio's](https://azure.microsoft.com/regions/):

| Noord- en Zuid-Amerika | Europa | Azië en Stille Oceaan |
|----------|--------|--------------|
|  Brazilië - zuid<br> Canada - midden<br> VS - oost 2<br> Noord-centraal VS<br> Zuid-centraal VS<br> West-centraal VS<br> VS - west | Noord-Europa<br> Verenigd Koninkrijk Zuid<br> West-Europa |   Australië - zuidwest<br> Japan - oost<br> Zuidoost-Azië<br> West-India  |

Er worden voortdurend nieuwe regio's toegevoegd. Mogelijk is deze lijst daarom onvolledig. U kiest uw locatie tijdens het maken van uw server in Azure Portal of met behulp van een Azure Resource Manager-sjabloon. Voor optimale prestaties moet u een locatie kiezen die zich zo dicht mogelijk bij uw grootste gebruikersgroep bevindt. Een [hoge beschikbaarheid](analysis-services-bcdr.md) garandeert u door uw modellen op redundante servers in meerdere regio's te implementeren.

## <a name="get-up-and-running-quickly"></a>Snel aan de slag
Via Azure Portal kunt u binnen enkele minuten [een server maken](analysis-services-create-server.md). En met behulp van Azure Resource Manager-sjablonen en PowerShell kunt u servers inrichten aan de hand van een declaratieve sjabloon. Met één enkele sjabloon kunt u meerdere services implementeren, samen met andere Azure-onderdelen, zoals opslagaccounts.  Raadpleeg voor meer informatie [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

Nadat u een server hebt gemaakt, kunt u rechtstreeks in Azure Portal een tabellair model maken. Met de nieuwe functie Web Designer (preview) kunt u verbinding maken met een Azure SQL-database of een Azure SQL Data Warehouse-gegevensbron, maar ook een pbix-bestand uit Power BI Desktop importeren. De relaties tussen de tabellen worden automatisch aangebracht. Bovendien kunt u rechtstreeks vanuit uw browser metingen instellen en het model.bim-bestand in JSON-indeling bewerken.

## <a name="migrate-existing-tabular-models"></a>Bestaande tabellaire modellen migreren
Als u een bestaand on-premises SQL Server Analysis Services-model hebt, kunt u dit zonder noemenswaardige wijzigingen naar Azure Analysis Services migreren. Bij de migratie kunt u SSDT gebruiken om het model op uw server te implementeren. In SSMS kunt u ook Back-up maken en terugzetten gebruiken, evenals TMSL.

Als u on-premises gegevensbronnen hebt, moet u een [on-premises gegevensgateway](analysis-services-gateway.md) installeren en configureren. Als u al rollen hebt geconfigureerd en toegewezen, kunt u uw rollen gewoon migreren, maar moet u de toewijzingen lezen met behulp van SSMS of PowerShell.


## <a name="data-sources"></a>Gegevensbronnen
Azure Analysis Services biedt ondersteuning voor het maken van verbinding met on-premises gegevensbronnen binnen uw organisatie en met gegevensbronnen in de cloud. Combineer gegevens uit zowel on-premises als in de cloud opgeslagen gegevensbronnen voor een hybride oplossing. 

Voor nieuwe tabellaire modellen met compatibiliteitsniveau 1400 wordt in SSDT gebruikgemaakt van de innovatieve functie Get Data, op basis van de querytaal M Formula. Met Get Data beschikt u over meer functies voor gegevenstransformatie en mashup, en over de mogelijkheid om uw eigen geavanceerde query's in M Formula te maken en te bewerken. U kunt een tabellair model met compatibiliteitsniveau 1400 bijvoorbeeld baseren op gegevensbestanden in Azure Blob Storage.

Azure Analysis Services ondersteunt het gebruik van [DirectQuery](https://docs.microsoft.com/sql/analysis-services/tabular-models/directquery-mode-ssas-tabular) voor een rechtstreekse verbinding met Azure SQL Database, Azure SQL Data Warehouse, SQL Server, SQL Server Data Warehouse, Oracle en relationele databases in Teradata.

Raadpleeg voor meer informatie [Data sources supported in Azure Analysis Services](analysis-services-datasource.md) (Gegevensbronnen die in Azure Analysis Services worden ondersteund).

## <a name="use-the-tools-you-already-know"></a>Werken met de tools die u al kent

![Tools voor BI-ontwikkelaars](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>SQL Server Data Tools (SSDT) voor Visual Studio
Ontwikkel en implementeer modellen met de gratis [SQL Server Data Tools (SSDT) voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). SSDT bevat Analysis Services-projectsjablonen waarmee u snel aan de slag kunt gaan. Bovendien biedt SSDT voor tabellaire modellen met compatibiliteitsniveau 1400 nu ook de innovatieve queryfunctie Get Data en mashup-functionaliteit. Als u al bekend bent met Get Data in Power BI Desktop en Excel 2016, weet u hoe eenvoudig het is om voor gegevensbronnen query's te maken die naadloos aansluiten bij uw behoeften.

Met de nieuwe versie van SSDT in combinatie met tabellaire modellen met compatibiliteitsniveau 1400 is het niet meer nodig om een lokaal exemplaar van Analysis Services te installeren voor het hosten van een werkruimtedatabase. SSDT bevat nu een eigen geïntegreerde Analysis Services-engine en -database. Wanneer u klaar bent, kunt u rechtstreeks vanuit SSDT naar uw Azure-servers implementeren. SSDT wordt bovendien maandelijks bijgewerkt, zodat u altijd snel aan de slag kunt met de nieuwste functies.

#### <a name="sql-server-management-studio"></a>SQL Server Management Studio
Servers en modeldatabases beheert u met behulp van [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Maak verbinding met uw servers in de cloud. Voer rechtstreeks vanuit het XMLA-queryvenster TMSL-scripts uit en automatiseer hiermee taken. Omdat er in hoog tempo nieuwe functies en mogelijkheden worden toegevoegd, wordt SSMS maandelijks bijgewerkt.

#### <a name="powershell"></a>PowerShell
Gebruik de cmdlets in de Azure Resource Manager-module (AzureRM-module) om beheertaken voor serverresources uit te voeren, zoals servers maken, serverbewerkingen onderbreken of hervatten, of het serviceniveau (de servicelaag) wijzigen. Voor andere databasebeheertaken, zoals het toevoegen of verwijderen van roltoewijzingen, het toepassen van bewerkingen of het uitvoeren van TMSL-scripts, gebruikt u de cmdlets van de SQL Server-module. De AzureRM-module en de SQL Server-module zijn beschikbaar in de [PowerShell Gallery](https://www.powershellgallery.com/).


## <a name="secure"></a>Beveiligen
![Gegevensvisualisaties](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Authentication
Gebruikersverificatie voor Azure Analysis services wordt afgehandeld door [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Wanneer gebruikers zich willen aanmelden bij een Azure Analysis Services-database, moeten ze de id van een organisatie-account gebruiken die toegang heeft tot de database die ze willen raadplegen. Deze gebruikers-id's moeten lid zijn van de standaard Azure Active Directory voor het abonnement waarin de Azure Analysis Services-server is ondergebracht. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).

#### <a name="data-security"></a>Gegevensbeveiliging
Azure Analysis Services maakt gebruik van Azure Blob-opslag om opslag en metagegevens voor Analysis Services-databases persistent te maken. Gegevensbestanden in Blob worden versleuteld met behulp van Azure Blob Server Side Encryption (SSE). Wanneer de Direct Query-modus wordt gebruikt, worden alleen metagegevens opgeslagen. De werkelijke gegevens worden op het moment van uitvoering van de query opgehaald uit de gegevensbron.

#### <a name="on-premises-data-sources"></a>On-premises gegevensbronnen
Beveiligde toegang tot gegevens die on-premises zijn opgeslagen binnen uw organisatie, is mogelijk door installatie en configuratie van een [on-premises gegevensgateway](analysis-services-gateway.md). Gateways bieden toegang tot gegevens voor zowel de Direct Query-modus als de modus In-memory. Wanneer een Azure Analysis Services-model verbinding maakt met een on-premises gegevensbron, wordt er een query gemaakt die ook de versleutelde referenties voor de on-premises gegevensbron bevat. De gateway-cloudservice analyseert de query en verstuurt de aanvraag naar een Azure Service Bus. De on-premises gateway pollt de Azure Service Bus op aanvragen die in behandeling zijn. De gateway ontvangt vervolgens de query, ontsleutelt de referenties en maakt verbinding met de gegevensbron voor uitvoering. De resultaten worden vervolgens vanuit de gegevensbron verzonden naar de gateway en vervolgens verder naar de Azure Analysis Services-database.

Azure Analysis Services is onderhevig aan de [gebruiksvoorwaarden van Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [privacyverklaring van Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Ga naar het [Microsoft Vertrouwenscentrum](https://www.microsoft.com/trustcenter/Security/AzureSecurity) voor meer informatie over Azure-beveiliging.

## <a name="client-connections"></a>Clientverbindingen
![Gegevensvisualisaties](./media/analysis-services-overview/aas-overview-clients.png)

Moderne hulpmiddelen voor het verkennen en visualiseren van gegevens, zoals Power BI, Excel en andere hulpprogramma's van derden, bieden eindgebruikers interactieve en visueel aantrekkelijke inzichten in de gegevens van uw model.

Clients gebruiken MSOLAP-, AMO- of ADOMD-[clientbibliotheken](analysis-services-data-providers.md) om verbinding te maken met Analysis Services-servers. In Microsoft-clienttoepassingen als Power BI Desktop en Excel zijn alle drie deze clientbibliotheken geïnstalleerd. Afhankelijk van de versie van de toepassing of de updatefrequentie zijn dit echter mogelijk niet de clientbibliotheekversies die door Azure Analysis Services worden vereist (de meest recente versies). Dit geldt ook voor aangepaste toepassingen of andere interfaces, zoals AsCmd, TOM en ADOMD.NET. Voor deze toepassingen moeten de bibliotheken doorgaans handmatig worden geïnstalleerd als onderdeel van een pakket.


## <a name="get-help"></a>Help opvragen

#### <a name="documentation"></a>Documentatie
Azure Analysis Services is eenvoudig in te stellen en te beheren. U kunt hier alle informatie vinden die u nodig hebt om uw servergebaseerde services te maken en te beheren. Wanneer u een gegevensmodel gaat maken voor implementatie op uw server, is de procedure grotendeels hetzelfde als bij het maken van een gegevensmodel dat u implementeert op een on-premises server. Er is een uitgebreide bibliotheek met overzichtsinformatie, procedures, zelfstudies en naslagartikelen beschikbaar op [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services).

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

