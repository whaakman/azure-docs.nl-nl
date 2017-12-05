---
title: Maak een back-up van Windows Server naar Azure | Microsoft Docs
description: Deze zelfstudie gegevens back-ups van de lokale Windows-Servers in een Recovery Services-kluis.
services: back up
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: WindowsServer back-up; back-up van WindowsServer. een back-up en herstel na noodgevallen
ms.assetid: 
ms.service: back up
ms.workload: storage-back up-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/23/2017
ms.author: saurabhsensharma;markgal;
ms.custom: mvc
ms.openlocfilehash: 0dbf3850c7fcccb1a02e70a19b498522a4ce0e79
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="back-up-windows-server-to-azure"></a>Maak back-ups van Windows Server naar Azure


Azure Backup kunt u uw Windows-Server beschermen tegen beschadigingen aanvallen en noodsituaties. Azure Backup biedt een lichtgewicht hulpprogramma bekend als de agent van Microsoft Azure Recovery Services (MARS). De MARS-agent is geïnstalleerd op de Windows Server-bestanden en mappen en configuratiegegevens van de server via Windows Server System State te beveiligen. Deze zelfstudie wordt uitgelegd hoe u MARS-Agent kunt gebruiken voor back-up van uw Windows-Server naar Azure. In deze zelfstudie leert u het volgende: 


> [!div class="checklist"]
> * Downloaden en stelt u de MARS-Agent
> * Maak een back-up van tijden configureren en het bewaarschema voor back-ups van uw server
> * Een ad-hoc back-up uitvoeren


## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Voordat u kunt back-up van Windows Server, moet u een locatie voor de back-ups maken of herstelpunten worden opgeslagen. Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is een container in Azure die de back-ups van uw Windows-Server worden opgeslagen. Volg onderstaande stappen voor het maken van een Recovery Services-kluis in de Azure-portal. 

1. Selecteer op het menu links **alle services** en typt u in de lijst met services **Recovery Services**. Klik op **Recovery Services-kluizen**.

   ![Recovery Services-kluis openen](./media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)

2.  Klik in het menu **Recovery Services-kluizen** op **Toevoegen**.

   ![Geef informatie op voor de kluis](./media/tutorial-backup-windows-server-to-azure/provide-vault-detail-2.png)

3.  In de **Recovery Services-kluis** menu

    - Type *myRecoveryServicesVault* in **naam**.
    - De huidige abonnements-ID komt in **abonnement**.
    - Voor **resourcegroep**, selecteer **gebruik bestaande** en kies *myResourceGroup*. Als *myResourceGroup* niet bestaat, selecteer **nieuw** en het type *myResourceGroup*. 
    - Van de **locatie** vervolgkeuzelijst, kies *West-Europa*.
    - Klik op **maken** Recovery Services-kluis maken.
 
Zodra de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen.

## <a name="download-recovery-services-agent"></a>Recovery Services-agent downloaden

De Microsoft Azure Recovery Services (MARS)-agent maakt een koppeling tussen Windows Server en de Recovery Services-kluis. De volgende procedure wordt uitgelegd hoe u de agent downloaden naar uw server.

1.  Selecteer in de lijst met Recovery Services-kluizen **myRecoveryServicesVault** om het dashboard te openen.

   ![Geef informatie op voor de kluis](./media/tutorial-backup-windows-server-to-azure/open-vault-from-list.png)

2.  Klik op het dashboard kluismenu **back-up**.

3.  Op de **back-updoel** menu:

    - voor **waar wordt uw workload uitgevoerd?**, selecteer**On-premises**, 
    - voor **wat wilt u back-up maken?**, selecteer **bestanden en mappen** en **systeemstatus** 

    ![Geef informatie op voor de kluis](./media/tutorial-backup-windows-server-to-azure/backup-goal.png)
    
4.  Klik op **infrastructuur voorbereiden** openen de **infrastructuur voorbereiden** menu.
5.  Op de **infrastructuur voorbereiden** menu, klikt u op **Agent downloaden voor Windows Server of Windows Client** voor het downloaden van de *MARSAgentInstaller.exe*. 

    ![infrastructuur voorbereiden](./media/tutorial-backup-windows-server-to-azure/prepare-infrastructure.png)

    Het installatieprogramma opent een browservenster afzonderlijke en downloadt **MARSAgentInstaller.exe**.
 
6.  Voordat u het gedownloade bestand hebt uitgevoerd, klikt u op de **downloaden** knop op de blade van de infrastructuur voorbereiden voor het downloaden en opslaan van de **Kluisreferenties** bestand. Dit bestand is vereist voor de MARS-Agent verbindt met de Recovery Services-kluis.

    ![infrastructuur voorbereiden](./media/tutorial-backup-windows-server-to-azure/download-vault-credentials.png)
 
## <a name="install-and-register-the-agent"></a>De agent installeren en registreren

1. Zoek en dubbelklik op de gedownloade **MARSagentinstaller.exe**.
2. De **installatiewizard voor Microsoft Azure Recovery Services Agent** wordt weergegeven. Als u de wizard doorloopt, geef de volgende informatie wanneer u wordt gevraagd en klik op **registreren**.
    - Locatie voor de installatie en -cache-map.
    - Proxy serverinformatie als u een proxyserver gebruiken voor verbinding met internet.
    - De gebruiker en wachtwoord invoeren als u een geverifieerde proxyserver gebruiken.

    ![infrastructuur voorbereiden](./media/tutorial-backup-windows-server-to-azure/mars-installer.png) 

3. Aan het einde van de wizard, klikt u op **doorgaan met de registratie** en geef de **Kluisreferenties** dat u hebt gedownload in de vorige procedure.
 
4. Geef desgevraagd een wachtwoordzin voor versleuteling voor het versleutelen van back-ups van Windows Server. Sla de wachtwoordzin op een veilige locatie zoals Microsoft de wachtwoordzin niet herstellen als deze verbroken wordt.

5. Klik op **Voltooien**. 

## <a name="configure-backup-and-retention"></a>Configureer back-up en retentie

De Microsoft Azure Recovery Services agent kunt u schema voor back-ups naar Azure, op Windows Server. De volgende stappen uitvoeren op de server waar u de agent gedownload.

1. Open de Microsoft Azure Recovery Services-agent. U vindt deze door te zoeken naar **Microsoft Azure Backup** op uw machine.

2.  Klik in de console van de Recovery Services-agent op **back-up plannen** onder de **actiedeelvenster**.

    ![infrastructuur voorbereiden](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

3. Klik op **volgende** om te navigeren naar de **Items selecteren voor Back up** pagina.

4. Klik op **Items toevoegen** en selecteer in het dialoogvenster **systeemstatus** en bestanden of mappen die u back wilt-up. Klik vervolgens op **OK**.

5. Klik op **Volgende**.

6. Op de **back-upschema opgeven (systeemstatus)** pagina, geef de tijd van de dag of week wanneer back-ups moeten worden geactiveerd voor systeemstatus en klikt u op **volgende** 

7.  Op de **retentiebeleid selecteren (systeemstatus)** pagina, selecteert u het bewaarbeleid voor de back-up van systeemstatus en klikt u op **volgende**
8. Op dezelfde manier, selecteer het back-planning en het bewaren van beleid voor de geselecteerde bestanden en mappen. 
8.  Op de **Kies eerste Back-up Type** pagina, laat de optie **automatisch via het netwerk** geselecteerd en klik vervolgens op **volgende**.
9.  Op de **bevestiging** pagina, lees de informatie en klik vervolgens op **voltooien**.
10. Nadat u de wizard voor het maken van een back-upschema hebt doorlopen, klikt u op **Sluiten**.

## <a name="perform-an-ad-hoc-back-up"></a>Een ad-hoc back-up uitvoeren

U kunt de planning hebt vastgesteld, wanneer back-uptaken uitgevoerd. Echter, u hebt geen back-up de server. Het is disaster recovery aanbevolen een back-up op-aanvraag om ervoor te zorgen gegevenstolerantie voor uw server uitvoeren.

1.  Klik in de console van de Microsoft Azure Recovery Services-agent op **Back-Up uit**.

    ![infrastructuur voorbereiden](./media/tutorial-backup-windows-server-to-azure/backup-now.png)

2.  Op de **Back-Up uit** wizard, schakelt u een van **bestanden en mappen** of **systeemstatus** die u wilt back-up en klikt u op **volgende** 
3. Op de **bevestiging** pagina, controleert u de instellingen die de **Back-Up uit** wizard gebruikt voor back-up van uw server. Klik vervolgens op **Back-up maken**.
4.  Klik op **Sluiten** om de wizard te sluiten. Als u de wizard sluit voordat de back-up proces is voltooid, blijft de wizard op de achtergrond uitgevoerd.
4.  Nadat de eerste back-up is voltooid, **taak voltooid** status wordt weergegeven **taken** deelvenster van de console MARS-agent.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u de Azure-portal: 
 
> [!div class="checklist"] 
> * Een Recovery Services-kluis maken 
> * De Microsoft Azure Recovery Services-agent downloaden 
> * De agent installeren 
> * Back-up configureren voor Windows Server 
> * Een back-up op aanvraag 

Doorgaan met de volgende zelfstudie voor het herstellen van bestanden van Azure naar Windows Server

> [!div class="nextstepaction"] 
> [Bestanden herstellen van Azure naar Windows Server](./tutorial-backup-restore-files-windows-server.md) 

