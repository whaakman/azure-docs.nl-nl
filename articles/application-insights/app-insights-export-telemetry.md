---
title: Continue export van telemetrie in Application Insights | Microsoft Docs
description: Diagnostische gegevens en gebruiksgegevens exporteren naar opslag in Microsoft Azure en download deze vanaf daar.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: bwren
ms.openlocfilehash: 6ac3bda5101593b5ca66b4c9035e2fdac9d1e833
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="export-telemetry-from-application-insights"></a>Exporteren van telemetrie in Application Insights
Wilt u uw telemetrie langer dan de standaard bewaarperiode houden? Of op een speciale wijze worden verwerkt? Continue Export is ideaal voor dit. De gebeurtenissen die u in de Application Insights-portal ziet kunnen worden geëxporteerd naar JSON-indeling in Microsoft Azure-opslag. Daar kunt u downloaden van uw gegevens en wat u code schrijven moet verwerken.  

Met behulp van continue Export mogelijk extra kosten in rekening gebracht. Controleer uw [prijzen model](http://azure.microsoft.com/pricing/details/application-insights/).

Voordat u de continue export instelt, zijn er enkele alternatieven die kunt u overwegen:

* De knop exporteren boven aan een blade metrische gegevens of zoeken kunt u transfer tabellen en grafieken met een Excel-werkblad.

* [Analytics](app-insights-analytics.md) biedt een krachtige querytaal voor telemetrie. Het kan ook resultaten exporteren.
* Als u op zoek bent naar [Verken uw gegevens in Power BI](app-insights-export-power-bi.md), kunt u dat doen zonder gebruik van continue Export.
* De [REST-API toegang tot de gegevens](https://dev.applicationinsights.io/) kunt u programmatisch toegang krijgen tot uw telemetrie.

Nadat de continue Export uw gegevens worden gekopieerd naar opslag (waar het kunt blijven voor als u wilt), is het nog steeds beschikbaar in Application Insights voor de gebruikelijke [bewaarperiode](app-insights-data-retention-privacy.md).

## <a name="setup"></a>Maken van een continue Export
1. Open continue Export in de Application Insights-resource voor uw app en kies **toevoegen**:

    ![Schuif naar beneden en klik op de continue Export](./media/app-insights-export-telemetry/01-export.png)

2. Kies de telemetrie-gegevenstypen die u wilt exporteren.

3. Maak of Selecteer een [Azure storage-account](../storage/common/storage-introduction.md) waar u de gegevens worden opgeslagen.

    > [!Warning]
    > Standaard wordt de locatie voor de opslag worden ingesteld op dezelfde geografische regio als uw Application Insights-resource. Als u in een andere regio opslaat, u mogelijk ook wijzigingskosten overdracht.

    ![Klik op toevoegen, exporteren bestemming Storage-account en maakt een nieuwe winkel of kies een bestaand archief](./media/app-insights-export-telemetry/02-add.png)

4. Maak of Selecteer een container in de opslag:

    ![Klik op de typen gebeurtenissen kiezen](./media/app-insights-export-telemetry/create-container.png)

Als u uw export hebt gemaakt, wordt er gestart gaan. U alleen ophalen gegevens waarvoor binnenkomt nadat u de export hebt gemaakt.

Kan er een vertraging van ongeveer een uur voordat gegevens worden weergegeven in de opslag zijn.

### <a name="to-edit-continuous-export"></a>Continue export bewerken

Als u de gebeurtenistypen later wijzigen wilt, bewerkt u gewoon het exporteren:

![Klik op de typen gebeurtenissen kiezen](./media/app-insights-export-telemetry/05-edit.png)

### <a name="to-stop-continuous-export"></a>Continue export stoppen

Als u wilt stoppen met het exporteren, klikt u op uitschakelen. Als u opnieuw inschakelen op, kan de export wordt opnieuw opgestart met nieuwe gegevens. U kunt de gegevens die tijdens het exporteren is uitgeschakeld in de portal is aangekomen won't ophalen.

Als u wilt de export permanent stoppen, moet u deze verwijderen. In dat geval verwijdert niet de gegevens van opslag.

### <a name="cant-add-or-change-an-export"></a>Kan niet toevoegen of wijzigen van een exporteren?
* Als u wilt toevoegen of wijzigen van de uitvoer, moet u de eigenaar, bijdrager of Application Insights Inzender toegangsrechten. [Meer informatie over functies][roles].

## <a name="analyze"></a>Welke gebeurtenissen krijgt u?
De geëxporteerde gegevens is de onbewerkte telemetrie die we van uw toepassing ontvangen, behalve dat we locatiegegevens op die we berekenen van de client-IP-adres toevoegen.

Gegevens die zijn genegeerd door [steekproeven](app-insights-sampling.md) is niet opgenomen in de geëxporteerde gegevens.

Andere berekende waarden zijn niet opgenomen. Bijvoorbeeld: gemiddelde CPU-gebruik niet worden geëxporteerd, maar we Exporteer de onbewerkte telemetrie van waaruit het gemiddelde wordt berekend.

De gegevens omvatten ook de resultaten van een [webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md) die u hebt ingesteld.

> [!NOTE]
> **Een steekproef.** Als uw toepassing grote hoeveelheden gegevens verzendt, wordt de functie steekproeven werkt en wordt alleen een fractie van de gegenereerde telemetrie verzenden. [Meer informatie over steekproeven.](app-insights-sampling.md)
>
>

## <a name="get"></a>De gegevens controleren
U kunt de opslag rechtstreeks in de portal inspecteren. Klik op **Bladeren**, selecteer uw storage-account en open vervolgens **Containers**.

Om te controleren op Azure-opslag in Visual Studio, open **weergave**, **Cloud Explorer**. (Als u deze menuopdracht niet hebt, moet u de Azure SDK installeren: Open de **nieuw Project** dialoogvenster Vouw Visual C# / Cloud en kies **ophalen van Microsoft Azure SDK voor .NET**.)

Wanneer u uw bloblarchief opent, ziet u een container met een set van blob-bestanden. De URI van elk bestand dat is afgeleid van de naam van uw Application Insights-resource, de instrumentatiesleutel, telemetrie-type/datum/tijd. (De resourcenaam is alleen kleine letters en streepjes bevatten de instrumentatiesleutel wordt weggelaten.)

![Inspecteer de blob-opslag met een geschikt hulpprogramma](./media/app-insights-export-telemetry/04-data.png)

De datum en tijd UTC zijn en zijn wanneer de telemetrie is in de store - gedeponeerd niet de tijd die is gegenereerd. Dus als u code schrijven om te downloaden van de gegevens, kunt het verplaatsen lineair via de gegevens.

Hier volgt de vorm van het pad:

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

waar

* `blobCreationTimeUtc`tijdstip waarop de blob is gemaakt in het interne is staging-opslag
* `blobDeliveryTimeUtc`is de tijd wanneer de blob wordt gekopieerd naar het doelopslagaccount exporteren

## <a name="format"></a>Indeling van gegevens
* Elke blob is een tekstbestand dat meerdere bevat ' \n'-separated rijen. Het bevat de telemetrie die gedurende een periode van ongeveer een halve minuut is verwerkt.
* Elke rij vertegenwoordigt een gegevenspunt telemetrie zoals een aanvraag of een pagina weergave.
* Elke rij is een niet-opgemaakte JSON-document. Als u wilt plaatsen en staart op het, opent u het in Visual Studio en kiest u bewerken en Geavanceerd indelingsbestand:

![De telemetrie met een geschikt hulpprogramma weergeven](./media/app-insights-export-telemetry/06-json.png)

Tijdsduren zijn in ticks, waarbij 10 000 bedraagt ticks = 1ms. Deze waarden wordt bijvoorbeeld een tijd van 1ms een aanvraag te verzenden vanuit de browser, 3 MS te ontvangen en 1.8s voor het verwerken van de pagina in de browser weergeven:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Gedetailleerde gegevens model verwijzing voor de eigenschaptypen en waarden.](app-insights-export-data-model.md)

## <a name="processing-the-data"></a>Verwerken van de gegevens
Op kleine schaal, kunt u bepaalde code kunt u gegevens uit elkaar, in een werkblad kunnen lezen, enzovoort. Bijvoorbeeld:

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
Houd er rekening mee dat u zelf verantwoordelijk bent voor het beheren van uw opslagcapaciteit en het verwijderen van de oude gegevens zo nodig.

## <a name="if-you-regenerate-your-storage-key"></a>Als u uw opslagsleutel opnieuw genereren...
Als u de sleutel naar uw opslag wijzigt, wordt continue export niet meer. U ziet een melding in uw Azure-account.

Open de blade continue Export en uw export bewerken. De bestemming exporteren bewerken, maar stelt u de dezelfde opslag die is geselecteerd. Klik op OK om te bevestigen.

![Bewerk de continue export openen en sluiten thee bestemming voor exporteren.](./media/app-insights-export-telemetry/07-resetstore.png)

De continue export wordt opnieuw opgestart.

## <a name="export-samples"></a>Exporteren van voorbeelden

* [Exporteren naar SQL met Stream Analytics][exportasa]
* [Stream Analytics voorbeeld 2](app-insights-export-stream-analytics.md)

Overweeg op grotere schaal [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop-clusters in de cloud. HDInsight biedt een reeks technologieën voor het beheren en analyseren van grote gegevens en u deze kunt gebruiken om gegevens die zijn geëxporteerd uit de Application Insights te verwerken.

## <a name="q--a"></a>Vragen en antwoorden
* *Maar alle gewenste is een eenmalige download van een grafiek.*  

    Ja, kunt u dat doen. Klik boven aan de blade op **gegevens exporteren**.
* *Ik heb een export ingesteld, maar er zijn geen gegevens in de winkel.*

    Heeft Application Insights ontvangen alle telemetrie van uw app omdat u de export instellen? U ontvangt alleen nieuwe gegevens.
* *Geprobeerd een export instellen, maar is toegang geweigerd*

    Als het account eigendom is van uw organisatie, moet u lid zijn van de groepen eigenaars of medewerkers.
* *Kan ik meteen naar mijn eigen winkel lokale exporteren?*

    Nee, momenteel. De engine voor het exporteren wordt momenteel alleen ondersteund met Azure storage op dit moment.  
* *Is er een limiet voor de hoeveelheid gegevens die u in Mijn archief plaatsen?*

    Nee. We je houden gegevens worden gepusht totdat u de export verwijdert. We stopt als we de buitenste limieten voor blob-opslag bereikt, maar dat heel groot is. Het is aan u om te bepalen hoeveel opslagruimte die u gebruikt.  
* *Hoeveel blobs moet ik Zie in de opslag?*

  * Voor elk gegevenstype dat u hebt geselecteerd om te exporteren, wordt een nieuwe blob elke minuut gemaakt (als de gegevens beschikbaar is).
  * Bovendien voor toepassingen met intensief verkeer moet extra partitie eenheden zijn toegewezen. Elke eenheid maakt een blob in dit geval elke minuut.
* *Ik heb de sleutel wordt opnieuw gegenereerd met mijn opslag of de naam van de container gewijzigd en nu de export niet werkt.*

    De export bewerken en open de blade van de bestemming exporteren. Laat dezelfde opslag als voorheen geselecteerd en klik op OK om te bevestigen. Exporteren wordt opnieuw opgestart. Als de wijziging in de afgelopen paar dagen was, geen gegevens verloren gaan.
* *Kan ik de export onderbreken?*

    Ja. Klik op uitschakelen.

## <a name="code-samples"></a>Codevoorbeelden

* [Stream Analytics-voorbeeld](app-insights-export-stream-analytics.md)
* [Exporteren naar SQL met Stream Analytics][exportasa]
* [Gedetailleerde gegevens model verwijzing voor de eigenschaptypen en waarden.](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
