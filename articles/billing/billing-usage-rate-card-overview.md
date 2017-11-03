---
title: Azure API's voor facturering | Microsoft Docs
description: Meer informatie over het gebruik van Azure facturering en RateCard APIs's die worden gebruikt voor het bieden van inzicht in Azure brongebruik en trends.
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 10/9/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: 26217d6f4e14166a89fbb561cb12d0af78ae6f4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-billing-apis-to-programmatically-get-insight-into-your-azure-usage"></a>Azure Billing-API's gebruiken om u te programmatisch inzicht verkrijgen in uw Azure-gebruik
Azure Billing-API's gebruikt om pull gebruiks- en gegevens bij uw voorkeur hulpprogramma's voor gegevensanalyse. Het gebruik van Azure-bronnen en RateCard APIs kunt u nauwkeurige voorspellen en beheren van uw kosten. De API's worden geïmplementeerd als een Resource Provider en een deel van de reeks API's beschikbaar gesteld door de Azure Resource Manager.  

## <a name="azure-invoice-download-api-preview"></a>Azure-factuur downloaden API (Preview)
Eenmaal de [opt-in is voltooid](billing-manage-access.md#opt-in), download facturen met de preview-versie van [factuur API](/rest/api/billing). De functies:

* **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot krijgen kunnen de gebruiksgegevens van abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement.
* **Datum filteren** -gebruik de `$filter` parameter alle facturen in omgekeerde volgorde ophalen door de einddatum van de factuur. 

> [!NOTE]
> Deze functie is in de eerste versie van de preview en gelden neerwaarts compatibele wijzigingen mogelijk. Op dit moment is is het niet beschikbaar voor bepaalde abonnement aanbiedingen (EA, CSP, AIO niet ondersteund) en Duitse Azure.

## <a name="azure-resource-usage-api-preview"></a>Azure-Resource gebruiks-API (Preview)
Gebruik de Azure [Resource gebruik API](https://msdn.microsoft.com/library/azure/mt219003) om uw gegevens geschatte Azure-verbruik. De API bevat:

* **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot krijgen kunnen de gebruiksgegevens van abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement.
* **Elk uur of dagelijkse samenvoegingen** - aanroepfuncties kunnen opgeven of ze hun Azure gebruiksgegevens wilt elk uur tijdsintervallen of dagelijks tijdsintervallen. De standaardwaarde is dagelijks.
* **Metagegevens van het exemplaar (inclusief resourcetags)** – ophalen op exemplaarniveau details, zoals de volledig gekwalificeerde resource-uri (/subscriptions/ {abonnement-id} /..), wordt de informatie over de resource en resourcetags. Deze metagegevens kunt u deterministisch en programmatisch gebruik toewijzen door de labels voor use cases als cross in rekening gebracht.
* **Bron-metagegevens** -Resourcedetails zoals de naam van de meter, meter categorie meter subcategorie, eenheid en regio geven de aanroeper een beter inzicht in wat is verbruikt. We ook proberen om te worden uitgelijnd resource metagegevens terminologie via de Azure-portal, Azure gebruik CSV, EA facturering CSV en andere ervaringen openbare waarmee u correleren van gegevens over ervaringen.
* **Gebruik voor andere aanbiedingstypen** – gebruiksgegevens is beschikbaar voor aanbiedingstypen zoals betalen naar gebruik, MSDN, bedrag, financieel tegoed en EA, met uitzondering van [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-invoice#retrieve-usage-data-for-a-specific-subscription).

## <a name="azure-resource-ratecard-api-preview"></a>Azure-Resource RateCard API (Preview)
Gebruik de [API van Azure Resource RateCard](https://msdn.microsoft.com/library/azure/mt219005) ophalen van de lijst met beschikbare Azure-resources en informatie over de geschatte prijzen voor elk. De API bevat:

* **Azure op rollen gebaseerd toegangsbeheer** -uw beleidsregels configureren op de [Azure-portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot krijgen kunnen de gegevens RateCard. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer, de eigenaar of bijdrager rol voor toegang tot de gebruiksgegevens voor een bepaald Azure-abonnement.
* **Ondersteuning voor betalen per gebruik, MSDN, bedrag en financieel tegoed aanbiedingen (EA en [CSP](https://docs.microsoft.com/azure/cloud-solution-provider/billing/azure-csp-pricelist#get-prices-by-using-the-azure-rate-card) niet ondersteund)** -voor deze API biedt Azure-aanbieding niveau snelheid informatie.  De aanroeper van deze API moet in de aanbieding informatie ophalen van de resourcedetails van de en tarieven doorgeven. We zijn momenteel niet mogelijk om de EA omdat EA aanbiedingen tarieven per inschrijving hebt aangepast. 

## <a name="scenarios"></a>Scenario's
Hier volgen enkele van de scenario's die zijn aangebracht met de combinatie van het gebruik en de APIs RateCard mogelijk:

* **Azure besteden tijdens de maand** - gebruiken om de combinatie van het gebruik en RateCard APIs ophalen beter inzicht in uw cloud-uitgaven in de maand. U kunt de uur- en buckets van gebruiks- en kosten maakt een schatting analyseren.
* **Stel waarschuwingen** – het gebruik en de APIs RateCard gebruiken voor het ophalen van de geschatte cloud verbruik en de kosten en resource of monetaire gebaseerde waarschuwingen instellen.
* **Factuur voorspellen** – Get uw geschatte gebruiks- en cloud besteden en machine learning-algoritmen om te voorspellen wat de factuur aan het einde van de factureringscyclus zou zijn van toepassing.
* **Vooraf verbruik kosten analysis-** – de RateCard-API gebruiken om te voorspellen hoeveel uw factuur voor niet het verwachte gebruik wanneer u uw workloads naar Azure verplaatst. Als u bestaande workloads in andere clouds of privéclouds hebt, kunt u ook uw gebruik met de Azure toewijzen te besteden aan de tarieven voor een betere schatting van Azure ophalen. Deze schatting biedt u de mogelijkheid om terug te draaien op aanbieding, en vergelijken en contrast tussen de verschillende aanbiedingstypen dan betalen naar gebruik, zoals bedrag en financieel tegoed. De API biedt u de mogelijkheid om te zien kostenverschillen per regio ook en kunt u een wat-als-kostenanalyse waarmee u beslissingen voor de implementatie uitvoeren.
* **Wat-als-analyse** -
  
  * U kunt bepalen of het meest efficiënt werkbelastingen worden uitgevoerd in een andere regio of op een andere configuratie van de Azure-resource. Azure-resourcekosten kunnen verschillen op basis van de Azure-regio u.
  * U kunt ook bepalen als een ander Azure-aanbiedingtype resulteert in een betere rentabiliteit op een Azure-resource.
  
## <a name="partner-solutions"></a>Partneroplossingen
[Cloud Cruiser en Microsoft Azure Billing API-integratie](billing-usage-rate-card-partner-solution-cloudcruiser.md) wordt beschreven hoe [Cloud Cruiser van Express voor Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) werkt rechtstreeks vanuit de portal Windows Azure Pack (WAP). U kunt operationele en financiële aspecten van de Microsoft Azure privé of gehoste openbare cloud naadloos beheren vanuit één gebruikersinterface.   

## <a name="next-steps"></a>Volgende stappen
* Bekijk de codevoorbeelden op GitHub:
  * [Voorbeeld van de factuur API-code](https://go.microsoft.com/fwlink/?linkid=845124)

  * [Voorbeeld van gebruiks-API-code](https://github.com/Azure-Samples/billing-dotnet-usage-api)

  * [Voorbeeld van RateCard API-code](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)

* Zie voor meer informatie over de Azure Resource Manager, [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 

* Voor meer informatie over de suite met hulpprogramma's die nodig is om u aan een goed begrip van de cloud te besteden, Zie het artikel Gartner [markt-handleiding voor IT financiële Management (ITFM)-hulpprogramma's voor](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

