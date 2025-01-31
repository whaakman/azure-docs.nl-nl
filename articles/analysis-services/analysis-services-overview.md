---
title: Wat is Azure Analysis Services | Microsoft Docs
description: Hier vindt u een algemene beschrijving van Analysis Services in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: overview
ms.date: 07/01/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: ac27f3b1ea777e7a88a44a16edf8867f97381297
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508992"
---
# <a name="what-is-azure-analysis-services"></a>Wat is Azure Analysis Services?

![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services is een volledig beheerd platform als een service (PaaS) dat gegevensmodellen van ondernemingsklasse in de cloud levert. Gebruik geavanceerde mashup- en modelleringsfuncties om gegevens uit meerdere gegevensbronnen te verfijnen en te combineren, metrische gegevens te definiëren, en uw gegevens te beveiligen in één vertrouwd tabellarisch semantisch gegevensmodel. Het gegevensmodel biedt een gemakkelijkere en snellere manier voor gebruikers om te bladeren enorme hoeveelheden gegevens voor ad hoc gegevensanalyse.

![Gegevensbronnen](./media/analysis-services-overview/aas-overview-overall.png)

**Video:** Bekijk [Overzicht van Azure Analysis Services](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) om te ontdekken hoe Azure Analysis Services past in de algemene BI-mogelijkheden van Microsoft.

## <a name="get-up-and-running-quickly"></a>Snel aan de slag

In Azure Portal kunt u binnen enkele minuten [een server maken](analysis-services-create-server.md). En met Azure Resource Manager-[sjablonen](../azure-resource-manager/resource-manager-create-first-template.md) en PowerShell kunt u servers maken met behulp van een declaratieve sjabloon. Met één enkele sjabloon kunt u meerdere serverresources implementeren, samen met andere Azure-onderdelen als opslagaccounts en Azure Functions. 

**Video:** Bekijk [Implementatie automatiseren](https://channel9.msdn.com/series/Azure-Analysis-Services/AzureAnalysisServicesAutomation) en ontdek hoe u met Azure Automation sneller servers kunt maken.

Azure Analysis Services kan worden geïntegreerd met veel Azure-services, zodat u geavanceerde analyseoplossingen kunt creëren. Integratie met [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) biedt beveiligde, op rollen gebaseerde toegang tot kritieke gegevens. Integreer met [Azure Data Factory](../data-factory/introduction.md)-pijplijnen door een activiteit toe te voegen die gegevens in het model laadt. [Azure Automation](../automation/automation-intro.md) en [Azure Functions](../azure-functions/functions-overview.md) kunnen worden gebruikt voor de eenvoudige indeling van modellen met behulp van aangepaste code. 

## <a name="the-right-tier-when-you-need-it"></a>Altijd de juiste laag

Azure Analysis Services is beschikbaar in de servicelagen **Developer**, **Basic** en **Standard**. Binnen elke servicelaag variëren de abonnementskosten afhankelijk van verwerkingskracht, QPU's en geheugen. Wanneer u een server maakt, selecteert u binnen een servicelaag een abonnement. U kunt een abonnement binnen dezelfde servicelaag omhoog of omlaag bijstellen en ook upgraden naar een hogere servicelaag. U kunt echter niet downgraden naar een lagere servicelaag.

### <a name="developer-tier"></a>Developer-laag

Deze laag wordt aanbevolen voor evaluatie-, ontwikkelings- en testscenario's. Eén abonnement bevat alle functionaliteit van de Standard-laag, maar heeft beperkte verwerkingskracht, QPU's en geheugengrootte. Uitschalen van queryreplica’s *is niet beschikbaar* voor deze laag. Voor deze laag is geen SLA beschikbaar.

|Plannen  |QPU's  |Geheugen (GB)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Basislaag

Deze-laag wordt aanbevolen voor productieoplossingen met kleinere tabellarische modellen, een beperkt aantal gelijktijdige gebruikers en eenvoudige vereisten voor gegevensvernieuwing. Uitschalen van queryreplica’s *is niet beschikbaar* voor deze laag. Perspectieven, meerdere partities en functies van het tabellarische DirectQuery-model *worden niet ondersteund* in deze laag.  

|Plannen  |QPU's  |Geheugen (GB)  |
|---------|---------|---------|
|B1    |    40     |    10     |
|B2    |    80     |    20     |

### <a name="standard-tier"></a>Standaardlaag

Deze laag is de meest geschikte voor essentiële productietoepassingen waarvoor elastische gebruikersgelijktijdigheid is vereist en waarvan de gegevensmodellen snel groter worden. Deze laag ondersteunt geavanceerde gegevensvernieuwing voor bijna real-time gegevensmodelupdates en ondersteunt alle tabellarische modelleringsfuncties.

|Plannen  |QPU's  |Geheugen (GB)  |
|---------|---------|---------|
|S0    |    40     |    10     |
|S1    |    100     |    25     |
|S2    |    200     |    50     |
|S4    |    400     |    100     |
|S8*    |    320     |    200     |
|S9*    |    640    |    400     |

\* Niet beschikbaar in alle regio’s.  

## <a name="availability-by-region"></a>Beschikbaarheid per regio

Azure Analysis Services wordt ondersteund in regio's in de hele wereld. Ondersteunde abonnementen en beschikbaarheid van querreplica's is afhankelijk van de regio die u kiest. De beschikbaarheid van abonnementen en queryreplica's kan veranderen, afhankelijk van de behoefte aan en de beschikbare resources voor elke regio. 

### <a name="americas"></a>Noord- en Zuid-Amerika

|Regio  | Ondersteunde abonnementen | Queryreplica's (alleen Standard-abonnementen) |
|---------|---------|:---------:|
|Brazilië - zuid     |    B1, B2, S0, S1, S2, S4, D1     |     1    |
|Canada - midden    |     B1, B2, S0, S1, S2, S4, D1    |     1    |
|East US     |     B1, B2, S0, S1, S2, S4, D1    |    1     |
|US - oost 2     |     B1, B2, S0, S1, S2, S4, D1   |    7    |
|US - oost 2     |     S8, S9   |    1    |
|US - noord-centraal     |     B1, B2, S0, S1, S2, S4, D1     |    1     |
|US - centraal     |    B1, B2, S0, S1, S2, S4, D1     |    1     |
|US - zuid-centraal     |    B1, B2, S0, S1, S2, S4, D1     |    1     |
|US - west-centraal   |     B1, B2, S0, S1, S2, S4, D1    |    3     |
|US - west     |    B1, B2, S0, S1, S2, S4, D1    |    7   |
|US - west     |    S8, S9   |    2  |
|US - west 2    |    B1, B2, S0, S1, S2, S4, D1    |    3   |
|US - west 2    |    S8, S9  |    1     |

### <a name="europe"></a>Europa

|Regio  | Ondersteunde abonnementen | Queryreplica's (alleen Standard-abonnementen) |
|---------|---------|:---------:|
|Europa - noord     |    B1, B2, S0, S1, S2, S4, D1      |    7     |
|Verenigd Koninkrijk Zuid     |    B1, B2, S0, S1, S2, S4, D1      |     1    |
|Europa -west     |    B1, B2, S0, S1, S2, S4, D1   |    7    |
|Europa -west    |   S8, S9  |  1  |

### <a name="asia-pacific"></a>Azië en Stille Oceaan 

|Regio  | Ondersteunde abonnementen | Queryreplica's (alleen Standard-abonnementen) |
|---------|---------|:---------:|
|Australië - oost     |    B1, B2, S0, S1, S2, S4     |    3     |
|Australië - oost     |    S8, S9    |    1     |
|Australië - zuidoost     | B1, B2, S0, S1, S2, S4, D1       |    1     |
|Japan - oost     |   B1, B2, S0, S1, S2, S4, D1       |    1     |
|Azië - zuidoost     |     B1, B2, S0, S1, S2, S4, S8, S9, D1     |   1      |
|India - west     |    B1, B2, S0, S1, S2, S4, D1     |    1     |

## <a name="scale-to-your-needs"></a>Schalen naar uw behoeften

### <a name="scale-updown-pause-and-resume"></a>Omhoog/omlaag schalen, onderbreken en hervatten

U kunt uw server omhoog of omlaag schalen en zelfs onderbreken. Dit doet u via Azure Portal of, voor volledige controle op elk moment, met behulp van PowerShell. U betaalt alleen voor wat u gebruikt.  

### <a name="scale-out-resources-for-fast-query-responses"></a>Resources uitschalen voor snellere queryrespons

Door uit te schalen worden clientquery's verdeeld over meerdere *queryreplica's* in een querypool. Queryreplica's bevatten gesynchroniseerde kopieën van uw modellen in tabelvorm. Door de werkbelasting van query's te spreiden, kunnen responstijden tijdens hoge querywerkbelastingen worden verlaagd. Taken voor modelverwerking kunnen worden gescheiden van de querypool, zodat de prestaties van clientquery's niet nadelig worden beïnvloed door verwerkingstaken. 

U kunt een querypool maken met maximaal zeven extra queryreplica's (acht in totaal, met inbegrip van uw server). Het aantal queryreplica's dat u in uw pool kunt hebben, is afhankelijk van het abonnement en de regio die u hebt gekozen. Queryreplica's kunnen niet buiten de regio van uw server worden verspreid. Queryreplica's worden gefactureerd tegen hetzelfde tarief als uw server.

Net zoals bij het wijzigen van lagen, kunt u queryreplica's uitschalen als dat nodig is. Dit kan in de portal of met behulp van REST-API's. Zie [Azure Analysis Services uitschalen](analysis-services-scale-out.md) voor meer informatie.

## <a name="pricing"></a>Prijzen

Totale kosten zijn afhankelijk van een aantal factoren; bijvoorbeeld uw gekozen regio, laag, queryreplica's en onderbreken/hervatten. Gebruik de calculator voor [Prijzen voor Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/) om typische prijzen voor uw regio te bepalen. Dit hulpprogramma berekent prijzen voor één single-serverexemplaar voor één regio. Denk eraan dat queryreplica's worden gefactureerd tegen hetzelfde tarief als de server. 

## <a name="built-on-sql-server-analysis-services"></a>Gebaseerd op SQL Server Analysis Services

Azure Analysis Services is compatibel met veel geweldige functies die al deel uitmaken van SQL Server Analysis Services Enterprise Edition. Azure Analysis Services ondersteunt tabellarische modellen met het [compatibiliteitsniveau](analysis-services-compat-level.md) 1200 of hoger. Tabellarische modellen zijn constructies voor relationele modellering (model, tabellen, kolommen), geformuleerd in tabellarische metagegevensobjectdefinities in TMSL- (Tabular Model Scripting Language) en TOM-code (Tabular Object Model. Partities, perspectieven, beveiliging op rijniveau, bidirectionele relaties en vertalingen worden allemaal ondersteund\*. Multidimensionale modellen en PowerPivot voor SharePoint *worden niet* ondersteund in Azure Analysis Services.

Tabellarische modellen in zowel in-memory- als DirectQuery-modi worden ondersteund. In de in-memory-modus (standaard) ondersteunen tabellarische modellen meerdere gegevensbronnen. Omdat modelgegevens in hoge mate worden gecomprimeerd en in het geheugen worden gecachet, biedt deze modus de snelste queryrespons op grote hoeveelheden gegevens. Deze modus biedt ook de meeste flexibiliteit voor complexe gegevenssets en query's. Partitionering maakt incrementele belastingen mogelijk, verhoogt de parallellisatie en vermindert het geheugenverbruik. Andere geavanceerde functies voor gegevensmodellering, zoals berekende tabellen, en alle DAX-functies worden ondersteund. Geheugenmodellen moeten worden vernieuwd (verwerkt) om gecachete gegevens uit gegevensbronnen bij te werken. Dankzij Azure-service-principal-ondersteuning bieden onbeheerde vernieuwingsbewerkingen met PowerShell, TOM, TMSL en REST flexibiliteit om ervoor te zorgen dat uw modelgegevens altijd up-to-date zijn. 

DirectQuery-modus* maakt gebruik van de relationele back-end-database voor opslag en query-uitvoering. Extreem grote gegevenssets in afzonderlijke SQL Server-, SQL Server Data Warehouse-, Azure SQL Database-, Azure SQL Data Warehouse-, Oracle- en Teradata-gegevensbronnen worden ondersteund. Backend-gegevenssets kunnen het beschikbare serverresourcesgeheugen overschrijden. Complexe scenario's voor het vernieuwen van gegevensmodellen zijn niet nodig. Er zijn ook enkele beperkingen, zoals beperkte gegevensbrontypen en DAX-formulebeperkingen, en enkele geavanceerde functies voor gegevensmodellering worden niet ondersteund. Zie [DirectQuery-modus](https://docs.microsoft.com/sql/analysis-services/tabular-models/directquery-mode-ssas-tabular)voordat u bepaalt wat voor u de beste modus is.

\* Welke functies beschikbaar zijn hangt af van de laag.

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen

Tabellarische modellen in Azure Analysis Services ondersteunen een breed scala aan gegevensbronnen, van eenvoudige tekstbestanden tot Big Data in Azure Data Lake Store. Raadpleeg voor meer informatie [Data sources supported in Azure Analysis Services](analysis-services-datasource.md) (Gegevensbronnen die in Azure Analysis Services worden ondersteund).

## <a name="compatibility-level"></a>Compatibiliteitsniveau

Compatibiliteitsniveau verwijst naar de release-specifieke gedrag in de Analysis Services-engine. Azure Analysis Services ondersteunt tabellaire modellen met het compatibiliteitsniveau 1200 en hoger. Zie voor meer informatie, [compatibiliteitsniveau voor tabellaire modellen van Analysis Services](analysis-services-compat-level.md).


## <a name="your-data-is-secure"></a>Gegevens zijn beveiligd

Azure Analysis Services beveiligt uw gevoelige gegevens op meerdere niveaus. Als Azure-service is bij Analysis Services een **basisniveau** aan DDoS-aanvallen automatisch ingeschakeld als onderdeel van het Azure-platform. Zie voor meer informatie het [overzicht van de DDoS Protection-standaard](../virtual-network/ddos-protection-overview.md). 

Op serverniveau biedt Analysis Services firewall, Azure-verificatie, serverbeheerdersrollen en serverversleuteling. Op gegevensmodelniveau zorgen gebruikersrollen en beveiliging op rijniveau en objectniveau ervoor dat uw gegevens veilig zijn en alleen worden gezien door de gebruikers voor wie ze bestemd zijn.

### <a name="firewall"></a>Firewall

De firewall van Azure Analysis Services blokkeert alle clientverbindingen behalve die met IP-adressen die zijn opgegeven in regels. Firewall-beveiliging is standaard niet ingeschakeld voor nieuwe servers. Het verdient aanbeveling om onmiddellijk nadat de server is gemaakt de firewallbeveiliging in te schakelen en de regels te configureren als onderdeel van een serverinrichtingsscript of in de portal. Configureer regels voor toegestane IP-adressen als individuele client-IP-adressen of als een bereik. Verbindingen van Power BI (service) kunnen ook worden toegestaan of geblokkeerd. Configureer de firewall en regels in de portal of met behulp van PowerShell. Zie [Configure a server firewall](analysis-services-qs-firewall.md) (een serverfirewall configureren) voor meer informatie.

### <a name="authentication"></a>Verificatie

Gebruikersverificatie wordt afgehandeld door [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md). Bij het inloggen gebruiken gebruikers de identiteit van een organisatie-account met op rollen gebaseerde toegang tot de database. Gebruikers-id's moeten lid zijn van de standaard Azure Active Directory voor het abonnement waarin de server is ondergebracht. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).

### <a name="data-security"></a>Gegevensbeveiliging

Azure Analysis Services maakt gebruik van Azure Blob-opslag om opslag en metagegevens voor Analysis Services-databases persistent te maken. Gegevensbestanden in Blob worden versleuteld met behulp van [Azure Blob Server Side Encryption (SSE)](../storage/common/storage-service-encryption.md). Wanneer de Direct Query-modus wordt gebruikt, worden alleen metagegevens opgeslagen. De werkelijke gegevens worden op het moment van uitvoering van de query via een versleuteld protocol opgehaald uit de gegevensbron.

Beveiligde toegang tot on-premises gegevens binnen uw organisatie wordt gerealiseerd door een [on-premises gegevensgateway](analysis-services-gateway.md) te installeren en te configureren. Gateways bieden toegang tot gegevens voor zowel de DirectQuery-modus als de in-memory-modus.

### <a name="roles"></a>Rollen

Analysis Services maakt gebruik van [op rollen gebaseerde autorisatie](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular) die toegang verleent tot server- en modeldatabasebewerkingen, -objecten en -gegevens. Alle gebruikers die toegang hebben tot een server of database, doen dit met hun Azure AD-gebruikersaccount binnen een toegewezen rol. De serverbeheerdersrol bevindt zich op serverresourceniveau. Standaard wordt het account dat wordt gebruikt bij het maken van een server automatisch opgenomen in de rol Serverbeheerder. Extra gebruikers en groepen worden toegevoegd met behulp van de portal, SSMS of PowerShell.
  
Eindgebruikers die geen beheerder zijn, krijgen toegang via databaserollen wanneer ze gegevens opvragen. Een databaserol wordt gemaakt als een afzonderlijk object in de database en is alleen van toepassing op de database waarin die rol is gemaakt. Databaserollen worden gedefinieerd door de machtigingen (database)Beheerder, Lezen, en Lezen en verwerken. Gebruikers- en groepsaccounts worden toegevoegd met behulp van SSMS of PowerShell.

### <a name="row-level-security"></a>Beveiliging op rijniveau

Tabellarische modellen op alle compatibiliteitsniveaus ondersteunen beveiliging op rijniveau. Beveiliging op rijniveau wordt geconfigureerd in het model door DAX-expressies te gebruiken die de rijen in een tabel definiëren, en alle rijen in de vele richtingen van een gerelateerde tabel die een gebruiker kan opvragen. Rijfilters met DAX-expressies worden gedefinieerd voor de machtigingen Lezen en Lezen en verwerken. 

### <a name="object-level-security"></a>Beveiliging op objectniveau 

Tabellarische modellen op het 1400-compatibiliteitsniveau ondersteunen beveiliging op objectniveau, waaronder beveiliging op paginaniveau en beveiliging op kolomniveau. Beveiliging op objectniveau wordt met behulp van TMSL of TOM ingesteld in de op JSON-gebaseerde metagegevens in het bestand Model.bim. Zie [Object-level security](https://docs.microsoft.com/sql/analysis-services/tabular-models/object-level-security) (beveiliging op objectniveau) voor meer informatie.

### <a name="automation-through-service-principals"></a>Automatisering via service-principals

Service-principals zijn een Azure Active Directory-toepassingsresource die u in uw tenant maakt om onbeheerde bewerkingen op resource- en serviceniveau uit te voeren. Service-principals worden gebruikt met Azure Automation, de onbeheerde PowerShell-modus, aangepaste clienttoepassingen en webapps om veelvoorkomende taken zoals gegevensvernieuwing, omhoog/omlaag schalen en onderbreken/hervatten te automatiseren. Machtigingen worden aan service-principals toegewezen via rollidmaatschappen. Zie [Automatisering met service-principals](analysis-services-service-principal.md) voor meer informatie.

### <a name="azure-governance"></a>Azure-governance

Azure Analysis Services is onderhevig aan de [gebruiksvoorwaarden van Microsoft Online Services](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement).
Ga naar het [Microsoft Vertrouwenscentrum](https://www.microsoft.com/trustcenter) voor meer informatie over Azure-beveiliging.

## <a name="use-the-tools-you-already-know"></a>Werken met de tools die u al kent

![Tools voor BI-ontwikkelaars](./media/analysis-services-overview/aas-overview-dev-tools.png)

### <a name="visual-studio"></a>Visual Studio

Ontwikkel en implementeer modellen met de gratis [SQL Server Data Tools (SSDT) voor Visual Studio](/sql/ssdt/download-sql-server-data-tools-ssdt). SSDT bevat Analysis Services-projectsjablonen waarmee u snel aan de slag kunt gaan. Bovendien biedt SSDT voor tabellaire modellen met compatibiliteitsniveau 1400 nu ook de innovatieve queryfunctie Get Data en mashup-functionaliteit. Als u al bekend bent met Get Data in Power BI Desktop en Excel 2016, weet u hoe eenvoudig het is om voor gegevensbronnen query's te maken die naadloos aansluiten bij uw behoeften. 

Als u Visual Studio 2017 of hoger, Microsoft Analysis Services-projecten beschikbaar als een gratis installeerbare VSIX-pakket zijn. [Downloaden van Marketplace](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftAnalysisServicesModelingProjects).

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

Servers en modeldatabases beheert u met behulp van [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Maak verbinding met uw servers in de cloud. Voer rechtstreeks vanuit het XMLA-queryvenster TMSL-scripts uit en automatiseer taken met behulp van TMSL-scripts en PowerShell. Omdat er in hoog tempo nieuwe functies en mogelijkheden worden toegevoegd, wordt SSMS maandelijks bijgewerkt.

### <a name="powershell"></a>PowerShell

Beheertaken voor serverresources, zoals het maken van serverresources, het onderbreken of hervatten van serverbewerkingen of het wijzigen van het serviceniveau (de servicelaag), maken gebruik van Azure PowerShell-cmdlets. Voor andere databasebeheertaken, zoals het toevoegen of verwijderen van roltoewijzingen, het toepassen van bewerkingen of het uitvoeren van TMSL-scripts, gebruikt u de cmdlets van de SQL Server-module. Zie [Azure Analysis Services beheren met PowerShell](analysis-services-powershell.md) voor meer informatie.

### <a name="object-model-and-scripting"></a>Objectmodel en scripts

De tabellaire modellen kunnen snel worden ontwikkeld en zijn in hoge mate aanpasbaar. Tabellarische modellen bevatten ook het [Tabular Object Model](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (TOM) om modelobjecten te beschrijven. TOM wordt in JSON weergegeven via [TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) en in de AMO Data Definition Language met behulp van de naamruimte [Microsoft.AnalysisServices.Tabular](/dotnet/api/microsoft.analysisservices.tabular). 

## <a name="supports-the-latest-client-tools"></a>Ondersteuning voor de nieuwste clienthulpprogramma's

![Gegevensvisualisaties](./media/analysis-services-overview/aas-overview-clients.png)

Moderne hulpprogramma's voor het verkennen en visualiseren van gegevens, zoals Power BI, Excel, Reporting Services en hulpprogramma's van derden, bieden gebruikers interactieve en visueel aantrekkelijke inzichten in de gegevens van uw model. 

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose

Azure Analysis Services is geïntegreerd met metrische gegevens van Azure en biedt een groot aantal resourcespecifieke metrische gegevens waarmee u de prestaties en de status van uw servers kunt volgen. Zie [Monitor server metrics](analysis-services-monitor.md) (Metrische servergegevens bewaken) voor meer informatie. Leg metrische gegevens vast met [diagnostische logboeken van Azure-resources](../azure-monitor/platform/diagnostic-logs-overview.md). Bewaak en verzend logbestanden naar [Azure Storage](https://azure.microsoft.com/services/storage/), stream ze naar [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)en exporteer ze naar [Azure Monitor-logboeken](https://azure.microsoft.com/services/log-analytics/), een service van [Azure](https://www.microsoft.com/cloud-platform/operations-management-suite). Zie [Registratie in diagnoselogboek instellen](analysis-services-logging.md) voor meer informatie.

Azure Analysis Services ondersteunt ook het gebruik van [DMV's (dynamische beheerweergaven)](https://docs.microsoft.com/sql/analysis-services/instances/use-dynamic-management-views-dmvs-to-monitor-analysis-services). DMV’s zijn gebaseerd op SQL-syntaxis en werken samen met schemarijensets die metagegevens en bewakingsgegevens over serverexemplaren retourneren.

## <a name="documentation"></a>Documentatie

Specifieke documentatie bij Azure Analysis Services is hier opgenomen. Gebruik de inhoudsopgave aan de linkerkant van uw browserscherm om artikelen te zoeken. 

Omdat Azure Analysis Services-modellen in tabelvorm bijna hetzelfde als tabellaire modellen in SQL Server Analysis Services zijn, er is een uitgebreide bibliotheek met gedeelde overzichtsinformatie, procedures, developer en naslaginformatie in [SQL Server Analysis Services Documentatie](https://docs.microsoft.com/sql/analysis-services/analysis-services). Bij artikelen in de documentatie van SQL Server Analysis Services wordt aangegeven of ze ook van toepassing zijn op Azure Analysis Services door de banner VAN TOEPASSING OP onder de titel.

![Gedeelde documentatie](./media/analysis-services-overview/aas-overview-applies-to.png)

### <a name="contribute"></a>U kunt bijdragen!

Analysis Services-documentatie, zoals dit artikel, is open source. Als u een GitHub-account hebt, kunt u een artikel bewerken door op Edit (potlood) in de rechterbovenhoek van uw browserscherm te klikken. Gebruik de editor in de browser en klik vervolgens op Propose file change (bestandswijziging voorstellen). 

![Gedeelde documentatie](./media/analysis-services-overview/aas-overview-edit.png)

Uw bijdrage wordt beoordeeld door het documentatieteam en indien deze wordt goedgekeurd, wordt de naam van uw GitHub-account weergegeven als bijdrager. Zie de [handleiding voor bijdragers aan documenten](https://docs.microsoft.com/contribute/) voor meer informatie.

In de documentatie van Azure Analysis Services wordt ook gebruikgemaakt van [GitHub Issues](https://docs.microsoft.com/teamblog/a-new-feedback-system-is-coming-to-docs). U kunt feedback geven over het product of de documentatie. Gebruik **Feedback** onderaan een artikel. GitHub Issues zijn nog niet ingeschakeld voor SQL Server Analysis Services-documentatie. 

## <a name="blogs"></a>Blogs

Er veranderen nog veel dingen. Krijg de meest recente informatie in het [blog van het Analysis Services-team](https://blogs.msdn.microsoft.com/analysisservices/) en in het [Azure-blog](https://azure.microsoft.com/blog/).

## <a name="community"></a>Community

Analysis Services heeft een zeer actieve gebruikerscommunity. Doe mee op [het Azure Analysis Services-forum](https://aka.ms/azureanalysisservicesforum).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meld u aan voor een gratis proefversie van Azure](https://azure.microsoft.com/offers/ms-azr-0044p/)   

> [!div class="nextstepaction"]
> [Snelstartgids: Een server maken - portal](analysis-services-create-server.md)   

> [!div class="nextstepaction"]
> [Snelstartgids: Een server maken - PowerShell](analysis-services-create-powershell.md)  
