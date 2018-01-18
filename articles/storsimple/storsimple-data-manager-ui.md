---
title: Gebruikersinterface van Microsoft Azure StorSimple Data Manager | Microsoft Docs
description: Beschrijft hoe u StorSimple Data Manager service gebruikersinterface
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: d704cf8e6840c6a7b0a637c404d421f9f1497c46
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="manage-the-storsimple-data-manager-service-in-azure-portal"></a>Beheren van de StorSimple Data Manager-service in Azure-portal

Dit artikel wordt uitgelegd hoe u de UI StorSimple Data Manager kunt gebruiken om de gegevens die zich op de apparaten uit de StorSimple 8000 serie te transformeren. De getransformeerde gegevens kunnen vervolgens worden gebruikt door andere Azure-services zoals Azure Media Services, Azure HDInsight Azure Machine Learning en Azure Search.


## <a name="use-storsimple-data-transformation"></a>StorSimple gegevenstransformatie gebruiken

De StorSimple Data Manager is de bron binnen welke gegevens transformatie wordt geïnstantieerd. De Data Transformation service kunt u gegevens van de StorSimple-indeling naar native-indeling in blobs of Azure-bestanden te transformeren. Voor het transformeren van de gegevens van de oorspronkelijke indeling StorSimple, moet u de details over uw StorSimple 8000 series apparaat en de gegevens van belang dat u wilt transformeren opgeven.

### <a name="create-a-storsimple-data-manager-service"></a>Een StorSimple Data Manager-service maken

De volgende stappen uitvoeren om te maken van een StorSimple Data Manager-service.

1. Gebruik de referenties van uw Microsoft-account om u aan te melden bij [Azure Portal](https://portal.azure.com/).

2. Klik op **+ maken van een resource** en de zoekcriteria voor StorSimple Data Manager.

    ![Maken van een StorSimple Data Manager-service 1](./media/storsimple-data-manager-ui/create-service-1.png)

3. StorSimple Data Manager en klik op **maken**.
    
    ![Maken van een StorSimple Data Manager-service 2](./media/storsimple-data-manager-ui/create-service-3.png)

3. Voor de nieuwe service het volgende opgeven:

    1. Geef een unieke **servicenaam** voor uw StorSimple Data Manager. Dit is een beschrijvende naam die kan worden gebruikt om de service te identificeren. De naam kan tussen 3 en 24 tekens die letters, cijfers en afbreekstreepjes worden kunnen hebben. De naam moet beginnen en eindigen met een letter of cijfer.

    2. Kies een **abonnement** uit de vervolgkeuzelijst. Het abonnement is gekoppeld aan uw factureringsrekening. Dit veld is automatisch ingevulde (en niet geselecteerd) als u slechts één abonnement hebt.

    3. Selecteer een bestaande resourcegroep of maak een nieuwe groep. Zie [Azure-resourcegroepen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/) voor meer informatie.

    4. Geef de **locatie** voor uw service met de storage-accounts en uw StorSimple Data Manager-service. Uw StorSimple-apparaat Manager-service, Data Manager-service en het bijbehorende opslagaccount moeten in de ondersteunde regio's.
    
    5. Als u een koppeling naar deze service op uw dashboard, selecteert u **vastmaken aan dashboard**.
    
    6. Klik op **Create**.

    ![Maken van een StorSimple Data Manager-service 3](./media/storsimple-data-manager-ui/create-service-4.png)

Het maken van de service duurt enkele minuten. U ziet een melding nadat de service is gemaakt en de nieuwe service die wordt weergegeven.

### <a name="create-a-data-transformation-job-definition"></a>Een definitie van de transformatie-taak maken

Binnen een StorSimple Data Manager-service moet u een taakdefinitie voor transformatie van gegevens maken. De taakdefinitie van een geeft de details van de StorSimple-gegevens die u geïnteresseerd bent in het verplaatsen naar een opslagaccount in de oorspronkelijke indeling. Wanneer u de taakdefinitie van een maakt, vervolgens u kunt deze taak uitvoeren opnieuw met andere runtime-instellingen.

De volgende stappen voor het maken van de taakdefinitie van een uitvoeren.

1. Navigeer naar de service die u hebt gemaakt. Ga naar **Management > taakdefinities**.

2. Klik op **+ taak definitie**.

    ![Klik op + taakdefinitie](./media/storsimple-data-manager-ui/create-job-definition-1.png)

3. Geef een naam voor de taakdefinitie. De naam mag tussen de 3 en 63 tekens lang zijn. De naam kunt hoofdletters en kleine letters, cijfers en afbreekstreepjes bevatten.

4. Geef een locatie waarin de taak wordt uitgevoerd. Deze locatie kan afwijken van de locatie waar de service is geïmplementeerd.

5. Klik op **bron** om op te geven van de data source-opslagplaats.

    ![Bron-repo-gegevens configureren](./media/storsimple-data-manager-ui/create-job-definition-2.png)

6. Aangezien dit een nieuwe Data Manager-service, worden geen gegevens opslagplaatsen geconfigureerd. In **gegevensbron configureren**, geef de details van uw StorSimple 8000 series apparaat en de gegevens van belang.

   Als u wilt uw StorSimple-apparaat Manager toevoegen als een gegevensopslagplaats, klikt u op **nieuwe toevoegen** in de vervolgkeuzelijst opslagplaats voor gegevens en klik op **gegevensopslagplaats toevoegen**.

    ![Toevoegen van nieuwe gegevens opslagplaats](./media/storsimple-data-manager-ui/create-job-definition-3.png)
  
    1. Kies **StorSimple 8000-serie Manager** als het gegevenstype van de opslagplaats.
    
    2. Voer een beschrijvende naam voor de bron-gegevensopslagplaats.
    
    3. Kies een abonnement dat is gekoppeld aan uw StorSimple-apparaat Manager-service in de vervolgkeuzelijst.
    
    4. Geef de naam van het StorSimple-Apparaatbeheer voor de **Resource**.

    5. Voer de **Service gegevensversleuteling** sleutel voor de service Manager voor StorSimple-apparaat. 

    ![Bron gegevens opslagplaats 1 configureren](./media/storsimple-data-manager-ui/create-job-definition-4.png)

    Klik op **OK** wanneer u klaar bent. Hiermee slaat u de gegevensopslagplaats van uw. Dit apparaat StorSimple Manager in andere taakdefinities hergebruiken zonder deze parameters opnieuw invoeren. Het duurt enkele seconden nadat u op **OK** voor de opslagplaats van de zojuist gemaakte gegevensbron worden weergegeven in de vervolgkeuzelijst.

7. In de vervolgkeuzelijst voor **gegevensopslagplaats**, selecteer de gegevensopslagplaats die u hebt gemaakt. 

    1. Voer de naam van het StorSimple 8000 series apparaat met de gegevens van belang.

    2. Geef de naam van het volume op het StorSimple-apparaat dat de gegevens van belang.

    3. In de **Filter** subsectie, voert u de hoofdmap waarin de gegevens van belang in _\MyRootDirectory\Data_ indeling. Stationsletters zoals _\C:\Data_ worden niet ondersteund. U kunt ook een bestandsfilters hier toevoegen.

    4. De data transformation service werkt op de gegevens die naar de Azure via momentopnamen van is gepusht. Wanneer u deze taak uitvoert, kunt u een back-up uitvoeren telkens wanneer deze taak wordt uitgevoerd (om te werken op de meest recente gegevens) of gebruik de laatste bestaande back-up in de cloud (als u op sommige gearchiveerde gegevens werkt).

    5. Klik op **OK**.

    ![Bron gegevens opslagplaats 2 configureren](./media/storsimple-data-manager-ui/create-job-definition-8.png)

8. De doel-gegevensopslagplaats moet vervolgens worden geconfigureerd. Kies de storage-accounts, bestanden in BLOB's in het account te plaatsen. Selecteer in de vervolgkeuzelijst **nieuwe toevoegen** en vervolgens **configureren**.

9. Selecteer het type van de doelopslagplaats die u wilt toevoegen en de andere parameters die zijn gekoppeld aan de opslagplaats.

    Als u een doel Storage account selecteert, kunt u een beschrijvende naam abonnement opgeven (Kies gelijk zijn aan die van de service of andere), en een opslagaccount.
        ![Doel gegevens opslagplaats 1 configureren](./media/storsimple-data-manager-ui/create-job-definition-10.png)

    Storage-wachtrij wordt gemaakt wanneer de taak wordt uitgevoerd. Deze wachtrij wordt gevuld met berichten over getransformeerde blobs wanneer deze gereed zijn. De naam van deze wachtrij is hetzelfde als de naam van de taakdefinitie.
    
10. Nadat u de gegevensopslagplaats toegevoegd, wacht u enkele minuten duren.
    
    1. Selecteer de opslagplaats die u hebt gemaakt als het doel in de vervolgkeuzelijst in de **doel-accountnaam**.

    2. Kies het opslagtype als blobs of bestanden. Geef de naam van de opslagcontainer waarin de getransformeerde gegevens zich bevindt. Klik op **OK**.

        ![Doel gegevens opslagplaats storage-account configureren](./media/storsimple-data-manager-ui/create-job-definition-16.png)

11. U kunt ook de optie om een schatting van de duur van de taak voordat u de taak controleren. Klik op **OK** voor het maken van de taakdefinitie. De taakdefinitie van de is voltooid. U kunt deze taakdefinitie meerdere keren via de gebruikersinterface gebruiken met andere runtime-instellingen.

    ![Volledige taakdefinitie](./media/storsimple-data-manager-ui/create-job-definition-13.png)

    De zojuist gemaakte taakdefinitie wordt toegevoegd aan de lijst met taakdefinities voor deze service.

### <a name="run-the-job-definition"></a>De taakdefinitie uitvoeren

Wanneer u verplaatsen van gegevens van StorSimple naar het opslagaccount dat u hebt opgegeven in de taakdefinitie wilt, moet u deze uitvoeren. Tijdens runtime, kunnen sommige parameters anders worden opgegeven. De stappen zijn als volgt:

1. Selecteer uw StorSimple Data Manager-service en Ga naar **Management > taakdefinities**. Selecteer en klik op de taakdefinitie die u wilt uitvoeren.
     
     ![Taak 1 uitvoeren starten](./media/storsimple-data-manager-ui/start-job-run1.png)

2. Klik op **nu uitvoeren**.
     
     ![Taak 2 uitvoeren starten](./media/storsimple-data-manager-ui/start-job-run2.png)

3. Klik op **instellingen uitvoeren** te wijzigen van alle instellingen die u mogelijk wilt wijzigen voor deze taak uitvoeren. Klik op **OK** en klik vervolgens op **uitvoeren** uw taak starten.

    ![Taak 3 uitvoeren starten](./media/storsimple-data-manager-ui/start-job-run3.png)

4. Voor het bewaken van deze taak gaat u naar **taken** in uw StorSimple Data Manager. Naast het controleren van de **taken** blade, u kunt ook luisteren op de opslagwachtrij waar een bericht telkens wanneer een bestand wordt verplaatst van StorSimple naar het opslagaccount is toegevoegd.

    ![Starttaak uitgevoerd 4](./media/storsimple-data-manager-ui/start-job-run4.png)


## <a name="next-steps"></a>Volgende stappen

[.NET SDK gebruiken om te StorSimple Data Manager-taken starten](storsimple-data-manager-dotnet-jobs.md).