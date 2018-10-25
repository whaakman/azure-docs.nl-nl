---
title: Continue export van telemetrie van Application Insights | Microsoft Docs
description: Diagnostische gegevens en gebruiksgegevens exporteren naar opslag in Microsoft Azure en van daaruit te downloaden.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: mbullwin
ms.openlocfilehash: bc505aa9dc2258a8752d1986489957c401e4e4c4
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50023494"
---
# <a name="export-telemetry-from-application-insights"></a>Telemetrie exporteren vanuit Application Insights
Wilt u uw telemetrie langer dan de standaard bewaarperiode houden? Of op een bepaalde gespecialiseerde manier te verwerken? Continue Export is ideaal voor dit. De gebeurtenissen die u in de Application Insights-portal ziet kunnen worden geëxporteerd naar de opslag in Microsoft Azure in JSON-indeling. Van daaruit kunt u uw gegevens downloaden en wat u code schrijven moet verwerken.  

Voordat u continue export instelt, zijn er enkele alternatieven die u wilt gebruiken:

* De knop exporteren aan de bovenkant van een blade met metrische gegevens of een zoekopdracht kunt u overbrengen van tabellen en grafieken met een Excel-werkblad.

* [Analytics](app-insights-analytics.md) biedt een krachtige querytaal voor telemetrie. Het kan ook resultaten exporteren.
* Als u op zoek bent naar [Verken uw gegevens in Power BI](app-insights-export-power-bi.md), kunt u dat doen zonder gebruik van continue Export.
* De [REST-API voor gegevenstoegang](https://dev.applicationinsights.io/) kunt u via een programma toegang krijgen tot uw telemetrie.
* U kunt setup ook openen [continue export via Powershell](https://docs.microsoft.com/powershell/module/azurerm.applicationinsights/new-azurermapplicationinsightscontinuousexport?view=azurermps-5.7.0).

Nadat de continue Export uw gegevens worden gekopieerd naar de opslag (waar deze kan blijven voor als u wilt), is het nog steeds beschikbaar in Application Insights voor de gebruikelijke [bewaarperiode](app-insights-data-retention-privacy.md).

## <a name="setup"></a> Maken van een continue Export
1. Open continue Export in de Application Insights-resource voor uw app, en kies **toevoegen**:

2. Kies de telemetrie gegevenstypen die u wilt exporteren.

3. Maak of Selecteer een [Azure storage-account](../storage/common/storage-introduction.md) waar u de gegevens worden opgeslagen. Ga voor meer informatie over prijsopties opslag naar de [officiële pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/).

    > [!Warning]
    > Standaard wordt de opslaglocatie worden ingesteld op dezelfde geografische regio als uw Application Insights-resource. Als u in een andere regio opslaat, worden er kosten voor gegevensoverdracht.

    ![Klik op toevoegen, exporteren bestemming, Storage-account en maakt een nieuwe winkel of kies een bestaande store](./media/app-insights-export-telemetry/02-add.png)

4. Maak of Selecteer een container in de opslag:

    ![Klik op de typen gebeurtenissen kiezen](./media/app-insights-export-telemetry/create-container.png)

Als u de uitvoer hebt gemaakt, begint deze gaan. U wordt alleen gegevens die binnenkomt nadat u de export hebt gemaakt.

Er is een vertraging van ongeveer een uur voordat gegevens worden weergegeven in de opslag.

### <a name="to-edit-continuous-export"></a>Continue export bewerken

Als u de gebeurtenistypen later wijzigen wilt, bewerkt u gewoon de uitvoer:

![Klik op de typen gebeurtenissen kiezen](./media/app-insights-export-telemetry/05-edit.png)

### <a name="to-stop-continuous-export"></a>Continue export stoppen

Als u wilt stoppen met het exporteren, klikt u op uitschakelen. Als u het opnieuw inschakelen klikt, wordt de uitvoer opnieuw opgestart met nieuwe gegevens. U kunt de gegevens die in de portal ontvangen tijdens het exporteren is uitgeschakeld wordt niet ophalen.

Als u wilt de export permanent stoppen, moet u deze verwijderen. In dat geval verwijderd niet uw gegevens uit de opslag.

### <a name="cant-add-or-change-an-export"></a>Kan niet toevoegen of wijzigen van een export?
* Als u wilt toevoegen of wijzigen van de uitvoer, moet u toegangsrechten voor de eigenaar, bijdrager of Inzender voor Application Insights. [Meer informatie over rollen][roles].

## <a name="analyze"></a> Welke gebeurtenissen waar u krijgen?
De geëxporteerde gegevens is de onbewerkte telemetrie van uw toepassing ontvangen, behalve dat we locatiegegevens die we berekenen van de IP-adres van client toevoegen.

Gegevens die zijn verwijderd door [steekproeven](app-insights-sampling.md) is niet opgenomen in de geëxporteerde gegevens.

Andere berekende metrische gegevens zijn niet opgenomen. Bijvoorbeeld gemiddelde CPU-gebruik niet worden geëxporteerd, maar we Exporteer de onbewerkte telemetrie van waaruit het gemiddelde wordt berekend.

De gegevens bevatten ook de resultaten van een [webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md) die u hebt ingesteld.

> [!NOTE]
> **Een steekproef.** Als uw toepassing grote hoeveelheden gegevens verzendt, kan de functie steekproeven werken en slechts een fractie van de gegenereerde telemetrie te verzenden. [Meer informatie over steekproeven.](app-insights-sampling.md)
>
>

## <a name="get"></a> De gegevens controleren
U kunt de opslag rechtstreeks in de portal controleren. Klik op **Bladeren**, selecteert u uw storage-account en open vervolgens **Containers**.

Als u wilt controleren in Azure storage in Visual Studio, open **weergave**, **Cloud Explorer**. (Als u deze opdracht hebt, moet u de Azure SDK installeren: Open de **nieuw Project** dialoogvenster Vouw Visual C# / in de Cloud en kies **Microsoft Azure SDK voor .NET ophalen**.)

Wanneer u uw blob-archief opent, ziet u een container met een set van blob-bestanden. De URI van elk bestand dat is afgeleid van de naam van uw Application Insights-resource, de instrumentatiesleutel, telemetrie-type/datum/tijd. (De resourcenaam van de is alleen kleine letters bevatten en de instrumentatiesleutel wordt weggelaten streepjes bevatten.)

![De blob-archief met een geschikt hulpprogramma controleren](./media/app-insights-export-telemetry/04-data.png)

De datum en tijd zijn UTC en zijn wanneer de telemetrie is geplaatst in de store - niet op het tijdstip waarop dat het foutrapport is gegenereerd. Dus als u code schrijven om te downloaden van de gegevens, kunt het verplaatsen lineair worden door de gegevens.

Dit is de vorm van het pad:

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

Waar

* `blobCreationTimeUtc` tijd waarop de blob is gemaakt in de interne is opslag staging
* `blobDeliveryTimeUtc` is de tijd die als blob wordt gekopieerd naar de export-doelopslag

## <a name="format"></a> Gegevensindeling
* Elke blob is een tekstbestand dat meerdere bevat ' \n'-separated rijen. Het bevat de telemetrie die gedurende een periode van ongeveer een halve minuut is verwerkt.
* Elke rij vertegenwoordigt een gegevenspunt telemetrie, zoals een aanvraag of paginaweergave-weergave.
* Elke rij is een niet-opgemaakte JSON-document. Als u wilt blijven en staart bekijken, opent u het in Visual Studio en kies bewerken, Geavanceerd indelingsbestand:

![De telemetrie met een geschikt hulpprogramma bekijken](./media/app-insights-export-telemetry/06-json.png)

Tijdsduur van de zijn in tikken, waarbij 10 000 bedraagt tikken = 1 ms. Bijvoorbeeld deze waarden weergegeven voor een periode van 1 ms een aanvraag te verzenden vanuit de browser, 3 ms voor het ontvangen en 1.8 s voor het verwerken van de pagina in de browser:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Gedetailleerde gegevens modelleren verwijzing voor de typen eigenschappen en waarden.](app-insights-export-data-model.md)

## <a name="processing-the-data"></a>Verwerken van de gegevens
Op kleine schaal, kunt u bepaalde code kunt u gegevens uit elkaar gehaald, worden gelezen in een werkblad, enzovoort. Bijvoorbeeld:

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

Zie voor een grotere codevoorbeeld [met behulp van een werkrol][exportasa].

## <a name="delete"></a>Uw oude gegevens verwijderen
Houd er rekening mee dat u verantwoordelijk bent voor het beheren van de opslagcapaciteit van uw en het verwijderen van de oude gegevens indien nodig.

## <a name="if-you-regenerate-your-storage-key"></a>Als u uw storage-sleutel opnieuw genereren...
Als u de sleutel in de opslagruimte wijzigt, meer continue export gebruiken. U ziet een melding in uw Azure-account.

Open de blade continue Export en bewerk de export. De bestemming exporteren bewerken, maar laat de dezelfde opslag die is geselecteerd. Klik op OK om te bevestigen.

![Bewerken de continue export, openen en sluiten thee bestemming voor het exporteren.](./media/app-insights-export-telemetry/07-resetstore.png)

De continue export wordt opnieuw opgestart.

## <a name="export-samples"></a>Exporteren van voorbeelden

* [Exporteren naar SQL met Stream Analytics][exportasa]
* [Voorbeeld 2 van de Stream Analytics](app-insights-export-stream-analytics.md)

Op grotere schaal, houd rekening met [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop-clusters in de cloud. HDInsight biedt een reeks technologieën voor het beheren en analyseren van big data en u deze kunt gebruiken voor het verwerken van gegevens die zijn geëxporteerd uit Application Insights.

## <a name="q--a"></a>Vragen en antwoorden
* *Maar ik wil, is een eenmalig downloaden van een grafiek.*  

    Ja, kunt u dat doen. Aan de bovenkant van de blade, klikt u op **gegevens exporteren**.
* *Kan ik een export instellen, maar er zijn geen gegevens in mijn store.*

    Heeft Application Insights ontvangen geen telemetriegegevens van uw app omdat u de export ingesteld? U ontvangt alleen nieuwe gegevens.
* *Er is geprobeerd om in te stellen een export, maar is toegang geweigerd*

    Als het account eigendom is van uw organisatie, moet u lid zijn van de groepen eigenaren of bijdragers.
* *Kan ik exporteren meteen naar mijn eigen on-premises gegevensopslag?*

    Nee, er. De exportengine werkt op dit moment alleen met Azure storage op dit moment.  
* *Is er een limiet voor de hoeveelheid gegevens die u in de store plaatsen?*

    Nee. We u houden pushen van gegevens totdat u de export verwijdert. We u stoppen als de buitenste limieten voor blob-opslag is bereikt, maar dat heel groot is. Het is aan u om te bepalen hoeveel opslagcapaciteit die u gebruikt.  
* *Het aantal blobs moet ik Zie in de opslag?*

  * Voor elk gegevenstype dat u hebt geselecteerd om te exporteren, wordt een nieuwe blob elke minuut gemaakt (als de gegevens beschikbaar is).
  * Voor toepassingen met intensief verkeer, worden bovendien extra partitie eenheden toegewezen. Elke eenheid maakt een blob in dit geval elke minuut.
* *Kan ik de sleutel naar Mijn opslag opnieuw gegenereerd of de naam van de container gewijzigd en nu de export niet werkt.*

    De export bewerken en open de blade van de bestemming exporteren. Laat de dezelfde opslag als voorheen geselecteerd en klik op OK om te bevestigen. Exporteren wordt opnieuw opgestart. Als de wijziging in de afgelopen paar dagen is, kunt u gegevens verliest.
* *Kan ik de export onderbreken?*

    Ja. Klik op uitschakelen.

## <a name="code-samples"></a>Codevoorbeelden

* [Voorbeeld van Stream Analytics](app-insights-export-stream-analytics.md)
* [Exporteren naar SQL met Stream Analytics][exportasa]
* [Gedetailleerde gegevens modelleren verwijzing voor de typen eigenschappen en waarden.](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
