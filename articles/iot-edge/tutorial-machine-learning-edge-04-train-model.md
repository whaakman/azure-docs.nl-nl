---
title: Trainen en implementeren van een model - Machine Learning in Azure IoT Edge | Microsoft Docs
description: Een machine learning-model met behulp van Azure Machine Learning te trainen en het model als een containerinstallatiekopie die kan worden geïmplementeerd als een Azure IoT Edge-Module in het pakket.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4a94560bcb66c2ed59f78eef4f6a08b3f0227dc4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057636"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Zelfstudie: Trainen en implementeren van een Azure Machine Learning-model

> [!NOTE]
> In dit artikel maakt deel uit van een reeks voor een zelfstudie over het gebruik van Azure Machine Learning op IoT Edge. Als u rechtstreeks in dit artikel zijn ontvangen, u wordt aangeraden te beginnen met de [eerste artikel](tutorial-machine-learning-edge-01-intro.md) in de reeks voor de beste resultaten.

In dit artikel, we gebruiken Azure-notitieblokken eerst naar een machine learning-model met behulp van Azure Machine Learning te trainen en vervolgens het pakket dat model als een containerinstallatiekopie die kan worden geïmplementeerd als een Azure IoT Edge-Module. De Azure-Notebooks profiteren van een Azure Machine Learning-service-werkruimte en dit is een fundamentele blok gebruikt om te experimenteren en implementeren van machine learning-modellen trainen.

De activiteiten in dit gedeelte van de zelfstudie zijn verdeeld in twee notitieblokken.

* **01-turbofan\_regression.ipynb:** Dit notitieblok begeleid bij de stappen voor het trainen en publiceren van een model met behulp van Azure Machine Learning. Ruime zin is zijn de vereiste stappen:

  1. Downloaden en verkennen van de trainingsgegevens voorbereiden
  2. Gebruik de service-werkruimte maken en uitvoeren van een machine learning-experiment
  3. Evalueren van de modelresultaten van het experiment
  4. Het beste model publiceren naar de werkruimte van de service

* **02-turbofan\_deploy\_model.ipynb:** Dit notitieblok neemt het model in de vorige notebook gemaakt en gebruikt voor het maken van een containerinstallatiekopie die kan worden geïmplementeerd voor een Azure IoT Edge-apparaat.

  1. Een scoring-script voor het model maken
  2. Maken en publiceren van de installatiekopie
  3. De installatiekopie implementeert als een webservice op Azure Container Instance
  4. De web-service gebruiken om te valideren van het model en de installatiekopie werken zoals verwacht

De stappen in dit artikel kunnen doorgaans worden uitgevoerd door de datawetenschappers.

## <a name="set-up-azure-notebooks"></a>Azure-notitieblokken instellen

We gebruiken Azure-notitieblokken voor het hosten van de twee Jupyter Notebooks en ondersteunende bestanden. Hier we maken en configureren van een Azure-notitieblokken-project. Als u niet de Jupyter-en/of Azure-notitieblokken gebruikt hebt, zijn hier een aantal inleidende documenten:

* **Snelstartgids:** [Een notebook maken en delen](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Zelfstudie:** [Maken en uitvoeren van een Jupyter-notebook met Python](../notebooks/tutorial-create-run-jupyter-notebook.md)

Als met de ontwikkelaar van virtuele machine, met behulp van Azure notebooks zorgt ervoor dat een consistente omgeving voor de oefening.

> [!NOTE]
> Eenmaal hebt ingesteld, wordt de service Azure notitieblokken toegankelijk vanaf elke virtuele machine. Tijdens de installatie, moet u de ontwikkeling van virtuele machine waaraan alle van de bestanden die u nodig hebt.

### <a name="create-an-azure-notebooks-account"></a>Een Azure-notitieblokken-account maken

Azure Notebook-accounts zijn onafhankelijk van de Azure-abonnementen. Voor het gebruik van Azure-laptops, moet u een account maken.

1. Navigeer naar [Azure notitieblokken](http://notebooks.azure.com).

2. Klik op **aanmelden** in de bovenste, rechter hoek van de pagina.

3. Meld u aan met uw werk of school-account (Azure Active Directory) of uw persoonlijke account (Microsoft-Account).

4. Als u Azure-notitieblokken voordat u niet hebt gebruikt, wordt u gevraagd om toegang voor de Azure-notitieblokken-app te verlenen.

5. Een gebruikers-ID voor Azure-notitieblokken maken.

### <a name="upload-jupyter-notebooks-files"></a>Jupyter-notebooks bestanden uploaden

In deze stap wordt een nieuw project voor de Azure-notitieblokken maken en bestanden uploaden naar het. Met name zijn de bestanden die we uploaden:

* **01-turbofan\_regression.ipynb**: Jupyter notebook bestand die helpt bij het proces van het downloaden van de gegevens die worden gegenereerd door de basis van het apparaat van de Azure storage-account; verkennen en voor te bereiden voor de classificatie; training het model trainen testen van de gegevens met behulp van de testgegevensset gevonden in de Test\_FD003.txt bestand; ten slotte het opslaan van de classificatie voor het model en in de werkruimte van de Machine Learning-service.

* **02-turbofan\_deploy\_model.ipynb:** Jupyter-notebook die u bij het proces begeleidt van het gebruik van de classificatie-model dat is opgeslagen in de werkruimte van de Machine Learning-service voor het produceren van een containerinstallatiekopie. Nadat de installatiekopie is gemaakt, wordt de notebook wordt beschreven hoe u door het proces van het implementeren van de installatiekopie als een webservice, zodat u kunt controleren of deze zoals werkt verwacht. De installatiekopie van het gevalideerde wordt geïmplementeerd op de Edge-apparaat in de [maken en implementeren van aangepaste IoT Edge-modules](tutorial-machine-learning-edge-06-custom-modules.md) gedeelte van deze zelfstudie.

* **Test\_FD003.txt:** Dit bestand bevat de gegevens die we onze test ingesteld gebruiken bij het valideren van onze getraind classificatie. We hebben gekozen om de testgegevens als de oorspronkelijke wedstrijd bedoeld als onze test is ingesteld voor het gemak van het voorbeeld te gebruiken.

* **RUL\_FD003.txt:** Dit bestand bevat de resterende Levensduur voor de laatste cyclus van elk apparaat in de Test\_FD003.txt-bestand. Zie de **Leesmij.txt** en de **schade doorgifte Modeling.pdf** bestanden in de C:\\bron\\IoTEdgeAndMlSample\\gegevens\\Turbofan voor een gedetailleerde uitleg van de gegevens.

* **Utils.py:** Bevat een set functies voor Python-hulpprogramma voor het werken met gegevens. Het eerste notitieblok bevat een gedetailleerde beschrijving van de functies.

* **README.md:** Leesmij-bestand met een beschrijving van het gebruik van de notebooks.

Een nieuw project maken en de bestanden uploaden naar uw laptop.

1. Selecteer **Mijn projecten** in de bovenste menubalk.

1. Selecteer **+ nieuw Project**. Geef een naam en een ID. Er is niet nodig voor het project op openbaar of hebt u een Leesmij-bestand.

1. Selecteer **uploaden** en kies **van Computer**.

1. Selecteer **bestanden kiezen**.

1. Navigeer naar **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Selecteer alle bestanden en klikt u op **Open**.

1. Selecteer **uploaden** moet beginnen met uploaden en selecteer vervolgens **gedaan** zodra het proces voltooid is.

## <a name="run-azure-notebooks"></a>Azure-notitieblokken uitvoeren

Nu dat project is gemaakt, voert de **01-turbofan\_regression.ipynb** notebook.

1. Selecteer in de projectpagina turbofan **01-turbofan\_regression.ipynb**.

    ![Selecteer eerst notitieblok om uit te voeren](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

2. Als u hierom wordt gevraagd, kiest u de Python 3.6 Kernel in het dialoogvenster en selecteer **ingesteld Kernel**.

3. Als de notebook wordt vermeld als **niet wordt vertrouwd**, klikt u op de **niet wordt vertrouwd** widget in de rechterbovenhoek van de notebook. Wanneer het dialoogvenster weergegeven wordt, selecteert u **vertrouwen**.

4. Volg de instructies in het notitieblok.

    * `Ctrl + Enter` wordt uitgevoerd een cel.
    * `Shift + Enter` een cel wordt uitgevoerd en gaat u naar de volgende cel.
    * Wanneer een cel wordt uitgevoerd, heeft deze een sterretje tussen de vierkante haken, zoals **[\*]** . Wanneer deze bewerking voltooid is, het sterretje door een getal vervangen wordt en relevante uitvoer mogelijk worden hieronder weergegeven. Aangezien cellen vaak op het werk van de vorige adressen maken, kan slechts één cel tegelijk uitgevoerd.

5. Wanneer u klaar bent met het uitvoeren de **01-turbofan\_regression.ipynb** laptop, Ga terug naar de pagina van het project.

6. Open **02-turbofan\_implementeren\_model.ipynb** en Herhaal de stappen in deze sectie voor het uitvoeren van het tweede notitieblok.

## <a name="next-steps"></a>Volgende stappen

In dit artikel gebruikt we twee Jupyter-Notebooks die worden uitgevoerd in Azure-notitieblokken gebruiken de gegevens van de turbofan-apparaten met het trainen van een resterende nuttige levensduur (RUL)-classificatie, classificatie opslaan als een model, om een containerinstallatiekopie te maken en te implementeren en testen van de afbeelding als een web-se rvice.

Doorgaan met het volgende artikel voor het maken van een IoT Edge-apparaat.

> [!div class="nextstepaction"]
> [Een IoT Edge-apparaat configureren](tutorial-machine-learning-edge-05-configure-edge-device.md)
