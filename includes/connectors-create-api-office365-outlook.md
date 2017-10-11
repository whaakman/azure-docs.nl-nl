#### <a name="prerequisites"></a>Vereisten
* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [Office 365](https://office365.com) account  

Voordat u uw Office 365-account in een logische app, de autorisatie voor de logische app verbinding maken met uw Office 365-account. U kunt dit eenvoudig doen in uw logische app in de Azure portal.  

Toestaan dat uw logische app verbinding maken met uw Office 365-account met behulp van de volgende stappen uit:

1. Een logische app maken. Selecteer in de ontwerpfunctie Logic Apps **beheerde API's van Microsoft weergeven** in de vervolgkeuzelijst en voer vervolgens 'office 365' in het zoekvak. Selecteer een van de triggers of acties:  
    ![Stap voor Office 365 verbinding maken](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Als u verbindingen met Office 365 nog niet eerder hebt gemaakt, wordt u gevraagd aan te melden met uw Office 365-referenties:  
    ![Stap voor Office 365 verbinding maken](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Selecteer **aanmelden**, en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**:  
    ![Stap voor Office 365 verbinding maken](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Deze referenties worden gebruikt voor het autoriseren van uw logische app verbinding te maken, toegang tot uw Office 365-account. 
4. U ziet dat de verbinding is gemaakt. Ga nu verder met de overige stappen in uw logische app:   
    ![Stap voor Office 365 verbinding maken](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

