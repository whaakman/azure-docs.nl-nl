## <a name="create-a-storage-account-by-using-the-azure-portal"></a>Een opslagaccount maken met behulp van de Azure-portal

Maak eerst een nieuw opslagaccount voor algemeen gebruik ten behoeve van deze quickstart. 

1. Ga naar de [Azure-portal](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) en meld u aan met behulp van uw Azure-account. 
2. Voer een unieke naam in voor het opslagaccount. Neem de volgende regels in acht als u het opslagaccount een naam geeft:
    - De naam moet 3 tot 24 tekens lang zijn.
    - De naam mag alleen cijfers en kleine letters bevatten.
3. Zorg dat de volgende standaardwaarden zijn ingesteld: 
    - **Implementatiemodel** is ingesteld op **Resource Manager**.
    - **Soort account** is ingesteld op **Algemeen gebruik**.
    - **Prestatie** is ingesteld op **Standaard**.
    - **Replicatie** is ingesteld op **Lokaal redundante opslag (LRS)**.
4. Selecteer uw abonnement. 
5. Maak bij **Resourcegroep** een nieuwe en geef deze een unieke naam. 
6. Selecteer de locatie moet worden gebruikt voor uw opslagaccount.
7. Selecteer **vastmaken aan dashboard** en selecteer **maken** om uw opslagaccount te maken. 

Nadat uw storage-account is gemaakt, is vastgemaakt aan het dashboard. Selecteer deze te openen. Onder **instellingen**, selecteer **toegangssleutels**. Selecteer de primaire sleutel en de bijbehorende verbindingsreeks naar het Klembord kopiëren. Plak de tekenreeks in een teksteditor voor later gebruik.
