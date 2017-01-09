
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

1. Ga op de blade van de SQL-server naar Instellingen en klik op **Firewall** om de blade Firewall van de SQL-server te openen.

    ![sql-serverfirewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png)
    
2. Controleer of het weergegeven IP-adres van de client uw IP-adres is in een internetbrowser naar keuze (vraag "Wat is mijn IP-adres?"). Soms komen ze niet overeen. Dit kan verschillende redenen hebben.

    ![uw IP-adres](../articles/sql-database/media/sql-database-get-started/your-ip-address.png)

3. Klik als de IP-adressen overeenkomen, op **IP van client toevoegen** in de taakbalk.

    ![IP van client toevoegen](../articles/sql-database/media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > U kunt de SQL-databasefirewall op de server naar een enkel IP-adres of een reeks adressen openen. Als u de firewall opent, kunnen SQL-beheerders en -gebruikers zich aanmelden bij elke database op de server waarvoor ze geldige aanmeldgegevens hebben.
    >

4. Klik in de taakbalk op **Opslaan** om deze serverfirewallregel op te slaan. Klik vervolgens op **OK**.

    ![IP van client toevoegen](../articles/sql-database/media/sql-database-get-started/save-firewall-rule.png)

> [!Tip]
> Zie [SQL Database-zelfstudie: een server, serverfirewallregel, voorbeelddatabase en databasefirewallregel maken en koppelen met SQL Server](../articles/sql-database/sql-database-get-started.md).    
>


<!--HONumber=Jan17_HO1-->


