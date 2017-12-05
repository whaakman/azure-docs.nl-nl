---
title: Maak een back-up van virtuele machines op de gewenste schaal in Azure | Microsoft Docs
description: Tegelijkertijd back-up van meerdere virtuele machines naar Azure
services: backup
keywords: back-up van virtuele machine. virtuele machine back-up; back-up van vm; back-vm; back-up Azure vm; back-up en herstel na noodgevallen
author: markgalioto
ms.author: markgal
ms.date: 09/16/2017
ms.topic: tutorial
ms.service: backup
ms.custom: mvc
ms.openlocfilehash: 74ccf95b559b690eb53c2f4df14513dab5a94677
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Azure portal gebruiken voor back-up van meerdere virtuele machines

Wanneer u back-up van gegevens in Azure, kunt u die gegevens opslaan in een Azure-resource een Recovery Services-kluis wordt aangeroepen. De Recovery Services-kluis resource is beschikbaar via het menu instellingen van de meeste Azure-services. Het voordeel dat de Recovery Services-kluis die is geïntegreerd in het menu instellingen van de meeste Azure-services kunt heel eenvoudig back-up van gegevens. Echter, is het omslachtig afzonderlijk werkt met elke database of de virtuele machine in uw bedrijf. Wat gebeurt er als u wilt back-up van de gegevens voor alle virtuele machines in een afdeling of op één locatie? Het is eenvoudig back-up meerdere virtuele machines door het maken van een back-upbeleid en dat beleid toepassen op de gewenste virtuele machines. Deze zelfstudie wordt uitgelegd hoe:

> [!div class="checklist"]
> * Een Recovery Services-kluis maken
> * Een back-upbeleid definiëren
> * Het back-upbeleid ter bescherming van meerdere virtuele machines toepassen
> * Activeren van een back-uptaak op aanvraag voor de beveiligde virtuele machines

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

De Recovery Services-kluis bevat de back-upgegevens en de back-upbeleid toegepast op de beveiligde virtuele machines. Het maken van back-ups van virtuele machines is een lokaal proces. U kunt geen back-up een virtuele machine vanaf één locatie naar een Recovery Services-kluis in een andere locatie. Ten minste één Recovery Services-kluis moet dus voor elke Azure-locatie met een back-up van virtuele machines, bestaan op die locatie.

1. Selecteer op het menu links **meer services** en typt u in de lijst met services *Recovery Services*. Terwijl u typt, wordt de lijst met resources gefilterd. Wanneer u een Recovery Services-kluizen in de lijst ziet, selecteert u deze om de Recovery Services-kluizen menu te openen.

    ![Menu openen Recovery Services-kluis](./media/tutorial-backup-vm-at-scale/full-browser-open-rs-vault.png) <br/>

2. In de **Recovery Services-kluizen** menu, klikt u op **toevoegen** om de Recovery Services-kluismenu te openen.

    ![Menu kluis openen](./media/tutorial-backup-vm-at-scale/provide-vault-detail-2.png)

3. In de Recovery Services-kluis menu 

    - Type *myRecoveryServicesVault* in **naam**,
    - De huidige abonnements-ID komt in **abonnement**. Als u extra abonnementen hebt, kunt u een ander abonnement voor de nieuwe kluis.
    - Voor **resourcegroep** Selecteer **gebruik bestaande** en kies *myResourceGroup*. Als *myResourceGroup* niet bestaat, selecteer **nieuw** en het type *myResourceGroup*.
    - Van de **locatie** vervolgkeuzelijst, kies *West-Europa*.
    - Klik op **maken** Recovery Services-kluis maken.

Een Recovery Services-kluis moet zich in dezelfde locatie als de virtuele machines die worden beveiligd. Als u virtuele machines in meerdere regio's hebt, maakt u een Recovery Services-kluis in elke regio. Deze zelfstudie maakt u een Recovery Services-kluis in *West-Europa* omdat dat waar *myVM* (de virtuele machine gemaakt met de Snelstartgids) is gemaakt.

Het kan enkele minuten duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen rechtsboven in de portal. Zodra de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen.

Wanneer u een Recovery Services-kluis maakt, heeft de kluis standaard geografisch redundante opslag. Als u gegevenstolerantie, geografisch redundante opslag, repliceert de gegevens meerdere keren tussen twee Azure-regio's.

## <a name="set-backup-policy-to-protect-vms"></a>Back-upbeleid voor het beveiligen van virtuele machines instellen

Nadat de Recovery Services-kluis is gemaakt, is de volgende stap voor het configureren van de kluis voor het type gegevens en de back-upbeleid instellen. Back-upbeleid is de planning voor hoe vaak en wanneer de herstelpunten worden gemaakt. Het beleid bevat ook de bewaartermijn voor de herstelpunten. Voor deze zelfstudie gaan we ervan uit uw bedrijf is een complexe met een hotel, stadium, en restaurant en concessies sport en beveiligt u de gegevens op de virtuele machines. De volgende stappen maakt een back-upbeleid voor de financiële gegevens.

1. Selecteer in de lijst met Recovery Services-kluizen **myRecoveryServicesVault** om het dashboard te openen.

   ![Scenario menu openen](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

2. Klik op het dashboard kluismenu **back-up** om de back-up-menu te openen.

3. Klik in het menu back-updoel in de **waar wordt uw workload uitgevoerd** vervolgkeuzelijst, kies *Azure*. Van de **wat wilt u back-up maken** vervolgkeuzelijst, kiest u *virtuele machine*, en klik op **back-**.

    Deze acties voorbereiden op de Recovery Services-kluis communiceert met een virtuele machine. Recovery Services-kluizen hebben een standaardbeleid dat elke dag van een herstelpunt gemaakt en herstelpunten behoudt gedurende 30 dagen.

    ![Scenario menu openen](./media/tutorial-backup-vm-at-scale/backup-goal.png)

4. Een nieuw beleid in het menu van het beleid voor back-up maken van de **back-upbeleid kiezen** vervolgkeuzelijst, selecteer *nieuw*.

    ![Workload selecteren](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

5. In de **back-up maken van beleid** menu voor **beleidsnaam** type *financiën*. Voer de volgende wijzigingen voor de Backup-beleid: 
    - Voor **back-upfrequentie** ingesteld van de tijdzone voor *Central Time*. Omdat het complexe sport in Texas is, wil de eigenaar de timing lokaal. Laat de back-upfrequentie die is ingesteld op elke dag om 3:30 uur.
    - Voor **bewaren van dagelijkse back-uppunt**, de periode instellen op 90 dagen.
    - Voor **bewaren van wekelijkse back-uppunt**, gebruiken de *maandag* herstelpunt en deze te behouden voor 52 weken.
    - Voor **bewaren van de maandelijkse back-uppunt**, gebruikt u het herstelpunt van eerste zondag van de maand en deze te behouden voor 36 maanden.
    - Deselecteer het **bewaren van jaarlijkse back-uppunt** optie. Het opvulteken van Financiën wilt niet gegevens bewaren langer is dan de 36 maanden.
    - Klik op **OK** om het back-upbeleid te maken.

    ![Workload selecteren](./media/tutorial-backup-vm-at-scale/set-new-policy.png) 

    Na het maken van de back-upbeleid, koppelt u het beleid aan de virtuele machines.

6. In de **virtuele machines selecteren** dialoogvenster Selecteer *myVM* en klik op **OK** voor het implementeren van de back-upbeleid voor de virtuele machines. 

    Alle virtuele machines die zich in dezelfde locatie en nog niet is gekoppeld aan een back-upbeleid worden weergegeven. *myVMH1* en *myVMR1* zijn geselecteerd moeten worden gekoppeld aan de *financiën* beleid.

    ![Workload selecteren](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png) 

    Wanneer de implementatie is voltooid, ontvangt u een melding die implementatie is voltooid.

## <a name="initial-backup"></a>Eerste back-up

U kunt back-up voor de Recovery Services-kluizen hebt ingeschakeld, maar een eerste back-up is niet gemaakt. Is een herstel na noodgevallen aanbevolen procedure voor het activeren van de eerste back-up, zodat uw gegevens worden beveiligd. 

Een back-uptaak op aanvraag uitvoeren:

1. Klik op het kluisdashboard op **3** onder **back-Upitems**, om de back-Upitems menu te openen.

    ![Het pictogram Instellingen](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    De **back-Upitems** menu wordt geopend.

2. Op de **back-Upitems** menu, klikt u op **Azure virtuele Machine** om de lijst van virtuele machines die zijn gekoppeld aan de kluis te openen.

    ![Het pictogram Instellingen](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    De lijst **Back-upitems** wordt geopend.

    ![Geactiveerde back-uptaak](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

3. Klik in de lijst **Back-upitems** op het weglatingsteken **...**  om het contextmenu te openen.

4. Selecteer in het contextmenu **back-up nu**.

    ![Contextmenu](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Het menu back-up nu wordt geopend.

5. Voer de laatste dag het herstelpunt behouden en klik op in het menu nu back-up **back-up**.

    ![de laatste dag instellen dat het herstelpunt van Nu back-up maken wordt bewaard](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Implementatiemeldingen laten u weten dat de back-uptaak is geactiveerd en dat u de voortgang van de taak op de pagina Back-uptaken kunt controleren. Afhankelijk van de grootte van uw virtuele machine kan maken van de eerste back-up duren.

    Wanneer de eerste back-uptaak is voltooid, ziet u de status ervan in het menu van de taak back-up. De back-uptaak op aanvraag gemaakt in het eerste herstelpunt voor *myVM*. Als u back-up van andere virtuele machines wilt, herhaalt u deze stappen voor elke virtuele machine. 

    ![Tegel Back-uptaken](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)
  
## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent om door te gaan op werken met de volgende zelfstudies, geen clean up maakt van de resources in deze zelfstudie hebt gemaakt. Als u niet van plan bent om door te gaan, gebruik de volgende stappen uit om te verwijderen van alle resources die zijn gemaakt met deze zelfstudie in de Azure portal.

1. Op de **myRecoveryServicesVault** dashboard, klikt u op **3** onder **back-Upitems**, om de back-Upitems menu te openen.

    ![Het pictogram Instellingen](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)


2. Op de **back-Upitems** menu, klikt u op **Azure virtuele Machine** om de lijst van virtuele machines die zijn gekoppeld aan de kluis te openen.

    ![Het pictogram Instellingen](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    De lijst **Back-upitems** wordt geopend.

3. In de **back-Upitems** menu, klikt u op het weglatingsteken om te openen van het contextmenu.

    ![Het pictogram Instellingen](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

4. Selecteer in het contextmenu **back-up stoppen** om back-up stoppen menu te openen. 

    ![Het pictogram Instellingen](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

5. In de **back-up stoppen** menu, selecteer het bovenste menu van de vervolgkeuzelijst en kies **back-up-gegevens verwijderen**.

6. In de **typt u de naam van het artikel back-up** dialoogvenster, type *myVM*.
 
7. Zodra de back-item is geverifieerd (een vinkje weergegeven), **back-up stoppen** knop ingeschakeld. Klik op **back-up stoppen** stoppen van het beleid en de herstelpunten te verwijderen. 

    ![Klik op stoppen back-up kluis verwijderen](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png).

8. In de **myRecoveryServicesVault** menu, klikt u op **verwijderen**.

    ![Klik op stoppen back-up kluis verwijderen](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Zodra de kluis zijn verwijderd, wordt u terugkeren naar de lijst met Recovery Services-kluizen.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u de Azure-portal:

> [!div class="checklist"]
> * Een Recovery Services-kluis maken
> * Instellen van de kluis voor de virtuele machines beveiligen
> * Maak een back-up en retentie-beleid
> * Het beleid aan het beveiligen van meerdere virtuele machines toewijzen
> * Op aanvraag voor virtuele machines opnieuw activeren

Blijven de volgende zelfstudie in Azure een virtuele machine van de schijf te herstellen. 

> [!div class="nextstepaction"]
> [Herstellen van virtuele machines met CLI](./tutorial-restore-disk.md)
