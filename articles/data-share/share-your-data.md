---
title: 'Zelfstudie: gegevens delen met klanten en partners met behulp van Azure Data Share-Preview'
description: 'Zelfstudie: gegevens delen met klanten en partners met behulp van Azure Data Share-Preview'
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 01888f3656765b922c1b646e7ca8e07d81e799f3
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838430"
---
# <a name="tutorial-share-your-data-using-azure-data-share-preview"></a>Zelfstudie: Deel uw gegevens met behulp van Azure Data Share-Preview

In deze zelfstudie leert u hoe u een nieuwe Azure het delen van gegevens instellen en begin met het delen van uw gegevens met klanten en partners buiten uw organisatie Azure. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een Gegevensshare.
> * Gegevenssets toevoegen aan het delen van uw gegevens.
> * Schakel een synchronisatieplanning voor het delen van uw gegevens. 
> * Geadresseerden toevoegen aan het delen van uw gegevens. 

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Een Azure Storage-account: Als u dit niet al hebt, kunt u een [Azure Storage-account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* Machtiging roltoewijzing toevoegen aan de storage-account, deze is beschikbaar in de *Microsoft.Authorization/role toewijzingen schrijftijd* machtiging. Deze machtiging bestaat in de rol van eigenaar. 
* Uw ontvangers Azure-aanmelding e-mailadres (met behulp van hun e-mailadres werkt niet).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-data-share-account"></a>Een gegevens-Share-Account maken

Maak een resource delen van de Azure-gegevens in een Azure-resourcegroep.

1. Selecteer de knop **Een resource maken** (+) in de linkerbovenhoek van de portal.

1. Zoeken naar *het delen van gegevens*.

1. Selecteer het delen van gegevens (preview) en selecteer **maken**.

1. Vul de basisgegevens van uw resource Azure het delen van gegevens met de volgende informatie. 

     **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | Name | *datashareacount* | Geef een naam voor uw gegevens delen van een account. |
    | Subscription | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken voor uw gegevens delen van een account.|
    | Resource group | *test-resource-group* | Gebruik een bestaande resourcegroep of maak een nieuwe. |
    | Location | *VS-Oost 2* | Selecteer een regio voor uw gegevens delen van een account.
    | | |

1. Selecteer **maken** voor het inrichten van uw gegevens delen van een account. Inrichten van een nieuwe gegevens delen van een account doorgaans duurt het ongeveer 2 minuten of minder. 

1. Nadat de implementatie is voltooid, selecteert u **Ga naar resource**.

## <a name="create-a-data-share"></a>Maak een Gegevensshare

1. Navigeer naar de pagina overzicht van Data-Share.

    ![Uw gegevens delen](./media/share-receive-data.png "uw gegevens delen") 

1. Selecteer **Start met het delen van uw gegevens**.

1. Selecteer **Maken**.   

1. Vul de details in voor het delen van uw gegevens. Geef een naam, beschrijving van share-inhoud en de gebruiksvoorwaarden (optioneel). 

    ![EnterShareDetails](./media/enter-share-details.png "sharedetails invoeren") 

1. Selecteer **gaan**

1. Selecteer om gegevenssets toe aan het delen van uw gegevens **gegevenssets toevoegen**. 

    ![Datasets](./media/datasets.png "Datasets")

1. Selecteer het gegevenssettype die u wilt toevoegen. 

    ![AddDatasets](./media/add-datasets.png "gegevenssets toevoegen")    

1. Ga naar het object dat u wilt delen en selecteer 'Gegevenssets toevoegen'. 

    ![SelectDatasets](./media/select-datasets.png "gegevenssets selecteren")    

1. Voer in het e-mailadressen van de consumenten van uw gegevens door te selecteren op het tabblad ontvangers ' + ontvanger toevoegen '. 

    ![AddRecipients](./media/add-recipient.png "geadresseerden toevoegen") 

1. Selecteer **gaan**

1. Als u uw gegevensverbruiker wilt kunnen incrementele updates van uw gegevens ophalen, moet u de momentopname-planning inschakelt. 

    ![EnableSnapshots](./media/enable-snapshots.png "momentopnamen inschakelen") 

1. Selecteer een start-interval voor tijd en het terugkeerpatroon. 

1. Selecteer **gaan**

1. In de beoordeling + tab maken, Controleer uw pakketinhoud, de instellingen, de ontvangers en het synchronisatie-instellingen. Selecteer **Maken**

Het delen van uw Azure-gegevens is nu gemaakt en de ontvanger van het delen van uw gegevens is nu klaar om uw uitnodiging te accepteren. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt vernomen u dat het delen van een Azure-gegevens maken en geadresseerden uitnodigen. Voor meer informatie over hoe een Gegevensgebruiker kunnen accepteren en het delen van een gegevens ontvangen, blijven de [accepteren en gegevens ontvangen](subscribe-to-data-share.md) zelfstudie. 