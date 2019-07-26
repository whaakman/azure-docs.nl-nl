---
title: 'Quickstart: Een notitie blok uitvoeren in de Cloud'
titleSuffix: Azure Machine Learning service
description: In deze zelf studie leert u hoe u aan de slag kunt met de Azure Machine Learning-service en hoe u een beheerde notebook server gebruikt in de Cloud.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 1124bb17abb9340b442d8d6075551ffe0dc681f7
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68371053"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Quickstart: Een op de cloud gebaseerde Notebook server gebruiken om aan de slag te gaan met Azure Machine Learning

In deze Quick Start leert u hoe u de Azure Machine Learning-service kunt gebruiken met behulp van een beheerde notebook server in de Cloud. Er is geen installatie vereist. Als u in plaats daarvan de SDK wilt installeren in uw eigen python-omgeving [, raadpleegt u Quick Start: Gebruik uw eigen notebook server om aan de slag te](quickstart-run-local-notebook.md)gaan met Azure machine learning.

In deze Quick start ziet u hoe u de [Azure machine learning service-werk ruimte](concept-azure-machine-learning-architecture.md) kunt gebruiken om uw machine learning (ml) experimenten bij te houden. U doet dit door een [virtuele machine voor een notebook te maken (preview)](how-to-configure-environment.md#notebookvm): een veilig Azure-werk station in de cloud dat een Jupyter notebook server, jjupyterlab en een volledig voor bereide ml-omgeving biedt. Vervolgens voert u een python-notebook uit op deze virtuele machine (VM) die waarden registreert in de werk ruimte.

U kunt dit doen door de volgende acties uit te voeren:

* Een werk ruimte maken.
* Maak een notebook-VM in uw werk ruimte.
* Open de Jupyter-webinterface.
* Open een notitie blok dat code bevat om pi te ramen en fouten bij elke herhaling te melden.
* Voer het notitie blok uit.
* Bekijk de vastgelegde fout waarden in uw werk ruimte. In het volgende voor beeld ziet u hoe u met de werk ruimte gegevens kunt bijhouden die in een script worden gegenereerd.

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer de [gratis of betaalde versie van de Azure machine learning service](https://aka.ms/AMLFree).

## <a name="create-a-workspace"></a>Een werkruimte maken

Als u een werk ruimte van Azure Machine Learning service hebt, gaat u naar de [volgende sectie](#create-notebook). Anders maakt u er nu een.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-notebook"></a>Een VM voor een notebook maken

 Maak in uw werk ruimte een Cloud resource om te beginnen met het gebruik van Jupyter-notebooks. Deze resource is een op de cloud gebaseerd platform dat vooraf is geconfigureerd met alles wat u nodig hebt om de Azure Machine Learning-service uit te voeren.

1. Open uw werk ruimte in de [Azure Portal](https://portal.azure.com/). Als u niet zeker weet hoe u uw werk ruimte in de portal kunt vinden, raadpleegt u [uw werk ruimte weer geven](how-to-manage-workspace.md#view).

1. Selecteer op de pagina werk ruimte de optie **laptop-vm's** aan de linkerkant.

1. Selecteer **+ Nieuw** om een VM van een notebook te maken.  

     ![Nieuwe VM selecteren](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. Geef een naam op voor de virtuele machine. Selecteer vervolgens **Maken**.

    > [!NOTE]
    > De naam van de VM van uw notebook moet tussen 2 en 16 tekens lang zijn. Letters, cijfers en afbreek streepjes zijn geldige tekens. De naam moet uniek zijn binnen uw Azure-abonnement.

    ![Een nieuwe virtuele machine maken](media/quickstart-run-cloud-notebook/create-new-workstation.png)

1. Wacht ongeveer 4 tot 5 minuten totdat de status wordt gewijzigd in **wordt uitgevoerd**.


## <a name="open-the-jupyter-web-interface"></a>Open de Jupyter-webinterface

Nadat de virtuele machine is uitgevoerd, gebruikt u de sectie **virtuele notebook** -apps om de Jupyter-webinterface te openen.

1. Selecteer **Jupyter** in de kolom **URI** voor uw VM.  

    ![De Jupyter-notebook server starten](./media/quickstart-run-cloud-notebook/start-server.png)

    Met de koppeling wordt uw notebook server gestart en wordt de Jupyter-notebook-webpagina geopend in een nieuw browser tabblad.  Deze koppeling werkt alleen voor de persoon die de virtuele machine heeft gemaakt.  Elke gebruiker van de werk ruimte moet een eigen VM maken.

1. Op de Jupyter notebook-webpagina is de naam van de bovenste map uw gebruikers naam. Selecteer deze map.

    > [!TIP]
    > Deze map bevindt zich in de [opslag container](concept-workspace.md#resources) in uw werk ruimte, in plaats van op de VM van de notebook zelf.  U kunt de VM van het notitie blok verwijderen en toch al uw werk behouden.  Wanneer u later een nieuwe VM voor een notitie blok maakt, wordt dezelfde map geladen.  Als u uw werk ruimte deelt met anderen, wordt uw map weer geven en ziet u de mappen. 

1. De naam van de map met voor beelden bevat een versie nummer (bijvoorbeeld voor**beelden-1.0.33.1**). Selecteer de map met voor beelden.

1. Selecteer de map **Quick** start.

## <a name="run-the-notebook"></a>Het notitieblok uitvoeren

Voer een notitie blok uit met een schatting van pi en registreert de fout in uw werk ruimte.

1. Selecteer **01.run experiment.ipynb** om het notebook te openen.

1. Als de waarschuwing kernel niet gevonden wordt weer gegeven, selecteert u de kernel **Python 3,6-AzureML** (ongeveer halverwege de lijst) en stelt u de kernel in.

1. Selecteer de eerste code cel en selecteer vervolgens **uitvoeren**.

    > [!NOTE]
    > Code cellen staan vóór deze haken. Als de vier Kante haken leeg zijn ( __[]__ ), is de code niet uitgevoerd. Terwijl de code wordt uitgevoerd, wordt een sterretje ( __[*]__ ) weer gegeven. Nadat de code is voltooid, wordt het nummer **[1]** weer gegeven.  Het nummer vertelt u de volg orde waarin de cellen worden uitgevoerd.
    >
    > Gebruik **SHIFT + ENTER** als snelkoppeling om een cel uit te voeren.

    ![De eerste code-cel uitvoeren](media/quickstart-run-cloud-notebook/cell1.png)

1. Voer de tweede code-cel uit. Als u instructies voor verificatie ziet, kopieert u de code en volgt u de koppeling om u aan te melden. Nadat u zich hebt aangemeld, wordt deze instelling onthouden in uw browser.  

    ![Authenticeren](media/quickstart-run-cloud-notebook/authenticate.png)

1. Wanneer het uitvoeren van de code cel is geslaagd, wordt het cel nummer __[2]__ weer gegeven. Als u zich hebt aangemeld, ziet u een geslaagd verificatie status bericht.   Als u zich niet hebt aangemeld, ziet u geen uitvoer voor deze cel. U ziet alleen het nummer dat wordt weer gegeven om aan te geven dat de cel is uitgevoerd.

    ![Geslaagd](media/quickstart-run-cloud-notebook/success.png)

1. Voer de rest van de code cellen uit. Wanneer de uitvoering van elke cel is voltooid, wordt het bijbehorende cel nummer weer gegeven. In alleen de laatste cel wordt een andere uitvoer weer gegeven.  

    In de cel `run.log` met de hoogste code wordt weer gegeven op meerdere locaties. Elke `run.log` waarde wordt toegevoegd aan uw werk ruimte.

## <a name="view-logged-values"></a>Logboekwaarden weergeven

1. De uitvoer van de `run` cel bevat een koppeling terug naar de Azure Portal, waar u de resultaten van het experiment in uw werk ruimte kunt bekijken.

    ![Experimenten bekijken](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Selecteer **koppeling naar Azure Portal** om informatie weer te geven over de uitvoering in uw werk ruimte. Met deze koppeling wordt uw werkruimte in de Microsoft Azure-portal geopend.

1. De grafieken van vastgelegde waarden die u ziet, zijn automatisch in de werkruimte gemaakt. Wanneer u meerdere waarden met dezelfde naamparameter vastlegt, wordt automatisch een grafiek voor u gegenereerd. Hier volgt een voorbeeld:

   ![Geschiedenis weergeven](./media/quickstart-run-cloud-notebook/web-results.png)

Omdat de code om pi te benaderen, wille keurige waarden gebruikt, kunnen uw waarnemings punten er anders uitzien.  

## <a name="clean-up-resources"></a>Resources opschonen

### <a name="stop-the-notebook-vm"></a>De VM van het notebook stoppen

Stop de VM van het notebook wanneer u deze niet gebruikt om de kosten te verlagen.  

1. Selecteer in uw werk ruimte de optie **laptop vm's**.

   ![De VM-server stoppen](./media/quickstart-run-cloud-notebook/stop-server.png)

1. Selecteer de VM in de lijst.

1. Selecteer **stoppen**.

1. Wanneer u klaar bent om de server opnieuw te gebruiken, selecteert u **starten**.

### <a name="delete-everything"></a>Alles verwijderen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

U kunt de resourcegroep ook bewaren en slechts één werkruimte verwijderen. Bekijk de eigenschappen van de werkruimte en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Nadat u deze taken hebt voltooid, gaat u naar de webpagina van Jupyter Notebook. Open in de map **Quick** start de **02. Deploy-web-service. ipynb-** notebook om te leren hoe u een webservice implementeert.

Als u andere Python-pakketten wilt installeren in uw Jupyter-omgeving, gebruikt u deze code binnen een notitie blok:

```
!source activate py36 && pip install <packagename>
```

Blader op de Jupyter Notebook webpagina ook door andere notitie blokken in de map met voor beelden voor meer informatie over de Azure Machine Learning-service.

Voor een diepgaande werkstroomervaring volgt u de zelfstudies over Machine Learning om een ​​model te trainen en te implementeren:  

> [!div class="nextstepaction"]
> [Zelfstudie: Een model trainen voor de classificatie van afbeeldingen](tutorial-train-models-with-aml.md)
