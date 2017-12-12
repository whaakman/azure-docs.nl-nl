
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer in de lijst aan de linkerkant **Bladeren**. 

3. Blader en selecteer **SQL-servers**. 
   
    ![Uw Azure SQL Database-server niet vinden in de portal][b21-FindServerInPortal]
4. Voor het gemak minimaliseren de **Bladeren** blade.

5. Typ de naam van uw server in het filtertekstvak. De rij wordt weergegeven.

6. Selecteer de rij voor uw server. Een blade voor uw server wordt weergegeven.

7. Selecteer op de blade van uw server **instellingen**. 

8. Selecteer **Firewall**. 
   
    ![Selecteer instellingen > Firewall][b31-SettingsFirewallNavig]
9. Selecteer **-Client toevoegen IP**. Typ een naam voor de nieuwe regel in het tekstvak eerste.

10. Typ in de lage en hoge IP-adreswaarden voor het bereik dat u wilt inschakelen.
    
    * Het kan zijn bij de hand hebt om het einde van de lage waarde met **.0** en de hoogste waarde eindigen met **.255**.
    
    ![Toevoegen van een IP-adresbereik om toe te staan][b41-AddRange]
11. Selecteer **Opslaan**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
