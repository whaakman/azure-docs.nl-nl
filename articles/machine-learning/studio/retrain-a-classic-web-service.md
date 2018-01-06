---
title: Een klassieke webservice Retrain | Microsoft Docs
description: Informatie over het programmatisch opnieuw trainen van een model en het bijwerken van de webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: ad18d82109e3048625f32d90af9677956350fb84
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="retrain-a-classic-web-service"></a>Een klassieke webservice opnieuw trainen
De voorspellende webservice die u hebt geïmplementeerd is de standaardinstelling score-eindpunt. Standaardeindpunten worden gesynchroniseerd met de oorspronkelijke training en experimenten score berekenen en daarom het getrainde model voor het standaardeindpunt kan niet worden vervangen. Als u wilt opnieuw trainen van de webservice, moet u een nieuw eindpunt toevoegen aan de webservice. 

## <a name="prerequisites"></a>Vereisten
U moet een trainingsexperiment en een Voorspellend experiment hebt ingesteld zoals wordt weergegeven in [Retrain Machine Learning-modellen programmatisch](retrain-models-programmatically.md). 

> [!IMPORTANT]
> De Voorspellend experiment moet worden geïmplementeerd als een klassiek machine learning-webservice. 
> 
> 

Zie voor meer informatie over webservices implementeren [Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Een nieuw eindpunt toevoegen
De voorspellende webservice die u hebt geïmplementeerd, bevat een standaardexemplaar score-eindpunt dat is gesynchroniseerd met de oorspronkelijke training en experimenten getrainde model score berekenen. Als u wilt uw webservice bijwerken met een nieuwe getraind model, moet u een nieuw score-eindpunt. 

Een nieuw score-eindpunt op de voorspellende webservice die kan worden bijgewerkt met het getrainde model maken:

> [!NOTE]
> Zorg ervoor dat u het eindpunt wilt toevoegen aan de voorspellende Web Service, niet de trainings-webservice. Als u correct zowel een trainings- en een Voorspellend webservice hebt geïmplementeerd, ziet u twee afzonderlijke webservices die worden vermeld. De voorspellende webservice moet eindigen met '[voorspellende exp.]'.
> 
> 

Er zijn twee manieren waarop u een nieuw eindpunt bij een webservice kunt toevoegen:

1. Programmatisch
2. Gebruik de Microsoft Azure Web Services-portal

### <a name="programmatically-add-an-endpoint"></a>Programmatisch een eindpunt toevoegen
U kunt toevoegen scoreprofiel eindpunten voor de voorbeeldcode in dit [github-opslagplaats](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a>De Microsoft Azure Web Services-portal gebruiken voor een eindpunt toevoegen
1. Klik op webservices op de linkernavigatiebalk-kolom in Machine Learning Studio.
2. Klik aan de onderkant van het dashboard van web service **beheren eindpunten preview**.
3. Klik op **Add**.
4. Typ een naam en beschrijving voor het nieuwe eindpunt. Selecteer het niveau van logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie voor meer informatie over logboekregistratie [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Het toegevoegde eindpunt getraind model bijwerken
De retraining om proces te voltooien, moet u het getrainde model van het nieuwe eindpunt dat u hebt toegevoegd bijwerken.

Als u het eindpunt met de voorbeeldcode hebt toegevoegd, dit omvat de locatie van de help-URL die is geïdentificeerd door de *HelpLocationURL* waarde in de uitvoer.

Voor het ophalen van de pad-URL:

1. Kopieer en plak de URL in uw browser.
2. Klik op de koppeling van de Update-Resource.
3. Kopieer de POST-URL van de PATCH-aanvraag. Bijvoorbeeld:
   
     URL VAN DE PATCH: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2

U kunt nu het getrainde model gebruiken om bij te werken het score-eindpunt dat u eerder hebt gemaakt.

De volgende voorbeeldcode laat zien u hoe u de *BaseLocation*, *RelativeLocation*, *SasBlobToken*, en PATCH URL naar het eindpunt worden bijgewerkt.

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

De *apiKey* en de *endpointUrl* voor de aanroep kan worden verkregen van endpoint-dashboard.

De waarde van de *naam* parameter in *Resources* moet overeenkomen met de naam van de Resource van het opgeslagen getraind Model in de Voorspellend experiment. De resourcenaam ophalen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **Machine Learning**.
3. Onder de naam, uw werkruimte op en klik op **webservices**.
4. Klik onder de naam, **telling Model [voorspellende exp].** .
5. Klik op het nieuwe eindpunt dat u hebt toegevoegd.
6. Klik op het dashboard endpoint **Update Resource**.
7. U kunt vinden op de pagina Update Resource API-documentatie voor de webservice de **resourcenaam** onder **bij te werken bronnen**.

Als uw SAS-token is verstreken voordat u klaar bent met het bijwerken van het eindpunt, moet u een GET met de taak-Id verkrijgen van een nieuw token uitvoeren.

Wanneer de code met succes is uitgevoerd, kan het nieuwe eindpunt moet beginnen met het retrained model in ongeveer 30 seconden.

## <a name="summary"></a>Samenvatting
De Retraining API's kunt u het getrainde model van een Voorspellend webservice inschakelen van scenario's zoals bijwerken:

* Periodieke model retraining met nieuwe gegevens.
* Distributie van een model voor klanten met het doel van zodat ze opnieuw trainen van het model met hun eigen gegevens.

## <a name="next-steps"></a>Volgende stappen
[Het oplossen van de retraining van een klassieke Azure Machine Learning-webservice](troubleshooting-retraining-models.md)

