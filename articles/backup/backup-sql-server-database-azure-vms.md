---
title: Een back-up van SQL Server-databases maken in Azure | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u een back-up van SQL Server maakt in Azure.
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: sachdevaswati
ms.openlocfilehash: 5fe4161c688570cc3adaacc79a0dd1fe38460142
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2019
ms.locfileid: "58118898"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Back-up van SQL Server-databases in virtuele Azure-machines

SQL Server-databases zijn essentiële workloads waarvoor een laag Recovery Point Objective (RPO, beoogd herstelpunt) en langetermijnretentie nodig zijn. Met behulp van [Azure Backup](backup-overview.md) kunt u back-ups maken van SQL Server-databases die worden uitgevoerd op virtuele Azure-VM's.

In dit artikel wordt uitgelegd hoe u een back-up naar een Azure Backup Recovery Services-kluis maakt van een SQL Server-database die op een Azure-VM wordt uitgevoerd. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een kluis maken en configureren.
> * Databases detecteren en back-ups instellen.
> * Automatische beveiliging van databases instellen.


## <a name="prerequisites"></a>Vereisten

Voordat u een back-up van uw SQL Server-database maakt, controleert u de volgende voorwaarden:

1. Bepalen of [maken](backup-azure-sql-database.md#create-a-recovery-services-vault) een Recovery Services-kluis in dezelfde regio of land als de virtuele machine die als host fungeert voor de SQL Server-exemplaar.
2. [Controleer de VM-machtigingen](#fix-sql-sysadmin-permissions) die nodig zijn voor de back-up van de SQL-databases.
3. Controleer of de virtuele machine [netwerkverbinding](backup-azure-sql-database.md#establish-network-connectivity) heeft.
4. Controleer of de SQL Server-databases zijn benoemd in overeenstemming met de [naamgevingsrichtlijnen](#verify-database-naming-guidelines-for-azure-backup) van Azure Backup.
5. Controleer of er geen andere back-upoplossingen zijn ingeschakeld voor de database. Schakel alle andere SQL Server-back-ups uit voordat u dit scenario instelt. U kunt zonder problemen Azure Backup inschakelen voor een Azure-VM en tegelijkertijd voor een SQL Server-database die wordt uitgevoerd op de virtuele machine.


### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

Voor alle bewerkingen is voor de SQL Server-VM een verbinding met openbare IP-adressen van Azure nodig. VM-bewerkingen (database-detectie, back-ups configureren, back-ups, herstelpunten herstellen enzovoort) uitvallen zonder dat de verbinding met het openbare IP-adressen. Maak verbinding met een van deze opties:

- **Sta de IP-bereiken voor Azure Datacenter toe**: Sta de [IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653) in de download toe. Voor toegang tot de netwerkbeveiligingsgroep (NSG), gebruikt de **Set AzureNetworkSecurityRule** cmdlet.
- **Implementeer een HTTP-proxyserver om verkeer te routeren**: Wanneer u een back-up van een SQL Server-database op een Azure-VM maakt, gebruikt de back-upextensie op de virtuele machine de HTTPS-API's voor het verzenden van beheeropdrachten naar Azure Backup en gegevens naar Azure Storage. De back-upextensie maakt ook gebruik van Azure Active Directory (Azure AD) voor verificatie. Leid het verkeer van de back-upextensie voor deze drie services via de HTTP-proxy. Van de extensie zijn het enige onderdeel dat geconfigureerd voor toegang tot het openbare internet.

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

   - Niet-beveiligde virtuele machines zouden na detectie in de lijst moeten verschijnen, gesorteerd op naam en resourcegroep.
   - Als een virtuele machine niet zoals verwacht op de lijst verschijnt, controleert u of hiervan al een back-up bestaat in een kluis.
   - Meerdere virtuele machines kunnen dezelfde naam hebben, maar behoren altijd tot verschillende resourcegroepen.

     ![Back-up in behandeling tijdens het zoeken naar databases in VM's](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Selecteer in de lijst met virtuele machines de VM waarop de SQL Server-database wordt uitgevoerd > **DB's detecteren**.

7. Database-detectie bijhouden in het gebied **Meldingen**. Het kan even duren voor de taak is voltooid, afhankelijk van hoeveel databases zich op de virtuele machine bevinden. Wanneer de geselecteerde databases zijn gedetecteerd, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Backup detecteert alle SQL Server-databases op de virtuele machine. Tijdens de detectie gebeurt het volgende op de achtergrond:

    - Azure Backup registreert de virtuele machine met de kluis voor een back-up van de werkbelasting. Van alle databases op de geregistreerde virtuele machine kan alleen op deze kluis een back-up worden gemaakt.
    - Azure Backup installeert de extensie **AzureBackupWindowsWorkload** op de virtuele machine. Er is geen agent geïnstalleerd op de SQL-database.
    - Azure Backup maakt het serviceaccount **NT Service\AzureWLBackupPluginSvc** op de virtuele machine.
      - Het serviceaccount wordt gebruikt voor alle back-up- en herstelbewerkingen.
      - Voor **NT-Service\AzureWLBackupPluginSvc** zijn systeembeheerdersrechten voor SQL vereist. Op alle SQL Server-VM's die in de Azure Marketplace zijn gemaakt, is **SqlIaaSExtension** geïnstalleerd. De extensie **AzureBackupWindowsWorkload** maakt gebruik van de extensie **SQLIaaSExtension** om automatische de benodigde machtigingen op te halen.
    - Als u de virtuele machine niet vanuit de marketplace hebt gemaakt, beschikt de virtuele machine niet over **SqlIaaSExtension** en niet over de detectiebewerkingsfouten met het foutbericht **UserErrorSQLNoSysAdminMembership**. Volg de instructies in [#fix-sql-sysadmin-permissions] om dit probleem op te lossen.

        ![De VM en database selecteren](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Back-up configureren  

Configureer de back-up als volgt:

1. In **Doel van de back-up** selecteert u **Back-up configureren**.

   ![Back-up configureren selecteren](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. Klik op **back-up configureren**, wordt de **items selecteren om back-up te** blade wordt weergegeven. Dit geeft een lijst van alle geregistreerde beschikbaarheidsgroepen en zelfstandige SQL-Servers. Vouw de pijl-omlaag links van de rij om te zien van alle niet-beveiligde databases in dat exemplaar of altijd op AG.  

    ![Alle SQL Server-exemplaren met zelfstandige databases weergeven](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Selecteer alle databases die u wilt beveiligen > **OK**.

   ![De database beveiligen](./media/backup-azure-sql-database/select-database-to-protect.png)

   Ter optimalisering van de back-upbelastingen stelt Azure Backup het maximumaantal databases in één back-uptaak in op 50.

     * Om meer dan 50 back-ups te beschermen moet u meerdere databases configureren.
     * U kunt ook [automatische beveiliging](#enable-auto-protection) inschakelen voor het hele exemplaar of de AlwaysOn-beschikbaarheidsgroep door de optie **ON** (AAN) te selecteren in de bijbehorende vervolgkeuzelijst in de kolom **AUTOPROTECT** (AUTOMATISCHE BEVEILIGING). Met de functie voor [automatische beveiliging](#enable-auto-protection) kunt u niet alleen alle bestaande databases in één keer beveiligen, maar ook automatisch nieuwe databases beveiligen die in de toekomst aan dat exemplaar of de beschikbaarheidsgroep worden toegevoegd.  

4. Klik op **OK** openen de **back-upbeleid** blade.

    ![Automatische beveiliging voor dat exemplaar uitschakelen](./media/backup-azure-sql-database/disable-auto-protection.png)

5. In **back-upbeleid kiezen**, selecteert u een beleid en klik vervolgens op **OK**.

   - Het standaardbeleid selecteren: HourlyLogBackup.
   - Een bestaand back-upbeleid kiezen dat u eerder hebt gemaakt voor SQL.
   - [Een nieuw beleid definiëren](#configure-a-backup-policy) op basis van uw RPO en retentiebereik.

     ![Back-upbeleid selecteren](./media/backup-azure-sql-database/select-backup-policy.png)

6. Op **back-up** in het menu **back-up inschakelen**.

    ![Het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

7. Bijhouden van de voortgang van de configuratie in de **meldingen** gebied van de portal.

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

    - Minimale periode voor gegevensbehoud voor elk type back-up (volledige/differentiële/logboek) is 7 dagen.
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


### <a name="modify-policy"></a>Beleid wijzigen
Beleid voor het wijzigen van de back-upfrequentie of retentie bereik wijzigen.

> [!NOTE]
> Eventuele wijzigingen in de bewaarperiode wordt retroactief worden toegepast op alle oudere herstelpunten naast de nieuwe licenties.

In het dashboard van de kluis, gaat u naar **beheren** > **back-upbeleid** en kies het beleid dat u wilt bewerken.

  ![Back-upbeleid beheren](./media/backup-azure-sql-database/modify-backup-policy.png)


## <a name="enable-auto-protection"></a>Automatische beveiliging inschakelen  

Schakel automatische beveiliging in om een automatische back-up te maken van alle bestaande databases en van databases die in de toekomst worden toegevoegd aan een zelfstandig exemplaar van SQL Server of een AlwaysOn-beschikbaarheidsgroep van SQL Server.

  - Wanneer u automatische beveiliging inschakelt en een beleid selecteert, blijven de bestaande beveiligde databases gebruikmaken van het vorige beleid.
  - Er is geen limiet voor het aantal databases die u voor automatische beveiliging in één go selecteren kunt.

Schakel automatische beveiliging als volgt in:

  1. In **Items voor back-up** selecteert u het exemplaar waarvoor u automatische beveiliging wilt inschakelen.
  2. Selecteer de vervolgkeuzelijst onder **Automatische beveiliging** en stel deze in op **Aan**. Klik vervolgens op **OK**.

      ![Automatische beveiliging inschakelen voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. Back-up wordt voor alle databases tegelijk getriggerd en kan worden gevolgd in **Back-uptaken**.

Als u automatische beveiliging uitschakelen, klikt u op de naam van het exemplaar onder **back-up configureren**, en selecteer **uitschakelen Beveilig automatisch** voor het exemplaar. Er worden continu back-ups van de databases gemaakt. Toekomstige databases worden echter niet automatisch beveiligd.


## <a name="fix-sql-sysadmin-permissions"></a>Problemen met systeembeheerdersrechten voor SQL oplossen

  Als u machtigingen nodig hebt i.v.m. een **UserErrorSQLNoSysadminMembership**-fout, gaat u als volgt te werk:

  1. Meld u bij SQL Server Management Studio (SSMS) aan met een account met systeembeheerdersrechten voor SQL Server. Windows-verificatie zou moeten werken, tenzij u speciale machtigingen nodig hebt.
  2. Open de map **Security/Logins** op de SQL-server.

      ![De map Security/Logins openen om accounts te bekijken](./media/backup-azure-sql-database/security-login-list.png)

  3. Klik met de rechtermuisknop op de map **Logins** en selecteer **Nieuwe aanmelding**. In **Aanmelding - Nieuw** selecteert u **Zoeken**.

      ![In het dialoogvenster Aanmelding - Nieuw selecteert u Zoeken](./media/backup-azure-sql-database/new-login-search.png)

  4. Het virtuele Windows-serviceaccount **NT SERVICE\AzureWLBackupPluginSvc** is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectiefase. Geef de accountnaam op, zoals wordt weergegeven in **Objectnaam invoeren die moet worden geselecteerd**. Selecteer **Namen controleren** om de naam te herleiden. Klik op **OK**.

      ![Namen controleren selecteren om de naam van de onbekende service te herleiden](./media/backup-azure-sql-database/check-name.png)

  5. Controleer of in **Serverrollen** de rol **sysadmin** is geselecteerd. Klik op **OK**. De vereiste machtigingen moeten nu bestaan.

      ![Controleren of de serverfunctie sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

  6. Koppel de database nu aan de Recovery Services-kluis. Klik in de Azure-portal in de lijst **Beschermde servers** met de rechtermuisknop op de server met een foutstatus > **DB's opnieuw detecteren**.

      ![Controleren of de server de juiste machtigingen heeft](./media/backup-azure-sql-database/check-erroneous-server.png)

  7. Controleer de voortgang in het gebied **Meldingen**. Wanneer de geselecteerde databases zijn gevonden, wordt er een slagingsbericht weergegeven.

      ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

 
## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](restore-sql-database-azure-vm.md) over het herstellen van SQL Server-databases vanuit back-up.
- [Meer informatie](manage-monitor-sql-database-backup.md) over het beheren van SQL Server-databases vanuit back-up.
