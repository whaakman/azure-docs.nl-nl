---
title: Iteratieve ontwikkeling en foutopsporing in Azure Data Factory | Microsoft Docs
description: Informatie over het ontwikkelen en testen van Data Factory-pijplijnen iteratief in Azure portal.
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
ms.openlocfilehash: a4d3f991dbba8a686c7242aabff11d9228300777
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865162"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Iteratieve ontwikkeling en foutopsporing met Azure Data Factory

Azure Data Factory kunt u iteratief ontwikkelen en fouten opsporen in Data Factory-pijplijnen.

Bekijk de volgende video voor een inleiding van acht minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="iterative-debugging-features"></a>Iteratieve foutopsporingsfuncties
Pijplijnen maken en ik wil deze test wordt uitgevoerd met behulp van de **Debug** mogelijkheden op het pijplijncanvas zonder één regel code te schrijven.

![Fouten opsporen in op het pijplijncanvas-mogelijkheid](media/iterative-development-debugging/iterative-development-image1.png)

Weergave de resultaten van de test wordt uitgevoerd in de **uitvoer** venster van de pijplijncanvas.

![Uitvoervenster van het pijplijncanvas](media/iterative-development-debugging/iterative-development-image2.png)

Nadat een testuitvoering is geslaagd, meer activiteiten toevoegen aan uw pijplijn en foutopsporing in een iteratief manier doorgaan. U kunt ook **annuleren** een testuitvoering terwijl deze uitgevoerd wordt.

![Een testuitvoering annuleren](media/iterative-development-debugging/iterative-development-image3.png)

Wanneer u wordt uitgevoerd test, hoeft u uw wijzigingen aan de data factory publiceren voordat u **Debug**. Dit is handig in situaties waar u om ervoor te zorgen dat de wijzigingen werken zoals verwacht voordat u de werkstroom van de data factory bijwerken.

## <a name="more-info-about-debugging"></a>Meer informatie over het opsporen van fouten

1. De test wordt uitgevoerd gestart met de **Debug** mogelijkheid zijn niet beschikbaar in de lijst op de **Monitor** tabblad. U kunt alleen Zie wordt uitgevoerd met geactiveerde **nu activeren**, **planning**, of **Tumblingvenster** wordt geactiveerd de **Monitor** tabblad. Ziet u de laatste test uitvoeren gestart met de **Debug** -mogelijkheid in de **uitvoer** venster van de pijplijncanvas.

2. Selecteren **Debug** daadwerkelijk voert de pijplijn. Dus, bijvoorbeeld als de pijplijn copy-activiteit bevat, de testuitvoering kopieert gegevens van bron naar bestemming. Als gevolg hiervan is het beste testen mappen in uw activiteiten kopiëren en andere activiteiten te gebruiken bij het opsporen van fouten. Nadat u de pijplijn opsporen hebt, schakelt u naar de werkelijke mappen die u wilt gebruiken in normale bewerkingen.

## <a name="setting-breakpoints-for-debugging"></a>Onderbrekingspunten instellen voor foutopsporing

Data Factory kunt u fouten opsporen in totdat u een bepaalde activiteit op het pijplijncanvas. Alleen een onderbrekingspunt in de activiteit tot welke u wilt testen, en selecteer plaatsen **Debug**. Data Factory zorgt ervoor dat de test wordt uitgevoerd tot het onderbrekingspunt-activiteit op het pijplijncanvas. Dit *Debug totdat* functie is handig als u niet wilt dat voor het testen van de hele pijplijn, maar alleen een subset van de activiteiten in de pijplijn.

![Onderbrekingspunten op het pijplijncanvas](media/iterative-development-debugging/iterative-development-image4.png)

Als u wilt een onderbrekingspunt instellen, selecteert u een element op het pijplijncanvas. Een *Debug totdat* optie wordt weergegeven als een lege rode cirkel in de rechterbovenhoek van het element.

![Voordat een onderbrekingspunt in op het geselecteerde element](media/iterative-development-debugging/iterative-development-image5.png)

Nadat u hebt geselecteerd de *Debug totdat* optie, verandert deze in een gevulde rode cirkel om aan te geven het onderbrekingspunt is ingeschakeld.

![Na het instellen van een onderbrekingspunt in op het geselecteerde element](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Volgende stappen
[Continue integratie en implementatie in Azure Data Factory](continuous-integration-deployment.md)
