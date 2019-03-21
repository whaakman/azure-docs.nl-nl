---
title: Azure Data Box Edge gebruikers beheren | Microsoft Docs
description: Beschrijft hoe u Azure portal gebruiken voor het beheren van gebruikers voor uw Azure Data Box-edge-apparaten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/11/2019
ms.author: alkohli
ms.openlocfilehash: 5642f0c1bcb7d7d8dcb17885aa6d3e9197889685
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57888416"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-edge"></a>De Azure-portal gebruiken voor het beheren van gebruikers voor uw Azure Data Box-edge-apparaten

In dit artikel wordt beschreven hoe u gebruikers voor uw Azure Data Box-edge-apparaten beheren. U kunt beheren van de rand van het Azure Data Box via Azure portal of via de lokale webgebruikersinterface. Gebruik Azure Portal om gebruikers toe te voegen, te wijzigen of te verwijderen.

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een gebruiker toevoegen
> * Gebruiker wijzigen
> * Een gebruiker verwijderen

## <a name="about-users"></a>Over gebruikers

Gebruikers kunnen het recht alleen-lezen of volledige bevoegdheid hebben. Zoals de naam al aangeeft, kunnen gebruikers met het recht alleen-lezen de sharegegevens alleen weergeven. Gebruikers met volledige bevoegdheid kunnen sharegegevens lezen, schrijven, wijzigen of verwijderen.

 - **Gebruiker met volledige bevoegdheden** - een lokale gebruiker met volledige toegang.
 - **Alleen-lezengebruiker** - een lokale gebruiker met alleen-lezentoegang. Deze gebruikers zijn gekoppeld aan shares waarmee alleen-lezenbewerkingen mogelijk zijn.

De gebruikersmachtigingen worden eerst gedefinieerd wanneer de gebruiker wordt gemaakt tijdens het maken van de share. Wanneer de machtigingen voor een gebruiker zijn gedefinieerd, kunnen deze worden gewijzigd via Windows Verkenner. 


## <a name="add-a-user"></a>Een gebruiker toevoegen

Voer in Azure Portal de volgende stappen uit om een gebruiker toe te voegen.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **overzicht > gebruikers**. Selecteer **+ gebruiker toevoegen** op de opdrachtbalk.

    ![Selecteer een gebruiker toevoegen](media/data-box-edge-manage-users/add-user-1.png)

2. Geef de gebruikersnaam en het wachtwoord op voor de gebruiker die u wilt toevoegen. Bevestig het wachtwoord en selecteer **toevoegen**.

    ![Gebruikersnaam en wachtwoord opgeven](media/data-box-edge-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Deze gebruikers zijn gereserveerd door het systeem en mag niet worden gebruikt: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Een melding wordt weergegeven wanneer het maken van de gebruiker wordt gestart en is voltooid. Nadat de gebruiker is gemaakt, vanuit de opdrachtbalk en selecteer **vernieuwen** om de bijgewerkte lijst met gebruikers weer te geven.


## <a name="modify-user"></a>Gebruiker wijzigen

U kunt het wachtwoord dat is gekoppeld aan een gebruiker, wijzigen wanneer de gebruiker is gemaakt. Selecteer in de lijst met gebruikers. Typ en Bevestig het nieuwe wachtwoord. Sla de wijzigingen op.
 
![Gebruiker wijzigen](media/data-box-edge-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Een gebruiker verwijderen

Voer in Azure Portal de volgende stappen uit om een gebruiker te verwijderen.


1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **overzicht > gebruikers**.

    ![Selecteer gebruiker verwijderen](media/data-box-edge-manage-users/delete-user-1.png)

2. Selecteer een gebruiker in de lijst met gebruikers en selecteer vervolgens **verwijderen**.  

   ![Verwijderen selecteren](media/data-box-edge-manage-users/delete-user-2.png)

3. Bevestig de verwijdering als u daarom wordt gevraagd. 

   ![De verwijdering bevestigen](media/data-box-edge-manage-users/delete-user-3.png)

De lijst met gebruikers wordt bijgewerkt en de verwijderde gebruiker wordt niet meer weergegeven.

![Bijgewerkte lijst van gebruikers](media/data-box-edge-manage-users/delete-user-4.png)


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Bandbreedte beheren](data-box-edge-manage-bandwidth-schedules.md).
