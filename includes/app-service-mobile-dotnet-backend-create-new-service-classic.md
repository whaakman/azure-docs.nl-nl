1. Meld u aan bij de [Azure-portal].
2. Selecteer **+ nieuw** > **Web en mobiel** > **mobiele App**, en geef vervolgens een naam voor uw mobiele Apps van back-end.
3. Voor **resourcegroep**, selecteer een bestaande resourcegroep of maak een nieuwe (met behulp van dezelfde naam als uw app). 
4. Voor **App Service-abonnement**, het standaardabonnement (in de [standaardcategorie](https://azure.microsoft.com/pricing/details/app-service/)) is geselecteerd. U kunt ook een ander abonnement selecteren of [Maak een nieuwe](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan). 

   Instellingen voor de App Service-abonnement bepalen de [locatie, functies, kosten en rekenresources](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app. Voor meer informatie over App Service-abonnementen en hoe u een nieuw plan maakt in een andere prijscategorie servicetier en op de gewenste locatie, Zie [gedetailleerd overzicht van Azure App Service-plannen](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
5. Selecteer **Maken**. Deze stap maakt de back-end van Mobile Apps. 
6. In de **instellingen** deelvenster voor mobiele Apps van de nieuwe back-end, selecteer **snel starten** > uw clientplatform app > **verbinding maken met een database**. 
   
   ![De selecties voor het verbinden van een database](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
7. In de **gegevensverbinding toevoegen** deelvenster **SQL-Database** > **een nieuwe database maken**. Voer de naam van de database, kies een prijscategorie en selecteer vervolgens **Server**. U kunt deze nieuwe database opnieuw gebruiken. Als u al een database op dezelfde locatie hebt, kunt u in plaats daarvan **Een bestaande database gebruiken** kiezen. Het gebruik van een database in een andere locatie, vanwege bandbreedtekosten en hogere latentie wordt niet aanbevolen.
   
   ![Een database selecteren](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
8. In de **nieuwe server** deelvenster, voer een unieke naam in de **servernaam** Geef een gebruikersnaam en wachtwoord, selecteer **toestaan Azure services voor toegang tot server**, en selecteer  **OK**. Deze stap maakt de nieuwe database.
9. Terug in de **gegevensverbinding toevoegen** deelvenster **verbindingsreeks**, geef de waarden aanmeldingsnaam en het wachtwoord voor uw database en selecteer **OK**. 

   Wacht een paar minuten voor de database is geïmplementeerd voordat u doorgaat.

<!-- URLs. -->
[Azure-portal]: https://portal.azure.com/
