---
title: API's van Azure facturering | Microsoft Docs
description: Meer informatie over Azure Billing gebruiks- en RateCard APIs's die worden gebruikt voor het bieden van inzicht in het gebruik van Azure-resources en trends.
services: ''
documentationcenter: ''
author: tonguyen
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 5/10/2018
ms.author: erikre
ms.openlocfilehash: 79cc543e2a106fa8cbff31bdd0e6bb35afe68e4b
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392037"
---
# <a name="use-azure-billing-apis-to-programmatically-get-insight-into-your-azure-usage"></a>Azure facturerings-API's gebruiken om via een programma inzicht uw gebruik van Azure
Gebruik Azure facturering API's voor pull-gebruik en de resource-gegevens in uw favoriete hulpprogramma's voor gegevensanalyse. De Azure Resource Usage- en RateCard-API’s kunnen u helpen uw kosten nauwkeurig te voorspellen en te beheren. De API's worden geïmplementeerd als een Resource Provider en een deel van de API's beschikbaar gemaakt door de Azure Resource Manager-familie.  

> [!div class="nextstepaction"]
> [Help bij het verbeteren van documenten over Azure-facturering](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="azure-invoice-download-api-preview"></a>API voor Azure-facturen downloaden (Preview)
Zodra de [aanmelden is voltooid](billing-manage-access.md#opt-in), downloaden van facturen met behulp van de preview-versie van [factuur API](/rest/api/billing). De functies zijn onder andere:

* **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot krijgen kunnen de gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement.
* **Datum filteren** -gebruik de `$filter` parameter om op te halen van alle facturen in omgekeerde volgorde door de einddatum van de factuur. 

> [!NOTE]
> Deze functie is in eerste versie van de Preview-versie en zijn mogelijk onderhevig aan wijzigingen achterwaarts compatibel. Het is momenteel niet beschikbaar voor bepaalde abonnementaanbiedingen (EA, CSP, AIO niet ondersteund) en Azure Duitsland.

## <a name="azure-resource-usage-api-preview"></a>Azure Resourcegebruik-API (Preview)
Gebruik de Azure [Resource Usage API](https://msdn.microsoft.com/library/azure/mt219003) om uw geschatte Azure-verbruik-gegevens. De API omvat:

* **Azure Role-based Access Control** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot krijgen kunnen de gegevens over het gebruik van het abonnement. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement.
* **Elk uur of dagelijks aggregaties** - aanroepers kunnen opgeven of ze hun gebruik van Azure-gegevens wilt per uur duurbuckets of dagelijks tijdsintervallen. De standaardwaarde is dagelijks.
* **Metagegevens van het exemplaar (met inbegrip van resourcetags)** – informatie zoals de volledig gekwalificeerde resource-uri op exemplaarniveau krijgen (/subscriptions/ {abonnement-id} /...), wordt de informatie over resource en resourcetags. Deze metagegevens kunt u via een programma heel deterministisch en gebruik toewijzen door de labels voor use cases als cross-kosten in rekening gebracht.
* **De resource metagegevens** -Resourcedetails zoals meternaam, metercategorie, subcategorie van de meter, eenheid en regio geven de aanroeper een beter begrip van wat is verbruikt. We werken ook om te worden uitgelijnd resource metagegevens terminologie voor Azure portal, Azure-gebruik CSV, EA facturering CSV en andere openbare-ervaringen, zodat u gegevens correleren met ervaringen.
* **Gebruik voor andere typen** – gebruiksgegevens is beschikbaar voor de aanbiedingstypen zoals betalen per gebruik, MSDN, monetaire toezegging, financieel tegoed en EA, met uitzondering van [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-invoice#retrieve-usage-data-for-a-specific-subscription).

## <a name="azure-resource-ratecard-api-preview"></a>Azure-Resource RateCard API (Preview)
Gebruik de [RateCard API van Azure Resource](https://msdn.microsoft.com/library/azure/mt219005) om op te halen van de lijst met beschikbare Azure-resources en de geschatte informatie over de prijzen voor elk. De API omvat:

* **Azure Role-based Access Control** -het-beleid configureren op de [Azure-portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot krijgen kunnen de RateCard-gegevens. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de lezer, de eigenaar of Inzender rol toegang krijgen tot gegevens over gebruik voor een bepaald Azure-abonnement.
* **Ondersteuning voor betalen per gebruik, MSDN, monetaire toezegging en monetaire creditaanbiedingen (EA en [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-pricelist#get-prices-by-using-the-azure-rate-card) niet ondersteund)** -deze API biedt Azure-aanbieding op serverniveau snelheid informatie.  De aanroeper van deze API moet doorgeven in de informatie over de aanbieding om details van de resource en tarieven te verkrijgen. We zijn momenteel niet voor EA-tarieven omdat EA-aanbiedingen tarieven per inschrijving hebt aangepast. 

## <a name="scenarios"></a>Scenario's
Hier volgen enkele van de scenario's die zijn aangebracht mogelijk is met de combinatie van het gebruik en de APIs RateCard:

* **Azure-uitgaven gedurende de maand** - gebruik van de combinatie van het gebruik en RateCard-APIs beter inzicht krijgt in uw cloud uitgaven gedurende de maand. U kunt de buckets per uur en dagelijkse gebruik en kosten schattingen analyseren.
* **Waarschuwingen instellen** : het gebruik en de APIs RateCard gebruiken voor het ophalen van de geschatte cloudverbruik en kosten in rekening gebracht en waarschuwingen op basis van een resource of monetaire op basis van instellen.
* **Factuur voorspellen** – Get uw geschatte verbruik en de cloud besteden en machine learning-algoritmen om te voorspellen wat de factuur is aan het einde van de factureringscyclus van toepassing.
* **Vooraf verbruikskosten analysis** – de RateCard API gebruiken om te voorspellen hoeveel uw factuur zou zijn voor uw verwachte gebruik wanneer u uw werkbelastingen naar Azure verplaatsen. Als u bestaande workloads in andere clouds en privéclouds hebt, kunt u ook uw gebruik met de Azure toewijzen tarieven voor een betere schatting van de Azure-uitgaven. Deze schatting biedt u de mogelijkheid om terug te draaien op de aanbieding, en vergelijk en Maak onderscheid tussen de verschillende aanbiedingstypen dan betalen per gebruik, zoals monetaire toezegging en financieel tegoed. De API is ook biedt u de mogelijkheid om te zien van kostenverschillen per regio en kunt u een wat-als kostenanalyse zodat u implementatie beslissingen kunt komen.
* **Wat als-analyse** -
  
  * U kunt bepalen of deze rendabeler voor het uitvoeren van workloads in een andere regio, of op een andere configuratie van de Azure-resource is. Kosten voor Azure-resource kunnen verschillen op basis van de Azure-regio u.
  * U kunt ook bepalen als een ander Azure-aanbiedingtype resulteert in een betere rentabiliteit op een Azure-resource.
  
## <a name="partner-solutions"></a>Partneroplossingen
In [Cloud Cruiser en Microsoft Azure Billing API-integratie](billing-usage-rate-card-partner-solution-cloudcruiser.md) wordt beschreven hoe [Cloud Cruiser van Express voor Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) rechtstreeks vanuit de WAP-portal (Windows Azure Pack) werkt. U kunt de operationele en financiële aspecten van de openbare of gehoste openbare cloud van Microsoft Azure naadloos beheren vanuit één gebruikersinterface.   

## <a name="next-steps"></a>Volgende stappen
* Bekijk de codevoorbeelden op GitHub:
  * [Codevoorbeeld Invoice API](https://go.microsoft.com/fwlink/?linkid=845124)

  * [Codevoorbeeld Usage API](https://github.com/Azure-Samples/billing-dotnet-usage-api)

  * [Codevoorbeeld RateCard API](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)

* Zie voor meer informatie over de Azure Resource Manager, [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 



