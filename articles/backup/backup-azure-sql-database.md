---
title: Maak een back-up van SQL Server-database naar Azure | Microsoft Docs
description: Deze zelfstudie wordt uitgelegd hoe SQL Server back-up naar Azure. Het artikel wordt ook uitgelegd SQL Server herstellen.
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
ms.date: 6/29/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 89a1df607c220e5dc12bc6263955d6e445e529bd
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116025"
---
# <a name="back-up-sql-server-database-in-azure"></a>Back-up van SQL Server-database in Azure

SQL Server-databases zijn kritieke werkbelastingen waarvoor lage Recovery Point Objective (RPO) en op lange termijn. Azure Backup biedt een Serverbackup SQL-oplossing die nul infrastructuur, wat betekent dat er geen back-upserver van complexe, geen beheeragent of back-upopslag voor het beheren van vereist. Azure Backup biedt gecentraliseerd beheer voor uw back-ups via alle SQL-servers, of zelfs verschillende werkbelastingen.

 In dit artikel uitgelegd:

> [!div class="checklist"]
> * Vereisten voor het back-up van SQL Server naar Azure
> * Maken en gebruiken van een Recovery Services-kluis
> * Back-ups van SQL Server-database configureren
> * Beleid voor back-up (of bewaren) voor de herstelpunten instellen
> * De database herstellen

Voordat u de procedures in dit artikel, hebt u een SQL-Server worden uitgevoerd in Azure. [U kunt virtuele machines van SQL marketplace snel maken van een SQL Server](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Openbare Preview-beperkingen

De volgende items zijn de bekende beperkingen voor de openbare Preview.

- Virtuele machine van SQL is verbinding met internet te krijgen tot Azure openbare IP-adressen vereist. Zie de sectie voor meer details [netwerkverbinding tot stand brengen](backup-azure-sql-database.md#establish-network-connectivity).
- U kunt maximaal 2000 SQL-databases in één Recovery Services-kluis beveiligen. Aanvullende SQL-databases moeten worden opgeslagen in een afzonderlijke Recovery Services-kluis.
- [Back-up van gedistribueerde beschikbaarheidsgroepreplica groepen heeft beperkingen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- SQL Failover Cluster exemplaren (FCI) worden niet ondersteund.
- De Azure portal gebruiken voor het configureren van Azure Backup ter bescherming van SQL Server-databases. Ondersteuning voor Azure PowerShell, CLI en REST-API's is momenteel niet beschikbaar.

## <a name="supported-azure-geos"></a>Ondersteunde Azure geografische gebieden

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

De volgende ondersteunde besturingssystemen en versies van SQL Server zijn van toepassing op marketplace SQL Azure virtuele machines en niet-marketplace virtuele machines (waar SQL Server is handmatig geïnstalleerd).

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux wordt momenteel niet ondersteund.

### <a name="supported-versionseditions-of-sql-server"></a>Ondersteunde versies/edities van SQL Server

- SQL 2012 Enterprise, Standard, Web, Developer, Express
- SQL 2014 Enterprise, Standard, Web, Developer, Express
- SQL 2016 Enterprise, Standard, Web, Developer, Express
- SQL 2017 Enterprise, Standard, Web, Developer, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>Vereisten voor het gebruik van Azure Backup naar SQL Server beveiligen 

Voordat u kunt back-up van uw SQL Server-database, controleert u de volgende voorwaarden. :

- Bepalen of [een Recovery Services-kluis maken](backup-azure-sql-database.md#create-a-recovery-services-vault) in dezelfde regio, of landinstellingen, als de virtuele machine hosten van SQL Server.
- [Controleer de machtigingen voor de virtuele machine](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) die nodig zijn voor de back-up van SQL-databases.
- [Virtuele machine van SQL netwerkverbinding heeft](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> U kunt slechts één back-upoplossing hebben op een tijdstip aan back-up van SQL Server-databases. Schakel alle andere SQL-back-voordat u deze functie, anders back-ups wordt beïnvloeden en mislukken. U kunt inschakelen Azure Backup voor IaaS VM samen met de SQL-back-up zonder een conflict 
>

Als deze voorwaarden aanwezig zijn in uw omgeving, gaat u verder naar de sectie [uw kluis ter bescherming van een SQL-database configureren](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database). Als een van de vereisten nog niet bestaan, blijven lezen van deze sectie.


## <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

Voor alle bewerkingen moet de virtuele machine van SQL verbinding hebben met Azure openbare IP-adressen. SQL-bewerkingen voor virtuele machine (zoals detectie van de database, configureren back-up, geplande back-ups, terugzetten herstelpunten, enzovoort) is mislukt zonder verbinding met het openbare IP-adressen. Gebruik een van de volgende opties voor een duidelijke pad opgeven voor back-verkeer:

- Geaccepteerde het Azure datacenter IP ranges - aan geaccepteerde het Azure datacenter IP-bereiken, gebruikt u de [center downloadpagina voor meer informatie over de IP-adresbereiken en instructies](https://www.microsoft.com/download/details.aspx?id=41653). 
- Implementeren van een HTTP-proxyserver voor het routeren van verkeer - wanneer u een back-up van een SQL-database op een virtuele machine, de Backup-extensie op de virtuele machine maakt gebruik van HTTPS-API's opdrachten voor het beheer verzenden naar Azure Backup en gegevens naar Azure Storage. De Backup-extensie wordt ook Azure Active Directory (AAD) gebruikt voor authenticatie. Het verkeer van de Backup-extensie voor deze drie services via de HTTP-proxy sturen omdat dit het enige onderdeel dat is geconfigureerd voor toegang tot het openbare internet.

De wisselwerking tussen de opties zijn: beheerbaarheid, gedetailleerde controle en kosten.

> [!NOTE]
>Service-tags voor Azure Backup moet beschikbaar zijn door de algemene beschikbaarheid.
>

| Optie | Voordelen | Nadelen |
| ------ | ---------- | ------------- |
| Whitelist IP-adresbereiken | Er zijn geen extra kosten. <br/> Gebruik voor toegang tot te openen in een NSG, **Set AzureNetworkSecurityRule** cmdlet. | Moeilijk te beheren als de betrokken IP-adresbereiken op den duur veranderen. <br/>Biedt toegang tot de volledige Azure, niet alleen opslag.|
| Gebruik een HTTP-proxy   | Gedetailleerd beheer in de proxy van de opslag URL's is toegestaan. <br/>Één punt internet toegang tot virtuele machines. <br/> Ten aanzien van wijzigingen in de Azure-IP-adres. | Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Stel machtigingen voor niet-marketplace virtuele SQL-machines

Als u wilt back-up van een virtuele machine, Azure Backup vereist de **AzureBackupWindowsWorkload** extensie worden geïnstalleerd. Als u virtuele machines van Azure marketplace gebruikt, gaat u verder met [detecteren SQL server-databases](backup-azure-sql-database.md#discover-sql-server-databases). Als de virtuele machine die als host fungeert voor de SQL-databases niet vanuit de Azure marketplace gemaakt is, voert u de volgende sectie voor het installeren van de extensie en stel de juiste machtigingen. Naast de **AzureBackupWindowsWorkload** -uitbreiding, Azure Backup vereist SQL sysadmin-machtigingen voor het beveiligen van SQL-databases. Tijdens het detecteren van databases op de virtuele machine, maakt Azure Backup een Service\AzureWLBackupPluginSvc NT-account. Voor Azure Backup voor het detecteren van SQL-databases, moet de account NT Service\AzureWLBackupPluginSvc SQL-logbestanden in- en SQL sysadmin-bevoegdheden hebben. De volgende procedure wordt uitgelegd hoe u deze machtigingen opgeven.

Om machtigingen te configureren:

1. In de [Azure-portal](https://portal.azure.com), opent u de Recovery Services-kluis die u gebruikt voor het beveiligen van SQL-databases.
2. Klik in het dashboard kluismenu op **+ back-up** openen de **back-updoel** menu.

   ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Op de **back-updoel** menu, in de **waar wordt uw workload uitgevoerd?** menu laat **Azure** als standaardwaarde.

4. Op de **wat wilt u back-up maken** menu, vouw de vervolgkeuzelijst en selecteer **SQL-Server in Azure VM**.

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    De **back-updoel** menu ziet nu twee nieuwe stappen: **databases detecteren in virtuele machines in** en **back-up configureren**. **Databases in virtuele machines detecteren** wordt gestart van een zoekopdracht voor Azure virtual machines.

    ![Bevat de stappen voor het doel van nieuwe back-up](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Klik op **Start detectie** om te zoeken naar onbeveiligde virtuele machines in het abonnement. Afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement, het kan even duren te doorlopen van alle virtuele machines.

    ![Back-up in behandeling](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Zodra een niet-beveiligde virtuele machine wordt gedetecteerd, wordt deze weergegeven in de lijst. Onbeveiligde virtuele machines worden weergegeven door de virtuele machine en de bron-groep. Het is mogelijk voor meerdere virtuele machines naar dezelfde naam hebben. Echter, virtuele machines met dezelfde naam behoren tot verschillende resourcegroepen. Als een verwachte virtuele machine niet wordt weergegeven in de lijst, kunt u zien als de virtuele machine al is beveiligd in een kluis.

6. Selecteer in de lijst van virtuele machines, de virtuele machine met de SQL-database die u wilt back-up en klikt u op **detecteren databases**. 

    Het detectieproces installeert de **AzureBackupWindowsWorkload** uitbreiding op de virtuele machine. De extensie kan de Azure Backup-service om te communiceren met de virtuele machine, zodat back-up van de SQL-databases kunt maken. Eenmaal de extensie installeert, Azure back-up maakt het serviceaccount virtuele Windows **NT Service\AzureWLBackupPluginSvc**, op de virtuele machine. De virtuele-serviceaccount vereist SQL sysadmin-machtiging. Tijdens de virtuele service account installatie, als u de fout ziet **UserErrorSQLNoSysadminMembership**, Zie de sectie [hersteld SQL sysadmin-bevoegdheden](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    Het gebied meldingen geeft de voortgang van de detectie van de database. Afhankelijk van hoeveel databases zich op de virtuele machine, het kan even duren voor de taak te voltooien. Wanneer de geselecteerde databases zijn gedetecteerd, wordt een bericht weergegeven.

    ![melding van de geslaagde implementatie](./media/backup-azure-sql-database/notifications-db-discovered.png)

Zodra u de database met de Recovery Services-kluis koppelt, wordt de volgende stap is het [configureren van de back-up](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>SQL sysadmin-bevoegdheden is hersteld

Tijdens het installatieproces als u de fout ziet **UserErrorSQLNoSysadminMembership**, aanmelding bij SQL Server Management Studio (SSMS) met een account dat sysadmin-SQL-machtiging heeft. Tenzij u speciale machtigingen nodig hebt, moet u Windows-verificatie gebruiken voor het herkennen van het account zijn.

1. Open op de SQL Server, de **beveiliging/aanmeldingen** map.

    ![De SQL Server- en beveiligings- en aanmelding mappen om te zien van accounts openen](./media/backup-azure-sql-database/security-login-list.png)

2. Klik met de rechtermuisknop op de map aanmeldingen en selecteert u **New Login**, en klik op in de aanmelding - nieuwe dialoogvenster **zoeken**

    ![Zoekopdracht in de aanmelding — nieuw dialoogvenster openen](./media/backup-azure-sql-database/new-login-search.png)

3. Omdat het serviceaccount virtuele Windows **NT Service\AzureWLBackupPluginSvc** al is gemaakt tijdens de registratie van de virtuele machine en de SQL-detectie fase, geef de accountnaam op die wordt weergegeven in de  **Voer de naam van het object te selecteren** dialoogvenster. Klik op **namen controleren** om de naam te herleiden. 

    ![Klik op de knop Namen controleren om op te lossen de onbekende servicenaam](./media/backup-azure-sql-database/check-name.png)

4. Klik op **OK** om het dialoogvenster gebruiker of groep selecteren te sluiten.

5. In de **serverfuncties** dialoogvenster, zorg ervoor dat de **sysadmin** rol is geselecteerd. Klik vervolgens op **OK** sluiten **Login - nieuwe**.

    ![Zorg ervoor dat de serverrol sysadmin is geselecteerd](./media/backup-azure-sql-database/sysadmin-server-role.png)

    De vereiste machtigingen moeten nu bestaan.

6. Hoewel u de Machtigingsfout hebt opgelost, moet u nog steeds de database koppelen aan de Recovery Services-kluis. In de Azure portal **beveiligde Servers** lijst, klik met de rechtermuisknop op de server in de fout en selecteer **detecteren databases**.

    ![Controleer of dat de server heeft de juiste machtigingen](./media/backup-azure-sql-database/check-erroneous-server.png)

    Het gebied meldingen geeft de voortgang van de detectie van de database. Afhankelijk van hoeveel databases zich op de virtuele machine, het kan even duren voor de taak te voltooien. Wanneer de geselecteerde databases zijn gevonden, wordt een bericht weergegeven in het gebied meldingen.

    ![melding van de geslaagde implementatie](./media/backup-azure-sql-database/notifications-db-discovered.png)

Zodra u de database met de Recovery Services-kluis koppelt, wordt de volgende stap is het [configureren van de back-up](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server-databases te detecteren

Azure Backup kan alle databases op een SQL Server-exemplaar worden gedetecteerd zodat u ze per de vereisten van uw back-up beschermen kunt. Gebruik de volgende procedure voor het identificeren van de virtuele machine die als host fungeert voor de SQL-databases. Zodra u de virtuele machine hebt geïdentificeerd, wordt in Azure Backup een lichtgewicht-extensie voor het detecteren van de SQL server-databases installeert.

1. Aanmelden bij uw abonnement in de [Azure-portal](https://portal.azure.com/).
2. Selecteer in het menu links **alle Services**.

    ![De optie alle Services in het hoofdmenu](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Typ in het dialoogvenster alle services *Recovery Services*. Als u te typen begint, filtert uw invoer in de lijst met resources. Als u wordt weergegeven, selecteert u **Recovery Services-kluizen**.

    ![Typ de Recovery Services in het dialoogvenster met alle services](./media/backup-azure-sql-database/all-services.png) <br/>

    De lijst met Recovery Services-kluizen in het abonnement wordt weergegeven. 

4. Selecteer in de lijst met Recovery Services-kluis, de kluis die u gebruiken wilt voor het beveiligen van uw SQL-databases.

5. Klik in het dashboard kluismenu op **+ back-up** openen de **back-updoel** menu.

   ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/open-backup-menu.png)

6. Op de **back-updoel** menu, in de **waar wordt uw workload uitgevoerd?** menu laat **Azure** als standaardwaarde.

7. Op de **wat wilt u back-up maken** menu, vouw de vervolgkeuzelijst en selecteer **SQL-Server in Azure VM**.

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Eenmaal hebt geselecteerd, de **back-updoel** menu ziet u twee stappen: databases detecteren in virtuele machines en back-up configureren. 

    ![Bevat de stappen voor het doel van nieuwe back-up](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Klik op **Start detectie** om te zoeken naar onbeveiligde virtuele machines in het abonnement. Afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement, het kan even duren te doorlopen van alle virtuele machines.

    ![Back-up in behandeling](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Zodra een niet-beveiligde virtuele machine wordt gedetecteerd, wordt deze weergegeven in de lijst. Meerdere virtuele machines hebben dezelfde naam. Echter behoren meerdere virtuele machines met dezelfde naam tot verschillende resourcegroepen. De niet-beveiligde virtuele machines worden weergegeven door de virtuele machine en de bron-groep. Als een verwachte virtuele machine niet wordt weergegeven, kunt u zien als dat de virtuele machine al is beveiligd in een kluis.

9. Selecteer in de lijst van virtuele machines, het selectievakje van de virtuele machine met de SQL-databases die u wilt beveiligen en op **detecteren databases**.

    Azure Backup detecteert alle SQL-databases op de virtuele machine. Zie de volgende sectie voor informatie over wat er tijdens de fase van de detectie van database gebeurt [back-end operations bij het detecteren van SQL-databases](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). Na het detecteren van de SQL-databases, bent u klaar om [configureren van de back-uptaak](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>Back-end operations bij het detecteren van SQL-databases

Wanneer u gebruikt de **detecteren databases** hulpprogramma voor Azure Backup wordt de volgende bewerkingen uitgevoerd op de achtergrond:

- Hiermee registreert de virtuele machine met de Recovery Services-kluis voor back-up werkbelasting. Alle databases op de geregistreerde virtuele machine kunnen alleen een back-up in deze Recovery Services-kluis. 

- installeert de **AzureBackupWindowsWorkload** uitbreiding op de virtuele machine. Back-ups van een SQL-database is een oplossing voor zonder agent, dat wil zeggen, geen agent met de extensie is geïnstalleerd op de virtuele machine, is geïnstalleerd op de SQL-database.

- het serviceaccount maakt **NT Service\AzureWLBackupPluginSvc**, op de virtuele machine. Alle bewerkingen voor back-up en herstel de serviceaccount gebruiken. **NT Service\AzureWLBackupPluginSvc** SQL sysadmin-machtigingen nodig. Alle SQL Marketplace virtuele machines worden geleverd met de SqlIaaSExtension geïnstalleerd en AzureBackupWindowsWorkload SQLIaaSExtension gebruikt automatisch de vereiste machtigingen ophalen. Als uw virtuele machine heeft geen SqlIaaSExtension geïnstalleerd, de bewerking DB detecteren mislukt en ophalen van het foutbericht **UserErrorSQLNoSysAdminMembership**. Als u wilt toevoegen de sysadmin-machtigingen voor back-up, volg de instructies in [instellen van Azure Backup-machtigingen voor niet-marketplace SQL VMs](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![Selecteer de virtuele machine en de database](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>Back-up voor SQL Server-database configureren

Azure Backup biedt beheerservices voor SQL Server-databases beveiligen en beheren van back-uptaken. Het beheer en de mogelijkheden voor bewaking, is afhankelijk van de Recovery Services-kluis. 

> [!NOTE]
> U kunt slechts één back-upoplossing hebben op een tijdstip aan back-up van SQL Server-databases. Schakel alle andere SQL-back-voordat u deze functie, anders back-ups wordt beïnvloeden en mislukken. U kunt inschakelen Azure Backup voor IaaS VM samen met de SQL-back-up zonder een conflict 
>

Beveiliging voor de SQL-database configureren:

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. Klik in het dashboard kluismenu op **+ back-up** openen de **back-updoel** menu.

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Op de **back-updoel** menu, in de **waar wordt uw workload uitgevoerd?** menu laat **Azure** als standaardwaarde.

4. Op de **wat wilt u back-up maken** menu, vouw de vervolgkeuzelijst en selecteer **SQL-Server in Azure VM**.

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Eenmaal hebt geselecteerd, de **back-updoel** menu ziet u twee stappen: databases detecteren in virtuele machines en back-up configureren. Als u dit artikel in volgorde hebt doorlopen, hebt u al de niet-beveiligde virtuele machines gevonden en deze kluis is geregistreerd bij een virtuele machine. U bent nu klaar om te configureren van beveiliging voor de SQL-databases.

5. Klik in het menu back-updoel **back-up configureren**.

    ![Bevat de stappen voor het doel van nieuwe back-up](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    De Azure Backup-service wordt weergegeven voor alle SQL-exemplaren met zowel zelfstandige databases als SQL AlwaysOn-beschikbaarheidsgroepen. De zelfstandige databases in de SQL-exemplaar, klikt u op de dubbele punthaak naast de naam van het exemplaar om de databases weer te geven. De volgende afbeeldingen ziet u voorbeelden van een zelfstandig exemplaar en een AlwaysOn-beschikbaarheidsgroep.

    > [!NOTE]
    > Volledige en differentiële back-ups gebeuren van het primaire knooppunt, zoals SQL-platform die beperking heeft. Logboekback-up kan gebeuren op basis van uw voorkeur back-up. Vanwege deze beperking, moet het primaire knooppunt worden geregistreerd.
    >

    ![Lijst met databases in SQL-exemplaar](./media/backup-azure-sql-database/discovered-databases.png)

    Klik op de dubbele punthaak naast AlwaysOn-beschikbaarheidsgroepen om de lijst met databases weer te geven.

    ![Lijst met databases in AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Uit de lijst met databases, selecteer alle opties die u wilt beveiligen en op **OK**.

    ![Selecteer beveiligen door ze door meerdere databases](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    U kunt maximaal 50 databases in één keer selecteren. Als u meer dan 50 databases beveiligen wilt, moet u meerdere fasen. Nadat u de eerste 50 databases hebt beveiligd, Herhaal deze stap ter bescherming van de volgende set van databases.
    > [!Note] 
    > Voor het optimaliseren van de back-belastingen wordt Azure Backup grote back-uptaken in meerdere batches. Het maximale aantal databases in één back-uptaak is 50.
    >
    >

7. Maken of kies een back-upbeleid in de **back-** selecteert u **back-up maken van beleid**, om het menu te openen.

    ![Selecteer de optie back-upbeleid](./media/backup-azure-sql-database/select-backup-policy.png)

8. Van de **back-upbeleid kiezen** vervolgkeuzelijst, kiest u een back-upbeleid en op **OK**. Zie de sectie voor informatie over het maken van uw eigen back-upbeleid [een back-upbeleid definiëren](backup-azure-sql-database.md#define-a-backup-policy).

    ![Kies een back-upbeleid in de vervolgkeuzelijst](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    In het menu van het beleid voor back-up van de **back-upbeleid kiezen** vervolgkeuzelijst, kunt u kiezen: 
    - het standaardbeleid HourlyLogBackup 
    - een bestaande back-upbeleid eerder hebt gemaakt voor SQL,
    - naar [definiëren een nieuw beleid](backup-azure-sql-database.md#define-a-backup-policy) op basis van uw herstelpuntdoel (RPO) en de bewaartermijn. 

    > [!Note]
    > Azure Backup ondersteunt lange bewaartermijn op basis van de back-upschema niet opa-vader-zoon back-end opslagverbruik bij de behoeften van de naleving te optimaliseren.
    >

9. Als u een back-upbeleid hebt gekozen de **back-up menu** klikt u op **back-up**.

    ![het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

    U kunt de voortgang van de configuratie in het meldingengebied van de portal volgen.

    ![systeemvak weergeven](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Een back-upbeleid definiëren

Een back-upbeleid definieert een matrix met wanneer de back-ups zijn gemaakt en hoe lang de back-ups blijven behouden. U kunt drie soorten back-up voor SQL-databases plannen Azure Backup gebruiken:

* Volledige back-up - een volledige databaseback-up maakt een back-up van de gehele database. Een volledige back-up bevat de gegevens in een specifieke database of een set bestandsgroepen of bestanden en voldoende logboek om die gegevens te herstellen. U kunt maximaal één volledige back-up per dag activeren. U kunt een volledige back-up te nemen in een interval van dagelijks of wekelijks. 
* Differentiële back-up - een differentiële back-up is gebaseerd op de meest recente, de vorige volledige gegevens back-up. Een differentiële back-up legt alleen de gegevens die zijn gewijzigd sinds de volledige back-up. U kunt maximaal één differentiële back-up per dag activeren. U kunt een volledige back-up en een differentiële back-up niet configureren op dezelfde dag.
* Transactielogboek - kan een logboekback-up herstel tot een bepaald punt in tijd het tweede. Maximaal, kunt u configureren transactionele logboekback-ups om de 15 minuten.

Beleid wordt gemaakt in de Recovery Services-kluis niveau. Als u meerdere kluizen hebt, wordt de kluizen hetzelfde back-upbeleid kunnen gebruiken, maar u moet het back-upbeleid toepassen op elke kluis. Bij het maken van een back-upbeleid, de dagelijkse volledige back-up de standaardwaarde is. Als u overschakelt van volledige back-ups per week wordt uitgevoerd, kunt u een differentiële back-up, maar alleen toevoegen. De volgende procedure wordt uitgelegd hoe u een back-upbeleid voor een SQL server maken in Azure een virtuele machine.

Een back-upbeleid maken

1. In het menu van het beleid voor back-up van de **back-upbeleid kiezen** vervolgkeuzelijst, selecteer **nieuw**.

   ![nieuwe back-upbeleid maken](./media/backup-azure-sql-database/create-new-backup-policy.png)

    De back-up beleid menu switches voor de velden die nodig zijn voor elke nieuwe back-upbeleid voor SQL server.

   ![nieuwe back-upbeleid velden](./media/backup-azure-sql-database/blank-new-policy.png)

2. In **beleidsnaam**, Geef een naam op. 

3. Een volledige back-up is verplicht. Accepteer de standaardwaarden voor de volledige back-up of klik op **volledige back-up** om het beleid te bewerken.

    ![nieuwe back-upbeleid velden](./media/backup-azure-sql-database/full-backup-policy.png)

    Kies in het beleid volledige back-up dagelijks of wekelijks voor de frequentie. Als u dagelijks, kiest u de uur- en tijdzone, wanneer de back-uptaak wordt gestart. Als u dagelijkse volledige back-ups kiest, kunt u geen differentiële back-ups maken.

   ![dagelijks interval-instelling](./media/backup-azure-sql-database/daily-interval.png)

    Als u wekelijks, kiest u de dag van de week, uur en tijdzone als de back-uptaak begint.

   ![wekelijkse interval-instelling](./media/backup-azure-sql-database/weekly-interval.png)

4. Standaard zijn alle bewaartermijn opties (dagelijkse, wekelijkse, maandelijkse en jaarlijkse) geselecteerd. Schakel de limieten voor de bewaartermijnen u niet wilt, en stel het interval om te gebruiken. Klik in het menu van het beleid voor volledige back-up op **OK** te accepteren van de instellingen.

   ![bewaartermijn bereik interval instellen](./media/backup-azure-sql-database/retention-range-interval.png)

    Herstelpunten zijn voor het bewaren, op basis van hun bewaartermijn gelabeld. Als u een dagelijkse selecteert, wordt volledige back-up slechts één volledige back-up elke dag geactiveerd. Afhankelijk van uw wekelijkse bewaren van de specifieke dag back-up is gemarkeerd en behouden op basis van de wekelijkse bewaartermijn. De maandelijkse en jaarlijkse bewaartermijn gedraagt zich op dezelfde manier.

5. Klik op als u een differentiële back-upbeleid **differentiële back-** om het menu te openen. 

   ![Open differentiële beleid](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Selecteer in het menu van het beleid differentiële back-up **inschakelen** openen van de frequentie en het bewaren van besturingselementen. U kunt activeren, hoogstens één differentiële back-up per dag.
    > [!Important] 
    > Maximaal kunnen differentiële back-ups worden bewaard gedurende 180 dagen. Als u langere bewaartermijn nodig hebt, moet u volledige back-ups, differentiële back-ups kunt u niet gebruiken.
    >

   ![differentiële beleid bewerken](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Klik op **OK** het beleid opslaan en terugkeren naar het hoofdmenu van de Backup-beleid.

6. Een transactionele logboekback-up als beleid wilt toevoegen, klikt u op **logboekback-up** om het menu te openen. Selecteer in het menu logboekback-up **inschakelen**, en stel de frequentie en het bewaren van besturingselementen. Logboekback-ups zo vaak als om de 15 minuten kunnen optreden, en tot maximaal 35 dagen kunnen worden bewaard. Klik op **OK** het beleid opslaan en terugkeren naar het hoofdmenu van de Backup-beleid.

   ![back-upbeleid logboek bewerken](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. Kies of u compressie voor SQL-back-up moet worden ingeschakeld. Compressie is standaard uitgeschakeld.

    Op de back-end gebruikt Azure Backup SQL systeemeigen back-compressie.

8. Wanneer u alle wijzigingen hebt aangebracht aan het beleid van de back-up, klikt u op **OK**. 

   ![differentiële bewaartermijn](./media/backup-azure-sql-database/differential-backup-policy.png)

## <a name="restore-a-sql-database"></a>Een SQL-database herstellen

Azure Backup biedt functionaliteit voor het herstellen van afzonderlijke databases naar een specifieke datum of tijd, tot aan een specifieke ten tweede met transactielogboekback-ups. Op basis van hersteltijden die u opgeeft, Azure Backup bepaalt automatisch de juiste volledige, differentiële en de keten van logboekback-ups die zijn vereist voor het herstellen van uw gegevens.

U kunt ook een specifieke Full of differentiële back-up te herstellen naar een specifiek herstelpunt in plaats van een bepaald tijdstip selecteren.

Een database te herstellen

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. Selecteer in het kluisdashboard **gebruik** Items voor back-up om de back-Upitems menu te openen.

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. In de **back-ups** menu, selecteer het type back-up management **SQL in Azure VM**. 

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    De lijst met Items voor back-up wordt aangepast om de lijst weergeven met de SQL-databases. 

4. Selecteer in de lijst met SQL-databases, de database die u wilt herstellen.

    ![SQL in Azure VM selecteren in lijst](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt de menu wordt geopend. Dit menu biedt de details van de back-up voor de database, waaronder:

    * de oudste en de meest recente herstelpunten,
    * back-status in logboek registreren voor de afgelopen 24 uur (voor databases in de volledige en bulksgewijs geregistreerde herstelmodel als geconfigureerd voor transactionele logboekback-ups)

5. Klik in het menu van de geselecteerde database **Restore DB** om het menu herstellen te openen.

    ![Herstel de database selecteren](./media/backup-azure-sql-database/restore-db-button.png)

    De **herstellen** menu wordt geopend en in dat geval hoeft de **Restore Configuration** menu. De **Restore Configuration** menu is de eerste stap bij het configureren van de herstelbewerking. In dit menu selecteert u waar u de gegevens herstelt. De opties zijn:
    * Alternatieve locatie: Gebruik deze optie als u wilt dat de database herstellen naar een alternatieve locatie terwijl de oorspronkelijke brondatabase blijven behouden.
    * DB - Hiermee herstelt u de gegevens naar hetzelfde exemplaar van SQL Server als de oorspronkelijke bron worden overschreven. Het effect van dit is het overschrijven van de oorspronkelijke database.

    > [!Important]
    > Als de geselecteerde database deel uitmaakt van een AlwaysOn-beschikbaarheidsgroep, is de database worden overschreven niet toegestaan door SQL. In dit geval wordt alleen de **alternatieve locatie** optie is ingeschakeld.
    >

    ![Klik op configureren om te herstellen configuratie menu openen](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Herstellen naar een alternatieve locatie

Deze procedure helpt bij het herstellen van gegevens naar een alternatieve locatie. Als u wilt dat de database te overschrijven bij het herstellen, Ga naar de sectie [terugzetten en de database overschrijven](backup-azure-sql-database.md#restore-and-overwrite-the-database). Deze procedure wordt ervan uitgegaan dat u hebt uw Recovery Services-kluis is geopend, en op het menu herstellen configuratie. Als u niet, begint u met de sectie [een SQL-database herstellen](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> U kunt de database herstellen naar een SQL-Server in dezelfde Azure-regio en de doelserver moet worden geregistreerd in de Recovery Services-kluis. 
>

De **Server** vervolgkeuzelijst bevat alleen de SQL-servers geregistreerd bij de Recovery Services-kluis. Als de server die u wilt dat zich niet in de **Server** lijst, Zie de sectie [detecteren SQL server-databases](backup-azure-sql-database.md#discover-sql-server-databases) de server te vinden. Tijdens het detectieproces voor database zijn nieuwe servers geregistreerd bij de Recovery Services-kluis.

1. In de **Restore Configuration** menu:

    * Selecteer **alternatieve locatie**,
    * voor **Server**, kiest u de SQL server waar u de database herstellen.
    * in de **exemplaar** vervolgkeuzelijst, kiest u een SQL-exemplaar
    * in de **databasenaam hersteld** dialoogvenster, geef de naam van de doeldatabase.
    * Selecteer indien van toepassing, **overschrijven als de database met dezelfde naam al op de geselecteerde SQL-exemplaar bestaat**.
    * Klik op **OK** voltooid met het configureren van de bestemming en verplaatsen naar een herstelpunt kiezen.

    ![Selecteer alternatieve locatie, exemplaar en de naam in het menu van de configuratie herstellen](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. In de **herstelpunt selecteren** menu, kunt u een Logboeken (punt in tijd) of volledige & differentiële herstelpunt. Als u herstellen naar een bepaald punt in tijd wilt, gaat u verder met deze stap. Als u een volledige of differentiële herstelpunt herstellen wilt, gaat u verder met stap 3.

    ![Selecteer herstellen punt menu](./media/backup-azure-sql-database/recovery-point-menu.png)

    Punt in tijd terugzetten is alleen beschikbaar voor logboekback-ups voor databases met volledige en bulksgewijs geregistreerde herstelmodel. Herstellen naar een bepaald punt in tijd:

    1. Selecteer **Logboeken (punt in tijd)** als de optie voor terugzetten.

        ![Kies het type herstel](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **datum/tijd herstellen**, klikt u op het kalenderpictogram om de kalender. Datums vet herstelpunten bevatten en de huidige datum is gemarkeerd. Selecteer een datum in de kalender met herstelpunten. U kunt geen datums met er zijn geen herstelpunten selecteren.

        ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Wanneer u een datum selecteert, geeft de tijdlijngrafiek de beschikbare herstelpunten weer in een continue reeks.

    3. Met behulp van de tijdlijngrafiek of het dialoogvenster tijd geeft u een specifieke tijd voor het herstelpunt en klik op **OK** om het herstelpunt stap te voltooien.
    
       ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        De **herstelpunt selecteren** menu wordt gesloten, en de **geavanceerde configuratie** menu wordt geopend.

       ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Van de **geavanceerde configuratie** menu:

        * Voor het behouden van de database niet-operationeel na het terugzetten, op de **herstellen met GEENHERSTEL** selecteert u **ingeschakeld**.
        * Als u wijzigen van de locatie op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **geavanceerde configuratie**.

    5. Op de **herstellen** menu, klikt u op **herstellen** starten van de hersteltaak. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen van taken volgen.

       ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-job-notification.png)

3. In de **herstelpunt selecteren** menu, kies een herstelpunt. U kunt een Logboeken (punt in tijd) of volledige & differentiële. Als u een point-in-time-logboek te herstellen wilt, gaat u terug naar stap 2. Deze stap herstelt een specifieke volledig of differentieel herstelpunt. Met deze optie is ziet u alle volledige en differentiële herstelpunten voor de afgelopen 30 dagen. Als u zien van de herstelpunten die ouder zijn dan 30 dagen wilt, klikt u op **Filter** openen de **Filter herstelpunten** menu. Als u een differentiële herstelpunt, Azure Backup herstelt eerst het juiste volledige herstelpunt en past vervolgens het geselecteerde differentiële herstelpunt.

    ![Selecteer herstellen punt menu](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In de **herstelpunt selecteren** menu Selecteer **volledige & differentiële**.

       ![Selecteer herstellen punt menu](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        De lijst met beschikbare herstelpunten weergegeven.

    2. Selecteer een herstelpunt in de lijst van herstelpunten, en klikt u op **OK** herstelpunt procedure wilt uitvoeren. 

        ![de volledige herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        De **herstelpunt** menu wordt gesloten, en de **geavanceerde configuratie** menu wordt geopend.

        ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Van de **geavanceerde configuratie** menu:

        * Voor het behouden van de database niet-operationeel na het terugzetten, op de **herstellen met GEENHERSTEL** selecteert u **ingeschakeld**. **Herstellen met GEENHERSTEL** is standaard uitgeschakeld.
        * Als u wijzigen van de locatie op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **geavanceerde configuratie**.

    4. Op de **herstellen** menu, klikt u op **herstellen** starten van de hersteltaak. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen van taken volgen.

       ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Terugzetten en de database overschrijven

Deze procedure helpt bij het herstellen van gegevens en de database wordt overschreven. Als u herstellen naar een alternatieve locatie wilt, Ga naar de sectie [herstellen naar een alternatieve locatie](backup-azure-sql-database.md#restore-to-an-alternate-location). Deze procedure wordt ervan uitgegaan dat u hebt uw Recovery Services-kluis is geopend, en op de **Restore Configuration** menu (Zie de volgende afbeelding). Als u niet, begint u met de sectie [een SQL-database herstellen](backup-azure-sql-database.md#restore-a-sql-database).

![Klik op configureren om te herstellen configuratie menu openen](./media/backup-azure-sql-database/restore-overwrite-db.png)

De **Server** vervolgkeuzelijst bevat alleen de SQL-servers geregistreerd bij de Recovery Services-kluis. Als de server die u wilt dat zich niet in de **Server** lijst, Zie de sectie [detecteren SQL server-databases](backup-azure-sql-database.md#discover-sql-server-databases) de server te vinden. Tijdens het detectieproces voor database zijn nieuwe servers geregistreerd bij de Recovery Services-kluis.

1. In de **Restore Configuration** selecteert u **DB overschrijven** en klik op **OK** voltooid met het configureren van de bestemming. 

   ![Klik op de database overschrijven](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    De **Server**, **exemplaar**, en **databasenaam hersteld** dialoogvensters zijn niet nodig.

2. In de **herstelpunt selecteren** menu, kunt u een Logboeken (punt in tijd) of volledige & differentiële herstelpunt. Als u herstellen van een punt in tijd logboek wilt, gaat u verder met deze stap. Als u een volledige & differentiële herstelpunt herstellen wilt, gaat u verder met stap 3.

    ![Selecteer herstellen punt menu](./media/backup-azure-sql-database/recovery-point-menu.png)

    Punt in tijd terugzetten is alleen beschikbaar voor logboekback-ups voor databases met volledige en bulksgewijs geregistreerde herstelmodel. Een punt in tijd herstellen naar een specifieke tweede kiezen:

    1. Selecteer **Logboeken (punt in tijd)** als de optie voor terugzetten.

        ![Kies het type herstel](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Onder **datum/tijd herstellen**, klikt u op het kalenderpictogram om de kalender. Datums vet herstelpunten bevatten en de huidige datum is gemarkeerd. Selecteer een datum in de kalender met herstelpunten. U kunt geen datums met er zijn geen herstelpunten selecteren.

        ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Zodra u een datum selecteren, wordt de beschikbare herstelpunten weergegeven in de tijdlijngrafiek.

    3. Met behulp van de tijdlijngrafiek of het dialoogvenster tijd geeft u een specifieke tijd voor het herstelpunt en klik op **OK** om het herstelpunt stap te voltooien.
    
       ![agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        De **herstelpunt selecteren** menu wordt gesloten, en de **geavanceerde configuratie** menu wordt geopend.

       ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Van de **geavanceerde configuratie** menu:

        * Voor het behouden van de database niet-operationeel na het terugzetten, op de **herstellen met GEENHERSTEL** selecteert u **ingeschakeld**.
        * Als u wijzigen van de locatie op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **geavanceerde configuratie**.

    5. Op de **herstellen** menu, klikt u op **herstellen** starten van de hersteltaak. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen van taken volgen.

       ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-job-notification.png)

3. In de **herstelpunt selecteren** menu, kies een herstelpunt. U kunt een Logboeken (punt in tijd) of volledige & differentiële. Als u een point-in-time-logboek te herstellen wilt, gaat u terug naar stap 2. Deze stap herstelt een specifieke volledig of differentieel herstelpunt. Met deze optie is ziet u alle volledige en differentiële herstelpunten voor de afgelopen 30 dagen. Als u zien van de herstelpunten die ouder zijn dan 30 dagen wilt, klikt u op **Filter** openen de **Filter herstelpunten** menu. Als u een differentiële herstelpunt, Azure Backup herstelt eerst het juiste volledige herstelpunt en past vervolgens het geselecteerde differentiële herstelpunt.

    ![Selecteer herstellen punt menu](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. In de **herstelpunt selecteren** menu Selecteer **volledige & differentiële**.

       ![Selecteer herstellen punt menu](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        De lijst met beschikbare herstelpunten weergegeven.

    2. Selecteer een herstelpunt in de lijst van herstelpunten, en klikt u op **OK** herstelpunt procedure wilt uitvoeren. 

        ![de volledige herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        De **herstelpunt** menu wordt gesloten, en de **geavanceerde configuratie** menu wordt geopend.

        ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Van de **geavanceerde configuratie** menu:

        * Voor het behouden van de database niet-operationeel na het terugzetten, op de **herstellen met GEENHERSTEL** selecteert u **ingeschakeld**. **Herstellen met GEENHERSTEL** is standaard uitgeschakeld.
        * Als u wijzigen van de locatie op de doelserver wilt, geeft u een nieuw pad in de **doel** kolom.
        * Klik op **OK** voor het goedkeuren van de instellingen en sluit **geavanceerde configuratie**.

    4. Op de **herstellen** menu, klikt u op **herstellen** starten van de hersteltaak. U kunt de voortgang volgen in het gebied meldingen. U kunt ook de voortgang in de database herstellen van taken volgen.

       ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Azure Backup-bewerkingen van SQL op Azure VM's beheren

Deze sectie bevat informatie over de verschillende Azure Backup beheerbewerkingen beschikbaar voor SQL op virtuele machines in Azure. De volgende bewerkingen uit op hoog niveau bestaan:

* Taken controleren
* Back-waarschuwingen
* Stop de beveiliging op een SQL-database
* Beveiliging voor een SQL-database hervatten
* Een ad-hoc back-uptaak wordt geactiveerd
* Hef de registratie van een SQL-server

### <a name="monitor-jobs"></a>Taken controleren
Azure Backup wordt een bedrijfsoplossing klasse biedt geavanceerde back-up van waarschuwingen en meldingen fouten (Zie onderstaande sectie voor waarschuwingen voor back-up). Als u nog steeds wilt bewaken van specifieke taken kunt u een van de volgende opties op basis van uw behoeften:

#### <a name="using-azure-portal---recovery-services-vault-for-all-ad-hoc-operations"></a>Met Azure portal -> Recovery Services-kluis voor alle ad-hoc-bewerkingen
Azure back-up bevat alle handmatig geactiveerd of ad-hoc, taken in de portal Backup-taken. De taken die beschikbaar zijn in de portal opnemen: alle back-upbewerkingen configureren, handmatig geactiveerd back-upbewerkingen, terugzetbewerkingen, registratie en databasebewerkingen detecteren en back-upbewerkingen stoppen. 
![Geavanceerde configuratie menu](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Alle geplande back-uptaken inclusief volledige, differentiële en logboek back-up wordt niet weergegeven in de portal en kan worden gecontroleerd met behulp van SQL Server Management Studio, zoals hieronder wordt beschreven.
>

#### <a name="using-sql-server-management-studio-ssms-for-backup-jobs"></a>Met behulp van SQL Server Management Studio (SSMS) voor back-uptaken
Azure Backup gebruikt systeemeigen SQL-API's voor alle back-upbewerkingen. Met de systeemeigen API's, kunt u alle informatie over de taak uit ophalen de [SQL-back-upset tabel](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in de msdb-database. 

U kunt de onderstaande query als voorbeeld voor het ophalen van alle back-uptaken voor een specifieke database met naam 'DB1'. U kunt de onderstaande query voor meer geavanceerde bewaking.
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

### <a name="backup-alerts"></a>Back-waarschuwingen

Logboekback-ups om de 15 minuten plaatsvindt, bewaking van de back-uptaken van tijd tot tijd worden omslachtig. Azure Backup zijn gepland voor deze situatie mogelijk omslachtig dankzij de e-mailwaarschuwingen geactiveerd door een back-upfouten. Waarschuwingen worden geconsolideerd op het databaseniveau van de-foutcode. Bijvoorbeeld als een database heeft meerdere back-upfouten in plaats van de ontvangst van de waarschuwing voor elke mislukt, ontvangt u e-mailadres voor de eerste fout. Vervolgens kunt u aanmelden bij de Azure-portal voor het bewaken van de volgende fouten voor die database. 

Back-waarschuwingen bewaken:

1. Meld u aan bij uw Azure-abonnement in de [Azure-portal](https://portal.azure.com).

2. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

3. Selecteer in het menu Recovery Services-kluis **waarschuwingen en gebeurtenissen**. 

   ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. In de **waarschuwingen en gebeurtenissen** selecteert u **waarschuwingen voor back-up** om de lijst met waarschuwingen weer te geven.

   ![Geavanceerde configuratie menu](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Stop de beveiliging op een SQL Server-database

Als u de beveiliging van een SQL Server-database stopt, wordt Azure Backup gevraagd als u wilt de herstelpunten bewaren. Er zijn twee manieren om te stoppen met het beveiligen van SQL-database:

* Alle toekomstige back-uptaken stoppen en verwijderen van alle herstelpunten
* Alle toekomstige back-uptaken stoppen, maar laat u de herstelpunten 

Als u de herstelpunten zijn voor de kosten als de herstelpunten voor SQL transport van het SQL-exemplaar beveiligde prijzen kosten, plus de opslag verbruikt. Zie voor meer informatie over de prijzen van Azure Backup voor SQL, de [Azure Backup pagina met prijzen](https://azure.microsoft.com/pricing/details/backup/). Beveiliging voor de database stoppen:

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. Selecteer in het kluisdashboard **gebruik** Items voor back-up om de back-Upitems menu te openen.

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. In de **back-ups** menu, selecteer het type back-up management **SQL in Azure VM**. 

    ![Klik op + back-up naar de back-up doel menu openen](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    De lijst met Items voor back-up wordt aangepast om de lijst weergeven met de SQL-databases. 

4. Selecteer de database die u wilt stoppen met het beveiligen van in de lijst van SQL-databases.

    ![SQL in Azure VM selecteren in lijst](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Wanneer u de database selecteert, wordt de menu wordt geopend. 

5. Klik in het menu van de geselecteerde database **back-up stoppen** te stoppen met het beschermen van de database.

    ![Herstel de database selecteren](./media/backup-azure-sql-database/stop-db-button.png)

    De **back-up stoppen** menu wordt geopend.

6. In de **back-up stoppen** menu wilt back-up gegevens behouden of verwijderen van de back-upgegevens. U kunt desgewenst een reden voor het stoppen van de beveiliging en een opmerking opgeven.

    ![Herstel de database selecteren](./media/backup-azure-sql-database/stop-backup-button.png)

7. Klik op **back-up stoppen** Stop de beveiliging op de database. 

### <a name="resume-protection-for-a-sql-database"></a>Beveiliging voor een SQL-database hervatten

Als de **back-up gegevens behouden** optie is geselecteerd bij de beveiliging moet worden gestopt voor de SQL-database, is het mogelijk te beveiliging hervatten. Als de back-upgegevens niet behouden is, kan beveiliging niet hervatten. 

1. Als u wilt hervatten beveiliging voor de SQL-database, opent u het back-item en klik op **back-up hervatten**.

    ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/resume-backup-button.png)

   Het menu van de Backup-beleid wordt geopend.

2. Van de **Backup-beleid** menu, selecteert u een beleid en op **opslaan**.

### <a name="trigger-an-adhoc-backup"></a>Activeert een ad-hoc back-up

Als u wilt, kunt u een ad-hoc back-up activeren. Er zijn vier typen van ad-hoc back-ups. Zie voor meer informatie over elk type, het artikel [typen van de SQL-back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Volledige back-up
* Alleen volledige back-up kopiëren
* Differentiële back-up
* Logboekback-up

### <a name="unregister-a-sql-server"></a>Hef de registratie van een SQL-Server

Registratie van een SQL server na het verwijderen van beveiliging, maar voordat de kluis verwijderen

1. Open de Recovery Services-kluis geregistreerd bij de virtuele machine van SQL.

2. In de **beheren** sectie van de kluismenu, klikt u op **back-upinfrastructuur**.  

   ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. In de **beheerservers** sectie, klikt u op **beveiligde Servers**.

   ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/protected-servers.png)

    Het menu beveiligde Servers wordt geopend. 

4. In de **beveiligde Servers** menu, selecteert u de registratie van de gewenste server. Als u verwijderen van de kluis wilt, moet u de registratie ongedaan maken voor alle servers.

5. In het menu beveiligde Servers met de rechtermuisknop op de beveiligde server en selecteer **verwijderen**. 

   ![Databasebeveiliging hervatten](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Backup het PowerShell-voorbeeld voor het maken van back-ups van versleutelde virtuele machines.

> [!div class="nextstepaction"]
> [Back-up maken van een versleutelde VM](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
