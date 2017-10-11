#### <a name="prerequisites"></a>Vereisten
* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) account 

Voordat u uw OneDrive-account in een logische app gebruiken kunt, de autorisatie voor de logische app verbinding maken met uw OneDrive-account.  U kunt dit eenvoudig doen in uw logische app in de Azure portal. 

Toestaan dat uw logische app verbinding maken met uw OneDrive-account met behulp van de volgende stappen uit:

1. Een logische app maken. Selecteer in de ontwerpfunctie Logic Apps **beheerde API's van Microsoft weergeven** in de vervolgkeuzelijst en voer vervolgens 'onedrive' in het zoekvak. Selecteer een van de triggers of acties:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Als u verbindingen met OneDrive nog niet eerder hebt gemaakt, wordt u gevraagd aan te melden met uw OneDrive-referenties:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Selecteer **aanmelden**, en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Deze referenties worden gebruikt voor het autoriseren van uw logische app verbinding te maken, toegang tot de gegevens in uw OneDrive-account. 
4. Selecteer **Ja** voor het autoriseren van de logische app naar uw OneDrive-account gebruiken:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. U ziet dat de verbinding is gemaakt. Ga nu verder met de overige stappen in uw logische app:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

