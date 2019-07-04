---
title: 'Quickstart: Een notitieblok in de cloud uitvoeren'
titleSuffix: Azure Machine Learning service
description: Aan de slag met Azure Machine Learning Service. Een beheerde notebookserver in de cloud gebruiken voor het uitproberen van uw werkruimte.  Uw werkruimte is het fundamentele blok in de cloud die u gebruiken om te experimenten, te trainen en implementeren van machine learning-modellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 68f24d4d019af873a0ca45792a3cbcc3dd3c3c3f
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476046"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Quickstart: Een cloud-gebaseerde notebookserver aan de slag met Azure Machine Learning gebruiken

Geen installatie vereist.  Aan de slag met Azure Machine Learning-service met behulp van een beheerde notebook-server in de cloud. Als u installeren in plaats daarvan de SDK in uw eigen Python-omgeving wilt, Zie [Quick Start: Gebruik uw eigen notebook-server aan de slag met Azure Machine Learning](quickstart-run-local-notebook.md).

Deze quickstart laat zien hoe u kunt de [werkruimte van Azure Machine Learning-service](concept-azure-machine-learning-architecture.md) voor het bijhouden van uw machine learning-experimenten.  U maakt een [notebook VM (Preview)](how-to-configure-environment.md#notebookvm), een veilige, cloud-gebaseerde Azure werkstation dat voorziet in een Jupyter-notebook-server, Jjupyterlab en een volledig voorbereid ML-omgeving. U kunt vervolgens een Python-notebook uitvoeren op deze virtuele machine die zich aanmelden waarden in de werkruimte.

In deze snelstart voert u de volgende acties uit:

* Een werkruimte maken
* Maak een VM-notebook in uw werkruimte.
* Start de Jupyter-webinterface.
* Open een laptop met code voor een schatting van pi en logboeken voor fouten bij elke iteratie.
* Uitvoeren van het notitieblok.
* Bekijk de geregistreerde foutwaarden in uw werkruimte. Dit is een voorbeeld van hoe de werkruimte u kan helpen bij het bijhouden van informatie die in een script wordt gegenereerd.

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="create-a-workspace"></a>Een werkruimte maken

Als u een Azure Machine Learning-service-werkruimte hebt, gaat u naar de [volgende sectie](#create-notebook). Anders wordt er nu een maken.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-notebook"></a>Een VM-notebook maken

 Vanuit uw werkruimte maken van een cloudresource om te beginnen met Jupyter-notebooks. Deze resource biedt u een cloud-gebaseerde platform vooraf geconfigureerd met alles wat die u nodig hebt om uit te voeren van Azure Machine Learning-service.

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).  Als u niet zeker weet hoe u uw werkruimte niet vinden in de portal, Zie hoe u [vinden van uw werkruimte](how-to-manage-workspace.md#view).

1. Selecteer op de pagina van uw werkruimte in de Azure-portal **Notebook VMs** aan de linkerkant.

1. Selecteer **+ nieuw** om een notitieblok VM te maken.

     ![Nieuwe virtuele machine selecteren](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. Geef een naam voor uw virtuele machine. Selecteer vervolgens **Maken**.

    > [!NOTE]
    > De naam van uw laptop VM moet tussen 2 tot 16 tekens. Geldige tekens zijn letters, cijfers en -teken.  De naam moet ook uniek zijn in uw Azure-abonnement.

    ![Een nieuwe virtuele machine maken](media/quickstart-run-cloud-notebook/create-new-workstation.png)

1. Wacht u ongeveer 4 en 5 minuten, totdat de status gewijzigd in **met**.


## <a name="launch-jupyter-web-interface"></a>Jupyter-webinterface starten

Nadat de virtuele machine wordt uitgevoerd, gebruikt u de **Notebook VMs** sectie om te starten van de Jupyter-webinterface.

1. Selecteer **Jupyter** in de **URI** kolom voor uw virtuele machine.  

    ![Start de Jupyter-notebook-server](./media/quickstart-run-cloud-notebook/start-server.png)

    De koppeling wordt de notebook-server wordt gestart en wordt de Jupyter-notebook webpagina in een nieuw browsertabblad geopend.  Deze koppeling werkt alleen voor de persoon die de virtuele machine maakt.

1. De bovenste mapnaam is op de webpagina Jupyter-notebook uw gebruikersnaam.  Selecteer deze map.

    > [!TIP]
    > Deze map bevindt zich op de [opslagcontainer](concept-workspace.md#resources) in uw werkruimte, in plaats van in de notebook virtuele machine zelf.  U kunt de notebook VM verwijderen en toch al het werk blijven.  Wanneer u een nieuwe notebook VM later maakt, wordt deze dezelfde map laden.

1. De mapnaam voorbeelden bevat een versienummer, bijvoorbeeld **voorbeelden 1.0.33.1**.  Selecteer de map met voorbeelden.

1. Selecteer de **snelstartgids** map.

## <a name="run-the-notebook"></a>Het notitieblok uitvoeren

Voer een laptop die u maakt een schatting van pi en de fout vastgelegd in uw werkruimte.

1. Selecteer **01.run experiment.ipynb** om het notebook te openen.

1. Als u een waarschuwing 'Kernel niet gevonden' ziet, selecteert u de kernel **Python 3.6 - AzureML** (ongeveer halverwege om in de lijst) en stel de kernel.

1. Klik in de eerste codecel en selecteer **uitvoeren**.

    > [!NOTE]
    > Code cellen hebben vierkante haken voordat ze. Als de vierkante haken leeg zijn ( __[]__ ), de code niet is uitgevoerd. Terwijl de code wordt uitgevoerd, ziet u een sterretje ( __[*]__ ). Nadat de code is voltooid, wordt een getal **[1]** wordt weergegeven.  Het aantal ziet u de volgorde waarin de cellen is uitgevoerd.
    >
    > Gebruik **Shift + Enter** als een snelkoppeling naar een cel worden uitgevoerd.

    ![De eerste codecel uitvoert](media/quickstart-run-cloud-notebook/cell1.png)

1. De tweede codecel uitvoert. Als er instructies om te verifiëren, kopieert u de code en volg de koppeling aan te melden. Als u zich aanmeldt, wordt deze instelling in uw browser onthouden.  

    ![Verifiëren](media/quickstart-run-cloud-notebook/authenticate.png)

1. Wanneer u klaar bent, het aantal cellen __[2]__ wordt weergegeven.  Als u zich aanmeldt, ziet u een statusbericht verificatie is geslaagd.   Als u niet aanmelden, geen uitvoer voor deze cel niet zichtbaar is voor u, alleen het nummer wordt weergegeven om weer te geven dat de cel is uitgevoerd.

    ![Geslaagd](media/quickstart-run-cloud-notebook/success.png)

1. De rest van de cellen met code worden uitgevoerd.  Omdat elke cel is voltooid, ziet u het aantal cellen worden weergegeven. Alleen de laatste cel eventuele andere uitvoer wordt weergegeven.  

    In de grootste codecel, ziet u `run.log` gebruikt op meerdere plaatsen. Elke `run.log` wordt de waarde toegevoegd aan uw werkruimte.

## <a name="view-logged-values"></a>Logboekwaarden weergeven

1. De uitvoer van de cel `run` bevat een koppeling terug naar de Microsoft Azure-portal om de resultaten van het experiment in uw werkruimte weer te geven.

    ![Experimenten bekijken](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Klik op de **koppelen aan Azure-portal** om informatie over het uitvoeren in uw werkruimte weer te geven.  Met deze koppeling wordt uw werkruimte in de Microsoft Azure-portal geopend.

1. De grafieken van vastgelegde waarden die u ziet, zijn automatisch in de werkruimte gemaakt. Wanneer u meerdere waarden met dezelfde naamparameter vastlegt, wordt automatisch een grafiek voor u gegenereerd. Hier volgt een voorbeeld:

   ![Geschiedenis weergeven](./media/quickstart-run-cloud-notebook/web-results.png)

Omdat de code voor het geschatte pi gebruikmaakt van willekeurige waarden, uw grafieken kunnen er anders uitzien.  

## <a name="clean-up-resources"></a>Resources opschonen

### <a name="stop-the-notebook-vm"></a>De notebook VM stoppen

Stop de notebook VM wanneer u deze niet worden gebruikt om kosten te beperken.  

1. Selecteer in uw werkruimte **Notebook VMs**.

   ![Stop de virtuele machine-server](./media/quickstart-run-cloud-notebook/stop-server.png)

1. Selecteer de VM in de lijst.

1. Selecteer **stoppen**.

1. Wanneer u klaar bent om de server opnieuw te gebruiken, selecteert u **Start**.

### <a name="delete-everything"></a>Alles verwijderen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

U kunt de resourcegroep ook bewaren en slechts één werkruimte verwijderen. Bekijk de eigenschappen van de werkruimte en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start, moet u deze taken uitgevoerd:

* Een werkruimte maken
* Een VM-notebook maken.
* Start de Jupyter-webinterface.
* Open een laptop met code voor een schatting van pi en logboeken voor fouten bij elke iteratie.
* Uitvoeren van het notitieblok.
* Bekijk de geregistreerde foutwaarden in uw werkruimte.  Dit is een voorbeeld van hoe de werkruimte u kan helpen bij het bijhouden van informatie die in een script wordt gegenereerd. 

Op de webpagina Jupyter-Notebook in de **snelstartgids** map openen en voer de **02.deploy-web-service.ipynb** -notebook geïnstrueerd om informatie over het implementeren van een webservice.

Ook op de webpagina Jupyter-Notebook Blader door de andere notebooks onder in de map met voorbeelden voor meer informatie over Azure Machine Learning-service.

Voor een diepgaande werkstroomervaring volgt u de zelfstudies over Machine Learning om een ​​model te trainen en te implementeren:  

> [!div class="nextstepaction"]
> [Zelfstudie: Een model trainen voor de classificatie van afbeeldingen](tutorial-train-models-with-aml.md)
