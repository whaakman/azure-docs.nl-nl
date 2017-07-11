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
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 34726377836d00d484ca01edb098f6c7cbfa9dbf
ms.contentlocale: nl-nl
ms.lasthandoff: 06/17/2017


---
<a id="what-is-azure-analysis-services" class="xliff"></a>

# Wat is Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services is ontworpen op basis van de bewezen analyse-engine in Microsoft SQL Server Analysis Services en levert hoogwaardige gegevensmodellen in de cloud. 

Bekijk deze video om te zien hoe Azure Analysis Services aansluit bij de overige BI-voorzieningen van Microsoft en waarom het zinvol is om uw gegevensmodellen over te brengen naar de cloud.


>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>


<a id="built-on-sql-server-analysis-services" class="xliff"></a>

## Gebaseerd op SQL Server Analysis Services
Azure Analysis Services is compatibel met de voor u bekende SQL Server Analysis Services Enterprise Edition. Azure Analysis Services ondersteunt tabellaire modellen met het compatibiliteitsniveau 1200 of hoger. DirectQuery, partities, beveiliging op rijniveau, bidirectionele relaties en vertalingen worden allemaal ondersteund.

<a id="use-the-tools-you-already-know" class="xliff"></a>

## Werken met de tools die u al kent
![Tools voor BI-ontwikkelaars](./media/analysis-services-overview/aas-overview-dev-tools.png)

Wanneer u gegevensmodellen maakt voor Azure Analysis Services, gebruikt u dezelfde tools als voor SQL Server Analysis Services. U ontwerpt en implementeert modellen met behulp van [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx). Servers en modeldatabases beheert u met behulp van [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Het automatiseren van taken kan met sjablonen van [PowerShell](analysis-services-powershell.md) en [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 

<a id="supports-the-latest-features" class="xliff"></a>

## Ondersteuning voor de nieuwste functies
Azure Analysis Services ondersteunt tabellaire modellen met het compatibiliteitsniveau 1200 en 1400 Preview.

**Tabular 1200**: het niveau 1200 is voor het eerst opgenomen in SQL Server 2016 Analysis Services en introduceert TOM (Tabular Object Model) voor het beschrijven van modelobjecten zoals tabellen, kolommen en relaties. TOM wordt in JSON weergegeven via [TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) en in de AMO Data Definition Language met behulp van de naamruimte [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx).

**Tabular 1400 Preview**: introduceert ondersteuning voor detailrijen, beveiliging op objectniveau, ondersteuning voor een onregelmatige hiërarchie, een moderne Get Data-ervaring voor toegang tot gegevens en veel andere verbeteringen. U moet de nieuwste versie van [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) gebruiken om te beschikken over al deze functionaliteit. Omdat Tabular 1400 nog steeds in Preview is, veranderen er nog steeds dingen. Bekijk ons [blogbericht](https://azure.microsoft.com/blog/1400-models-in-azure-as/) voor de meest actuele informatie.

<a id="data-sources" class="xliff"></a>

## Gegevensbronnen
Gegevensmodellen die zijn geïmplementeerd op servers in Azure maken het mogelijk om verbinding te maken met gegevensbronnen die zich on-premises in uw organisatie bevinden of in de cloud. Combineer gegevens uit zowel on-premises als in de cloud opgeslagen gegevensbronnen voor een hybride BI-oplossing.

Omdat de server zich in de cloud bevindt, kan er naadloos verbinding worden gemaakt met gegevensbronnen in de cloud. Bij het verbinden met on-premises gegevensbronnen zorgt de [on-premises gegevensgateway](analysis-services-gateway.md) voor snelle en veilige verbindingen met de server in de cloud. Zie [Ondersteunde gegevensbronnen in Azure Analysis Services](analysis-services-datasource.md) voor meer informatie over welke on-premises gegevensbronnen worden ondersteund.


<a id="explore-your-data-from-anywhere" class="xliff"></a>

## Uw gegevens vanaf elke locatie toegankelijk
U kunt vanaf elke locatie verbinding maken met uw servers en gegevens ophalen. Azure Analysis Services ondersteunt verbindingen vanuit Power BI Desktop, Excel, maatwerk-apps en browser-gebaseerde hulpprogramma's.

![Gegevensvisualisaties](./media/analysis-services-overview/aas-overview-visualization.png)


<a id="secure" class="xliff"></a>

## Beveiligen
<a id="user-authentication" class="xliff"></a>

#### Gebruikersverificatie
Gebruikersverificatie voor Azure Analysis services wordt afgehandeld door [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Wanneer gebruikers zich willen aanmelden bij een Azure Analysis Services-database, moeten ze de id van een organisatie-account gebruiken die toegang heeft tot de database die ze willen raadplegen. Deze gebruikers-id's moeten lid zijn van de standaard Azure Active Directory voor het abonnement waarin de Azure Analysis Services-server is ondergebracht. [Directory-integratie](https://technet.microsoft.com/library/jj573653.aspx) tussen AAD en on-premises Active Directory is een uitstekende manier om uw gebruikers toegang te geven tot een Azure Analysis Services-database, maar is niet voor alle scenario's vereist.

Gebruikers melden zich aan met de UPN (User Principal Name) van hun eigen account en met hun wachtwoord. Bij synchronisatie met een on-premises Active Directory, bestaat de UPN van de gebruiker vaak uit hun -e-mailadres binnen de organisatie.

Machtigingen voor het beheren van de Azure Analysis Services-server-resource worden afgehandeld door het toewijzen van gebruikers aan rollen binnen uw Azure-abonnement. De standaardinstelling is dat abonnementbeheerders eigenaarsmachtigingen hebben voor de server-resource in Azure. Extra gebruikers kunnen worden toegevoegd met behulp van Azure Resource Manager.

<a id="data-security" class="xliff"></a>

#### Gegevensbeveiliging
Azure Analysis Services maakt gebruik van Azure Blob-opslag om opslag en metagegevens voor Analysis Services-databases persistent te maken. Gegevensbestanden in Blob worden versleuteld met behulp van Azure Blob Server Side Encryption (SSE). Wanneer de Direct Query-modus wordt gebruikt, worden alleen metagegevens opgeslagen. De werkelijke gegevens worden op het moment van uitvoering van de query opgehaald uit de gegevensbron.

<a id="on-premises-data-sources" class="xliff"></a>

#### On-premises gegevensbronnen
Beveiligde toegang tot gegevens die on-premises zijn opgeslagen in uw organisatie is mogelijk door het installeren en configureren van een [on-premises gegevensgateway](analysis-services-gateway.md). Gateways bieden toegang tot gegevens voor zowel de Direct Query-modus als de modus In-memory. Wanneer een Azure Analysis Services-model verbinding maakt met een on-premises gegevensbron, wordt er een query gemaakt die ook de versleutelde referenties voor de on-premises gegevensbron bevat. De gateway-cloudservice analyseert de query en verstuurt de aanvraag naar een Azure Service Bus. De on-premises gateway pollt de Azure Service Bus op aanvragen die in behandeling zijn. De gateway ontvangt vervolgens de query, ontsleutelt de referenties en maakt verbinding met de gegevensbron voor uitvoering. De resultaten worden vervolgens vanuit de gegevensbron verzonden naar de gateway en vervolgens verder naar de Azure Analysis Services-database.

Azure Analysis Services is onderhevig aan de [gebruiksvoorwaarden van Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [privacyverklaring van Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Ga naar het [Microsoft Vertrouwenscentrum](https://www.microsoft.com/trustcenter/Security/AzureSecurity) voor meer informatie over Azure-beveiliging.

<a id="get-help" class="xliff"></a>

## Help opvragen

<a id="documentation" class="xliff"></a>

### Documentatie
Azure Analysis Services is eenvoudig in te stellen en te beheren. U kunt hier alle informatie vinden die u nodig hebt om een server te maken en te beheren. Wanneer u een gegevensmodel gaat maken voor implementatie op uw server, is de procedure grotendeels hetzelfde als bij het maken van een gegevensmodel dat u implementeert op een on-premises server. Er is een uitgebreide bibliotheek met overzichtsinformatie, procedures, zelfstudies en naslagartikelen beschikbaar op [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services).

<a id="videos" class="xliff"></a>

### Video's
Bekijk nuttige video's over [Azure Analysis Services op Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

<a id="blogs" class="xliff"></a>

### Blogs
Er veranderen nog veel dingen. U krijgt altijd de meest recente informatie in het [blog van het Analysis Services-team](https://blogs.msdn.microsoft.com/analysisservices/) en in het [Azure-blog](https://azure.microsoft.com/blog/).

<a id="community" class="xliff"></a>

### Community
Analysis Services heeft een zeer actieve gebruikerscommunity. Doe mee op [het Azure Analysis Services-forum](https://aka.ms/azureanalysisservicesforum).

<a id="feedback" class="xliff"></a>

## Feedback
Hebt u suggesties of functieverzoeken? Uw [feedback voor Azure Analysis Services](https://aka.ms/azureanalysisservicesfeedback) wordt zeer op prijs gesteld.

Hebt u suggesties over de documentatie? U kunt met behulp van Livefyre onderaan elk artikel opmerkingen toevoegen.

<a id="next-steps" class="xliff"></a>

## Volgende stappen
U weet nu in grote lijnen wat Azure Analysis Services is en dus is het tijd om echt aan de slag te gaan. Lees hier meer over het [maken van een server](analysis-services-create-server.md) in Azure en het [implementeren van een tabellair model op de server](analysis-services-deploy.md).

