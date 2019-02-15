---
title: Een Machine Learning Studio-werkruimte maken
titleSuffix: Azure Machine Learning Studio
description: Voor het gebruik van Azure Machine Learning Studio, moet u een Machine Learning Studio-werkruimte. Deze werkruimte bevat de hulpprogramma's die u nodig hebt om experimenten te maken, beheren en publiceren.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 12/07/2017
ms.openlocfilehash: bcd5b377f00ad43ff727c581471aad3ac651bdbb
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56270090"
---
# <a name="create-and-share-an-azure-machine-learning-studio-workspace"></a>Maken en delen van een Azure Machine Learning Studio-werkruimte

Voor het gebruik van Azure Machine Learning Studio, moet u een Machine Learning Studio-werkruimte. Deze werkruimte bevat de hulpprogramma's die u nodig hebt om experimenten te maken, beheren en publiceren.

## <a name="create-a-studio-workspace"></a>Een Studio-werkruimte maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/)

    > [!NOTE]
    > Meld u aan en een Studio-werkruimte maken, moet u een beheerder zijn Azure-abonnement. 
    >
    > 

2. Klik op **+ nieuwe**

3. Typ in het zoekvak **Machine Learning Studio-werkruimte** en selecteert u het bijbehorende item. Selecteer klikt u vervolgens **maken** aan de onderkant van de pagina.

4. Voer uw werkruimte:

    - De *Werkruimtenaam* mogelijk maximaal 260 tekens bevatten, niet eindigen op een spatie. De naam kan niet deze tekens niet bevatten: `< > * % & : \ ? + /`
    - De *web service-plan* u kiest u (of maak), samen met de bijbehorende *prijscategorie* u selecteert, wordt gebruikt als u deze werkruimte-webservices implementeren.

    ![Maak een nieuwe Studio-werkruimte](./media/create-workspace/create-new-workspace.png)

5. Klik op **Create**.

> [!NOTE]
> Machine Learning Studio, is afhankelijk van een Azure storage-account dat u opgeeft om op te slaan om gegevens tussentijds bij het uitvoeren van de werkstroom. Nadat de werkruimte is gemaakt, als het opslagaccount is verwijderd, of als de toegangssleutels zijn gewijzigd, functioneert de werkruimte en alle experimenten in deze werkruimte mislukken.
Als u per ongeluk het opslagaccount verwijdert, maakt u het opslagaccount met dezelfde naam in dezelfde regio als het verwijderde opslagaccount opnieuw en de toegangssleutel opnieuw synchroniseren. Als u de toegangssleutel voor een opslagaccount hebt gewijzigd, moet u de toegangssleutel in Azure Portal opnieuw synchroniseren in de werkruimte.

Nadat de werkruimte is geïmplementeerd, kunt u deze kunt openen in Machine Learning Studio.

1. Blader naar de Machine Learning Studio [ https://studio.azureml.net/ ](https://studio.azureml.net/).

2. Selecteer uw werkruimte in de rechterbovenhoek rechterkant te klikken.

    ![Werkruimte selecteren](./media/create-workspace/open-workspace.png)

3. Klik op **mijn experimenten**.

    ![Open experimenten](./media/create-workspace/my-experiments.png)

Zie voor meer informatie over het beheren van de Studio-werkruimte [een Azure Machine Learning Studio-werkruimte beheren](manage-workspace.md).
Als er een probleem opgetreden bij het maken van uw werkruimte, Zie [Troubleshooting guide: Maken en verbinding maken met een Machine Learning Studio-werkruimte](troubleshooting-creating-ml-workspace.md).


## <a name="share-an-azure-machine-learning-studio-workspace"></a>Delen van een Azure Machine Learning Studio-werkruimte
Wanneer een Machine Learning Studio-werkruimte is gemaakt, kunt u gebruikers uitnodigen aan uw werkruimte voor het delen van toegang tot uw werkruimte en alle bijbehorende experimenten, gegevenssets, laptops, enzovoort. U kunt gebruikers toevoegen in een van twee rollen:

* **Gebruiker** -werkruimte van een gebruiker kunt maken, openen, wijzigen en verwijderen van experimenten, gegevenssets, enz. in de werkruimte.
* **De eigenaar van** - eigenaar kunt uitnodigen en gebruikers in de werkruimte, behalve wat een gebruiker verwijderen kunnen doen.

> [!NOTE]
> De administrator-account dat wordt gemaakt van de werkruimte wordt automatisch toegevoegd aan de werkruimte als werkruimte eigenaar. Echter andere beheerders of gebruikers in het desbetreffende abonnement worden niet automatisch toegang verleend tot de werkruimte - moet u ze expliciet uitnodigen.
> 
> 

### <a name="to-share-a-studio-workspace"></a>Voor het delen van een Studio-werkruimte

1. Meld u aan bij Machine Learning Studio [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. Klik in het linkerdeelvenster op **instellingen**

3. Klik op de **gebruikers** tabblad

4. Klik op **meer gebruikers UITNODIGEN** aan de onderkant van de pagina

    ![Studio-instellingen](./media/create-workspace/settings.png)

5. Voer een of meer e-mailadressen. De gebruikers moeten een geldig Microsoft-account of een organisatie-account (van Azure Active Directory).

6. Selecteer of u wilt dat de gebruikers toevoegen als eigenaar of gebruiker.

7. Klik op de **OK** vinkje knop.

Elke gebruiker die u ontvangt een e-mailbericht met instructies voor het aanmelden bij de gedeelde werkruimte.

> [!NOTE]
> Gebruikers kunnen te implementeren of beheren van webservices in deze werkruimte, moeten ze een bijdrager of beheerder zijn in de Azure-abonnement. 



