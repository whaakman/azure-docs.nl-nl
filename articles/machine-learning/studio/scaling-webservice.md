---
Titel: Schaal van een Machine Learning Studio web service titleSuffix: Azure Machine Learning Studio description: Leer hoe u gelijktijdigheid van een Azure Machine Learning Studio-webservice verbeteren door het toevoegen van extra eindpunten.
Services: machine learning ms.service: machine learning ms.subservice: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18, vorige ms.author=yahajiza, vorige auteur = YasinMSFT ms.date: 01/23/2017
---
# <a name="scaling-an-azure-machine-learning-studio-web-service-by-adding-additional-endpoints"></a>Een Azure Machine Learning Studio-webservice schalen door extra eindpunten toe te voegen
> [!NOTE]
> Dit onderwerp wordt beschreven technieken die van toepassing op een **klassieke** Machine Learning-webservice. 
> 
> 

Standaard is elke gepubliceerde webservice is geconfigureerd voor ondersteuning van 20 gelijktijdige aanvragen, en mag wel 200 gelijktijdige aanvragen. Azure Machine Learning optimaliseert automatisch de instelling voor de beste prestaties voor uw webservice en de portal waarde wordt genegeerd. 

Als u wilt de API met een hogere belasting dan een waarde voor maximum aantal gelijktijdige aanroepen van 200 aanroepen ondersteunt, moet u meerdere eindpunten maken van de dezelfde Web-service. U kunt uw load willekeurig distribueren op alle.

De schaal van een webservice is een veelvoorkomende taak. Er zijn enkele redenen om te schalen naar meer dan 200 gelijktijdige aanvragen ondersteunen, hogere mate van beschikbaarheid via meerdere eindpunten of het bieden van afzonderlijke eindpunten voor de webservice. U kunt de schaal vergroten door het toevoegen van extra eindpunten voor de dezelfde webservice via de [Azure Machine Learning-webservice](https://services.azureml.net/) portal.

Zie voor meer informatie over het toevoegen van nieuwe eindpunten [eindpunten maken](create-endpoint.md).

Houd er rekening mee dat met behulp van een aantal hoge gelijktijdigheid schadelijk als u niet van de API met een overeenkomstige hoge frequentie aanroepen bent. U ziet er kunnen sporadisch time-outs en/of pieken in de latentie als u een relatief lage belasting voor een API die is geconfigureerd voor hoge belasting.

De synchrone API's worden meestal gebruikt in situaties waar een lage latentie is vereist. Hier latentie betekent de tijd die nodig is voor de API om een aanvraag te voltooien en niet wordt gehouden met eventuele vertragingen in het netwerk. Stel dat u hebt een API met een latentie 50 ms. Volledig gebruiken voor de beschikbare capaciteit met toevoerregelniveau hoog en het maximum aantal gelijktijdige aanroepen = 20, moet u deze API 20 * 1000 aanroepen / 50 = 400 keer per seconde. Een maximum aantal gelijktijdige aanroepen van 200 kunt dit verder uitgebreid, u voor het aanroepen van de API 4000 mislukte opvragingen per seconde, ervan uitgaande dat het een 50 ms latentie.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
