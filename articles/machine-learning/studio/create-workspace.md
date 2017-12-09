---
title: Een Machine Learning Studio-werkruimte maken | Microsoft Docs
description: Het maken van een werkruimte voor Azure Machine Learning Studio
services: machine-learning
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 12/07/2017
ms.author: garye
ms.reviewer: garyericson, jasonwhowell, MicrosoftDocs/mlreview
ms.openlocfilehash: 7aad885b1643ddeb25e5d00a151005b8444315fa
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Een Azure Machine Learning-werkruimte maken en delen
Dit menukoppelingen naar onderwerpen waarin wordt beschreven hoe u de verschillende gegevens wetenschappelijke omgevingen gebruikt door de Cortana Analytics proces (CAP's).

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

U moet een Machine Learning-werkruimte hebt voor het gebruik van Azure Machine Learning Studio. Deze werkruimte bevat de hulpprogramma's die u wilt maken, beheren en experimenten publiceren.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

### <a name="to-create-a-workspace"></a>Om een werkruimte te maken
1. Aanmelden bij de [Azure-portal](https://portal.azure.com/)

    > [!NOTE]
    > Als u wilt aanmelden en een werkruimte maken, moet u Azure-abonnementbeheerder. 
    >
    > 

2. Klik op **+ nieuw**

3. Typ in het zoekvak **Machine Learning Studio-werkruimte** en selecteer het overeenkomende item. Selecteer Klik **maken** aan de onderkant van de pagina.

4. Voer de werkruimtegegevens van uw:

    - De *Werkruimtenaam* mogelijk maximaal 260 tekens lang zijn, niet eindigen met een spatie. De naam kan niet deze tekens niet bevatten:`< > * % & : \ ? + /`
    - De *web service-abonnement* u kiest (of maken), samen met de bijbehorende *prijscategorie* u selecteert, wordt gebruikt als u web-services uit deze werkruimte implementeert.

    ![Maak een nieuwe werkruimte](./media/create-workspace/create-new-workspace.png)

5. Klik op **Create**.

Zodra de werkruimte is geïmplementeerd, kunt u deze kunt openen in Machine Learning Studio.

1. Blader naar Machine Learning Studio [https://studio.azureml.net/](https://studio.azureml.net/).

2. Selecteer uw werkruimte in de bovenste--rechterhoek.

    ![Werkruimte selecteren](./media/create-workspace/open-workspace.png)

3. Klik op **mijn experimenten**.

    ![Open experimenten](./media/create-workspace/my-experiments.png)

Zie voor meer informatie over het beheren van uw werkruimte [beheren van een Azure Machine Learning-werkruimte](manage-workspace.md).
Als er een probleem bij het maken van uw werkruimte optreden, Raadpleeg [Troubleshooting guide: maken en verbinding maken met een Machine Learning-werkruimte](troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Delen van een Azure Machine Learning-werkruimte
Eenmaal een Machine Learning-werkruimte is gemaakt, kunt u gebruikers uitnodigen naar de werkruimte voor het delen van toegang tot uw werkruimte en alle bijbehorende experimenten, gegevenssets, laptops, enzovoort. U kunt gebruikers toevoegen in een van twee functies:

* **Gebruiker** -werkruimte van een gebruiker kunt maken, openen, wijzigen en verwijderen van experimenten, gegevenssets, enz. in de werkruimte.
* **Eigenaar** - eigenaar kunt uitnodigen en gebruikers in de werkruimte naast wat een gebruiker verwijderen kunnen doen.

> [!NOTE]
> De administrator-account dat wordt gemaakt van de werkruimte wordt automatisch toegevoegd aan de werkruimte als eigenaar-werkruimte. Echter andere beheerders of gebruikers in het desbetreffende abonnement worden niet automatisch toegang tot de werkruimte - moet u ze expliciet uitnodigen.
> 
> 

### <a name="to-share-a-workspace"></a>Voor het delen van een werkruimte

1. Aanmelden bij Machine Learning Studio [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. Klik in het linkerdeelvenster op **instellingen**

3. Klik op de **gebruikers** tabblad

4. Klik op **meer gebruikers UITNODIGEN** aan de onderkant van de pagina

    ![Studio-instellingen](./media/create-workspace/settings.png)

5. Voer een of meer e-mailadressen. De gebruikers moeten een geldig Microsoft-account of een organisatie-account (van Azure Active Directory).

6. Selecteer of u wilt de gebruikers toevoegen als eigenaar of gebruiker.

7. Klik op de **OK** vinkje knop.

Elke gebruiker die u toevoegt, ontvangt een e-mail met instructies voor het aanmelden bij de gedeelde werkruimte.

> [!NOTE]
> Voor gebruikers om te implementeren of beheren van webservices in deze werkruimte, moeten ze een bijdrager of een beheerder in de Azure-abonnement zijn. 



