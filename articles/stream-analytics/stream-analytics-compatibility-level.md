---
title: Het compatibiliteitsniveau voor Azure Stream Analytics-taken te begrijpen. | Microsoft Docs
description: Informatie over het compatibiliteitsniveau voor een Azure Stream Analytics-taak en belangrijke wijzigingen in de meest recente versie van het compatibiliteitsniveau instellen
keywords: Het compatibiliteitsniveau, streamen van gegevens
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
editor: 
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/03/2018
ms.author: sngun
ms.openlocfilehash: 0d89259d54fba0bd57881ec69cb61b5af6d603b5
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Het compatibiliteitsniveau voor Azure Stream Analytics-taken
 
Het compatibiliteitsniveau verwijst naar de release-specifieke gedrag van een Azure Stream Analytics-service. Azure Stream Analytics is een beheerde service, met de reguliere functie-updates en verbeteringen. Meestal worden updates automatisch beschikbaar gesteld aan eindgebruikers. Enkele nieuwe functies kunnen echter introduceren grote wijzigingen van dergelijke als-Wijzig in het gedrag van een bestaande taak, wijziging van de processen gebruiken van gegevens uit deze taken enz. Compatibiliteitsniveau wordt gebruikt om een grote wijziging die is geïntroduceerd in Stream Analytics vertegenwoordigen. Belangrijke wijzigingen worden altijd geïntroduceerd met nieuwe compatibiliteitsniveau. 

Het compatibiliteitsniveau zorgt ervoor dat bestaande taken worden uitgevoerd zonder een storing. Wanneer u een nieuwe Stream Analytics-taak maakt, wordt het aanbevolen om deze te maken met behulp van de meest recente compatibiliteitsniveau die voor u beschikbaar is. 
 
## <a name="set-a-compatibility-level"></a>Een compatibiliteitsniveau instellen 

Het compatibiliteitsniveau bepaalt het runtimegedrag van een stream analytics-taak. U kunt het compatibiliteitsniveau van een Stream Analytics-taak instellen met behulp van de portal of met behulp van de [taak REST API-aanroep maken](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Azure Stream Analytics ondersteunt momenteel twee compatibiliteit niveaus-"1.0" en '1.1'. Het compatibiliteitsniveau is standaard ingesteld op "1.0" dat tijdens de algemene beschikbaarheid van Azure Stream Analytics werd geïntroduceerd. Voor het bijwerken van de standaardwaarde, gaat u naar uw bestaande Stream Analytics-taak > Selecteer de **compatibiliteitsniveau** optie in **configureren** sectie en wijzig de waarde. 

Zorg ervoor dat u de taak stoppen voordat u het compatibiliteitsniveau bijwerkt. U kunt het compatibiliteitsniveau niet bijwerken als de taak actief is is. 

![Compatibiliteitsniveau in de portal](media\stream-analytics-compatibility-level/image1.png)

 
Wanneer u het compatibiliteitsniveau bijwerkt, valideert de T-SQL-compiler de taak met de syntaxis die overeenkomt met het geselecteerde compatibiliteitsniveau. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Belangrijke wijzigingen in de meest recente compatibiliteitsniveau (1.1)

De volgende belangrijke wijzigingen worden in compatibiliteitsniveau 1.1 geïntroduceerd:

* **Service Bus-XML-indeling**  

  * **vorige versies:** Azure Stream Analytics gebruikt DataContractSerializer, zodat de inhoud van het bericht XML-labels opgenomen. Bijvoorbeeld:
    
   @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{{'SensorId': '1', 'temperatuur': 64\}\u0001 

  * **huidige versie:** inhoud van het bericht bevat de stroom rechtstreeks met er zijn geen extra labels. Bijvoorbeeld:
  
   {{'SensorId': '1', 'temperatuur': 64} 
 
* **Persisting hoofdlettergevoeligheid voor de veldnamen**  

  * **vorige versies:** veldnamen zijn gewijzigd in kleine letters wanneer door de Azure Stream Analytics-engine verwerkt. 

  * **huidige versie:** hoofdlettergevoeligheid voor de veldnamen worden bewaard wanneer ze worden verwerkt door de Azure Stream Analytics-engine. 
 
* **FloatNaNDeserializationDisabled**  

  * **vorige versies:** CREATE TABLE-opdracht is geen filter voor gebeurtenissen met NaN (Not a Number. Bijvoorbeeld oneindig, -oneindig) in een FLOAT-kolom typen omdat ze buiten het gedocumenteerde bereik voor deze getallen zijn.

  * **huidige versie:** CREATE TABLE kunt u een sterk schema opgeven. De Stream Analytics-engine valideert dat de gegevens aan dit schema voldoen. Met dit model kan de opdracht filter gebeurtenissen met NaN-waarden. 

* **Schakel automatische upcast voor datum/tijd-tekenreeksen in JSON.**  

  * **vorige versies:** de JSON-parser zou automatisch opgewaardeerd tekenreeks waarden met datum/tijdzone-informatie naar DateTime typen en vervolgens weer converteren naar UTC. Dit resulteert in het verlies van de informatie over de tijdzone.

  * **huidige versie:** bestaat niet meer automatisch opgewaardeerd van tekenreekswaarden datum/tijdzone-informatie over het type DateTime. Als gevolg hiervan, wordt informatie over de tijdzone bewaard. 

## <a name="next-steps"></a>Volgende stappen
* [Gids voor probleemoplossing voor Azure Stream Analytics](stream-analytics-troubleshooting-guide.md)
* [Stream Analytics resourceblade health](stream-analytics-resource-health.md)