---
title: Azure Backup-Server installeren op Azure Stack | Microsoft Docs
description: Azure Backup-Server beveiligen of back-up van workloads in Azure-Stack gebruiken.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: Azure Backup-server; beveiligen van workloads; back-up van werkbelastingen
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/5/2018
ms.author: markgal
ms.openlocfilehash: c79ca93138961e294f03e283466dd66250472dae
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801735"
---
# <a name="install-azure-backup-server-on-azure-stack"></a>Azure Backup-Server op Azure Stack installeren

Dit artikel wordt uitgelegd hoe u Azure Backup-Server installeert op Azure-Stack. U kunt toepassing werkbelastingen die worden uitgevoerd in Azure-Stack vanaf één console beveiligen met Azure Backup-Server.

> [!NOTE]
> Raadpleeg voor meer informatie over de beveiligingsmogelijkheden naar [security voor Azure Backup biedt documentatie](backup-azure-security-feature.md).
>

U kunt ook infrastructuur beveiligen als een Service (IaaS) werkbelastingen, zoals virtuele machines in Azure.

De eerste stap naar de Azure Backup-Server slag is voor het instellen van een virtuele machine in Azure-Stack.

## <a name="using-an-iaas-vm-in-azure-stack"></a>Met behulp van een IaaS-virtuele machine in Azure-Stack

Als u een server voor Azure Backup-Server, kunt u beginnen met een Windows Server 2012 R2 Datacenter of Windows Server 2016 Datacenter afbeelding. Het artikel [uw eerste virtuele Windows-machine maken in de Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), bevat een zelfstudie voor het aan de slag met de aanbevolen virtuele machine. De aanbevolen minimale vereisten voor de server virtuele machine (VM) moeten zijn: standaard A2 twee kernen en 3.5 GB RAM-geheugen.

Werklasten beveiligen met Azure Backup-Server heeft veel nuances. Het artikel [DPM installeren als een virtuele machine van Azure](https://technet.microsoft.com/library/jj852163.aspx), helpt deze nuances uitgelegd. Lees dit artikel volledig voordat de machine is geïmplementeerd.

> [!NOTE]
> Azure Backup-Server is ontworpen om uit te voeren op een specifieke, één doel-virtuele machine. U kunt Azure Backup-Server niet installeren op:
> - Een computer die wordt uitgevoerd als een domeincontroller
> - Een computer waarop de toepassingsserverfunctie is geïnstalleerd
> - Een computer waarop Exchange Server wordt uitgevoerd
> - Een computer die een knooppunt van een cluster is

Altijd Azure Backup-Server toevoegen aan een domein. Als u Azure Backup-Server naar een ander domein verplaatst moet, eerst Azure Backup-Server installeren en vervolgens toevoegen aan het nieuwe domein. Wanneer u Azure Backup-Server implementeert, kunt u deze niet verplaatsen naar een nieuw domein.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Opslagreplicatie instellen

De Recovery Services-kluis opslagoptie replicatie kunt u kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Recovery Services-kluizen gebruiken standaard geografisch redundante opslag. Als deze kluis uw primaire kluis is, laat u de opslagoptie die is ingesteld op geografisch redundante opslag. Kies lokaal redundante opslag als u een goedkopere optie die minder duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy-grs.md) en [lokaal redundante](../storage/common/storage-redundancy-lrs.md) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Selecteer uw kluis om het kluisdashboard en het menu instellingen te openen. Als de **instellingen** menu niet wordt geopend, klikt u op **alle instellingen** op het kluisdashboard.
2. Op de **instellingen** menu, klikt u op **back-upinfrastructuur** > **back-upconfiguratie** openen de **back-upconfiguratie**menu. Op de **back-upconfiguratie** menu, kies de optie voor opslagreplicatie voor uw kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

## <a name="download-azure-backup-server-installer"></a>Azure Backup-Server-installatieprogramma downloaden

Wanneer u een Recovery Services-kluis maakt, het installatieprogramma van de Azure Backup-Server downloaden naar uw Azure-Stack virtuele machine via het menu aan de slag in de Recovery Services-kluis. De volgende stappen plaatsvinden in uw Azure-abonnement.

1. Naar uw Azure-Stack virtuele machine, [aanmelden bij uw Azure-abonnement in de Azure portal](https://portal.azure.com/).
2. Selecteer in het menu links **alle Services**.

    ![De optie alle Services in het hoofdmenu](./media/backup-mabs-install-azure-stack/click-all-services.png)

3. In de **alle services** dialoogvenster, type *Recovery Services*. Als u te typen begint, filtert uw invoer in de lijst met resources. Als u wordt weergegeven, selecteert u **Recovery Services-kluizen**.

    ![Typ de Recovery Services in het dialoogvenster met alle services](./media/backup-mabs-install-azure-stack/all-services.png)

    De lijst met Recovery Services-kluizen in het abonnement wordt weergegeven.

4. Selecteer in de lijst met Recovery Services-kluizen, uw kluis om het dashboard te openen.

    ![Typ de Recovery Services in het dialoogvenster met alle services](./media/backup-mabs-install-azure-stack/rs-vault-dashboard.png)

5. Klik in het menu aan de slag van de kluis op **back-up** om de wizard aan de slag te openen.

    ![Back-up aan de slag](./media/backup-mabs-install-azure-stack/getting-started-backup.png)

    De back-menu wordt geopend.

    ![Back-up-doelen-standaard-geopend](./media/backup-mabs-install-azure-stack/getting-started-menu.png)

6. In het menu back-up van de **waar wordt uw workload uitgevoerd** selecteert u **On-premises**. Van de **wat wilt u back-up maken?** vervolgkeuzelijst, selecteer de werkbelastingen die u wilt beveiligen met Azure Backup-Server. Als u niet weet welke workloads te selecteren, kiest u **Hyper-V virtuele Machines** en klik vervolgens op **infrastructuur voorbereiden**.

    ![on-premises en werkbelastingen als doelstellingen](./media/backup-mabs-install-azure-stack/getting-started-menu-onprem-hyperv.png)

    De **infrastructuur voorbereiden** menu wordt geopend.

7. In de **infrastructuur voorbereiden** menu, klikt u op **downloaden** om een webpagina om te downloaden van de installatiebestanden van Azure Backup-Server te openen.

    ![Getting Started wizard wijzigen](./media/backup-mabs-install-azure-stack/prepare-infrastructure.png)

    De Microsoft-webpagina die als host fungeert voor de downloadbare bestanden voor Azure Backup-Server wordt geopend.

8. Selecteer een taal in de downloadpagina van Microsoft Azure Backup-Server en klik op **downloaden**.

    ![Download center wordt geopend](./media/backup-mabs-install-azure-stack/mabs-download-center-page.png)

9. Het installatieprogramma van de Azure Backup-Server is samengesteld van acht bestanden - installatieprogramma en zeven .bin-bestanden. Controleer **bestandsnaam** alle vereiste bestanden selecteren en op **volgende**. Alle bestanden downloaden naar dezelfde map.

    ![Download center 1](./media/backup-mabs-install-azure-stack/download-center-selected-files.png)

    Omdat de grootte van het downloadpakket van alle bestanden > 3G, klikt u op een 10 Mbps downloaden koppeling het tot 60 minuten voor het downloaden duurt te voltooien. De bestanden gedownload naar de opgegeven locatie.

## <a name="extract-azure-backup-server-install-files"></a>Pak de bestanden van Azure Backup-Server installeren

Nadat u alle bestanden naar uw virtuele machine hebt gedownload, gaat u naar de downloadlocatie.

![Download center 1](./media/backup-mabs-install-azure-stack/download-mabs-installer.png)

1. U start de installatie uit de lijst met de gedownloade bestanden, klikt u op **MicrosoftAzureBackupserverInstaller.exe**.

    > [!WARNING]
    > Ten minste 4GB aan vrije ruimte is vereist voor het uitpakken van de setup-bestanden.
    >

2. Klik in het installatieprogramma van Azure Backup-Server op **volgende** om de wizard te starten.

    ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wiz-1.png)

3. Kies waar Azure Backup-Server installeren en klik op **volgende**.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-select-destination-1.png)

4. Controleer of de installatielocatie en klikt u op **extraheren**.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-extract-2.png)

5. Het installatieprogramma de bestanden worden uitgepakt en bereidt het installatieproces.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-install-3.png)

6. Zodra de extractie-proces is voltooid, klikt u op **voltooien** starten *setup.exe*. Setup.exe installeert Microsoft Azure Backup-Server.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-finish-4.png)

## <a name="install-the-software-package"></a>Het softwarepakket installeren

In de vorige stap die u hebt geklikt **voltooien** afsluiten van de extractie-fase en start de installatiewizard van de Azure Backup-Server.

![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5.png)

Azure Backup-Server deelt code met Data Protection Manager. Hier ziet u verwijzingen naar Data Protection Manager en DPM in het installatieprogramma van Azure Backup-Server. Hoewel Azure Backup-Server en Data Protection Manager zijn afzonderlijke producten, verwijzingen of hulpprogramma's met Data Protection Manager of DPM, van toepassing op Azure Backup-Server.

1. Start de wizard setup, klikt u op **Microsoft Azure Backup**.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5b.png)

2. Klik op het welkomstscherm op **volgende**.

    ![Azure Backup-Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-setup-6.png)

3. Op de *vereiste controleert* scherm, klikt u op **controleren** om te bepalen of de hardware en software-vereisten voor Azure Backup-Server is voldaan.

    ![Azure Backup-Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-7.png)

    Als uw omgeving de benodigde vereisten heeft, ziet u een bericht weergegeven dat aangeeft dat de computer voldoet aan de vereisten. Klik op **Volgende**.

    ![Azure Backup-Server - controle van vereisten is geslaagd](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-passed-8.png)

4. Microsoft Azure Backup-Server vereist dat SQL Server. Het installatiepakket voor de Azure Backup-Server wordt meegeleverd met de juiste SQL Server-binaire bestanden nodig als u niet wilt dat uw eigen SQL gebruiken. Aanbevolen wordt om het installatieprogramma een nieuw exemplaar van SQL Server toevoegen. Om te controleren of uw omgeving Gebruik SQL Server, klikt u op **controleren en installeren**.

   > [!NOTE]
   > Azure Backup-Server komt niet overeen met een extern exemplaar van SQL Server. Het exemplaar dat wordt gebruikt door Azure Backup-Server moet lokaal zijn.
   >

    ![Azure Backup-Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-install-9.png)

    Nadat u hebt gecontroleerd, als de computer de vereisten heeft voor het installeren van Azure Backup-Server, klikt u op **volgende**.

    ![Azure Backup-Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-ready-10.png)

    Als er een fout optreedt bij de aanbeveling om de computer opnieuw hebt opgestart, doet, start het installatieprogramma van dit scherm wordt weergegeven, klikt u op **opnieuw controleren**.

5. In de **installatie-instellingen**, Geef een locatie voor de installatie van bestanden van Microsoft Azure Backup-server en klikt u op **volgende**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-settings-11.png)

    De tijdelijke locatie is vereist voor back-up naar Azure. Zorg ervoor dat de grootte van de nieuwe locatie is gelijk aan ten minste 5% van de gegevens die zijn gepland voor een back-up naar Azure. Voor de beveiliging van schijven moeten afzonderlijke schijven worden geconfigureerd nadat de installatie is voltooid. Zie voor meer informatie over opslaggroepen [Configureer de opslaggroepen en schijfruimte](https://technet.microsoft.com/library/hh758075.aspx).

6. Op de **beveiligingsinstellingen** scherm een sterk wachtwoord opgeven voor de beperkte lokale gebruikersaccounts en klikt u op **volgende**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-security-12.png)

7. Op de **Microsoft Update Opt-In** scherm, selecteer of u wilt gebruiken *Microsoft Update* om te controleren op updates en klik op **volgende**.

   > [!NOTE]
   > U wordt aangeraden met Windows Update u naar Microsoft Update beveiligingsupdates en belangrijke updates voor Windows en andere producten, zoals Microsoft Azure Backup-Server biedt.
   >

    ![Back-PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-update-13.png)

8. Controleer de *samenvatting van instellingen* en klik op **installeren**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-summary-14.png)

    Afloop van de Azure Backup-Server installeert, wordt het installatieprogramma van Microsoft Azure Recovery Services agent onmiddellijk gestart door het installatieprogramma.

9. De Microsoft Azure Recovery Services Agent-installatieprogramma wordt geopend en de controles voor verbinding met Internet. Als de verbinding met Internet beschikbaar is, gaat u verder met de installatie. Als er geen verbinding, vindt u informatie van de proxy verbinding maken met Internet. Nadat u uw proxy-instellingen hebt opgegeven, klikt u op **volgende**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-proxy-15.png)

10. Klik op om de Microsoft Azure Recovery Services Agent **installeren**.

    ![Azure Backup-Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-mars-agent-16.png)

    De Microsoft Azure Recovery Services-agent, ook wel de Azure Backup-agent configureert de Azure Backup-Server naar de Recovery Services-kluis. Na de configuratie wordt Azure Backup-Server altijd back-upgegevens naar dezelfde Recovery Services-kluis.

11. Wanneer de Microsoft Azure Recovery Services-agent de installatie heeft voltooid, klikt u op **volgende** starten van de volgende fase: Azure Backup-Server registreren met de Recovery Services-kluis.

    ![Azure Backup-Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-complete-16.png)

    Het installatieprogramma start de **Wizard Server registreren**.

12. Schakel over naar uw Azure-abonnement en de Recovery Services-kluis. In de **infrastructuur voorbereiden** menu, klikt u op **downloaden** kluisreferenties downloaden. Als de **downloaden** knop in stap 2 is niet actief, selecteer **al gedownload of met behulp van de meest recente Azure Backup-Server-installatie** voor het activeren van de knop. De kluisreferenties downloaden naar de locatie waar u downloads opslaat. Let op deze locatie omdat u hebt deze nodig voor de volgende stap.

    ![Azure Backup-Server PreReq2](./media/backup-mabs-install-azure-stack/download-mars-credentials-17.png)

13. In de **kluis identificatie** menu, klikt u op **Bladeren** de Recovery Services-kluisreferenties vinden.

    ![Azure Backup-Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-vault-id-18.png)

    In de **Kluisreferenties Selecteer** dialoogvenster, gaat u naar de downloadlocatie, selecteert u uw referenties voor de kluis en op **Open**.

    Het pad naar de referenties wordt weergegeven in het menu identificatie van de kluis. Klik op **volgende** om door te gaan naar de instelling voor wachtwoordversleuteling.

14. In de **Versleutelingsinstelling** dialoogvenster bieden een wachtwoordzin voor de back-codering en een locatie voor het opslaan van de wachtwoordzin en klikt u op **volgende**.

    ![Azure Backup-Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-encryption-19.png)

    U kunt uw eigen wachtwoordzin opgeven of de wachtwoordzin generator gebruiken om te maken voor u. De wachtwoordzin is jouw e-mailadres en Microsoft niet opslaan of deze wachtwoordzin beheren. Om voor te bereiden voor een noodgeval, sla uw wachtwoordzin op een toegankelijke locatie.

    Nadat u op **volgende**, de Azure Backup-Server is geregistreerd bij de Recovery Services-kluis. Het installatieprogramma blijft de installatie van SQL Server en de Azure Backup-Server.

    ![Azure Backup-Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-still-installing-20.png)

15. Wanneer het installatieprogramma is voltooid, toont de Status dat alle software is geïnstalleerd.

    ![Azure Backup-Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-done-22.png)

    Wanneer de installatie is voltooid, worden de back-upserver van Azure-console en de Azure Backup-Server PowerShell pictogrammen op het bureaublad van de server gemaakt.

### <a name="add-backup-storage"></a>Back-upopslag toevoegen

De eerste back-up wordt opgeslagen op opslag die is gekoppeld aan de Azure Backup-Server-machine. Zie voor meer informatie over het toevoegen van schijven [Configureer de opslaggroepen en schijfruimte](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> U moet back-upopslag toevoegen, zelfs als u van plan bent om gegevens te verzenden naar Azure. In de huidige architectuur van Azure Backup-Server, de Azure Backup-kluis bevat de *tweede* kopie van de gegevens bij de lokale opslag bevat de eerste (en verplichte) back-up.
>
>

## <a name="network-connectivity"></a>Netwerkverbinding

Azure Backup-Server is vereist voor de verbinding met de Azure Backup-service voor het product werkt met succes. U kunt controleren of de machine de verbinding met Azure heeft, gebruiken de ```Get-DPMCloudConnection``` cmdlet in de Azure Backup-Server PowerShell-console. Als de uitvoer van de cmdlet ingesteld op TRUE is, wordt de verbinding, anders bestaat er is geen netwerkverbinding.

Op hetzelfde moment moet het Azure-abonnement in een foutloze toestand bevindt. De status van uw abonnement en beheert, meld u aan bij de [abonnement portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

Zodra u de status van de Azure-connectiviteit en het Azure-abonnement hebt, kunt u de onderstaande tabel om erachter te komen de gevolgen voor de back-up/herstel functionaliteit beschikbaar wordt gesteld.

| Status connectiviteit | Azure-abonnement | Back-up naar Azure | Back-up op schijf | Herstellen van Azure | Herstellen van de schijf |
| --- | --- | --- | --- | --- | --- |
| Verbonden |Actief |Toegestaan |Toegestaan |Toegestaan |Toegestaan |
| Verbonden |Verlopen |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verbonden |Gemaakt |Gestopt |Gestopt |Gestopt en Azure herstelpunten verwijderd |Gestopt |
| Verloren connectiviteit > 15 dagen |Actief |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verloren connectiviteit > 15 dagen |Verlopen |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verloren connectiviteit > 15 dagen |Gemaakt |Gestopt |Gestopt |Gestopt en Azure herstelpunten verwijderd |Gestopt |

### <a name="recovering-from-loss-of-connectivity"></a>Herstellen van het verlies van verbinding

Als een firewall of een proxy is verhinderen van toegang tot Azure, adressen geaccepteerde het volgende domein in de firewall/proxy-profiel:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

Zodra de verbinding naar Azure is hersteld naar de Azure Backup-Server, bepaalt de status van de Azure-abonnement de bewerkingen die kunnen worden uitgevoerd. Zodra de server is **verbonden**, gebruikt u de tabel in [netwerkverbinding](backup-mabs-install-azure-stack.md#network-connectivity) om de beschikbare bewerkingen te bekijken.

### <a name="handling-subscription-states"></a>Afhandeling van abonnementsstatussen

Het is mogelijk om te wijzigen van een Azure-abonnement van *verlopen* of *Deprovisioned* status naar *Active* status. Bij de abonnementsstatus geen is *Active*:

- Terwijl een abonnement is *Deprovisioned*, verliest functionaliteit. Herstellen van het abonnement op *Active*, revives van de back-up/herstel-functionaliteit. Als de back-upgegevens op de lokale schijf gehandhaafd met een groot genoeg bewaarperiode dat back-upgegevens kan worden opgehaald. Back-upgegevens in Azure is echter onherstelbaar verloren zodra het abonnement voert de *Deprovisioned* status.
- Terwijl een abonnement is *verlopen*, verliest functionaliteit. Geplande back-ups niet uitvoeren terwijl een abonnement is *verlopen*.

## <a name="troubleshooting"></a>Problemen oplossen

Als u Microsoft Azure Backup-server is mislukt met fouten tijdens de installatiefase (of backup of restore), raadpleegt u de [fout codes document](https://support.microsoft.com/kb/3041338).
U kunt ook verwijzen naar [Azure Backup gerelateerde Veelgestelde vragen](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Volgende stappen

Het artikel [uw omgeving voorbereiden voor DPM](https://technet.microsoft.com/library/hh758176.aspx), bevat informatie over ondersteunde configuraties voor Azure Backup-Server.

De volgende artikelen kunt u een beter begrip van werkbelastingsbescherming met Microsoft Azure Backup-Server te krijgen.

- [Back-up van SQL Server](backup-azure-backup-sql.md)
- [Back-up van SharePoint server](backup-azure-backup-sharepoint.md)
- [Alternatieve server back-up](backup-azure-alternate-dpm-server.md)
