---
title: Technische vereisten voor het maken van een Service van gegevens voor de Marketplace | Microsoft Docs
description: Begrip van de vereisten voor het maken van een gegevensservice om te implementeren en te verkopen op Azure Marketplace
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: aaff609a-1cd1-4146-98f4-d04166b0fce0
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 52827723477677bc292c645e2390c435fbad3ee4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Technische vereisten voor het maken van een gegevensservice bieden voor Azure Marketplace
> [!IMPORTANT]
> **Op dit moment wordt niet langer zijn voorbereiden op alle nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet goedgekeurd voor de aanbieding.** Als u een SaaS business-toepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u beschikt over een IaaS-toepassingen of developer-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Het proces zorgvuldig door voordat u begint lees en begrijp waar en waarom elke stap wordt uitgevoerd. Zo veel mogelijk, u moet voorbereiden van uw bedrijfsgegevens en andere gegevens, vereiste hulpprogramma's downloaden en/of technische onderdelen maken voordat u begint met een proces voor het maken van de aanbieding.

U hebt de volgende items gereed is voordat u begint met het proces:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Een beslissing over welke technologie wordt gebruikt voor het publiceren van uw Data-Service-aanbieding maken
Een uitgever kunt kiezen tussen meerdere technologieën bij het publiceren van Data-Service in Azure Marketplace. De belangrijkste technologieën die worden ondersteund die hieronder worden beschreven. Ongeacht welke technologie wordt gebruikt voor het publiceren van de Data-Service, de eindgebruiker maakt gebruik van de gegevens via de **OData-feed** die worden weergegeven door de Service voor Azure Marketplace. Gedetailleerde informatie over het OData-service die u kunt vinden op [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure Database
Dataset gereed in SQL Azure met is de verantwoordelijkheid van de uitgever. U moet zich abonneert op Azure, het inrichten van de juiste grootte van de Database en uw gegevens uploaden naar SQL Azure DB. Uitgever is tevens verantwoordelijk stelt zijn/haar gegevens altijd up-to-date kunt houden. Meer informatie over het abonnement op Azure-Services die u kunt vinden op [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)

Voor informatie over het verplaatsen van de gegevens naar Azure SQL Azure Marketplace kunnen worden blootgesteld tabellen en weergaven. De Publisher kunt opgeven welke tabellen/weergaven en kolommen worden blootgesteld aan de eindgebruiker. Provider van de inhoud kunt verder ook opgeven welke kolommen kunnen worden opgevraagd met de eindgebruiker en welke alleen worden geretourneerd in de nettolading. Hiermee geeft u een hoge mate van flexibiliteit over welke gegevens in de database moet worden weergegeven. Kolommen die kunnen worden opgevraagd moeten worden ondersteund door een of meer database-indexen.

## <a name="rest-based-web-service"></a>Op basis van REST-webservice
Ondersteund protocol: **alleen HTTPS**

Bestaande op basis van REST-services kunnen worden blootgesteld via de Azure Marketplace. Omdat de gegevensset wordt altijd blootgesteld aan de eindgebruiker als een OData-feed, basis de behoeften van de service Azure Marketplace kunnen de service toewijzen aan een OData service. Hiervoor eindpunten op basis van de REST nodig om alle parameters als HTTP-parameters weer te geven.

De nettolading moet zich in een formulier dat kan worden toegewezen aan een ATOM-antwoord. Daarom het antwoord van de services moet zich in de XML-indeling en kan slechts één herhalende element met de waarden van de nettolading (zoals Recordset) bevatten. De Azure Marketplace-service wordt het herhalende knooppunt naar het knooppunt vermelding in ATOM en de waarden nettolading toewijzen aan eigenschap knooppunten in het knooppunt vermelding.

Autorisatie-informatie (zoals API key, verificatie token, enz.) moet worden opgegeven als een HTTP-parameter of in de HTTP-header (sleutel-waardepaar) – basisverificatie wordt ook ondersteund. Een geldige sleutel moet worden opgegeven en alle aanvragen via Azure Marketplace worden gesteld via deze sleutel. Gebruiker bewaking en facturering gebeurt op de laag van Azure Marketplace.

Fouten geretourneerd door de service moeten worden toegewezen in HTTP-statuscodes. Als de service retourneert een XML-bestand dat de fout bevat gaat deze door de service Azure Marketplace voor HTTP-statuscodes worden toegewezen.

## <a name="soap-based-web-services"></a>Op basis van SOAP-webservices
Protocol: **alleen HTTPS**

De vereisten zijn dat hetzelfde als in de REST gebaseerde service sectie. Het enige verschil is dat parameters kunnen ook worden opgegeven in een XML-hoofdtekst die naar de Publisher-service wordt geboekt met elk verzoek via Azure Marketplace. Dit betekent dat de gebruiker op de front geeft-HTTP-parameters worden wordt omgezet in XML-elementen van een XML-document dat aan het verzoek om de inhoud provider-webservice worden verzonden.

## <a name="odata-based-web-services"></a>Op basis van OData-webservices
Protocol: **alleen HTTPS**

Gegevens kunnen worden weergegeven als een OData-service naar Azure Marketplace. Het systeem om door te geven van de service via gaat en vervangt de hoofdmap van de service met de hoofdmap van een Azure Marketplace service – om te controleren of dat alle volgende aanroepen gaat via Azure Marketplace.

OData-services hoeft alleen te gaan op basis van een database in de back-end. OData biedt ondersteuning voor elk type opslag of de bedrijfslogica voor het station van de service.

## <a name="next-steps"></a>Volgende stappen
Nu dat u gecontroleerd aan de vereisten en de benodigde taken uitgevoerd, u kunt verder gaan met het maken van uw aanbieding gegevensservice zoals beschreven in de [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).

Of, als u controleren van het algehele proces en de bijbehorende artikelen voor elk van de publicatie fasen wilt, gaat u naar het artikel [aan de slag: hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
