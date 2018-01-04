---
title: Azure Backup-Server gebruiken voor back-up van werkbelastingen naar Azure | Microsoft Docs
description: Azure Backup-Server beveiligen of back-up van werkbelastingen naar de Azure portal gebruiken.
services: backup
documentationcenter: 
author: PVRK
manager: shivamg
editor: 
keywords: Azure Backup-server; beveiligen van workloads; back-up van werkbelastingen
ms.assetid: e7fb1907-9dc1-4ca1-8c61-50423d86540c
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: masaran;trinadhk;pullabhk;markgal;adigan
ms.openlocfilehash: addb4312ce1eb57ce86afae449eb3d31d0037418
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="preparing-to-back-up-workloads-using-azure-backup-server"></a>Met Azure Backup Server voorbereiden op het maken van back-ups van workloads
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

In dit artikel wordt uitgelegd hoe u uw omgeving back-up werkbelastingen zijn die gebruikmaken van Azure Backup-Server voorbereiden. U kunt de toepassing werkbelastingen zoals Hyper-V-machines, Microsoft SQL Server, SharePoint Server, Microsoft Exchange en Windows-clients vanuit één console beveiligen met Azure Backup-Server.

> [!NOTE]
> Azure Backup-Server kan de VMware-machines nu beveiligen en biedt mogelijkheden voor betere beveiliging. Het product te installeren, zoals wordt beschreven in de secties hieronder; Update 1 en de meest recente Azure Backup-Agent toepassen. Voor meer informatie over back-ups van VMware-servers met Azure Backup-Server, Zie het artikel [gebruik Azure back-up-Server voor back-up van een VMware-server](backup-azure-backup-server-vmware.md). Raadpleeg voor meer informatie over de beveiligingsmogelijkheden naar [Azure-back-beveiliging biedt documentatie](backup-azure-security-feature.md).
>
>

U kunt ook infrastructuur beveiligen als een Service (IaaS) werkbelastingen, zoals virtuele machines in Azure.

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel bevat de informatie en procedures voor het herstellen van virtuele machines die zijn geïmplementeerd met behulp van de Resource Manager-model.
>
>

Azure Backup-Server neemt veel van de back-functionaliteit van de werkbelasting van Data Protection Manager (DPM). Dit artikel bevat koppelingen naar de DPM-documentatie bevatten uitleg over enkele van de gedeelde functionaliteit. Azure Backup-Server deelt Hoewel veel van dezelfde functionaliteit als DPM. Azure Backup-Server heeft geen back-up naar tape of is deze dan geïntegreerd met System Center.

## <a name="1-choose-an-installation-platform"></a>1. Kies een installatieplatform
De eerste stap naar de Azure Backup-Server slag is voor het instellen van een Windows-Server. Uw server worden in Azure of on-premises.

### <a name="using-a-server-in-azure"></a>Gebruik van een server in Azure
Bij het kiezen van een server voor het uitvoeren van Azure Backup-Server, kunt dat u beginnen met een afbeelding van Windows Server 2012 R2 Datacenter. Het artikel [uw eerste virtuele Windows-machine maken in de Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), bevat een zelfstudie voor het aan de slag met de aanbevolen virtuele machine in Azure, zelfs als u nooit Azure al eerder hebt gebruikt. De aanbevolen minimale vereisten voor de server virtuele machine (VM) moeten zijn: A2 Standard met twee kernen en 3.5 GB RAM-geheugen.

Werklasten beveiligen met Azure Backup-Server heeft veel nuances. Het artikel [DPM installeren als een virtuele machine van Azure](https://technet.microsoft.com/library/jj852163.aspx), helpt deze nuances uitgelegd. Lees dit artikel volledig voordat de machine is geïmplementeerd.

### <a name="using-an-on-premises-server"></a>Met behulp van een on-premises server
Als u niet uitvoeren van de basis-server in Azure wilt, kunt u de server kunt uitvoeren op een Hyper-V-machine, een VMware-VM of een fysieke host. De aanbevolen minimale vereisten voor de serverhardware zijn twee kernen en 4 GB RAM-geheugen. De volgende tabel geeft een overzicht van de ondersteunde besturingssystemen:

| Besturingssysteem | Platform | SKU |
|:--- | --- |:--- |
| Windows Server 2016 en de meest recente SP 's |64 bits |Standard, Datacenter, Essentials (MABS v2 of hoger) |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Standard, Datacenter, Foundation |
| Windows Server 2012 en de meest recente SP's |64 bits |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 en de meest recente SP's |64 bits |Standard, Workgroup |

U kunt de DPM-opslag met behulp van Windows Server-Ontdubbeling ontdubbelen. Meer informatie over het [DPM en Ontdubbeling](https://technet.microsoft.com/library/dn891438.aspx) samenwerken als geïmplementeerd in Hyper-V-machines.

> [!NOTE]
> Azure Backup-Server is ontworpen om uit te voeren op een speciale server met één doel. U kunt Azure Backup-Server niet installeren op:
> - Een computer die als een domeincontroller
> - Een computer waarop de rol toepassingsserver is geïnstalleerd
> - Een computer die een System Center Operations Manager-beheerserver
> - Een computer waarop Exchange Server wordt uitgevoerd
> - Een computer die een knooppunt van een cluster

Altijd Azure Backup-Server toevoegen aan een domein. Als u van plan bent de server naar een ander domein verplaatsen, is het raadzaam dat u de server aan het nieuwe domein toevoegen voordat u Azure Backup-Server installeert. Een bestaande back-upserver van Azure-machine verplaatsen naar een nieuw domein nadat implementatie *niet ondersteund*.

## <a name="2-recovery-services-vault"></a>2. Recovery Services-kluis
Of u back-upgegevens naar Azure verzenden of lokaal houden, wordt de software moet zijn verbonden met Azure. Meer specifiek, de Azure Backup-Server-machine moet worden geregistreerd met een recovery services-kluis.

Een Recovery Services-kluis maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Bladeren** en typ in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluis**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.
3. Klik in het menu **Recovery Services-kluizen** op **Toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-microsoft-azure-backup/rs-vault-menu.png)

    De blade Recovery Services-kluis wordt geopend en u wordt gevraagd een **naam**, **abonnement**, **resourcegroep** en **locatie** in te voeren.

    ![Een Recovery Services-kluis maken, stap 5](./media/backup-azure-microsoft-azure-backup/rs-vault-attributes.png)
4. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam die tussen 2 en 50 tekens bevat. De naam moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.
5. Klik op **Abonnement** om de beschikbare lijst met abonnementen te bekijken. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u het standaard- (of voorgestelde) abonnement. Er zijn alleen meerdere mogelijkheden als uw organisatieaccount is gekoppeld aan meerdere Azure-abonnementen.
6. Klik op **Resourcegroep** om de lijst met beschikbare resourcegroepen te bekijken of klik op **Nieuw** om een nieuwe resourcegroep te maken. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.
7. Klik op **Locatie** om de geografische regio voor de kluis te selecteren.
8. Klik op **Create**. Het kan even duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen rechtsboven in de portal.
   Zodra uw kluis is gemaakt, wordt het in de portal geopend.

### <a name="set-storage-replication"></a>Opslagreplicatie instellen
U kunt met de optie voor opslagreplicatie kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Als deze kluis uw primaire kluis is, laat u de opslagoptie die is ingesteld op geografisch redundante opslag. Kies lokaal redundante opslag als u een goedkopere optie wilt die niet zo duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy.md#geo-redundant-storage) en [lokaal redundante](../storage/common/storage-redundancy.md#locally-redundant-storage) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Selecteer de kluis om het kluisdashboard en de blade Instellingen te openen. Als de blade **Instellingen** niet wordt geopend, klikt u op **Alle instellingen** op het kluisdashboard.
2. Klik op de blade **Instellingen** op **Back-upinfrastructuur** > **Back-upconfiguratie** om de blade **Back-upconfiguratie** te openen. Kies op de blade **Back-upconfiguratie** de optie voor opslagreplicatie voor uw kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Nadat u de opslagoptie voor uw kluis hebt gekozen, bent u klaar om de VM aan de kluis te koppelen. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

## <a name="3-software-package"></a>3. Softwarepakket
### <a name="downloading-the-software-package"></a>Het softwarepakket downloaden
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Als u al een Recovery Services-kluis is geopend, gaat u verder met stap 3. Als er nog geen Recovery Services-kluis is geopend, maar Azure Portal wordt weergeven, klikt u in het menu Hub op **Bladeren**.

   * Typ in de lijst met resources **Recovery Services**.
   * Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer **Recovery Services-kluizen** wordt weergegeven, klikt u erop.

     ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     De lijst met Recovery Services-kluizen wordt weergegeven.
   * Selecteer in de lijst met Recovery Services-kluizen een kluis.

     Het geselecteerde kluisdashboard wordt geopend.

     ![Blade Kluis openen](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. De **instellingen** blade wordt geopend standaard. Als dit is gesloten, klikt u op **instellingen** om de instellingenblade te openen.

    ![Blade Kluis openen](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Klik op **back-up** om de wizard aan de slag te openen.

    ![Back-up aan de slag](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    In de **aan de slag met back-up** blade die wordt geopend, **back-doelen** worden automatisch geselecteerd.

    ![Back-up-doelen-standaard-geopend](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. In de **back-updoel** blade van de **waar wordt uw workload uitgevoerd** selecteert u **On-premises**.

    ![on-premises en werkbelastingen als doelstellingen](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    Van de **wat wilt u back-up maken?** vervolgkeuzelijst, selecteer de werkbelastingen die u wilt beveiligen met behulp van Azure Backup-Server en klik vervolgens op **OK**.

    De **aan de slag met back-up** wizard switches de **infrastructuur voorbereiden** optie voor het back-up van werkbelastingen naar Azure.

   > [!NOTE]
   > Als u alleen back-up van bestanden en mappen wilt, raden wij aan met behulp van de Azure Backup agent Volg de instructies in het artikel [eerste kennismaking: Maak een back-up van bestanden en mappen](backup-try-azure-backup-in-10-mins.md). Als u wilt meer dan de bestanden en mappen beveiligen of u van plan bent om uit te breiden de behoeften van de beveiliging in de toekomst, selecteert u deze werkbelastingen.
   >
   >

    ![Getting Started wizard wijzigen](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. In de **infrastructuur voorbereiden** blade die wordt geopend op de **downloaden** koppelingen voor Azure Backup-Server installeren en de kluisreferenties downloaden. U kunt de kluisreferenties gebruiken tijdens de registratie van Azure Backup-Server in de recovery services-kluis. De koppelingen gaat u naar het Download Center waar het softwarepakket kan worden gedownload.

    ![Infrastructuur voorbereiden voor Azure Backup-Server](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Selecteer alle bestanden en klik op **volgende**. Alle bestanden die afkomstig zijn van de downloadpagina van Microsoft Azure Backup in downloaden en alle bestanden in dezelfde map te plaatsen.

    ![Download center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Omdat de grootte van het downloadpakket van alle bestanden samen > 3G, klikt u op een 10 Mbps downloaden koppeling het tot 60 minuten voor het downloaden duurt te voltooien.

### <a name="extracting-the-software-package"></a>Het uitpakken van het softwarepakket
Nadat u alle bestanden hebt gedownload, klikt u op **MicrosoftAzureBackupInstaller.exe**. Hiermee start u de **Wizard Setup van Microsoft Azure Backup** uitpakken van de setup-bestanden naar een door u opgegeven locatie. Volg de stappen in de wizard en klik op de **extraheren** knop om te beginnen met de extractie.

> [!WARNING]
> Ten minste 4GB aan vrije ruimte is vereist voor het uitpakken van de setup-bestanden.
>
>

![Back-installatiewizard voor Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Zodra de extractie proces is voltooid, schakel het selectievakje in om te starten na uitgepakte *setup.exe* om te beginnen met de installatie van Microsoft Azure Backup-Server en klik op de **voltooien** knop.

### <a name="installing-the-software-package"></a>Het softwarepakket installeren
1. Klik op **Microsoft Azure Backup** starten van de wizard setup.

    ![Back-installatiewizard voor Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Klik op het welkomstscherm op de **volgende** knop. Hiermee gaat u naar de *vereiste controleert* sectie. Klik op dit scherm **controleren** om te bepalen of de hardware en software-vereisten voor Azure Backup-Server is voldaan. Als aan alle vereisten zijn voldaan is, ziet u een bericht weergegeven dat aangeeft dat de computer voldoet aan de vereisten. Klik op de **volgende** knop.

    ![Azure Backup-Server - welkomstpagina en de vereisten controleren](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup-Server vereist dat SQL Server Standard en het installatiepakket voor de Azure Backup-Server wordt geleverd met de juiste SQL Server-binaire bestanden die nodig zijn. Bij het starten met een nieuwe installatie van de Azure Backup-Server, moet u de optie kiest **nieuw exemplaar van SQL Server installeren met deze installatie** en klik op de **controleren en installeren** knop. Nadat de vereiste onderdelen zijn geïnstalleerd, klikt u op **volgende**.

    ![Azure Backup-Server - controle van SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Als een fout bij de aanbeveling optreedt om de computer opnieuw hebt opgestart, doen en klikt u op **opnieuw controleren**.

   > [!NOTE]
   > Azure Backup-Server komt niet overeen met een extern exemplaar van SQL Server. Het exemplaar dat wordt gebruikt door Azure Backup-Server moet lokaal zijn.
   >
   >
4. Geef een locatie voor de installatie van bestanden van Microsoft Azure Backup-server en klik op **volgende**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    De tijdelijke locatie is een vereiste voor back-up naar Azure. Zorg ervoor dat de scratchruimte locatie is ten minste 5% van de gegevens die zijn gepland voor een back-up naar de cloud. Voor de beveiliging van schijven moeten afzonderlijke schijven worden geconfigureerd nadat de installatie is voltooid. Zie voor meer informatie over opslaggroepen [Configureer de opslaggroepen en schijfruimte](https://technet.microsoft.com/library/hh758075.aspx).
5. Geef een sterk wachtwoord voor beperkte lokale gebruikersaccounts en klik op **volgende**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Selecteer of u wilt gebruiken *Microsoft Update* om te controleren op updates en klik op **volgende**.

   > [!NOTE]
   > U wordt aangeraden met Windows Update u naar Microsoft Update beveiligingsupdates en belangrijke updates voor Windows en andere producten, zoals Microsoft Azure Backup-Server biedt.
   >
   >

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Controleer de *samenvatting van instellingen* en klik op **installeren**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. De installatie gebeurt in fasen. In de eerste fase wordt de Microsoft Azure Recovery Services Agent geïnstalleerd op de server. De wizard controleert ook of er verbinding met Internet. Als de verbinding met Internet beschikbaar kunt u doorgaan met installatie, zo niet, moet u proxy-gegevens voor verbinding met het Internet.

    De volgende stap is het configureren van de Microsoft Azure Recovery Services Agent. Als onderdeel van de configuratie moet u uw kluisreferenties opgeven voor de machine om de recovery services-kluis te registreren. U wordt ook een wachtwoordzin in om te versleutelen/ontsleutelen van de gegevens die worden verzonden tussen Azure en uw bedrijf opgeven. U kunt automatisch genereren van een wachtwoordzin of uw eigen minimaal 16 tekens wachtwoordzin opgeven. Ga door met de wizard totdat de agent is geconfigureerd.

    ![Azure Backup Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Nadat de registratie van de Microsoft Azure Backup-server is voltooid, gaat de installatiewizard van de algehele door naar de installatie en configuratie van SQL Server en de Azure Backup-Server-onderdelen. Zodra de installatie van de SQL Server-onderdelen is voltooid, wordt de Azure Backup-Server-onderdelen zijn geïnstalleerd.

    ![Azure Backup-server](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Wanneer de installatiestap is voltooid, wordt bureaubladpictogrammen van het product gemaakt ook. Dubbelklik op het pictogram voor het starten van het product.

### <a name="add-backup-storage"></a>Back-upopslag toevoegen
De eerste back-up wordt opgeslagen op opslag die is gekoppeld aan de Azure Backup-Server-machine. Zie voor meer informatie over het toevoegen van schijven [Configureer de opslaggroepen en schijfruimte](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> U moet back-upopslag toevoegen, zelfs als u van plan bent om gegevens te verzenden naar Azure. In de huidige architectuur van Azure Backup-Server, de Azure Backup-kluis bevat de *tweede* kopie van de gegevens bij de lokale opslag bevat de eerste (en verplichte) back-up.
>
>

## <a name="4-network-connectivity"></a>4. Netwerkverbinding
Azure Backup-Server is vereist voor de verbinding met de Azure Backup-service voor het product werkt met succes. U kunt controleren of de machine de verbinding met Azure heeft, gebruiken de ```Get-DPMCloudConnection``` cmdlet in de Azure Backup-Server PowerShell-console. Als de uitvoer van de cmdlet TRUE is en vervolgens de verbinding is, anders er is geen netwerkverbinding.

Op hetzelfde moment moet het Azure-abonnement in een foutloze toestand bevindt. De status van uw abonnement en beheert, meld u aan bij de [abonnement portal](https://account.windowsazure.com/Subscriptions).

Zodra u de status van de Azure-connectiviteit en het Azure-abonnement hebt, kunt u de onderstaande tabel om erachter te komen de gevolgen voor de back-up/herstel functionaliteit beschikbaar wordt gesteld.

| Status connectiviteit | Azure-abonnement | Back-up naar Azure | Back-up op schijf | Herstellen van Azure | Herstellen van de schijf |
| --- | --- | --- | --- | --- | --- |
| Verbonden |Actief |Toegestaan |Toegestaan |Toegestaan |Toegestaan |
| Verbonden |Verlopen |Stopped |Stopped |Toegestaan |Toegestaan |
| Verbonden |Gemaakt |Stopped |Stopped |Gestopt en Azure herstelpunten verwijderd |Stopped |
| Verloren connectiviteit > 15 dagen |Actief |Stopped |Stopped |Toegestaan |Toegestaan |
| Verloren connectiviteit > 15 dagen |Verlopen |Stopped |Stopped |Toegestaan |Toegestaan |
| Verloren connectiviteit > 15 dagen |Gemaakt |Stopped |Stopped |Gestopt en Azure herstelpunten verwijderd |Stopped |

### <a name="recovering-from-loss-of-connectivity"></a>Herstellen van het verlies van verbinding
Als u een firewall of een proxy die is verhinderen van toegang tot Azure hebt, moet u goedgekeurde IP-adressen de volgende adressen domein in de firewall/proxy-profiel:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Zodra de verbinding met Azure met de Azure Backup-Server-machine is hersteld, worden de bewerkingen die kunnen worden uitgevoerd worden bepaald door de status van de Azure-abonnement. De bovenstaande tabel bevat informatie over de bewerkingen die zijn toegestaan als de machine 'verbonden'.

### <a name="handling-subscription-states"></a>Afhandeling van abonnementsstatussen
Het is mogelijk te houden van een Azure-abonnement van een *verlopen* of *Deprovisioned* status naar de *Active* status. Maar dit heeft enkele gevolgen van het gedrag van het product terwijl de status niet is *Active*:

* Een *Deprovisioned* abonnement verliest functionaliteit voor de periode dat deze is gemaakt. In te schakelen *Active*, de functionaliteit van het product van de back-up/herstel wordt weer actief. De back-upgegevens op de lokale schijf kunnen ook worden opgehaald als deze is gehouden met een groot genoeg bewaarperiode. De back-upgegevens in Azure is echter onherstelbaar verloren zodra het abonnement voert de *Deprovisioned* status.
* Een *verlopen* functionaliteit voor het abonnement alleen verliest totdat deze is gemaakt *Active* opnieuw. Alle back-ups voor de periode die het abonnement is gepland *verlopen* kan niet worden uitgevoerd.

## <a name="troubleshooting"></a>Problemen oplossen
Als Microsoft Azure Backup-server is mislukt met fouten tijdens de installatiefase (of backup of restore), raadpleegt u dit [fout codes document](https://support.microsoft.com/kb/3041338) voor meer informatie.
U kunt ook verwijzen naar [Azure Backup gerelateerde Veelgestelde vragen](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Volgende stappen
U kunt gedetailleerde informatie ophalen over [uw omgeving voorbereiden voor DPM](https://technet.microsoft.com/library/hh758176.aspx) op de website Microsoft TechNet. Het bevat ook informatie over ondersteunde configuraties waarop Azure Backup-Server kan worden geïmplementeerd en gebruikt.

Deze artikelen kunt u een beter begrip van de beveiliging van werkbelastingen met behulp van Microsoft Azure Backup-server te krijgen.

* [Back-up van SQL Server](backup-azure-backup-sql.md)
* [Back-up van SharePoint server](backup-azure-backup-sharepoint.md)
* [Alternatieve server back-up](backup-azure-alternate-dpm-server.md)
