---
title: Azure Data Factory gegevensstroom foutopsporingsmodus toewijzen
description: Start een interactieve foutopsporingssessie bij het bouwen van gegevens stromen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: d86725718217caf7fd1d9dd6d5d67362e5de7270
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147368"
---
# <a name="mapping-data-flow-debug-mode"></a>Foutopsporingsmodus voor de stroom van de gegevens van de toewijzing

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory toewijzing gegevensstroom van foutopsporingsmodus kan worden overgeschakeld op met de knop "Gegevens Flow Debug" aan de bovenkant van het ontwerpoppervlak. Bij het ontwerpen van gegevensstromen, foutopsporingsmodus inschakelen, kunt u interactief Bekijk hoe de gegevens transformeren vorm te geven tijdens het bouwen en fouten opsporen in uw data-stromen. De foutopsporingssessie kan worden gebruikt, zowel in de gegevensstroom sessies, evenals tijdens het uitvoeren van de pijplijn foutopsporing van gegevensstromen.

![Fouten opsporen in knop](media/data-flow/debugbutton.png "knop foutopsporing")

## <a name="overview"></a>Overzicht
Als foutopsporingsmodus ingeschakeld is, bouwt u interactief de gegevensstroom met een actieve Spark-cluster. De sessie wordt gesloten wanneer u fouten opsporen in Azure Data Factory uitschakelen. U moet rekening houden met de per uur kosten in rekening gebracht door de Azure Databricks gedurende de tijd die u de foutopsporingssessie ingeschakeld hebt.

In de meeste gevallen is het raadzaam om te maken van uw gegevens stromen in de foutopsporingsmodus zodat u kunt uw bedrijfslogica valideren en uw gegevenstransformaties bekijken voordat u uw werk in Azure Data Factory publiceert. Gebruik de knop "Debug" in het venster pijplijn om uw gegevensstroom binnen een pijplijn te testen.

> [!NOTE]
> Tijdens de foutopsporing modus licht groen op de werkbalk van de Data Factory is, u in rekening gebracht tegen het tarief foutopsporing gegevensstroom van 8 kernen/uur reken-en algemene met een 60 minuten time-to-live 

> [!NOTE]
>Bij uitvoering in de foutopsporingsmodus in gegevensstroom, uw gegevens niet worden geschreven naar de Sink transformeren. Een foutopsporingssessie is bedoeld om te fungeren als een testframework voor uw transformaties. Sinks zijn niet vereist tijdens de foutopsporing en worden genegeerd in de gegevensstroom. Als u testen met het schrijven van gegevens in uw Sink wilt, de gegevensstroom worden uitgevoerd vanuit een Azure Data Factory-pijplijn en de uitvoering fouten opsporen in een pijplijn gebruiken.

## <a name="debug-settings"></a>Instellingen voor foutopsporing
Fouten opsporen in instellingen kunnen worden bewerkt door te klikken op 'Instellingen voor fouten opsporen in' op de werkbalk canvas gegevensstroom. U kunt de limieten en/of de bron moet worden gebruikt voor elk van de bron-transformaties hier selecteren. Er zijn de rijlimieten in deze instelling alleen voor de huidige foutopsporingssessie. U kunt ook de aan fasering gekoppelde service moet worden gebruikt voor een SQL DW-bron selecteren. 

![Instellingen voor foutopsporing](media/data-flow/debug-settings.png "instellingen voor foutopsporing")

## <a name="cluster-status"></a>De clusterstatus
Er is een statusindicator cluster aan de bovenkant van het ontwerpoppervlak dat groen inschakelen wanneer het cluster gereed voor foutopsporing is. Als uw cluster al warm is, wordt vrijwel onmiddellijk de groene indicator weergegeven. Als uw cluster is niet al wordt uitgevoerd wanneer u de foutopsporingsmodus hebt ingevoerd, u klikt u vervolgens hebt moet worden gewacht 5-7 minuten voor het cluster om te activeren. De indicator wordt draaien totdat de gereed.

Wanneer u klaar met uw foutopsporing bent uit te schakelen de switch foutopsporing zodat uw Azure Databricks-cluster kunt beëindigd en wordt u niet meer gefactureerd voor foutopsporing activiteit.

## <a name="data-preview"></a>Voorbeeld van gegevens
Met fouten opsporen op het tabblad Voorbeeld van gegevens wordt licht weergegeven op het onderste deelvenster. Zonder de foutopsporingsmodus op ziet gegevensstroom u alleen de huidige metagegevens en afmelden bij elk van uw transformaties op het tabblad inspecteren. Voorbeeld van de gegevens wordt alleen het aantal rijen dat u hebt ingesteld als de limiet in uw foutopsporingsinstellingen query. Mogelijk moet u op 'Gegevens op te halen' voorbeeld van de gegevens te vernieuwen.

![Voorbeeld van gegevens](media/data-flow/datapreview.png "voorbeeldweergave van gegevens")

## <a name="data-profiles"></a>Gegevens-profielen
Afzonderlijke kolommen selecteren in het tabblad gegevens Preview-versie wordt weergegeven op de helemaal rechts van het gegevensraster met gedetailleerde statistieken over elk veld in een grafiek. Azure Data Factory maakt om te bepalen op basis van de steekproef nemen voor gegevens van het type grafiek om weer te geven. Hoge kardinaliteit velden standaard /niet-NULL grafieken terwijl categorische en numerieke gegevens die weinig kardinaliteit staafdiagrammen met gegevensfrequentie waarde wordt weergegeven. U ziet ook maximale/len lengte van tekenreeksvelden, min/max-waarden in numerieke velden, standaard dev percentielen, aantallen en gemiddelde. 

![Kolomstatistieken](media/data-flow/stats.png "kolomstatistieken")

## <a name="next-steps"></a>Volgende stappen

Zodra u klaar bent het bouwen van en opsporen van fouten in de gegevensstroom [uitvoeren via een pijplijn.](control-flow-execute-data-flow-activity.md)

Bij het testen van uw pijplijn met een gegevensstroom, gebruikt u de pijplijn [foutopsporing uitvoeren optie kan worden uitgevoerd.](iterative-development-debugging.md)
