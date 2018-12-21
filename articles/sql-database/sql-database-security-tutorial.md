---
title: Eén enkele database beveiligen in Azure SQL Database | Microsoft Docs
description: Informatie over technieken en functies voor het beveiligen van één enkele database in Azure SQL-database.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 12/13/2018
ms.openlocfilehash: 814d558efee4a72a25d956828e0db237424cab24
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409765"
---
# <a name="tutorial-secure-a-single-database-in-azure-sql-database"></a>Zelfstudie: één enkele database beveiligen in Azure SQL Database

SQL Database beveiligt de gegevens in één Azure SQL-database door:

- De toegang tot uw database te beperken met behulp van firewallregels
- Verificatiemechanismen te gebruiken die hun identiteit vereisen
- Autorisatie voor gegevens via op rollen gebaseerde lidmaatschappen en machtigingen,
- Beveiliging op rijniveau
- Dynamische gegevensmaskering

SQL Database heeft ook geavanceerde bewaking, controle en bedreigingsdetectie.

> [!IMPORTANT]
> Azure SQL Database beveiligt een database in een beheerd exemplaar door gebruik te maken van netwerkbeveiligingsregels en privé-eindpunten. Zie [Azure SQL Database Managed Instance](sql-database-managed-instance-index.yml) en [Connectiviteitsarchitectuur van Azure SQL Database Managed Instance](sql-database-managed-instance-connectivity-architecture.md) voor meer informatie.

U hoeft slechts een paar eenvoudige stappen uit te voeren om de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang te verbeteren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> - Firewallregels op serverniveau instellen voor uw server in de Azure-portal
> - Firewallregels op databaseniveau instellen voor uw database met SSMS
> - Verbinding maakt met uw database met behulp van een beveiligde verbindingsreeks
> - Azure Active Directory-beheerder configureren voor Azure SQL
> - Gebruikerstoegang beheert
> - Uw gegevens beschermt met versleuteling
> - Controle inschakelt voor SQL Database
> - Bedreigingsdetectie inschakelt voor SQL Database

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze zelfstudie te voltooien:

- De nieuwste versie van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) is geïnstalleerd.
- Microsoft Excel is geïnstalleerd
- Een Azure SQL-server en -database - Zie [Een Azure SQL-database maken in de Azure-portal](sql-database-get-started-portal.md), [Een individuele Azure SQL-database maken met de Azure CLI](sql-database-cli-samples.md) en [Een individuele Azure SQL Database maken met PowerShell](sql-database-powershell-samples.md).

> [!NOTE]
> In deze zelfstudie wordt ervan uitgegaan dat u Azure Active Directory al hebt geconfigureerd of dat u van het initiële beheerde domein van Azure Active Directory gebruikmaakt. Zie [Uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md), [Uw domeinnaam toevoegen in Azure AD](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure ondersteunt nu federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Uw Azure AD-adreslijst beheren](../active-directory/fundamentals/active-directory-administer.md), [Azure AD beheren met Windows PowerShell](/powershell/azure/overview?view=azureadps-2.0) en [Poorten en protocollen waarvoor hybride identiteit is vereist](../active-directory/hybrid/reference-connect-ports.md) voor meer informatie over het configureren van Azure Active Directory voor diverse scenario's.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij de Azure-portal

Meld u aan bij de [Azure-portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in de Azure-portal

SQL-databases worden in Azure beveiligd door een firewall. De standaardinstelling is dat alle verbindingen met de server en de databases op de server worden geweigerd, behalve verbindingen van andere Azure-services. Zie [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie.

De veiligste configuratie is om 'Toegang tot Azure services toestaan' in te stellen op UIT. Als u verbinding moet maken met de database vanaf een virtuele machine in Azure of vanuit een cloudservice, dient u een [gereserveerd IP-adres (klassieke implementatie)](../virtual-network/virtual-networks-reserved-public-ip.md) te maken en alleen dat gereserveerde IP-adres toegang te geven via de firewall. Als u het [Resource Manager](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)-implementatiemodel gebruikt, wordt er een speciaal IP-adres aan de resource toegewezen en dient u dit IP-adres toegang te geven via de firewall.

Volg deze stappen voor het maken van een [firewallregel op serverniveau voor uw SQL-database](sql-database-firewall-configure.md) voor uw server om verbindingen vanaf een specifiek IP-adres toe te staan.

> [!NOTE]
> Als u in een van de eerdere zelfstudies of snelstartgidsen een voorbeelddatabase hebt gemaakt in Azure en u deze zelfstudie volgt op een computer met het IP-adres dat ook aan de computer was toegewezen op het moment van de eerdere zelfstudie, kunt u deze stap overslaan omdat u al een firewallregel op serverniveau hebt gemaakt.

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

Gebruik firewallregels op databaseniveau om verschillende firewallinstellingen voor verschillende databases binnen dezelfde logische server te maken en om firewallregels te maken die overdraagbaar zijn, wat betekent dat ze de database volgen tijdens een failover en niet op de SQL-server worden opgeslagen. Firewallregels op databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL-instructies en alleen nadat u de eerste firewallregel op serverniveau hebt geconfigureerd. Zie [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md) voor meer informatie.

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

Er wordt dan verbinding gemaakt via TLS (Transport Layer Security) waardoor het risico op man-in-the-middle-aanvallen afneemt. U kunt correct geconfigureerde verbindingsreeksen voor uw SQL-database voor ondersteunde clientstuurprogramma's opvragen in de Azure-portal, zoals hier weergegeven voor ADO.NET. Voor informatie over TLS en connectiviteit, zie [TLS-overwegingen](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

1. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**.

2. Klik op de pagina **Overzicht** voor uw database op **Verbindingsreeksen database weergeven**.

3. Bekijk de volledige **ADO.NET**-verbindingsreeks.

    ![ADO.NET-verbindingsreeks](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server"></a>Een Azure Active Directory-beheerder voor uw Azure SQL Database-server inrichten

Richt een Azure Active Directory-beheerder voor uw Azure SQL-server in in de Azure-portal.

1. Ga naar de [Azure-portal](https://portal.azure.com/) en selecteer in de rechterbovenhoek uw verbinding om een lijst met mogelijke Active Directories weer te geven. Kies de juiste Active Directory als de standaard-Azure AD. In deze stap wordt de aan het abonnement gekoppelde Active Directory gekoppeld aan de Azure SQL-server, zodat u zeker weet dat hetzelfde abonnement wordt gebruikt voor zowel Azure AD als SQL Server. (De Azure SQL-server kan Azure SQL Database of Azure SQL Data Warehouse hosten.)

    ![ad-kiezen](./media/sql-database-aad-authentication/8choose-ad.png)

2. Ga naar de pagina **SQL Server** en selecteer **Active Directory-beheerder**. Selecteer op de pagina **Active Directory-beheerder** de optie **Beheerder instellen**.  ![Active Directory selecteren](./media/sql-database-aad-authentication/select-active-directory.png)  

3. Zoek op de pagina **Beheerder toevoegen** een gebruiker. Selecteer de gebruiker of groep die beheerder moet zijn en selecteer **Selecteren**. (Op de pagina Active Directory-beheerder ziet u alle leden en groepen van uw Active Directory.) Gebruikers of groepen die grijs zijn gekleurd, kunnen niet worden geselecteerd omdat ze niet worden ondersteund als beheerders voor Azure AD. (Zie de lijst met ondersteunde beheerders in de sectie **Azure AD-functies en -beperkingen** van [Azure Active Directory-verificatie gebruiken voor verificatie met behulp van SQL Database of SQL Data Warehouse](sql-database-aad-authentication.md).) Op rollen gebaseerd toegangsbeheer (RBAC) is alleen van toepassing op de portal en wordt niet doorgegeven aan SQL Server.
    ![beheerder selecteren](./media/sql-database-aad-authentication/select-admin.png)  

4. Selecteer boven aan de pagina **Active Directory-beheerder** de optie **Opslaan**.
    ![beheerder opslaan](./media/sql-database-aad-authentication/save-admin.png)

Het wijzigen van de beheerder kan enkele minuten duren. Vervolgens wordt de nieuwe beheerder weergegeven in het vak **Active Directory-beheerder**.

   > [!NOTE]
   > Als u de Azure AD-beheerder instelt, mag de naam van de beheerder (gebruiker of groep) niet al aanwezig zijn in de virtuele hoofddatabase als een gebruiker van SQL Server-verificatie. Indien wel aanwezig, mislukt het instellen van de Azure AD-beheerder, waarna het maken ervan wordt teruggedraaid en er wordt aangegeven dat deze beheerder (naam) al bestaat. Omdat een dergelijke gebruiker van SQL Server-verificatie geen deel uitmaakt van Azure AD, mislukt het verbinding maken met de server met Azure AD-verificatie.

## <a name="creating-database-users"></a>Databasegebruikers maken

Voordat u gebruikers gaat maken, moet u eerst kiezen uit de twee verificatietypen die worden ondersteund door Azure SQL Database:

**SQL-verificatie**, waarbij gebruikersnamen en wachtwoorden worden gebruikt voor aanmeldingen en gebruikers die alleen geldig zijn in de context van een bepaalde database binnen een logische server.

**Azure Active Directory-verificatie**, waarbij identiteiten worden gebruikt die worden beheerd in Azure Active Directory.

### <a name="create-a-user-using-sql-authentication"></a>Gebruiker maken met behulp van SQL-verificatie

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

### <a name="create-a-user-using-azure-active-directory-authentication"></a>Gebruiker maken met behulp van Azure Active Directory-verificatie

Voor verificatie met Azure Active Directory is vereist dat databasegebruikers als gebruikers van ingesloten databases worden gemaakt. Een gebruiker van een ingesloten database op basis van een Azure AD-identiteit is een databasegebruiker die zich niet kan aanmelden bij de hoofddatabase en die wordt verwezen naar een identiteit in de Azure AD-directory die aan de database is gekoppeld. De Azure AD-identiteit kan een individueel gebruikersaccount of een groep zijn. Zie [Contained Database Users- Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx) (Gebruikers van ingesloten databases: een draagbare database maken) voor meer informatie over gebruikers van ingesloten databases.

> [!NOTE]
> Databasegebruikers kunnen niet in de Azure-portal worden gemaakt (met uitzondering van beheerders). RBAC-rollen worden niet doorgegeven aan SQL Server, SQL Database of SQL Data Warehouse. Azure RBAC-rollen worden gebruikt voor het beheren van Azure-resources en zijn niet van toepassing op databasemachtigingen. Bijvoorbeeld: de rol **Inzender voor SQL Server** verleent geen toegang om verbinding te maken met SQL Database of SQL Data Warehouse. De toestemming tot het verlenen van toegang moet rechtstreeks plaatsvinden in de database met behulp van Transact-SQL-instructies.
> [!WARNING]
> Speciale tekens als de dubbele punt `:` of de ampersand `&` worden niet ondersteund indien deze worden opgenomen als gebruikersnamen in de instructies T-SQL CREATE LOGIN of CREATE USER.

1. Maak verbinding met de Azure SQL-server met behulp van uw Azure Active Directory-account, waarbij u ten minste over de machtiging **ALTER ANY USER** moet beschikken.
2. Klik in Object Explorer met de rechtermuisknop op de database waaraan u een nieuwe gebruiker wilt toevoegen en klik op **New Query**. Er wordt een leeg queryvenster geopend dat is verbonden met de geselecteerde database.

3. Voer in het queryvenster de volgende query in en wijzig `<Azure_AD_principal_name>` in de gewenste user principal name van een Azure AD-gebruiker of de weergavenaam voor een Azure AD-groep:

   ```sql
   CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
   ```

   > [!NOTE]
   > Azure AD-gebruikers worden in de metagegevens van de database gemarkeerd met het type E (EXTERNAL_USER) en voor groepen met het type X (EXTERNAL_GROUPS). Zie [sys.database_principals](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql) voor meer informatie.

## <a name="protect-your-data-with-encryption"></a>Uw gegevens beschermt met versleuteling

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

6. Klik in de e-mail op de koppeling voor de **Azure SQL-controlelogboeken**. U gaat dan naar de Azure-portal, waar u de relevante controlerecords ziet die zijn vastgelegd rond het tijdstip van de verdachte gebeurtenis.

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
> - Firewallregels instelt voor uw server en/of database
> - Verbinding maakt met uw database met behulp van een beveiligde verbindingsreeks
> - Azure Active Directory-beheerder configureren voor Azure SQL
> - Gebruikerstoegang beheert
> - Uw gegevens beschermt met versleuteling
> - Controle inschakelt voor SQL Database
> - Bedreigingsdetectie inschakelt voor SQL Database

Ga door naar de volgende zelfstudie om te leren hoe u een database met geo-replicatie implementeert.

> [!div class="nextstepaction"]
>[Een geografisch gedistribueerde database implementeren](sql-database-implement-geo-distributed-database.md)
