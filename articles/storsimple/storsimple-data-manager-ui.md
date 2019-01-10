---
title: Microsoft Azure StorSimple Data Manager UI | Microsoft Docs
description: Wordt beschreven hoe u StorSimple Data Manager-service UI
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: e773986d11a11c6cc84df367aa48ac1b01a9183e
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54157119"
---
# <a name="manage-the-storsimple-data-manager-service-in-azure-portal"></a>De StorSimple Data Manager-service in Azure portal beheren

In dit artikel wordt uitgelegd hoe u de StorSimple-gegevensbeheer-UI kunt gebruiken om de gegevens die zich bevinden op apparaten uit de StorSimple 8000 serie te transformeren. De getransformeerde gegevens kunnen vervolgens worden gebruikt door andere Azure-services zoals Azure Media Services, Azure HDInsight, Azure Machine Learning en Azure Search.


## <a name="use-storsimple-data-transformation"></a>Gegevenstransformatie StorSimple gebruiken

De StorSimple-gegevensbeheer is de resource binnen welke gegevens transformatie wordt geïnstantieerd. De transformatie-service kunt u gegevens van de StorSimple-indeling naar de oorspronkelijke indeling in blobs of Azure Files transformeren. Voor het transformeren van de gegevens van de oorspronkelijke indeling StorSimple, moet u de details over de StorSimple 8000-apparaat en de gegevens van belang dat u wilt transformeren opgeven.

### <a name="create-a-storsimple-data-manager-service"></a>Een StorSimple Data Manager-service maken

Voer de volgende stappen uit voor het maken van een StorSimple Data Manager-service.

1. Gebruik de referenties van uw Microsoft-account om u aan te melden bij [Azure Portal](https://portal.azure.com/).

2. Klik op **+ een resource maken** en zoeken voor het StorSimple-gegevensbeheer.

    ![Een StorSimple Data Manager-service 1 maken](./media/storsimple-data-manager-ui/create-service-1.png)

3. StorSimple Data Manager en klik op **maken**.
    
    ![Een StorSimple Data Manager-service 2 maken](./media/storsimple-data-manager-ui/create-service-3.png)

3. Voor de nieuwe service, geeft u het volgende:

    1. Geef een unieke **servicenaam** voor uw StorSimple-gegevensbeheer. Dit is een beschrijvende naam die kan worden gebruikt om de service te identificeren. De naam kan tussen 3 en 24 tekens bevatten (letters, cijfers en afbreekstreepjes). De naam moet beginnen en eindigen met een letter of cijfer.

    2. Kies een **abonnement** in de vervolgkeuzelijst. Het abonnement is gekoppeld aan uw factureringsrekening. Dit veld wordt automatisch ingevuld (en niet worden geselecteerd) als u slechts één abonnement hebt.

    3. Selecteer een bestaande resourcegroep of maak een nieuwe groep. Zie [Azure-resourcegroepen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/) voor meer informatie.

    4. Geef de **locatie** voor uw service met de storage-accounts en wordt uw service StorSimple Data Manager. Uw StorSimple Device Manager-service, Data Manager-service en het bijbehorende opslagaccount moeten in de ondersteunde regio's.
    
    5. Als u een koppeling naar deze service op uw dashboard, selecteert u **vastmaken aan dashboard**.
    
    6. Klik op **Create**.

    ![Maken van een StorSimple Data Manager-service 3](./media/storsimple-data-manager-ui/create-service-4.png)

Het maken van de service duurt enkele minuten. U ziet een melding nadat de service is gemaakt en de nieuwe service wordt weergegeven.

### <a name="create-a-data-transformation-job-definition"></a>Een definitie van de transformatie-taak maken

Binnen een StorSimple Data Manager-service moet u een taakdefinitie voor transformatie van gegevens maken. De taakdefinitie van een Hiermee geeft u de details van de StorSimple-gegevens die u geïnteresseerd bent in verplaatsen naar een opslagaccount in de oorspronkelijke indeling. Zodra u de taakdefinitie van een hebt gemaakt, kunt klikt u vervolgens u deze taak opnieuw uitvoeren met verschillende runtime-instellingen.

Voer de volgende stappen uit voor het maken van de taakdefinitie van een.

1. Navigeer naar de service die u hebt gemaakt. Ga naar **Management > taakdefinities**.

2. Klik op **+ taak definitie**.

    ![Klik op + taakdefinitie](./media/storsimple-data-manager-ui/create-job-definition-1.png)

3. Geef een naam op van de taakdefinitie. De naam kan tussen de 3 en 63 tekens lang zijn. De naam mag hoofdletters en kleine letters, cijfers en afbreekstreepjes bevatten.

4. Geef een locatie waar de taak wordt uitgevoerd. Deze locatie is anders dan de locatie waar de service is geïmplementeerd.

5. Klik op **bron** om op te geven van de bron-gegevensopslagplaats.

    ![Opslagplaats van gegevens configureren](./media/storsimple-data-manager-ui/create-job-definition-2.png)

6. Omdat dit een nieuwe Data Manager-service, worden er geen gegevensopslagplaatsen geconfigureerd. In **gegevensbron configureren**, geef de details van de StorSimple 8000-apparaat en de gewenste gegevens.

   Als u wilt uw StorSimple Device Manager als een gegevensopslagplaats toevoegen, klikt u op **nieuwe toevoegen** in de gegevens opslagplaats vervolgkeuzelijst en klik vervolgens op **gegevensopslagplaats toevoegen**.

    ![Nieuwe gegevens opslagplaats toevoegen](./media/storsimple-data-manager-ui/create-job-definition-3.png)
  
    1. Kies **StorSimple 8000-serie Manager** als het gegevenstype van de opslagplaats.
    
    2. Voer een beschrijvende naam voor uw gegevensopslagplaats bron.
    
    3. Kies een abonnement dat is gekoppeld aan uw StorSimple Device Manager-service in de vervolgkeuzelijst.
    
    4. Geef de naam van de StorSimple Device Manager voor de **Resource**.

    5. Voer de **Service gegevensversleuteling** sleutel voor de StorSimple Device Manager-service. 

    ![Configureren van de opslagplaats van het data 1](./media/storsimple-data-manager-ui/create-job-definition-4.png)

    Klik op **OK** wanneer u klaar bent. Hiermee slaat u uw gegevensopslagplaats. Deze StorSimple-Apparaatbeheerfunctie in andere taakdefinities hergebruiken zonder deze parameters opnieuw in te voeren. Het duurt een paar seconden nadat u op **OK** voor de opslagplaats van de zojuist gemaakte gegevensbron worden weergegeven in de vervolgkeuzelijst.

7. In de vervolgkeuzelijst voor **gegevensopslagplaats**, selecteert u de gegevensopslagplaats die u hebt gemaakt. 

    1. Voer de naam van de StorSimple 8000-apparaat met de gegevens van belang zijn.

    2. Geef de naam van het volume op het StorSimple-apparaat dat uw gegevens van belang is.

    3. In de **Filter** subsectie, voer de hoofdmap waarin de gegevens van belang zijn in _\MyRootDirectory\Data_ indeling. Stationsletters zoals _\C:\Data_ worden niet ondersteund. U kunt ook alle filters voor bestanden hier toevoegen.

    4. De transformatie gegevensservice werkt op de gegevens die via de momentopnamen van wordt gepusht naar Azure. Wanneer u deze taak uitvoert, kunt u een back-up uitvoeren telkens wanneer deze taak wordt uitgevoerd (om te werken op de meest recente gegevens) of gebruikt u de laatste bestaande back-up in de cloud (als u op sommige gearchiveerde gegevens werkt).

    5. Klik op **OK**.

    ![Configureren van de opslagplaats van het data 2](./media/storsimple-data-manager-ui/create-job-definition-8.png)

8. De doel-gegevensopslagplaats moet vervolgens worden geconfigureerd. Kies de storage-accounts om bestanden in de blobs in dat account. Selecteer in de vervolgkeuzelijst **nieuwe toevoegen** en vervolgens **-instellingen configureren**.

9. Selecteer het type van de doelopslagplaats die u wilt toevoegen en de andere parameters die zijn gekoppeld aan de opslagplaats.

    Als u een doel Storage-account selecteert, kunt u een beschrijvende naam, abonnement opgeven (Kies hetzelfde als die van de service of andere), en een opslagaccount.
        ![Gegevens doelopslagplaats 1 configureren](./media/storsimple-data-manager-ui/create-job-definition-10.png)

    Een storage-wachtrij wordt gemaakt wanneer de taak wordt uitgevoerd. Deze wachtrij wordt gevuld met berichten over getransformeerde blobs wanneer deze gereed zijn. De naam van deze wachtrij is hetzelfde als de naam van de taakdefinitie.
    
10. Nadat u de gegevensopslagplaats hebt toegevoegd, wacht u enkele minuten.
    
    1. Selecteer de opslagplaats die u hebt gemaakt als het doel in de vervolgkeuzelijst in de **naam van het doelaccount**.

    2. Kies het opslagtype als blobs of -bestanden. Geef de naam van de storage-container waarin de getransformeerde gegevens zich bevindt. Klik op **OK**.

        ![Doelopslagaccount voor opslagplaats van gegevens configureren](./media/storsimple-data-manager-ui/create-job-definition-16.png)

11. U kunt ook controleren op de optie om een schatting van de duur van de taak voordat u de taak uitvoert. Klik op **OK** te maken van de taakdefinitie. De taakdefinitie van de is voltooid. Met verschillende runtime-instellingen kunt u deze taakdefinitie meerdere keren via de gebruikersinterface.

    ![Volledige taakdefinitie](./media/storsimple-data-manager-ui/create-job-definition-13.png)

    De definitie van de nieuwe taak wordt toegevoegd aan de lijst met definities voor deze service.

### <a name="run-the-job-definition"></a>De definitie taakuitvoering

Wanneer u gegevens uit StorSimple verplaatsen naar het opslagaccount dat u hebt opgegeven in de taakdefinitie wilt, moet u uit te voeren. Tijdens runtime, kunnen sommige parameters anders worden opgegeven. De stappen zijn als volgt:

1. Selecteer uw StorSimple Data Manager-service en Ga naar **Management > taakdefinities**. Selecteer en klik op de taakdefinitie die u wilt uitvoeren.
     
     ![Taak 1 uitvoeren starten](./media/storsimple-data-manager-ui/start-job-run1.png)

2. Klik op **nu uitvoeren**.
     
     ![Taak 2 uitvoeren starten](./media/storsimple-data-manager-ui/start-job-run2.png)

3. Klik op **uitvoeringsinstellingen** te wijzigen van alle instellingen die u wilt wijzigen voor deze taak uitvoeren. Klik op **OK** en klik vervolgens op **uitvoeren** uw taak starten.

    ![Taak 3 uitvoeren starten](./media/storsimple-data-manager-ui/start-job-run3.png)

4. Voor het bewaken van deze taak gaat u naar **taken** in uw StorSimple-gegevensbeheer. Naast het controleren van de **taken** blade, u kunt ook luisteren op het storage-wachtrij wanneer een bericht telkens wanneer een bestand van StorSimple wordt verplaatst naar het opslagaccount dat wordt toegevoegd.

    ![Taak 4 uitvoeren starten](./media/storsimple-data-manager-ui/start-job-run4.png)


## <a name="next-steps"></a>Volgende stappen

[.NET SDK gebruiken om te starten van StorSimple Data Manager-taken](storsimple-data-manager-dotnet-jobs.md).