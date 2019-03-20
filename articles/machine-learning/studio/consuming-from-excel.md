---
title: Webservice in Excel gebruiken
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio maakt het eenvoudig om aan te roepen webservices rechtstreeks vanuit Excel zonder code te schrijven.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: ef1d8f1a72c5936ff661636c4c51acf439a0a5ea
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855194"
---
# <a name="consuming-an-azure-machine-learning-studio-web-service-from-excel"></a>Een Azure Machine Learning Studio-webservice uit Excel gebruiken

 Azure Machine Learning Studio maakt het eenvoudig om aan te roepen webservices rechtstreeks vanuit Excel zonder code te schrijven.

Als u Excel 2013 (of hoger) of Excel Online, gebruikt, raden wij aan dat u de Excel [Excel-invoegtoepassing](excel-add-in-for-web-services.md).



## <a name="steps"></a>Stappen
Een webservice publiceren. [Zelfstudie 3: Tegoed risicomodel implementeren](tutorial-part3-credit-risk-deploy.md) wordt uitgelegd hoe u om dit te doen. De Excel-werkmap-functie wordt momenteel alleen ondersteund voor aanvraag/antwoord-services die een enkele uitvoer (dat wil zeggen, een enkel scoring label) hebben. 

Als u een webservice hebt, klikt u op de **WEBSERVICES** sectie aan de linkerkant van de studio en selecteer vervolgens de webservice om te gebruiken vanuit Excel.

**Klassieke webservice**

1. Op de **DASHBOARD** tabblad voor de webservice is een rij voor de **aanvraag/antwoord** service. Als deze service een enkele uitvoer heeft, ziet u de **Excel-werkmap downloaden** koppeling in die rij.

    ![Excel-werkmap met behulp van de portal van de Studio-webservice downloaden](./media/consuming-from-excel/excellink.png)
2. Klik op **Excel-werkmap downloaden**.

**Nieuwe webservice**

1. Selecteer in de portal voor Azure Machine Learning-webservice **verbruiken**.
2. Op de pagina verbruiken in de **Web service verbruik opties** sectie, klikt u op het Excel-pictogram.

**Met behulp van de werkmap**

1. Open de werkmap.
2. Er wordt een beveiligingswaarschuwing weergegeven. Klik op de **Enable Editing** knop.

    ![Als u wilt verwijderen van de beveiligingswaarschuwing beveiligde weergave bewerken inschakelen](./media/consuming-from-excel/enableeditting.png)
3. Er wordt een beveiligingswaarschuwing weergegeven. Klik op de **inhoud inschakelen** knop macro's uitvoeren op uw werkblad.

    ![Inschakelen van inhoud voor het verwijderen van de beveiligingswaarschuwing macro's uitschakelen](./media/consuming-from-excel/enablecontent.png)
4. Zodra de macro's zijn ingeschakeld, wordt een tabel wordt gegenereerd. Kolommen in blauw zijn vereist als invoer voor de webservice RRS of **PARAMETERS**. Houd er rekening mee de uitvoer van de service RRS **VOORSPELDE waarden** in het groen. Wanneer alle kolommen voor een bepaalde rij zijn ingevuld, de werkmap wordt automatisch de scoring-API-aanroepen en de beoordeelde resultaten worden weergegeven.

    ![Tabel voor invoer van de parameter en de resulterende voorspelde waarden](./media/consuming-from-excel/sampletable.png)
5. Voor het scoren van meer dan één rij opvulling van de tweede rij met gegevens en de voorspelde waarden worden geproduceerd. U kunt zelfs meerdere rijen in één keer plakken.

U kunt een van de Excel-functies (grafieken, power map, voorwaardelijke opmaak, enzovoort) met de voorspelde waarden gebruiken om u te helpen bij het visualiseren van de gegevens.

## <a name="sharing-your-workbook"></a>Delen van uw werkmap
Voor de macro's om te werken, moet uw API-sleutel deel uitmaken van het werkblad. Dat betekent dat u de werkmap moet delen alleen met entiteiten/personen die u vertrouwt.

## <a name="automatic-updates"></a>Automatische updates
Een RRS-aanroep wordt uitgevoerd in deze twee gevallen:

1. De eerste keer dat een rij bevat inhoud in alle van de **PARAMETERS**
2. Telkens wanneer u een van de **PARAMETERS** wijzigingen in een rij die al had de **PARAMETERS** ingevoerd.