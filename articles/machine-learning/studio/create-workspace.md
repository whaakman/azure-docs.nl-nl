---
title: Een werkruimte - Azure Machine Learning Studio maken | Microsoft Docs
description: Voor het gebruik van Azure Machine Learning Studio, moet u een Machine Learning Studio-werkruimte. Deze werkruimte bevat de hulpprogramma's die u nodig hebt om experimenten te maken, beheren en publiceren.
services: machine-learning
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: e6835afd66d054a79c422a35839d64ae89031de5
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268254"
---
# <a name="create-and-share-an-azure-machine-learning-studio-workspace"></a>Maken en delen van een Azure Machine Learning Studio-werkruimte

Voor het gebruik van Azure Machine Learning Studio, moet u een Machine Learning Studio-werkruimte. Deze werkruimte bevat de hulpprogramma's die u nodig hebt om experimenten te maken, beheren en publiceren.



### <a name="to-create-a-workspace"></a>Een werkruimte maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/)

    > [!NOTE]
    > Meld u aan en een werkruimte maken, moet u een beheerder zijn Azure-abonnement. 
    >
    > 

2. Klik op **+ nieuwe**

3. Typ in het zoekvak **Machine Learning Studio-werkruimte** en selecteert u het bijbehorende item. Selecteer klikt u vervolgens **maken** aan de onderkant van de pagina.

4. Voer uw werkruimte:

    - De *Werkruimtenaam* mogelijk maximaal 260 tekens bevatten, niet eindigen op een spatie. De naam kan niet deze tekens niet bevatten: `< > * % & : \ ? + /`
    - De *web service-plan* u kiest u (of maak), samen met de bijbehorende *prijscategorie* u selecteert, wordt gebruikt als u deze werkruimte-webservices implementeren.

    ![Een nieuwe werkruimte maken](./media/create-workspace/create-new-workspace.png)

5. Klik op **Create**.

Nadat de werkruimte is geïmplementeerd, kunt u deze kunt openen in Machine Learning Studio.

1. Blader naar de Machine Learning Studio [ https://studio.azureml.net/ ](https://studio.azureml.net/).

2. Selecteer uw werkruimte in de rechterbovenhoek rechterkant te klikken.

    ![Werkruimte selecteren](./media/create-workspace/open-workspace.png)

3. Klik op **mijn experimenten**.

    ![Open experimenten](./media/create-workspace/my-experiments.png)

Zie voor meer informatie over het beheren van uw werkruimte [een Azure Machine Learning-werkruimte beheren](manage-workspace.md).
Als er een probleem opgetreden bij het maken van uw werkruimte, Zie [Troubleshooting guide: Maken en verbinding maken met een Machine Learning-werkruimte](troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Delen van een Azure Machine Learning-werkruimte
Wanneer een Machine Learning-werkruimte is gemaakt, kunt u gebruikers uitnodigen aan uw werkruimte voor het delen van toegang tot uw werkruimte en alle bijbehorende experimenten, gegevenssets, laptops, enzovoort. U kunt gebruikers toevoegen in een van twee rollen:

* **Gebruiker** -werkruimte van een gebruiker kunt maken, openen, wijzigen en verwijderen van experimenten, gegevenssets, enz. in de werkruimte.
* **De eigenaar van** - eigenaar kunt uitnodigen en gebruikers in de werkruimte, behalve wat een gebruiker verwijderen kunnen doen.

> [!NOTE]
> De administrator-account dat wordt gemaakt van de werkruimte wordt automatisch toegevoegd aan de werkruimte als werkruimte eigenaar. Echter andere beheerders of gebruikers in het desbetreffende abonnement worden niet automatisch toegang verleend tot de werkruimte - moet u ze expliciet uitnodigen.
> 
> 

### <a name="to-share-a-workspace"></a>Voor het delen van een werkruimte

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



