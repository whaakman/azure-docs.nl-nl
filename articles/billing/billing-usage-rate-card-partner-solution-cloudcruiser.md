---
title: Cloud Cruiser en Microsoft Azure Billing API-integratie | Microsoft Docs
description: "Biedt een unieke perspectief van Microsoft Azure Billing partner Cruiser Cloud, op hun ervaringen van de Azure Billing API's integreren in hun product.  Dit is vooral nuttig voor Azure en Cloud Cruiser klanten die zijn geïnteresseerd zijn in gebruik/probeert Cloud Cruiser voor Microsoft Azure-pakket."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 10/09/2017
ms.author: mobandyo;sirishap;bryanla
ms.openlocfilehash: 7d66cac98afa72c807f597403b1e2bd278e45cec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser en Microsoft Azure Billing API-integratie
Dit artikel wordt beschreven hoe de gegevens van de nieuwe Microsoft Azure Billing-API's kan worden gebruikt in de Cloud Cruiser voor werkstroom kosten simulatie en analyse.

## <a name="azure-ratecard-api"></a>Azure RateCard API
De API RateCard biedt informatie van de snelheid van Azure. Na verificatie met de juiste referenties opgeeft, kunt u een query de API voor het verzamelen van metagegevens over de beschikbare services in Azure, samen met de tarieven van uw bieden.

Het volgende voorbeeldantwoord is van de API met de prijzen voor de A0 (Windows)-exemplaar:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cloud Cruiser van Interface naar Azure RateCard API
Cloud Cruiser kunt u de informatie RateCard API gebruiken op verschillende manieren. Voor dit artikel wordt gedemonstreerd hoe deze kan worden gebruikt om ervoor te IaaS werkbelasting kosten simulatie en -analyse.

Stel dat een werkbelasting van meerdere instanties die actief zijn op Microsoft Azure Pack (WAP) ter illustratie van deze gebruiksvoorbeeld. Het doel is om te simuleren van deze dezelfde werkbelasting op Azure en schat de kosten van dergelijke migratie uitvoeren. Om te kunnen maken van deze simulatie, zijn er twee belangrijke taken worden uitgevoerd:

1. **Importeren en verwerken van de API RateCard verzamelde gegevens van de service.** Deze taak wordt ook uitgevoerd op de werkmappen, waarbij het uitpakken van de API RateCard getransformeerd en gepubliceerd naar een nieuw plan voor snelheid. Dit nieuwe snelheid abonnement wordt gebruikt op de simulaties als indicatie van de prijzen van Azure.
2. **Normaliseren WAP-services en Azure IaaS-services.** Standaard WAP-services zijn gebaseerd op afzonderlijke resources (CPU, geheugen, schijfgrootte, enz.) bij Azure services zijn gebaseerd op de exemplaargrootte van het (A0, A1, A2, enzovoort). Deze eerste taak kan worden uitgevoerd door de Cloud Cruiser ETL-engine, aangeroepen werkmappen, waar deze resources kunnen worden gebundeld op exemplaar grootten, vergelijkbaar met het exemplaar van Azure-services.

### <a name="import-data-from-the-ratecard-api"></a>Gegevens importeren uit de RateCard-API
Cloud Cruiser werkmappen bieden een geautomatiseerde manier voor het verzamelen en informatie van de API RateCard verwerken.  Werkmappen voor ETL (uitpakken-transformatie-load) kunnen u de verzameling, transformatie en publiceren van gegevens naar de Cloud Cruiser-database configureren.

Elke werkmap, kan een of meerdere verzamelingen, zodat u gegevens uit verschillende bronnen naar een aanvulling vormen op of uitbreiding van de gebruiksgegevens correleren hebben. De volgende twee schermafbeeldingen laten zien hoe u een nieuwe *verzameling* in een bestaande werkmap en het importeren van gegevens in de *verzameling* van de API RateCard:

![Afbeelding 1: een nieuwe verzameling maken][1]

![Afbeelding 2: gegevens importeren uit de nieuwe verzameling][2]

Na het importeren van de gegevens in de werkmap is het mogelijk te maken van meerdere stappen en processen voor transformatie, om te wijzigen en model van de gegevens. Bijvoorbeeld, aangezien we alleen geïnteresseerd in de infrastructuur-as-a-Service (IaaS bent) we kunnen de transformatie-stappen gebruiken om onnodige rijen te verwijderen of records, gerelateerd aan services dan IaaS.

De volgende schermafbeelding ziet de transformatie stappen voor het verwerken van de gegevens die worden verzameld van RateCard API:

![Afbeelding 3 - transformatie stappen voor het verwerken van de verzamelde gegevens van RateCard API][3]

### <a name="defining-new-services-and-rate-plans"></a>Plannen voor het definiëren van nieuwe Services en snelheid
Er zijn verschillende manieren voor het definiëren van services op Cloud Cruiser. Een van de opties is voor het importeren van de services van de gebruiksgegevens. Deze methode wordt doorgaans gebruikt bij het werken met openbare clouds, waarbij de services al zijn gedefinieerd door de provider.

Een Tariefplan is een reeks tarieven of prijzen die kunnen worden toegepast op verschillende services, op basis van ingangsdatums of groep aan klanten, onder andere opties. Tarieven kunnen ook worden gebruikt op Cloud Cruiser simulatie of 'Wat-als'-scenario's om te begrijpen hoe wijzigingen in services invloed hebben op de totale kosten van een werklast te maken.

In dit voorbeeld gebruiken we de service-informatie van de API RateCard voor het definiëren van nieuwe services in de Cloud Cruiser. We kunnen op dezelfde manier de tarieven tot de services gebruiken voor het maken van een nieuwe Tariefplan op Cloud Cruiser.

Aan het einde van het proces voor transformatie is het mogelijk te maken van een nieuwe stap en de gegevens van de API RateCard publiceren als nieuwe services en tarieven.

![Afbeelding 4: de gegevens van de API RateCard publiceren als een nieuwe Services en tarieven][4]

### <a name="verify-azure-services-and-rates"></a>Controleer of de Azure-Services en tarieven
Na het publiceren van de services en tarieven, kunt u controleren of de lijst met geïmporteerde services in de Cloud Cruiser *Services* tabblad:

![Afbeelding 5: de nieuwe Services verifiëren][5]

Op de *snelheid plannen* tabblad kunt u het nieuwe snelheid plan genaamd 'AzureSimulation' met de tarieven geïmporteerd uit de API RateCard controleren.

![Afbeelding 6 - verifiëren van de nieuwe Tariefplan en de bijbehorende tarieven][6]

### <a name="normalize-wap-and-azure-services"></a>Normaliseren WAP en Azure-Services
WAP biedt standaard gebruiksgegevens op basis van het gebruik van de berekenings-, geheugen- en netwerkbronnen. In de Cloud Cruiser, definieert u uw services op basis van rechtstreeks op de toewijzing of het gecontroleerde gebruik van deze bronnen. U kunt bijvoorbeeld een basic frequentie instellen voor elk uur van de CPU-gebruik of de GB geheugen toegewezen aan een exemplaar kosten in rekening gebracht.

In dit voorbeeld moet vergelijken kosten tussen WAP en Azure, voor het cumuleren van het gebruik van bronnen op WAP in bundels, die vervolgens kunnen worden toegewezen aan Azure-services. Deze transformatie kan gemakkelijk worden geïmplementeerd in de werkmappen:

![Afbeelding 7: transformeren WAP-gegevens voor het normaliseren van services][7]

De laatste stap in de werkmap is voor het publiceren van de gegevens in de Cloud Cruiser-database. De gebruiksgegevens is tijdens deze stap nu gebundeld in services (die zijn toegewezen aan de Azure-Services) en gekoppeld aan standaardtarieven voor het maken van de kosten.

Nadat de werkmap is voltooid, kunt u de verwerking van de gegevens automatiseren door een taak toevoegen op de scheduler en het opgeven van de frequentie en het tijdstip voor de werkmap om uit te voeren.

![Afbeelding 8 - werkmap plannen][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Rapporten voor werkbelasting kosten simulatie analyse maken
We kunnen nadat de gebruiksgegevens worden verzameld en kosten zijn geladen in de Cloud Cruiser-database, de Cloud Cruiser Insights-module voor het maken van de werkbelasting kosten simulatie die we willen gebruiken.

Om dit scenario demonstreren, wordt het volgende rapport gemaakt:

![Vergelijking van kosten][9]

De bovenste grafiek bevat een kostenvergelijking per services, vergelijken van de prijs van het uitvoeren van de werkbelasting voor elke specifieke service tussen WAP (donker blauw) en Azure (lichte blauw).

De grafiek onder toont dezelfde gegevens, maar uitgesplitst per afdeling. De kosten voor elke afdeling in voor het uitvoeren van hun werkbelasting op WAP en Azure, samen met het verschil tussen deze twee wordt weergegeven in de balk besparingen (groen).

## <a name="azure-usage-api"></a>Gebruik van Azure-API
### <a name="introduction"></a>Inleiding
Microsoft heeft onlangs geïntroduceerd voor de Azure-gebruik API, abonnees programmatisch ophalen van gebruiksgegevens inzicht in hun verbruik toestaan. Cloud Cruiser klanten kunnen profiteren van de rijkere gegevensset beschikbaar via deze API.

Cloud Cruiser kunt u de integratie met de API-gebruik op verschillende manieren gebruiken. De granulatie (elk uur gebruiksgegevens) en metagegevens broninformatie beschikbaar is via de API biedt de benodigde gegevensset ter ondersteuning van flexibele Showback of terugstorting modellen. 

In deze zelfstudie presenteren we een voorbeeld van hoe Cloud Cruiser van de informatie over het gebruik API-informatie profiteren kunt. Meer specifiek, wordt er een resourcegroep maken in Azure, tags voor de structuur van het account koppelen en beschrijven het proces van binnenhalen en verwerken van de tag-informatie in de Cloud Cruiser.

Het uiteindelijke doel is mogelijk te maken van rapporten, zoals de volgende en kunnen analyseren van kosten en het verbruik op basis van de account-structuur gevuld door de labels.

![Afbeelding 10 - rapport met storingen met tags][10]

### <a name="microsoft-azure-tags"></a>Labels van de Microsoft Azure
De gegevens beschikbaar zijn via de API voor het gebruik van Azure bevat niet alleen informatie over het verbruik, maar ook met inbegrip van alle codes die zijn gekoppeld aan het bron-metagegevens. Labels zijn een gemakkelijke manier om uw resources te organiseren, maar om te kunnen werken, moet u ervoor te zorgen dat:

* Labels zijn correct toegepast op de bronnen tijdens het inrichten
* Labels zijn correct van het proces Showback/verrekenen gebruikt om te koppelen van het gebruik van de organisatie-account structuur.

Beide van deze vereisten kunnen lastig zijn, vooral wanneer er is een handmatig proces over het inrichten of opladen van kant. Verkeerd getypt, onjuiste of zelfs ontbrekende tags zijn algemene klachten van klanten wanneer met labels en deze fouten moeilijk om levensduur van de zijde geladen maken kan.

Met de nieuwe Azure gebruik API Cloud Cruiser pull-labels resourcegegevens, en via een geavanceerde ETL hulpprogramma werkmappen aangeroepen, deze algemene tagging fouten zijn opgelost. Via de transformatie met behulp van reguliere expressies en correlatie van gegevens, Cloud Cruiser onjuist gelabelde bronnen identificeren en toepassen van de juiste labels, zodat de juiste koppeling van de resources met de consument.

Aan de geladen Cloud Cruiser automatiseert de Showback/verrekenen en de labelgegevens kunt gebruiken om te koppelen van het gebruik van de juiste gebruiker (afdeling, afdeling, Project, enzovoort). Deze automatisering biedt een enorme verbetering en kunt controleren of een consistente en controleerbare geladen proces.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Maken van een resourcegroep met labels op Microsoft Azure
De eerste stap in deze zelfstudie is het een resourcegroep maken in de Azure portal maakt u nieuwe labels te koppelen aan de resources. Bijvoorbeeld, maken we de volgende codes: afdeling, omgeving, eigenaar, Project.

De volgende Schermafbeelding toont een voorbeeld van een resourcegroep met de bijbehorende tags.

![Afbeelding 11 - resourcegroep met de bijbehorende tags op Azure-portal][11]

De volgende stap is het ophalen van de informatie van de API voor gebruik in de Cloud Cruiser. De Usage-API biedt momenteel reacties in JSON-indeling. Hier volgt een voorbeeld van de gegevens zijn opgehaald:

    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Gegevens van de API voor gebruik in de Cloud Cruiser importeren
Cloud Cruiser werkmappen bieden een geautomatiseerde manier voor het verzamelen en de verwerking van gegevens van de API-gebruik. Een werkmap van ETL (uitpakken-transformatie-load) kunt u de verzameling, transformatie en publiceren van gegevens naar de Cloud Cruiser-database configureren.

Elke werkmap, kan een of meerdere verzamelingen hebben. Hiermee kunt u gegevens uit verschillende bronnen naar een aanvulling vormen op of uitbreiding van de gebruiksgegevens correleren. Bijvoorbeeld, maken we een nieuw werkblad in de werkmap Azure-sjabloon (*UsageAPI)* en stel een nieuw *verzameling* voor het importeren van gegevens van de API-gebruik.

![Afbeelding 3 - API gebruiksgegevens geïmporteerd in het blad UsageAPI][12]

U ziet dat deze werkmap al andere werkbladen heeft voor het importeren van de services van Azure (*ImportServices*), en de informatie over het verbruik van de API facturering verwerken (*PublishData*).

We gebruik vervolgens de Usage-API voor het vullen van de *UsageAPI* werkblad en het correleren van de gegevens voor de gegevens over het verbruik van de API facturering op de *PublishData* blad.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Verwerking van de tag-informatie van de API-gebruik
Na het importeren van de gegevens in de werkmap maken we transformatie stappen in de *UsageAPI* blad om de gegevens van de API. Eerste stap is het gebruik van een 'JSON splitsen'-processor en uitpakken van de labels van een veld vervolgens velden voor elk van deze (afdeling, Project eigenaar en omgeving) maken.

![Afbeelding 4: nieuwe velden voor de tag-informatie maken][13]

U ziet de "Netwerken" service ontbreekt de labelgegevens (geel vak), maar we kunnen verifiëren dat het deel van dezelfde resourcegroep uitmaakt door te kijken de *ResourceGroupName* veld. Aangezien we de tags voor de andere bronnen van deze resourcegroep hebben, kunnen we gebruik van deze informatie om de ontbrekende tags toepassen op deze resource later in het proces.

De volgende stap is het maken van een opzoektabel koppelen van de gegevens van de codes voor de *ResourceGroupName*. Deze opzoektabel wordt gebruikt op de volgende stap voor het aanvullen van de verbruiksgegevens label informatie.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>De labelgegevens toe te voegen aan de verbruiksgegevens
Nu we kunt gaan naar de *PublishData* werkblad, welke processen de informatie over het verbruik van de API facturering en de velden die zijn opgehaald uit de labels toevoegen. Dit proces wordt uitgevoerd door te kijken in de opzoektabel op de vorige stap hebt gemaakt met behulp van de *ResourceGroupName* als de sleutel voor zoekopdrachten.

![Afbeelding 5: de structuur van het account met de gegevens van zoekopdrachten in te vullen][14]

U ziet dat de juiste account structure-velden voor de service "Netwerken" zijn toegepast, en het verhelpen van het probleem met de ontbrekende codes. We ook de velden van de structuur account voor bronnen dan de doelgegevensruimte resourcegroep met 'Andere' ingevuld om ze in de rapporten te onderscheiden.

Nu moet we alleen een stap voor het publiceren van de gebruiksgegevens toevoegen. De juiste tarieven voor elke service op onze Tariefplan gedefinieerd worden tijdens deze stap wordt toegepast op de informatie over het gebruik, met de resulterende kosten geladen in de database.

Het beste onderdeel is dat u alleen eenmaal door dit proces te gaan. Als de werkmap is voltooid, moet u alleen toe te voegen aan de planner en deze wordt elk uur of dagelijks uitgevoerd op het geplande tijdstip. Is alleen een kwestie van nieuwe rapporten maken of bestaande relaties aanpassen om te kunnen analyseren van de gegevens zodat u betekenisvolle inzichten verkrijgen van uw gebruik van cloud.

### <a name="next-steps"></a>Volgende stappen
* Voor gedetailleerde instructies over het maken van Cloud Cruiser werkmappen en de rapporten die verwijzen naar de Cloud Cruiser online [documentatie](http://docs.cloudcruiser.com/) (geldig aanmelden vereist).  Raadpleeg voor meer informatie over Cloud Cruiser [ info@cloudcruiser.com ](mailto:info@cloudcruiser.com).
* Zie [inzicht in uw Microsoft Azure-brongebruik](billing-usage-rate-card-overview.md) voor een overzicht van het gebruik van Azure-bronnen en RateCard APIs.
* Bekijk de [Azure Billing REST API-verwijzing](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's die deel uitmaken van de reeks API's die zijn opgegeven door de Azure Resource Manager.
* Als u meteen rechts in de voorbeeldcode wilt, Bekijk onze Microsoft Azure Billing API codevoorbeelden op [Azure codevoorbeelden](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Meer informatie
* Zie voor meer informatie over de Azure Resource Manager, de [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) artikel.

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Afbeelding 1: een nieuwe verzameling maken"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Afbeelding 2: gegevens importeren uit de nieuwe verzameling"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Afbeelding 3 - transformatie stappen voor het verwerken van de verzamelde gegevens van RateCard API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Afbeelding 4: de gegevens van de API RateCard publiceren als een nieuwe Services en tarieven"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Afbeelding 5: de nieuwe Services verifiëren"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Afbeelding 6 - verifiëren van de nieuwe Tariefplan en de bijbehorende tarieven"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Afbeelding 7: transformeren WAP-gegevens voor het normaliseren van services"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Afbeelding 8 - werkmap plannen"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Afbeelding 9 - voorbeeldrapport voor de werkbelasting kosten vergelijking scenario"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Afbeelding 10 - rapport met storingen met tags"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Afbeelding 11 - resourcegroep met de bijbehorende tags op Azure-portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Afbeelding 12 - API gebruiksgegevens geïmporteerd in het blad UsageAPI"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Afbeelding 13 - nieuwe velden voor de tag-informatie maken"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Afbeelding 14 - vullen met de structuur van het account met de gegevens van zoekopdrachten"
