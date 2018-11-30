---
title: Maak een back-up van SQL Server-databases in Azure | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u back-up van SQL Server naar Azure. Het artikel wordt ook uitgelegd voor herstel van SQL Server.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 6091a3b3506adf87418b529c3cca6b96e9bb2af9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52317684"
---
# <a name="back-up-sql-server-databases-to-azure"></a>Back-up van SQL Server-databases naar Azure

SQL Server-databases zijn essentiële workloads waarvoor een lage herstelpuntdoel (RPO) en langetermijnbehoud. Azure Backup biedt een SQL Server-back-upoplossing die nul infrastructuur vereist: geen complexe back-upserver, er wordt geen beheeragent en er is geen back-upopslag om te beheren. Azure Backup biedt gecentraliseerd beheer voor uw back-ups voor alle servers waarop SQL Server, of zelfs in verschillende workloads.

In dit artikel leert u:

> [!div class="checklist"]
> * De vereisten voor het back-up van een exemplaar van SQL Server naar Azure.
> * Het maken en gebruiken van een Recovery Services-kluis.
> * Klik hier voor meer informatie over het configureren van back-ups van SQL Server-database.
> * Over het instellen van een beleid voor back-up (of behoud) voor de herstelpunten.
> * Het herstellen van de database.

Voordat u de procedures in dit artikel, hebt u een SQL Server-exemplaar dat wordt uitgevoerd in Azure. [Gebruik SQL Marketplace virtuele machines om snel een SQL Server-exemplaar](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Beperkingen voor openbare Preview

De volgende items zijn bekende beperkingen voor de openbare preview-versie:

- De SQL-virtuele machine (VM) is vereist voor toegang tot Azure openbare IP-adressen verbinding met internet. Zie voor meer informatie, [netwerkverbinding tot stand brengen](backup-azure-sql-database.md#establish-network-connectivity).
- Maximaal 2.000 SQL-databases in een Recovery Services-kluis beveiligen. Aanvullende SQL-databases moeten worden opgeslagen in een afzonderlijke Recovery Services-kluis.
- [Back-ups van gedistribueerde beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) hebben beperkingen.
- SQL Server Always On Failover Cluster Instances (Failoverclusterinstanties) worden niet ondersteund.
- De Azure portal gebruiken voor Azure Backup configureren om het beveiligen van SQL Server-databases. Azure PowerShell, de Azure CLI en de REST API's worden momenteel niet ondersteund.

Raadpleeg [gedeelte met veelgestelde vragen](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#faq) voor meer informatie over de ondersteuning/niet-ondersteunde scenario's.

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

Deze sectie beschrijft de back-up van Azure-ondersteuning voor besturingssystemen en versies van SQL Server. SQL Marketplace Azure virtual machines en niet-Marketplace virtuele machines (waarop SQL Server is handmatig geïnstalleerd) worden ondersteund.

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux wordt momenteel niet ondersteund.

### <a name="supported-sql-server-versions-and-editions"></a>Ondersteunde SQL Server-versies en edities

- SQL Server 2012 Enterprise, Standard, Web, Developer, Express
- SQL Server 2014 Enterprise, Standard, Web, Developer, Express
- SQL Server 2016 Enterprise, Standard, Web, Developer, Express
- SQL Server 2017 Enterprise, Standard, Web, Developer, Express

## <a name="prerequisites"></a>Vereisten

Voordat u back-up van SQL Server-database, controleert u de volgende voorwaarden:

- Bepalen of [maken van een Recovery Services-kluis](backup-azure-sql-database.md#create-a-recovery-services-vault) in dezelfde regio of land als de virtuele machine die als host fungeert voor uw SQL Server-exemplaar.
- [Controleer de machtigingen voor de virtuele machine](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) die nodig zijn voor het back-up van de SQL-databases.
- Controleer de [SQL virtuele machine heeft een netwerkverbinding](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> U kunt slechts één back-upoplossing hebben op een moment naar de back-up van SQL Server-databases. Alle andere SQL-back-ups uitschakelen voordat u deze functie. anders wordt de back-ups leiden tot problemen en mislukken. U kunt inschakelen Azure back-up voor IaaS-VM samen met de SQL-back-up zonder een conflict.
>

Als deze voorwaarden in uw omgeving bestaan, blijven [back-up configureren voor SQL Server-databases](backup-azure-sql-database.md#configure-backup-for-sql-server-databases). Lees verder als een van de vereiste nog niet bestaan.


## <a name="establish-network-connectivity"></a>De netwerkverbinding tot stand brengen

Voor alle bewerkingen moet de SQL-virtuele machine verbinding met Azure openbare IP-adressen. SQL VM-bewerkingen (zoals databasedetectie, back-ups configureren, back-ups, herstelpunten herstellen, enzovoort) uitvallen zonder dat de verbinding met het openbare IP-adressen. Gebruik een van de volgende opties voor een duidelijk pad voor back-upverkeer:

- De Azure-datacenter-IP-bereiken van geaccepteerde: aan lijst met geaccepteerde IP-bereiken met de Azure-datacenter, gebruiken de [Download Center-pagina voor meer informatie over de IP-bereiken en instructies](https://www.microsoft.com/download/details.aspx?id=41653).
- Een HTTP-proxy-server implementeren om verkeer te routeren: wanneer u back-up van een SQL-database in een virtuele machine, de back-upextensie op de virtuele machine maakt gebruik van de HTTPS-API's voor het verzenden van opdrachten voor beheer met Azure Backup en gegevens naar Azure Storage. De Backup-extensie gebruikt ook Azure Active Directory (Azure AD) voor verificatie. De back-upextensie verkeer omleiden voor deze drie services via de HTTP-proxy. De extensie's het enige onderdeel dat geconfigureerd voor toegang tot het openbare internet.

De balans tussen de opties zijn beheerbaarheid, gedetailleerde controle en kosten.

> [!NOTE]
> Servicetags voor Azure Backup moet beschikbaar zijn op algemene beschikbaarheid.
>

| Optie | Voordelen | Nadelen |
| ------ | ---------- | ------------- |
| Whitelist IP-bereiken | Er zijn geen extra kosten. <br/> Voor toegang tot het openen in een NSG, gebruikt u de **Set AzureNetworkSecurityRule** cmdlet. | Ingewikkeld om te beheren, omdat de betrokken IP-adresbereiken verloop van tijd veranderen. <br/>Biedt toegang tot het geheel van Azure, niet alleen Azure Storage.|
| Gebruik een HTTP-proxy   | Nauwkeurige controle in de proxy die over de opslag URL's is toegestaan. <br/>Één punt van internet toegang tot VM's. <br/> Niet onderhevig aan Azure-IP-adres verandert. | Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Machtigingen instellen voor niet - Marketplace SQL VM 's

Als u wilt back-up van een virtuele machine, Azure Backup moet de **AzureBackupWindowsWorkload** extensie worden geïnstalleerd. Als u Azure Marketplace virtual machines gebruiken, blijven [detecteren SQL Server-databases](backup-azure-sql-database.md#discover-sql-server-databases). Als de virtuele machine die als host fungeert voor uw SQL-databases wordt niet vanuit de Azure Marketplace gemaakt, voert u de volgende procedure om de extensie installeren en instellen van de juiste machtigingen. Naast de **AzureBackupWindowsWorkload** SQL sysadmin-bevoegdheden in SQL-databases beveiligen door Azure Backup-extensie is vereist. Voor het detecteren van databases op de virtuele machine, maakt Azure Backup de account **NT Service\AzureWLBackupPluginSvc**. Voor back-up van Azure voor het detecteren van SQL-databases, de **NT Service\AzureWLBackupPluginSvc** account moet hebben, SQL en SQL sysadmin-bevoegdheden. De volgende procedure wordt uitgelegd hoe u deze machtigingen leveren.

Om machtigingen te configureren:

1. In de [Azure-portal](https://portal.azure.com), opent u de Recovery Services-kluis die u gebruiken voor het beveiligen van de SQL-databases.

2. Op de **Recovery Services-kluis** dashboard, selecteer **back-up**. De **back-updoel** menu wordt geopend.

   ![Back-up om het menu back-updoel selecteren](./media/backup-azure-sql-database/open-backup-menu.png)

3. Op de **back-updoel** instellen in het menu **waar wordt uw werkbelasting uitgevoerd** op de standaardwaarde: **Azure**.

4. Vouw de **waarvan wilt u back-up maken** vervolgkeuzelijst in en selecteer **SQL Server in virtuele Azure-machine**.

    ![SQL Server in virtuele Azure-machine voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    De **back-updoel** menu u ziet nu twee stappen: **DB's detecteren in VM's** en **back-up configureren**. De **DB's detecteren in VM's** stap start een zoekopdracht voor virtuele machines van Azure.

    ![Bekijk de twee stappen van de back-updoel](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Onder **DB's detecteren in VM's**, selecteer **detectie starten** om te zoeken naar niet-beveiligde virtuele machines in het abonnement. Het kan even om te zoeken naar alle van de virtuele machines. De zoektijd, is afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement.

    ![Back-up in behandeling is bij het zoeken naar databases in virtuele machines](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Selecteer de virtuele machine die is voorzien van de SQL database-back-ups en selecteer vervolgens in de lijst met virtuele machines, **DB's detecteren**.

    Het detectieproces installeert de **AzureBackupWindowsWorkload** -extensie op de virtuele machine. De extensie kan de Azure Backup-service om te communiceren met de virtuele machine, zodat het back-up van de SQL-databases. Nadat de extensie is geïnstalleerd, Azure back-up wordt gemaakt van de virtuele Windows-serviceaccount **NT Service\AzureWLBackupPluginSvc** op de virtuele machine. Het virtuele serviceaccount vereist SQL sysadmin-machtiging. Tijdens het virtual service account installatieproces, als u de foutmelding `UserErrorSQLNoSysadminMembership`, Zie [oplossen SQL sysadmin-bevoegdheden](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

    De **meldingen** gebied toont de voortgang van de databasedetectie. Het kan even duren voor de taak is voltooid. De tijd voor de taak is afhankelijk van hoeveel databases bevinden zich in de virtuele machine. Wanneer de geselecteerde databases worden gedetecteerd, wordt er een bericht weergegeven.

    ![Implementatie-bericht](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nadat u de database aan de Recovery Services-kluis koppelen, de volgende stap is het [configureren van de back-uptaak](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="fix-sql-sysadmin-permissions"></a>SQL sysadmin-rechten oplossen

Tijdens het installatieproces, als u de foutmelding `UserErrorSQLNoSysadminMembership`, een account gebruiken met SQL Server sysadmin-machtigingen aan te melden bij naar SQL Server Management Studio (SSMS). Tenzij u speciale machtigingen nodig hebt, werkt Windows-verificatie.

1. Open de map beveiliging/aanmeldingen op de SQL-Server.

    ![Open de map beveiliging/aanmeldingen als u wilt zien van accounts](./media/backup-azure-sql-database/security-login-list.png)

2. Met de rechtermuisknop op de map aanmeldingen en selecteer **nieuwe aanmelding**. In de **aanmelding - nieuw** in het dialoogvenster, selecteer **zoeken**.

    ![Selecteer in de aanmelding - nieuw in het dialoogvenster Zoeken](./media/backup-azure-sql-database/new-login-search.png)

3. De virtuele Windows-serviceaccount **NT Service\AzureWLBackupPluginSvc** is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectiefase. Geef de accountnaam op, zoals wordt weergegeven in de **Enter the object named selecteren** vak. Selecteer **namen controleren** om de naam te herleiden.

    ![Selecteer namen op te lossen van de naam van de onbekende controleren](./media/backup-azure-sql-database/check-name.png)

4. Selecteer **OK** om het dialoogvenster te sluiten.

5. In de **serverfuncties** , zorg ervoor dat de **sysadmin** rol is geselecteerd. Selecteer **OK** om het dialoogvenster te sluiten.

    ![Zorg ervoor dat de serverrol sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

    De vereiste machtigingen moeten nu bestaan.

6. Hoewel u de Machtigingsfout hebt opgelost, moet u de database koppelen aan de Recovery Services-kluis. In de Azure-portal in de **beschermde Servers** lijst, met de rechtermuisknop op de server die zich in een fout staat en selecteer **DB's opnieuw detecteren**.

    ![Controleer of dat de server de juiste machtigingen heeft](./media/backup-azure-sql-database/check-erroneous-server.png)

    De **meldingen** gebied toont de voortgang van de databasedetectie. Het kan even duren voor de taak is voltooid. De tijd voor de taak is afhankelijk van hoeveel databases bevinden zich in de virtuele machine. Wanneer de geselecteerde databases worden gevonden, wordt er een bericht weergegeven.

    ![Implementatie-bericht](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nadat u de database aan de Recovery Services-kluis koppelen, de volgende stap is het [configureren van de back-uptaak](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detecteren van SQL Server-databases

Azure Backup wordt gedetecteerd voor alle databases op een SQL Server-exemplaar. U kunt de databases beveiligen op basis van uw back-upvereisten. Gebruik de volgende procedure voor het identificeren van de virtuele machine die als host fungeert voor de SQL-databases. Nadat u de virtuele machine hebt geïdentificeerd, wordt een lichtgewicht-extensie voor het detecteren van de SQL Server-databases in Azure Backup geïnstalleerd.

1. Meld u aan uw abonnement in de [Azure-portal](https://portal.azure.com/).

2. Selecteer in het menu links **alle services**.

    ![Alle services selecteren](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. In de **alle services** dialoogvenster vak, voer **herstelservices**. Terwijl u typt, filtert de invoer van de lijst met resources. Selecteer **Recovery Services-kluizen** in de lijst.

    ![Voer in en kies de Recovery Services-kluizen](./media/backup-azure-sql-database/all-services.png) <br/>

    De lijst met Recovery Services-kluizen in het abonnement wordt weergegeven.

4. In de lijst met Recovery Services-kluizen, selecteer de kluis voor het beveiligen van uw SQL-databases.

5. Op de **Recovery Services-kluis** dashboard, selecteer **back-up**. De **back-updoel** menu wordt geopend.

   ![Back-up om het menu back-updoel selecteren](./media/backup-azure-sql-database/open-backup-menu.png)

6. Op de **back-updoel** instellen in het menu **waar wordt uw werkbelasting uitgevoerd** op de standaardwaarde: **Azure**.

7. Vouw de **wat wilt u een back-up** vervolgkeuzelijst in en selecteer **SQL Server in virtuele Azure-machine**.

    ![SQL Server in virtuele Azure-machine voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    De **back-updoel** menu u ziet nu twee stappen: **DB's detecteren in VM's** en **back-up configureren**.

    ![Bekijk de twee stappen van de back-updoel](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Onder **DB's detecteren in VM's**, selecteer **detectie starten** om te zoeken naar niet-beveiligde virtuele machines in het abonnement. Het kan even om te zoeken door alle van de virtuele machines. De zoektijd, is afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement.

    ![Back-up in behandeling is bij het zoeken naar databases in virtuele machines](./media/backup-azure-sql-database/discovering-sql-databases.png)

    Nadat een niet-beveiligde virtuele machine is gedetecteerd, wordt deze weergegeven in de lijst. Meerdere virtuele machines hebben dezelfde naam. Echter, virtuele machines met dezelfde naam behoren tot verschillende resourcegroepen bevinden. Niet-beveiligde virtuele machines worden weergegeven door de virtuele machine en de bron-groep. Als een verwachte virtuele machine niet wordt weergegeven, kunt u zien als die virtuele machine al worden beveiligd door een kluis.

9. Selecteer de virtuele machine die is voorzien van de SQL database-back-ups en selecteer vervolgens in de lijst met virtuele machines, **DB's detecteren**.

    Azure Backup worden alle SQL-databases op de virtuele machine gedetecteerd. Zie voor meer informatie over wat er tijdens de detectiefase database gebeurt [bewerkingen op de achtergrond](backup-azure-sql-database.md#background-operations). Nadat de SQL-databases zijn gedetecteerd, kunt u [configureren van de back-uptaak](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="background-operations"></a>Bewerkingen op de achtergrond

Wanneer u gebruikt de **DB's detecteren** hulpprogramma voor Azure Backup wordt de volgende bewerkingen uitgevoerd op de achtergrond:

- Registreer de virtuele machine bij de Recovery Services-kluis voor back-up werkbelasting. Alle databases op de geregistreerde virtuele machine kunnen worden back-ups op alleen deze Recovery Services-kluis.

- Installeer de **AzureBackupWindowsWorkload** -extensie op de virtuele machine. Een back-up van een SQL is database een zonder agent. De extensie wordt geïnstalleerd op de virtuele machine en de geen agent geïnstalleerd op de SQL-database.

- De service-account maakt **NT Service\AzureWLBackupPluginSvc** op de virtuele machine. Alle bewerkingen voor back-up en herstel de serviceaccount gebruiken. **NT-Service\AzureWLBackupPluginSvc** SQL sysadmin-machtigingen hebben. Alle virtuele machines van de SQL-Marketplace worden geleverd met de **SqlIaaSExtension** geïnstalleerd. De **AzureBackupWindowsWorkload** extensie gebruikt de **SQLIaaSExtension** om automatisch de vereiste machtigingen. Als uw virtuele machine beschikt niet over de **SqlIaaSExtension** geïnstalleerd, het detecteren van DB-bewerking is mislukt met het foutbericht `UserErrorSQLNoSysAdminMembership`. Als u wilt toevoegen de sysadmin-machtigingen voor back-up, volg de instructies in [instellen van machtigingen voor niet - Marketplace SQL VM's van de Azure Backup](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

    ![Selecteer de virtuele machine en de database](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-databases"></a>Back-up voor SQL Server-databases configureren

Azure Backup biedt beheerservices voor het beveiligen van uw SQL Server-databases en back-uptaken beheren. De beheer- en bewakingsfuncties, is afhankelijk van uw Recovery Services-kluis.

> [!NOTE]
> U kunt slechts één back-upoplossing hebben op een moment naar de back-up van SQL Server-databases. Alle andere SQL-back-ups uitschakelen voordat u deze functie. anders wordt de back-ups leiden tot problemen en mislukken. U kunt inschakelen Azure back-up voor IaaS-VM samen met de SQL-back-up zonder een conflict.
>

Beveiliging voor een SQL-database configureren:

1. Open de Recovery Services-kluis die geregistreerd bij de virtuele machine van SQL.

2. Op de **Recovery Services-kluis** dashboard, selecteer **back-up**. De **back-updoel** menu wordt geopend.

   ![Back-up om het menu back-updoel selecteren](./media/backup-azure-sql-database/open-backup-menu.png)

3. Op de **back-updoel** instellen in het menu **waar wordt uw werkbelasting uitgevoerd** op de standaardwaarde: **Azure**.

4. Vouw de **wat wilt u een back-up** vervolgkeuzelijst in en selecteer **SQL Server in virtuele Azure-machine**.

    ![SQL Server in virtuele Azure-machine voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    De **back-updoel** menu u ziet nu twee stappen: **DB's detecteren in VM's** en **back-up configureren**.

    Als u de stappen in dit artikel in volgorde hebt voltooid, hebt u de niet-beveiligde virtuele machines gevonden en deze kluis is geregistreerd bij een virtuele machine. U bent nu klaar om te configureren van beveiliging voor de SQL-databases.

5. Op de **back-updoel** in het menu **back-up configureren**.

    ![Selecteer de back-up configureren](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    De Azure Backup-service wordt weergegeven voor alle SQL Server-exemplaren met zelfstandige databases en SQL Server Always On availability groups. Als u wilt de zelfstandige databases weergeven in de SQL Server-exemplaar, de pijl-omlaag links van de naam van het exemplaar selecteren Op dezelfde manier, selecteer de pijl-omlaag links van de AlwaysOn-beschikbaarheidsgroep om de lijst met databases weer te geven. De volgende afbeelding is een voorbeeld van een zelfstandig exemplaar en een AlwaysOn-beschikbaarheidsgroep.

      ![Alle SQL Server-exemplaren met zelfstandige databases weergeven](./media/backup-azure-sql-database/list-of-sql-databases.png)

6. Selecteer in de lijst met databases, alle databases die u wilt beveiligen, en klikt u op **OK**.

    ![Beveiliging van de database](./media/backup-azure-sql-database/select-database-to-protect.png)

    U kunt maximaal 50 databases tegelijk selecteren. Ter bescherming van meer dan 50 databases, moet u verschillende passen. Nadat u de eerste 50 databases beveiligen, Herhaal deze stap ter bescherming van de volgende set van databases.

    > [!Note]
    > Voor het optimaliseren van back-belastingen, Azure Backup-einden grote back-uptaken in meerdere batches. Het maximum aantal databases in één back-uptaak is 50.
    >

    U kunt ook kunt u automatische beveiliging op de volledige instantie of AlwaysOn-beschikbaarheidsgroep inschakelen door het selecteren van de **ON** optie in de bijbehorende vervolgkeuzelijst in de **AUTOPROTECT** kolom. De functie Automatische beveiliging niet alleen kunt beveiliging op alle bestaande databases in één go maar ook automatisch beveiligd door een nieuwe databases die in de toekomst naar dat exemplaar of de beschikbaarheidsgroep worden toegevoegd.  

      ![Inschakelen van automatische beveiliging van de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

      Als een exemplaar of een beschikbaarheidsgroep al enkele van de databases die zijn beveiligd heeft, kunt u nog steeds inschakelen **ON** de auto-protect-optie. In dit geval wordt de back-beleid is gedefinieerd in de volgende stap nu alleen zijn van toepassing op de niet-beveiligde databases terwijl de al beveiligde databases blijven met hun beleid wordt beveiligd.

      Er is geen limiet voor het aantal databases dat in één go ophalen geselecteerd met behulp van automatisch beveiligen functie (als er veel databases zoals in de kluis kunnen worden geselecteerd).  

      Het is raadzaam dat u op de automatische beveiliging voor alle instanties en AlwaysOn-beschikbaarheidsgroepen inschakelen als u wilt dat alle databases in de toekomst toegevoegd worden automatisch geconfigureerd voor beveiliging.


7. Om te maken of kies een back-upbeleid op de **back-up** in het menu **back-upbeleid**. De **back-upbeleid** menu wordt geopend.

    ![Back-upbeleid selecteren](./media/backup-azure-sql-database/select-backup-policy.png)

8. In de **back-upbeleid kiezen** vervolgkeuzelijst, kiest u een back-upbeleid en selecteer **OK**. Zie voor meer informatie over het maken van een back-upbeleid [een back-upbeleid definiëren](backup-azure-sql-database.md#define-a-backup-policy).

   > [!NOTE]
   > Tijdens de Preview, kunt u back-upbeleid niet bewerken. Als u een ander beleid dan wat er beschikbaar is in de lijst wilt, moet u dat beleid maken. Zie de sectie voor informatie over het maken van een nieuwe back-upbeleid [een back-upbeleid definiëren](backup-azure-sql-database.md#define-a-backup-policy).

    ![Een back-upbeleid kiezen uit de lijst](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    Op de **back-upbeleid** menu in de **back-upbeleid kiezen** vervolgkeuzelijst vak, kunt u:
    - Selecteer het standaardbeleid: **HourlyLogBackup**.
    - Kies een bestaand back-upbeleid eerder hebt gemaakt voor SQL.
    - [Een nieuw beleid definiëren](backup-azure-sql-database.md#define-a-backup-policy) op basis van uw bereik RPO en retentie.

    > [!Note]
    > Azure Backup biedt ondersteuning voor langetermijnretentie die gebaseerd op het zogenaamde grootvader-vader-zoon back-upschema's. Het schema optimaliseert verbruik van back-end-opslag terwijl naleving van de vergadering nodig heeft.
    >

9. Nadat u een back-upbeleid kiezen op de **menu back-up**, selecteer **back-up inschakelen**.

    ![het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

    Bijhouden van de voortgang van de configuratie in de **meldingen** gebied van de portal.

    ![Systeemvak](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Een back-upbeleid definiëren

Een back-upbeleid definieert een matrix met wanneer back-ups worden gemaakt en hoe lang ze worden bewaard. Met Azure Backup voor het plannen van de drie typen van back-up voor SQL-databases:

* Volledige back-up: een volledige databaseback-up een back-up van de gehele database. Een volledige back-up bevat alle gegevens in een specifieke database of een set bestandsgroepen of bestanden en voldoende Logboeken om die gegevens te herstellen. U kunt maximaal, een volledige back-up per dag activeren. U kunt een volledige back-up te nemen in een interval van dagelijks of wekelijks.
* Differentiële back-up: een differentiële back-up is gebaseerd op de meest recente, de vorige volledige gegevens back-up. Een differentiële back-up legt alleen de gegevens die zijn gewijzigd nadat de volledige back-up. U kunt maximaal één differentiële back-up per dag activeren. U kunt een volledige back-up en een differentiële back-up niet configureren op dezelfde dag.
* Transactielogboekback-up: een logboekback-up kunt punt in tijd herstel tot een specifieke seconde. Maximaal, kunt u configureren transactionele logboekback-ups om de 15 minuten.

Van het beleid gemaakt in de Recovery Services-kluis niveau. Meerdere kluizen het dezelfde back-upbeleid kunnen gebruiken, maar u moet het back-upbeleid toepassen op elke kluis. Wanneer u een back-upbeleid maakt, is de dagelijkse volledige back-up de standaardinstelling. Als u een volledige back-ups wekelijks optreden als u wilt configureren, kunt u een differentiële back-up, maar alleen toevoegen. De volgende procedure wordt uitgelegd hoe u een back-upbeleid voor een SQL Server-exemplaar maakt in een virtuele machine van Azure.

> [!NOTE]
> Preview-versie, kunt u een back-upbeleid niet bewerken. In plaats daarvan moet u een nieuw beleid maken met de gewenste gegevens.  

Een back-upbeleid maken:

1. Klik in de Recovery Services-kluis die worden beveiligd met de SQL-database op **back-upbeleid**, en klik vervolgens op **toevoegen**.

   ![Open het dialoogvenster nieuwe back-upbeleid maken](./media/backup-azure-sql-database/new-policy-workflow.png)

   De **toevoegen** menu wordt weergegeven.

2. In de **toevoegen** menu, klikt u op **SQL Server in virtuele Azure-machine**.

   ![Kies een beleidstype voor de nieuwe back-upbeleid](./media/backup-azure-sql-database/policy-type-details.png)

   SQL-Server in virtuele Azure-machine te selecteren definieert het beleidstype en het menu back-upbeleid wordt geopend. De **back-upbeleid** menu toont de velden die nodig voor een nieuwe SQL Server back-upbeleid zijn.

3. In **beleidsnaam**, voer een naam in voor het nieuwe beleid.

4. Een volledige back-up is verplicht. u kunt geen uitschakelen de **volledige back-up** optie. Klik op **volledige back-up** bekijken en bewerken van het beleid. Zelfs als u het back-upbeleid niet wijzigt, moet u de details van het beleid bekijken.

    ![nieuwe back-upbeleid velden](./media/backup-azure-sql-database/full-backup-policy.png)

    In de **volledige back-upbeleid** in het menu voor **back-upfrequentie**, kiest u **dagelijkse** of **wekelijkse**. Voor **dagelijkse**, selecteert u het tijdstip en tijdzone wanneer de back-uptaak wordt gestart. U kunt geen differentiële back-ups voor dagelijkse volledige back-ups maken.

   ![dagelijks interval instellen](./media/backup-azure-sql-database/daily-interval.png)

    Voor **wekelijkse**, selecteert u de dag van de week, uur en tijdzone wanneer de back-uptaak wordt gestart.

   ![wekelijks interval instellen](./media/backup-azure-sql-database/weekly-interval.png)

5. Standaard alle **bewaartermijn** opties zijn geselecteerd: dagelijkse, wekelijkse, maandelijkse en jaarlijkse. Schakel alle limieten voor het bereik van ongewenste bewaren. Stel de intervallen dat u wilt gebruiken. In de **volledige back-upbeleid** in het menu **OK** te accepteren van de instellingen.

   ![Instellingen voor het bewaren bereik interval](./media/backup-azure-sql-database/retention-range-interval.png)

    Herstelpunten zijn voor de bewaarperiode gebaseerd op de bewaartermijn gelabeld. Als u een dagelijkse volledige back-up selecteert, wordt slechts één volledige back-up elke dag geactiveerd. De back-up voor een specifieke dag is gemarkeerd en bewaard op basis van de wekelijkse bewaartermijn en de instelling van de wekelijkse bewaren. De maandelijkse en jaarlijkse bewaartermijnen gedragen zich op een soortgelijke manier.

6. Als u wilt toevoegen een differentiële back-upbeleid, selecteert u **differentiële back-**. De **differentiële back-upbeleid** menu wordt geopend.

   ![Open het menu differentiële back-upbeleid](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Op de **differentiële back-upbeleid** in het menu **inschakelen** te openen, de frequentie en retentie-besturingselementen. U kunt maximaal één differentiële back-up per dag activeren.

    > [!Important]
    > Differentiële back-ups kunnen maximaal 180 dagen worden bewaard. Als u langere bewaartermijn nodig hebt, moet u de volledige back-ups gebruiken.
    >

   ![De differentiële back-upbeleid bewerken](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Selecteer **OK** slaat u het beleid en terugkeren naar de hoofdpagina **back-upbeleid** menu.

7. Als u wilt toevoegen een transactionele logboek back-upbeleid, selecteert u **logboekback-up**. De **logboekback-up** menu wordt geopend.

    In de **logboekback-up** in het menu **inschakelen**, en stel de frequentie en retentie-besturingselementen. Logboekback-ups kunnen optreden zo vaak als elke 15 minuten en maximaal 35 dagen kunnen worden bewaard. Selecteer **OK** slaat u het beleid en terugkeren naar de hoofdpagina **back-upbeleid** menu.

   ![Het logboek back-upbeleid bewerken](./media/backup-azure-sql-database/log-backup-policy-editor.png)

8. Op de **back-upbeleid** menu, kies of u om in te schakelen **compressie van SQL-back-up**. Compressie is standaard uitgeschakeld.

    Op de back-end Azure Backup maakt gebruik van systeemeigen back-up-compressie van SQL.

9. Nadat u de bewerkingen van het back-upbeleid hebt voltooid, selecteert u **OK**.

   ![De nieuwe back-upbeleid te accepteren](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Een SQL-database herstellen
Azure Backup biedt functionaliteit voor het herstellen van afzonderlijke databases naar een specifieke datum of tijd (voor de tweede) met behulp van transactielogboekback-ups. Azure Backup bepaalt automatisch de juiste volledige differentiële en de keten van logboekback-ups die nodig zijn voor het herstellen van uw gegevens op basis van uw hersteltijden.

U kunt ook een specifieke volledige of differentiële back-up te herstellen naar een specifiek herstelpunt, in plaats van een bepaald tijdstip selecteren.

### <a name="pre-requisite-before-triggering-a-restore"></a>Vereiste voordat een herstelpunt wordt geactiveerd

1. U kunt de database herstellen naar een exemplaar van een SQL-Server in dezelfde Azure-regio. De doelserver moet worden geregistreerd in dezelfde Recovery Services-kluis als de bron.  
2. Als u wilt een versleutelde TDE-database herstellen naar een andere SQL Server, eerst herstel het certificaat naar de doelserver door de volgende stappen die zijn beschreven [hier](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
3. Voordat u een herstel van de database 'master' activeert, start u de SQL Server-exemplaar in de modus voor één gebruiker met de opstartoptie `-m AzureWorkloadBackup`. Het argument voor de `-m` optie is de naam van de client. Alleen deze client is toegestaan om de verbinding te openen. Voor alle systeemdatabases (model, master, msdb), door de SQL Agent-service te stoppen voordat u de terugzetbewerking kan worden geactiveerd. Sluit alle toepassingen waarbij wordt geprobeerd te stelen van een verbinding met een van deze databases.

### <a name="steps-to-restore-a-database"></a>Stappen om een database te herstellen:

1. Open de Recovery Services-kluis die geregistreerd bij de virtuele machine van SQL.

2. Op de **Recovery Services-kluis** dashboard onder **gebruik**, selecteer **back-Upitems** openen de **back-Upitems** menu.

    ![Open het menu back-Upitems](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Op de **back-Upitems** menu onder **Type back-upbeheer**, selecteer **SQL in Azure VM**.

    ![Selecteer SQL in Azure-VM](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    De **back-Upitems** menu bevat een lijst van de SQL-databases.

4. Selecteer in de lijst met SQL-databases, de database te herstellen.

    ![Selecteer de database herstellen](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt het menu wordt geopend. Het menu bevat de details van de back-up voor de database, met inbegrip van:

    * De oudste en de meest recente herstelpunten.
    * Meld u back-upstatus voor de afgelopen 24 uur (voor databases in de volledige en bulksgewijs geregistreerde herstelmodel, indien geconfigureerd voor transactionele logboekback-ups).

5. Selecteer in het menu voor de geselecteerde database **DB herstellen**. De **herstellen** menu wordt geopend.

    ![Selecteer Restore DB](./media/backup-azure-sql-database/restore-db-button.png)

    Wanneer de **herstellen** menu wordt geopend, de **configuratie herstellen** menu ook wordt geopend. De **configuratie herstellen** menu is de eerste stap bij het configureren van de herstelbewerking. Gebruik dit menu om te selecteren waar u de gegevens herstelt. De opties zijn:
    - **Alternatieve locatie**: de database herstellen naar een alternatieve locatie en de oorspronkelijke brondatabase behouden.
    - **DB overschrijven**: de gegevens te herstellen naar hetzelfde exemplaar van SQL Server als de oorspronkelijke bron. Het effect van deze optie is om de oorspronkelijke database te overschrijven.

    > [!Important]
    > Als de geselecteerde database bij een AlwaysOn-beschikbaarheidsgroep hoort, niet de database worden overschreven door SQL Server toegestaan. In dit geval wordt alleen de **alternatieve locatie** optie is ingeschakeld.
    >

    ![Menu van de configuratie herstellen](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Herstellen naar een alternatieve locatie

Deze procedure begeleidt u bij het herstellen van gegevens naar een alternatieve locatie. Als u wilt overschrijven van de database tijdens het herstellen, doorgaan met [herstellen en de database overschrijven](backup-azure-sql-database.md#restore-and-overwrite-the-database). In deze fase wordt de Recovery Services-kluis is geopend en de **configuratie herstellen** menu wordt weergegeven. Als u zich niet in deze fase, starten door [herstellen van een SQL-database](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> U kunt de database herstellen naar een exemplaar van een SQL-Server in dezelfde Azure-regio. De doelserver moet worden geregistreerd bij de Recovery Services-kluis.
>

Op de **configuratie herstellen** in het menu het **Server** vervolgkeuzelijst vak geeft de SQL Server-exemplaren die zijn geregistreerd bij de Recovery Services-kluis. Als de server die u wilt niet in de lijst, Zie [detecteren SQL Server-databases](backup-azure-sql-database.md#discover-sql-server-databases) de server te vinden. Tijdens het detectieproces worden nieuwe servers zijn geregistreerd bij de Recovery Services-kluis.

1. In de **configuratie herstellen** menu:

    * Onder **waar terugzetten**, selecteer **alternatieve locatie**.
    * Open de **Server** vervolgkeuzelijst en selecteert u de SQL Server-exemplaar om de database te herstellen.
    * Open de **exemplaar** vervolgkeuzelijst en selecteert u een SQL Server-exemplaar.
    * In de **databasenaam hersteld** voert u de naam van de doeldatabase.
    * Indien van toepassing, selecteer **overschrijven als de database met dezelfde naam al in het geselecteerde SQL-exemplaar bestaat**.
    * Selecteer **OK** voltooid de configuratie van de bestemming en doorgaan met het kiezen van een herstelpunt.

    ![Geef waarden op voor het herstellen van configuratie-menu](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Op de **herstelpunt selecteren** menu, kiest u **Logboeken (tijdstip)** of **volledig en differentieel** als het herstelpunt. Als u wilt herstellen naar een bepaald punt in tijd, doorgaan met deze stap. Als u een volledige en differentiële herstelpunt herstellen, doorgaan met stap 3.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    De point-in-time-restore is alleen beschikbaar voor logboekback-ups voor databases met een volledige en bulksgewijs geregistreerde herstelmodel. Om te herstellen naar een bepaald punt in tijd:

    1. Selecteer **Logboeken (tijdstip)** als het type herstel.

        ![Kies het type herstel](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **datum/tijd herstellen**, selecteert u de miniwerkbalk agenda openen de **agenda**. Op de **agenda**, de vet weergegeven datums hebben herstelpunten en de huidige datum is gemarkeerd. Selecteer een datum met herstelpunten. Datums zonder herstelpunten kunnen niet worden geselecteerd.

        ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Nadat u een datum selecteert, geeft de beschikbare herstelpunten in de tijdlijngrafiek weer in een continue reeks.

    3. De tijdlijn graph gebruiken of de **tijd** in het dialoogvenster om op te geven van een bepaald tijdstip voor het herstelpunt. Selecteer **OK** om de stap van de punt herstellen te voltooien.

       ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        De **herstelpunt selecteren** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Op de **Advanced Configuration** menu:

        * Instellen om te voorkomen dat de database niet-operationeel na het herstel, **herstellen met GEENHERSTEL** naar **ingeschakeld**.
        * Als u wilt wijzigen van de locatie voor terugzetten op de doelserver, voert u een nieuw pad in de **doel** kolom.
        * Selecteer **OK** goed te keuren van de instellingen. Sluit de **Advanced Configuration** menu.

    5. Op de **herstellen** in het menu **herstellen** de hersteltaak starten. De voortgang bijhouden van het herstel in de **meldingen** gebied of selecteer **hersteltaken** in het menu van de database.

       ![Taakvoortgang herstellen](./media/backup-azure-sql-database/restore-job-notification.png)

3. Op de **herstelpunt selecteren** menu, kiest u **Logboeken (tijdstip)** of **volledig en differentieel** als het herstelpunt. Voor het herstellen van een point-in-time-logboek, Ga terug naar stap 2. Deze stap herstelt een specifieke volledige of differentiële herstelpunt. U ziet alle van de volledige en differentiële herstelpunten voor de afgelopen 30 dagen. Herstelpunten ouder dan 30 dagen Selecteer **Filter** openen de **Filter herstelpunten** menu. Voor een differentiële herstelpunt, Azure Backup eerst de juiste volledig herstelpunt hersteld en is van toepassing de geselecteerde differentiële herstelpunt.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In de **herstelpunt selecteren** selecteren in het menu **volledig en differentieel**.

       ![Volledige en differentiële selecteren](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Het menu ziet u de lijst met beschikbare herstelpunten.

    2. Selecteer een herstelpunt in de lijst en selecteer **OK** om het herstelpunt procedure te voltooien.

        ![Een volledig herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        De **herstelpunt** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

        ![Menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Op de **Advanced Configuration** menu:

        * Instellen om te voorkomen dat de database niet-operationeel na het herstel, **herstellen met GEENHERSTEL** naar **ingeschakeld**. **Herstellen met GEENHERSTEL** is standaard uitgeschakeld.
        * Als u wilt wijzigen van de locatie voor terugzetten op de doelserver, voert u een nieuw pad in de **doel** kolom.
        * Selecteer **OK** goed te keuren van de instellingen. Sluit de **Advanced Configuration** menu.

    4. Op de **herstellen** in het menu **herstellen** de hersteltaak starten. De voortgang bijhouden van het herstel in de **meldingen** gebied of selecteer **hersteltaken** in het menu van de database.

       ![Taakvoortgang herstellen](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Herstellen en de database overschrijven

Deze procedure begeleidt u bij het herstellen van gegevens en een database wordt overschreven. Als u wilt herstellen naar een alternatieve locatie, blijven [herstellen naar een alternatieve locatie](backup-azure-sql-database.md#restore-to-an-alternate-location). In deze fase wordt de Recovery Services-kluis is geopend en de **configuratie herstellen** menu wordt weergegeven (Zie de volgende afbeelding). Als u zich niet in deze fase, starten door [herstellen van een SQL-database](backup-azure-sql-database.md#restore-a-sql-database).

![Menu van de configuratie herstellen](./media/backup-azure-sql-database/restore-overwrite-db.png)

Op de **configuratie herstellen** in het menu het **Server** vervolgkeuzelijst vak geeft de SQL Server-exemplaren die zijn geregistreerd bij de Recovery Services-kluis. Als de server die u wilt niet in de lijst, Zie [detecteren SQL Server-databases](backup-azure-sql-database.md#discover-sql-server-databases) de server te vinden. Tijdens het detectieproces worden nieuwe servers zijn geregistreerd bij de Recovery Services-kluis.

1. In de **configuratie herstellen** in het menu **DB overschrijven**, en selecteer vervolgens **OK** om de configuratie van de bestemming te voltooien.

   ![Selecteer overschrijven DB](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    De **Server**, **exemplaar**, en **databasenaam hersteld** instellingen zijn niet nodig.

2. Op de **herstelpunt selecteren** menu, kiest u **Logboeken (tijdstip)** of **volledig en differentieel** als het herstelpunt. Als u wilt herstellen naar een bepaald punt in tijd, doorgaan met deze stap. Als u een volledige en differentiële herstelpunt herstellen, doorgaan met stap 3.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    De point-in-time-restore is alleen beschikbaar voor logboekback-ups voor databases met een volledige en bulksgewijs geregistreerde herstelmodel. Om te herstellen naar een specifieke seconde:

    1. Selecteer **Logboeken (tijdstip)** als het herstelpunt.

        ![Kies het type herstel](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **datum/tijd herstellen**, selecteert u de miniwerkbalk agenda openen de **agenda**. Op de **agenda**, de vet weergegeven datums hebben herstelpunten en de huidige datum is gemarkeerd. Selecteer een datum met herstelpunten. Datums zonder herstelpunten kunnen niet worden geselecteerd.

        ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Nadat u een datum selecteert, geeft de beschikbare herstelpunten weer in de tijdlijngrafiek.

    3. De tijdlijn graph gebruiken of de **tijd** in het dialoogvenster om op te geven van een bepaald tijdstip voor het herstelpunt. Selecteer **OK** om de stap van de punt herstellen te voltooien.

       ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        De **herstelpunt selecteren** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Op de **Advanced Configuration** menu:

        * Instellen om te voorkomen dat de database niet-operationeel na het herstel, **herstellen met GEENHERSTEL** naar **ingeschakeld**.
        * Als u wilt wijzigen van de locatie voor terugzetten op de doelserver, voert u een nieuw pad in de **doel** kolom.
        * Selecteer **OK** goed te keuren van de instellingen. Sluit de **Advanced Configuration** menu.

    5. Op de **herstellen** in het menu **herstellen** de hersteltaak starten. De voortgang bijhouden van het herstel in de **meldingen** gebied of selecteer **hersteltaken** in het menu van de database.

       ![Taakvoortgang herstellen](./media/backup-azure-sql-database/restore-job-notification.png)

3. Op de **herstelpunt selecteren** menu, kiest u **Logboeken (tijdstip)** of **volledig en differentieel** als het herstelpunt. Voor het herstellen van een point-in-time-logboek, Ga terug naar stap 2. Deze stap herstelt een specifieke volledige of differentiële herstelpunt. U ziet alle van de volledige en differentiële herstelpunten voor de afgelopen 30 dagen. Herstelpunten ouder dan 30 dagen Selecteer **Filter** openen de **Filter herstelpunten** menu. Voor een differentiële herstelpunt, Azure Backup eerst de juiste volledig herstelpunt hersteld en is van toepassing de geselecteerde differentiële herstelpunt.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In de **herstelpunt selecteren** selecteren in het menu **volledig en differentieel**.

       ![Volledige en differentiële selecteren](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Het menu ziet u de lijst met beschikbare herstelpunten.

    2. Selecteer een herstelpunt in de lijst en selecteer **OK** om het herstelpunt procedure te voltooien.

        ![Een volledig herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        De **herstelpunt** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

        ![Menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Op de **Advanced Configuration** menu:

        * Instellen om te voorkomen dat de database niet-operationeel na het herstel, **herstellen met GEENHERSTEL** naar **ingeschakeld**. **Herstellen met GEENHERSTEL** is standaard uitgeschakeld.
        * Als u wilt wijzigen van de locatie voor terugzetten op de doelserver, voert u een nieuw pad in de **doel** kolom.
        * Selecteer **OK** goed te keuren van de instellingen. Sluit de **Advanced Configuration** menu.

    4. Op de **herstellen** in het menu **herstellen** de hersteltaak starten. De voortgang bijhouden van het herstel in de **meldingen** gebied of door te selecteren **hersteltaken** in het menu van de database.

       ![Taakvoortgang herstellen](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Bewerkingen van de Azure Backup voor SQL op Azure Virtual machines beheren

In deze sectie bevat informatie over de verschillende beheerbewerkingen voor back-up van Azure die beschikbaar voor SQL op Azure virtual machines zijn. De volgende bewerkingen uit op hoog niveau bestaat:

* Taken controleren
* Back-upwaarschuwingen
* Stop de beveiliging op een SQL-database
* Beveiliging van een SQL-database hervatten
* Een ad-hoc back-uptaak activeert
* Een server waarop SQL Server wordt de registratie ongedaan maken

### <a name="monitor-backup-jobs"></a>Back-uptaken controleren
Azure Backup is een bedrijfsoplossing van de klasse die voorziet in geavanceerde back-waarschuwingen en meldingen fouten. (Zie [back-waarschuwingen weergeven](backup-azure-sql-database.md#view-backup-alerts).) Gebruik een van de volgende opties op basis van uw vereisten voor het bewaken van specifieke taken.

#### <a name="use-the-azure-portal-for-adhoc-operations"></a>De Azure-portal gebruiken voor ad-hoc-bewerkingen
Azure Backup bevat alle handmatig geactiveerde of ad-hoc, taken de **back-uptaken** portal. De taken die beschikbaar zijn in de **back-uptaken** portal omvatten:
- Alle configureren back-upbewerkingen.
- Back-upbewerkingen handmatig worden geactiveerd.
- Herstelbewerkingen.
- Registratie en databasebewerkingen te detecteren.
- Back-upbewerkingen stoppen.

![Back-uptaken-portal](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Alle geplande back-uptaken, met inbegrip van de volledige, differentiële en logboekback-up, worden niet weergegeven in de **back-uptaken** portal. SQL Server Management Studio gebruiken voor het bewaken van de geplande back-uptaken, zoals beschreven in de volgende sectie.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>SQL Server Management Studio gebruiken voor back-uptaken
Azure Backup gebruikt systeemeigen SQL-API's voor alle back-upbewerkingen. De systeemeigen API's gebruiken om op te halen van alle taakgegevens van de [SQL-tabel op de back-upset](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in de msdb-database.

Het volgende voorbeeld wordt een query waarmee alle back-uptaken voor een database met de naam worden opgehaald **DB1**. De query voor geavanceerde bewaking aanpassen.

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

### <a name="view-backup-alerts"></a>Back-waarschuwingen weergeven

Omdat logboekback-ups worden uitgevoerd om de 15 minuten af en toe, back-uptaken controleren kan omslachtig zijn. Azure Backup biedt hulp bij deze situatie. E-mailwaarschuwingen worden geactiveerd voor alle mislukte back-ups. Waarschuwingen worden geconsolideerd op het databaseniveau van de-foutcode. Een e-mailmelding verzonden alleen voor de eerste back-upfouten voor een database. Meld u aan de Azure-portal voor het bewaken van alle fouten voor een database.

Voor het bewaken van back-upwaarschuwingen:

1. Aanmelden bij uw Azure-abonnement in de [Azure-portal](https://portal.azure.com).

2. Open de Recovery Services-kluis die geregistreerd bij de virtuele machine van SQL.

3. Op de **Recovery Services-kluis** dashboard, selecteer **waarschuwingen en gebeurtenissen**.

   ![Waarschuwingen en gebeurtenissen selecteren](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Op de **waarschuwingen en gebeurtenissen** in het menu **waarschuwingen voor back-up** om de lijst met waarschuwingen weer te geven.

   ![Selecteer de back-Upwaarschuwingen](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-for-a-sql-server-database"></a>Stop de beveiliging voor een SQL Server-database

Wanneer u beveiliging voor een SQL Server-database stopt, vraagt Azure back-up of de herstelpunten behouden. Er zijn twee manieren om stop de beveiliging voor een SQL-database:

* Alle toekomstige back-uptaken stoppen en verwijderen van alle herstelpunten.
* Alle toekomstige back-uptaken stoppen maar laat u de herstelpunten.

Als u back-up stoppen met behoud van gegevens, herstelpunten worden verwijderd volgens het back-upbeleid. U kunt het beveiligde exemplaar van SQL prijzen kosten in rekening gebracht, plus de opslag verbruikt totdat alle herstelpunten worden schoongemaakt wordt gebracht. Zie voor meer informatie over prijzen van Azure Backup voor SQL, de [pagina met prijzen voor back-up in Azure](https://azure.microsoft.com/pricing/details/backup/).

Beveiliging voor een database stoppen:

1. Open de Recovery Services-kluis die geregistreerd bij de virtuele machine van SQL.

2. Op de **Recovery Services-kluis** dashboard onder **gebruik**, selecteer **back-Upitems** openen de **back-Upitems** menu.

    ![Open het menu back-Upitems](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Op de **back-Upitems** menu onder **Type back-upbeheer**, selecteer **SQL in Azure VM**.

    ![Selecteer SQL in Azure-VM](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    De **back-Upitems** menu bevat een lijst van de SQL-databases.

4. Selecteer in de lijst met SQL-databases, stop de beveiliging van de database.

    ![Selecteer de database waarmee beveiliging stoppen](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt het menu wordt geopend.

5. Selecteer in het menu voor de geselecteerde database **back-up stoppen**.

    ![Selecteer de back-up stoppen](./media/backup-azure-sql-database/stop-db-button.png)

    De **back-up stoppen** menu wordt geopend.

6. Op de **back-up stoppen** in het menu wilt **back-upgegevens behouden** of **back-upgegevens verwijderen**. Geef een reden op voor het stoppen van de beveiliging en een opmerking als een optie.

    ![Menu back-up stoppen](./media/backup-azure-sql-database/stop-backup-button.png)

7. Selecteer **back-up stoppen** beveiliging stoppen op de database.

  Houd er rekening mee dat **back-up stoppen** optie werkt niet voor een database in een exemplaar automatisch wordt beveiligd. De enige manier om te stoppen met het beveiligen van deze database is de automatische beveiliging op het exemplaar van de tijd die uitschakelen en kies vervolgens de **back-up stoppen** onder de optie **back-Upitems** voor die database.  

  U kunt de automatische beveiliging op een exemplaar of de Always On availability group onder uitschakelen **back-up configureren**. Klik op de naam van het exemplaar te openen van het paneel aan de rechterkant informatie die is **uitschakelen Autoprotect** in de rechterbovenhoek. Klik op **uitschakelen Autoprotect** uitschakelen van automatische beveiliging op dat exemplaar.

    ![Schakel de beveiliging automatisch voor dat exemplaar](./media/backup-azure-sql-database/disable-auto-protection.png)

Alle databases in dat geval blijven worden beveiligd. Deze actie wordt echter uitgeschakeld. automatische beveiliging voor alle databases die in de toekomst worden toegevoegd.

Nadat u automatische beveiliging hebt uitgeschakeld, kunt u **back-up stoppen** voor de database onder **back-Upitems**. Het exemplaar kan opnieuw worden ingeschakeld voor automatische beveiliging nu.

### <a name="resume-protection-for-a-sql-database"></a>Beveiliging van een SQL-database hervatten

Als de **back-upgegevens behouden** optie is geselecteerd wanneer beveiliging voor de SQL-database is gestopt, kunt u de beveiliging hervatten. Als de back-upgegevens is niet aanwezig zijn, kan beveiliging niet hervatten.

1. Als u wilt hervatten beveiliging voor de SQL-database, de back-upitem te openen en selecteer **back-up hervatten**.

    ![Selecteer de back-up hervatten Hervatten van databasebeveiliging](./media/backup-azure-sql-database/resume-backup-button.png)

   De **back-upbeleid** menu wordt geopend.

2. Op de **back-upbeleid** in het menu selecteert u een beleid en selecteer vervolgens **opslaan**.

### <a name="trigger-an-adhoc-backup"></a>Een ad-hoc back-up activeren

Ad-hoc back-ups niet starten omdat die nodig zijn. Er zijn vier typen ad-hoc back-ups:

* Volledige back-up
* Kopie-alleen volledige back-up van
* Differentiële back-up
* Logboekback-up

Zie voor meer informatie over elk type, [typen van de SQL-back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

### <a name="unregister-a-sql-server-instance"></a>Registratie van een SQL Server-exemplaar

Registratie van een exemplaar van SQL Server na het verwijderen van beveiliging, maar voordat u de kluis verwijderen:

1. Open de Recovery Services-kluis die geregistreerd bij de virtuele machine van SQL.

2. Op de **Recovery Services-kluis** dashboard onder **beheren**, selecteer **back-upinfrastructuur**.  

   ![Selecteer een back-upinfrastructuur](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. Onder **beheerservers**, selecteer **beschermde Servers**.

   ![Beveiligde Servers selecteren](./media/backup-azure-sql-database/protected-servers.png)

    De **beschermde Servers** menu wordt geopend.

4. Op de **beschermde Servers** in het menu selecteert u de server registratie ongedaan te maken. Als u wilt verwijderen van de kluis, moet u de registratie van alle servers ongedaan maken.

5. Op de **beschermde Servers** in het menu met de rechtermuisknop op de beveiligde server en selecteer **verwijderen**.

   ![Selecteer verwijderen](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="faq"></a>Veelgestelde vragen

De volgende sectie bevat aanvullende informatie over back-up van SQL database.

### <a name="can-i-throttle-the-speed-of-the-sql-server-backup-policy"></a>Kan ik de snelheid van het SQL Server-back-upbeleid beperken?

Ja. De snelheid waarmee het back-upbeleid voor het minimaliseren van de impact op een exemplaar van SQL Server wordt uitgevoerd, kunt u beperken.

De instelling te wijzigen:

1. Open op de SQL Server-exemplaar in de map C:\Program Files\Azure werkbelasting Backup\bin de **TaskThrottlerSettings.json** bestand.

2. Wijzig in het bestand TaskThrottlerSettings.json de **DefaultBackupTasksThreshold** instellen op een lagere waarde (bijvoorbeeld 5).

3. Sla uw wijzigingen op. Sluit het bestand.

4. Open op de SQL Server-exemplaar **Taakbeheer**. Start opnieuw op de **Service van Azure-back-Workload Coordinator**.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kan ik een volledige back-up van een secundaire replica uitvoeren?

Nee. Deze functie wordt niet ondersteund.

### <a name="do-successful-backup-jobs-create-alerts"></a>Waarschuwingen in de geslaagde back-uptaken maken?

Nee. Geslaagde back-uptaken geen waarschuwingen genereren. Waarschuwingen worden alleen voor back-uptaken die niet voldoen aan verzonden.

### <a name="can-i-see-details-for-scheduled-backup-jobs-in-the-jobs-menu"></a>Kan ik details voor geplande back-uptaken in het menu taken zien?

Nee. De **back-uptaken** menu ziet u details van de ad-hoc-taak, maar geen geplande back-uptaken. Als een geplande back-uptaken mislukken, zijn de details beschikbaar in de mislukte taak waarschuwingen. Voor het bewaken van alle geplande en ad-hoc back-uptaken, gebruik [SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="when-i-select-a-sql-server-instance-are-future-databases-automatically-added"></a>Wanneer ik een exemplaar van SQL Server selecteert, worden toekomstige databases automatisch toegevoegd?

Nee. Wanneer u de beveiliging voor een SQL Server-exemplaar configureren als u de optie voor de server selecteert, worden alle databases toegevoegd. Als u databases aan een exemplaar van SQL Server toevoegen na het configureren van beveiliging, moet u de nieuwe databases om ze te beveiligen handmatig toevoegen. De databases worden niet automatisch opgenomen in de geconfigureerde beveiliging.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Als ik het herstelmodel wijzigt, hoe ik opnieuw beveiliging?

Activeer een volledige back-up. Logboekback-ups begint zoals verwacht.

### <a name="can-i-protect-sql-always-on-availability-groups-where-the-primary-replica-is-on-premises"></a>Kan ik SQL Always On Availability Groups waar de primaire replica on-premises is beveiligen?

Nee. Azure Backup beschermt SQL-Servers die worden uitgevoerd in Azure. Als een beschikbaarheid van groep (AG) wordt verdeeld tussen Azure en on-premises machines, kan de AG worden beveiligd, alleen als de primaire replica wordt uitgevoerd in Azure. Azure Backup beveiligt alleen de knooppunten die worden uitgevoerd in dezelfde Azure-regio als de Recovery Services-kluis.

### <a name="can-i-protect-sql-always-on-availability-groups-which-are-spread-across-azure-regions"></a>Kan ik beveiligen met SQL Always On Availability Groups die worden verdeeld over Azure-regio's?

Azure Backup Recovery Services-kluis kunt detecteren en alle knooppunten die zich in dezelfde regio als de Recovery Services-kluis beveiligen. Als u een SQL altijd op beschikbaarheidsgroep die meerdere Azure-regio's omvatten hebt, moet u back-up van de regio waaraan het primaire knooppunt configureren. Azure Backup is mogelijk om te detecteren en beveiligen van alle databases in de beschikbaarheidsgroep aan de hand van back-upvoorkeur. Als de back-upvoorkeur niet wordt voldaan, back-ups mislukken en krijgt u de waarschuwing voor fout.

### <a name="while-i-want-to-protect-most-of-the-databases-in-an-instance-i-would-like-to-exclude-a-few-is-it-possible-to-still-use-the-auto-protection-feature"></a>Terwijl ik wil de meeste van de databases in een exemplaar beveiligen, moet ik wil graag een paar uitsluiten. Is het mogelijk nog steeds gebruik van de functie Automatische beveiliging?

Nee, automatische beveiliging is van toepassing op het volledige-exemplaar. U kunt niet selectief beveiligen databases een exemplaar met behulp van automatische beveiliging.

### <a name="can-i-have-different-policies-for-different-databases-in-an-auto-protected-instance"></a>Kan ik verschillende beleidsregels voor verschillende databases hebben in een exemplaar automatisch wordt beveiligd?

Als u al enkele beveiligde databases in een exemplaar hebt, blijven deze moet worden beveiligd met hun beleid, zelfs nadat u de **ON** de optie Automatische beveiliging. Alle niet-beveiligde databases samen met degene die u in de toekomst zou toevoegen heeft echter slechts één beleid die u definieert onder **back-up configureren** nadat de databases zijn geselecteerd. In feite, in tegenstelling tot andere beveiligde databases wijzigen u niet ook het beleid voor een database onder een exemplaar automatisch wordt beveiligd.
Als u doen wilt, is de enige manier om te schakelen de automatische beveiliging op de instantie voor het voorlopig aanwezig en wijzig vervolgens het beleid voor die database. U kunt nu automatische beveiliging voor dit exemplaar opnieuw inschakelen.

### <a name="if-i-delete-a-database-from-an-auto-protected-instance-will-the-backups-for-that-database-also-stop"></a>Als ik een database niet uit een exemplaar automatisch wordt beveiligd verwijderen, wordt de back-ups voor die database ook stoppen?

Nee, als een database wordt verwijderd uit een exemplaar automatisch wordt beveiligd, worden de back-ups op dat de database nog steeds uitgevoerd. Dit betekent dat de verwijderde database moeten worden weergegeven als niet in orde onder begint **back-Upitems** en nog steeds wordt behandeld als beveiligd.

De enige manier om te stoppen met het beveiligen van deze database is de automatische beveiliging op het exemplaar van de tijd die uitschakelen en kies vervolgens de **back-up stoppen** onder de optie **back-Upitems** voor die database. U kunt nu automatische beveiliging voor dit exemplaar opnieuw inschakelen.

###  <a name="why-cant-i-see-the-newly-added-database-to-an-auto-protected-instance-under-the-protected-items"></a>Waarom zie ik de zojuist toegevoegde database naar een exemplaar automatisch wordt beveiligd onder de beveiligde items niet?

Mogelijk ziet u niet een nieuw toegevoegde database naar een direct beveiligd automatisch beveiligd exemplaar. Dit is omdat de detectie wordt doorgaans uitgevoerd om de 8 uur. De gebruiker kan echter uitvoeren een handmatige detectie met **databases herstellen** optie om te detecteren en bescherming van nieuwe databases onmiddellijk zoals wordt weergegeven in de onderstaande afbeelding:

  ![Toegevoegde Database weergeven](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Backup, de Azure PowerShell-voorbeeld op de back-ups maken van versleutelde virtuele machines.

> [!div class="nextstepaction"]
> [Back-up van een versleutelde VM](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
