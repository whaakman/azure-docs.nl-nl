---
Titel: Opnieuw trainen een klassieke web service titleSuffix: Azure Machine Learning Studio description: Leer hoe u programmatisch opnieuw trainen van een model en bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning.
Services: machine learning ms.service: machine learning ms.component: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18, vorige ms.author=yahajiza, vorige auteur = YasinMSFT ms.date: 04/19/2017
---
# <a name="retrain-a-classic-azure-machine-learning-studio-web-service"></a>Een klassieke Azure Machine Learning Studio-webservice opnieuw trainen
De voorspellende webservice die u hebt geïmplementeerd is de standaardinstelling scoringeindpunt. Standaardeindpunten worden bewaard gesynchroniseerd met de oorspronkelijke training en scoren van experimenten en daarom het getrainde model voor het standaardeindpunt kan niet worden vervangen. Als u wilt de webservice opnieuw trainen, moet u een nieuw eindpunt toevoegen aan de webservice.

## <a name="prerequisites"></a>Vereisten
U moet een opleidingsexperiment en een Voorspellend experiment hebt ingesteld zoals wordt weergegeven in [opnieuw trainen Machine Learning-modellen programmatisch](retrain-models-programmatically.md).

> [!IMPORTANT]
> De Voorspellend experiment moet worden geïmplementeerd als een klassieke virtuele machine learning-webservice.
>
>

Zie voor meer informatie over webservices implementeren, [een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Een nieuw eindpunt toevoegen
De voorspellende webservice die u hebt geïmplementeerd, bevat een standaardexemplaar scoringeindpunt dat worden bewaard gesynchroniseerd met de oorspronkelijke training en scoren experimenten getrainde model. Voor het bijwerken van uw webservice om met een nieuwe getrainde model, moet u een nieuw scoring-eindpunt maken.

Een nieuw scoring-eindpunt, op de voorspellende webservice die kunnen worden bijgewerkt met het getrainde model maken:

> [!NOTE]
> Zorg ervoor dat u het eindpunt wilt toevoegen aan de voorspellende webservice, niet de Training-webservice. Als u goed zowel een Training en een voorspellende webservice hebt geïmplementeerd, ziet u twee afzonderlijke services die worden vermeld. De voorspellende webservice moet eindigen met '[voorspellende exp.]'.
>
>

Er zijn twee manieren waarop u een nieuw eindpunt aan een webservice toevoegen kunt:

1. Programmatisch
2. Gebruik de webservices van Microsoft Azure-portal

### <a name="programmatically-add-an-endpoint"></a>Een eindpunt via een programma toevoegen
U kunt toevoegen scoring-eindpunten met behulp van de voorbeeldcode in dit [GitHub-opslagplaats](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint).

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a>Gebruik de webservices van Microsoft Azure-portal een eindpunt toevoegen
1. In Machine Learning Studio, klikt u op de linkernavigatiekolom op Web Services.
2. Klik aan de onderkant van het dashboard van de webservice op **beheren eindpunten preview**.
3. Klik op **Add**.
4. Typ een naam en beschrijving voor het nieuwe eindpunt. Selecteer het niveau van logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie voor meer informatie over logboekregistratie, [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Het getrainde model van het toegevoegde eindpunt bijwerken
Als u wilt de retraining voltooien, moet u het getrainde model van het nieuwe eindpunt dat u hebt toegevoegd bijwerken.

Als u het eindpunt met behulp van de voorbeeldcode hebt toegevoegd, dit omvat de locatie van de help-URL die is geïdentificeerd door de *HelpLocationURL* waarde in de uitvoer.

De pad-URL ophalen:

1. Kopieer en plak de URL in uw browser.
2. Klik op de koppeling resources bijwerken.
3. Kopieer de POST-URL van de PATCH-aanvraag. Bijvoorbeeld:
   
     URL VAN DE PATCH: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2

U kunt nu het getrainde model gebruiken om bij te werken het scoring-eindpunt dat u eerder hebt gemaakt.

De volgende voorbeeldcode laat zien hoe u de *BaseLocation*, *RelativeLocation*, *SasBlobToken*, en de URL PATCH om bij te werken van het eindpunt.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

De *apiKey* en de *endpointUrl* voor de aanroep kan worden opgehaald van de eindpunt-dashboard.

De waarde van de *naam* parameter in *Resources* moet overeenkomen met de naam van de Resource van de opgeslagen getrainde Model in de Voorspellend experiment. Naam van de Resource ophalen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **Machine Learning**.
3. Onder de naam, klikt u op uw werkruimte en klik vervolgens op **webservices**.
4. Onder de naam, klikt u op **telling Model [voorspellende exp.]** .
5. Klik op het nieuwe eindpunt dat u hebt toegevoegd.
6. Klik op het dashboard endpoint **resources bijwerken**.
7. Op de pagina Update Resource-API-documentatie voor de webservice, u vindt de **resourcenaam** onder **bij te werken Resources**.

Als uw SAS-token is verlopen voordat u klaar bent met het bijwerken van het eindpunt, moet u een ophalen met de taak-Id verkrijgen van een nieuwe token uitvoeren.

Wanneer de code met succes is uitgevoerd, kan het nieuwe eindpunt moet beginnen met het retrained model in ongeveer 30 seconden.

## <a name="summary"></a>Samenvatting
Met behulp van de Retraining API's, kunt u het getrainde model van een voorspellende webservice inschakelen van scenario's zoals bijwerken:

* Periodieke model opnieuw trainen met nieuwe gegevens.
* Distributie van een model voor klanten met het doel om ze opnieuw trainen het model met behulp van hun eigen gegevens.

## <a name="next-steps"></a>Volgende stappen
[Het oplossen van het opnieuw trainen van een klassieke Azure Machine Learning-webservice](troubleshooting-retraining-models.md)

