---
title: Iteratieve ontwikkeling en foutopsporing in Azure Data Factory | Microsoft Docs
description: Informatie over het ontwikkelen en testen van Data Factory-pijplijnen iteratief in de Azure portal.
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.date: 05/14/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.openlocfilehash: ef30958ed0a88853b20278e6d628639ffc0cea34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619470"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Iteratieve ontwikkeling en foutopsporing met Azure Data Factory

Azure Data Factory kunt u iteratief ontwikkelen en testen van Data Factory-pijplijnen.

Bekijk de volgende video voor een inleiding acht minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="iterative-debugging-features"></a>Iteratieve functies voor foutopsporing
Maken van de pijplijnen en ik wil deze test wordt uitgevoerd met behulp van de **Debug** mogelijkheid in het canvas pijplijn zonder een één regel code te schrijven.

![Mogelijkheid tot op het canvas pijplijn voor foutopsporing](media/iterative-development-debugging/iterative-development-image1.png)

Weergave de resultaten van de test wordt uitgevoerd in de **uitvoer** venster van het canvas pijplijn.

![Venster van de uitvoer van het canvas pijplijn](media/iterative-development-debugging/iterative-development-image2.png)

Nadat een test uitgevoerd is geslaagd, meer activiteiten toevoegen aan uw pijplijn en foutopsporing in een herhalende manier worden voortgezet. U kunt ook **annuleren** testuitvoering terwijl deze uitgevoerd wordt.

![Annuleren van een test uitvoeren](media/iterative-development-debugging/iterative-development-image3.png)

Wanneer u wordt uitgevoerd test, hoeft u uw wijzigingen aan de gegevensfactory publiceren voordat u de optie **Debug**. Dit is handig in situaties waar u controleren of de wijzigingen werken wilt zoals verwacht voordat u de werkstroom van de data factory bijwerken.

## <a name="more-info-about-debugging"></a>Meer informatie over foutopsporing

1. De test wordt uitgevoerd gestart met de **Debug** mogelijkheid zijn niet beschikbaar in de lijst op de **Monitor** tabblad. U kunt alleen Zie wordt uitgevoerd met triggered **Trigger nu**, **planning**, of **Tumblingvenster** activeert de **Monitor** tabblad. Ziet u de laatste test uitvoeren gestart met de **Debug** mogelijkheden in de **uitvoer** venster van het canvas pijplijn.

2. Selecteren **Debug** daadwerkelijk wordt uitgevoerd de pijplijn. Dus bijvoorbeeld kopieert als de pijplijn kopieeractiviteit bevat, de testcyclus gegevens van bron naar doel. Als gevolg hiervan is het raadzaam test mappen in uw activiteiten kopiëren en andere activiteiten te gebruiken als u fouten opspoort. Nadat u de pijplijn hebt gecontroleerd, overschakelen naar de werkelijke mappen die u wilt gebruiken tijdens normale bewerkingen.

## <a name="setting-breakpoints-for-debugging"></a>Onderbrekingspunten instellen voor foutopsporing

Data Factory kunt u fouten opsporen in totdat u een bepaalde activiteit op het canvas pijplijn. Alleen een onderbrekingspunt in de activiteit tot welke u wilt testen, en selecteer plaatsen **Debug**. Data Factory zorgt ervoor dat de test wordt uitgevoerd totdat de activiteit onderbrekingspunt alleen op het canvas pijplijn. Dit *Debug totdat* functie is handig als u niet wilt testen van de hele pipeline, maar alleen een subset van activiteiten in de pijplijn.

![Onderbrekingspunten op het canvas pijplijn](media/iterative-development-debugging/iterative-development-image4.png)

Stel een onderbrekingspunt in door een element op het canvas pijplijn te selecteren. Een *Debug totdat* optie wordt weergegeven als een lege rode cirkel in de rechterbovenhoek van het element.

![Voordat u een onderbrekingspunt instelt op het geselecteerde element](media/iterative-development-debugging/iterative-development-image5.png)

Nadat u hebt geselecteerd de *Debug totdat* optie, verandert deze in een gevulde rode cirkel om aan te geven de het onderbrekingspunt is ingeschakeld.

![Na het instellen van een onderbrekingspunt voor het geselecteerde element](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Volgende stappen
[Continue integratie en implementatie in Azure Data Factory](continuous-integration-deployment.md)
