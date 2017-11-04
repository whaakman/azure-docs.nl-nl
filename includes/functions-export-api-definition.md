## <a name="export-an-api-definition"></a>Exporteren van een API-definitie
U hebt een definitie van een OpenAPI voor de functie van [maken voor een functie een definitie van een OpenAPI](../articles/azure-functions/functions-openapi-definition.md). De volgende stap in dit proces is de API-definitie exporteren zodat PowerApps en Microsoft-Flow kan worden gebruikt in een aangepaste API gebruiken.

> [!IMPORTANT]
> Houd er rekening mee dat u moet zijn aangemeld bij Azure met dezelfde referenties die u voor uw PowerApps en Microsoft-Flow tenants. Hierdoor kunnen Azure voor het maken van de aangepaste API en beschikbaar maken voor zowel PowerApps als Microsoft Flow.

1. In de [Azure-portal](https://portal.azure.com), klikt u op de functie app-naam (zoals **functie demo energie**) > **platformfuncties** > **API-definitie** .

    ![API-definitie](media/functions-export-api-definition/api-definition.png)

1. Klik op **exporteren naar PowerApps + stroom**.

    ![API-definitie-bron](media/functions-export-api-definition/export-api-1.png)

1. Gebruik de instellingen die zijn opgegeven in de tabel in het rechterdeelvenster.

    |Instelling|Beschrijving|
    |--------|------------|
    |**Modus exporteren**|Selecteer **Express** voor het automatisch genereren van de aangepaste API. Selecteren **handmatige** uitvoer van de API-definitie, maar vervolgens u moet het importeren in PowerApps en Microsoft-Flow handmatig. Zie voor meer informatie [exporteren naar PowerApps en Microsoft-Flow](../articles/azure-functions/app-service-export-api-to-powerapps-and-flow.md).|
    |**Omgeving**|Selecteer de omgeving waarin de aangepaste API moet worden opgeslagen. Zie voor meer informatie [omgevingen overzicht (PowerApps)](https://powerapps.microsoft.com/tutorials/environments-overview/) of [omgevingen overzicht (Microsoft Flow)](https://us.flow.microsoft.com/documentation/environments-overview-admin/).|
    |**De naam van de aangepaste API gebruiken**|Voer een naam in, zoals `Turbine Repair`.|
    |**De naam van de API-sleutel**|Voer de naam die de app en stroom opbouwfuncties ziet in de aangepaste API-gebruikersinterface. Houd er rekening mee dat in het voorbeeld nuttige informatie bevat.|
 
    ![Exporteren naar PowerApps en Microsoft Flow](media/functions-export-api-definition/export-api-2.png)

1. Klik op **OK**. De aangepaste API is nu gemaakt en toegevoegd aan de omgeving die u hebt opgegeven.