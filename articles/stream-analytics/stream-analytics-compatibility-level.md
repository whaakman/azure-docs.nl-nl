---
title: Informatie over het compatibiliteitsniveau voor Azure Stream Analytics-taken
description: Informatie over het instellen van een compatibiliteitsniveau voor een Azure Stream Analytics-taak en belangrijke wijzigingen in de meest recente compatibiliteitsniveau
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 37d57b0dc381f0ed2edf1f3debb76e5b54eae157
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093825"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Compatibiliteitsniveau voor Azure Stream Analytics-taken
 
Compatibiliteitsniveau verwijst naar de release-specifieke gedrag van een Azure Stream Analytics-service. Azure Stream Analytics is een beheerde service, met de reguliere functie-updates en verbeterde prestaties. Meestal worden updates automatisch beschikbaar gemaakt voor eindgebruikers. Enkele nieuwe functies kunnen echter introduceert belangrijke wijzigingen dergelijke als-wijziging in het gedrag van een bestaande taak, wijziging van de processen gebruiken van gegevens van deze taken enzovoort. Een compatibiliteitsniveau wordt gebruikt voor belangrijke wijzigingen die zijn geïntroduceerd in Stream Analytics. Belangrijke wijzigingen zijn altijd met een nieuwe compatibiliteitsniveau geïntroduceerd. 

Compatibiliteitsniveau zorgt ervoor dat bestaande taken worden uitgevoerd zonder een storing. Wanneer u een nieuwe Stream Analytics-taak maakt, is het een aanbevolen procedure om deze te maken met behulp van de meest recente compatibiliteitsniveau die voor u beschikbaar is. 
 
## <a name="set-a-compatibility-level"></a>Stel een compatibiliteitsniveau 

Compatibiliteitsniveau bepaalt het runtimegedrag van een stream analytics-taak. U kunt het compatibiliteitsniveau voor een Stream Analytics-taak kunt instellen met behulp van de portal of met behulp van de [oproep voor de REST-API maken](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Azure Stream Analytics ondersteunt momenteel twee compatibiliteit niveaus-"1.0" en '1.1'. Het compatibiliteitsniveau is standaard ingesteld op "1.0", dat werd geïntroduceerd bij algemene beschikbaarheid van Azure Stream Analytics. Voor het bijwerken van de standaardwaarde, gaat u naar uw bestaande Stream Analytics-taak > Selecteer de **compatibiliteitsniveau** optie **configureren** sectie en wijzig de waarde. 

Zorg ervoor dat u de taak stoppen voordat u het compatibiliteitsniveau bijwerkt. U kunt het compatibiliteitsniveau niet bijwerken als de taak actief is is. 

![Compatibiliteitsniveau van Stream Analytics in Azure portal](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

 
Wanneer u het compatibiliteitsniveau bijwerkt, wordt de taak met de syntaxis die overeenkomt met het geselecteerde compatibiliteitsniveau gevalideerd door de T-SQL-compiler. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Belangrijke wijzigingen in de meest recente compatibiliteitsniveau (1.1)

De volgende belangrijke wijzigingen zijn geïntroduceerd in compatibiliteitsniveau 1.1 van:

* **Service Bus-XML-indeling**  

  * **vorige versies:** Azure Stream Analytics gebruikt DataContractSerializer, zodat de inhoud van het bericht XML-tags opgenomen. Bijvoorbeeld:
    
   @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ "SensorId": "1", "temperatuur": 64\}\u0001 

  * **huidige versie:** inhoud van het bericht bevat de stream rechtstreeks met geen andere labels. Bijvoorbeeld:
  
   {"SensorId": "1", "temperatuur": 64} 
 
* **Permanente hoofdlettergevoeligheid voor veldnamen**  

  * **vorige versies:** veldnamen zijn gewijzigd in kleine letters wanneer verwerkt door de Azure Stream Analytics-engine. 

  * **huidige versie:** hoofdlettergevoeligheid voor veldnamen worden bewaard wanneer ze worden verwerkt door de Azure Stream Analytics-engine. 

  > [!NOTE] 
  > Permanente hoofdlettergevoeligheid is nog niet beschikbaar voor Stream Analytics-taken die worden gehost met behulp van Edge-omgeving. Als gevolg hiervan, worden alle veldnamen geconverteerd naar kleine letters, als de taak voor edge-apparaten wordt gehost. 

* **FloatNaNDeserializationDisabled**  

  * **vorige versies:** CREATE TABLE-opdracht heeft geen filter voor gebeurtenissen met NaN (Not a Number. Bijvoorbeeld oneindig, -oneindig) in een FLOAT-kolom typt, omdat deze buiten het bereik beschreven voor deze getallen zijn.

  * **huidige versie:** CREATE TABLE kunt u een sterke schema opgeven. De Stream Analytics-engine valideert dat de gegevens aan dit schema voldoen. De opdracht kunt u gebeurtenissen met NaN waarden filteren met dit model. 

* **Schakel automatische upcast voor datum/tijd-tekenreeksen in JSON.**  

  * **vorige versies:** de JSON-parser zou automatisch opgewaardeerd tekenreeks waarden met de datum/tijdzone-informatie naar DateTime typt en vervolgens converteren naar UTC. Dit leidde tot verlies van informatie over de tijdzone.

  * **huidige versie:** bestaat niet meer automatisch opgewaardeerd van tekenreekswaarden met datum/tijdzone-informatie naar DateTime-type. Als gevolg hiervan, wordt informatie over de tijdzone bewaard. 

## <a name="next-steps"></a>Volgende stappen
* [Azure Stream Analytics-invoer oplossen](stream-analytics-troubleshoot-input.md)
* [Stream Analytics-Resource health-blade](stream-analytics-resource-health.md)
