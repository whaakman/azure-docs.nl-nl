---
Titel: Excel-invoegtoepassing voor web services titleSuffix: Azure Machine Learning Studio description: Het gebruik van Azure Machine Learning-webservices rechtstreeks in Excel zonder een code te schrijven.
Services: machine learning ms.service: machine learning ms.subservice: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 02/01/2018
---
# <a name="excel-add-in-for-azure-machine-learning-studio-web-services"></a>Excel-invoegtoepassing voor Azure Machine Learning Studio-webservices
Excel maakt het eenvoudig om aan te roepen webservices rechtstreeks zonder code te schrijven.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Stappen voor het gebruik van een bestaande webservice in de werkmap

1. Open de [Excel-voorbeeldbestand](https://aka.ms/amlexcel-sample-2), die de Excel-invoegtoepassing en de gegevens over passagiers op ramp met de Titanic bevat. 
 
> [!NOTE]
> U ziet de lijst met de webservices die betrekking hebben op het bestand en onder een selectievakje voor "automatisch"voorspellen. Als u inschakelt auto-voorspellen de voorspellingen van **alle** uw services worden bijgewerkt telkens wanneer er een wijziging in de invoer is. Als dit selectievakje is uitgeschakeld wordt u voor vernieuwing te klikken op 'Alle voorspellen' hebben. Voor het inschakelen van automatische-voorspellen op een service level gaat u naar stap 6.

2. De webservice kiezen door erop te klikken-' Titanic nagelaten voorspelde (Excel-invoegtoepassing voorbeeld) [Score] ' in dit voorbeeld.
   
    ![-Webservice selecteren][01]
3. Hiermee gaat u naar de **Predict** sectie.  Deze werkmap bevat al voorbeeldgegevens, maar voor een lege werkmap kunt u een cel selecteren in Excel en klikt u op **Gebruik voorbeeldgegevens**.
4. Selecteer de gegevens met kopteksten en klikt u op het pictogram voor het bereik van invoergegevens.  Zorg ervoor dat het selectievakje 'Mijn gegevens bevatten kopteksten' is ingeschakeld.
5. Onder **uitvoer**, voer het aantal cellen waar u de uitvoer te zijn, bijvoorbeeld "H1" hier.
6. Klik op **voorspellen**. Als u het selectievakje "auto-predict" activeert geen wijzigingen in de geselecteerde gebieden (de items die zijn opgegeven als invoer) een aanvraag en een update van de cellen uitvoer zonder de noodzaak voor u om de predict-knop te drukken.
   
    ![Sectie voorspellen][02]

Een webservice implementeren of gebruik een bestaande webservice. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: De Azure Machine Learning-webservice implementeren](walkthrough-5-publish-web-service.md).

Haal de API-sleutel voor uw webservice. Wanneer u deze actie is afhankelijk van of u een klassieke Machine Learning-webservice van een nieuwe Machine Learning-webservice hebt gepubliceerd.

**Een klassieke webservice gebruiken** 

1. In Machine Learning Studio, klikt u op de **WEBSERVICES** sectie in het linkerdeelvenster en selecteer vervolgens de web-service.
   
    ![Studio selecteert een webservice][04]
2. Kopieer de API-sleutel voor de webservice.
   
    ![Studio API-sleutel][05]
3. Op de **DASHBOARD** tabblad voor de webservice, klikt u op de **aanvraag/antwoord** koppeling.
4. Zoek de **aanvraag-URI** sectie.  Kopieer en bewaar de URL.

> [!NOTE]
> Het is nu mogelijk aan te melden bij de [Azure Machine Learning-webservices](https://services.azureml.net) -portal voor het verkrijgen van de API-sleutel voor een klassieke Machine Learning-webservice.
> 
> 

**Een nieuwe webservice gebruiken**

1. In de [Azure Machine Learning-webservices](https://services.azureml.net) en klik op **webservices**, selecteer vervolgens uw webservice. 
2. Klik op **gebruiken**.
3. Zoek de **Basic verbruik info** sectie. Kopieer en bewaar de **primaire sleutel** en de **Request Response** URL.

## <a name="steps-to-add-a-new-web-service"></a>Stappen voor het toevoegen van een nieuwe webservice

1. Een webservice implementeren of gebruik een bestaande webservice. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: De Azure Machine Learning-webservice implementeren](walkthrough-5-publish-web-service.md).
2. Klik op **gebruiken**.
3. Zoek de **Basic verbruik info** sectie. Kopieer en bewaar de **primaire sleutel** en de **Request Response** URL.
4. In Excel, gaat u naar de **webservices** sectie (als u zich in de **Predict** sectie, klikt u op de pijl-terug naar de lijst van webservices).
   
    ![Ga naar de Web service selecteren][03]
5. Klik op **-webservice toevoegen**.
6. Plak de URL in de Excel-invoegtoepassing tekstvak met het label **URL**.
7. Plak de API/primaire sleutel in het tekstvak met het label **API-sleutel**.
8. Klik op **Add**.
   
    ![URL en API-sleutel voor een klassieke webservice.][06]
9. Voor het gebruik van de webservice, volgt u de voorgaande instructies, "Stappen voor het gebruik van een bestaande web Service."

## <a name="sharing-your-workbook"></a>Delen van uw werkmap
Als u uw werkmap opslaat, wordt ook de API/primaire sleutel voor de web-services die u hebt toegevoegd opgeslagen. Dit betekent dat u moet de werkmap alleen delen met personen die u vertrouwt.

Vragen in de volgende sectie van de opmerking of op onze [forum](https://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/excel-add-in-for-web-services/image1.png
[02]: ./media/excel-add-in-for-web-services/image2.png
[03]: ./media/excel-add-in-for-web-services/image3.png
[04]: ./media/excel-add-in-for-web-services/image4.png
[05]: ./media/excel-add-in-for-web-services/image5.png
[06]: ./media/excel-add-in-for-web-services/image6.png
