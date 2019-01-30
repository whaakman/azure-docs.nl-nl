---
title: Gegevens kopiëren naar uw Microsoft Azure Data Box via SMB | Microsoft Docs
description: Meer informatie over hoe u gegevens naar uw Azure Data Box kopieert via de gegevenskopieerservice
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: a71635abd036bb89546dd3421af97cd9b88f4327
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54439947"
---
# <a name="tutorial-use-data-copy-service-to-directly-ingest-data-into-azure-data-box-preview"></a>Zelfstudie: De gegevenskopieerservice gebruiken om gegevens direct op te nemen in Azure Data Box (preview)

In deze zelfstudie wordt beschreven hoe u gegevens opneemt met behulp van de gegevenskopieerservice, zonder gebruik te maken van een tussenliggende host. De gegevenskopieerservice wordt lokaal uitgevoerd op de Data Box, maakt via SMB verbinding met uw NAS-apparaat en kopieert gegevens naar Data Box.

Gegevenskopieerservice gebruiken:

- In de NAS-omgevingen (network-attached storage) waarin tussenliggende hosts mogelijk niet beschikbaar zijn.
- Met kleine bestanden waarvoor soms weken nodig zijn om gegevens op te nemen en te uploaden. Met deze service wordt de opname- en uploadtijd aanzienlijk verbeterd.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gegevens kopiëren naar Data Box
> * Data Box voorbereiden voor verzending.

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de zelfstudie [ Azure Data Box instellen](data-box-deploy-set-up.md) voltooid.
2. U hebt uw Data Box ontvangen en de bestelstatus in de portal is **Geleverd**.
3. U hebt de referenties van het bron-NAS-apparaat waarmee u verbinding wilt maken om gegevens te kopiëren.
4. U bent verbonden met een netwerk met hoge snelheid. Het wordt aangeraden dat u beschikt over minstens één 10-GbE-verbinding. Als er geen 10 GbE-verbinding beschikbaar is, kan een 1 GbE-gegevenskoppeling worden gebruikt, maar dit heeft wel gevolgen voor de kopieersnelheid.

## <a name="copy-data-to-data-box"></a>Gegevens kopiëren naar Data Box

Wanneer u bent verbonden met het NAS-apparaat, kunt u de gegevens kopiëren. Neem de volgende punten door voordat u gegevens gaat kopiëren:

- Zorg er tijdens het kopiëren van gegevens voor dat de gegevensgrootte voldoet aan de limieten die worden vermeld in de [limieten voor Azure-opslag en Data Box](data-box-limits.md).
- Als de gegevens die door Data Box worden geüpload gelijktijdig door andere toepassingen buiten Data Box worden geüpload, kan dit tot fouten voor de uploadtaak en beschadigde gegevens leiden.
- Als de gegevens verlopen terwijl deze door de kopieerservice worden gelezen, is het mogelijk dat er fouten optreden of gegevens beschadigd zijn.

U moet een taak maken als u gegevens wilt kopiëren met behulp van de gegevenskopieerservice. Volg deze stappen om een taak te maken voor het kopiëren van gegevens.

1. Ga in de lokale webgebruikersinterface van uw Data Box naar **Beheren > Gegevens kopiëren**.
2. Klik op de pagina **Gegevens kopiëren** op **Maken**.

    ![Klik op de pagina **Gegevens kopiëren** op **Maken**](media/data-box-deploy-copy-data-via-copy-service/click-create.png)

3. Voer in het dialoogvenster **Configureren en starten** het volgende in.
    
    |Veld                          |Waarde    |
    |-------------------------------|---------|
    |Taaknaam                       |Een unieke naam van maximaal 230 tekens voor de taak. De volgende tekens zijn niet toegestaan in de naam van de taak: \<, \>, \|, \?, \*, \\, \:, \/ en \\\.         |
    |Bronlocatie                |Geef het SMB-pad naar de gegevensbron op in de indeling: `\\<ServerIPAddress>\<ShareName>` of `\\<ServerName>\<ShareName>`.        |
    |Gebruikersnaam                       |Gebruikersnaam voor toegang tot de gegevensbron.        |
    |Wachtwoord                       |Wachtwoord voor toegang tot de gegevensbron.           |
    |Doelopslagaccount    |Selecteer in de vervolgkeuzelijst het doelopslagaccount waarnaar u de gegevens wilt uploaden.         |
    |Doelopslagtype       |Selecteer het type van de doelopslag: blok-blob, pagina-blob of Azure Files.        |
    |Doelcontainer/-share    |Voer de naam in van de container of share waarnaar u gegevens wilt uploaden in uw doelopslagaccount. De naam kan een sharenaam of een containernaam zijn. Bijvoorbeeld `myshare` of `mycontainer`. U kunt deze ook invoeren in de indeling `sharename\directory_name` of `containername\virtual_directory_name` in de cloud.        |
    |Bestanden kopiëren die overeenkomen met het patroon    | U kunt het bestandsnaampatroon invoeren op de volgende twee manieren.<ul><li>**Wildcard-expressies gebruiken** Alleen `*` en `?` worden ondersteund in expressies met jokertekens. De expressie `*.vhd` bijvoorbeeld komt overeen met alle bestanden die de extensie .vhd hebben. En zo komt `*.dl?` overeen met alle bestanden die de extensie `.dl` of `.dll` hebben. En `*foo` komt overeen met alle bestanden waarvan de bestandsnaam eindigt op `foo`.<br>U kunt een expressie met jokertekens rechtstreeks invoeren in het veld. Standaard wordt een waarde die in het veld wordt ingevoerd, beschouwd als een expressie met jokertekens.</li><li>**Reguliere expressies gebruiken** - Op POSIX gebaseerde reguliere expressies worden ondersteund. De reguliere expressie `.*\.vhd` bijvoorbeeld komt overeen met alle bestanden die de extensie `.vhd` hebben. Voor de reguliere expressie geeft u het `<pattern>` rechtstreeks op als `regex(<pattern>)`. <li>Voor meer informatie over reguliere expressies gaat u naar [Regular expression language - a quick reference](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) (Reguliere expressies - een snelzoekgids).</li><ul>|
    |Bestandsoptimalisatie              |Als deze optie is ingeschakeld, worden de bestanden bij opname ingepakt. Dit versnelt het kopiëren van gegevens voor kleine bestanden.        |
 
4. Klik op **Start**. De invoer wordt gevalideerd en als de validatie is geslaagd, wordt een taak gestart. Het kan enkele minuten duren voordat de taak start.

    ![Een taak starten vanuit het dialoogvenster Configure job and start](media/data-box-deploy-copy-data-via-copy-service/configure-and-start.png)

5. Er wordt een taak gemaakt met de opgegeven instellingen. Schakel het selectievakje in, waarna u een taak kunt onderbreken en hervatten, annuleren of opnieuw kunt starten.

    ![Een taak beheren via de pagina Gegevens kopiëren](media/data-box-deploy-copy-data-via-copy-service/select-job.png)
    
    - U kunt deze taak onderbreken als deze tijdens de piekuren invloed heeft op de NAS-resources.

        ![Een taak onderbreken](media/data-box-deploy-copy-data-via-copy-service/pause-job.png)

        U kunt de taak later, na de piekuren, hervatten.

        ![Een taak hervatten](media/data-box-deploy-copy-data-via-copy-service/resume-job.png)

    - U kunt een taak op elk gewenst moment annuleren.

        ![Een taak annuleren](media/data-box-deploy-copy-data-via-copy-service/cancel-job.png) U moet de annulering van een taak bevestigen.

        ![Annulering van taak bevestigen](media/data-box-deploy-copy-data-via-copy-service/confirm-cancel-job.png)

        Als u een taak annuleert, worden de gegevens die al zijn gekopieerd, niet verwijderd. Als u gegevens wilt verwijderen die u naar uw Data Box hebt gekopieerd, moet u het apparaat opnieuw instellen.

        ![Apparaat opnieuw instellen](media/data-box-deploy-copy-data-via-copy-service/reset-device.png)

        >[!NOTE]
        > Als u een taak annuleert of onderbreekt, is het mogelijk dat grote bestanden die op dat moment worden gekopieerd gedeeltelijk zijn gekopieerd. Deze bestanden worden in dezelfde staat geüpload naar Azure. Wanneer u een kopieeropdracht probeert te annuleren of te onderbreken, controleer dan of uw bestanden correct zijn gekopieerd. U kunt de bestanden valideren door de SMB-shares te bekijken of het BOM-bestand te downloaden.

    - U kunt een taak opnieuw starten als deze onverwacht is afgebroken vanwege een tijdelijke fout, zoals een netwerkfout. U kunt een taak niet opnieuw starten als deze een definitieve status heeft bereikt, zoals Voltooid of Voltooid met fouten. De fouten kunnen het gevolg zijn van problemen met bestandsnamen of bestandsgrootten. Deze fouten worden vastgelegd in een logboek, maar de taak kan pas opnieuw worden gestart nadat deze is voltooid.

        ![Een mislukte taak opnieuw starten](media/data-box-deploy-copy-data-via-copy-service/restart-failed-job.png)

        Als er een fout optreedt en de taak niet opnieuw kan worden gestart, download dan de foutenlogboeken en bekijk de fout in de logboekbestanden. Nadat u het probleem hebt opgelost, kunt u een nieuwe taak maken om de bestanden te kopiëren. U kunt ook [de bestanden kopiëren via SMB](data-box-deploy-copy-data.md).
    
    - In deze release kunt u een taak niet verwijderen.
    
    - U kunt een onbeperkt aantal taken maken, maar maximaal 10 parallelle taken tegelijkertijd uitvoeren.
    - Als bestandsoptimalisatie is ingeschakeld, worden de kleine bestanden ingepakt bij opname om de kopieerprestaties te verbeteren. In deze exemplaren ziet u een ingepakt bestand (GUID als naam) zoals in de volgende schermafbeelding wordt weergegeven.

        ![Voorbeeld van een ingepakt bestand](media/data-box-deploy-copy-data-via-copy-service/packed-file-on-ingest.png)

6. Terwijl de taak wordt uitgevoerd, ziet u het volgende op de pagina **Gegevens kopiëren**:

    - In de kolom **Status** ziet u de status van de kopieertaak. De status kan zijn:
        - **Wordt uitgevoerd**
        - **Mislukt**
        - **Geslaagd**
        - **Wordt onderbroken**
        - **Onderbroken**
        - **Wordt geannuleerd**
        - **Geannuleerd**
        - **Voltooid met fouten**
    - In de kolom **Bestanden** ziet u het aantal en de grootte van alle bestanden die worden gekopieerd.
    - In de kolom **Verwerkt** ziet u het aantal en de grootte van alle bestanden die worden verwerkt.
    - In de kolom **Details** kunt u op **Weergeven** klikken als u de taakdetails wilt bekijken.
    - Als er fouten zijn opgetreden tijdens het kopieerproces, wat wordt weergegeven in de kolom **# fouten**, ga dan naar de kolom **Foutenlogboek** en download de foutenlogboeken om het probleem op te lossen.

Wacht tot de kopieertaken zijn voltooid. Omdat een aantal fouten alleen wordt vastgelegd op de pagina **Verbinding maken en kopiëren**, moet u wachten tot de kopieertaken zonder fouten zijn voltooid voordat u verder gaat met de volgende stap.

![Geen fouten op de pagina **Verbinding maken en kopiëren**](media/data-box-deploy-copy-data-via-copy-service/verify-no-errors-on-connect-and-copy.png)

Om de gegevensintegriteit te garanderen wordt de controlesom inline berekend terwijl de gegevens worden gekopieerd. Verifieer de gebruikte ruimte en vrije ruimte op uw apparaat na het kopiëren.
    
![Vrije en ongebruikte ruimte verifiëren op het dashboard](media/data-box-deploy-copy-data-via-copy-service/verify-used-space-dashboard.png)

Wanneer de kopieertaak is voltooid, kunt u naar **Voorbereiding voor verzending** gaan.

>[!NOTE]
> Voorbereiding voor verzending kan niet worden uitgevoerd terwijl de kopieertaken worden uitgevoerd.

## <a name="prepare-to-ship"></a>Voorbereiding voor verzending

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * Gegevens kopiëren naar Data Box
> * Data Box voorbereiden voor verzending

Ga naar de volgende zelfstudie om te lezen hoe u uw Data Box naar Microsoft verzendt.

> [!div class="nextstepaction"]
> [Uw Azure Data Box verzenden naar Microsoft](./data-box-deploy-picked-up.md)

