Nadat de records voor uw domeinnaam hebt doorgegeven, moet u mogelijk zijn voor gebruik met uw browser om te controleren dat de naam van uw aangepaste domein kan worden gebruikt voor toegang tot uw web-app in Azure App Service.

> [!NOTE]
> Het kan even duren voor uw CNAME worden doorgegeven via de DNS-systeem. U kunt een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> om te controleren of de CNAME beschikbaar.
> 
> 

Als u uw web-app nog niet als een Traffic Manager-eindpunt hebt toegevoegd, moet u dit doen voordat naamomzetting als het aangepaste domein naam routes aan Traffic Manager werkt. Traffic Manager stuurt vervolgens naar uw web-app. Gebruik de informatie in [toevoegen of verwijderen eindpunten](../articles/traffic-manager/traffic-manager-endpoints.md) uw web-app toevoegen als een eindpunt in uw Traffic Manager-profiel.

> [!NOTE]
> Als uw web-app niet wordt weergegeven wanneer u een eindpunt toevoegt, controleert u of deze is geconfigureerd voor **standaard** modus van App Service-plan. U moet gebruiken **standaard** modus voor uw web-app om te kunnen werken met Traffic Manager.
> 
> 

1. Open in uw browser de [Azure Portal](https://portal.azure.com).
2. In de **Web-Apps** en klik op de naam van uw web-app, selecteer **instellingen**, en selecteer vervolgens **aangepaste domeinen**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. In de **aangepaste domeinen** blade, klikt u op **hostnaam toevoegen**.
4. Gebruik de **hostnaam** tekstvakken in te voeren de Traffic Manager-domeinnaam wilt koppelen aan deze web-app.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Klik op **valideren** om op te slaan van de configuratie van de naam.
6. Wanneer u op klikt **valideren** Azure wordt ere van domeinverificatie werkstroom. Hiermee wordt gecontroleerd op domein eigendom, evenals de beschikbaarheid en rapport geslaagd hostnaam of gedetailleerde fout met prescriptieve guidence voor het oplossen van de fout.    
7. Na geslaagde validatie **hostnaam toevoegen** knop wordt actief en is het mogelijk naar de hostnaam van de toewijzing. Nu gaat u naar uw aangepaste domeinnaam in een browser. U ziet nu uw app wordt uitgevoerd met behulp van uw aangepaste domeinnaam. 
   
   Zodra de configuratie is voltooid, kunt u de aangepaste domeinnaam wordt weergegeven in de **domeinnamen** gedeelte van uw web-app.

Op dit moment moet u kunnen de Traffic Manager-domeinnaam invoeren in uw browser en Zie dat het met succes u naar uw web-app gaat.

