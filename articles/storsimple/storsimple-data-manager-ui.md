---
title: Gebruikersinterface van Microsoft Azure StorSimple Data Manager | Microsoft Docs
description: Beschrijft hoe u StorSimple Data Manager service UI (afgeschermd voorbeeld)
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a>Beheren met behulp van de service StorSimple Data Manager UI (afgeschermd voorbeeld)

Dit artikel wordt uitgelegd hoe u kunt de StorSimple Data Manager UI gegevenstransformatie voor gegevens die zich op de apparaten uit de StorSimple 8000 serie uitvoeren. De getransformeerde gegevens kunnen vervolgens worden gebruikt door andere Azure-services zoals Azure Media Services, Azure HDInsight Azure Machine Learning en Azure Search. 


## <a name="use-storsimple-data-transformation"></a>StorSimple gegevenstransformatie gebruiken

De StorSimple Data Manager is de bron waarbinnen gegevenstransformatie kan worden gemaakt. De Data Transformation service kunt u gegevens van uw on-premises StorSimple apparaat naar de blobs in Azure-opslag verplaatsen. In de werkstroom moet u daarom de details over uw StorSimple-apparaat en de gegevens van belang dat u wilt verplaatsen naar het opslagaccount opgeven.

### <a name="create-a-storsimple-data-manager-service"></a>Een StorSimple Data Manager-service maken

De volgende stappen uitvoeren om te maken van een StorSimple Data Manager-service.

1. Als u wilt maken van een StorSimple Data Manager-service, gaat u naar [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Klik op de  **+**  pictogram en de zoekcriteria voor StorSimple Data Manager. Klik op uw StorSimple Data Manager-service en klik vervolgens op **maken**.

3. Als uw abonnement voor het maken van deze service is ingeschakeld, ziet u de volgende blade.

    ![Maak een resource StorSimple gegevens Managers](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Voer de invoer en klik op **maken**. De opgegeven locatie moet zijn met uw storage-accounts en uw StorSimple Manager-service. Op dit moment worden alleen VS-West en West-Europa regio's ondersteund. Daarom moeten uw StorSimple Manager-service, Data Manager-service en het bijbehorende opslagaccount alle in de voorgaande ondersteunde regio's. Het duurt ongeveer een minuut om de service te maken.

### <a name="create-a-data-transformation-job-definition"></a>Een definitie van de transformatie-taak maken

Binnen een StorSimple Data Manager-service moet u een taakdefinitie voor transformatie van gegevens maken. De taakdefinitie van een geeft de details van de gegevens die u geïnteresseerd bent in het verplaatsen naar een opslagaccount in de oorspronkelijke indeling. 

De volgende stappen voor het maken van een nieuwe data transformation taakdefinitie uitvoeren.

1.  Navigeer naar de service die u hebt gemaakt. Klik op **+ taak definitie**.

    ![Klik op + taakdefinitie](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. De nieuwe taak definitie blade wordt geopend. Geef een naam op voor de taakdefinitie van de en klik op **bron**. In de **gegevensbron configureren** blade, geef de details van uw StorSimple-apparaat en de gegevens van belang.

    ![Taakdefinitie maken](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Aangezien dit een nieuwe Data Manager-service, worden geen gegevens opslagplaatsen geconfigureerd. Klik op als u uw StorSimple Manager als een gegevensopslagplaats **nieuwe toevoegen** in de vervolgkeuzelijst opslagplaats voor gegevens en klik op **gegevensopslagplaats toevoegen**.

4. Kies **StorSimple 8000-serie Manager** als de opslagplaats te typen van de eigenschappen van en uw **StorSimple Manager**. Voor de **Resource-Id** veld, moet u het nummer voor de **:** in de registratiesleutel van uw StorSimple manager.

    ![Gegevensbron maken](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Klik op **OK** wanneer u klaar bent. Hiermee slaat u de gegevensopslagplaats van uw en deze StorSimple Manager in andere taakdefinities zonder opnieuw invoeren van deze parameters kunnen worden gebruikt. Het duurt enkele seconden nadat u op **OK** voor de StorSimple Manager worden weergegeven in de vervolgkeuzelijst.

6.  In de **gegevensbron configureren** blade, voer de naam van het apparaat en de naam van het volume dat de gegevens van belang.

7.  In de **Filter** subsectie, voert u de hoofdmap waarin de gegevens van belang (dit veld moet beginnen met een `\`). U kunt ook een bestandsfilters hier toevoegen.

8.  De data transformation service werkt op de gegevens die naar de Azure via momentopnamen van is gepusht. Wanneer deze taak wordt uitgevoerd, kunt u een back-up uitvoeren telkens wanneer deze taak wordt uitgevoerd (om te werken op de meest recente gegevens) of voor het gebruik van de laatste bestaande back-up in de cloud (als u op sommige gearchiveerde gegevens werkt).

    ![Nieuwe details van gegevensbron](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. Vervolgens moeten de doel-instellingen worden geconfigureerd. Er zijn 2 soorten ondersteunde doelen – Azure Storage-accounts en Azure Media Services-accounts. Kies de storage-accounts, bestanden in BLOB's in het account te plaatsen. Media services-account om bestanden naar activa in het account kiezen. Opnieuw, moet een opslagplaats toevoegen. Selecteer in de vervolgkeuzelijst **nieuwe toevoegen** en vervolgens **configureren**.

    ![Gegevens sink maken](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Hier kunt u het type van de opslagplaats die u wilt toevoegen en de andere parameters die zijn gekoppeld aan de opslagplaats. In beide gevallen wordt een opslagwachtrij gemaakt wanneer de taak wordt uitgevoerd. Deze wachtrij wordt gevuld met berichten over getransformeerde blobs wanneer deze gereed zijn. De naam van deze wachtrij is hetzelfde als de naam van de taakdefinitie. Als u selecteert **Media Services** als het type opslagplaats vervolgens kunt u ook opgeven opslagaccountreferenties waarop de wachtrij is gemaakt.

    ![Nieuwe gegevens sink-details](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. Na het toevoegen van de gegevensopslagplaats (die duurt een paar seconden) vindt u de opslagplaats in de vervolgkeuzelijst in de **doel-accountnaam**.  Kies de doelmap die u nodig hebt.

12. Klik op **OK** voor het maken van de taakdefinitie. De taakdefinitie van de is nu ingesteld. U kunt deze taakdefinitie meerdere keren via de gebruikersinterface.

    ![Nieuwe taakdefinitie toevoegen](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a>De taakdefinitie uitvoeren

Wanneer u verplaatsen van gegevens van StorSimple naar het opslagaccount dat u hebt opgegeven in de taakdefinitie wilt, moet u deze aanroepen. Er is enige flexibiliteit in als u de parameters telkens wanneer u de taak aanroept. De stappen zijn als volgt:

1. Selecteer uw StorSimple Data Manager-service en Ga naar **bewaking**. Klik op **nu uitvoeren**.

    ![De definitie van de trigger](./media/storsimple-data-manager-ui/run-now.png)

2. Kies de taakdefinitie die u wilt uitvoeren. Klik op **instellingen uitvoeren** te wijzigen van alle instellingen die u mogelijk wilt wijzigen voor deze taak uitvoeren.

    ![Taakinstellingen uitvoeren](./media/storsimple-data-manager-ui/run-settings.png)

3. Klik op **OK** en klik vervolgens op **uitvoeren** uw taak starten. Voor het bewaken van deze taak gaat u naar de **taken** pagina in uw StorSimple Data Manager.

    ![Takenlijst met en status](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. Naast het controleren van de **taken** blade, u kunt ook luisteren op de opslagwachtrij waar een bericht telkens wanneer een bestand wordt verplaatst van StorSimple naar het opslagaccount is toegevoegd.


## <a name="next-steps"></a>Volgende stappen

[.NET SDK gebruiken om te StorSimple Data Manager-taken starten](storsimple-data-manager-dotnet-jobs.md).