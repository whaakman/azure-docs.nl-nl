---
title: Een back-up van SQL Server-databases maken in Azure | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u een back-up van SQL Server maakt in Azure. In het artikel wordt ook uitgelegd hoe u SQL Server kunt herstellen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: raynew
ms.openlocfilehash: 8e7e5d871fa1bb557de4e6fce22658115bf0fe94
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806979"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Over SQL Server-back-ups in virtuele Azure-machines

SQL Server-databases zijn essentiële workloads waarvoor een laag Recovery Point Objective (RPO, beoogd herstelpunt) en langetermijnretentie nodig zijn. U kunt back-up van SQL Server-databases die worden uitgevoerd op virtuele Azure-machines met behulp van [Azure Backup](backup-overview.md).

## <a name="backup-process"></a>Back-upproces

Deze oplossing maakt gebruik van de SQL native-API's als u wilt maken van back-ups van uw SQL-databases.

* Nadat u de SQL Server-VM die u wilt beveiligen en op te vragen voor de databases in deze opgeeft, Azure Backup-service de Backup-extensie van een werkbelasting op de virtuele machine wordt geïnstalleerd door de naam van de `AzureBackupWindowsWorkload`  extensie.
* Deze extensie bestaat uit een coördinator en een SQL-invoegtoepassing. Terwijl de coördinator verantwoordelijk is voor het activeren van werkstromen voor verschillende bewerkingen zoals back-up, back-up en herstel configureren, is de invoegtoepassing is verantwoordelijk voor het werkelijke gegevensstroom.
* Als u voor het detecteren van databases op deze virtuele machine, maakt Azure Backup de account `NT SERVICE\AzureWLBackupPluginSvc`. Dit account wordt gebruikt voor back-up en herstel en SQL sysadmin-machtigingen vereist. Azure Backup maakt gebruik van de `NT AUTHORITY\SYSTEM` account voor database-detectie/query, zodat dit account hoeft te worden van een openbare aanmelding via SQL. Als u de SQL Server-VM hebt gemaakt vanuit de Azure Marketplace, ontvangt u mogelijk een foutbericht **UserErrorSQLNoSysadminMembership**. Als dit het geval [Volg deze instructies](backup-azure-sql-database.md).
* Zodra u trigger beveiliging configureert voor de geselecteerde databases, stelt de Backup-service u de coördinator met de back-upschema's en andere beleidsdetails, waarvan de extensie caches lokaal op de virtuele machine 
* De coördinator communiceert met de invoegtoepassing op het geplande tijdstip, en start de back-upgegevens van de SQL-server met behulp van VDI streaming.  
* De invoegtoepassing verzendt die gegevens rechtstreeks naar de recovery services-kluis, waardoor dus niet nodig voor een tijdelijke locatie. De gegevens worden versleuteld en opgeslagen door de Azure Backup-service in de storage-accounts.
* Wanneer de overdracht van gegevens voltooid is, bevestigt coordinator doorvoeren met de Backup-service.

  ![SQL-back-architectuur](./media/backup-azure-sql-database/backup-sql-overview.png)

## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, controleert u of de onderstaande:

1. Zorg ervoor dat er een SQL Server-exemplaar wordt uitgevoerd in Azure. U kunt [snel een SQL Server-exemplaar maken](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md) in de marketplace.
2. Controleer de [functie overweging](#feature-consideration-and-limitations) en [scenario ondersteuning](#scenario-support).
3. [Lees de veelgestelde vragen](faq-backup-sql-server.md) over dit scenario.

## <a name="scenario-support"></a>Scenario-ondersteuning

**Ondersteuning** | **Details**
--- | ---
**Ondersteunde implementaties** | SQL Marketplace Azure-VM's en niet-Marketplace-VM's (SQL Server handmatig geïnstalleerd) worden ondersteund.
**Ondersteunde geografische gebieden** | Australië-Zuidoost (ASE), Oost-Australië (AE) <br> Brazilië - zuid (BRS)<br> Canada-centraal (CNC), Canada-Oost (CE)<br> Zuidoost-Azië (SEA), Oost-Azië (EA) <br> VS-Oost (EUS), VS-Oost 2 (EUS2), West Centraal (WCUS), VS-West (WUS); VS-West 2 (WUS 2) Noord-centraal VS (NCUS) VS-centraal (CUS) VS Zuid-centraal (SCUS) <br> India centraal (INC), India-Zuid (INS) <br> Japan East (JPE), Japan West (JPW) <br> Korea-centraal (KRC), Korea-Zuid (KRS) <br> Noord-Europa (NE), West-Europa <br> UK-Zuid (UKS), VK-West (UKW)
**Ondersteunde besturingssystemen** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux wordt momenteel niet ondersteund.
**Ondersteunde SQL Server-versies** | SQL Server 2017 als gedetailleerde [hier](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017), SQL Server 2016 en SP's als gedetailleerde [hier](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack), SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.
**Ondersteunde versies van .NET** | .NET framework 4.5.2 en hoger is geïnstalleerd op de virtuele machine

### <a name="support-for-sql-server-2008-and-sql-server-2008-r2"></a>Ondersteuning voor SQL Server 2008 en SQL Server 2008 R2

Azure Backup heeft onlangs aangekondigd voor ondersteuning voor [EOS SQL-servers](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support) -SQL Server 2008 en SQL Server 2008 R2. De oplossing is momenteel in preview voor EOS SQL Server en ondersteunt de volgende configuratie:

1. SQL Server 2008 en SQL Server 2008 R2 die worden uitgevoerd op Windows 2008 R2 SP1
2. .NET framework 4.5.2 of hoger moet zijn geïnstalleerd op de virtuele machine
3. Back-up voor FCI en gespiegelde databases wordt niet ondersteund

Gebruikers worden niet in rekening gebracht voor deze functie tot het moment dat deze algemeen beschikbaar is. Alle andere [functie overwegingen en beperkingen](#feature-consideration-and-limitations) zijn van toepassing op deze versies ook. Geval is, kunt u verwijzen naar de [vereisten](backup-sql-server-database-azure-vms.md#prerequisites) voordat u de beveiliging van SQL Server 2008 en 2008 R2, waaronder instelling configureren de [registersleutel](backup-sql-server-database-azure-vms.md#add-registry-key-to-enable-registration) (deze stap zou niet zijn vereist wanneer de functie in het algemeen is beschikbaar).


## <a name="feature-consideration-and-limitations"></a>Functie overwegingen en beperkingen

- SQL Server back-up kan worden geconfigureerd in Azure portal of **PowerShell**. We bieden geen ondersteuning voor CLI.
- De oplossing wordt ondersteund op beide typen van [implementaties](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) -virtuele machines van Azure Resource Manager en klassieke virtuele machines.
- Virtuele machine met SQL Server moet verbinding met internet voor toegang tot Azure openbare IP-adressen.
- SQL Server **Failover Cluster exemplaar (FCI)** en SQL Server Always on Failover Cluster Instance worden niet ondersteund.
- Back-up en herstelbewerkingen voor gespiegelde databases en momentopnamen van databases worden niet ondersteund.
- Met behulp van meer dan een back-upoplossingen back-up uw zelfstandige SQL Server kan-exemplaar of SQL Always on-beschikbaarheidsgroep leiden tot back-upfouten; in dat geval weerhouden.
- Back-ups van twee knooppunten van een beschikbaarheidsgroep afzonderlijk met dezelfde of verschillende oplossingen, kunnen ook leiden tot back-upfouten.
- Azure Backup ondersteunt alleen volledige en kopie-alleen volledige back-uptypen voor **alleen-lezen** databases
- Databases met zeer veel bestanden kunnen niet worden beveiligd. Het maximum aantal bestanden dat wordt ondersteund is **~ 1000**.  
- U kunt maximaal back **ongeveer 2000** SQL Server-databases in een kluis. Als u een groter aantal databases hebt, kunt u meerdere kluizen maken.
- U kunt back-up voor maximaal **50** databases in een gaan; deze beperking kunt u back-belastingen optimaliseren.
- We bieden ondersteuning voor databases tot **2TB** in grootte; voor groter dan die van prestatieproblemen kunnen afkomstig zijn.
- Als u een idee van de garantie voor het aantal databases per server kunnen worden beveiligd, moeten we Houd rekening met factoren zoals bandbreedte, VM-grootte, back-upfrequentie, de grootte van de database, enzovoort. We werken op een planner waarmee u deze getallen op uw eigen berekenen. We zullen worden gepubliceerd binnenkort.
- Back-ups zijn in het geval van beschikbaarheidsgroepen afkomstig uit de verschillende knooppunten op basis van een aantal factoren. Hieronder staat een samenvatting van de back-gedrag voor een beschikbaarheidsgroep.

### <a name="back-up-behavior-in-case-of-always-on-availability-groups"></a>Back-up gedrag in het geval van altijd op beschikbaarheidsgroepen

Het wordt aanbevolen dat de back-up is geconfigureerd op slechts één knooppunt van een Beschikbaarheidsgroep. Back-up moet altijd worden geconfigureerd in dezelfde regio als het primaire knooppunt. Met andere woorden, moet u altijd het primaire knooppunt aanwezig zijn in de regio waarin u back-up wilt configureren. Als alle knooppunten van de Beschikbaarheidsgroep zich in dezelfde regio bevinden waarin de back-up is geconfigureerd, is er geen vragen.

**Voor de regio-overschrijdende AG**
- Back-ups niet, ongeacht de back-upvoorkeur uitgevoerd van de knooppunten die zich niet in dezelfde regio bevinden waarin de back-up is geconfigureerd. Dit komt doordat de regio-overschrijdende back-ups worden niet ondersteund. Als u alleen 2 knooppunten hebt en het secundaire knooppunt in de andere regio is; in dit geval wordt de back-ups blijven optreden van het primaire knooppunt (tenzij uw back-upvoorkeur 'alleen secundaire').
- Als het geval is een failover naar een regio die anders is dan de waarin de back-up is geconfigureerd, wordt back-ups mislukken op de knooppunten in de regio van de failover.

Back-ups zijn afhankelijk van de back-upvoorkeur en typen van back-ups (volledige/differentiële/log/kopie-alleen volledig) afkomstig uit een bepaald knooppunt (primair/secundair).

- **Back-upvoorkeur: Primary**

**Type back-up** | **Node**
    --- | ---
    Volledig | Primair
    Differentiële | Primair
    logboek |  Primair
    Kopie-alleen volledige |  Primair

- **Back-upvoorkeur: Secondary Only**

**Type back-up** | **Node**
--- | ---
Volledig | Primair
Differentiële | Primair
logboek |  Secundair
Kopie-alleen volledige |  Secundair

- **Back-upvoorkeur: Secondary**

**Type back-up** | **Node**
--- | ---
Volledig | Primair
Differentiële | Primair
logboek |  Secundair
Kopie-alleen volledige |  Secundair

- **Geen voorkeur back-up**

**Type back-up** | **Node**
--- | ---
Volledig | Primair
Differentiële | Primair
logboek |  Secundair
Kopie-alleen volledige |  Secundair

## <a name="set-vm-permissions"></a>VM-machtigingen instellen

  Wanneer u detectie op een SQL Server uitvoert, is Azure Backup doet het volgende:

* De extensie AzureBackupWindowsWorkload toegevoegd.
* Hiermee maakt u een account NT SERVICE\AzureWLBackupPluginSvc voor het detecteren van databases op de virtuele machine. Dit account wordt gebruikt voor een back-up en herstellen en SQL sysadmin-machtigingen vereist.
* Databases die worden uitgevoerd op een virtuele machine, detecteert de account NT AUTHORITY\SYSTEM maakt gebruik van Azure Backup. Dit account moet een openbare-aanmelding op SQL.

Als u de SQL Server-VM hebt gemaakt in de Azure Marketplace of als u van SQL 2008 en 2008 R2 gebruikmaakt, ontvangt u mogelijk een **UserErrorSQLNoSysadminMembership** fout.

Voor het verlenen van machtigingen in geval van **SQL 2008** en **2008 R2** uitgevoerd op Windows 2008 R2, verwijzen [hier](#give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2).

Corrigeer machtigingen met de volgende stappen uit voor alle andere versies:

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

> [!NOTE]
> Als uw SQL-Server meerdere exemplaren van SQL Server is geïnstalleerd heeft, wordt u sysadmin-machtigingen voor moet toevoegen **NT Service\AzureWLBackupPluginSvc** account op alle SQL-exemplaren.

### <a name="give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2"></a>SQL sysadmin-machtigingen geven voor SQL 2008 en SQL 2008 R2

Voeg **NT AUTHORITY\SYSTEM** en **NT Service\AzureWLBackupPluginSvc** aanmeldingen bij de SQL Server-exemplaar:

1. Ga op de SQL Server-exemplaar in Object explorer.
2. Ga naar Security aanmeldingen ->
3. Klik met de rechtermuisknop op de aanmeldingen en klikt u op *nieuwe aanmelding...*

    ![Nieuwe aanmelding met behulp van SSMS](media/backup-azure-sql-database/sql-2k8-new-login-ssms.png)

4. Ga naar het tabblad Algemeen en voer **NT AUTHORITY\SYSTEM** als de naam van de aanmelding.

    ![aanmeldingsnaam voor SSMS](media/backup-azure-sql-database/sql-2k8-nt-authority-ssms.png)

5. Ga naar *serverfuncties* en kies *openbare* en *sysadmin* rollen.

    ![rollen kiezen in SSMS](media/backup-azure-sql-database/sql-2k8-server-roles-ssms.png)

6. Ga naar *Status*. *Verleen* de machtiging voor het verbinding maken met database-engine en meld u aan als *ingeschakeld*.

    ![Machtigingen verlenen in SSMS](media/backup-azure-sql-database/sql-2k8-grant-permission-ssms.png)

7. Klik op OK.
8. Herhaal stappen (1-7 hierboven) in dezelfde volgorde NT Service\AzureWLBackupPluginSvc aanmelding toevoegen aan de SQL Server-exemplaar. Als de aanmelding al bestaat, zorg ervoor dat de serverrol sysadmin en onder Status van het verlenen van machtiging voor het verbinding maken met database-engine en meld u aan als ingeschakeld heeft.
9. Na het toewijzen van machtigingen, **DB's opnieuw detecteren** in de portal: Kluis **->** back-up maken van infrastructuur **->** werkbelasting in Azure-VM:

    ![DB's opnieuw detecteren in Azure portal](media/backup-azure-sql-database/sql-rediscover-dbs.png)

U kunt ook automatiseren zodat de machtigingen door het uitvoeren van de volgende PowerShell-opdrachten in de beheermodus. Naam van het exemplaar is standaard ingesteld op MSSQLSERVER. Wijzig de instantienaam argument in script als moet worden:

```powershell
param(
    [Parameter(Mandatory=$false)]
    [string] $InstanceName = "MSSQLSERVER"
)
if ($InstanceName -eq "MSSQLSERVER")
{
    $fullInstance = $env:COMPUTERNAME   # In case it is the default SQL Server Instance
}
else
{
    $fullInstance = $env:COMPUTERNAME + "\" + $InstanceName   # In case of named instance
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT Service\AzureWLBackupPluginSvc', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT AUTHORITY\SYSTEM', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
```


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over](backup-sql-server-database-azure-vms.md) back-ups van SQL Server-databases.
* [Meer informatie](restore-sql-database-azure-vm.md) over het herstellen van SQL Server-databases vanuit back-up.
* [Meer informatie](manage-monitor-sql-database-backup.md) over het beheren van SQL Server-databases vanuit back-up.
