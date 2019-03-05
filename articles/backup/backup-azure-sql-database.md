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
ms.openlocfilehash: 61219fc4e1fc329708a7e58ee6a293e4e25cca31
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56887808"
---
# <a name="back-up-sql-server-databases-on-azure-vms"></a>Back-ups maken van SQL Server-databases in virtuele Azure-machines 

SQL Server-databases zijn essentiële workloads waarvoor een laag Recovery Point Objective (RPO, beoogd herstelpunt) en langetermijnretentie nodig zijn. Met behulp van [Azure Backup](backup-overview.md) kunt u back-ups maken van SQL Server-databases die worden uitgevoerd op virtuele Azure-VM's. 

In dit artikel wordt uitgelegd hoe u een back-up naar een Azure Backup Recovery Services-kluis maakt van een SQL Server-database die op een Azure-VM wordt uitgevoerd. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De vereisten voor het maken van een back-up van een SQL Server-exemplaar controleren.
> * Een kluis maken en configureren.
> * Databases detecteren en back-ups instellen.
> * Automatische beveiliging van databases instellen.

> [!NOTE]
> Deze functie is momenteel beschikbaar als openbare preview-versie.

## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, controleert u het volgende:

1. Zorg ervoor dat er een SQL Server-exemplaar wordt uitgevoerd in Azure. U kunt [snel een SQL Server-exemplaar maken](../sql-database/sql-database-get-started-portal.md) in de marketplace.
2. Bekijk de onderstaande beperkingen van openbare preview.
3. Bekijk scenario-ondersteuning.
4. [Lees de veelgestelde vragen](faq-backup-sql-server.md) over dit scenario.


## <a name="preview-limitations"></a>Preview-beperkingen

Deze openbare preview heeft een aantal beperkingen.

- Voor de VM waarop SQL Server wordt uitgevoerd is een internetverbinding vereist voor toegang tot openbare IP-adressen van Azure. 
- U kunt van maximaal 2000 SQL Server-databases een back-up maken in een kluis. Hebt u er meer? Maak dan nog een kluis. 
- Back-ups van [gedistribueerde beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) werken niet helemaal.
- SQL Server AlwaysOn Failover Cluster Instances (FCI's) worden niet ondersteund voor back-ups.
- Back-up van SQL Server moet in de portal worden geconfigureerd. Het is momenteel niet mogelijk om back-ups te configureren met Azure PowerShell, CLI of de REST API's.
- Back-up- en herstelbewerkingen voor gespiegelde FCI-databases, databasemomentopnamen en databases worden niet ondersteund.
- Databases met zeer veel bestanden kunnen niet worden beveiligd. Het maximumaantal ondersteunde bestanden is niet deterministisch. Het hangt niet alleen af van het aantal bestanden, maar ook van de padlengte van de bestanden. 

Bekijk [veelgestelde vragen](faq-backup-sql-server.md) over het maken van back-ups van SQL Server-databases.
## <a name="scenario-support"></a>Scenario-ondersteuning

**Ondersteuning** | **Details**
--- | ---
**Ondersteunde implementaties** | SQL Marketplace Azure-VM's en niet-Marketplace-VM's (SQL Server handmatig geïnstalleerd) worden ondersteund.
**Ondersteunde geografische gebieden** | Australië - zuidoost (ASE); Brazilië - zuid (BRS); Canada - centraal (CNC); Canada - oost (CE); VS - centraal (CUS); Azië - oost (EA); Australië - oost (AE); VS - oost (EUS); VS - oost 2 (EUS2); India - centraal (INC); India - zuid (INS); Japan - oost (JPE); Japan - west (JPW); Korea - centraal (KRC); Korea - zuid (KRS); VS - noord-centraal (NCUS); Europa - noord (NE); VS - zuid-centraal (SCUS); Azië - zuidoost (SEA); UK - zuid (UKS); UK - west (UKW); VS- west-centraal (WCUS); Europa -west (WE); VS - west (WUS); VS - west 2 (WUS 2)
**Ondersteunde besturingssystemen** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux wordt momenteel niet ondersteund.
**Ondersteunde SQL Server-versies** | SQL Server 2017; SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.


## <a name="prerequisites"></a>Vereisten

Voordat u een back-up van uw SQL Server-database maakt, controleert u de volgende voorwaarden:

1. Identificeer of [maak](backup-azure-sql-database.md#create-a-recovery-services-vault) een Recovery Services-kluis in hetzelfde gebied of land als de virtuele machine die het SQL Server-exemplaar host.V
2. [Controleer de VM-machtigingen](#fix-sql-sysadmin-permissions) die nodig zijn voor de back-up van de SQL-databases.
3. Controleer of de virtuele machine [netwerkverbinding](backup-azure-sql-database.md#establish-network-connectivity) heeft.
4. Controleer of de SQL Server-databases zijn benoemd in overeenstemming met de [naamgevingsrichtlijnen](backup-azure-sql-database.md) van Azure Backup.
5. Controleer of er geen andere back-upoplossingen zijn ingeschakeld voor de database. Schakel alle andere SQL Server-back-ups uit voordat u dit scenario instelt. U kunt zonder problemen Azure Backup inschakelen voor een Azure-VM en tegelijkertijd voor een SQL Server-database die wordt uitgevoerd op de virtuele machine.


### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

Voor alle bewerkingen is voor de SQL Server-VM een verbinding met openbare IP-adressen van Azure nodig. VM-bewerkingen (databasedetectie, back-ups configureren, back-ups plannen, herstelpunten herstellen etc.) zullen mislukken zonder verbinding met de openbare IP-adressen. Maak verbinding met een van deze opties:

- **Sta de IP-bereiken voor Azure Datacenter toe**: Sta de [IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653) in de download toe. Voor toegang in een NSG (netwerkbeveiligingsgroep) gebruikt u de cmdlet **Set-AzureNetworkSecurityRule**.
- **Implementeer een HTTP-proxyserver om verkeer te routeren**: Wanneer u een back-up van een SQL Server-database op een Azure-VM maakt, gebruikt de back-upextensie op de virtuele machine de HTTPS-API's voor het verzenden van beheeropdrachten naar Azure Backup en gegevens naar Azure Storage. De back-upextensie maakt ook gebruik van Azure Active Directory (Azure AD) voor verificatie. Leid het verkeer van de back-upextensie voor deze drie services via de HTTP-proxy. De extensie is het enige onderdeel dat wordt geconfigureerd voor toegang tot het openbare internet.

Elke optie heeft voor- en nadelen

**Optie** | **Voordelen** | **Nadelen**
--- | --- | ---
IP-bereiken toestaan | Geen extra kosten. | Ingewikkeld om te beheren, omdat de IP-adresbereiken na verloop van tijd veranderen. <br/><br/> Biedt toegang tot heel Azure, niet alleen tot Azure Storage.
Een HTTP-proxy gebruiken   | Gedetailleerde controle via de proxy over de opslag-URL's is toegestaan. <br/><br/> Eén internettoegangspunt voor VM's. <br/><br/> Niet onderhevig aan Azure-IP-adreswijzigingen. | Extra kosten voor het uitvoeren van een VM met de proxysoftware. 

### <a name="set-vm-permissions"></a>VM-machtigingen instellen

Azure Backup doet een aantal dingen wanneer u een back-up voor een SQL Server-database configureert:

- Voegt de extensie **AzureBackupWindowsWorkload** toe.
- Om databases op de virtuele machine te detecteren, wordt door Azure Backup het account **NT SERVICE\AzureWLBackupPluginSvc** gemaakt. Dit account wordt gebruikt voor back-up en herstel en vereist systeembeheerdersrechten voor SQL.
- Het account **NT AUTHORITY\SYSTEM** wordt door Azure Backup gebruikt voor databasedetectie/-aanvragen, dus dit account moet een openbare aanmelding via SQL zijn.

Als u de SQL Server-VM niet hebt gemaakt vanuit de Azure Marketplace, ontvangt u mogelijk het foutbericht **UserErrorSQLNoSysadminMembership**. Volg in dit geval [deze instructies](#fix-sql-sysadmin-permissions).

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Controleer de naamgevingsrichtlijnen voor databases van Azure Backup

Vermijd bij databasenamen het volgende:

  * Voorloop- en volgspaties
  * Volguitroepteken '!'
  * Vierkante haak sluiten ‘]’

We hebben wel aliasing voor niet-ondersteunde tekens in Azure-tabellen, maar we raden aan deze te vermijden. [Meer informatie](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server-databases detecteren

Detecteer databases die op de virtuele machine worden uitgevoerd. 
1. In de [Azure-portal](https://portal.azure.com) opent u de Recovery Services-kluis die u gebruikt om een back-up te maken van de database.

2. In het dashboard van de **Recovery Services-kluis** selecteert u **Backup**. 

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Stel in het menu **Doel van de back-up** de optie **Waar wordt uw werkbelasting uitgevoerd?** in op **Azure** (de standaardwaarde).

4. In **Waarvan wilt u een back-up maken?** selecteert u **SQL Server in Azure-VM**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)


5. In **Doel van de back-up** > **DB's detecteren in VM's** selecteert u **Detectie starten** om te zoeken naar niet-beveiligde VM's in het abonnement. Dit kan een tijdje duren, afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement.

    ![Back-up in behandeling tijdens het zoeken naar databases in VM's](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Selecteer in de lijst met virtuele machines de VM waarop de SQL Server-database wordt uitgevoerd > **DB's detecteren**.

7. Database-detectie bijhouden in het gebied **Meldingen**. Het kan even duren voor de taak is voltooid, afhankelijk van hoeveel databases zich op de virtuele machine bevinden. Wanneer de geselecteerde databases zijn gedetecteerd, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

    - Niet-beveiligde virtuele machines zouden na detectie in de lijst moeten verschijnen, gesorteerd op naam en resourcegroep.
    - Als een virtuele machine niet zoals verwacht op de lijst verschijnt, controleert u of hiervan al een back-up bestaat in een kluis.
    - Meerdere virtuele machines kunnen dezelfde naam hebben, maar behoren altijd tot verschillende resourcegroepen. 

9. Selecteer de VM waarop de SQL Server-database wordt uitgevoerd > **DB's detecteren**. 
10. Azure Backup detecteert alle SQL Server-databases op de virtuele machine. Tijdens de detectie gebeurt het volgende op de achtergrond:

    - Azure Backup registreert de virtuele machine met de kluis voor een back-up van de werkbelasting. Van alle databases op de geregistreerde virtuele machine kan alleen op deze kluis een back-up worden gemaakt.
    - Azure Backup installeert de extensie **AzureBackupWindowsWorkload** op de virtuele machine. Er is geen agent geïnstalleerd op de SQL-database.
    - Azure Backup maakt het serviceaccount **NT Service\AzureWLBackupPluginSvc** op de virtuele machine.
      - Het serviceaccount wordt gebruikt voor alle back-up- en herstelbewerkingen.
      - Voor **NT-Service\AzureWLBackupPluginSvc** zijn systeembeheerdersrechten voor SQL vereist. Op alle SQL Server-VM's die in de Azure Marketplace zijn gemaakt, is **SqlIaaSExtension** geïnstalleerd. De extensie **AzureBackupWindowsWorkload** maakt gebruik van de extensie **SQLIaaSExtension** om automatische de benodigde machtigingen op te halen.
    - Als u de virtuele machine niet vanuit de marketplace hebt gemaakt, beschikt de virtuele machine niet over **SqlIaaSExtension** en niet over de detectiebewerkingsfouten met het foutbericht **UserErrorSQLNoSysAdminMembership**. Volg de instructies in [#fix-sql-sysadmin-permissions] om dit probleem op te lossen.

        ![De VM en database selecteren](./media/backup-azure-sql-database/registration-errors.png)



## <a name="configure-a-backup-policy"></a>Een back-upbeleid configureren 

Ter optimalisering van de back-upbelastingen stelt Azure Backup het maximumaantal databases in één back-uptaak in op 50.

- Om meer dan 50 back-ups te beschermen moet u meerdere databases configureren.
- U kunt ook automatische beveiliging inschakelen. Automatische beveiliging beschermt in één keer bestaande databases en automatisch nieuwe databases die zijn toegevoegd aan het exemplaar van de beschikbaarheidsgroep.


Configureer de back-up als volgt:

1. Selecteer in het kluisdashboard **Back-up**. 

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Stel in het menu **Doel van de back-up** de optie **Waar wordt uw werkbelasting uitgevoerd?** in op **Azure**.

4. In **Waarvan wilt u een back-up maken?** selecteert u **SQL Server in Azure-VM**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    
5. In **Doel van de back-up** selecteert u **Back-up configureren**.

    ![Back-up configureren selecteren](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    ![Alle SQL Server-exemplaren met zelfstandige databases weergeven](./media/backup-azure-sql-database/list-of-sql-databases.png)

6. Selecteer alle databases die u wilt beveiligen > **OK**.

    ![De database beveiligen](./media/backup-azure-sql-database/select-database-to-protect.png)

    - Alle SQL Server-exemplaren worden weergegeven (zelfstandige en beschikbaarheidsgroepen).
    - Selecteer de pijl-omlaag aan de linkerkant van exemplaarnaam/beschikbaarheidsgroep om te filteren.

7. In het menu **Back-up** selecteert u **Back-upbeleid**. 

    ![Back-upbeleid selecteren](./media/backup-azure-sql-database/select-backup-policy.png)

8. In **Back-upbeleid kiezen** selecteert u een beleid en klikt u vervolgens op **OK**.

    - Het standaardbeleid selecteren: **HourlyLogBackup**.
    - Een bestaand back-upbeleid kiezen dat u eerder hebt gemaakt voor SQL.
    - [Een nieuw beleid definiëren](backup-azure-sql-database.md#configure-a-backup-policy) op basis van uw RPO en retentiebereik.
    - In de Preview-versie kunt u geen back-upbeleid bewerken.
    
9. In het **menu Back-up** selecteert u **Back-up inschakelen**.

    ![Het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

10. De voortgang van de configuratie volgen in het **Systeemvak** van de portal.

    ![Systeemvak](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Maak een back-upbeleid

Een back-upbeleid bepaalt wanneer back-ups worden gemaakt en hoe lang ze worden bewaard. 

- Een beleid wordt gemaakt op kluisniveau.
- U kunt hetzelfde back-upbeleid gebruiken voor meerdere kluizen, maar u moet het back-upbeleid toepassen op elke kluis.
- Wanneer u een back-upbeleid maakt, is een dagelijkse volledige back-up de standaardinstelling.
- U kunt een differentiële back-up toevoegen, maar alleen als u een wekelijkse volledige back-up configureert.
- [Meer informatie](backup-architecture.md#sql-server-backup-types) over verschillende soorten back-upbeleid.


Ga als volgt te werk om een back-upbeleid te maken:

1. Klik in de kluis op **Back-upbeleid** > **Toevoegen**.
2. Klik in het menu **Toevoegen** op **SQL Server in Azure-VM**. Hiermee definieert u het beleidstype.

   ![Een beleidstype voor het nieuwe back-upbeleid kiezen](./media/backup-azure-sql-database/policy-type-details.png)

3. Geef bij **Beleidsnaam** een naam voor het nieuwe beleid op. 
4. In **Beleid voor een volledige back-up** selecteert u een **back-upfrequentie** en kiest u **Dagelijks** of **Wekelijks**.

    - Als u **Dagelijks** kiest, selecteert u het tijdstip en de tijdzone waarop de back-uptaak moet worden gestart.
    - U moet een volledige back-up maken. U kunt de optie **Volledige back-up** niet uitschakelen.
    - Klik op **Volledige back-up** om het beleid te bekijken. 
    - U kunt geen differentiële back-ups maken voor dagelijkse volledige back-ups.
    - Als u **Wekelijks** kiest, selecteert u de dag van de week, het tijdstip en de tijdzone waarop de back-uptaak moet worden gestart.

    ![Velden voor nieuw back-upbeleid](./media/backup-azure-sql-database/full-backup-policy.png)  

5. Standaard zijn alle opties voor een **bewaartermijn** geselecteerd. Schakel alle ongewenste bewaartermijnlimieten die u niet wilt gebruiken uit en stel de intervallen in die u wilt gebruiken. 

    - Herstelpunten worden getagd voor retentie op basis van de bewaarperiode. Als u een dagelijkse volledige back-up selecteert, wordt slechts één volledige back-up per dag geactiveerd.
    - De back-up voor een specifieke dag wordt getagd en bewaard op basis van de wekelijkse bewaarperiode en uw wekelijkse bewaarbeleid.
    - De maandelijkse en jaarlijkse bewaarperioden werken op soortgelijke wijze.

   ![Intervalinstellingen voor bewaarperiode](./media/backup-azure-sql-database/retention-range-interval.png)

    

6. In het menu voor het **beleid voor een volledige back-up** selecteert u **OK** om de instellingen te accepteren.
7. Als u een beleid voor een differentiële back-up wilt toevoegen, selecteert u **Differentiële back-up**. 

   ![Intervalinstellingen voor bewaarperiode](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Het menu voor het beleid voor een differentiële back-up openen](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. In **Beleid voor een differentiële back-up** selecteert u **Inschakelen** om de frequentie- en bewaarinstellingen te openen. 

    - U kunt maximaal één differentiële back-up per dag activeren.
    - Differentiële back-ups kunnen maximaal 180 dagen worden bewaard. Als dat voor u te kort is, moet u volledige back-ups gebruiken.

9. Selecteer **OK** om het beleid op te slaan en terug te gaan naar het hoofdmenu **Back-upbeleid**.

10. Als u een back-upbeleid voor een transactielogboek wilt toevoegen, selecteert u **Logboekback-up**.
11. In **Logboekback-up** selecteert u **Inschakelen** en stelt u de frequentie- en bewaarinstellingen in. Logboekback-ups kunnen elke 15 minuten worden geactiveerd en maximaal 35 dagen worden bewaard.
12. Selecteer **OK** om het beleid op te slaan en terug te gaan naar het hoofdmenu **Back-upbeleid**.

   ![Het logboekback-upbeleid bewerken](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. Kies in het menu **Back-upbeleid** of u **Compressie van SQL-back-ups** wilt inschakelen.
    - Compressie is standaard uitgeschakeld.
    - Op de back-end maakt Azure Backup gebruik van systeemeigen compressie van SQL-back-ups.

14. Als u klaar bent met het bewerken van het back-upbeleid, selecteert u **OK**.



## <a name="enable-auto-protection"></a>Automatische beveiliging inschakelen  

Schakel automatische beveiliging in om een automatische back-up te maken van alle bestaande databases en van databases die in de toekomst worden toegevoegd aan een zelfstandig exemplaar van SQL Server of een AlwaysOn-beschikbaarheidsgroep van SQL Server. 

- Wanneer u automatische beveiliging inschakelt en een beleid selecteert, blijven de bestaande beveiligde databases gebruikmaken van het vorige beleid.
- Er is geen limiet voor het aantal databases dat u in één keer voor automatische beveiliging kunt selecteren.

Schakel automatische beveiliging als volgt in:

1. In **Items voor back-up** selecteert u het exemplaar waarvoor u automatische beveiliging wilt inschakelen.
2. Selecteer de vervolgkeuzelijst onder **Automatische beveiliging** en stel deze in op **Aan**. Klik vervolgens op **OK**.

    ![Automatische beveiliging inschakelen voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

3. Back-up wordt voor alle databases tegelijk getriggerd en kan worden gevolgd in **Back-uptaken**.


Om de automatische beveiliging uit te schakelen, klikt u op de naam van het exemplaar onder **Back-up configureren** en selecteert u **Automatische beveiliging uitschakelen** voor het exemplaar. Er worden continu back-ups van de databases gemaakt. Toekomstige databases worden echter niet automatisch beveiligd.

![Automatische beveiliging voor dat exemplaar uitschakelen](./media/backup-azure-sql-database/disable-auto-protection.png)


## <a name="fix-sql-sysadmin-permissions"></a>Problemen met systeembeheerdersrechten voor SQL oplossen

Als u machtigingen nodig hebt i.v.m. een **UserErrorSQLNoSysadminMembership**-fout, gaat u als volgt te werk:

1. Meld u bij SQL Server Management Studio (SSMS) aan met een account met systeembeheerdersrechten voor SQL Server. Windows-verificatie zou moeten werken, tenzij u speciale machtigingen nodig hebt.
2. Open de map **Security/Logins** op de SQL-server.

    ![De map Security/Logins openen om accounts te bekijken](./media/backup-azure-sql-database/security-login-list.png)

3. Klik met de rechtermuisknop op de map **Logins** en selecteer **Nieuwe aanmelding**. In **Aanmelding - Nieuw** selecteert u **Zoeken**.

    ![In het dialoogvenster Aanmelding - Nieuw selecteert u Zoeken](./media/backup-azure-sql-database/new-login-search.png)

3. Het virtuele Windows-serviceaccount **NT SERVICE\AzureWLBackupPluginSvc** is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectiefase. Geef de accountnaam op, zoals wordt weergegeven in **Objectnaam invoeren die moet worden geselecteerd**. Selecteer **Namen controleren** om de naam te herleiden. Klik op **OK**.

    ![Namen controleren selecteren om de naam van de onbekende service te herleiden](./media/backup-azure-sql-database/check-name.png)


4. Controleer of in **Serverrollen** de rol **sysadmin** is geselecteerd. Klik op **OK**. De vereiste machtigingen moeten nu bestaan.

    ![Controleren of de serverfunctie sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

5. Koppel de database nu aan de Recovery Services-kluis. Klik in de Azure-portal in de lijst **Beschermde servers** met de rechtermuisknop op de server met een foutstatus > **DB's opnieuw detecteren**.

    ![Controleren of de server de juiste machtigingen heeft](./media/backup-azure-sql-database/check-erroneous-server.png)

6. Controleer de voortgang in het gebied **Meldingen**. Wanneer de geselecteerde databases zijn gevonden, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

U kunt ook [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection) inschakelen voor het hele exemplaar of de AlwaysOn-beschikbaarheidsgroep door de optie **ON** (AAN) te selecteren in de bijbehorende vervolgkeuzelijst in de kolom **AUTOPROTECT** (AUTOMATISCHE BEVEILIGING). Met de functie voor [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection) kunt u niet alleen alle bestaande databases in één keer beveiligen, maar ook automatisch nieuwe databases beveiligen die in de toekomst aan dat exemplaar of de beschikbaarheidsgroep worden toegevoegd.  

   ![Automatische beveiliging inschakelen voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](restore-sql-database-azure-vm.md) over het herstellen van SQL Server-databases vanuit back-up.
- [Meer informatie](manage-monitor-sql-database-backup.md) over het beheren van SQL Server-databases vanuit back-up.

