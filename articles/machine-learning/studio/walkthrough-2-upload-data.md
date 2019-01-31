---
Titel: ' Stap 2: Gegevens uploaden naar een Machine Learning Studio-experiment' titleSuffix: Azure Machine Learning Studio description: ' Stap 2 van het ontwikkelen van een overzicht van de voorspellende oplossing: Uploaden van openbare gegevens opgeslagen in Azure Machine Learning Studio.'
Services: machine learning ms.service: machine learning ms.subservice: studio ms.topic: artikel

Auteur: garyericson ms.author: garye ms.custom: vorige auteur = heatherbshapiro, vorige ms.author=hshapiro ms.date: 03/23/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-studio-experiment"></a>Kennismaken, stap 2: Bestaande gegevens uploaden naar een Azure Machine Learning Studio-experiment
Dit is de tweede stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. **Bestaande gegevens uploaden**
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. [De webservice openen](walkthrough-6-access-web-service.md)

- - -
Voor het ontwikkelen van een Voorspellend model voor kredietrisico, moeten we de gegevens die we gebruiken kunnen om te trainen en vervolgens het model te testen. Voor dit scenario gebruiken we 'UCI Statlog (Duitse tegoed gegevens) Data Set' uit de opslagplaats UC Irvine Machine Learning. U kunt deze hier vinden:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

We gebruiken het bestand met de naam **german.data**. Dit bestand downloaden naar uw lokale vaste schijf.  

De **german.data** gegevensset bevat rijen met 20 variabelen voor 1000 afgelopen aanvragers tegoed. Deze 20 variabelen van de gegevensset en de set functies vertegenwoordigen (de *functie vector*), waarmee u identificerende kenmerken voor elke kandidaat-tegoed. Een extra kolom in elke rij vertegenwoordigt van de aanvrager berekende kredietrisico, met 700 sollicitanten geïdentificeerd als een lage kredietrisico en 300 als een hoog risico.

De UCI-website bevat een beschrijving van de kenmerken van de functie vector voor deze gegevens. Dit omvat financiële gegevens, kredietgeschiedenis werkstatus en persoonlijke gegevens. Voor elke kandidaat een binaire classificatie is opgegeven waarmee wordt aangegeven of ze een lage zijn of hoge kredietrisico. 

We gebruiken deze gegevens om een Voorspellend model te trainen. Als we klaar bent, het model zou het mogelijk om te accepteren van een functie-vector voor een nieuwe persoon en of hij of zij een lage of hoge kredietrisico is te voorspellen.  

Hier volgt een interessante twist. De beschrijving van de gegevensset op de website van UCI noemt wat kost het als we misclassify kredietrisico van een persoon.
Als het model een hoge kredietrisico voor iemand die daadwerkelijk een lage kredietrisico's is voorspelt, heeft het model een misclassification gemaakt.
Maar de omgekeerde misclassification is vijf keer duurder aan de financiële instelling: als het model voorspelt een lage kredietrisico voor iemand die daadwerkelijk een hoge kredietrisico's is.

Ja, we willen onze model te trainen, zodat de kosten van deze laatste type misclassification vijf keer hoger is dan de andere manier onjuiste.
Een eenvoudige manier om dit te doen bij het trainen van het model in onze experiment is door te dupliceren (vijf keer) de items die iemand met een hoge kredietrisico's vertegenwoordigen. Klik, als het model ten onrechte iemand als een lage kredietrisico's classificeert als ze daadwerkelijk een hoog risico, het model geen die dezelfde misclassification vijf keer één keer voor elke kopie. Hierdoor neemt de kosten van deze fout in de resultaten van de training.


## <a name="convert-the-dataset-format"></a>De gegevensset-indeling converteren
De oorspronkelijke gegevensset maakt gebruik van een lege gescheiden-indeling. Machine Learning Studio werkt beter met een bestand met door komma's gescheiden waarden (CSV), zodat we de gegevensset converteren u door spaties vervangen door komma's.  

Er zijn veel manieren om deze gegevens te converteren. Er is één manier met behulp van de volgende Windows PowerShell-opdracht:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Een andere manier is met behulp van de Unix-sed-opdracht:  

    sed 's/ /,/g' german.data > german.csv  

We hebben een versie met door komma's gescheiden van de gegevens in een bestand met de naam gemaakt in beide gevallen **german.csv** die we in onze experiment kunt gebruiken.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>Upload de gegevensset voor Machine Learning Studio
Zodra de gegevens is geconverteerd naar een CSV-indeling, moeten we het uploaden naar Machine Learning Studio. 

1. Open de startpagina van de Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Klik in het menu ![Menu][1] in de linkerbovenhoek van het venster, klikt u op **Azure Machine Learning**, selecteer **Studio**, en meld u aan.

3. Klik op **+ nieuw** aan de onderkant van het venster.

4. Selecteer **GEGEVENSSET**.

5. Selecteer **vanuit het lokale bestand**.

    ![Een gegevensset van een lokaal bestand toevoegen][2]

6. In de **uploaden van een nieuwe gegevensset** dialoogvenster, klikt u op **Bladeren** en zoek de **german.csv** bestand dat u hebt gemaakt.

7. Voer een naam voor de gegevensset. Voor dit scenario aanroepen 'UCI Duits creditcardgegevens'.

8. Selecteer voor het gegevenstype, **generieke CSV-bestand met geen header (. nh.csv)**.

9. Voeg een beschrijving toe als u wilt.

10. Klik op de **OK** selectievakje is ingeschakeld.  

    ![Upload de gegevensset][3]

Hiermee uploadt de gegevens in een gegevensset-module die we in een experiment kunt gebruiken.

U kunt beheren gegevenssets die u hebt geüpload naar Studio door te klikken op de **GEGEVENSSETS** tabblad aan de linkerkant van de Studio-venster.

![Gegevenssets beheren][4]

Zie voor meer informatie over het importeren van andere typen gegevens in een experiment [uw trainingsgegevens importeren in Azure Machine Learning Studio](import-data.md).

**Volgende: [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)**

[1]: media/walkthrough-2-upload-data/menu.png
[2]: media/walkthrough-2-upload-data/add-dataset.png
[3]: media/walkthrough-2-upload-data/upload-dataset.png
[4]: media/walkthrough-2-upload-data/dataset-list.png
