---
title: Cloud Cruiser en Microsoft Azure Billing API-integratie | Microsoft Docs
description: Biedt een unieke perspectief van Microsoft Azure Billing partner Cloud Cruiser, op hun ervaringen die de Azure Billing API's integreren in hun product.  Dit is vooral handig voor klanten met Azure en Cloud Cruiser die zijn geïnteresseerd in met behulp van/probeert Cloud Cruiser voor Microsoft Azure Pack.
services: ''
documentationcenter: ''
author: tonguyen
manager: mumami
editor: ''
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 10/09/2017
ms.author: erikre
ms.openlocfilehash: bae35f9e153227ddcca6e0d20ce862317b968eb1
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57536008"
---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser en Microsoft Azure Billing API-integratie
Dit artikel wordt beschreven hoe de gegevens die worden verzameld van de nieuwe Microsoft Azure Billing API's in de Cloud Cruiser kan worden gebruikt voor de werkstroom kosten simulatie en analyse.

## <a name="azure-ratecard-api"></a>Azure RateCard API
De RateCard API biedt gegevens over van Azure. Na verificatie met de juiste referenties opgeeft, kunt u een query de API voor het verzamelen van metagegevens over de services die beschikbaar zijn op Azure, samen met de tarieven die zijn gekoppeld aan uw aanbieding.

Het volgende voorbeeldantwoord afkomstig is van de API met de prijzen voor de A0-exemplaar (Windows):

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

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cloud Cruiser de Interface voor Azure RateCard API
Cloud Cruiser kunt de RateCard API-informatie op verschillende manieren gebruiken. In dit artikel laten we zien hoe het kan worden gebruikt voor het maken van IaaS werkbelasting kosten simulatie en analyse.

Stel een werkbelasting voor meerdere exemplaren op Microsoft Azure Pack (WAP) uitgevoerd om te demonstreren deze use case. Het doel is om te simuleren van deze dezelfde werkbelasting op Azure en schat de kosten van deze migratie. Als u wilt maken in deze simulatie, zijn er twee belangrijkste taken die moeten worden uitgevoerd:

1. **Importeren en verwerken van gegevens voor de service die is verzameld van de RateCard API.** Deze taak wordt ook uitgevoerd op de werkmappen, waarbij het uitpakken van de RateCard API getransformeerd en gepubliceerd naar een nieuwe tariefplan. Deze nieuwe tariefplan wordt gebruikt op de simulaties om in te schatten van de Azure-prijzen.
2. **Normaliseren WAP-services en Azure IaaS-services.** Standaard WAP-services zijn gebaseerd op afzonderlijke resources (CPU, geheugen, schijfgrootte, enzovoort) terwijl Azure services zijn afhankelijk van de grootte van het exemplaar (A0, A1, A2, enz.). Deze eerste taak kan worden uitgevoerd door de ETL-engine van Cloud Cruiser, aangeroepen werkmappen, waar deze resources kunnen worden gebundeld op exemplaargrootten, vergelijkbaar met de services van Azure-instantie.

### <a name="import-data-from-the-ratecard-api"></a>Gegevens importeren uit de RateCard-API
Cloud Cruiser werkmappen bieden een geautomatiseerde manier voor het verzamelen en verwerken van gegevens uit de RateCard API.  ETL (extract-transform-load) werkmappen kunnen u de verzameling, transformatie en publicatie van gegevens in de Cloud Cruiser-database configureren.

Elke werkmap kan een of meerdere verzamelingen, zodat u kunt het correleren van gegevens uit verschillende bronnen te vullen of Verbeter de gebruiksgegevens hebben. De volgende twee schermafbeeldingen laten zien hoe u een nieuw *verzameling* in een bestaande werkmap en importeren van gegevens in de *verzameling* van de RateCard API:

![Afbeelding 1: een nieuwe verzameling maken][1]

![Afbeelding 2: gegevens importeren van de nieuwe verzameling][2]

Na het importeren van de gegevens in de werkmap, is het mogelijk te maken van meerdere stappen en processen voor transformatie, om te wijzigen en de gegevens modelleren. In dit voorbeeld omdat we alleen geïnteresseerd bent in infrastructuur-as-a-Service (IaaS), gebruiken we de stappen voor gegevenstransformatie om onnodige rijen te verwijderen of records, die betrekking hebben op services dan IaaS.

De volgende schermafbeelding ziet u de stappen voor gegevenstransformatie gebruikt voor het verwerken van de gegevens die worden verzameld van RateCard API:

![Afbeelding 3 - transformatiestappen voor het verwerken van verzamelde gegevens van RateCard API][3]

### <a name="defining-new-services-and-rate-plans"></a>Nieuwe Services en snelheid definiëren plannen
Er zijn verschillende manieren voor het definiëren van services op Cloud Cruiser. Een van de opties is voor het importeren van de services van gegevens over gebruik. Deze methode wordt doorgaans gebruikt bij het werken met openbare clouds, waar de services die al zijn gedefinieerd door de provider.

Een Tariefplan is een set tarieven of prijzen die kunnen worden toegepast op verschillende services uit, op basis van ingangsdatums of groep van klanten, onder andere opties. Tariefplannen kunnen ook worden gebruikt in de Cloud Cruiser simulatie of 'What-if-scenario's, om te begrijpen hoe wijzigingen in services invloed hebben op de totale kosten van een werkbelasting te maken.

In dit voorbeeld gebruiken we de service-informatie van de RateCard API voor het definiëren van nieuwe services in de Cloud Cruiser. We kunnen op dezelfde manier, de tarieven die is gekoppeld aan de services gebruiken voor het maken van een nieuwe Tariefplan op Cloud Cruiser.

Aan het einde van het proces voor transformatie is het mogelijk te maken van een nieuwe stap en de gegevens van de RateCard API publiceren als nieuwe services en -tarieven.

![Afbeelding 4: de gegevens uit de RateCard API publiceren als een nieuwe Services en -tarieven][4]

### <a name="verify-azure-services-and-rates"></a>Controleer of de Azure-Services en -tarieven
Na het publiceren van de services en -tarieven, kunt u controleren of de lijst met geïmporteerde services in de Cloud Cruiser *Services* tabblad:

![Afbeelding 5: de nieuwe Services controleren][5]

Op de *tarief plannen* tabblad kunt u het nieuwe tariefplan met de naam 'AzureSimulation' met de tarieven geïmporteerd uit de RateCard API controleren.

![Afbeelding 6 - verifiëren van de nieuwe Tariefplan en de bijbehorende tarieven][6]

### <a name="normalize-wap-and-azure-services"></a>Normaliseren WAP en Azure-Services
WAP biedt standaard, informatie over het gebruik op basis van het gebruik van Reken-, geheugen- en netwerkbronnen. In de Cloud Cruiser, definieert u uw services op basis van rechtstreeks op de toewijzing of het met een datalimiet gebruik van deze resources. U kunt bijvoorbeeld het instellen van een basic-tarief voor elk uur van de CPU-gebruik of de GB aan geheugen toegewezen aan een exemplaar in rekening gebracht.

Bijvoorbeeld, om kosten tussen WAP en Azure, vergelijken moet we het gebruik van bronnen op WAP in pakketten, die vervolgens kunnen worden toegewezen aan Azure-services. Deze transformatie kan eenvoudig worden geïmplementeerd in de werkmappen:

![Afbeelding 7: transformeren van WAP-gegevens voor het normaliseren van services][7]

De laatste stap in de werkmap is voor het publiceren van de gegevens in de Cloud Cruiser-database. Gegevens over gebruik is tijdens deze stap nu gebundeld in services (die zijn toegewezen aan de Azure-Services) en gekoppeld aan de standaardtarieven voor het maken van de kosten in rekening gebracht.

Nadat de werkmap is voltooid, kunt u de verwerking van de gegevens, automatiseren door het toevoegen van een taak op de scheduler en de frequentie en het tijdstip voor de werkmap om uit te voeren op te geven.

![Afbeelding 8 - werkmap plannen][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Rapporten maken voor de werkbelasting kosten simulatie analyse
Nadat de gebruiksgegevens worden verzameld en kosten in rekening gebracht in de Cloud Cruiser-database worden geladen, kunt we het Cloud Cruiser Insights-module gebruiken voor het maken van de werkbelasting simulatie gewenste kosten.

Als u wilt laten zien in dit scenario, we hebben het volgende rapport gemaakt:

![Vergelijking van de kosten][9]

De bovenste grafiek toont een kostenvergelijking van de door services, de prijs van het uitvoeren van de werkbelasting voor elke specifieke service tussen WAP (Donkerblauw) en Azure (Lichtblauw) te vergelijken.

De onderste grafiek laat zien dezelfde gegevens maar onderverdeeld op basis van afdeling. De kosten voor elke afdeling om uit te voeren van hun werkbelasting op de WAP- en Azure, samen met het verschil tussen deze wordt weergegeven in de balk besparingen (groen).

## <a name="azure-usage-api"></a>Gebruik van Azure-API
### <a name="introduction"></a>Inleiding
Microsoft is onlangs geïntroduceerd voor het Azure Usage API, zodat abonnees om op te halen via een programma gegevens over gebruik voor inzicht in hun gebruik. Cloud Cruiser-klanten kunnen profiteren van de uitgebreide gegevensset beschikbaar via deze API.

Cloud Cruiser kunt u de integratie met de API-gebruik op verschillende manieren gebruiken. De granulatie (per uur gebruiksgegevens) en resource metagegevensinformatie beschikbaar via de API biedt de benodigde gegevensset ter ondersteuning van flexibele Showback of Chargeback modellen. 

In deze zelfstudie geven we een voorbeeld van hoe Cloud Cruiser van de Usage API-informatie profiteren kan. Meer specifiek, wordt er een resourcegroep maken op Azure, tags voor de structuur van het account koppelen en beschrijven het proces van ophalen en verwerken van de informatie over de assettag in Cloud Cruiser.

Het uiteindelijke doel is om te kunnen maken van rapporten, zoals de volgende uitvoer en kan om kosten en verbruik op basis van de account-structuur gevuld met de labels te analyseren.

![Afbeelding 10 - rapport met storingen met tags][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure-Tags
De gegevens beschikbaar zijn via de API voor het gebruik van Azure bevatten niet alleen informatie over het energieverbruik, maar ook met inbegrip van alle tags die zijn gekoppeld aan deze resource-metagegevens. -Tags bieden een eenvoudige manier om uw resources te organiseren, maar als u wilt worden van kracht, moet u ervoor te zorgen dat:

* Labels worden correct toegepast op de resources tegelijk inrichten
* Tags zijn goed gebruikt op het proces factureringen/terugstortingen om te koppelen van het gebruik van de organisatie-account structuur.

Beide van deze vereisten kunnen lastig zijn, met name wanneer er een handmatig proces voor de levering of aan clientzijde in rekening gebracht. Verkeerd getypt, onjuiste of zelfs ontbrekende tags zijn algemene klachten van klanten bij met behulp van tags en deze fouten kan leven aan de Bezig met laden moeilijk maken.

Met de nieuwe Azure Usage API Cloud Cruiser resource tagging informatie halen, en met een geavanceerde ETL hulpprogramma werkmappen genoemd, deze algemene labels fouten oplossen. Met de transformatie met behulp van reguliere expressies en correlatie van gegevens, Cloud Cruiser onjuist gelabelde resources identificeren en toepassen van de juiste tags, ervoor te zorgen dat de juiste koppeling van de resources met de consument.

Aan de Bezig met laden Cloud Cruiser de factureringen/terugstortingen automatiseert en de informatie over de assettag kunt gebruiken om te koppelen van het gebruik moet de juiste gebruiker (afdeling, afdeling, Project, enz.). Deze automatisering biedt een enorme verbetering vormt en kunt controleren of een consistente en controleerbare Bezig met laden proces.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Het maken van een resourcegroep met tags op Microsoft Azure
De eerste stap in deze zelfstudie is het maken van een resourcegroep in Azure portal, maakt u nieuwe tags wilt koppelen aan de resources. In dit voorbeeld maken we de volgende codes: Afdeling, milieu, de eigenaar, Project.

De volgende schermafbeelding ziet een voorbeeld van resourcegroep met de bijbehorende labels.

![Afbeelding 11 - resourcegroep met de gekoppelde tags in Azure portal][11]

De volgende stap is om op te halen van de gegevens van de API-gebruik in de Cloud Cruiser. De Usage-API biedt momenteel antwoorden in JSON-indeling. Hier volgt een voorbeeld van de gegevens opgehaald:

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


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Gegevens importeren uit de API-gebruik in de Cloud Cruiser
Cloud Cruiser werkmappen bieden een geautomatiseerde manier voor het verzamelen en de verwerking van gegevens van de API-gebruik. De werkmap van een ETL (extract-transform-load) kunt u de verzameling, transformatie en publicatie van gegevens in de Cloud Cruiser-database configureren.

Elke werkmap kan een of meer verzamelingen hebben. Hiermee kunt u correlaties zichtbaar maken tussen gegevens uit verschillende bronnen te vullen of Verbeter de gebruiksgegevens. In dit voorbeeld maken we een nieuw blad in de werkmap van de Azure-sjabloon (*UsageAPI)* en stel een nieuwe *verzameling* om gegevens te importeren vanuit de API-gebruik.

![Afbeelding 3: gegevens over gebruik API geïmporteerd in het werkblad UsageAPI][12]

U ziet dat deze werkmap al andere werkbladen voor het importeren van services van Azure (*ImportServices*), en verwerken van de informatie over het energieverbruik van de facturering-API (*PublishData*).

Vervolgens gebruiken we de Usage-API voor het vullen van de *UsageAPI* blad en correleren van de gegevens met de gegevens over het verbruik van de facturering-API op de *PublishData* blad.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Verwerking van de informatie over de assettag van de API-gebruik
Na het importeren van de gegevens in de werkmap, maken we de stappen voor gegevenstransformatie in de *UsageAPI* blad om de gegevens van de API. Eerste stap is het gebruik van een processor "JSON splitsen" uitpakken van de labels van een veld en vervolgens velden maken voor elk incident (afdeling, Project, eigenaar en omgeving).

![Afbeelding 4: nieuwe velden voor de informatie over de assettag maken][13]

U ziet de service 'Netwerken' ontbreekt de informatie over de assettag (geel vak), maar we kunnen verifiëren dat het is onderdeel van dezelfde resourcegroep door te kijken de *ResourceGroupName* veld. Aangezien we de tags voor de andere resources in deze resourcegroep hebben, kunnen we gebruiken deze gegevens naar de ontbrekende labels toevoegen aan deze resource later in het proces.

De volgende stap is het maken van een opzoektabel koppelen van de gegevens van de labels aan de *ResourceGroupName*. Deze opzoektabel wordt gebruikt op de volgende stap om de gegevens met informatie over de assettag verrijken.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>De informatie over de assettag toe te voegen aan de verbruiksgegevens
Nu we u naar gaat kunt de *PublishData* blad, welke processen de informatie over het energieverbruik van de facturering-API, en de velden die zijn geëxtraheerd uit de labels toevoegen. Dit proces wordt uitgevoerd door te kijken in de opzoektabel op de vorige stap hebt gemaakt met behulp van de *ResourceGroupName* als de sleutel voor zoekopdrachten.

![Afbeelding 5: de structuur van het account met de gegevens uit de opzoekbewerkingen vullen][14]

U ziet dat de juiste account structuurvelden voor de service "Netwerken" zijn toegepast, en het probleem is opgelost met de ontbrekende codes. We ook de velden van de structuur account voor resources dan ons doel resourcegroep met 'Ander' gevuld om u te onderscheiden van de rapporten.

Nu wilt we alleen toevoegen van een stap voor het publiceren van gegevens over gebruik. De juiste tarieven voor elke service die is gedefinieerd op onze Tariefplan worden tijdens deze stap wordt toegepast op de informatie over het gebruik, met de resulterende kosten in de database geladen.

Het mooiste is dat u alleen aan de slag door dit proces eenmaal hebt. Als de werkmap is voltooid, moet u alleen toe te voegen aan de scheduler en deze op het geplande tijdstip wordt elk uur of dagelijks uitgevoerd. Is alleen een kwestie van nieuwe rapporten te maken of bestaande toepassingen aanpassen om te kunnen analyseren van de gegevens voor het ophalen van bruikbare inzichten uit uw gebruik van de cloud.

### <a name="next-steps"></a>Volgende stappen
* Voor gedetailleerde instructies over het maken van Cloud Cruiser werkmappen en -rapporten verwijzen naar de Cloud Cruiser online [documentatie](https://docs.cloudcruiser.com/) (geldige aanmelding vereist).  Voor meer informatie over Cloud Cruiser, neem contact op met [ info@cloudcruiser.com ](mailto:info@cloudcruiser.com).
* Zie [inzicht in het gebruik van de Microsoft Azure-resources](billing-usage-rate-card-overview.md) voor een overzicht van het gebruik van Azure-resources en RateCard APIs.
* Bekijk de [Azure Billing REST API-verwijzing](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's die deel uitmaken van de set API's die zijn opgegeven door de Azure Resource Manager.
* Als u meteen in de voorbeeldcode wilt, bekijk dan onze Microsoft Azure Billing API-codevoorbeelden op [Azure-codevoorbeelden](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Meer informatie
* Zie voor meer informatie over de Azure Resource Manager, de [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) artikel.

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Afbeelding 1: een nieuwe verzameling maken"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Afbeelding 2: gegevens importeren van de nieuwe verzameling"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Afbeelding 3 - transformatiestappen voor het verwerken van verzamelde gegevens van RateCard API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Afbeelding 4: de gegevens uit de RateCard API publiceren als een nieuwe Services en -tarieven"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Afbeelding 5: de nieuwe Services controleren"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Afbeelding 6 - verifiëren van de nieuwe Tariefplan en de bijbehorende tarieven"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Afbeelding 7: transformeren van WAP-gegevens voor het normaliseren van services"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Afbeelding 8 - werkmap plannen"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Afbeelding 9 - voorbeeld-rapport voor de werkbelasting kosten vergelijking scenario"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Afbeelding 10 - rapport met storingen met tags"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Afbeelding 11 - resourcegroep met de gekoppelde tags in Azure portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Afbeelding 12 - API voor gebruiksgegevens geïmporteerd in het werkblad UsageAPI"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Afbeelding 13 - nieuwe velden voor de informatie over de assettag maken"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Afbeelding 14 - invullen van de structuur van het account met de gegevens uit de opzoekbewerkingen"
