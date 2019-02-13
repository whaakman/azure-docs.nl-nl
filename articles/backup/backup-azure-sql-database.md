---
title: Een back-up van SQL Server-databases maken in Azure | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u een back-up van SQL Server maakt in Azure. In het artikel wordt ook uitgelegd hoe u SQL Server kunt herstellen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: raynew
ms.openlocfilehash: fa154b79625fffb8174c510156b3a67df8bff785
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55770426"
---
# <a name="back-up-sql-server-databases-to-azure"></a>Een back-up van SQL Server-databases maken in Azure

SQL Server-databases zijn essentiële workloads waarvoor een laag Recovery Point Objective (RPO, beoogd herstelpunt) en langetermijnretentie nodig zijn. Azure Backup biedt een SQL Server-back-upoplossing die geen infrastructuur vereist: zo hoeft u geen complexe back-upserver, geen beheeragent en geen back-upopslag te beheren. Azure Backup voorziet in centraal beheer voor uw back-ups voor alle servers waarop SQL Server wordt uitgevoerd, of zelfs in verschillende workloads.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De vereisten voor het maken van een back-up van een exemplaar van SQL Server in Azure.
> * Een Recovery Services-kluis maken en gebruiken.
> * Back-ups van SQL Server-databases configureren.
> * Een back-upbeleid (of retentiebeleid) voor de herstelpunten instellen.
> * De database herstellen.

Voordat u de procedures in dit artikel uitvoert, moet u beschikken over een actief exemplaar van SQL Server in Azure. [Snel een SQL Server-exemplaar maken met SQL Marketplace-VM's](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Beperkingen van openbare preview

De volgende items zijn bekende beperkingen voor de openbare preview-versie:

- Voor de SQL-VM (virtuele machine) is een internetverbinding vereist voor toegang tot openbare IP-adressen van Azure. Zie voor meer informatie [Netwerkverbinding tot stand brengen](backup-azure-sql-database.md#establish-network-connectivity).
- U kunt maximaal 2000 SQL-databases veilig opslaan in één Recovery Services-kluis. Aanvullende SQL-databases moeten worden opgeslagen in een afzonderlijke Recovery Services-kluis.
- [Back-ups van gedistribueerde beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) hebben de volgende beperkingen.
- SQL Server AlwaysOn Failover Cluster Instances (FCI's) worden niet ondersteund voor back-ups.
- Gebruik Azure Portal om Azure Backup te configureren voor het beveiligen van SQL Server-databases. Azure PowerShell, Azure CLI en de REST API's worden momenteel niet ondersteund.
- Back-up- en herstelbewerkingen voor gespiegelde FCI-databases, databasemomentopnamen en databases worden niet ondersteund.
- Databases met zeer veel bestanden kunnen niet worden beveiligd. Het maximumaantal bestanden kan variëren, omdat dit niet alleen afhankelijk is van de hoeveelheid bestanden maar ook van de lengte van de bestandspaden. Dergelijke problemen komen echter niet zo vaak voor. We werken aan een oplossing hiervoor.

Raadpleeg het gedeelte [Veelgestelde vragen](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#faq) voor meer informatie over ondersteunde/niet-ondersteunde scenario's.

## <a name="support-for-azure-geos"></a>Ondersteuning voor geografische gebieden voor Azure

Azure Backup wordt ondersteund voor de volgende geografische gebieden:

- Australië - zuidoost (ASE)
- Brazilië - zuid (BRS)
- Canada centraal (CNC)
- Canada - oost (CE)
- US - centraal (CUS)
- Azië - oost (EA)
- Oost-Australië (AE)
- US - oost (EUS)
- US - oost 2 (EUS2)
- India - centraal (INC)
- India - zuid (INS)
- Japan - oost (JPE)
- Japan - west (JPW)
- Korea - centraal (KRC)
- Korea - zuid (KRS)
- US - noord-centraal (NCUS)
- Europa - noord (NE)
- US - zuid-centraal (SCUS)
- Azië - zuidoost (SEA)
- Verenigd Koninkrijk zuid (UKS)
- Verenigd Koninkrijk west (UKW)
- US - west-centraal (WCUS)
- Europa - west (WE)
- US - west (WUS)
- US - west 2 (WUS 2)

## <a name="support-for-operating-systems-and-sql-server-versions"></a>Ondersteuning voor besturingssystemen en versies van SQL Server

In dit gedeelte wordt de ondersteuning voor Azure Backup voor besturingssystemen en versies van SQL Server beschreven. SQL Azure Marketplace-VM's en niet-Marketplace-VM's (waarop SQL Server handmatig is geïnstalleerd) worden ondersteund.

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux wordt momenteel niet ondersteund.

### <a name="supported-sql-server-versions-and-editions"></a>Ondersteunde versies en edities van SQL Server

- SQL Server 2012 Enterprise, Standard, Web, Developer, Express
- SQL Server 2014 Enterprise, Standard, Web, Developer, Express
- SQL Server 2016 Enterprise, Standard, Web, Developer, Express
- SQL Server 2017 Enterprise, Standard, Web, Developer, Express

## <a name="prerequisites"></a>Vereisten

Voordat u een back-up van uw SQL Server-database maakt, controleert u de volgende voorwaarden:

- Identificeer of [maak een Recovery Services-kluis](backup-azure-sql-database.md#create-a-recovery-services-vault) in hetzelfde gebied of land als de virtuele machine die fungeert als host voor uw exemplaar van SQL Server.
- [Controleer de machtigingen voor de virtuele machine](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) die nodig zijn voor het maken van een back-up van de SQL-databases.
- Controleer of de [SQL-VM een netwerkverbinding heeft](backup-azure-sql-database.md#establish-network-connectivity).
- Controleer of de naamgeving van de SQL-databases voldoet aan de [richtlijnen voor namen](backup-azure-sql-database.md#sql-database-naming-guidelines-for-azure-backup) voor Azure Backup zodat er succesvol back-ups kunnen worden gemaakt.

> [!NOTE]
> U kunt slechts één back-upoplossing tegelijk hebben voor het maken van back-ups van SQL Server-databases. Schakel alle andere SQL-back-ups uit voordat u deze functie gebruikt, anders zullen de back-ups elkaar verstoren en mislukken. U kunt wel Azure Backup voor een IaaS-VM naast een SQL-back-up inschakelen zonder dat er een conflict optreedt.
>

Als dergelijke gevallen van toepassing zijn op uw omgeving, gaat u naar [Back-up configureren voor SQL Server-databases](backup-azure-sql-database.md#configure-backup-for-sql-server-databases). Als geen van de genoemde gevallen van toepassing is, kunt u verder lezen.


## <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

Voor alle bewerkingen is voor de SQL-VM een verbinding met openbare IP-adressen van Azure nodig. SQL-VM-bewerkingen (zoals databasedetectie, back-ups configureren, back-ups plannen, herstelpunten herstellen, enzovoort) zullen mislukken zonder verbinding met de openbare IP-adressen. Gebruik een van de volgende opties om een pad voor back-upverkeer op te geven:

- De IP-bereiken van het Azure-datacentrum toevoegen aan de lijst met toegestane IP-adressen: Als u de IP-bereiken van het Azure-datacentrum wilt toevoegen aan de lijst met toegestane IP-adressen, gebruikt u de [pagina Downloadcentrum voor meer informatie over de IP-bereiken en instructies](https://www.microsoft.com/download/details.aspx?id=41653).
- Een HTTP-proxyserver implementeren om verkeer te routeren: Wanneer u een back-up van een SQL-database op een VM maakt, gebruikt de back-upextensie op de virtuele machine de HTTPS-API's voor het verzenden van beheeropdrachten naar Azure Backup en gegevens naar Azure Storage. De back-upextensie maakt ook gebruik van Azure Active Directory (Azure AD) voor verificatie. Leid het verkeer van de back-upextensie voor deze drie services via de HTTP-proxy. De extensie is het enige onderdeel dat wordt geconfigureerd voor toegang tot het openbare internet.

Bij het kiezen van de opties moet u compromissen sluiten tussen beheerbaarheid, gedetailleerde controle en kosten.

> [!NOTE]
> Servicetags voor Azure Backup moeten algemeen beschikbaar zijn.
>

| Optie | Voordelen | Nadelen |
| ------ | ---------- | ------------- |
| IP-bereiken toevoegen aan whitelist | Geen extra kosten. <br/> Voor toegangsrechten voor openen in een NSG gebruikt u de cmdlet **Set-AzureNetworkSecurityRule**. | Ingewikkeld om te beheren, omdat de betrokken IP-adresbereiken na verloop van tijd veranderen. <br/>Biedt toegang tot heel Azure, niet alleen tot Azure Storage.|
| Een HTTP-proxy gebruiken   | Gedetailleerde controle via de proxy over de opslag-URL's is toegestaan. <br/>Eén internettoegangspunt voor VM's. <br/> Niet onderhevig aan Azure-IP-adreswijzigingen. | Extra kosten voor het uitvoeren van een VM met de proxysoftware. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Machtigingen instellen voor niet-Marketplace-SQL-VM's

Als u een back-up van een virtuele machine wilt maken, moet in Azure Backup de extensie **AzureBackupWindowsWorkload** worden geïnstalleerd. Als u Azure Marketplace-VM's gebruikt, gaat u door met [SQL Server-databases detecteren](backup-azure-sql-database.md#discover-sql-server-databases). Als de virtuele machine die als host fungeert voor uw SQL-databases niet vanuit Azure Marketplace wordt gemaakt, voert u de volgende procedure uit om de extensie te installeren en de juiste machtigingen in te stellen. Naast de extensie **AzureBackupWindowsWorkload** zijn in Azure Backup systeembeheerdersrechten voor SQL nodig voor het beveiligen van SQL-databases. Om databases op de virtuele machine te detecteren, wordt door Azure Backup het account **NT SERVICE\AzureWLBackupPluginSvc** gemaakt. Dit account wordt gebruikt voor back-up en herstel en vereist systeembeheerdersrechten voor SQL. Het account **NT AUTHORITY\SYSTEM** wordt door Azure Backup ook gebruikt voor databasedetectie/-aanvragen, dus dit account moet een openbare aanmelding via SQL zijn.

Ga als volgt te werk om machtigingen te configureren:

1. In [Azure Portal](https://portal.azure.com) opent u de Recovery Services-kluis die u gebruikt voor het beveiligen van de SQL-databases.

2. In het dashboard van de **Recovery Services-kluis** selecteert u **Backup**. Het menu **Doel van de back-up** wordt geopend.

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Stel in het menu **Doel van de back-up** de optie **Waar wordt uw werkbelasting uitgevoerd?** in op de standaardwaarde: **Azure**.

4. Vouw de vervolgkeuzelijst **Waarvan wilt u een back-up maken?** uit en selecteer **SQL Server in Azure VM**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    In het menu **Doel van de back-up** worden twee stappen weergegeven: **DB's detecteren in VM's** en **Back-up configureren**. Met de stap **DB's detecteren in VM's** start u een zoekopdracht naar Azure-VM's.

    ![De twee stappen van Doel van de back-up bekijken](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Selecteer onder **DB's detecteren in VM's** **Detectie starten** om te zoeken naar niet-beveiligde virtuele machines in het abonnement. Het kan even duren voordat alle virtuele machines zijn gevonden. De zoektijd is afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement.

    ![Back-up in behandeling tijdens het zoeken naar databases in VM's](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. In de lijst met virtuele machines selecteert u de VM met de SQL-database waarvan u een back-up wilt maken. Vervolgens selecteert u **DB's detecteren**.

    Het detectieproces installeert de extensie **AzureBackupWindowsWorkload** op de virtuele machine. De extensie staat communicatie tussen de Azure Backup-service en de virtuele machine toe, zodat er back-ups van de SQL-databases kunnen worden gemaakt. Nadat de extensie is geïnstalleerd, maakt Azure Backup het virtuele Windows-serviceaccount **NT Service\AzureWLBackupPluginSvc** op de virtuele machine. Voor het virtuele serviceaccount zijn systeembeheerdersrechten voor SQL vereist. Als tijdens het installatieproces van het virtuele serviceaccount de foutmelding `UserErrorSQLNoSysadminMembership` wordt weergegeven, raadpleegt u [Problemen met systeembeheerdersrechten voor SQL oplossen](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

    In het **Systeemvak** wordt de voortgang van de databasedetectie weergegeven. Het kan enige tijd duren voordat de taak is voltooid. De benodigde tijd is afhankelijk van het aantal databases op de virtuele machine. Wanneer de geselecteerde databases zijn gedetecteerd, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nadat u de database aan de Recovery Services-kluis hebt gekoppeld, kunt u [de back-uptaak configureren](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="fix-sql-sysadmin-permissions"></a>Problemen met systeembeheerdersrechten voor SQL oplossen

Als tijdens het installatieproces de foutmelding `UserErrorSQLNoSysadminMembership` wordt weergegeven, moet u zich bij SQL Server Management Studio (SSMS) aanmelden met een account met systeembeheerdersrechten voor SQL Server. Windows-verificatie zou moeten werken, tenzij u speciale machtigingen nodig hebt.

1. Open de map Security/Logins op de SQL-server.

    ![De map Security/Logins openen om accounts te bekijken](./media/backup-azure-sql-database/security-login-list.png)

2. Klik met de rechtermuisknop op de map Logins en selecteer **Nieuwe aanmelding**. In het dialoogvenster **Aanmelding - Nieuw** selecteert u **Zoeken**.

    ![In het dialoogvenster Aanmelding - Nieuw selecteert u Zoeken](./media/backup-azure-sql-database/new-login-search.png)

3. Het virtuele Windows-serviceaccount **NT SERVICE\AzureWLBackupPluginSvc** is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectiefase. Geef de accountnaam op, zoals wordt weergegeven in het vak **Objectnaam invoeren die moet worden geselecteerd**. Selecteer **Namen controleren** om de naam te herleiden.

    ![Namen controleren selecteren om de naam van de onbekende service te herleiden](./media/backup-azure-sql-database/check-name.png)

4. Selecteer **OK** om het dialoogvenster te sluiten.

5. Controleer in het vak **Serverfuncties** of de rol van systeembeheerder (**sysadmin**) is geselecteerd. Selecteer **OK** om het dialoogvenster te sluiten.

    ![Controleren of de serverfunctie sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

    De vereiste machtigingen moeten nu bestaan.

6. U hebt het probleem met de machtigingen opgelost. Nu moet u de database koppelen aan de Recovery Services-kluis. Klik in Azure Portal in de lijst **beschermde Servers** met de rechtermuisknop op de server met een foutstatus en selecteer **DB's opnieuw detecteren**.

    ![Controleren of de server de juiste machtigingen heeft](./media/backup-azure-sql-database/check-erroneous-server.png)

    In het **Systeemvak** wordt de voortgang van de databasedetectie weergegeven. Het kan enige tijd duren voordat de taak is voltooid. De benodigde tijd is afhankelijk van het aantal databases op de virtuele machine. Wanneer de geselecteerde databases zijn gevonden, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nadat u de database aan de Recovery Services-kluis hebt gekoppeld, kunt u [de back-uptaak configureren](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

## <a name="sql-database-naming-guidelines-for-azure-backup"></a>Richtlijnen voor namen van SQL-databases voor Azure Backup
Om goede back-ups te kunnen maken met Azure Backup voor SQL Server op IaaS-VM, mogen de volgende tekens niet voorkomen in de namen van de databases:

  * Voorloop- en volgspaties
  * Volguitroepteken '!'
  * Vierkante haak sluiten ‘]’

We hebben wel aliasing voor niet-ondersteunde tekens in Azure-tabellen, maar ook dat kunt u het beste vermijden. Raadpleeg [dit artikel](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN) voor meer informatie.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server-databases detecteren

Met Azure Backup worden alle databases op een SQL Server-exemplaar gedetecteerd. U kunt de databases beveiligen op basis van uw back-upvereisten. Gebruik de volgende procedure voor het identificeren van de virtuele machine die fungeert als host voor de SQL-databases. Nadat u de virtuele machine hebt geïdentificeerd, wordt een lichtgewicht extensie voor het detecteren van de SQL Server-databases in Azure Backup geïnstalleerd.

1. Meld u aan bij uw abonnement in [Azure Portal](https://portal.azure.com/).

2. Selecteer in het menu links **Alle services**.

    ![Alle services selecteren](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. In het dialoogvenster **Alle services** voert u **Recovery Services** in. Terwijl u typt, wordt de lijst met resources gefilterd op basis van uw invoer. Selecteer **Recovery Services-kluizen** in de lijst.

  ![Recovery Services-kluizen invoeren en kiezen](./media/backup-azure-sql-database/all-services.png) <br/>

    De lijst met Recovery Services-kluizen in het abonnement wordt weergeven.

4. In de lijst met Recovery Services-kluizen selecteert u de kluis waarmee u uw SQL-databases wilt beveiligen.

5. In het dashboard van de **Recovery Services-kluis** selecteert u **Backup**. Het menu **Doel van de back-up** wordt geopend.

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

6. Stel in het menu **Doel van de back-up** de optie **Waar wordt uw werkbelasting uitgevoerd?** in op de standaardwaarde: **Azure**.

7. Vouw de vervolgkeuzelijst **Waarvan wilt u een back-up maken?** uit en selecteer **SQL Server in Azure VM**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    In het menu **Doel van de back-up** worden twee stappen weergegeven: **DB's detecteren in VM's** en **Back-up configureren**.

    ![De twee stappen van Doel van de back-up bekijken](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Selecteer onder **DB's detecteren in VM's** **Detectie starten** om te zoeken naar niet-beveiligde virtuele machines in het abonnement. Het kan even duren voordat alle virtuele machines zijn opgezocht. De zoektijd is afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement.

    ![Back-up in behandeling tijdens het zoeken naar databases in VM's](./media/backup-azure-sql-database/discovering-sql-databases.png)

    Als een niet-beveiligde virtuele machine wordt gedetecteerd, wordt deze weergegeven in de lijst. Er kunnen meerdere virtuele machines met dezelfde naam voorkomen. Virtuele machines met dezelfde naam behoren tot verschillende resourcegroepen. Niet-beveiligde virtuele machines worden weergegeven op naam en resourcegroep. Als een verwachte virtuele machine niet wordt weergegeven, controleert u of die virtuele machine al wordt beveiligd door een kluis.

9. In de lijst met virtuele machines selecteert u de VM met de SQL-database waarvan u een back-up wilt maken. Vervolgens selecteert u **DB's detecteren**.

    In Azure Backup worden alle SQL-databases op de virtuele machine gedetecteerd. Zie [Bewerkingen op de achtergrond](backup-azure-sql-database.md#background-operations) voor meer informatie over de processen die tijdens de databasedetectiefase worden uitgevoerd. Nadat de SQL-databases zijn gedetecteerd, kunt u [de back-uptaak configureren](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="background-operations"></a>Bewerkingen op de achtergrond

Wanneer u het hulpprogramma **DB's detecteren** gebruikt, worden in Azure Backup de volgende bewerkingen uitgevoerd op de achtergrond:

- De virtuele machine registreren bij de Recovery Services-kluis voor workloadback-ups. Van alle databases op de geregistreerde virtuele machine kunnen alleen back-ups worden gemaakt in deze Recovery Services-kluis.

- De extensie **AzureBackupWindowsWorkload** installeren op de virtuele machine. De oplossing voor het maken van back-ups van SQL-databases is een oplossing zonder agent. De extensie wordt geïnstalleerd op de virtuele machine. Er wordt geen agent geïnstalleerd op de SQL-database.

- Het serviceaccount **NT Service\AzureWLBackupPluginSvc** maken op de virtuele machine. Het serviceaccount wordt gebruikt voor alle back-up- en herstelbewerkingen. Voor **NT-Service\AzureWLBackupPluginSvc** zijn systeembeheerdersrechten voor SQL vereist. Op elke SQL Marketplace-VM die wordt geleverd, is de extensie **SqlIaaSExtension** geïnstalleerd. De extensie **AzureBackupWindowsWorkload** maakt gebruik van de extensie **SQLIaaSExtension** om automatische de benodigde machtigingen op te halen. Als **SqlIaaSExtension** niet op uw virtuele machine is geïnstalleerd, mislukt het detecteren van databases met de foutmelding `UserErrorSQLNoSysAdminMembership`. Als u de systeembeheerdersrechten voor back-ups wilt toevoegen, volgt u de instructies voor het [instellen van machtigingen voor Azure Backup voor niet-Marketplace SQL-VM's](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

    ![De VM en database selecteren](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-databases"></a>Back-up voor SQL Server-databases configureren

Azure Backup biedt beheerservices voor het beveiligen van uw SQL Server-databases en het beheren van back-uptaken. De beheer- en bewakingsfuncties kunnen per Recovery Services-kluis verschillen.

> [!NOTE]
> U kunt slechts één back-upoplossing tegelijk hebben voor het maken van back-ups van SQL Server-databases. Schakel alle andere SQL-back-ups uit voordat u deze functie gebruikt, anders zullen de back-ups elkaar verstoren en mislukken. U kunt wel Azure Backup voor een IaaS-VM naast een SQL-back-up inschakelen zonder dat er een conflict optreedt.
>

Ga als volgt te werk om de beveiliging voor een SQL-database te configureren:

1. Open de Recovery Services-kluis die is geregistreerd bij de virtuele machine van SQL.

2. In het dashboard van de **Recovery Services-kluis** selecteert u **Backup**. Het menu **Doel van de back-up** wordt geopend.

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Stel in het menu **Doel van de back-up** de optie **Waar wordt uw werkbelasting uitgevoerd?** in op de standaardwaarde: **Azure**.

4. Vouw de vervolgkeuzelijst **Waarvan wilt u een back-up maken?** uit en selecteer **SQL Server in Azure VM**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    In het menu **Doel van de back-up** worden twee stappen weergegeven: **DB's detecteren in VM's** en **Back-up configureren**.

    Als u de stappen in dit artikel op volgorde hebt voltooid, hebt u de niet-beveiligde virtuele machines gevonden en is deze kluis geregistreerd bij een virtuele machine. U bent nu klaar om de beveiliging voor de SQL-databases te configureren.

5. In het menu **Doel van de back-up** selecteert u **Back-up configureren**.

    ![Back-up configureren selecteren](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    De Azure Backup-service wordt weergegeven voor alle SQL Server-exemplaren met zelfstandige databases en AlwaysOn-beschikbaarheidsgroepen van SQL Server. Als u de zelfstandige databases op het SQL Server-exemplaar wilt weergeven, selecteert u de pijl-omlaag links van de naam van het exemplaar. Op dezelfde manier selecteert u de pijl-omlaag links van de AlwaysOn-beschikbaarheidsgroep om de lijst met databases weer te geven. In de volgende afbeelding ziet u een voorbeeld van een zelfstandig exemplaar en een AlwaysOn-beschikbaarheidsgroep.

      ![Alle SQL Server-exemplaren met zelfstandige databases weergeven](./media/backup-azure-sql-database/list-of-sql-databases.png)

6. Selecteer alle databases die u wilt beveiligen in de lijst en klik op **OK**.

    ![De database beveiligen](./media/backup-azure-sql-database/select-database-to-protect.png)

    U kunt maximaal 50 databases tegelijk selecteren. Als u meer dan 50 databases wilt beveiligen, moet u deze stap meermaals uitvoeren. Nadat u de eerste 50 databases hebt beveiligd, herhaalt u deze stap om de volgende set databases te beveiligen.

    > [!Note]
    > Voor het optimaliseren van back-upbelastingen worden grote back-uptaken door Azure Backup gesplitst in meerdere batches. Het maximum aantal databases in één back-uptaak is 50.
    >

      U kunt ook [automatische beveiliging](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) inschakelen voor het hele exemplaar of de AlwaysOn-beschikbaarheidsgroep door de optie **ON** (AAN) te selecteren in de bijbehorende vervolgkeuzelijst in de kolom **AUTOPROTECT** (AUTOMATISCHE BEVEILIGING). Met de functie voor [automatische beveiliging](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) kunt u niet alleen alle bestaande databases in één keer beveiligen, maar ook automatisch nieuwe databases beveiligen die in de toekomst aan dat exemplaar of de beschikbaarheidsgroep worden toegevoegd.  

      ![Automatische beveiliging inschakelen voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

7. Als u een back-upbeleid wilt maken of kiezen, opent u het menu **Back-up** en selecteert u **Back-upbeleid**. Het menu **Back-upbeleid** wordt geopend.

    ![Back-upbeleid selecteren](./media/backup-azure-sql-database/select-backup-policy.png)

8. Kies een back-upbeleid in de vervolgkeuzelijst **Back-upbeleid kiezen** en selecteer **OK**. Zie [Een back-upbeleid definiëren](backup-azure-sql-database.md#define-a-backup-policy) voor meer informatie over het maken van een back-upbeleid.

   > [!NOTE]
   > In de Preview-versie kunt u back-upbeleid niet bewerken. Als u een ander beleid wilt dan wat er beschikbaar is in de lijst, moet u dat beleid maken. Zie het gedeelte [Een back-upbeleid definiëren](backup-azure-sql-database.md#define-a-backup-policy)voor informatie over het maken van een nieuw back-upbeleid.

    ![Een back-upbeleid kiezen in de lijst](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    In het menu **Back-upbeleid**, in de vervolgkeuzelijst **Back-upbeleid kiezen**, kunt u:
    - Het standaardbeleid selecteren: **HourlyLogBackup**.
    - Een bestaand back-upbeleid kiezen dat u eerder hebt gemaakt voor SQL.
    - [Een nieuw beleid definiëren](backup-azure-sql-database.md#define-a-backup-policy) op basis van uw RPO en retentiebereik.

    > [!Note]
    > Azure Backup biedt ondersteuning voor langetermijnretentie op basis van het zogenaamde grootvader-vader-zoon-back-upschema. Met dit schema wordt het gebruik van back-endopslag geoptimaliseerd terwijl tevens wordt voldaan aan de nalevingsvereisten.
    >

9. Nadat u een back-upbeleid hebt gekozen, opent u het menu **Back-up** en selecteert u **Back-up inschakelen**.

    ![Het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

    De voortgang van de configuratie volgen in het **Systeemvak** van de portal.

    ![Systeemvak](./media/backup-azure-sql-database/notifications-area.png)


## <a name="auto-protect-sql-server-in-azure-vm"></a>SQL Server automatisch beveiligen in Azure VM  

Met automatische beveiliging kunt u automatisch alle bestaande databases en databases die u in de toekomst toevoegt aan een zelfstandig exemplaar van SQL Server of een AlwaysOn-beschikbaarheidsgroep van SQL Server beveiligen. Als u automatische beveiliging **inschakelt** en een back-upbeleid kiest, wordt dat beleid toegepast op nieuwe beveiligde databases. Op de bestaande beveiligde databases wordt nog steeds het vorige beleid toegepast.

![Automatische beveiliging inschakelen voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

Er is geen limiet voor het aantal databases dat in één keer kan worden geselecteerd met behulp van de automatische beveiligingsfunctie. Back-up configureren wordt getriggerd voor alle databases tegelijk en kan worden gevolgd in **Back-uptaken**.

Als u om een bepaalde reden de automatische beveiliging voor een exemplaar moet uitschakelen, klikt u op de naam van het exemplaar onder **Back-up configureren** om het informatiepaneel aan de rechterkant te openen. Boven in het paneel vindt u de optie **Automatische beveiliging uitschakelen**. Klik op **Automatische beveiliging uitschakelen** om de automatische beveiliging voor dat exemplaar uit te schakelen.

![Automatische beveiliging voor dat exemplaar uitschakelen](./media/backup-azure-sql-database/disable-auto-protection.png)

Alle databases op dat exemplaar blijven beveiligd. Met deze actie wordt de automatische beveiliging echter wel uitgeschakeld voor alle databases die in de toekomst worden toegevoegd.

### <a name="define-a-backup-policy"></a>Een back-upbeleid definiëren

Een back-upbeleid bepaalt wanneer back-ups worden gemaakt en hoe lang ze worden bewaard. U kunt drie typen back-ups voor SQL-databases plannen in Azure Backup:

* Volledige back-up: Bij een volledige back-up wordt er een back-up van de hele database gemaakt. Een volledige back-up bevat alle gegevens van een specifieke database, of een set bestandsgroepen of bestanden, en voldoende logbestanden om die gegevens te kunnen terugzetten. U kunt maximaal één volledige back-up per dag activeren. U kunt kiezen of u dagelijks of wekelijks een volledige back-up wilt uitvoeren.
* Differentiële back-up: Een differentiële back-up is gebaseerd op de meest recente, vorige volledige gegevensback-up. Bij een differentiële back-up worden alleen de gegevens vastgelegd die na de volledige back-up zijn gewijzigd. U kunt maximaal één differentiële back-up per dag activeren. U kunt niet zowel een volledige back-up als een differentiële back-up configureren op dezelfde dag.
* Back-up transactielogboek: Met een logboekback-up kunt u herstel naar een bepaald tijdstip uitvoeren, tot op een specifieke seconde. U kunt maximaal elke 15 minuten een back-up van het transactielogboek configureren.

Het beleid wordt gemaakt op het niveau van de Recovery Services-kluis. U kunt hetzelfde back-upbeleid gebruiken voor meerdere kluizen, maar u moet het back-upbeleid toepassen op elke kluis. Wanneer u een back-upbeleid maakt, is de dagelijkse volledige back-up de standaardinstelling. U kunt een differentiële back-up toevoegen, maar alleen als u een wekelijkse volledige back-up configureert. In de volgende procedure wordt uitgelegd hoe u een back-upbeleid voor een SQL Server-exemplaar maakt op een virtuele machine van Azure.

> [!NOTE]
> In de Preview-versie kunt u een back-upbeleid niet bewerken. In plaats daarvan moet u een nieuw beleid maken met de gewenste gegevens.  

Ga als volgt te werk om een back-upbeleid te maken:

1. Klik in de Recovery Services-kluis waarmee de SQL-database wordt beveiligd op **Back-upbeleid** en vervolgens op **Toevoegen**.

   ![Het dialoogvenster voor het maken van een nieuw back-upbeleid openen](./media/backup-azure-sql-database/new-policy-workflow.png)

   Het menu **Toevoegen** wordt weergegeven.

2. Klik in het menu **Toevoegen** op **SQL Server in Azure VM**.

   ![Een beleidstype voor het nieuwe back-upbeleid kiezen](./media/backup-azure-sql-database/policy-type-details.png)

   Door SQL Server in Azure VM te selecteren, definieert u het beleidstype en wordt het menu Back-upbeleid geopend. In het menu **Back-upbeleid** ziet u de benodigde velden voor een nieuw back-upbeleid voor SQL Server.

3. Geef bij **Beleidsnaam** een naam voor het nieuwe beleid op.

4. Een volledige back-up is verplicht. U kunt de optie **Volledige back-up** niet uitschakelen. Klik op **Volledige back-up** om het beleid te bekijken en bewerken. Ook als u het back-upbeleid niet wilt wijzigen, moet u de details van het beleid bekijken.

    ![Velden voor nieuw back-upbeleid](./media/backup-azure-sql-database/full-backup-policy.png)

    In het menu voor het **beleid voor een volledige back-up** kiest u bij **Back-upfrequentie** de optie **Dagelijks** of **Wekelijks**. Als u **Dagelijks** kiest, selecteert u het tijdstip en de tijdzone waarop de back-uptaak moet worden gestart. U kunt geen differentiële back-ups maken voor dagelijkse volledige back-ups.

   ![Intervalinstelling Dagelijks](./media/backup-azure-sql-database/daily-interval.png)

    Als u **Wekelijks** kiest, selecteert u de dag van de week, het tijdstip en de tijdzone waarop de back-uptaak moet worden gestart.

   ![Intervalinstelling Wekelijks](./media/backup-azure-sql-database/weekly-interval.png)

5. Standaard zijn alle opties voor een **bewaarperiode** geselecteerd: Dagelijks, Wekelijk, Maandelijks en Jaarlijks. Schakel alle ongewenste bewaarperioden uit. Stel de gewenste intervallen in. In het menu voor het **beleid voor een volledige back-up** selecteert u **OK** om de instellingen te accepteren.

   ![Intervalinstellingen voor bewaarperiode](./media/backup-azure-sql-database/retention-range-interval.png)

    Herstelpunten worden getagd voor retentie op basis van de bewaarperiode. Als u een dagelijkse volledige back-up selecteert, wordt slechts één volledige back-up per dag geactiveerd. De back-up voor een specifieke dag wordt getagd en bewaard op basis van de wekelijkse bewaarperiode en uw wekelijkse bewaarbeleid. De maandelijkse en jaarlijkse bewaarperioden werken op soortgelijke wijze.

6. Als u een beleid voor een differentiële back-up wilt toevoegen, selecteert u **Differentiële back-up**. Het menu voor het **beleid voor een differentiële back-up** wordt geopend.

   ![Het menu voor het beleid voor een differentiële back-up openen](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    In het menu voor het **beleid voor een differentiële back-up** selecteert u **Inschakelen** om de frequentie- en bewaarinstellingen te openen. U kunt maximaal één differentiële back-up per dag activeren.

    > [!Important]
    > Differentiële back-ups kunnen maximaal 180 dagen worden bewaard. Als dat voor u te kort is, moet u volledige back-ups gebruiken.
    >

   ![Het beleid voor een differentiële back-up bewerken](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Selecteer **OK** om het beleid op te slaan en terug te gaan naar het hoofdmenu **Back-upbeleid**.

7. Als u een back-upbeleid voor een transactielogboek wilt toevoegen, selecteert u **Logboekback-up**. Het menu **Logboekback-up** wordt geopend.

    In het menu **Logboekback-up** selecteert u **Inschakelen** en stelt u de frequentie- en bewaarinstellingen in. Logboekback-ups kunnen elke 15 minuten worden geactiveerd en maximaal 35 dagen worden bewaard. Selecteer **OK** om het beleid op te slaan en terug te gaan naar het hoofdmenu **Back-upbeleid**.

   ![Het logboekback-upbeleid bewerken](./media/backup-azure-sql-database/log-backup-policy-editor.png)

8. Kies in het menu **Back-upbeleid** of u **Compressie van SQL-back-ups** wilt inschakelen. Compressie is standaard uitgeschakeld.

    Op de back-end maakt Azure Backup gebruik van systeemeigen compressie van SQL-back-ups.

9. Als u klaar bent met het bewerken van het back-upbeleid, selecteert u **OK**.

   ![Het nieuwe back-upbeleid accepteren](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Een SQL-database herstellen
Azure Backup biedt functionaliteit voor het herstellen van afzonderlijke databases naar een specifieke datum of tijd (tot op de seconde) met behulp van transactielogboekback-ups. Azure Backup bepaalt automatisch de juiste keten van volledige back-ups, differentiële back-ups en logboekback-ups die nodig zijn voor het herstellen van uw gegevens op basis van uw hersteltijden.

U kunt ook een specifieke volledige of differentiële back-up selecteren voor herstel naar een specifiek herstelpunt, in plaats van een specifiek tijdstip.

### <a name="pre-requisite-before-triggering-a-restore"></a>Vereiste voor het activeren van een herstelbewerking

1. U kunt de database herstellen naar een exemplaar van SQL Server in dezelfde Azure-regio. De doelserver moet zijn geregistreerd bij dezelfde Recovery Services-kluis als de bron.  
2. Als u een versleutelde TDE-database wilt herstellen naar een ander exemplaar van SQL Server, moet u eerst het certificaat terugzetten naar de doelserver door de [hier](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017) beschreven stappen uit te voeren.
3. Voordat u een herstelbewerking van de hoofddatabase activeert, start u het SQL Server-exemplaar in de modus voor één gebruiker met de opstartoptie `-m AzureWorkloadBackup`. Het argument voor de optie `-m` is de naam van de client. Alleen deze client mag de verbinding openen. Voor alle systeemdatabases (model, master, msdb), moet u de SQL Server Agent-service stoppen voordat u de herstelbewerking activeert. Sluit alle toepassingen waarmee mogelijk een verbinding met een van deze databases kan worden 'gestolen'.

### <a name="steps-to-restore-a-database"></a>Stappen voor het herstellen van een database:

1. Open de Recovery Services-kluis die is geregistreerd bij de virtuele machine van SQL.

2. In het dashboard van **Recovery Services-kluis**, onder **Gebruik**, selecteert u **Back-upitems** om het menu **Back-upitems** te openen.

    ![Het menu Back-upitems openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. In het menu **Back-upitems**, onder **Type back-upbeheer**, selecteert u **SQL in Azure VM**.

    ![SQL in Azure VM selecteren](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    In het menu **Back-upitems** wordt de lijst met SQL-databases weergegeven.

4. Selecteer de database die u wilt herstellen in de lijst met SQL-databases.

    ![De te herstellen database selecteren](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt het bijbehorende menu geopend. Het menu bevat de detailgegevens van de back-up van de database, met inbegrip van:

    * De oudste en meest recente herstelpunten.
    * Logboekback-upstatus voor de afgelopen 24 uur (voor hele databases en databases met herstelmodel met bulksgewijs geregistreerde wijzigingen, indien geconfigureerd voor transactielogboekback-ups).

5. Selecteer **DB herstellen** in het menu voor de geselecteerde database. Het menu **Herstellen** wordt geopend.

    ![DB herstellen selecteren](./media/backup-azure-sql-database/restore-db-button.png)

    Wanneer u het menu **Herstellen** opent, wordt ook het menu **Configuratie herstellen** geopend. Het menu **Configuratie herstellen** is de eerste stap voor het configureren van de herstelbewerking. Gebruik dit menu om te selecteren waar u de gegevens wilt herstellen. De opties zijn:
    - **Alternatieve locatie**: De database herstellen naar een alternatieve locatie en de oorspronkelijke brondatabase behouden.
    - **DB overschrijven**: De gegevens herstellen naar hetzelfde exemplaar van SQL Server als de oorspronkelijke bron. Als u deze optie kiest, wordt de oorspronkelijke database overschreven.

    > [!Important]
    > Als de geselecteerde database tot een AlwaysOn-beschikbaarheidsgroep behoort, wordt het overschrijven van de database niet toegestaan door SQL Server. In dat geval is alleen de optie **Alternatieve locatie** beschikbaar.
    >

    ![Het menu Configuratie herstellen](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Herstellen naar een alternatieve locatie

Deze procedure begeleidt u bij het herstellen van gegevens naar een alternatieve locatie. Als u de database wilt overschrijven tijdens het herstellen, gaat u naar [De database herstellen en overschrijven](backup-azure-sql-database.md#restore-and-overwrite-the-database). In deze fase wordt de Recovery Services-kluis geopend en is het menu **Configuratie herstellen** zichtbaar. Als dit nog niet het geval is, begint u met [Een SQL-database herstellen](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> U kunt de database herstellen naar een exemplaar van SQL Server in dezelfde Azure-regio. De doelserver moet bij de Recovery Services-kluis zijn geregistreerd.
>

In het menu **Configuratie herstellen** worden in de vervolgkeuzelijst **Server** alleen de SQL Server-exemplaren weergegeven die zijn geregistreerd bij de Recovery Services-kluis. Als de gewenste server niet in de lijst voorkomt, raadpleegt u [SQL Server-databases detecteren](backup-azure-sql-database.md#discover-sql-server-databases) om de server te vinden. Tijdens het detectieproces worden nieuwe servers geregistreerd bij de Recovery Services-kluis.<br>
Als u een SQL-database wilt herstellen, hebt u de volgende machtigingen nodig:

* **Back-upoperator** -machtigingen voor Recovery Services **Vault** waarin u de herstelbewerking uitvoert.
* Toegang als **Inzender (schrijven)** tot de **bron-SQL-VM** (de virtuele machine waarvan een back-up is gemaakt en van waaruit u probeert te herstellen).
* Toegang als **Inzender (schrijven)** tot de doel-SQL-VM (de virtuele machine waarvoor u de herstelbewerking uitvoert; dit is dezelfde virtuele machine als de bron-VM in geval van herstel naar de oorspronkelijke locatie).

Herstellen naar een alternatieve locatie:

1. Ga als volgt te werk in het menu **Configuratie herstellen**:

    * Onder **Terugzetten naar** selecteert u **Alternatieve locatie**.
    * Open de vervolgkeuzelijst **Server** en kies het SQL Server-exemplaar waarvoor u de database wilt herstellen.
    * Open de vervolgkeuzelijst **Exemplaar** en kies een SQL Server-exemplaar.
    * Geef in het vak **Herstelde databasenaam** de naam van de doeldatabase op.
    * Selecteer zo nodig **Overschrijven als de database met dezelfde naam al bestaat op het geselecteerde SQL-exemplaar**.
    * Selecteer **OK** om de configuratie van het doel te voltooien en door te gaan met het kiezen van een herstelpunt.

    ![Waarden opgeven voor het menu Configuratie herstellen](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. In het menu **Herstelpunt selecteren** kiest u **Logboeken (tijdstip)** of **Volledig en differentieel** als herstelpunt. Als u wilt herstellen naar een bepaald logboek (tijdstip), gaat u verder met deze stap. Als u wilt herstellen naar een volledig en differentieel herstelpunt, gaat u naar stap 3.

    ![Het menu Herstelpunt selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    Herstel naar een bepaald tijdstip is alleen beschikbaar voor logboekback-ups voor databases met een volledige back-up en een herstelmodel met bulksgewijs geregistreerde wijzigingen. Ga als volgt te werk voor herstel naar een specifiek tijdstip:

    1. Selecteer **Logboeken (tijdstip)** als hersteltype.

        ![Het type herstelpunt kiezen](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **Datum/tijd herstellen** selecteert u de mini-agenda om de **agenda** te openen. In de **agenda** worden datums met herstelpunten vet weergegeven en is de huidige datum gemarkeerd. Selecteer een datum met herstelpunten. Datums zonder herstelpunten kunnen niet worden geselecteerd.

        ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Als u een datum hebt geselecteerd, worden de beschikbare herstelpunten in chronologische volgorde weergegeven in de tijdlijngrafiek.

    3. Geef de tijdlijngrafiek of het dialoogvenster **Tijd** om een bepaald tijdstip voor het herstelpunt op te geven. Selecteer **OK** om de stap voor het herstelpunt te voltooien.

       ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        Het menu **Herstelpunt selecteren** wordt gesloten en het menu **Geavanceerde configuratie** wordt geopend.

       ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Ga als volgt te werk in het menu **Geavanceerde configuratie**:

        * Stel **Herstellen met NORECOVERY** in op **Ingeschakeld** om de database uitgeschakeld te laten nadat deze is hersteld.
        * Als u de herstellocatie op de doelserver wilt wijzigen, geeft u een nieuw pad op in de kolom **Doel**.
        * Selecteer **OK** om de instellingen toe te passen. Sluit het menu **Geavanceerde configuratie**.

    5. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten. U kunt de voortgang van het herstel volgen in het **Systeemvak** of **Hersteltaken** selecteren in het menu van de database.

       ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)

3. In het menu **Herstelpunt selecteren** kiest u **Logboeken (tijdstip)** of **Volledig en differentieel** als herstelpunt. Als u een logboek (tijdstip) wilt herstellen, gaat u terug naar stap 2. Met deze stap herstelt u een specifiek volledig of differentieel herstelpunt. U ziet alle volledige en differentiële herstelpunten van de afgelopen 30 dagen. Als u herstelpunten van meer dan 30 dagen geleden wilt weergeven, selecteert u **Filter** om het menu **Herstelpunten filteren** te openen. Bij een differentieel herstelpunt wordt in Azure Backup eerst het juiste volledige herstelpunt hersteld en vervolgens het geselecteerde differentiële herstelpunt toegepast.

    ![Het menu Herstelpunt selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In het menu **Herstelpunt selecteren** selecteert u **Volledig en differentieel**.

       ![Volledig en differentieel selecteren](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        In het menu ziet u de lijst met beschikbare herstelpunten.

    2. Selecteer een herstelpunt in de lijst en selecteer **OK** om de procedure voor het herstelpunt te voltooien.

        ![Een volledig herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        Het menu **Herstelpunt** wordt gesloten en het menu **Geavanceerde configuratie** wordt geopend.

        ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Ga als volgt te werk in het menu **Geavanceerde configuratie**:

        * Stel **Herstellen met NORECOVERY** in op **Ingeschakeld** om de database uitgeschakeld te laten nadat deze is hersteld. **Herstellen met NORECOVERY** is standaard uitgeschakeld.
        * Als u de herstellocatie op de doelserver wilt wijzigen, geeft u een nieuw pad op in de kolom **Doel**.
        * Selecteer **OK** om de instellingen toe te passen. Sluit het menu **Geavanceerde configuratie**.

    4. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten. U kunt de voortgang van het herstel volgen in het **Systeemvak** of **Hersteltaken** selecteren in het menu van de database.

       ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>De database herstellen en overschrijven

Deze procedure begeleidt u bij het herstellen van gegevens en het overschrijven van een database. Als u de database wilt herstellen naar een alternatieve locatie, gaat u naar [Herstellen naar een alternatieve locatie](backup-azure-sql-database.md#restore-to-an-alternate-location). In deze fase wordt de Recovery Services-kluis geopend en is het menu **Configuratie herstellen** zichtbaar (zie de volgende afbeelding). Als dit nog niet het geval is, begint u met [Een SQL-database herstellen](backup-azure-sql-database.md#restore-a-sql-database).

![Het menu Configuratie herstellen](./media/backup-azure-sql-database/restore-overwrite-db.png)

In het menu **Configuratie herstellen** worden in de vervolgkeuzelijst **Server** alleen de SQL Server-exemplaren weergegeven die zijn geregistreerd bij de Recovery Services-kluis. Als de gewenste server niet in de lijst voorkomt, raadpleegt u [SQL Server-databases detecteren](backup-azure-sql-database.md#discover-sql-server-databases) om de server te vinden. Tijdens het detectieproces worden nieuwe servers geregistreerd bij de Recovery Services-kluis.

1. In het menu **Configuratie herstellen** selecteert u **DB overschrijven** en vervolgens **OK** om de configuratie van het doel te voltooien.

   ![DB overschrijven selecteren](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    De instellingen **Server**, **Exemplaar** en **Herstelde databasenaam** zijn niet nodig.

2. In het menu **Herstelpunt selecteren** kiest u **Logboeken (tijdstip)** of **Volledig en differentieel** als herstelpunt. Als u wilt herstellen naar een bepaald logboek (tijdstip), gaat u verder met deze stap. Als u wilt herstellen naar een volledig en differentieel herstelpunt, gaat u naar stap 3.

    ![Het menu Herstelpunt selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    Herstel naar een bepaald tijdstip is alleen beschikbaar voor logboekback-ups voor databases met een volledige back-up en een herstelmodel met bulksgewijs geregistreerde wijzigingen. Ga als volgt te werk voor herstel naar een specifieke seconde:

    1. Selecteer **Logboeken (tijdstip)** als herstelpunt.

        ![Het type herstelpunt kiezen](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **Datum/tijd herstellen** selecteert u de mini-agenda om de **agenda** te openen. In de **agenda** worden datums met herstelpunten vet weergegeven en is de huidige datum gemarkeerd. Selecteer een datum met herstelpunten. Datums zonder herstelpunten kunnen niet worden geselecteerd.

        ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Als u een datum hebt geselecteerd, worden de beschikbare herstelpunten in weergegeven in de tijdlijngrafiek.

    3. Geef de tijdlijngrafiek of het dialoogvenster **Tijd** om een bepaald tijdstip voor het herstelpunt op te geven. Selecteer **OK** om de stap voor het herstelpunt te voltooien.

       ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        Het menu **Herstelpunt selecteren** wordt gesloten en het menu **Geavanceerde configuratie** wordt geopend.

       ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Ga als volgt te werk in het menu **Geavanceerde configuratie**:

        * Stel **Herstellen met NORECOVERY** in op **Ingeschakeld** om de database uitgeschakeld te laten nadat deze is hersteld.
        * Als u de herstellocatie op de doelserver wilt wijzigen, geeft u een nieuw pad op in de kolom **Doel**.
        * Selecteer **OK** om de instellingen toe te passen. Sluit het menu **Geavanceerde configuratie**.

    5. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten. U kunt de voortgang van het herstel volgen in het **Systeemvak** of **Hersteltaken** selecteren in het menu van de database.

       ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)

3. In het menu **Herstelpunt selecteren** kiest u **Logboeken (tijdstip)** of **Volledig en differentieel** als herstelpunt. Als u een logboek (tijdstip) wilt herstellen, gaat u terug naar stap 2. Met deze stap herstelt u een specifiek volledig of differentieel herstelpunt. U ziet alle volledige en differentiële herstelpunten van de afgelopen 30 dagen. Als u herstelpunten van meer dan 30 dagen geleden wilt weergeven, selecteert u **Filter** om het menu **Herstelpunten filteren** te openen. Bij een differentieel herstelpunt wordt in Azure Backup eerst het juiste volledige herstelpunt hersteld en vervolgens het geselecteerde differentiële herstelpunt toegepast.

    ![Het menu Herstelpunt selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In het menu **Herstelpunt selecteren** selecteert u **Volledig en differentieel**.

       ![Volledig en differentieel selecteren](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        In het menu ziet u de lijst met beschikbare herstelpunten.

    2. Selecteer een herstelpunt in de lijst en selecteer **OK** om de procedure voor het herstelpunt te voltooien.

        ![Een volledig herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        Het menu **Herstelpunt** wordt gesloten en het menu **Geavanceerde configuratie** wordt geopend.

        ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Ga als volgt te werk in het menu **Geavanceerde configuratie**:

        * Stel **Herstellen met NORECOVERY** in op **Ingeschakeld** om de database uitgeschakeld te laten nadat deze is hersteld. **Herstellen met NORECOVERY** is standaard uitgeschakeld.
        * Als u de herstellocatie op de doelserver wilt wijzigen, geeft u een nieuw pad op in de kolom **Doel**.
        * Selecteer **OK** om de instellingen toe te passen. Sluit het menu **Geavanceerde configuratie**.

    4. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten. U kunt de voortgang van het herstel volgen in het **Systeemvak** of door **Hersteltaken** te selecteren in het menu van de database.

       ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Bewerkingen van Azure Backup voor SQL in Azure VM's beheren

Dit gedeelte bevat informatie over de verschillende beschikbare Azure Backup-beheerbewerkingen voor SQL in Azure VM's. Mogelijke bewerkingen op hoog niveau zijn:

* Taken controleren
* Waarschuwingen voor back-ups
* Beveiliging voor een SQL-database stoppen
* Beveiliging voor een SQL-database hervatten
* Een ad-hocback-uptaak activeren
* Registratie van een server met SQL Server ongedaan maken

### <a name="monitor-backup-jobs"></a>Back-uptaken controleren
Azure Backup is een bedrijfsoplossing die voorziet in geavanceerde waarschuwingen voor back-ups en meldingen voor eventuele fouten. (Zie [Waarschuwingen voor back-ups weergeven](backup-azure-sql-database.md#view-backup-alerts).) Als u specifieke taken wilt volgen, gebruikt u een van de volgende opties al naargelang uw behoeften.

#### <a name="use-the-azure-portal-for-adhoc-operations"></a>Azure Portal gebruiken voor ad-hocbewerkingen
In Azure Backup worden alle handmatig geactiveerde taken of ad-hoctaken weergegeven in de portal **Back-uptaken**. De beschikbare taken in de portal **Back-uptaken** zijn onder meer:
- Alle back-upconfiguratiebewerkingen.
- Handmatig geactiveerde back-upbewerkingen.
- Herstelbewerkingen.
- Databaseregistratie- en -detectiebewerkingen.
- Back-up-stoppen-bewerkingen.

![De portal Back-uptaken](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Alle geplande back-uptaken, met inbegrip van volledige, differentiële en logboekback-ups, worden niet weergegeven in de portal **Back-uptaken**. Geplande back-uptaken kunt u bewaken met SQL Server Management Studio, zoals wordt beschreven in het volgende gedeelte.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>SQL Server Management Studio gebruiken voor back-uptaken
Azure Backup maakt gebruik van systeemeigen SQL-API's voor alle back-upbewerkingen. Gebruik de systeemeigen API's om alle taakgegevens op te halen uit de [SQL-tabel met back-ups](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in de msdb-database.

Het volgende voorbeeld is een query waarmee alle back-uptaken voor een database met de naam **DB1** worden opgehaald. Pas de query voor geavanceerde bewaking naar wens aan.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

### <a name="view-backup-alerts"></a>Waarschuwingen voor back-ups weergeven

Omdat er elke 15 minuten logboekback-ups worden gemaakt, kan het bewaken van back-uptaken veel tijd kosten. Azure Backup biedt uitkomst in deze situatie. E-mailwaarschuwingen worden geactiveerd voor alle mislukte back-ups. Waarschuwingen worden geconsolideerd op databaseniveau per foutcode. Er wordt alleen een e-mailmelding verzonden voor de eerste back-upfout voor een database. Meld u aan bij Azure Portal als u alle fouten voor een database wilt controleren.

Ga als volgt te werk om back-uptaken te controleren:

1. Meld u aan bij uw Azure-abonnement in [Azure Portal](https://portal.azure.com).

2. Open de Recovery Services-kluis die is geregistreerd bij de virtuele machine van SQL.

3. In het dashboard van de **Recovery Services-kluis** selecteert u **Waarschuwingen en gebeurtenissen**.

   ![Waarschuwingen en gebeurtenissen selecteren](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. In het menu **Waarschuwingen en gebeurtenissen** selecteert u **Waarschuwingen voor back-ups** om de lijst met waarschuwingen weer te geven.

   ![Waarschuwingen voor back-ups selecteren](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-for-a-sql-server-database"></a>Beveiliging van een SQL Server-database stoppen

Wanneer u de beveiliging van een SQL Server-database stopt in Azure Backup, wordt u gevraagd of u de herstelpunten wilt behouden. Er zijn twee manieren om de beveiliging van een SQL-database te stoppen:

* Alle toekomstige back-uptaken stoppen en alle herstelpunten verwijderen.
* Alle toekomstige back-uptaken stoppen en de herstelpunten behouden.

Als u ervoor kiest de back-uptaken te stoppen met behoud van gegevens, worden herstelpunten verwijderd volgens het back-upbeleid. Er worden kosten in rekening gebracht voor de beveiligde SQL-exemplaren en de gebruikte opslag totdat alle herstelpunten zijn verwijderd. Zie de [pagina met prijzen voor Azure Backup](https://azure.microsoft.com/pricing/details/backup/) voor meer informatie over prijzen van Azure Backup voor SQL.

Als u het maken van een back-up stopt met behoud van gegevens, verlopen de herstelpunten op basis van het retentiebeleid. Er blijft in Azure Backup echter altijd één herstelpunt behouden totdat u de back-upgegevens expliciet verwijdert. Op dezelfde manier mislukken back-ups, wanneer u een gegevensbron verwijdert zonder het maken van een back-up te stoppen. Oude herstelpunten verlopen dan op basis van het retentiebeleid, maar één herstelpunt blijft altijd behouden totdat u het maken van een back-up stopt met het verwijderen van gegevens.

Ga als volgt te werk om de beveiliging van een database te stoppen:

1. Open de Recovery Services-kluis die is geregistreerd bij de virtuele machine van SQL.

2. In het dashboard van **Recovery Services-kluis**, onder **Gebruik**, selecteert u **Back-upitems** om het menu **Back-upitems** te openen.

    ![Het menu Back-upitems openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. In het menu **Back-upitems**, onder **Type back-upbeheer**, selecteert u **SQL in Azure VM**.

    ![SQL in Azure VM selecteren](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    In het menu **Back-upitems** wordt de lijst met SQL-databases weergegeven.

4. Selecteer de database waarvoor u de beveiliging wilt stoppen in de lijst met SQL-databases.

    ![De database selecteren waarvoor u de beveiliging wilt stoppen](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt het bijbehorende menu geopend.

5. Selecteer **Back-up stoppen** in het menu voor de geselecteerde database.

    ![Back-up stoppen selecteren](./media/backup-azure-sql-database/stop-db-button.png)

    Het menu **Back-up stoppen** wordt geopend.

6. Kies de gewenste optie in het menu **Back-up stoppen**: **Back-upgegevens behouden** of **Back-upgegevens verwijderen**. Geef desgewenst een reden op voor het stoppen van de beveiliging, en een opmerking.

    ![Het menu Back-up stoppen](./media/backup-azure-sql-database/stop-backup-button.png)

7. Selecteer **Back-up stoppen** om de beveiliging van de database te stoppen.

  Houd er rekening mee dat de optie **Back-up stoppen** niet werkt voor een database op een exemplaar met [automatische beveiliging](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm). De enige manier om de beveiliging van deze database te stoppen, is de [automatische beveiliging](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) van het exemplaar voorlopig te stoppen en vervolgens **Back-up stoppen** te kiezen onder **Back-upitems** voor die database.<br>
  Nadat u de automatische beveiliging hebt uitgeschakeld, kunt u **Back-up stoppen** voor de database kiezen onder **Back-upitems**. Nu kunt u de automatische beveiliging voor het exemplaar weer inschakelen.


### <a name="resume-protection-for-a-sql-database"></a>Beveiliging voor een SQL-database hervatten

Als u de optie **Back-upgegevens behouden** hebt geselecteerd bij het stoppen van de beveiliging voor de SQL-database, kunt u de beveiliging hervatten. Als u de back-upgegevens niet hebt behouden, kan de beveiliging niet worden hervat.

1. Als u de beveiliging voor de SQL-database wilt hervatten, opent u het back-upitem en selecteert u **Back-up hervatten**.

    ![Back-up hervatten selecteren om de databasebeveiliging te hervatten](./media/backup-azure-sql-database/resume-backup-button.png)

   Het menu **Back-upbeleid** wordt geopend.

2. Selecteer een beleid in het menu **Back-upbeleid** en selecteer vervolgens **Opslaan**.

### <a name="trigger-an-adhoc-backup"></a>Een ad-hocback-up activeren

Activeer ad-hocback-ups naar behoefte. Er zijn vier typen ad-hocback-ups:

* Volledige back-up
* Alleen te kopiëren volledige back-up
* Differentiële back-up
* Logboekback-up

Klik [hier](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups) voor meer informatie over de verschillende typen SQL-back-ups.

### <a name="unregister-a-sql-server-instance"></a>Registratie van een SQL Server-exemplaar ongedaan maken

U kunt de registratie van een SQL Server-exemplaar als volgt ongedaan maken na het stoppen van de beveiliging, maar voordat u de kluis verwijdert:

1. Open de Recovery Services-kluis die is geregistreerd bij de virtuele machine van SQL.

2. In het dashboard van de **Recovery Services-kluis**, onder **Beheren**, selecteert u **Back-upinfrastructuur**.  

   ![Back-upinfrastructuur selecteren](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. Onder **Beheerservers** selecteert u **Beveiligde servers**.

   ![Beveiligde servers selecteren](./media/backup-azure-sql-database/protected-servers.png)

    Het menu **Beveiligde servers** wordt geopend.

4. In het menu **Beveiligde servers** selecteert u de server waarvan u de registratie ongedaan wilt maken. Als u de kluis wilt verwijderen, moet u de registratie van alle servers ongedaan maken.

5. Klik in het menu **Beveiligde servers** met de rechtermuisknop op de beveiligde server en selecteer **Verwijderen**.

   ![Verwijderen selecteren](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="faq"></a>Veelgestelde vragen

Het volgende gedeelte bevat aanvullende informatie over SQL-databaseback-ups.

### <a name="can-i-throttle-the-speed-of-the-sql-server-backup-policy"></a>Kan ik de snelheid van het SQL Server-back-upbeleid vertragen?

Ja. U kunt de snelheid waarmee het back-upbeleid wordt uitgevoerd beperken om het effect op een SQL Server-exemplaar te beperken.
Ga als volgt te werk om de instelling te wijzigen:
1. Maak op het SQL Server-exemplaar in de *map C:\Program Files\Azure Workload Backup\bin* het bestand **ExtensionSettingsOverrides.json**.
2. Wijzig in het bestand **ExtensionSettingsOverrides.json** de instelling **DefaultBackupTasksThreshold** in een lagere waarde (bijvoorbeeld 5). <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Sla uw wijzigingen op. Sluit het bestand.
4. Open **Taakbeheer** op het SQL Server-exemplaar. Start de service **AzureWLBackupCoordinatorSvc** opnieuw.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kan ik een volledige back-up van een secundaire replica uitvoeren?
Nee. Deze functie wordt niet ondersteund.

### <a name="do-successful-backup-jobs-create-alerts"></a>Maken succesvolle back-uptaken waarschuwingen?

Nee. Succesvolle back-uptaken maken geen waarschuwingen. Er worden alleen waarschuwingen verzonden voor mislukte back-uptaken.

### <a name="can-i-see-details-for-scheduled-backup-jobs-in-the-jobs-menu"></a>Kan ik de details voor geplande back-uptaken in het menu Taken bekijken?

Nee. In het menu **Back-uptaken** worden wel details van ad-hoctaken weergegeven, maar niet van geplande back-uptaken. Als een geplande back-uptaak mislukt, zijn de details beschikbaar in de waarschuwingen voor mislukte taken. Voor het bewaken van alle geplande en ad-hocback-uptaken gebruikt u [SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="when-i-select-a-sql-server-instance-are-future-databases-automatically-added"></a>Worden toekomstige databases automatisch toegevoegd wanneer ik een SQL Server-exemplaar selecteer?

Nee. Wanneer u de beveiliging voor een SQL Server-exemplaar configureert op serverniveau, worden alle databases toegevoegd. Als u databases aan een SQL Server-exemplaar toevoegt na het configureren van de beveiliging, moet u de nieuwe databases handmatig toevoegen om ze te beveiligen. De databases worden niet automatisch opgenomen in de geconfigureerde beveiliging.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Als ik het herstelmodel wijzig, hoe start ik dan de bescherming opnieuw op?

Activeer een volledige back-up. Logboekback-ups worden gestart zoals verwacht.

### <a name="can-i-protect-sql-always-on-availability-groups-where-the-primary-replica-is-on-premises"></a>Kan ik SQL AlwaysOn-beschikbaarheidsgroepen beveiligen als de primaire replica on-premises is?

Nee. Azure Backup beveiligt SQL Server-exemplaren die worden uitgevoerd in Azure. Als een beschikbaarheidsgroep wordt verdeeld over Azure en on-premises machines, kan de beschikbaarheidsgroep alleen worden beveiligd als de primaire replica wordt uitgevoerd in Azure. Azure Backup beveiligt ook alleen de knooppunten die worden uitgevoerd in dezelfde Azure-regio als de Recovery Services-kluis.

### <a name="can-i-protect-sql-always-on-availability-groups-which-are-spread-across-azure-regions"></a>Kan ik SQL AlwaysOn-beschikbaarheidsgroepen beveiligen die zijn verdeeld over Azure-regio's?

Met de Recovery Services-kluis van Azure Backup kunt u alle knooppunten detecteren en beveiligen die zich in dezelfde regio als de Recovery Services-kluis bevinden. Als u een SQL AlwaysOn-beschikbaarheidsgroep hebt die meerdere Azure-regio's omvat, moet u de back-up configureren vanuit de regio met het primaire knooppunt. Azure Backup kan alle databases in de beschikbaarheidsgroep detecteren en beveiligen op basis van de back-upvoorkeur. Als niet aan de back-upvoorkeur wordt voldaan, zullen de back-ups mislukken en wordt u gewaarschuwd voor fouten.

### <a name="while-i-want-to-protect-most-of-the-databases-in-an-instance-i-would-like-to-exclude-a-few-is-it-possible-to-still-use-the-auto-protection-feature"></a>Ik wil de meeste databases op een exemplaar beveiligen, maar enkele niet. Is het dan nog steeds mogelijk om automatische beveiliging te gebruiken?

Nee, [automatische beveiliging](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) is van toepassing op het hele exemplaar. U kunt niet selectief databases op een exemplaar beveiligen met behulp van automatische beveiliging.

### <a name="can-i-have-different-policies-for-different-databases-in-an-auto-protected-instance"></a>Kan ik verschillend beleid voor verschillende databases hebben op een exemplaar dat automatisch wordt beveiligd?

Als u al beveiligde databases op een exemplaar hebt, blijven deze beveiligd met hun respectievelijke beleid, ook als u de optie voor [automatische beveiliging](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) inschakelt (met **ON**). Alle niet-beveiligde databases en de databases die u in de toekomst toevoegt, hebben echter slechts één beleid: het beleid dat u definieert onder **Back-up configureren** nadat de databases zijn geselecteerd. In feite kunt u, in tegenstelling tot andere beveiligde databases, het beleid voor een database onder een exemplaar met automatische beveiliging niet wijzigen.
De enige manier om dat te doen, is de automatische beveiliging voor het exemplaar uit te schakelen en vervolgens het beleid voor die database te wijzigen. Nu kunt u de automatische beveiliging voor dit exemplaar weer inschakelen.

### <a name="if-i-delete-a-database-from-an-auto-protected-instance-will-the-backups-for-that-database-also-stop"></a>Als ik een database niet verwijder van een exemplaar met automatische beveiliging, worden de back-ups voor die database dan ook gestopt?

Nee, als een database wordt verwijderd van een exemplaar met automatische beveiliging, worden de back-ups voor die database nog steeds uitgevoerd. Dit betekent dat de verwijderde database wordt weergegeven als 'niet in orde' onder **Back-upitems** en nog steeds wordt behandeld alsof deze is beveiligd.

De enige manier om de beveiliging van deze database te stoppen, is de [automatische beveiliging](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) van het exemplaar voorlopig te stoppen en vervolgens **Back-up stoppen** te kiezen onder **Back-upitems** voor die database. Nu kunt u de automatische beveiliging voor dit exemplaar weer inschakelen.

###  <a name="why-cant-i-see-the-newly-added-database-to-an-auto-protected-instance-under-the-protected-items"></a>Waarom wordt de database die ik zojuist aan een exemplaar met automatische beveiliging heb toegevoegd, niet weergegeven onder de beveiligde items?

Meestal ziet u een zojuist toegevoegde database niet direct op een [automatisch beveiligd](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) exemplaar. Dat komt doordat de detectie doorgaans om de 8 uur wordt uitgevoerd. De gebruiker kan echter databases handmatig detecteren met behulp van de optie **DB's herstellen** om nieuwe databases onmiddellijk te detecteren en beveiligen, zoals u kunt zien in de onderstaande afbeelding:

  ![Zojuist toegevoegde database weergeven](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Backup het Azure PowerShell-voorbeeld voor het maken van back-ups van versleutelde virtuele machines.

> [!div class="nextstepaction"]
> [Back-up maken van een versleutelde VM](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
