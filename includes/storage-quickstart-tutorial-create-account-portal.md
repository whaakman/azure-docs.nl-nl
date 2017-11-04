## <a name="create-a-storage-account-using-the-azure-portal"></a>Een opslagaccount met de Azure portal maken

Maak eerst een nieuwe, algemeen opslagaccount moet worden gebruikt voor deze snelstartgids. 

1. Ga naar de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account. 
2. Selecteer in het menu Hub **nieuw** > **opslag** > **opslagaccount - blob, bestand, tabel, wachtrij**. 
3. Geef een unieke naam voor uw opslagaccount. Houd deze regels voor de naamgeving van uw opslagaccount:
    - De naam moet tussen 3 en 24 tekens lang zijn.
    - De naam mag alleen cijfers en kleine letters bevatten.
4. Zorg ervoor dat de volgende standaardwaarden zijn ingesteld: 
    - **Implementatiemodel** is ingesteld op **resourcemanager**.
    - **Account kind** is ingesteld op **algemeen**.
    - **Prestaties** is ingesteld op **standaard**.
    - **Replicatie** is ingesteld op **lokaal redundante opslag (LRS)**.
5. Selecteer uw abonnement. 
6. Voor **resourcegroep**, een nieuwe maken en een unieke naam geven. 
7. Selecteer de **locatie** moet worden gebruikt voor uw opslagaccount.
8. Controleer **vastmaken aan dashboard** en klik op **maken** om uw opslagaccount te maken. 

Nadat uw storage-account is gemaakt, wordt deze is vastgemaakt aan het dashboard. Klik op deze te openen. Onder **instellingen**, klikt u op **toegangssleutels**. Selecteer de primaire sleutel en kopieer het bijbehorende **verbindingsreeks** naar het Klembord, plak deze in een teksteditor voor later gebruik.
