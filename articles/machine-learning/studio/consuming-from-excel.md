---
title: Een Machine Learning-webservice vanuit Excel gebruiken | Microsoft Docs
description: Een Azure Machine Learning-webservice vanuit Excel gebruiken
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2017
ms.author: tedway
ms.openlocfilehash: 9a8b39853e1a90815758af1d8fd772db7cc18ac5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a>Een Azure Machine Learning-webservice gebruiken vanuit Excel
 Azure Machine Learning Studio kunt gemakkelijk aanroepen van webservices rechtstreeks vanuit Excel zonder code te schrijven.

Als u Excel 2013 (of hoger) of Excel Online gebruikt, wordt aangeraden dat u de Excel [Excel-invoegtoepassing](excel-add-in-for-web-services.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a>Stappen
Een webservice publiceert. [Deze pagina](walkthrough-5-publish-web-service.md) wordt uitgelegd hoe u dit doen. De functie van de Excel-werkmap is momenteel alleen ondersteund voor de aanvraag/antwoord-services die één uitvoer (dat wil zeggen, een enkel scoreprofiel label). 

Zodra u een webservice hebt, klikt u op in de **WEBSERVICES** sectie aan de linkerkant van de studio en selecteer vervolgens de web-service gebruiken vanuit Excel.

**Klassieke webservice**

1. Op de **DASHBOARD** tabblad voor de webservice is een rij voor de **aanvragen/reacties** service. Als deze service één uitvoer heeft, ziet u de **Excel-werkmap downloaden** koppeling in die rij.
   
    ![][1]
2. Klik op **Excel-werkmap downloaden**.

**Nieuwe webservice**

1. Selecteer in de portal voor Azure Machine Learning-webservice **verbruiken**.
2. Op de pagina verbruiken in de **Web-verbruik serviceopties** sectie, klikt u op het Excel-pictogram.

**Met behulp van de werkmap**

1. Open de werkmap.
2. Er verschijnt een beveiligingswaarschuwing; Klik op de **bewerken inschakelen** knop.
   
    ![][2]
3. Een beveiligingswaarschuwing weergegeven. Klik op de **inhoud inschakelen** knop voor het uitvoeren van macro's in het werkblad.
   
    ![][3]
4. Zodra de macro's zijn ingeschakeld, wordt een tabel wordt gegenereerd. Kolommen in blauw zijn vereist als invoer in de webservice RRS of **PARAMETERS**. De uitvoer van de service RRS **VOORSPELDE waarden** in groen. Wanneer alle kolommen voor een bepaalde rij gevuld zijn, de werkmap automatisch de score API aanroept en de scored resultaten worden weergegeven.
   
    ![][4]
5. Als u wilt beoordelen meer dan één rij, opvulling van de tweede rij met gegevens en de voorspelde waarden worden geproduceerd. U kunt zelfs meerdere rijen in één keer plakken.

U kunt de Excel-functies (grafieken, power-kaart, voorwaardelijke opmaak, enz.) met de voorspelde waarden gebruiken om u te helpen de gegevens visualiseren.    

## <a name="sharing-your-workbook"></a>Delen van uw werkmap
Voor de macro's werken, moet uw API-sleutel deel uitmaken van het werkblad. Dat betekent dat u de werkmap moet delen alleen met entiteiten/personen die u vertrouwt.

## <a name="automatic-updates"></a>Automatische updates
In deze situaties wordt een RRS-aanroep uitgevoerd:

1. De eerste keer dat een rij bevat inhoud in alle bijbehorende **PARAMETERS**
2. Telkens wanneer u een van de **PARAMETERS** wijzigingen in een rij die alle waren bijbehorende **PARAMETERS** ingevoerd.

[1]: ./media/consuming-from-excel/excellink.png
[2]: ./media/consuming-from-excel/enableeditting.png
[3]: ./media/consuming-from-excel/enablecontent.png
[4]: ./media/consuming-from-excel/sampletable.png
