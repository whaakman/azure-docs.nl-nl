---
title: Maak een back-up van SQL Server-databases in virtuele Azure-machines | Microsoft Docs
description: Meer informatie over het back-up van SQL Server-databases in virtuele Azure-machines
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: sachdevaswati
ms.openlocfilehash: 0307dc5c83782119f6c10279563b8b9f0a999d28
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236882"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Back-ups maken van SQL Server-databases in virtuele Azure-machines

SQL Server-databases zijn essentiële workloads waarvoor een lage herstelpunt objective (RPO) en langetermijnbehoud. U kunt back-up van SQL Server-databases op Azure virtual machines (VM's) met behulp van [Azure Backup](backup-overview.md).

Dit artikel leest hoe u back-up van een SQL Server-database die wordt uitgevoerd op een Azure-VM naar een Azure Backup Recovery Services-kluis.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> * Een kluis maken en configureren.
> * Databases detecteren en back-ups instellen.
> * Automatische beveiliging van databases instellen.


## <a name="prerequisites"></a>Vereisten

Voordat u back-up van een SQL Server-database, controleert u de volgende criteria:

1. Zoek of maak een [Recovery Services-kluis](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) in dezelfde regio of land als de virtuele machine die als host fungeert voor de SQL Server-exemplaar.
2. Controleer de [VM-machtigingen die nodig zijn](backup-azure-sql-database.md#fix-sql-sysadmin-permissions) back-up SQL-databases.
3. Controleer of de virtuele machine heeft [netwerkverbinding](backup-sql-server-database-azure-vms.md#establish-network-connectivity).
4. Zorg ervoor dat de SQL Server-databases volgt de [richtlijnen voor de naamgeving voor Azure Backup database](#database-naming-guidelines-for-azure-backup).
5. Controleer of er geen andere back-oplossingen worden ingeschakeld voor de database. Alle andere SQL Server back-ups uitschakelen voordat u back-up van de database.

> [!NOTE]
> U kunt Azure Backup inschakelen voor een Azure-VM en ook voor een SQL Server-database die wordt uitgevoerd op de virtuele machine zonder conflict.


### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

Voor alle bewerkingen vereist een SQL Server-VM verbinding met Azure openbare IP-adressen. VM-bewerkingen (databasedetectie configureren van back-ups, back-ups plannen, herstelpunten herstellen, enzovoort) mislukken zonder verbinding met Azure openbare IP-adressen.

Verbinding maken met behulp van een van de volgende opties:

- **Toestaan dat de IP-adresbereiken Azure datacenter**. Met deze optie kunt [IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653) in de download. Voor toegang tot een netwerkbeveiligingsgroep (NSG), gebruikt u de cmdlet Set-AzureNetworkSecurityRule. Als u geen veilige ontvangers lijst alleen regiospecifieke IP-adressen, moet u ook om bij te werken de lijst met veilige geadresseerden de servicetag Azure Active Directory (Azure AD) als verificatie wilt inschakelen.

- **Toegang met behulp van labels NSG**. Als u nsg's gebruiken om te beperken van connectiviteit, wordt deze optie een regel toegevoegd aan uw NSG waarmee uitgaande toegang tot Azure back-up met behulp van de AzureBackup-tag. Naast deze tag kunt u moet ook overeenkomen [regels](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) voor Azure AD en Azure Storage om toe te staan connectiviteit voor verificatie en gegevensoverdracht. De AzureBackup-tag is alleen op dit moment beschikbaar zijn in PowerShell. Een regel maken met behulp van de AzureBackup-tag:

    - Referenties voor Azure-account toevoegen en bijwerken van de nationale clouds<br/>
    `Add-AzureRmAccount`

    - Selecteer het abonnement NSG<br/>
    `Select-AzureRmSubscription "<Subscription Id>"`

     - Selecteer de NSG<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

    - Voeg uitgaande regel voor Azure Backup-servicetag toestaan<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

  - De Netwerkbeveiligingsgroep opslaan<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`
- **Toegang toestaan door het gebruik van Azure-Firewall tags**. Als u Azure-Firewall, een toepassing-regel maken met behulp van de AzureBackup [FQDN tag](https://docs.microsoft.com/azure/firewall/fqdn-tags). Hiermee kunt uitgaande toegang tot Azure Backup.
- **Een HTTP-proxy-server implementeren om verkeer te routeren**. Wanneer u een back-up van een SQL Server-database maakt op een Azure VM, gebruikt de back-upextensie op de virtuele machine de HTTPS-API's voor het verzenden van opdrachten voor beheer met Azure Backup en gegevens naar Azure Storage. Azure AD de Backup-extensie gebruikt voor verificatie. Leid het verkeer van de back-upextensie voor deze drie services via de HTTP-proxy. De extensies zijn het enige onderdeel dat geconfigureerd voor toegang tot het openbare internet.

Opties voor netwerkconnectiviteit zijn onder andere de volgende voordelen en nadelen:

**Optie** | **Voordelen** | **Nadelen**
--- | --- | ---
IP-bereiken toestaan | Er zijn geen extra kosten | Ingewikkeld om te beheren, omdat de IP-adresbereiken gedurende een periode wijzigen <br/><br/> Biedt toegang tot het geheel van Azure, niet alleen Azure Storage
NSG-servicetags gebruiken | Gemakkelijker te beheren als bereik wijzigingen automatisch worden samengevoegd <br/><br/> Er zijn geen extra kosten <br/><br/> | Alleen kunnen met nsg's worden gebruikt <br/><br/> Biedt toegang tot de hele service
Azure Firewall FQDN tags gebruiken | Gemakkelijker te beheren als de vereiste FQDN's worden automatisch beheerd. | Kan worden gebruikt met Azure-Firewall alleen
Een HTTP-proxy gebruiken | Nauwkeurige controle in de proxy die over de opslag URL's is toegestaan. <br/><br/> Toegang tot virtuele machines één punt van internet <br/><br/> Afhankelijk van de Azure-IP-adres verandert niet | Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware

### <a name="set-vm-permissions"></a>VM-machtigingen instellen

Bij het configureren van een back-up voor een SQL Server-database, doet Azure Backup het volgende:

- De extensie AzureBackupWindowsWorkload toegevoegd.
- Hiermee maakt u een account NT SERVICE\AzureWLBackupPluginSvc voor het detecteren van databases op de virtuele machine. Dit account wordt gebruikt voor een back-up en herstellen en SQL sysadmin-machtigingen vereist.
- Databases die worden uitgevoerd op een virtuele machine, detecteert de account NT AUTHORITY\SYSTEM maakt gebruik van Azure Backup. Dit account moet een openbare-aanmelding op SQL.

Als u de SQL Server-VM hebt gemaakt in de Azure Marketplace, ontvangt u mogelijk een foutbericht UserErrorSQLNoSysadminMembership. Zie voor meer informatie, de functie overwegingen en beperkingen sectie gevonden in [over SQL Server back-up in Azure VM's](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

### <a name="database-naming-guidelines-for-azure-backup"></a>Richtlijnen voor de naamgeving voor Azure Backup database

Vermijd het gebruik van de volgende elementen in de databasenamen van de:

  * Afsluitende en voorloopspaties
  * Afsluitende aanhalingstekens uitroepteken (!)
  * De afsluiting vierkante haken (])
  * Door puntkomma's (;)
  * Doorsturen schuine streep (/)

Aliasing is beschikbaar voor niet-ondersteunde tekens, maar u kunt het beste geen ze. Zie [Het gegevensmodel van de tabelservice](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN) voor meer informatie.


[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server-databases detecteren

Het detecteren van databases op een virtuele machine worden uitgevoerd:

1. In de [Azure-portal](https://portal.azure.com) opent u de Recovery Services-kluis die u gebruikt om een back-up te maken van de database.

2. In de **Recovery Services-kluis** dashboard, selecteer **back-up**.

   ![Backup selecteren om het menu Doel van de back-up te openen](./media/backup-azure-sql-database/open-backup-menu.png)

3. In **back-updoel**, stel **waar wordt uw werkbelasting uitgevoerd?** naar **Azure**.

4. In **Waarvan wilt u een back-up maken?** selecteert u **SQL Server in Azure-VM**.

    ![SQL Server in Azure VM voor de back-up selecteren](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. In **Doel van de back-up** > **DB's detecteren in VM's** selecteert u **Detectie starten** om te zoeken naar niet-beveiligde VM's in het abonnement. Deze zoekopdracht kunt tijd duren, afhankelijk van het aantal niet-beveiligde virtuele machines in het abonnement.

   - Niet-beveiligde virtuele machines zouden na detectie in de lijst moeten verschijnen, gesorteerd op naam en resourcegroep.
   - Als een virtuele machine niet wordt weergegeven zoals verwacht, kunt u zien of deze al gemaakt om in een kluis.
   - Meerdere virtuele machines kunnen dezelfde naam hebben, maar ze moeten behoren tot verschillende resourcegroepen bevinden.

     ![Back-up in behandeling tijdens het zoeken naar databases in VM's](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Selecteer in de lijst met virtuele machines de VM waarop de SQL Server-database wordt uitgevoerd > **DB's detecteren**.

7. Database-detectie bijhouden in **meldingen**. De tijd die nodig is voor deze actie is afhankelijk van het aantal VM-databases. Wanneer de geselecteerde databases zijn gedetecteerd, wordt er een slagingsbericht weergegeven.

    ![Bericht dat de implementatie is geslaagd](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Backup detecteert alle SQL Server-databases op de virtuele machine. Tijdens de detectie gebeuren de volgende elementen op de achtergrond:

    - Azure Backup wordt de virtuele machine geregistreerd bij de kluis voor back-up van de werkbelasting. Alle databases op de geregistreerde VM kunnen een back-worden gemaakt bij deze kluis alleen.
    - Azure Backup wordt de extensie AzureBackupWindowsWorkload geïnstalleerd op de virtuele machine. Geen agent is geïnstalleerd op een SQL-database.
    - Azure Backup maakt het serviceaccount NT Service\AzureWLBackupPluginSvc op de virtuele machine.
      - Het serviceaccount wordt gebruikt voor alle back-up- en herstelbewerkingen.
      - NT-Service\AzureWLBackupPluginSvc vereist SQL sysadmin-machtigingen. Alle SQL Server-VM's gemaakt in de Marketplace worden geleverd met de SqlIaaSExtension geïnstalleerd. De extensie AzureBackupWindowsWorkload maakt gebruik van de SQLIaaSExtension om automatisch de vereiste machtigingen.
    - Als u de virtuele machine hebt gemaakt vanuit de Marketplace, de virtuele machine geen de SqlIaaSExtension geïnstalleerd en de detectiebewerking is mislukt met het foutbericht UserErrorSQLNoSysAdminMembership. U kunt dit probleem oplossen, gaat u als volgt de [instructies](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

        ![De VM en database selecteren](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Back-up configureren  

1. In **back-Updoelstelling** > **stap 2: Back-up configureren**, selecteer **back-up configureren**.

   ![Back-up configureren selecteren](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. In **items selecteren om back-up te**, ziet u alle geregistreerde beschikbaarheidsgroepen en zelfstandige SQL Server-exemplaren. Selecteer de pijl links van een rij in de lijst met alle niet-beveiligde databases in die instantie of AlwaysOn-beschikbaarheidsgroep uitbreiden.  

    ![Alle SQL Server-exemplaren met zelfstandige databases weergeven](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Kies alle databases die u wilt beveiligen, en selecteer vervolgens **OK**.

   ![De database beveiligen](./media/backup-azure-sql-database/select-database-to-protect.png)

   Ter optimalisering van de back-upbelastingen stelt Azure Backup het maximumaantal databases in één back-uptaak in op 50.

     * Om meer dan 50 back-ups te beschermen moet u meerdere databases configureren.
     * Om in te schakelen [ ](#enable-auto-protection) het gehele exemplaar of de Always On-beschikbaarheidsgroep. In de **AUTOPROTECT** vervolgkeuzelijst, selecteer **ON**, en selecteer vervolgens **OK**.

    > [!NOTE]
    > De [automatische beveiliging](#enable-auto-protection) onderdeel kunt u niet alleen beveiliging op alle bestaande databases in één keer, maar geen nieuwe databases die zijn toegevoegd aan dit exemplaar of de beschikbaarheidsgroep worden ook automatisch beveiligd.  

4. Selecteer **OK** openen **back-upbeleid**.

    ![Inschakelen van automatische beveiliging voor de AlwaysOn-beschikbaarheidsgroep](./media/backup-azure-sql-database/enable-auto-protection.png)

5. In **back-upbeleid**, kiest u een beleid en selecteer vervolgens **OK**.

   - Selecteer het standaardbeleid als HourlyLogBackup.
   - Een bestaand back-upbeleid kiezen dat u eerder hebt gemaakt voor SQL.
   - Definieer een nieuw beleid op basis van uw bereik RPO en retentie.

     ![Back-upbeleid selecteren](./media/backup-azure-sql-database/select-backup-policy.png)

6. In **back-up**, selecteer **back-up inschakelen**.

    ![Het gekozen back-upbeleid inschakelen](./media/backup-azure-sql-database/enable-backup-button.png)

7. Bijhouden van de voortgang van de configuratie in de **meldingen** gebied van de portal.

    ![Systeemvak](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Maak een back-upbeleid

Een back-upbeleid bepaalt wanneer back-ups worden gemaakt en hoe lang ze worden bewaard.

- Een beleid wordt gemaakt op kluisniveau.
- U kunt hetzelfde back-upbeleid gebruiken voor meerdere kluizen, maar u moet het back-upbeleid toepassen op elke kluis.
- Wanneer u een back-upbeleid maakt, is een dagelijkse volledige back-up de standaardinstelling.
- U kunt een differentiële back-up toevoegen, maar alleen als u een wekelijkse volledige back-up configureert.
- Meer informatie over [verschillende typen back-upbeleid](backup-architecture.md#sql-server-backup-types).

Ga als volgt te werk om een back-upbeleid te maken:

1. Selecteer in de kluis **back-upbeleid** > **toevoegen**.
2. In **toevoegen**, selecteer **SQL Server in virtuele Azure-machine** voor het definiëren van het beleidstype.

   ![Een beleidstype voor het nieuwe back-upbeleid kiezen](./media/backup-azure-sql-database/policy-type-details.png)

3. Geef bij **Beleidsnaam** een naam voor het nieuwe beleid op.
4. In **volledige back-upbeleid**, selecteer een **back-upfrequentie**. Kies een **dagelijkse** of **wekelijkse**.

   - Als u **Dagelijks** kiest, selecteert u het tijdstip en de tijdzone waarop de back-uptaak moet worden gestart.
   - Als u **Wekelijks** kiest, selecteert u de dag van de week, het tijdstip en de tijdzone waarop de back-uptaak moet worden gestart.
   - Een volledige back-up, worden uitgevoerd omdat u niet uitschakelen de **volledige back-up** optie.
   - Selecteer **volledige back-up** om het beleid weer te geven.
   - U kunt geen differentiële back-ups maken voor dagelijkse volledige back-ups.

     ![Velden voor nieuw back-upbeleid](./media/backup-azure-sql-database/full-backup-policy.png)  

5. In **BEWAARTERMIJN**, alle opties zijn standaard geselecteerd. Een bewaartermijn beperkt wissen waarmee u niet wilt, en stel vervolgens de intervallen dat u wilt gebruiken.

    - Minimale bewaartermijn voor elk type back-up (volledige, differentiële en logboekbestanden) is zeven dagen.
    - Herstelpunten worden getagd voor retentie op basis van de bewaarperiode. Als u een dagelijkse volledige back-up selecteert, wordt slechts één volledige back-up per dag geactiveerd.
    - De back-up voor een specifieke dag is gemarkeerd en bewaard op basis van de wekelijkse bewaartermijn en de instelling van de wekelijkse bewaren.
    - Maandelijkse en jaarlijkse bewaartermijnen gedragen zich op een soortgelijke manier.

       ![Intervalinstellingen voor bewaarperiode](./media/backup-azure-sql-database/retention-range-interval.png)

6. In het menu voor het **beleid voor een volledige back-up** selecteert u **OK** om de instellingen te accepteren.
7. Als u een beleid voor een differentiële back-up wilt toevoegen, selecteert u **Differentiële back-up**.

   ![Intervalinstellingen voor bewaarperiode](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Het menu voor het beleid voor een differentiële back-up openen](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. In **Beleid voor een differentiële back-up** selecteert u **Inschakelen** om de frequentie- en bewaarinstellingen te openen.

    - U kunt slechts één differentiële back-up per dag activeren.
    - Differentiële back-ups kunnen maximaal 180 dagen worden bewaard. Gebruik de volledige back-ups voor langere bewaartermijn.

9. Selecteer **OK** om het beleid op te slaan en terug te gaan naar het hoofdmenu **Back-upbeleid**.

10. Als u een back-upbeleid voor een transactielogboek wilt toevoegen, selecteert u **Logboekback-up**.
11. In **Logboekback-up** selecteert u **Inschakelen** en stelt u de frequentie- en bewaarinstellingen in. Logboekback-ups kunnen optreden zo vaak als elke 15 minuten en maximaal 35 dagen kunnen worden bewaard.
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

Hier kunt u automatische beveiliging automatisch back-up alle bestaande en toekomstige databases naar een zelfstandige SQL Server-exemplaar of naar een Always On-beschikbaarheidsgroep.

- Er is geen limiet voor het aantal databases dat u in één keer automatische implementatie kunt selecteren.
- U kan selectief beveiligen of databases uitsluiten van beveiliging in een exemplaar op het moment dat u automatische beveiliging inschakelen.
- Als uw exemplaar al een beveiligde databases bevat, wordt ze beveiligde onder hun beleid blijven, zelfs nadat u automatische beveiliging inschakelen. Alle niet-beveiligde databases later toegevoegd heeft slechts één beleid die u op het moment definieert van het inschakelen van automatische beveiliging, die worden vermeld onder **back-up configureren**. U kunt echter het beleid dat is gekoppeld aan een database automatisch beschermd later wijzigen.  

Automatische beveiliging inschakelen:

  1. In **Items voor back-up** selecteert u het exemplaar waarvoor u automatische beveiliging wilt inschakelen.
  2. Selecteer de vervolgkeuzelijst onder **AUTOPROTECT**, kiest u **ON**, en selecteer vervolgens **OK**.

      ![Inschakelen van automatische beveiliging van de beschikbaarheidsgroep.](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. Back-up wordt voor alle databases tegelijk getriggerd en kan worden gevolgd in **Back-uptaken**.

Als u automatische beveiliging uitschakelen moet, selecteert u de naam van het exemplaar onder **back-up configureren**, en selecteer vervolgens **uitschakelen Autoprotect** voor het exemplaar. Alle databases op een back-up wordt voortgezet, maar toekomstige databases wordt niet automatisch worden beveiligd.

![Schakel automatische beveiliging op dat exemplaar](./media/backup-azure-sql-database/disable-auto-protection.png)

 
## <a name="next-steps"></a>Volgende stappen

Leer hoe u het volgende doet:

- [SQL Server-databases voor back-up herstellen](restore-sql-database-azure-vm.md)
- [Een back-up SQL Server-databases beheren](manage-monitor-sql-database-backup.md)
