### <a name="prerequisites"></a>Vereisten
U moet hebben een [Service Bus](https://azure.microsoft.com/services/service-bus/) account.  

Voordat u uw Azure Service Bus-account in een logische app gebruiken kunt, moet u de logische app verbinding maken met uw service bus-account toestaan. U kunt dit eenvoudig vanuit gelukkig doen in uw logische app in de Azure portal.  

Hier volgen de stappen voor het autoriseren van uw logische app verbinding maken met uw Service Bus-account:  

1. Voor het maken van een verbinding met Service Bus in de ontwerpfunctie voor logic app selecteert **beheerde API's van Microsoft weergeven** in de vervolgkeuzelijst. Voer vervolgens **service bus** in het zoekvak. Selecteer de trigger of de actie die u wilt gebruiken.  
    ![Afbeelding voor de service Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Als u verbindingen met Service Bus voordat u dit nog niet hebt gemaakt, wordt u gevraagd uw Service Bus-referenties op te geven. Deze referenties worden gebruikt voor het autoriseren van uw logische app verbinding maken met en toegang tot gegevens van uw Service Bus-account. De Service Bus-connector moet de verbindingsreeks voor de Service Bus-naamruimte. Vereist ook **beheren** machtigingen. Een goede manier om te weten of de verbindingsreeks voor de naamruimte is of een specifieke entiteit is aan of deze bevat de `EntityPath` parameter. Als dit het geval is, is het niet de juiste verbindingsreeks voor een logische app.  
    ![Service Bus-verbindingsreeks](./media/connectors-create-api-servicebus/connectionstring.png)
3. Nadat u de verbindingsreeks voor de naamruimte hebt ontvangen, kunt u deze kunt gebruiken voor de API-verbinding in Logic Apps.  
    ![Afbeelding voor de service Bus 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. U ziet de verbinding is gemaakt en u bent nu vrij om door te gaan met de overige stappen in uw logische app.  
    ![Afbeelding voor de service Bus 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

