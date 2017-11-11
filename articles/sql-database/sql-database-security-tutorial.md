---
title: Beveiligen van uw Azure SQL database | Microsoft Docs
description: Meer informatie over de technieken en functies voor het beveiligen van uw Azure SQL database.
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 90c03f1538197e1cd1c90165417a4ec74c9c5961
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="secure-your-azure-sql-database"></a>Beveiligen van uw Azure SQL Database

Uw gegevens worden beveiligd met SQL Database door de toegang tot uw database te beperken met behulp van firewallregels, verificatiemechanismen die vereisen dat gebruikers hun identiteit bewijzen, en autorisatie voor gegevens via lidmaatschappen en machtigingen op basis van rollen. Ook wordt gebruikgemaakt van beveiliging op rijniveau en dynamische gegevensmaskering.

U kunt de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang met een paar eenvoudige stappen kunt verbeteren. In deze zelfstudie leert u naar: 

> [!div class="checklist"]
> * Niveau van de server firewallregels voor uw server in de Azure-portal instellen
> * Regels voor uw database met behulp van SSMS firewallregel op databaseniveau instellen
> * Verbinding maken met uw database met behulp van een beveiligde verbindingsreeks
> * Gebruikerstoegang beheren
> * Uw gegevens beschermen met versleuteling
> * Inschakelen van controle voor SQL-Database
> * Detectie van dreigingen SQL-Database inschakelen

Als u een Azure-abonnement geen [een gratis account maken](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie, zorg ervoor dat u hebt het volgende:

- De nieuwste versie van geïnstalleerd [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Geïnstalleerde Microsoft Excel
- Zie gemaakt van een Azure SQL-server en database - [maken van een Azure SQL database in de Azure portal](sql-database-get-started-portal.md), [maken van één Azure SQL database met de Azure CLI](sql-database-get-started-cli.md), en [maken van een enkele Azure SQL de database met behulp van PowerShell](sql-database-get-started-powershell.md). 

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

SQL-databases worden beveiligd door een firewall in Azure. Standaard worden alle verbindingen met de server en de databases binnen de server geweigerd, met uitzondering van verbindingen met andere Azure-services. Zie [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie.

De veiligste configuratie is 'Toegang tot Azure-services toestaan' op OFF instellen. Als u verbinding maken met de database vanaf een virtuele machine in Azure of cloud-service wilt, moet u een [gereserveerde IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) en alleen de gereserveerde IP-adres toegang toestaan via de firewall. 

Volg deze stappen voor het maken van een [SQL-Database firewallregel op serverniveau](sql-database-firewall-configure.md) voor uw server verbindingen van een specifiek IP-adres wilt toestaan. 

> [!NOTE]
> Als u een voorbeelddatabase in Azure met behulp van een van de vorige zelfstudies of snelstartgidsen hebt gemaakt en deze zelfstudie op een computer met hetzelfde IP-adres hebben uitvoert wanneer u deze zelfstudie hebt doorlopen, kunt u deze stap overslaan als u al maken hebt hand een firewallregel op serverniveau.
>

1. Klik op **SQL-databases** vanaf de linkerkant en klik op de database die u wilt configureren, de firewall-regel voor op de **SQL-databases** pagina. De overzichtspagina voor uw database wordt geopend, waarin u de volledig gekwalificeerde servernaam (zoals **mynewserver 20170313.database.windows.net**) en biedt opties voor verdere configuratie.

      ![serverfirewallregel](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Klik op de werkbalk op **Serverfirewall instellen** zoals in de vorige afbeelding is weergegeven. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend. 

3. Klik op **client-IP toevoegen** op de werkbalk om het openbare IP-adres van de computer die is verbonden met de portal met toevoegen of de firewallregel handmatig invoeren en klik vervolgens op **opslaan**.

      ![serverfirewallregel instellen](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Klik op **OK** en vervolgens op **X** om de pagina **Firewallinstellingen** te sluiten.

U kunt nu verbinding met een database in de server met de opgegeven IP-adres of IP-adresbereik.

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u alleen verbinding maken met uw Azure SQL Database-server als uw IT-afdeling poort 1433 openstelt.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Een firewallregel op databaseniveau met behulp van SSMS maken

Firewallregel op databaseniveau regels kunt u verschillende firewall-instellingen voor verschillende databases binnen dezelfde logische server maken en te maken van de firewall-regels die draagbare zijn-wat betekent dat ze volgt u de database tijdens een [failover](sql-database-geo-replication-overview.md) in plaats van op de SQL-server worden opgeslagen. Firewallregels databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL-instructies en alleen nadat u de eerste niveau van de server-firewallregel hebt geconfigureerd. Zie [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie.

Volg deze stappen voor het maken van een database-specifieke firewallregel.

1. Verbinding maken met uw database, bijvoorbeeld met behulp van [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. In Object Explorer met de rechtermuisknop op de database die u wilt een firewallregel voor toevoegen en klik op **nieuwe Query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.

3. Het IP-adres van uw openbare IP-adres wijzigen en vervolgens de volgende query wordt uitgevoerd in het queryvenster:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Klik op de werkbalk op **Execute** om de firewallregel te maken.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Verbinding maken tussen een toepassing met de database met behulp van een beveiligde verbindingsreeks weergeven

Een beveiligde, gecodeerde verbinding tussen een client-toepassing en de SQL-Database, zodat heeft de verbindingsreeks om te worden geconfigureerd:

- Aanvragen van een versleutelde verbinding, en
- Het servercertificaat niet vertrouwen. 

Dit een verbinding maken met behulp van Transport Layer Security (TLS) en vermindert het risico van man-in-the-middle-aanvallen. U kunt verkrijgen juist geconfigureerde verbindingsreeksen voor uw SQL-Database voor ondersteunde client stuurprogramma's vanuit de Azure-portal zoals weergegeven voor ADO.net in deze schermafbeelding.

1. Selecteer **SQL-databases** in het menu links, en klikt u op uw database in de **SQL-databases** pagina.

2. Op de **overzicht** pagina voor uw database, klikt u op **databaseverbindingsreeksen tonen**.

3. Bekijk de volledige **ADO.NET**-verbindingsreeks.

    ![ADO.NET-verbindingsreeks](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Databasegebruikers te maken

Voordat u alle gebruikers, moet u eerst kiezen uit twee soorten verificatie wordt ondersteund door Azure SQL Database: 

**SQL-verificatie**, die gebruikmaakt van gebruikersnaam en wachtwoord voor aanmeldingen en gebruikers die alleen geldig in de context van een bepaalde database binnen een logische server zijn. 

**Azure Active Directory-verificatie**, waarbij identiteiten die worden beheerd door Azure Active Directory wordt gebruikt. 

Als u wilt gebruiken [Azure Active Directory](./sql-database-aad-authentication.md) om te verifiëren op SQL-Database, een ingevuld Azure Active Directory moet bestaan voordat u verder kunt gaan.

Volg deze stappen voor het maken van een gebruiker via SQL-verificatie:

1. Verbinding maken met uw database, bijvoorbeeld met behulp van [SQL Server Management Studio](./sql-database-connect-query-ssms.md) met uw beheerdersreferenties voor de server.

2. In Object Explorer met de rechtermuisknop op de database die u wilt een nieuwe gebruiker toevoegen aan en klik op **nieuwe Query**. Een lege queryvenster wordt geopend die is verbonden met de geselecteerde database.

3. Voer de volgende query in het queryvenster in:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Klik op de werkbalk op **Execute** voor het maken van de gebruiker.

5. Standaard wordt in de gebruiker verbinding kan maken met de database, maar heeft geen machtigingen om te lezen of schrijven van gegevens. Als u wilt deze machtigingen verlenen voor de nieuwe gebruiker, de volgende twee opdrachten in een nieuwe queryvenster uitvoeren

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Het is best practice voor het maken van deze accounts niet-beheerders op databaseniveau verbinding maken met uw database, tenzij u beheerderstaken moet zoals het maken van nieuwe gebruikers uitvoeren. Controleer de [Azure Active Directory-zelfstudie](./sql-database-aad-authentication-configure.md) over het verifiëren met behulp van Azure Active Directory.


## <a name="protect-your-data-with-encryption"></a>Uw gegevens beschermen met versleuteling

Azure SQL Database transparante gegevensversleuteling (TDE) versleutelt uw gegevens in rust, automatisch zonder wijzigingen aan de toepassing toegang tot de versleutelde database. Voor nieuwe databases is TDE standaard ingeschakeld. Ga als volgt TDE inschakelen voor uw database of om te controleren of TDE op:

1. Selecteer **SQL-databases** in het menu links, en klikt u op uw database in de **SQL-databases** pagina. 

2. Klik op **transparante gegevensversleuteling** de configuratiepagina voor TDE openen.

    ![Transparante gegevensversleuteling](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Indien nodig stelt **gegevensversleuteling** op ON en klik op **opslaan**.

Het versleutelingsproces wordt gestart op de achtergrond. U kunt de voortgang controleren door verbinding te maken voor het gebruik van SQL-Database [SQL Server Management Studio](./sql-database-connect-query-ssms.md) door het opvragen van de kolom encryption_state van de `sys.dm_database_encryption_keys` weergeven.

## <a name="enable-sql-database-auditing-if-necessary"></a>Controle in te schakelen SQL-Database, indien nodig

Azure SQL Database Auditing houdt databasegebeurtenissen en schrijft die deze naar een auditlogboek Meld u bij uw Azure Storage-account. Controle, kunt u de naleving van regelgeving onderhouden, de activiteit in een database begrijpen en meer inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op mogelijke schendingen van de beveiliging. Volg deze stappen om een standaard controlebeleid voor de SQL-database te maken:

1. Selecteer **SQL-databases** in het menu links, en klikt u op uw database in de **SQL-databases** pagina. 

2. Selecteer in de blade instellingen **controle en detectie van dreigingen**. Kennisgeving serverniveaus controle is uitgeschakeld en of er een **serverinstellingen weergeven** koppeling waarmee u weergeven of wijzigen van de server controle-instellingen in deze context.

    ![Controle-Blade](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Als u liever een Audit type (of locatie?) inschakelen verschilt van de opgegeven op serverniveau, schakelt u **ON** controle, en kies de **Blob** controletype. Als server Auditingfunctie voor blobs is ingeschakeld, wordt de database geconfigureerd audit bestaan naast de Blob-controle-server.

    ![Controle inschakelen](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Selecteer **opslaggroep** om de Blade Audit logboeken opslag te openen. Selecteer de Azure storage-account waarbij logboeken worden opgeslagen en de bewaarperiode, waarna de oude logboeken worden verwijderd, klikt u vervolgens op **OK** onderaan. 

   > [!TIP]
   > Gebruik hetzelfde opslagaccount voor alle gecontroleerde databases optimaal buiten de controle rapporten sjablonen.
   > 

5. Klik op **Opslaan**.

> [!IMPORTANT]
> Als u aanpassen van de gecontroleerde gebeurtenissen wilt, kunt u dit doen via PowerShell of REST-API - Zie [SQL database auditing](sql-database-auditing.md) voor meer informatie.
>

## <a name="enable-sql-database-threat-detection"></a>Detectie van dreigingen SQL-Database inschakelen

Detectie van dreigingen biedt een nieuwe laag van beveiliging, waarmee klanten om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen doordat beveiligingswaarschuwingen op vreemde activiteiten worden gedetecteerd. Gebruikers kunnen de verdachte gebeurtenissen om te bepalen of ze het gevolg zijn van een poging om te openen, inbreuk of misbruik van gegevens in de database met behulp van SQL Database Auditing verkennen. Detectie van dreigingen kunt u eenvoudig op mogelijke bedreigingen adres met de database hoeft te worden van een deskundige beveiliging of systemen bewaking van de geavanceerde beveiliging te beheren.
Detectie van dreigingen detecteert bijvoorbeeld bepaalde afwijkende databaseactiviteiten potentiële SQL-injectie pogingen die aangeeft. SQL-injectie is een van de algemene Web application beveiligingsproblemen op het Internet worden gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de invoervelden toepassing voor schendingen veroorzaken of wijzigen van gegevens in de database.

1. Navigeer naar de blade van de configuratie van de SQL-database die u wilt bewaken. Selecteer in de blade instellingen **controle en detectie van dreigingen**.

    ![Navigatiedeelvenster](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. In de **controle en detectie van dreigingen** configuratie blade Schakel **ON** controle, wordt die de threat detectie-instellingen weergegeven.

3. Schakel **ON** bedreigingendetectie.

4. Configureer de lijst met e-mailberichten die beveiligingswaarschuwingen na detectie van afwijkende databaseactiviteiten ontvangt.

5. Klik op **opslaan** in de **controle en detectie van bedreigingen** blade op te slaan de nieuwe of bijgewerkte controle dreiging van beleid.

    ![Navigatiedeelvenster](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Als er worden afwijkende databaseactiviteiten gedetecteerd, ontvangt u een e-mailmelding na detectie van afwijkende databaseactiviteiten. Het e-mailadres bevatten informatie over de verdachte-gebeurtenis met inbegrip van de aard van de afwijkende activiteiten, databasenaam, de servernaam van de en de tijd van de gebeurtenis. Bovendien bevatten informatie over mogelijke oorzaken en aanbevolen acties te onderzoeken en de mogelijke bedreiging voor de database te verminderen. De volgende stappen maakt u via wat te doen moet u deze een e-mail ontvangen:

    ![Threat detectie e](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. Klik in de e-mail op de **Azure SQL-controle logboek** koppeling, die wordt de Azure-portal te starten en de relevante controle records rond de tijd van de verdachte gebeurtenis wilt weergeven.

    ![AuditRecords](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Klik op de controlerecords naar meer details weergeven over de op verdachte databaseactiviteiten zoals SQL-instructie is mislukt reden en client-IP.

    ![Details van de records](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. Klik op de blade controle Records **openen in Excel** openen van een vooraf geconfigureerde excel sjabloon importeren en uitvoeren van de diepgaande analyse van het controlelogboek rond de tijd van de verdachte activiteit.

    > [!NOTE]
    > In Excel 2010 of hoger, Power Query en de **snel combineren** instelling is vereist.

    ![Open records in Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. Voor het configureren van de **snel combineren** instellen - In de **POWER QUERY** linttabblad, selecteer **opties** om het dialoogvenster opties weer te geven. Selecteer de sectie Privacy en kiest u de tweede optie - 'Negeren van de privacyniveaus en mogelijk verbeterde prestaties':

    ![Excel snel combineren](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. Als u wilt laden controlelogboeken SQL, zorg ervoor dat de parameters in de instellingen juist zijn ingesteld en selecteer vervolgens het lint 'Data' en klik op de knop Alles vernieuwen tabblad.

    ![Excel-parameters](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. De resultaten worden weergegeven de **SQL controlelogboeken** blad waarmee u meer gedetailleerde analyse van de afwijkende activiteiten die zijn gedetecteerd uitvoeren, en het effect van de beveiligingsgebeurtenis in uw toepassing.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd verbeteren de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang met slechts enkele eenvoudige stappen.  U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Firewallregels voor uw server en/of database instellen
> * Verbinding maken met uw database met behulp van een beveiligde verbindingsreeks
> * Gebruikerstoegang beheren
> * Uw gegevens beschermen met versleuteling
> * Inschakelen van controle voor SQL-Database
> * Detectie van dreigingen SQL-Database inschakelen

Ga naar de volgende zelfstudie voor informatie over het implementeren van een database verschillende geografische locaties.

> [!div class="nextstepaction"]
>[Een geografisch gedistribueerde database implementeren](sql-database-implement-geo-distributed-database.md)

