Azure Marketplace maakt een groot aantal populaire web-apps beschikbaar die zijn ontwikkeld door Microsoft, door derden of via OOS-initiatieven (Open Source Software). Voor web-apps uit de Microsoft Azure Marketplace hoeft u geen andere software te installeren dan de browser die wordt gebruikt om verbinding te maken met de [Azure Preview Portal](http://go.microsoft.com/fwlink/?LinkId=529715). 

In deze zelfstudie leert u:

* Hoe u een nieuwe web-app maakt via Azure Marketplace.
* Hoe u de web-app implementeert via de Azure Preview Portal.

U maakt een WordPress-blog die gebruikmaakt van een standaardsjabloon. In de volgende afbeelding wordt de voltooide toepassing weergegeven:

![Wordpress-blog][13]

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 

## <a name="create-a-web-app-in-the-portal"></a>Een web-app maken in de portal
1. Meld u aan bij de Azure Preview Portal.
2. Open Azure Marketplace door op het pictogram van de **Marketplace** te klikken:
   
    ![Pictogram van Marketplace][marketplace]
   
    Of door rechtsboven in het dashboard op het pictogram **Nieuw** te klikken en onder aan de lijst de optie **Marketplace** te selecteren.
   
    ![Create New][5]
3. Selecteer **Web en mobiel**. Zoek naar **WordPress** en klik op het **WordPress**-pictogram.
   
    ![WordPress via een lijst][7]
4. Lees de beschrijving van de WordPress-app en selecteer **Maken**.
5. Klik op **Web-app** en geef de vereiste waarden op om de web-app te configureren.
   
    ![de app configureren][8]
6. Klik op **Database** en geef de vereiste waarden op om de MySQL-database te configureren. 
   
    ![database configureren][database]
7. Geef een naam op voor een nieuwe resourcegroep.
   
    ![Resourcegroep instellen][groupname]
8. Klik indien nodig op **ABONNEMENT** en geef het abonnement op dat u wilt gebruiken. 
9. Wanneer u klaar bent met het definiëren van de web-app, klikt u op **Maken** en wacht u tot de nieuwe web-app is gemaakt.
   
   Wanneer de app is gemaakt, ziet u de resourcegroep die de web-app en database bevat.
   
   ![groep tonen][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>De WordPress-web-app starten en beheren
1. Klik op de nieuwe web-app om details over de app te bekijken.
   
    ![dashboard starten][10]
2. Klik op de pagina **Essentials** op **Bladeren** of op de koppeling onder **URL** om de welkomstpagina van de web-app te openen.
   
    ![site-URL][browse]
3. Als WordPress niet is geïnstalleerd, voert u de juiste configuratiegegevens voor WordPress in en klikt u op **WordPress installeren** om de configuratie te voltooien en de aanmeldingspagina van de web-app te openen.
4. Klik op **Aanmelden** en voer uw referenties in.  
5. U hebt een nieuwe WordPress-web-app die er ongeveer zo uitziet als de onderstaande web-app.    
   
    ![uw WordPress-site][13]

[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png


<!--HONumber=Jan17_HO3-->


