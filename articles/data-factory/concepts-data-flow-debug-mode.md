---
title: Azure Data Factory gegevensstroom foutopsporingsmodus toewijzen
description: Start een interactieve foutopsporingssessie bij het bouwen van gegevens stromen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: d3cabbcc04766d4a209ecf96db3f842902b465a5
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792392"
---
# <a name="mapping-data-flow-debug-mode"></a>Foutopsporingsmodus voor de stroom van de gegevens van de toewijzing

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory toewijzing gegevensstroom heeft een foutopsporingsmodus, die kan worden ingeschakeld met de knop gegevens Flow foutopsporing aan de bovenkant van het ontwerpoppervlak. Wanneer gegevensstromen, foutopsporingsmodus instellen op het ontwerpen kunt u interactief Bekijk hoe de gegevens transformeren vorm te geven tijdens het bouwen en fouten opsporen in uw data-stromen. De foutopsporingssessie kan worden gebruikt, zowel in de gegevensstroom sessies, evenals tijdens het uitvoeren van de pijplijn foutopsporing van gegevensstromen.

![Fouten opsporen in knop](media/data-flow/debugbutton.png "knop foutopsporing")

## <a name="overview"></a>Overzicht
Als foutopsporingsmodus ingeschakeld is, maakt u interactief de gegevensstroom met een actieve Spark-cluster. De sessie wordt gesloten wanneer u fouten opsporen in Azure Data Factory uitschakelen. U moet rekening houden met de per uur kosten in rekening gebracht door de Azure Databricks gedurende de tijd die u de foutopsporingssessie ingeschakeld hebt.

In de meeste gevallen is het raadzaam om te maken van uw gegevens stromen in de foutopsporingsmodus zodat u kunt uw bedrijfslogica valideren en uw gegevenstransformaties bekijken voordat u uw werk in Azure Data Factory publiceert. U moet ook de knop "Debug" in het deelvenster van de pijplijn gebruiken om uw gegevensstroom binnen een pijplijn te testen.

## <a name="debug-mode-on"></a>Fouten opsporen in de modus op
Als u op de foutopsporingsmodus gebruikt overschakelt, wordt u gevraagd een zijpaneel formulier die u moet verwijzen naar het interactieve Azure Databricks-cluster en selecteer opties voor de bron-steekproeven. U moet een interactieve cluster op basis van Azure Databricks gebruiken en selecteer een van beide een grootte van de steekproeven van elk uw bron-transformaties of kies een tekstbestand dat u wilt gebruiken voor de testgegevens van uw.

<img src="media/data-flow/upload.png" width="400">

> [!NOTE]
>Bij uitvoering in de foutopsporingsmodus in gegevensstroom, uw gegevens niet worden geschreven naar de Sink transformeren. Een foutopsporingssessie is bedoeld om te fungeren als een test > basis voor uw transformaties. Sinks zijn niet vereist tijdens de foutopsporing en worden genegeerd in de gegevensstroom. Als u wilt testen met het schrijven van gegevens > in uw Sink de gegevensstroom worden uitgevoerd vanuit een Azure Data Factory-pijplijn en gebruikt u de uitvoering fouten opsporen in een pijplijn.

## <a name="debug-settings"></a>Instellingen voor foutopsporing
Instellingen voor foutopsporing kan worden voor elke bron van de gegevensstroom wordt weergegeven in het paneel aan clientzijde en kan ook worden bewerkt door te selecteren in uw "source instellingen' op de werkbalk gegevensstroom ontwerper. U kunt de limieten en/of het bestandsbron moet worden gebruikt voor elke hier uw bron-transformatie selecteren. Er zijn de rijlimieten in deze instelling alleen voor de huidige foutopsporingssessie. U kunt ook de instelling van de steekproef nemen in de bron gebruiken voor het beperken van rijen in de bron-transforamtion.

## <a name="cluster-status"></a>Clusterstatus
Er is een statusindicator cluster aan de bovenkant van het ontwerpoppervlak dat groen inschakelen wanneer het cluster gereed voor foutopsporing is. Als uw cluster al warm is, wordt vrijwel onmiddellijk de groene indicator weergegeven. Als uw cluster is niet al wordt uitgevoerd wanneer u de foutopsporingsmodus hebt ingevoerd, hebt u wacht 5-7 minuten voor het cluster om te activeren. Het licht indicator zijn gele totdat deze klaar is. Zodra het cluster gereed voor foutopsporing gegevensstroom is, wordt de indicator lichte groen inschakelen.

Wanneer u klaar bent met uw foutopsporing, schakel de foutopsporing uitschakelen zodat uw Azure Databricks-cluster kunt beëindigen.

<img src="media/data-flow/datapreview.png" width="400">

## <a name="data-preview"></a>Voorbeeld van gegevens
Met fouten opsporen op het tabblad Voorbeeld van gegevens wordt licht weergegeven op het onderste deelvenster. Zonder de foutopsporingsmodus op ziet gegevensstroom u alleen de huidige metagegevens en afmelden bij elk van uw transformaties op het tabblad inspecteren. Voorbeeld van de gegevens wordt alleen het aantal rijen dat u hebt ingesteld als de limiet in uw instellingen voor gegevensbron opvragen. Mogelijk moet u op 'Gegevens op te halen' voorbeeld van de gegevens te vernieuwen.

<img src="media/data-flow/stats.png" width="400">

## <a name="data-profiles"></a>Gegevens-profielen
Te selecteren van de afzonderlijke kolommen in de preview-tabblad van gegevens wordt pop-upvenster op de helemaal rechts van het gegevensraster met gedetailleerde statistieken over elk veld in een grafiek. Azure Data Factory maakt om te bepalen op basis van de steekproef nemen voor gegevens van het type grafiek om weer te geven. Hoge kardinaliteit velden wordt standaard NULL / NOT NULL grafieken terwijl categorische en numerieke gegevens die weinig kardinaliteit staafdiagrammen met gegevensfrequentie waarde wordt weergegeven. U ziet ook maximale / len lengte van tekenreeksvelden, min / max-waarden in numerieke velden, standaard ontwikkelen, percentielen, aantallen en gemiddelde. 

<img src="media/data-flow/chart.png" width="400">

## <a name="next-steps"></a>Volgende stappen

Wanneer u bent klaar gebouw en foutopsporing van uw gegevensstroom [uitvoeren via een pijplijn.](control-flow-execute-data-flow-activity.md)

Bij het testen van uw pijplijn met een gegevensstroom, gebruikt u de pijplijn [foutopsporing uitvoeren optie kan worden uitgevoerd.](iterative-development-debugging.md)
