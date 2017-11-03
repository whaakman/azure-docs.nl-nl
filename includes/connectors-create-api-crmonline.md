#### <a name="prerequisites"></a>Vereisten
* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) account 

Voordat u uw Dynamics-account in een logische app, de autorisatie voor de logische app verbinding maken met uw CRM Online-account. U kunt dit eenvoudig doen in uw logische app in de Azure portal. 

Toestaan dat uw logische app verbinding maken met uw CRM Online-account met de volgende stappen uit:

1. Een logische app maken. Selecteer in de ontwerpfunctie Logic Apps **beheerde API's van Microsoft weergeven** in de vervolgkeuzelijst en voer vervolgens 'dynamics' in het zoekvak. Selecteer een van de triggers of acties:  
   ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Als u verbindingen met Dynamics nog niet eerder hebt gemaakt, wordt u gevraagd aan te melden met uw referenties Dynamics:  
   ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Selecteer **aanmelden**, en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**. 
   
    Deze referenties worden gebruikt voor het autoriseren van uw logische app verbinding te maken, toegang tot de gegevens in uw Dynamics-account. 
4. U ziet dat de verbinding is gemaakt. Ga nu verder met de overige stappen in uw logische app:  
   ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

