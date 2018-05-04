---
title: Uw Azure SQL-database beveiligen | Microsoft Docs
description: Lees hier alles over technieken en functies voor het beveiligen van uw Azure SQL-database.
services: sql-database
author: DRediske
manager: craigg
ms.service: sql-database
ms.custom: mvc,security
ms.topic: tutorial
ms.date: 04/01/2018
ms.author: daredis
ms.openlocfilehash: 68a2a61dd5821470d30e3735ea6a2df89360cbb2
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="secure-your-azure-sql-database"></a>Uw Azure SQL-database beveiligen

SQL Database beveiligt uw gegevens door: 
- De toegang tot uw database te beperken met behulp van firewallregels 
- Verificatiemechanismen te gebruiken die hun identiteit vereisen
- Autorisatie voor gegevens via op rollen gebaseerde lidmaatschappen en machtigingen, 
- Beveiliging op rijniveau
- Dynamische gegevensmaskering

SQL Database heeft ook geavanceerde bewaking, controle en bedreigingsdetectie. 

U hoeft slechts een paar eenvoudige stappen uit te voeren om de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang te verbeteren. In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Firewallregels op serverniveau instellen voor uw server in Azure Portal
> * Firewallregels op databaseniveau instellen voor uw database met SSMS
> * Verbinding maken met uw database met behulp van een beveiligde verbindingsreeks
> * Gebruikerstoegang beheert
> * Uw gegevens beschermt met versleuteling
> * Controle inschakelt voor SQL Database
> * Bedreigingsdetectie inschakelt voor SQL Database

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze zelfstudie te voltooien:

- De nieuwste versie van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) is geïnstalleerd. 
- Microsoft Excel is geïnstalleerd
- Een Azure SQL-server en -database - Zie [Een Azure SQL-database maken in Azure Portal](sql-database-get-started-portal.md), [Een individuele Azure SQL-database maken met de Azure CLI](sql-database-get-started-cli.md) en [Een individuele Azure SQL Database maken met PowerShell](sql-database-get-started-powershell.md). 

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

SQL-databases worden in Azure beveiligd door een firewall. De standaardinstelling is dat alle verbindingen met de server en de databases op de server worden geweigerd, behalve verbindingen van andere Azure-services. Zie [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie.

De veiligste configuratie is om 'Toegang tot Azure services toestaan' in te stellen op UIT. Als u verbinding moet maken met de database vanaf een virtuele machine in Azure of vanuit een cloudservice, moet u een [gereserveerd IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) maken en alleen dat gereserveerde IP-adres toegang geven via de firewall. 

Volg deze stappen voor het maken van een [firewallregel op serverniveau voor uw SQL-database](sql-database-firewall-configure.md) voor uw server om verbindingen vanaf een specifiek IP-adres toe te staan. 

> [!NOTE]
> Als u in een van de eerdere zelfstudies of snelstarts een voorbeelddatabase hebt gemaakt in Azure en u deze zelfstudie volgt op een computer met het IP-adres dat ook aan de computer was toegewezen op het moment van de eerdere zelfstudie, kunt u deze stap overslaan omdat u al een firewallregel op serverniveau hebt gemaakt.
>

1. Klik in het linkermenu op **SQL-databases** en klik vervolgens op de database waarvoor u een firewallregel op serverniveau wilt configureren op de pagina **SQL-databases**. De overzichtspagina voor de database wordt geopend, met de volledig gekwalificeerde servernaam (bijvoorbeeld **mynewserver-20170313.database.windows.net**) en opties voor verdere configuratie.

      ![serverfirewallregel](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Klik op de werkbalk op **Serverfirewall instellen** zoals in de vorige afbeelding is weergegeven. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend. 

3. Klik op **Client-IP-adres toevoegen** op de werkbalk om het openbare IP-adres toe te voegen van de computer die is verbonden met de portal of voer de firewallregel handmatig in en klik vervolgens op **Opslaan**.

      ![serverfirewallregel instellen](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Klik op **OK** en vervolgens op **X** om de pagina **Firewallinstellingen** te sluiten.

U kunt nu verbinding maken met elke database op de server met het opgegeven IP-adres of IP-adresbereik.

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u alleen verbinding maken met uw Azure SQL Database-server als uw IT-afdeling poort 1433 openstelt.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Een firewallregel op databaseniveau maken met behulp van SSMS

Gebruik firewallregel op databaseniveau om verschillende firewallinstellingen voor verschillende databases binnen dezelfde logische server te maken en om firewallregels te maken die overdraagbaar zijn, wat betekent dat ze de database volgen tijdens een [failover](sql-database-geo-replication-overview.md) en niet op de SQL-server worden opgeslagen. Firewallregels op databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL-instructies en alleen nadat u de eerste firewallregel op serverniveau hebt geconfigureerd. Zie [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie.

Volg deze stappen voor het maken van een databasespecifieke firewallregel.

1. Maak verbinding met uw database, bijvoorbeeld met behulp van [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. Klik in Object Explorer met de rechtermuisknop op de database waarvoor u een firewallregel wilt toevoegen en klik op **New Query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.

3. Wijzig het IP-adres in het queryvenster in uw openbare IP-adres en voer vervolgens de volgende query uit:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Klik op de werkbalk op **Execute** om de firewallregel te maken.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Een toepassing verbinden met uw database met behulp van een beveiligde verbindingsreeks

Om een beveiligde, gecodeerde verbinding tot stand te brengen tussen een clienttoepassing en SQL Database, moet de verbindingsreeks als volgt worden geconfigureerd:

- Vraag een versleutelde verbinding aan, en
- Vertrouw het servercertificaat niet. 

Er wordt dan verbinding gemaakt via TLS (Transport Layer Security) waardoor het risico op man-in-the-middle-aanvallen afneemt. U kunt correct geconfigureerde verbindingsreeksen voor uw SQL-database voor ondersteunde clientstuurprogramma's opvragen in Azure Portal, zoals hier weergegeven voor ADO.NET.

1. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**.

2. Klik op de pagina **Overzicht** voor uw database op **Verbindingsreeksen database weergeven**.

3. Bekijk de volledige **ADO.NET**-verbindingsreeks.

    ![ADO.NET-verbindingsreeks](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Databasegebruikers maken

Voordat u gebruikers gaat maken, moet u eerst kiezen uit de twee verificatietypen die worden ondersteund door Azure SQL Database: 

**SQL-verificatie**, waarbij gebruikersnamen en wachtwoorden worden gebruikt voor aanmeldingen en gebruikers die alleen geldig zijn in de context van een bepaalde database binnen een logische server. 

**Azure Active Directory-verificatie**, waarbij identiteiten worden gebruikt die worden beheerd in Azure Active Directory. 

Als u [Azure Active Directory](./sql-database-aad-authentication.md) wilt gebruiken voor verificatie bij SQL Database, moet u toegang hebben tot een voorbereide Azure Active Directory voordat u verder kunt gaan.

Volg deze stappen voor het maken van een gebruiker met SQL-verificatie:

1. Maak verbinding maken met uw database, bijvoorbeeld met behulp van [SQL Server Management Studio](./sql-database-connect-query-ssms.md) en de referenties van een serverbeheerder.

2. Klik in Object Explorer met de rechtermuisknop op de database waaraan u een nieuwe gebruiker wilt toevoegen en klik op **New Query**. Er wordt een leeg queryvenster geopend dat is verbonden met de geselecteerde database.

3. Voer de volgende query in het queryvenster in:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Klik op de werkbalk op **Execute** om de gebruiker te maken.

5. De standaardinstelling is dat de gebruiker verbinding kan maken met de database, maar geen machtigingen heeft om gegevens te lezen of te schrijven. Als u deze machtigingen wilt toekennen aan de nieuwe gebruiker, voert u de volgende twee opdrachten uit in een nieuw queryvenster:

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Het wordt aanbevolen om deze niet-beheerdersaccounts te maken op databaseniveau om verbinding te maken met uw database, tenzij u beheerderstaken moet uitvoeren zoals het maken van nieuwe gebruikers. In de [zelfstudie over Azure Active Directory](./sql-database-aad-authentication-configure.md) vindt u informatie over verificatie met behulp van Azure Active Directory.


## <a name="protect-your-data-with-encryption"></a>Uw gegevens beschermen met versleuteling

Met Transparent Data Encryption (TDE) van Azure SQL Database worden uw inactieve gegevens (data-at-rest) automatisch versleuteld, zonder dat hiervoor aanpassingen nodig zijn van de toepassing die toegang wil hebben tot de versleutelde database. Voor nieuwe databases is TDE standaard ingeschakeld. Ga als volgt te werk om TDE in te schakelen voor uw database of om te controleren of TDE is ingeschakeld:

1. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 

2. Klik op **Transparante gegevensversleuteling** om de configuratiepagina voor TDE te openen.

    ![Transparante gegevensversleuteling](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Zet **Gegevensversleuteling** indien nodig op AAN en klik op **Opslaan**.

Het versleutelingsproces wordt op de achtergrond gestart. U kunt de voortgang volgen door via [SQL Server Management Studio](./sql-database-connect-query-ssms.md) verbinding te maken met SQL Database en de kolom encryption_state van de weergave [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql?view=sql-server-2017) op te vragen. De status 3 geeft aan dat de database is versleuteld. 

## <a name="enable-sql-database-auditing-if-necessary"></a>Controle inschakelen voor SQL Database, indien nodig

Met Azure SQL Database Auditing worden databasegebeurtenissen bijgehouden en naar een auditlogboek in uw Azure Storage-account geschreven. Dankzij controles kunt u zorgen voor naleving van wet- en regelgeving, krijgt u inzicht in de activiteit in uw database en in de afwijkingen en discrepanties die kunnen wijzen op mogelijke schendingen van de beveiliging. Volg deze stappen om een standaardcontrolebeleid te maken voor de SQL-database:

1. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 

2. Selecteer **Controle en detectie van bedreigingen** op de blade Instellingen. U ziet dat controle op serverniveau is uitgeschakeld. Selecteer de koppeling **Serverinstellingen weergeven** om de controle-instellingen voor de server vanaf hier te bekijken of te wijzigen.

    ![De blade Controle](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Als u liever een ander type controle (of locatie) inschakelt dan het type dat is opgegeven op serverniveau, zet u Controle op **AAN** en kiest u **Blob** bij Controletype. Als u de controle van serverblobs inschakelt, wordt die controle aanvullend uitgevoerd op de controle die voor de database is geconfigureerd.

    ![Controle inschakelen](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Selecteer **Opslaggegevens** om de blade Auditlogboeken voor opslag te openen. Selecteer het Azure-opslagaccount waarin logboeken moeten worden opgeslagen, evenals de bewaarperiode waarna de oude logboeken worden verwijderd, en klik vervolgens onderaan op **OK**. 

   > [!TIP]
   > Gebruik hetzelfde opslagaccount voor alle gecontroleerde databases om optimaal voordeel te hebben van de sjablonen voor controlerapporten.
   > 

5. Klik op **Opslaan**.

> [!IMPORTANT]
> Als u de gecontroleerde gebeurtenissen wilt aanpassen, kan dat via PowerShell of REST-API. Zie [Aan de slag met SQL Database Auditing](sql-database-auditing.md) voor meer informatie.
>

## <a name="enable-sql-database-threat-detection"></a>Bedreigingsdetectie inschakelen voor SQL Database

Bedreigingsdetectie biedt een nieuwe beveiligingslaag, waarmee klanten potentiële bedreigingen kunnen detecteren en erop reageren zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers kunnen de verdachte gebeurtenissen onderzoeken met behulp van SQL Database Auditing om te bepalen of die het gevolg zijn van pogingen om toegang te krijgen tot, of misbruik te maken van, gegevens in de database. Met bedreigingsdetectie is het eenvoudig om potentiële bedreigingen voor de database weg te nemen zonder de hulp van een beveiligingsexpert of het moeten beheren van geavanceerde bewakingssystemen.
Zo kunt u bijvoorbeeld bepaalde afwijkende databaseactiviteiten detecteren die wijzen op mogelijke pogingen van SQL-injectie. SQL-injectie is een van de veelvoorkomende beveiligingsproblemen voor webtoepassingen op internet, en wordt gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers richten zich op de zwakke plekken van de toepassing om schadelijke SQL-instructies te injecteren in invoervelden van de toepassing om zo gegevens in de database te raadplegen of te wijzigen.

1. Navigeer naar de configuratieblade van de SQL-database die u wilt bewaken. Selecteer **Controle en detectie van bedreigingen** op de blade Instellingen.

    ![Navigatievenster](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. Zet op de configuratieblade **Controle en detectie van bedreigingen** Controle op **AAN** om de instellingen voor bedreigingsdetectie weer te geven.

3. Zet Detectie van dreigingen op **AAN**.

4. Configureer de lijst met e-mailadressen waarnaar beveiligingswaarschuwingen worden gestuurd als er afwijkende databaseactiviteiten zijn vastgesteld.

5. Klik op **Opslaan** op de blade **Controle en detectie van bedreigingen** om het nieuwe of bijgewerkte beleid voor controle en detectie van bedreigingen op te slaan.

    ![Navigatievenster](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Als er afwijkende databaseactiviteiten worden gedetecteerd, ontvangt u hiervan per e-mail een melding. De e-mail bevat informatie over de verdachte beveiligingsgebeurtenis, inclusief de aard van de afwijkende activiteiten, de databasenaam, de servernaam en het tijdstip van de gebeurtenis. Daarnaast bevat de e-mail informatie over mogelijke oorzaken en aanbevolen acties om de mogelijke bedreiging van de database te onderzoeken en weg te nemen. In de volgende stappen wordt uitgelegd wat u moet doen als u een dergelijke e-mail ontvangt:

    ![E-mail met waarschuwing voor detectie van bedreiging](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. Klik in de e-mail op de koppeling voor de **Azure SQL-controlelogboeken**. U gaat dan naar Azure Portal, waar u de relevante controlerecords ziet die zijn vastgelegd rond het tijdstip van de verdachte gebeurtenis.

    ![Controlerecords](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Klik op de controlerecords voor meer informatie over de verdachte databaseactiviteiten, zoals SQL-instructie, reden van mislukken en het client-IP-adres.

    ![Details van record](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. Klik op de blade Controlerecords op **Openen in Excel** om een vooraf geconfigureerde Excel sjabloon te openen, zodat u een diepgaande analyse van het controlelogboek rond het tijdstip van de verdachte activiteit kunt importeren en uitvoeren.

    > [!NOTE]
    > In Excel 2010 of hoger zijn Power Query en instelling **Snel combineren** vereist.

    ![Records openen in Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. Als u de instelling **Snel combineren** wilt configureren, gaat u naar het tabblad **POWER QUERY** op het lint en selecteert u **Opties** om het gelijknamige dialoogvenster weer te geven. Selecteer het onderdeel Privacy en kies de tweede optie, De privacyniveaus en mogelijk verbeterde prestaties negeren:

    ![Snel combineren in Excel](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. Als u SQL-controlelogboeken wilt laden, zorgt u ervoor dat de parameters op het tabblad Instellingen juist zijn ingesteld en selecteert u vervolgens de knop Alles vernieuwen op het tabblad Gegevens van het lint.

    ![Excel-parameters](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. De resultaten worden weergegeven in een **afzonderlijk werkblad**, waarmee u een meer gedetailleerde analyse kunt uitvoeren van de afwijkende activiteiten die zijn gedetecteerd om zo de gevolgen van de beveiligingsgebeurtenis in uw toepassing te beperken.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u met slechts een paar eenvoudige stappen de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang kunt verbeteren.  U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Firewallregels instelt voor uw server en/of database
> * Verbinding maakt met uw database met behulp van een beveiligde verbindingsreeks
> * Gebruikerstoegang beheert
> * Uw gegevens beschermt met versleuteling
> * Controle inschakelt voor SQL Database
> * Bedreigingsdetectie inschakelt voor SQL Database

Ga door naar de volgende zelfstudie om te leren hoe u een database met geo-replicatie implementeert.

> [!div class="nextstepaction"]
>[Een geografisch gedistribueerde database implementeren](sql-database-implement-geo-distributed-database.md)

