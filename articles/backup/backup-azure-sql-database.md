---
title: Een back-up van SQL Server-databases maken in Azure | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u een back-up van SQL Server maakt in Azure. In het artikel wordt ook uitgelegd hoe u SQL Server kunt herstellen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: b08d4e55deac4c9dd6a11fc9a278bdcfaa739627
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224308"
---
# <a name="back-up-sql-server-databases-on-azure-vms"></a>Back-ups maken van SQL Server-databases in virtuele Azure-machines 

SQL Server-databases zijn essentiële workloads waarvoor een laag Recovery Point Objective (RPO, beoogd herstelpunt) en langetermijnretentie nodig zijn. U kunt back-up van SQL Server-databases die worden uitgevoerd op virtuele Azure-machines met behulp van [Azure Backup](backup-overview.md). 

Deze zelfstudie leert u hoe u back-up van een SQL Server-database die wordt uitgevoerd op een Azure-VM naar een Azure Backup Recovery Services-kluis. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De vereisten voor het maken van een back-up van een SQL Server-exemplaar controleren.
> * Een kluis maken en configureren.
> * Databases detecteren en back-ups instellen.
> * Automatische beveiliging van databases instellen.

> [!NOTE]
> Deze functie is momenteel beschikbaar als openbare preview-versie.

## <a name="before-you-start"></a>Voordat u begint


- Zorg ervoor dat u een exemplaar van SQL Server die wordt uitgevoerd in Azure. U kunt snel [maken van een SQL Server-exemplaar](../sql-database/sql-database-get-started-portal.md) in de Azure Marketplace.
- De openbare preview-beperkingen bekijken.
- Bekijk scenario-ondersteuning.
- [Lees de veelgestelde vragen](faq-backup-sql-server.md) over dit scenario.

## <a name="preview-limitations"></a>Preview-beperkingen

Deze openbare preview heeft een aantal beperkingen.

- De virtuele machine met SQL Server moet verbinding met internet voor toegang tot Azure openbare IP-adressen. 
- U kunt back-up 2.000 SQL Server-databases in een kluis. Hebt u er meer? Maak dan nog een kluis. 
- Back-ups van [gedistribueerde beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) werken niet helemaal.
- SQL Server AlwaysOn Failover Cluster Instances (FCI's) worden niet ondersteund voor back-ups.
- Back-up van SQL Server configureren in de portal. Het is momenteel niet mogelijk om back-ups te configureren met Azure PowerShell, CLI of de REST API's.
- Back-up en herstel bewerkingen voor FCI gespiegelde databases, momentopnamen van databases en databases worden niet ondersteund.
- Databases met een groot aantal bestanden kunnen niet worden beveiligd. Het maximum aantal bestanden die worden ondersteund is niet deterministisch. Het is afhankelijk van het aantal bestanden en ook afhankelijk van de padlengte van de bestanden. 

Bekijk [veelgestelde vragen](faq-backup-sql-server.md) over het maken van back-ups van SQL Server-databases.
## <a name="scenario-support"></a>Scenario-ondersteuning

**Ondersteuning** | **Details**
--- | ---
**Ondersteunde implementaties** | Virtuele machines van Azure Marketplace en niet-Marketplace (SQL Server handmatig is geïnstalleerd) VM's worden ondersteund.
**Ondersteunde geografische gebieden** | Australië - zuidoost (ASE); Brazilië - zuid (BRS); Canada - centraal (CNC); Canada - oost (CE); VS - centraal (CUS); Azië - oost (EA); Australië - oost (AE); VS - oost (EUS); VS - oost 2 (EUS2); India - centraal (INC); India - zuid (INS); Japan - oost (JPE); Japan - west (JPW); Korea - centraal (KRC); Korea - zuid (KRS); VS - noord-centraal (NCUS); Europa - noord (NE); VS - zuid-centraal (SCUS); Azië - zuidoost (SEA); UK - zuid (UKS); UK - west (UKW); VS- west-centraal (WCUS); Europa -west (WE); VS - west (WUS); VS - west 2 (WUS 2)
**Ondersteunde besturingssystemen** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux wordt momenteel niet ondersteund.
**Ondersteunde SQL Server-versies** | SQL Server 2017, SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.

## <a name="prerequisites"></a>Vereisten

Voordat u een back-up van uw SQL Server-database maakt, controleert u de volgende voorwaarden:

- Bepalen of [maken van een Recovery Services-kluis](backup-azure-sql-database.md#create-a-recovery-services-vault) in dezelfde regio of land als de virtuele machine die als host voor de SQL Server-exemplaar fungeert.
- [Controleer de machtigingen van de virtuele machine](#fix-sql-sysadmin-permissions) die nodig zijn voor het back-up van de SQL-databases.
- Controleer of de virtuele machine heeft [netwerkverbinding](backup-azure-sql-database.md#establish-network-connectivity).
- Controleer of de SQL Server-databases zijn met de naam in overeenstemming met de [naamgevingsregels voor Azure Backup](backup-azure-sql-database.md).
- Controleer of er geen andere back-upoplossingen zijn ingeschakeld voor de database. Schakel alle andere SQL Server-back-ups uit voordat u dit scenario instelt. U kunt Azure Backup inschakelen voor een Azure-VM, samen met Azure Backup voor een SQL Server-database die wordt uitgevoerd op de virtuele machine, zonder een conflict.

### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

Voor alle bewerkingen is voor de SQL Server-VM een verbinding met openbare IP-adressen van Azure nodig. VM-bewerkingen (database-detectie, back-ups configureren, back-ups, herstelpunten herstellen) uitvallen zonder dat de verbinding met het openbare IP-adressen. Maak verbinding met een van deze opties:

- **Sta de IP-bereiken voor Azure Datacenter toe**: Sta de [IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653) in de download toe. Voor toegang in een netwerkbeveiligingsgroep (NSG), gebruikt u de `Set-AzureNetworkSecurityRule` cmdlet.
- **Implementeer een HTTP-proxyserver om verkeer te routeren**: Wanneer u een back-up van een SQL Server-database maakt op een Azure VM, gebruikt de back-upextensie op de virtuele machine de HTTPS-API's voor het verzenden van opdrachten voor het beheer aan Azure Backup en voor het verzenden van gegevens naar Azure Storage. De back-upextensie maakt ook gebruik van Azure Active Directory (Azure AD) voor verificatie. Leid het verkeer van de back-upextensie voor deze drie services via de HTTP-proxy. De extensie is het enige onderdeel dat geconfigureerd voor toegang tot het openbare internet.

Elke optie heeft voor- en nadelen.

**Optie** | **Voordelen** | **Nadelen**
--- | --- | ---
IP-bereiken toestaan | Geen extra kosten. | Ingewikkeld om te beheren, omdat de IP-adresbereiken na verloop van tijd veranderen. <br/><br/> Biedt toegang tot het geheel van Azure, niet alleen naar Azure Storage.
Een HTTP-proxy gebruiken   | Gedetailleerde controle via de proxy over de opslag-URL's is toegestaan. <br/><br/> Eén internettoegangspunt voor VM's. <br/><br/> Niet onderhevig aan Azure-IP-adreswijzigingen. | Extra kosten voor het uitvoeren van een VM met de proxysoftware. 

### <a name="set-vm-permissions"></a>VM-machtigingen instellen

Azure Backup biedt verschillende mogelijkheden bij het configureren van back-up voor een SQL Server-database:

- Azure Backup wordt toegevoegd de **AzureBackupWindowsWorkload** extensie.
- Het account wordt gemaakt van Azure Backup **NT SERVICE\AzureWLBackupPluginSvc** voor het detecteren van databases op de virtuele machine. Dit account wordt gebruikt voor back-up en herstel en SQL sysadmin-machtigingen vereist.
- Azure Backup maakt gebruik van de **NT AUTHORITY\SYSTEM** account voor databasedetectie en onderzoek. Dit account moet een openbare-aanmelding op SQL Server.

Als u de SQL Server-VM hebt gemaakt vanuit de Azure Marketplace, ontvangt u mogelijk een **UserErrorSQLNoSysadminMembership** fout. Als deze fout optreedt, [Volg deze instructies](#fix-sql-sysadmin-permissions).

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Controleer de naamgevingsrichtlijnen voor databases van Azure Backup

Vermijd bij databasenamen het volgende:

* Afsluitende/toonaangevende spaties
* Afsluitende uitroepteken (!)
* De afsluiting rechte haak (])

We hebben wel aliasing voor Azure-tabel niet-ondersteunde tekens, maar het is raadzaam dat u deze kunt vermijden. [Meer informatie](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server-databases detecteren

Databases die worden uitgevoerd op de virtuele machine detecteren. 

1. In de [Azure-portal](https://portal.azure.com), opent u de Recovery Services-kluis die u gebruiken om back-up van de database.

1. In het dashboard van de **Recovery Services-kluis** selecteert u **Backup**. 

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

1. Stel in het menu **Doel van de back-up** de optie **Waar wordt uw werkbelasting uitgevoerd?** in op **Azure** (de standaardwaarde).

1. In **Waarvan wilt u een back-up maken?** selecteert u **SQL Server in Azure-VM**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)


1. In **Doel van de back-up** > **DB's detecteren in VM's** selecteert u **Detectie starten** om te zoeken naar niet-beveiligde VM's in het abonnement. Dit kan een tijdje duren, afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement.

    ![Back-up in behandeling tijdens het zoeken naar databases in VM's](./media/backup-azure-sql-database/discovering-sql-databases.png)

1. Selecteer de virtuele machine waarop de SQL Server-database wordt in de lijst met virtuele machine > **DB's detecteren**.

1. Database-detectie bijhouden in het gebied **Meldingen**. Het kan even duren voor de taak is voltooid, afhankelijk van hoeveel databases bevinden zich in de virtuele machine. Wanneer de geselecteerde databases zijn gedetecteerd, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

    - Niet-beveiligde virtuele machines zouden na detectie in de lijst moeten verschijnen, gesorteerd op naam en resourcegroep.
    - Als een virtuele machine niet zoals verwacht op de lijst verschijnt, controleert u of hiervan al een back-up bestaat in een kluis.
    - Meerdere virtuele machines kunnen dezelfde naam hebben, maar ze moeten behoren tot verschillende resourcegroepen bevinden. 

1. Selecteer de virtuele machine met de SQL Server-database > **DB's detecteren**. 
1. Azure Backup detecteert alle SQL Server-databases op de virtuele machine. Tijdens de detectie, wordt de volgende activiteit plaatsvindt op de achtergrond:

    - Azure Backup wordt de virtuele machine geregistreerd bij de kluis voor back-up van de werkbelasting. Alle databases op de geregistreerde VM kunnen een back-worden gemaakt alleen bij deze kluis.
    - Azure Backup installeert de extensie **AzureBackupWindowsWorkload** op de virtuele machine. Er is geen agent geïnstalleerd op de SQL-database.
    - Azure Backup maakt het serviceaccount **NT Service\AzureWLBackupPluginSvc** op de virtuele machine.
      - Het serviceaccount wordt gebruikt voor alle back-up- en herstelbewerkingen.
      - Voor **NT-Service\AzureWLBackupPluginSvc** zijn systeembeheerdersrechten voor SQL vereist. Alle SQL Server-VM's die zijn gemaakt in de Azure Marketplace worden geleverd met de **SqlIaaSExtension** geïnstalleerd. De extensie **AzureBackupWindowsWorkload** maakt gebruik van de extensie **SQLIaaSExtension** om automatische de benodigde machtigingen op te halen.
    - Als u de virtuele machine hebt gemaakt vanuit de Marketplace, de virtuele machine beschikt niet over de **SqlIaaSExtension** geïnstalleerd, en de detectiebewerking is mislukt met de **UserErrorSQLNoSysAdminMembership** foutbericht weergegeven. Volg de instructies in [#fix-sql-sysadmin-permissions] om dit probleem op te lossen.

        ![De VM en database selecteren](./media/backup-azure-sql-database/registration-errors.png)



## <a name="configure-a-backup-policy"></a>Een back-upbeleid configureren 

Ter optimalisering van de back-upbelastingen stelt Azure Backup het maximumaantal databases in één back-uptaak in op 50.

- Om meer dan 50 back-ups te beschermen moet u meerdere databases configureren.
- U kunt ook automatische beveiliging inschakelen. Automatische beveiliging beschermt bestaande databases in één go, en nieuwe databases die zijn toegevoegd aan het exemplaar van de beschikbaarheidsgroep worden automatisch beveiligd.


Configureer de back-up als volgt:

1. Selecteer in het kluisdashboard **Back-up**. 

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

1. In **back-updoel**, stel **waar wordt uw werkbelasting uitgevoerd?** naar **Azure**.

1. In **wat wilt u een back-up?**, selecteer **SQL Server in virtuele Azure-machine**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    
1. In **back-updoel**, selecteer **back-up configureren**.

    ![Back-up configureren selecteren](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    ![Alle SQL Server-exemplaren met zelfstandige databases weergeven](./media/backup-azure-sql-database/list-of-sql-databases.png)

1. Selecteer de databases die u wilt beveiligen > **OK**.

    ![De database beveiligen](./media/backup-azure-sql-database/select-database-to-protect.png)

    - Alle SQL Server-exemplaren worden weergegeven (zelfstandige en beschikbaarheidsgroepen).
    - Selecteer de pijl-omlaag links van de instantie beschikbaarheid of de naam van de groep om te filteren.

1. In de **back-up** in het menu **back-upbeleid**. 

    ![Back-upbeleid selecteren](./media/backup-azure-sql-database/select-backup-policy.png)

1. In **back-upbeleid kiezen**, selecteer een beleid en selecteer vervolgens **OK**.

    - Het standaardbeleid selecteren: **HourlyLogBackup**.
    - Kies een bestaand back-upbeleid die eerder is gemaakt voor SQL.
    - [Een nieuw beleid definiëren](backup-azure-sql-database.md#configure-a-backup-policy) op basis van uw RPO en retentiebereik.
    - In de Preview-versie kunt u geen back-upbeleid bewerken.
    
1. In de **back-up** in het menu **back-up inschakelen**.

    ![Het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

1. De voortgang van de configuratie volgen in het **Systeemvak** van de portal.

    ![Systeemvak](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Maak een back-upbeleid

Een back-upbeleid bepaalt wanneer de back-ups worden gemaakt en hoe lang ze worden bewaard. 

- Een beleid wordt gemaakt op kluisniveau.
- U kunt hetzelfde back-upbeleid gebruiken voor meerdere kluizen, maar u moet het back-upbeleid toepassen op elke kluis.
- Wanneer u een back-upbeleid maakt, is een dagelijkse volledige back-up de standaardinstelling.
- U kunt een differentiële back-up toevoegen, maar alleen als u een wekelijkse volledige back-up configureert.
- [Meer informatie](backup-architecture.md#sql-server-backup-types) over verschillende soorten back-upbeleid.

Volg deze stappen voor het maken van een back-upbeleid:

1. Selecteer in de kluis **back-upbeleid** > **toevoegen**.
1. In de **toevoegen** in het menu **SQL Server in virtuele Azure-machine**. Hiermee definieert u het beleidstype.

   ![Een beleidstype voor het nieuwe back-upbeleid kiezen](./media/backup-azure-sql-database/policy-type-details.png)

1. Geef bij **Beleidsnaam** een naam voor het nieuwe beleid op. 
1. In **volledige back-upbeleid**, selecteer een **back-upfrequentie**. Kies **dagelijkse** of **wekelijkse**.

    - Voor **dagelijkse**, selecteert u het tijdstip en tijdzone voor de back-up om te beginnen.
    - U moet een volledige back-up uitvoeren. U kunt geen uitschakelen de **volledige back-up** optie.
    - Selecteer **volledige back-up** om het beleid weer te geven. 
    - U kunt geen differentiële back-ups maken voor dagelijkse volledige back-ups.
    - Voor **wekelijkse**, selecteert u de dag van de week, uur en tijdzone voor de back-up om te beginnen.

     ![Velden voor nieuw back-upbeleid](./media/backup-azure-sql-database/full-backup-policy.png)  

1. In **bewaartermijn**, alle opties zijn standaard geselecteerd. Schakel alle limieten voor het bereik van ongewenste bewaren die u niet wilt gebruiken, en stel de intervallen die u wilt gebruiken. 

    - Herstelpunten worden getagd voor retentie op basis van de bewaarperiode. Als u een dagelijkse volledige back-up selecteert, wordt slechts één volledige back-up per dag geactiveerd.
    - De back-up voor een specifieke dag wordt getagd en bewaard op basis van de wekelijkse bewaarperiode en uw wekelijkse bewaarbeleid.
    - De maandelijkse en jaarlijkse bewaarperioden werken op soortgelijke wijze.

   ![Intervalinstellingen voor bewaarperiode](./media/backup-azure-sql-database/retention-range-interval.png)

    

1. In het menu voor het **beleid voor een volledige back-up** selecteert u **OK** om de instellingen te accepteren.
1. Als u een beleid voor een differentiële back-up wilt toevoegen, selecteert u **Differentiële back-up**. 

   ![Intervalinstellingen voor bewaarperiode](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Het menu voor het beleid voor een differentiële back-up openen](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

1. In **Beleid voor een differentiële back-up** selecteert u **Inschakelen** om de frequentie- en bewaarinstellingen te openen. 

    - U kunt maximaal één differentiële back-up per dag activeren.
    - Differentiële back-ups kunnen maximaal 180 dagen worden bewaard. Als dat voor u te kort is, moet u volledige back-ups gebruiken.

1. Selecteer **OK** om het beleid op te slaan en terug te gaan naar het hoofdmenu **Back-upbeleid**.

1. Als u een back-upbeleid voor een transactielogboek wilt toevoegen, selecteert u **Logboekback-up**.
1. In **Logboekback-up** selecteert u **Inschakelen** en stelt u de frequentie- en bewaarinstellingen in. Logboekback-ups kunnen optreden zo vaak als elke 15 minuten en maximaal 35 dagen kunnen worden bewaard.
1. Selecteer **OK** om het beleid op te slaan en terug te gaan naar het hoofdmenu **Back-upbeleid**.

    ![Het logboekback-upbeleid bewerken](./media/backup-azure-sql-database/log-backup-policy-editor.png)

1. In de **back-upbeleid** menu, kies of u om in te schakelen **compressie van SQL-back-up**.
    - Compressie is standaard uitgeschakeld.
    - Op de back-end maakt Azure Backup gebruik van systeemeigen compressie van SQL-back-ups.

1. Als u klaar bent met het bewerken van het back-upbeleid, selecteert u **OK**.



## <a name="enable-auto-protection"></a>Automatische beveiliging inschakelen  

Schakel automatische beveiliging automatisch back-up van alle bestaande databases en databases die in de toekomst worden toegevoegd aan een zelfstandige SQL Server-exemplaar of een SQL Server Always On-beschikbaarheidsgroep. 

- Wanneer u automatische beveiliging inschakelen en selecteer een beleid, blijven de bestaande beveiligde databases gebruikmaken van het vorige beleid.
- Er is geen limiet voor het aantal databases die u voor automatische beveiliging selecteren kunt.

Volg deze stappen voor het inschakelen van automatische beveiliging:

1. In **Items voor back-up** selecteert u het exemplaar waarvoor u automatische beveiliging wilt inschakelen.
1. Selecteer de vervolgkeuzelijst onder **Automatische beveiliging** en stel deze in op **Aan**. Selecteer vervolgens **OK**.

    ![Automatische beveiliging inschakelen voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

1. Back-up wordt voor alle databases tegelijk getriggerd en kan worden gevolgd in **Back-uptaken**.


Als u automatische beveiliging uitschakelen moet, selecteert u de naam van het exemplaar onder **back-up configureren**, en selecteer **uitschakelen Autoprotect** voor het exemplaar. Er worden continu back-ups van de databases gemaakt. Echter wordt niet toekomstige databases automatisch worden beveiligd.

![Automatische beveiliging voor dat exemplaar uitschakelen](./media/backup-azure-sql-database/disable-auto-protection.png)


## <a name="fix-sql-sysadmin-permissions"></a>Problemen met systeembeheerdersrechten voor SQL oplossen

Als u nodig hebt om op te lossen machtigingen vanwege een **UserErrorSQLNoSysadminMembership** fout als volgt te werk:

1. Meld u bij SQL Server Management Studio (SSMS) aan met een account met systeembeheerdersrechten voor SQL Server. Windows-verificatie zou moeten werken, tenzij u speciale machtigingen nodig hebt.
1. Open op de SQL Server-exemplaar de **beveiliging/aanmeldingen** map.

    ![De map Security/Logins openen om accounts te bekijken](./media/backup-azure-sql-database/security-login-list.png)

1. Klik met de rechtermuisknop op de map **Logins** en selecteer **Nieuwe aanmelding**. In **Aanmelding - Nieuw** selecteert u **Zoeken**.

    ![In het dialoogvenster Aanmelding - Nieuw selecteert u Zoeken](./media/backup-azure-sql-database/new-login-search.png)

1. Het virtuele Windows-serviceaccount **NT SERVICE\AzureWLBackupPluginSvc** is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectiefase. Geef de accountnaam op, zoals wordt weergegeven in **Objectnaam invoeren die moet worden geselecteerd**. Selecteer **Namen controleren** om de naam te herleiden. Selecteer **OK**.

    ![Namen controleren selecteren om de naam van de onbekende service te herleiden](./media/backup-azure-sql-database/check-name.png)


1. Controleer of in **Serverrollen** de rol **sysadmin** is geselecteerd. Selecteer **OK**. De vereiste machtigingen moeten nu bestaan.

    ![Controleren of de serverfunctie sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

1. De database koppelen aan de Recovery Services-kluis. Klik in de Azure-portal in de lijst **Beschermde servers** met de rechtermuisknop op de server met een foutstatus > **DB's opnieuw detecteren**.

    ![Controleren of de server de juiste machtigingen heeft](./media/backup-azure-sql-database/check-erroneous-server.png)

1. Controleer de voortgang in het gebied **Meldingen**. Wanneer de geselecteerde databases zijn gevonden, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

U kunt ook inschakelen [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection) op de volledige instantie of AlwaysOn-beschikbaarheidsgroep. Als u wilt inschakelen, selecteert u de **ON** optie in de bijbehorende vervolgkeuzelijst in de **AUTOPROTECT** kolom. De [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection) functie kunt u beveiliging op alle bestaande databases. Het beveiligt ook automatisch een nieuwe databases die in de toekomst naar dat exemplaar of de beschikbaarheidsgroep worden toegevoegd.  

   ![Automatische beveiliging inschakelen voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](restore-sql-database-azure-vm.md) over het herstellen van SQL Server-databases vanuit back-up.
- [Meer informatie](manage-monitor-sql-database-backup.md) over het beheren van SQL Server-databases vanuit back-up.

