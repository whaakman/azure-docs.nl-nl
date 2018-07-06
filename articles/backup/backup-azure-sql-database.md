---
title: Maak een back-up van SQL Server-database naar Azure | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd van SQL Server back-up naar Azure. Het artikel wordt ook uitgelegd voor herstel van SQL Server.
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
ms.date: 7/04/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 13876991583292ec04120b9d59fb150ad236e864
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858558"
---
# <a name="back-up-sql-server-database-in-azure"></a>Back-up van SQL Server-database in Azure

SQL Server-databases zijn essentiële workloads waarvoor lage Recovery Point Objective (RPO) en langdurige bewaarperioden vereist. Azure Backup biedt een oplossing voor SQL Serverbackup waarvoor geen infrastructuur, wat betekent dat er geen back-upserver van complexe, geen beheeragent of back-upopslag voor het beheren van. Azure Backup biedt gecentraliseerd beheer voor uw back-ups voor alle SQL-servers, of zelfs in verschillende workloads.

 In dit artikel leert u:

> [!div class="checklist"]
> * Vereisten voor het back-up van SQL Server naar Azure
> * Maken en gebruiken van een Recovery Services-kluis
> * Back-ups van SQL Server-database configureren
> * Een beleid voor back-up (of behoud) voor de herstelpunten instellen
> * De database herstellen

Voordat u begint met de procedures in dit artikel, moet u een SQL-Server die wordt uitgevoerd in Azure hebt. [U kunt SQL marketplace virtuele machines snel maken van een SQL-Server](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Beperkingen voor openbare Preview

De volgende items zijn de bekende beperkingen voor de openbare Preview.

- De SQL-virtuele machine vereist verbinding met internet voor toegang tot Azure openbare IP-adressen. Voor meer informatie, Zie de sectie [netwerkverbinding tot stand brengen](backup-azure-sql-database.md#establish-network-connectivity).
- U kunt maximaal 2000 SQL-databases in een Recovery Services-kluis beveiligen. Aanvullende SQL-databases moeten worden opgeslagen in een afzonderlijke Recovery Services-kluis.
- [Back-up van gedistribueerde beschikbaarheidsgroepreplica groepen heeft beperkingen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- SQL Failover Cluster Instances (FCI) worden niet ondersteund.
- De Azure portal gebruiken voor Azure Backup configureren om het beveiligen van SQL Server-databases. Ondersteuning voor Azure PowerShell, CLI en REST-API's is momenteel niet beschikbaar.

## <a name="supported-azure-geos"></a>Ondersteunde geografische gebieden voor Azure

- Australië - zuidoost (ASE) 
- Brazilië - zuid (BRS)
- Canada centraal (CNC)
- Canada - oost (CE)
- VS - centraal (CUS)
- Oost-Azië (EA)
- Oost-Australië (AE) 
- VS - oost (EUS)
- VS - oost 2 (EUS2)
- Japan - oost (JPE)
- Japan - west (JPW)
- India - centraal (INC) 
- India - zuid (INS)
- Korea - centraal (KRC)
- Korea - zuid (KRS)
- VS - noord centraal (NCUS) 
- Noord-Europa (NE) 
- VS - zuid-centraal (SCUS) 
- Zuidoost-Azië (SEA)
- Verenigd Koninkrijk zuid (UKS) 
- Verenigd Koninkrijk west (UKW) 
- West-Europa (WE) 
- VS - west (WUS)
- VS - west-centraal (WCUS)
- VS - west 2 (WUS 2) 

## <a name="supported-operating-systems-and-versions-of-sql-server"></a>Ondersteunde besturingssystemen en versies van SQL server

De volgende ondersteunde besturingssystemen en versies van SQL Server van toepassing op SQL marketplace virtuele machines van Azure en niet-marketplace virtuele machines (waarop SQL Server is handmatig geïnstalleerd).

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux is momenteel niet ondersteund.

### <a name="supported-versionseditions-of-sql-server"></a>Ondersteunde versies/edities van SQL Server

- SQL 2012 Enterprise, Standard, Web, Developer, Express
- SQL 2014 Enterprise, Standard, Web, Developer, Express
- SQL 2016 Enterprise, Standard, Web, Developer, Express
- SQL 2017 Enterprise, Standard, Web, Developer, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>Vereisten voor het gebruik van Azure Backup voor SQL Server beveiligen 

Voordat u kunt back-up van SQL Server-database, controleert u de volgende voorwaarden. :

- Bepalen of [maken van een Recovery Services-kluis](backup-azure-sql-database.md#create-a-recovery-services-vault) in dezelfde regio, of landinstellingen, als de virtuele machine hosten van SQL Server.
- [Controleer de machtigingen voor de virtuele machine](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) die nodig zijn voor de back-up van SQL-databases.
- [SQL-VM heeft een netwerkverbinding](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> U kunt slechts één back-upoplossing hebben op een moment naar de back-up van SQL Server-databases. Schakel alle andere SQL-back-voordat u deze functie, anders back-ups zal leiden tot problemen en mislukken. U kunt inschakelen Azure back-up voor IaaS-VM samen met de SQL-back-up zonder een conflict 
>

Als deze voorwaarden aanwezig zijn in uw omgeving, gaat u verder naar de sectie [uw kluis ter bescherming van een SQL-database configureren](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database). Als een van de vereiste nog niet bestaan, blijven lezen van deze sectie.


## <a name="establish-network-connectivity"></a>De netwerkverbinding tot stand brengen

Voor alle bewerkingen moet de SQL-virtuele machine verbinding met Azure openbare IP-adressen. SQL VM-bewerkingen (zoals databasedetectie, configureren back-up, geplande back-ups, herstel herstelpunten, enzovoort) is mislukt zonder verbinding met het openbare IP-adressen. Gebruik een van de volgende opties voor een duidelijk pad voor back-upverkeer:

- De Azure-datacenter-IP-bereiken van geaccepteerde - aan lijst met geaccepteerde Azure datacenter IP-bereiken, gebruikt u de [center downloadpagina voor meer informatie over de IP-bereiken en instructies](https://www.microsoft.com/download/details.aspx?id=41653). 
- Een HTTP-proxy-server voor het routeren van verkeer implementeren - wanneer u een back-up van een SQL-database in een virtuele machine, de back-upextensie op de virtuele machine maakt gebruik van HTTPS-API's voor het verzenden van opdrachten voor beheer met Azure Backup en gegevens naar Azure Storage. De Backup-extensie gebruikt ook Azure Active Directory (AAD) voor verificatie. De back-upextensie verkeer omleiden voor deze drie services via de HTTP-proxy, omdat dit het enige onderdeel dat is geconfigureerd voor toegang tot het openbare internet.

De balans tussen de opties zijn: beheerbaarheid, gedetailleerde controle en kosten.

> [!NOTE]
>Servicetags voor Azure Backup moet beschikbaar zijn op algemene beschikbaarheid.
>

| Optie | Voordelen | Nadelen |
| ------ | ---------- | ------------- |
| Whitelist IP-bereiken | Er zijn geen extra kosten. <br/> Gebruiken om toegang te openen in een NSG, **Set AzureNetworkSecurityRule** cmdlet. | Moeilijk te beheren als de betrokken IP-adresbereiken verloop van tijd veranderen. <br/>Biedt toegang tot het geheel van Azure, niet alleen opslag.|
| Gebruik een HTTP-proxy   | Nauwkeurige controle in de proxy die over de opslag URL's is toegestaan. <br/>Één punt van internet toegang tot VM's. <br/> Niet onderhevig aan Azure-IP-adres verandert. | Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Machtigingen instellen voor niet-marketplace SQL-VM 's

Als u wilt back-up van een virtuele machine, Azure Backup moet de **AzureBackupWindowsWorkload** extensie worden geïnstalleerd. Als u van virtuele machines van Azure marketplace gebruikmaakt, gaat u verder met [detecteren SQL server-databases](backup-azure-sql-database.md#discover-sql-server-databases). Als de virtuele machine die als host fungeert voor uw SQL-databases niet vanuit de Azure marketplace gemaakt is, voert u de volgende sectie voor het installeren van de extensie en stel de juiste machtigingen. Naast de **AzureBackupWindowsWorkload** SQL sysadmin-bevoegdheden in SQL-databases beveiligen door Azure Backup-extensie is vereist. Tijdens het ontdekken van databases op de virtuele machine, maakt Azure Backup een NT-Service\AzureWLBackupPluginSvc-account. Voor Azure Backup voor het detecteren van SQL-databases, moet het account NT Service\AzureWLBackupPluginSvc logboek in SQL en SQL sysadmin-bevoegdheden hebben. De volgende procedure wordt uitgelegd hoe u deze machtigingen leveren.

Om machtigingen te configureren:

1. In de [Azure-portal](https://portal.azure.com), opent u de Recovery Services-kluis die u gebruikt voor het beveiligen van SQL-databases.
2. Klik in het menu kluis dashboard **+ back-up** te openen de **back-updoel** menu.

   ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Op de **back-updoel** menu in de **waar wordt uw werkbelasting uitgevoerd?** laat in het menu **Azure** als standaardwaarde.

4. Op de **waarvan wilt u back-up maken** menu, vouw de vervolgkeuzelijst en selecteer **SQL Server in virtuele Azure-machine**.

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    De **back-updoel** menu u ziet nu twee nieuwe stappen: **DB's detecteren in VM's** en **back-up configureren**. **DB's detecteren in VM's** Hiermee start u een zoekopdracht voor virtuele machines van Azure.

    ![Ziet u de stappen voor het doel van nieuwe back-up](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Klik op **detectie starten** om te zoeken naar niet-beveiligde virtuele machines in het abonnement. Afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement duurt het even om te gaan via alle virtuele machines.

    ![Back-up in behandeling](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Zodra een niet-beveiligde virtuele machine wordt gedetecteerd, wordt deze weergegeven in de lijst. Niet-beveiligde virtuele machines worden weergegeven door de virtuele machine en de bron-groep. Het is mogelijk voor meerdere virtuele machines met dezelfde naam. Echter, virtuele machines met dezelfde naam behoren tot verschillende resourcegroepen bevinden. Als een verwachte virtuele machine niet wordt weergegeven in de lijst, kunt u zien als de virtuele machine is al worden beveiligd door een kluis.

6. Selecteer in de lijst met virtuele machines, de virtuele machine met de SQL-database die u wilt back-up maken en klikt u op **DB's detecteren**. 

    Het detectieproces installeert de **AzureBackupWindowsWorkload** -extensie op de virtuele machine. De extensie kan de Azure Backup-service om te communiceren met de virtuele machine, zodat het back-up van de SQL-databases. Nadat de extensie wordt geïnstalleerd, Azure back-up maakt het virtuele serviceaccount van Windows **NT Service\AzureWLBackupPluginSvc**, op de virtuele machine. Het virtuele serviceaccount vereist SQL sysadmin-machtiging. Tijdens het virtual service account installatieproces, als de fout wordt weergegeven, **UserErrorSQLNoSysadminMembership**, Zie de sectie [vaststelling SQL sysadmin-bevoegdheden](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    Het meldingengebied toont de voortgang van de databasedetectie. Afhankelijk van hoeveel databases zich op de virtuele machine, duurt het even voor de taak is voltooid. Wanneer de geselecteerde databases zijn gedetecteerd, wordt er een bericht weergegeven.

    ![melding implementatie is voltooid](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nadat u de database aan de Recovery Services-kluis koppelen, de volgende stap is het [configureren van de back-up](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>SQL sysadmin-rechten oplossen

Tijdens het installatieproces, als de fout wordt weergegeven, **UserErrorSQLNoSysadminMembership**, meld u in SQL Server Management Studio (SSMS) met een account dat SQL sysadmin-machtiging heeft. Tenzij u speciale machtigingen nodig hebt, zou het mogelijk voor het gebruik van Windows-verificatie voor het herkennen van het account.

1. Open op de SQL Server, de **beveiliging/aanmeldingen** map.

    ![De mappen van SQL Server en beveiliging en meld u aan om te zien van accounts openen](./media/backup-azure-sql-database/security-login-list.png)

2. Klik met de rechtermuisknop op de map aanmeldingen en selecteer **nieuwe aanmelding**, en in de aanmelding - nieuw dialoogvenster, klikt u op **zoeken**

    ![Zoeken in de aanmelding - nieuw dialoogvenster geopend](./media/backup-azure-sql-database/new-login-search.png)

3. Nadat het virtuele serviceaccount van Windows **NT Service\AzureWLBackupPluginSvc** al is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectiefase, geef de accountnaam op die wordt weergegeven in de  **Voer de naam van het object te selecteren** dialoogvenster. Klik op **namen controleren** om de naam te herleiden. 

    ![Klik op de knop Namen controleren om op te lossen van de naam van de onbekende service](./media/backup-azure-sql-database/check-name.png)

4. Klik op **OK** om het dialoogvenster gebruiker of groep selecteren te sluiten.

5. In de **serverfuncties** dialoogvenster, zorg ervoor dat de **sysadmin** rol is geselecteerd. Klik vervolgens op **OK** sluiten **aanmelding - nieuw**.

    ![Zorg ervoor dat de serverrol sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

    De vereiste machtigingen moeten nu bestaan.

6. Hoewel u de Machtigingsfout hebt opgelost, moet u nog steeds de database koppelen aan de Recovery Services-kluis. In de Azure-portal **beschermde Servers** lijst, klik met de rechtermuisknop op de server in de fout en selecteer **DB's opnieuw detecteren**.

    ![Controleer of dat de server heeft de juiste machtigingen](./media/backup-azure-sql-database/check-erroneous-server.png)

    Het meldingengebied toont de voortgang van de databasedetectie. Afhankelijk van hoeveel databases zich op de virtuele machine, duurt het even voor de taak is voltooid. Wanneer geselecteerde databases zijn gevonden, wordt een bericht weergegeven in het meldingengebied weergegeven.

    ![melding implementatie is voltooid](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nadat u de database aan de Recovery Services-kluis koppelen, de volgende stap is het [configureren van de back-up](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detecteren van SQL Server-databases

Azure Backup kan alle databases op een exemplaar van SQL Server detecteren, zodat u ze per uw back-upvereisten kunt beveiligen. Gebruik de volgende procedure voor het identificeren van de virtuele machine die als host fungeert voor de SQL-databases. Wanneer u de virtuele machine hebt geïdentificeerd, wordt een lichtgewicht-extensie voor het detecteren van de SQL server-databases in Azure Backup geïnstalleerd.

1. Meld u aan uw abonnement in de [Azure-portal](https://portal.azure.com/).
2. Selecteer in het menu links **alle Services**.

    ![Kies de optie alle Services in in het hoofdmenu](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Typ in het dialoogvenster alle *herstelservices*. Als u te typen begint, filtert de invoer van de lijst met resources. Als u dit ziet, selecteert u **Recovery Services-kluizen**.

    ![Recovery Services typt in het dialoogvenster voor alle services](./media/backup-azure-sql-database/all-services.png) <br/>

    De lijst met Recovery Services-kluizen in het abonnement wordt weergegeven. 

4. Selecteer in de lijst met Recovery Services-kluis, de kluis die u wilt gebruiken voor het beveiligen van uw SQL-databases.

5. Klik in het menu kluis dashboard **+ back-up** te openen de **back-updoel** menu.

   ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/open-backup-menu.png)

6. Op de **back-updoel** menu in de **waar wordt uw werkbelasting uitgevoerd?** laat in het menu **Azure** als standaardwaarde.

7. Op de **waarvan wilt u back-up maken** menu, vouw de vervolgkeuzelijst en selecteer **SQL Server in virtuele Azure-machine**.

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Eenmaal geselecteerd, de **back-updoel** menu u ziet nu twee stappen: DB's detecteren in VM's en back-up configureren. 

    ![Ziet u de stappen voor het doel van nieuwe back-up](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Klik op **detectie starten** om te zoeken naar niet-beveiligde virtuele machines in het abonnement. Afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement duurt het even om te gaan via alle virtuele machines.

    ![Back-up in behandeling](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Zodra een niet-beveiligde virtuele machine wordt gedetecteerd, wordt deze weergegeven in de lijst. Meerdere virtuele machines hebben dezelfde naam. Echter, meerdere virtuele machines met dezelfde naam behoren tot verschillende resourcegroepen bevinden. De niet-beveiligde virtuele machines worden weergegeven door de virtuele machine en de bron-groep. Als een verwachte virtuele machine niet wordt vermeld, kunt u zien als die virtuele machine is al worden beveiligd door een kluis.

9. In de lijst met virtuele machines, schakelt u het selectievakje van de virtuele machine met de SQL-databases die u wilt beveiligen, en klikt u op **DB's detecteren**.

    Azure Backup worden alle SQL-databases op de virtuele machine gedetecteerd. Zie voor informatie over wat er tijdens de detectiefase van de database gebeurt de volgende sectie, [back-end-bewerkingen bij het detecteren van SQL-databases](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). Na het detecteren van de SQL-databases, bent u klaar om [configureren van de back-uptaak](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>Back-end-bewerkingen bij het detecteren van SQL-databases

Wanneer u gebruikt de **DB's detecteren** hulpprogramma voor Azure Backup wordt de volgende bewerkingen uitgevoerd op de achtergrond:

- Hiermee registreert de virtuele machine met de Recovery Services-kluis voor back-up werkbelasting. Alle databases op de geregistreerde virtuele machine kunnen alleen worden back-ups op deze Recovery Services-kluis. 

- installeert de **AzureBackupWindowsWorkload** -extensie op de virtuele machine. Back-ups van een SQL-database is een oplossing voor zonder agent, dat wil zeggen, geen agent met de extensie is geïnstalleerd op de virtuele machine, is geïnstalleerd op de SQL-database.

- Hiermee maakt u het serviceaccount **NT Service\AzureWLBackupPluginSvc**, op de virtuele machine. Alle bewerkingen voor back-up en herstel de serviceaccount gebruiken. **NT-Service\AzureWLBackupPluginSvc** SQL sysadmin-machtigingen hebben. Alle virtuele machines van de SQL-Marketplace worden geleverd met de SqlIaaSExtension geïnstalleerd en AzureBackupWindowsWorkload SQLIaaSExtension gebruikt om automatisch vereiste machtigingen. Als uw virtuele machine geen SqlIaaSExtension geïnstalleerd, de bewerking DB detecteren mislukt en u de foutmelding krijgt **UserErrorSQLNoSysAdminMembership**. Als u wilt toevoegen de sysadmin-machtigingen voor back-up, volg de instructies in [instellen van Azure Backup-machtigingen voor niet-marketplace SQL-VM's](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![Selecteer de virtuele machine en de database](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>Back-up voor SQL Server-database configureren

Azure Backup biedt beheerservices voor het beveiligen van uw SQL Server-databases en back-uptaken beheren. Het beheer en de mogelijkheden voor bewaking zijn afhankelijk van uw Recovery Services-kluis. 

> [!NOTE]
> U kunt slechts één back-upoplossing hebben op een moment naar de back-up van SQL Server-databases. Schakel alle andere SQL-back-voordat u deze functie, anders back-ups zal leiden tot problemen en mislukken. U kunt inschakelen Azure back-up voor IaaS-VM samen met de SQL-back-up zonder een conflict 
>

Beveiliging voor uw SQL-database configureren:

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. Klik in het menu kluis dashboard **+ back-up** te openen de **back-updoel** menu.

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Op de **back-updoel** menu in de **waar wordt uw werkbelasting uitgevoerd?** laat in het menu **Azure** als standaardwaarde.

4. Op de **waarvan wilt u back-up maken** menu, vouw de vervolgkeuzelijst en selecteer **SQL Server in virtuele Azure-machine**.

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Eenmaal geselecteerd, de **back-updoel** menu u ziet nu twee stappen: DB's detecteren in VM's en back-up configureren. Als u dit artikel in volgorde hebt doorlopen, u hebt al ontdekt de niet-beveiligde virtuele machines en deze kluis is geregistreerd bij een virtuele machine. U bent nu klaar om te configureren van beveiliging voor de SQL-databases.

5. Klik in het menu back-updoel **back-up configureren**.

    ![Ziet u de stappen voor het doel van nieuwe back-up](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    De Azure Backup-service wordt weergegeven voor alle SQL-exemplaren met zelfstandige databases, evenals SQL AlwaysOn-beschikbaarheidsgroepen. Als u wilt de zelfstandige databases weergeven in het SQL-exemplaar, klikt u op de pijl-omlaag naast de naam van het exemplaar om de databases weer te geven. De volgende afbeeldingen ziet u voorbeelden van een zelfstandig exemplaar en een AlwaysOn-beschikbaarheidsgroep.

    > [!NOTE]
    > In het geval van SQL AlwaysOn-beschikbaarheidsgroep, we voldoen aan de SQL-back-upvoorkeur. Maar vanwege een platformbeperking SQL volledige en differentiële back-ups moeten worden uitgevoerd in het primaire knooppunt. Logboekback-up kan gebeuren op basis van uw voorkeur back-up. Vanwege deze beperking, moet het primaire knooppunt altijd worden geregistreerd voor beschikbaarheidsgroepen.
    >

    ![Lijst met databases in SQL-exemplaar](./media/backup-azure-sql-database/discovered-databases.png)

    Klik op de pijl-omlaag naast de AlwaysOn-beschikbaarheidsgroepen om de lijst met databases weer te geven.

    ![Lijst met databases in de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Selecteer in de lijst van databases, alles wat u wilt beveiligen, en klikt u op **OK**.

    ![Selecteer meerdere databases volgen](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    U kunt maximaal 50 databases in één keer selecteren. Als u meer dan 50 databases beveiligen wilt, moet u meerdere fasen. Nadat u de eerste 50 databases beveiligen, Herhaal deze stap ter bescherming van de volgende set van databases.
    > [!Note] 
    > Voor het optimaliseren van back-belastingen, Azure Backup-einden grote back-uptaken in meerdere batches. Het maximum aantal databases in één back-uptaak is 50.
    >
    >

7. Om te maken of een back-upbeleid kiezen in de **back-up** in het menu **back-upbeleid**om het menu te openen.

    ![Selecteer de optie back-upbeleid](./media/backup-azure-sql-database/select-backup-policy.png)

8. Uit de **back-upbeleid kiezen** vervolgkeuzelijst, kiest u een back-upbeleid en op **OK**. Zie de sectie voor informatie over het maken van uw eigen back-upbeleid [een back-upbeleid definiëren](backup-azure-sql-database.md#define-a-backup-policy).

    ![een back-upbeleid kiezen uit de vervolgkeuzelijst](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    In het menu back-upbeleid van de **back-upbeleid kiezen** vervolgkeuzelijst, kunt u kiezen: 
    - het standaardbeleid HourlyLogBackup 
    - een bestaand back-upbeleid eerder hebt gemaakt voor SQL,
    - naar [definiëren een nieuw beleid](backup-azure-sql-database.md#define-a-backup-policy) op basis van uw herstelpuntdoel (RPO) en de bewaartermijn. 

    > [!Note]
    > Azure Backup ondersteunt op basis van het zogenaamde grootvader-vader-zoon back-upschema's gebruik van de back-end-opslag aan de wensen van nalevingsbehoeften optimaliseren met een langetermijnbewaarperiode.
    >

9. Nadat u een back-upbeleid hebt gekozen de **menu back-up** klikt u op **back-up inschakelen**.

    ![het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

    U kunt de voortgang van de configuratie in het meldingengebied van de portal kunt bijhouden.

    ![systeemvak weergeven](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Een back-upbeleid definiëren

Een back-upbeleid definieert een matrix met wanneer de back-ups worden gemaakt en hoe lang de back-ups worden bewaard. U kunt Azure Backup gebruiken voor het plannen van de drie typen van back-up voor SQL-databases:

* Volledige back-up - een volledige databaseback-up een back-up van de gehele database. Een volledige back-up bevat alle gegevens in een specifieke database of een set van bestanden of bestandsgroepen en genoeg logboekbestand om die gegevens te herstellen. U kunt maximaal, een volledige back-up per dag activeren. U kunt een volledige back-up te nemen in een interval van dagelijks of wekelijks. 
* Differentiële back-up - een differentiële back-up is gebaseerd op de meest recente, de vorige volledige gegevens back-up. Een differentiële back-up legt alleen de gegevens die zijn gewijzigd sinds de volledige back-up. U kunt maximaal één differentiële back-up per dag activeren. U kunt een volledige back-up en een differentiële back-up niet configureren op dezelfde dag.
* Transactielogboekback-up - kan een logboekback-up punt in tijd herstel tot een specifieke het tweede. Maximaal, kunt u configureren transactionele logboekback-ups om de 15 minuten.

Beleid wordt gemaakt op de Recovery Services-kluis niveau. Als u meerdere kluizen hebt, wordt de kluizen het dezelfde back-upbeleid kunnen gebruiken, maar u moet het back-upbeleid toepassen op elke kluis. Bij het maken van een back-upbeleid, de dagelijkse volledige back-up de standaardwaarde is. U kunt een differentiële back-up, maar alleen toevoegen als u een volledige back-ups wekelijks optreden als u wilt overschakelen. De volgende procedure wordt uitgelegd hoe u een back-upbeleid voor een SQL-server maken in een virtuele machine van Azure.

Een back-upbeleid maken

1. In het menu van het beleid voor back-up van de **back-upbeleid kiezen** vervolgkeuzelijst in het menu **nieuw**.

   ![nieuwe back-upbeleid maken](./media/backup-azure-sql-database/create-new-backup-policy.png)

    Het menu back-upbeleid wordt overgeschakeld naar de velden die nodig zijn voor een nieuw back-upbeleid van de SQL-server bieden.

   ![nieuwe back-upbeleid velden](./media/backup-azure-sql-database/blank-new-policy.png)

2. In **beleidsnaam**, Geef een naam op. 

3. Een volledige back-up is verplicht. Accepteer de standaardwaarden voor de volledige back-up of klik op **volledige back-up** om het beleid te bewerken.

    ![nieuwe back-upbeleid velden](./media/backup-azure-sql-database/full-backup-policy.png)

    Kies in het beleid volledige back-up dagelijks of wekelijks voor de frequentie. Als u dagelijks, kiest u het uur en tijdzone, wanneer de back-uptaak wordt gestart. Als u dagelijkse volledige back-ups kiest, kunt u geen differentiële back-ups maken.

   ![dagelijks interval instellen](./media/backup-azure-sql-database/daily-interval.png)

    Als u wekelijks, kiest u de dag van de week, uur en tijdzone wanneer de back-uptaak wordt gestart.

   ![wekelijks interval instellen](./media/backup-azure-sql-database/weekly-interval.png)

4. Standaard worden alle opties voor bewaartermijn (dagelijkse, wekelijkse, maandelijkse en jaarlijkse) geselecteerd. Schakel het selectievakje bewaartermijnen u niet wilt, en stel de intervallen dat u wilt gebruiken. Klik in het menu voor beleid voor volledige back-up, **OK** te accepteren van de instellingen.

   ![retentie bereik interval instellen](./media/backup-azure-sql-database/retention-range-interval.png)

    Herstelpunten zijn gecodeerd om te bewaren, op basis van de bewaartermijn. Als u een dagelijkse selecteert, is volledige back-up alleen volledige back-up elke dag geactiveerd. Afhankelijk van uw wekelijkse bewaarperiode van de dag van de specifieke back-up is gemarkeerd en bewaard op basis van de wekelijkse bewaartermijn. De maandelijkse en jaarlijkse bewaartermijn gedraagt zich op dezelfde manier.

5. Als u wilt een differentiële back-upbeleid toevoegen, klikt u op **differentiële back-** om het menu te openen. 

   ![Open differentiële beleid](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Selecteer in het menu voor beleid voor differentiële back-up, **inschakelen** te openen, de frequentie en retentie-besturingselementen. U kunt activeren, hoogstens één differentiële back-up per dag.
    > [!Important] 
    > Differentiële back-ups kunnen maximaal 180 dagen worden bewaard. Als u langere bewaartermijn nodig hebt, moet u volledige back-ups, differentiële back-ups kunt u niet gebruiken.
    >

   ![differentiële beleid bewerken](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Klik op **OK** aan het beleid opslaan en terug te keren naar het hoofdmenu van back-upbeleid.

6. Als u wilt toevoegen een transactionele logboekback-up-beleid, klikt u op **logboekback-up** om het menu te openen. Selecteer in het menu back-up van **inschakelen**, en stel de frequentie en retentie-besturingselementen. Logboekback-ups kunnen optreden zo vaak als elke 15 minuten en maximaal 35 dagen kunnen worden bewaard. Klik op **OK** aan het beleid opslaan en terug te keren naar het hoofdmenu van back-upbeleid.

   ![back-upbeleid log bewerken](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. Kies of u compressie voor SQL-back-up moet worden ingeschakeld. Compressie is standaard uitgeschakeld.

    Op de back-end, Azure Backup maakt gebruik van systeemeigen back-up-compressie van SQL.

8. Wanneer u alle wijzigingen hebt aangebracht in het back-upbeleid, klikt u op **OK**. 

   ![differentiële bewaartermijn](./media/backup-azure-sql-database/differential-backup-policy.png)

## <a name="restore-a-sql-database"></a>Een SQL-database herstellen

Azure Backup biedt functionaliteit voor het herstellen van afzonderlijke databases naar een specifieke datum of tijd, tot een specifieke ten tweede met transactielogboekback-ups. Op basis van hersteltijden die u opgeeft, Azure Backup bepaalt automatisch de juiste volledige, differentiële en de keten van logboekback-ups nodig zijn om uw gegevens te herstellen.

U kunt ook een specifieke volledige differentiële back-up en herstellen naar een specifiek herstelpunt in plaats van een bepaald tijdstip selecteren.

Een database te herstellen

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. Selecteer in het kluisdashboard **gebruik** back-Upitems om het menu back-Upitems te openen.

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. In de **back-Upitems** menu, selecteer het type back-upbeheer **SQL in Azure VM**. 

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    De lijst met Items voor back-up wordt aangepast om de lijst met SQL-databases weer te geven. 

4. Selecteer in de lijst met SQL-databases, de database die u wilt herstellen.

    ![SQL in Azure VM selecteren in lijst](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt het menu wordt geopend. Dit menu bevat de details van de back-up voor de database, waaronder:

    * de oudste en de meest recente herstelpunten,
    * status voor het back-up van het logboek voor de afgelopen 24 uur (voor databases in de volledige en bulksgewijs geregistreerde herstelmodel, indien geconfigureerd voor transactionele logboekback-ups)

5. Klik in het menu aan de geselecteerde database **DB herstellen** om het menu herstellen te openen.

    ![Selecteer de database herstellen](./media/backup-azure-sql-database/restore-db-button.png)

    De **herstellen** menu geopend en kiest, wordt dit het geval is de **configuratie herstellen** menu. De **configuratie herstellen** menu is de eerste stap bij het configureren van de herstelbewerking. In dit menu selecteert u waar u wilt de gegevens herstelt. De opties zijn:
    * Alternatieve locatie: Gebruik deze optie als u wilt dat de database naar een alternatieve locatie herstellen tijdens de oorspronkelijke brondatabase blijven behouden.
    * DB - Hiermee herstelt u de gegevens naar hetzelfde exemplaar van SQL Server als de oorspronkelijke bron worden overschreven. Het effect van dit is het overschrijven van de oorspronkelijke database.

    > [!Important]
    > Als de geselecteerde database bij een AlwaysOn-beschikbaarheidsgroep hoort, SQL staat niet toe dat de database worden overschreven. In dit geval wordt alleen de **alternatieve locatie** optie is ingeschakeld.
    >

    ![Klik op configureren om het menu in de configuratie van herstel](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Herstellen naar een alternatieve locatie

Deze procedure begeleidt bij het herstellen van gegevens naar een alternatieve locatie. Als u overschrijven van de database wilt bij het herstellen, gaat u naar de sectie [herstellen en de database overschrijven](backup-azure-sql-database.md#restore-and-overwrite-the-database). Deze procedure wordt ervan uitgegaan dat u hebt uw Recovery Services-kluis is geopend, en zijn op het menu van de configuratie herstellen. Als u niet, begint u met de sectie [een SQL-database herstellen](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> U kunt de database herstellen naar een SQL-Server in dezelfde Azure-regio en de doelserver moet worden geregistreerd op de Recovery Services-kluis. 
>

De **Server** vervolgkeuzelijst bevat alleen de SQL-servers geregistreerd bij de Recovery Services-kluis. Als de server die u wilt dat zich niet in de **Server** lijst, Zie de sectie [detecteren SQL server-databases](backup-azure-sql-database.md#discover-sql-server-databases) de server te vinden. Tijdens het detectieproces database, worden nieuwe servers zijn geregistreerd bij de Recovery Services-kluis.

1. In de **configuratie herstellen** menu:

    * Selecteer **alternatieve locatie**,
    * voor **Server**, kiest u de SQL-server waar u de database wilt herstellen.
    * in de **exemplaar** vervolgkeuzelijst, kiest u een SQL-exemplaar
    * in de **databasenaam hersteld** dialoogvenster, geeft u de naam van de doeldatabase.
    * indien van toepassing, selecteert u **overschrijven als de database met dezelfde naam al in het geselecteerde SQL-exemplaar bestaat**.
    * Klik op **OK** voltooid met het configureren van de bestemming en verplaatsen naar een herstelpunt te kiezen.

    ![alternatieve locatie, exemplaar en de naam in het menu van de configuratie herstellen selecteren](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. In de **herstelpunt selecteren** menu, kunt u een Logboeken (tijdstip) of volledig en differentieel herstelpunt. Als u herstellen naar een bepaald punt in tijd wilt, gaat u verder met deze stap. Als u een volledige of differentiële herstelpunt herstellen wilt, gaat u verder met stap 3.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    Het punt in tijd herstel is alleen beschikbaar voor logboekback-ups voor databases met volledige en bulksgewijs geregistreerde herstelmodel. Om te herstellen naar een bepaald punt in tijd:

    1. Selecteer **Logboeken (tijdstip)** als de optie voor terugzetten.

        ![Kies het type herstel](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **datum/tijd herstellen**, klikt u op het kalenderpictogram om de agenda te openen. Datums in vet bevatten herstelpunten en de huidige datum is gemarkeerd. Selecteer een datum in de kalender met herstelpunten. U kunt geen datums met er zijn geen herstelpunten selecteren.

        ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Wanneer u een datum selecteert, geeft de beschikbare herstelpunten in de tijdlijngrafiek weer in een continue reeks.

    3. Met behulp van de tijdlijngrafiek of het dialoogvenster tijd geeft u een specifieke tijd voor het herstelpunt en klik op **OK** om het herstelpunt stap te voltooien.
    
       ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        De **herstelpunt selecteren** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Uit de **Advanced Configuration** menu:

        * De database niet-operationeel om na te behouden terugzetten op de **herstellen met GEENHERSTEL** in het menu **ingeschakeld**.
        * Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **Advanced Configuration**.

    5. Op de **herstellen** menu, klikt u op **herstellen** de hersteltaak starten. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen taken volgen.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-job-notification.png)

3. In de **herstelpunt selecteren** menu, kies een herstelpunt. U kunt ervoor kiezen een Logboeken (tijdstip) of volledig en differentieel. Als u herstellen van een point-in-time-logboek wilt, gaat u terug naar stap 2. Deze stap herstelt een specifieke volledige of differentiële herstelpunt. Met deze optie kunt u alle volledige en differentiële herstelpunten voor de afgelopen 30 dagen bekijken. Als u zien van de herstelpunten die ouder zijn dan 30 dagen wilt, klikt u op **Filter** openen de **Filter herstelpunten** menu. Als u ervoor een differentiële herstelpunt kiest, Azure Backup eerst de juiste volledig herstelpunt herstelt en het geselecteerde differentiële herstelpunt wordt toegepast.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In de **herstelpunt selecteren** selecteren in het menu **volledig en differentieel**.

       ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        De lijst met beschikbare herstelpunten weergegeven.

    2. Selecteer een herstelpunt in de lijst met herstelpunten, en klik op **OK** om het herstelpunt procedure te voltooien. 

        ![de volledige herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        De **herstelpunt** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

        ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Uit de **Advanced Configuration** menu:

        * De database niet-operationeel om na te behouden terugzetten op de **herstellen met GEENHERSTEL** in het menu **ingeschakeld**. **Herstellen met GEENHERSTEL** is standaard uitgeschakeld.
        * Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **Advanced Configuration**.

    4. Op de **herstellen** menu, klikt u op **herstellen** de hersteltaak starten. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen taken volgen.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Herstellen en de database overschrijven

Deze procedure beschrijft het herstellen van gegevens en de database wordt overschreven. Als u herstellen naar een alternatieve locatie wilt, gaat u naar de sectie [herstellen naar een alternatieve locatie](backup-azure-sql-database.md#restore-to-an-alternate-location). Deze procedure wordt ervan uitgegaan dat u hebt uw Recovery Services-kluis is geopend, en zijn op de **configuratie herstellen** menu (Zie de volgende afbeelding). Als u niet, begint u met de sectie [een SQL-database herstellen](backup-azure-sql-database.md#restore-a-sql-database).

![Klik op configureren om het menu in de configuratie van herstel](./media/backup-azure-sql-database/restore-overwrite-db.png)

De **Server** vervolgkeuzelijst bevat alleen de SQL-servers geregistreerd bij de Recovery Services-kluis. Als de server die u wilt dat zich niet in de **Server** lijst, Zie de sectie [detecteren SQL server-databases](backup-azure-sql-database.md#discover-sql-server-databases) de server te vinden. Tijdens het detectieproces database, worden nieuwe servers zijn geregistreerd bij de Recovery Services-kluis.

1. In de **configuratie herstellen** in het menu **DB overschrijven** en klikt u op **OK** om uit te voeren met het configureren van de bestemming. 

   ![Klik op DB overschrijven](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    De **Server**, **exemplaar**, en **databasenaam hersteld** dialoogvensters zijn niet nodig.

2. In de **herstelpunt selecteren** menu, kunt u een Logboeken (tijdstip) of volledig en differentieel herstelpunt. Als u herstellen van een point-in-time-logboek wilt, gaat u verder met deze stap. Als u herstellen van een herstelpunt volledig en differentieel wilt, gaat u verder met stap 3.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    Het punt in tijd herstel is alleen beschikbaar voor logboekback-ups voor databases met volledige en bulksgewijs geregistreerde herstelmodel. Een punt in tijd herstel naar een specifieke tweede kiezen:

    1. Selecteer **Logboeken (tijdstip)** als de optie voor terugzetten.

        ![Kies het type herstel](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **datum/tijd herstellen**, klikt u op het kalenderpictogram om de agenda te openen. Datums in vet bevatten herstelpunten en de huidige datum is gemarkeerd. Selecteer een datum in de kalender met herstelpunten. U kunt geen datums met er zijn geen herstelpunten selecteren.

        ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Wanneer u een datum selecteert, geeft de beschikbare herstelpunten weer in de tijdlijngrafiek.

    3. Met behulp van de tijdlijngrafiek of het dialoogvenster tijd geeft u een specifieke tijd voor het herstelpunt en klik op **OK** om het herstelpunt stap te voltooien.
    
       ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        De **herstelpunt selecteren** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Uit de **Advanced Configuration** menu:

        * De database niet-operationeel om na te behouden terugzetten op de **herstellen met GEENHERSTEL** in het menu **ingeschakeld**.
        * Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **Advanced Configuration**.

    5. Op de **herstellen** menu, klikt u op **herstellen** de hersteltaak starten. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen taken volgen.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-job-notification.png)

3. In de **herstelpunt selecteren** menu, kies een herstelpunt. U kunt ervoor kiezen een Logboeken (tijdstip) of volledig en differentieel. Als u herstellen van een point-in-time-logboek wilt, gaat u terug naar stap 2. Deze stap herstelt een specifieke volledige of differentiële herstelpunt. Met deze optie kunt u alle volledige en differentiële herstelpunten voor de afgelopen 30 dagen bekijken. Als u zien van de herstelpunten die ouder zijn dan 30 dagen wilt, klikt u op **Filter** openen de **Filter herstelpunten** menu. Als u ervoor een differentiële herstelpunt kiest, Azure Backup eerst de juiste volledig herstelpunt herstelt en het geselecteerde differentiële herstelpunt wordt toegepast.

    ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In de **herstelpunt selecteren** selecteren in het menu **volledig en differentieel**.

       ![restore point menu selecteren](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        De lijst met beschikbare herstelpunten weergegeven.

    2. Selecteer een herstelpunt in de lijst met herstelpunten, en klik op **OK** om het herstelpunt procedure te voltooien. 

        ![de volledige herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        De **herstelpunt** menu wordt gesloten, en de **Advanced Configuration** menu wordt geopend.

        ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Uit de **Advanced Configuration** menu:

        * De database niet-operationeel om na te behouden terugzetten op de **herstellen met GEENHERSTEL** in het menu **ingeschakeld**. **Herstellen met GEENHERSTEL** is standaard uitgeschakeld.
        * Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **Advanced Configuration**.

    4. Op de **herstellen** menu, klikt u op **herstellen** de hersteltaak starten. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen taken volgen.

       ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Bewerkingen van de Azure Backup voor SQL op Azure Virtual machines beheren

In deze sectie bevat informatie over de verschillende Azure Backup-bewerkingen beschikbaar voor SQL op Azure virtual machines. De volgende bewerkingen uit op hoog niveau bestaat:

* Taken controleren
* Back-Upwaarschuwingen
* Stop de beveiliging op een SQL-database
* Beveiliging van een SQL-database hervatten
* Een ad-hoc back-uptaak activeren
* Registratie van een SQL-server

### <a name="monitor-jobs"></a>Taken controleren
Azure Backup wordt een klasse bedrijfsoplossing biedt geavanceerde back-up waarschuwingen en meldingen fouten (Zie onderstaande sectie voor waarschuwingen voor back-up). Als u nog steeds wilt bewaken van specifieke taken kunt u een van de volgende opties op basis van de vereiste gebruiken:

#### <a name="using-azure-portal---recovery-services-vault-for-all-ad-hoc-operations"></a>Recovery Services-kluis voor alle ad-hoc-bewerkingen met behulp van Azure portal ->
Azure back-up bevat alle handmatig hebt geactiveerd, of ad-hoc, taken in de portal voor back-up-taken. De beschikbare projecten in de portal opnemen: alle back-upbewerkingen configureren, handmatig geactiveerd back-upbewerkingen, bewerkingen voor het herstellen, registratie en databasebewerkingen detecteren en back-upbewerkingen stoppen. 
![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Alle geplande back-uptaken met inbegrip van volledige, differentiële en logboek back-up wordt niet weergegeven in de portal en kan worden bewaakt met behulp van SQL Server Management Studio, zoals hieronder wordt beschreven.
>

#### <a name="using-sql-server-management-studio-ssms-for-backup-jobs"></a>Met behulp van SQL Server Management Studio (SSMS) voor back-uptaken
Azure Backup gebruikt systeemeigen SQL-API's voor alle back-upbewerkingen. Met de systeemeigen API's, kunt u alle informatie over een taak uit ophalen de [SQL-tabel op de back-upset](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in de msdb-database. 

U kunt de onderstaande query als voorbeeld voor het ophalen van alle back-uptaken voor een specifieke database met de naam 'DB1'. U kunt de onderstaande query voor meer geavanceerde controle.
```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  
 
```

### <a name="backup-alerts"></a>Back-Upwaarschuwingen

Logboekback-ups om de 15 minuten plaatsvindt, bewaking van tijd tot tijd de back-uptaken worden omslachtig. Azure Backup zijn gepland voor deze situatie mogelijk moeite door te geven van e-mailwaarschuwingen geactiveerd door een back-upfouten. Waarschuwingen worden geconsolideerd op het databaseniveau van de-foutcode. Bijvoorbeeld, als een database meerdere mislukte back-ups heeft, in plaats van het ontvangen van een waarschuwing voor elke mislukt, ontvangt u e-mailadres voor de eerste fout. Vervolgens kunt u zich bij de Azure-portal voor het bewaken van de volgende fouten voor die database. 

Voor het bewaken van back-upwaarschuwingen:

1. Meld u aan bij uw Azure-abonnement in de [Azure-portal](https://portal.azure.com).

2. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

3. Selecteer in het menu Recovery Services-kluis **waarschuwingen en gebeurtenissen**. 

   ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. In de **waarschuwingen en gebeurtenissen** in het menu **waarschuwingen voor back-up** om de lijst met waarschuwingen weer te geven.

   ![menu voor geavanceerde configuratie](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Stop de beveiliging op een SQL Server-database

Als u stopt met het beveiligen van een SQL Server-database, Azure Backup wordt gevraagd of u wilt bewaren van de herstelpunten. Er zijn twee manieren om te stoppen met het beveiligen van SQL-database:

* Alle toekomstige back-uptaken stoppen en verwijderen van alle herstelpunten
* Alle toekomstige back-uptaken stoppen, maar laat u de herstelpunten 

De herstelpunten laten uitvoert kosten, zoals het aantal herstelpunten voor SQL voeren het beveiligde exemplaar van SQL prijzen kosten in rekening gebracht, plus de opslag die wordt gebruikt. Zie voor meer informatie over prijzen van Azure Backup voor SQL, de [pagina met prijzen voor back-up in Azure](https://azure.microsoft.com/pricing/details/backup/). Beveiliging voor de database stoppen:

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. Selecteer in het kluisdashboard **gebruik** back-Upitems om het menu back-Upitems te openen.

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. In de **back-Upitems** menu, selecteer het type back-upbeheer **SQL in Azure VM**. 

    ![Klik op + back-up naar het menu doel van de back-up openen](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    De lijst met Items voor back-up wordt aangepast om de lijst met SQL-databases weer te geven. 

4. Selecteer in de lijst met SQL-databases, de database die u wilt stoppen met het beveiligen.

    ![SQL in Azure VM selecteren in lijst](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt het menu wordt geopend. 

5. Klik in het menu aan de geselecteerde database **back-up stoppen** om te stoppen met het beschermen van de database.

    ![Selecteer de database herstellen](./media/backup-azure-sql-database/stop-db-button.png)

    De **back-up stoppen** menu wordt geopend.

6. In de **back-up stoppen** in het menu back-upgegevens behouden wilt, of back-upgegevens verwijderen. U kunt desgewenst een reden op voor het stoppen van beveiliging en een opmerking opgeven.

    ![Selecteer de database herstellen](./media/backup-azure-sql-database/stop-backup-button.png)

7. Klik op **back-up stoppen** beveiliging stoppen op de database. 

### <a name="resume-protection-for-a-sql-database"></a>Beveiliging van een SQL-database hervatten

Als de **back-upgegevens behouden** optie is geselecteerd bij het stoppen beveiliging voor de SQL-database, het is mogelijk om de beveiliging hervatten. Als de back-upgegevens is niet aanwezig zijn, kan beveiliging niet hervatten. 

1. Als u wilt hervatten beveiliging voor de SQL-database, opent u het back-upitem en klikt u op **back-up hervatten**.

    ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/resume-backup-button.png)

   Het menu back-upbeleid wordt geopend.

2. Uit de **back-upbeleid** menu selecteert u een beleid en klikt u op **opslaan**.

### <a name="trigger-an-adhoc-backup"></a>Een ad-hoc back-up activeren

Wanneer u maar wilt, kunt u een ad-hoc back-up activeren. Er zijn vier typen ad-hoc back-ups. Voor meer informatie over elk type, Zie het artikel [typen van de SQL-back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Volledige back-up
* Alleen volledige back-up kopiëren
* Differentiële back-up
* Een back-up

### <a name="unregister-a-sql-server"></a>Registratie van een SQL-Server

Registratie van een SQL-server na het verwijderen van beveiliging, maar voordat de kluis verwijderen

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. In de **beheren** sectie van de kluismenu, klikt u op **back-upinfrastructuur**.  

   ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. In de **beheerservers** sectie, klikt u op **beschermde Servers**.

   ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/protected-servers.png)

    Het menu van de beveiligde Servers wordt geopend. 

4. In de **beschermde Servers** in het menu selecteert u de server die u wilt de registratie ongedaan maken. Als u verwijderen van de kluis wilt, moet u de registratie ongedaan maken voor alle servers.

5. In het menu aan de beveiligde Servers met de rechtermuisknop op de beveiligde server en selecteer **verwijderen**. 

   ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Backup het PowerShell-voorbeeld voor het maken van back-ups van versleutelde virtuele machines.

> [!div class="nextstepaction"]
> [Back-up maken van een versleutelde VM](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
