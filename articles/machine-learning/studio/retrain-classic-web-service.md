---
title: Een klassieke webservice opnieuw trainen en implementeren
titleSuffix: Azure Machine Learning Studio
description: Informatie over het opnieuw trainen van een model en het bijwerken van een klassieke webservice voor het gebruik van het zojuist getrainde model in Azure Machine Learning Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: peterlu
ms.author: amlstudiodocs
ms.custom: seodec18, previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 02/14/2019
ms.openlocfilehash: 73ab816c7043c7fb0f572677c6ceeb722653dec1
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650767"
---
# <a name="retrain-and-deploy-a-classic-studio-web-service"></a>Opnieuw trainen en implementeren van een klassieke Studio-webservice

Opnieuw trainen van modellen voor machine learning is een manier om te zorgen dat ze nauwkeurig en op basis van de meest relevante gegevens beschikbaar blijven. Dit artikel wordt beschreven hoe u een klassieke Studio-webservice opnieuw trainen. Voor richtlijnen voor hoe u een nieuwe Studio-webservice opnieuw trainen [weergeven in dit artikel met instructies.](retrain-machine-learning-model.md)

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt al een retraining experiment zowel een Voorspellend experiment. Deze stappen worden beschreven [opnieuw trainen en implementeren van een machine learning-model.](retrain-models-programmatically.md) In plaats van uw machine learning-model als een nieuwe webservice implementeert, wordt u echter uw Voorspellend experiment implementeren als een klassieke webservice.
     
## <a name="add-a-new-endpoint"></a>Een nieuw eindpunt toevoegen

De voorspellende webservice die u hebt geïmplementeerd, bevat een standaardexemplaar scoringeindpunt dat worden bewaard gesynchroniseerd met de oorspronkelijke training en scoren experimenten getrainde model. Voor het bijwerken van uw webservice om met een nieuwe getrainde model, moet u een nieuw scoring-eindpunt maken.

Er zijn twee manieren waarop u een nieuw eindpunt aan een webservice toevoegen kunt:

* Programmatisch
* Met behulp van de webservices van Azure-portal

> [!NOTE]
> Zorg ervoor dat u het eindpunt wilt toevoegen aan de voorspellende webservice, niet de Training-webservice. Als u goed zowel een Training en een voorspellende webservice hebt geïmplementeerd, ziet u twee afzonderlijke services die worden vermeld. De voorspellende webservice moet eindigen met '[voorspellende exp.]'.
>

### <a name="programmatically-add-an-endpoint"></a>Een eindpunt via een programma toevoegen

U kunt toevoegen scoring-eindpunten met behulp van de voorbeeldcode in dit [GitHub-opslagplaats](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint).

### <a name="use-the-azure-web-services-portal-to-add-an-endpoint"></a>Gebruik de webservices van Azure portal een eindpunt toevoegen

1. In Machine Learning Studio, klikt u op de linkernavigatiekolom op Web Services.
1. Klik aan de onderkant van het dashboard van de webservice op **beheren eindpunten preview**.
1. Klik op **Add**.
1. Typ een naam en beschrijving voor het nieuwe eindpunt. Selecteer het niveau van logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie voor meer informatie over logboekregistratie, [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Het getrainde model van het toegevoegde eindpunt bijwerken

### <a name="retrieve-patch-url"></a>PATCH-URL ophalen

### <a name="option-1-programmatically"></a>Optie 1: Programmatisch

Als u de juiste URL voor het vullen van de programmacode, de volgende stappen uit:

1. Voer de [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) voorbeeldcode.
1. Uit de uitvoer van AddEndpoint, vinden de *HelpLocation* waarde en kopieer de URL.

   ![HelpLocation in de uitvoer van het voorbeeld addEndpoint.](./media/retrain-classic/addEndpoint-output.png)
1. Plak de URL in een browser om te navigeren naar een pagina met help-koppelingen voor de webservice.
1. Klik op de **resources bijwerken** koppeling om de patch help-pagina te openen.

### <a name="option-2-use-the-azure-machine-learning-web-services-portal"></a>Optie 2: Gebruik de portal voor Azure Machine Learning-webservices

Volg deze stappen voor het ophalen van de juiste URL vullen met behulp van de web-portal:

1. Aanmelden bij de [Azure Machine Learning-webservices](https://services.azureml.net/) portal.
1. Klik op **webservices** of **klassieke webservices** aan de bovenkant.
1. Klik op de scoringwebservice u met werkt (als u de standaardnaam van de webservice niet wijzigt, deze wordt beëindigd in "[score Exp.]").
1. Klik op **+ nieuw**.
1. Nadat het eindpunt is toegevoegd, klik op de naam van het eindpunt.
1. Onder de **Patch** URL, klikt u op **API Help** de patch Helppagina te openen.

> [!NOTE]
> Als u het eindpunt met de webservice voor de Training in plaats van de voorspellende webservice hebt toegevoegd, ontvangt de volgende fout wanneer u klikt op de **resources bijwerken** koppeling: "Onze excuses, maar deze functie wordt niet ondersteund of beschikbaar in deze context. Deze webservice heeft geen resources bij te werken. We excuses voor het ongemak en worden gewerkt aan het verbeteren van deze werkstroom."
>

De PATCH help-pagina bevat de vullen van de URL die u moet gebruiken en voorbeeldcode die u gebruiken kunt om aan te roepen van het biedt.

![URL van de patch.](./media/retrain-classic/ml-help-page-patch-url.png)

### <a name="update-the-endpoint"></a>Het eindpunt bijwerken

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
1. Klik in het menu links op **Machine Learning**.
1. Onder de naam, klikt u op uw werkruimte en klik vervolgens op **webservices**.
1. Onder de naam, klikt u op **telling Model [voorspellende exp.]** .
1. Klik op het nieuwe eindpunt dat u hebt toegevoegd.
1. Klik op het dashboard endpoint **resources bijwerken**.
1. Op de pagina Update Resource-API-documentatie voor de webservice, u vindt de **resourcenaam** onder **bij te werken Resources**.

Als uw SAS-token is verlopen voordat u klaar bent met het bijwerken van het eindpunt, moet u een ophalen met de taak-ID verkrijgen van een nieuwe token uitvoeren.

Wanneer de code met succes is uitgevoerd, kan het nieuwe eindpunt moet beginnen met het retrained model in ongeveer 30 seconden.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beheren van webservices of bijhouden van meerdere experimenten wordt uitgevoerd, de volgende artikelen:

* [Maak kennis met de Web Services-portal](manage-new-webservice.md)
* [Experimentherhalingen beheren](manage-experiment-iterations.md)