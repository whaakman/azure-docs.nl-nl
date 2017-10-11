
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) op http://portal.azure.com/.
2. Klik in het linkerdeelvenster vaandel **door alles bladeren**. De **Bladeren** blade wordt weergegeven.
3. Schuif en klik op **SQL-servers**. De **SQL-servers** blade wordt weergegeven.
   
    ![Uw Azure SQL Database-server niet vinden in de portal][b21-FindServerInPortal]
4. Klik op het besturingselement minimaliseren in de eerdere voor het gemak **Bladeren** blade.
5. Typ de naam van uw server in het filtertekstvak. De rij wordt weergegeven.
6. Klik op de rij voor uw server. Een blade voor uw server wordt weergegeven.
7. Klik op de blade van uw server **instellingen**. De **instellingen** blade wordt weergegeven.
8. Klik op **Firewall**. De **firewallinstellingen** blade wordt weergegeven.
   
    ![Klik op Instellingen > Firewall][b31-SettingsFirewallNavig]
9. Klik op **-Client toevoegen IP**. Typ een naam voor de nieuwe regel in het eerste tekstvak.
10. Typ in de lage en hoge IP-adreswaarden voor het bereik dat u wilt inschakelen.
    
    * Het kan zijn bij de hand hebt om het einde van de lage waarde met **.0** en de hoge met **.255**.
    
    ![Toevoegen van een IP-adresbereik om toe te staan][b41-AddRange]
11. Klik op **Opslaan**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
