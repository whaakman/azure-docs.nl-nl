---
title: Nieuwe en gewijzigde bestanden door LastModifiedDate met Azure Data Factory kopiëren | Microsoft Docs
description: Informatie over het gebruik van een oplossingssjabloon om nieuwe en gewijzigde bestanden door LastModifiedDate met Azure Data Factory te kopiëren.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 3/8/2019
ms.openlocfilehash: cae75f4d64c8b3f74cc40e94a675c0f10a6bd9ec
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2019
ms.locfileid: "58111936"
---
# <a name="copy-new-and-changed-files-by-lastmodifieddate-with-azure-data-factory"></a>Nieuwe en gewijzigde bestanden door LastModifiedDate met Azure Data Factory kopiëren

Dit artikel beschrijft een oplossingssjabloon die u kunt nieuwe en gewijzigde bestanden alleen door LastModifiedDate in een store, op basis van een bestand kopiëren naar een bestemming-store. 

## <a name="about-this-solution-template"></a>Over deze oplossingssjabloon

Deze sjabloon eerst selecteert u de nieuwe en gewijzigde bestanden alleen door hun kenmerken **LastModifiedDate**, en worden de geselecteerde bestanden uit de brongegevensopslag gekopieerd naar de bestemming data store.

De sjabloon bevat één activiteit:
- **Kopie** nieuwe en gewijzigde bestanden alleen door LastModifiedDate in een store, bestand kopiëren naar een doelarchief.

De sjabloon definieert vier parameters:
-  *FolderPath_Source* is het pad naar de map waar de bestanden kan worden gelezen uit de bron-store. U moet de standaardwaarde vervangen door uw eigen mappad.
-  *FolderPath_Destination* is het pad naar de map waar u bestanden kopiëren naar het doelarchief. U moet de standaardwaarde vervangen door uw eigen mappad.
-  *LastModified_From* wordt gebruikt voor het selecteren van de bestanden waarvan LastModifiedDate het kenmerk is na of gelijk zijn aan deze datum / tijdwaarde.  Als u wilt de nieuwe bestanden die alleen, die niet is gekopieerd laatst, kan deze datum / tijdwaarde de tijd waarop de pijplijn is geactiveerd nadat deze tijd van laatste zijn. U kunt de standaardwaarde ' 2019 vervangen-02-01T00:00:00Z' aan de verwachte LastModifiedDate in UTC-tijdzone. 
-  *LastModified_To* wordt gebruikt om de bestanden waarvan LastModifiedDate het kenmerk vóór deze datum / tijdwaarde is te selecteren. Als u wilt de nieuwe bestanden die alleen, die niet is gekopieerd laatst, kan deze datum / tijdwaarde de huidige tijd zijn.  U kunt de standaardwaarde ' 2019 vervangen-02-01T00:00:00Z' aan de verwachte LastModifiedDate in UTC-tijdzone. 

## <a name="how-to-use-this-solution-template"></a>Het gebruik van deze oplossingssjabloon

1. Ga naar de sjabloon **kopieert u nieuwe bestanden alleen door LastModifiedDate**. Maak een **nieuw** verbinding met uw bron-opslag-archief. De bron-opslag-store is waarnaar u wilt kopiëren van bestanden uit.

    ![Maak een nieuwe verbinding met de bron](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate1.png)
    
2. Selecteer eerst de opslag **Type**. Nadat u dat de opslag invoer **accountnaam** en de **accountsleutel**. Selecteer ten slotte **voltooien**.

    ![Voer een verbindingsreeks](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate2.png)
    
3. Maak een **nieuw** verbinding met uw doelarchief. Het doelarchief is waarnaar u wilt kopiëren van bestanden. U moet ook de verbindingsgegevens van de gegevens doelarchief vergelijkbaar invoer als stap 2.

    ![Maak een nieuwe verbinding met het doel](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate3.png)

4. Selecteer **Gebruik deze sjabloon**.

    ![Deze sjabloon gebruiken](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate4.png)
    
5. U ziet de pijplijn die beschikbaar zijn in het deelvenster, zoals wordt weergegeven in het volgende voorbeeld:

    ![De pijplijn weergeven](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate5.png)

6. Selecteer **Debug**, schrijft de waarde voor de **Parameters** en selecteer **voltooien**.  In de volgende afbeelding stellen we de parameters als volgt.
   - **FolderPath_Source** = **/source/**
   - **FolderPath_Destination** = **/destination/**
   - **LastModified_From** =  **2019-02-01T00:00:00Z**
   - **LastModified_To** = **2019-03-01T00:00:00Z**
    
     Het voorbeeld is de bestanden die het laatst zijn gewijzigd in de tijd tussen de aangeeft *2019-02-01T00:00:00Z* en *2019-03-01T00:00:00Z* moeten worden gekopieerd vanuit een map */source/*  naar een map */destination/*.  U kunt deze met uw eigen parameters vervangen.
    
     ![De pijplijn uitvoeren](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate6.png)

7. Bekijk het resultaat. U ziet alleen de bestanden die binnen de geconfigureerde het laatst is gewijzigd timespan is gekopieerd naar de doel-store.

    ![Bekijk het resultaat](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate7.png)
    
8. U kunt nu een tumblingvenstertrigger windows-trigger voor het automatiseren van deze pijplijn toevoegen zodat de pijplijn kan nieuwe en gewijzigde bestanden alleen door LastModifiedDate altijd periodiek kopiëren.  Selecteer **toevoegen trigger**, en selecteer **nieuw/bewerken**.

    ![Bekijk het resultaat](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate8.png)
    
9. In de **Triggers toevoegen** venster **+ nieuw**.

    ![Bekijk het resultaat](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate9.png)

10. Selecteer **Tumblingvenster** instellen voor het triggertype **elke 15 minuten** als het terugkeerpatroon (u kunt op elk gewenst moment interval wijzigen), en selecteer vervolgens **volgende**.

    ![Trigger maken](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate10.png)    
    
11. De waarde voor de **Parameters voor Pijplijnuitvoeringen** als volgende, en selecteer **voltooien**.
    - **FolderPath_Source** = **/source/**.  U kunt vervangen door uw map in de bron-gegevensopslag.
    - **FolderPath_Destination** = **/destination/**.  U kunt vervangen door uw map in doelgegevensarchief.
    - **LastModified_From** =  **@trigger().outputs.windowStartTime**.  Er is een systeemvariabele door de trigger bepalen van de tijd wanneer de pijplijn de laatste keer is geactiveerd.
    - **LastModified_To** = **@trigger(). outputs.windowEndTime**.  Er is een systeemvariabele door de trigger voor het bepalen van de tijd wanneer de pijplijn dit moment wordt geactiveerd.
    
    ![Invoerparameters](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate11.png)
    
12. Selecteer **Alles publiceren**.
    
    ![Alles publiceren](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate12.png)

13. Hiermee maakt u nieuwe bestanden in de bronmap van de brongegevensopslag.  U nu wachten op voor de pijplijn wordt automatisch geactiveerd en wordt alleen de nieuwe bestanden worden gekopieerd naar de doel-store.

14. Selecteer **bewaking** tabblad in het linkernavigatievenster, en wacht ongeveer 15 minuten als het terugkeerpatroon van de trigger is ingesteld op om de 15 minuten. 

    ![Schakel bewaking](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate14.png)

15. Bekijk het resultaat. U ziet de pijplijn wordt geactiveerd automatisch om de 15 minuten, en wordt alleen de nieuwe of gewijzigde bestanden uit de brongegevensopslag worden gekopieerd naar het doelarchief in elke pijplijnuitvoering.

    ![Bekijk het resultaat](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate15.png)
    
## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Data Factory](introduction.md)