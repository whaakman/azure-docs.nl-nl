### <a name="prerequisites"></a>Vereisten
* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [Azure Blob Storage-account](../articles/storage/common/storage-create-storage-account.md) met inbegrip van de opslagaccountnaam en de toegangssleutel. Deze informatie wordt vermeld in de eigenschappen van het opslagaccount in de Azure-portal. Lees meer over [Azure Storage](../articles/storage/common/storage-introduction.md).

Voordat u uw Azure Blob Storage-account in een logische app, verbinding maken met uw Azure Blob Storage-account. U kunt dit eenvoudig doen in uw logische app in de Azure portal.  

Verbinding maken met uw Azure Blob Storage-account met behulp van de volgende stappen uit:  

1. Een logische app maken. In de ontwerpfunctie voor Logic Apps een trigger toevoegen en voeg vervolgens een actie. Selecteer **beheerde API's van Microsoft weergeven** in de vervolgkeuzelijst en voer vervolgens 'blob' in het zoekvak. Selecteer een van de acties:  
   
    ![Stap met Azure Blob Storage verbinding maken](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Als u verbindingen met Azure storage eerder hebt gemaakt, wordt u gevraagd de verbindingsdetails op te geven:   
   
    ![Stap met Azure Blob Storage verbinding maken](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Voer de accountdetails van de opslag. Eigenschappen met een sterretje zijn vereist.
   
   | Eigenschap | Details |
   | --- | --- |
   | Verbindingsnaam * |Voer een naam voor de verbinding. |
   | Naam van het Azure-Opslagaccount * |Voer de naam van het opslagaccount. Naam van het opslagaccount wordt weergegeven in de eigenschappen van de opslag in de Azure portal. |
   | Azure Storage-Accounttoegangssleutel * |Voer de sleutel van het opslagaccount. De sneltoetsen worden weergegeven in de eigenschappen van de opslag in de Azure portal. |
   
    Deze referenties worden gebruikt voor het autoriseren van uw logische app verbinding maken en toegang tot uw gegevens. 
4. Selecteer **Maken**.
5. U ziet dat de verbinding is gemaakt. Ga nu verder met de overige stappen in uw logische app: 
   
    ![Stap met Azure Blob Storage verbinding maken](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

