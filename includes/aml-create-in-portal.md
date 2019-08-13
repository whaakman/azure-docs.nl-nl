---
title: bestand opnemen
description: bestand opnemen
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/31/2019
ms.openlocfilehash: 0a497ce506e1aa3b0f9afc47bf1ab8382c9c0405
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68951420"
---
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) met behulp van de referenties voor het Azure-abonnement dat u gebruikt. 

1. Selecteer in de linkerbovenhoek van Azure Portal **een resource maken**.

1. Gebruik de zoek balk om **machine learning service-werk ruimte**te vinden.

1. Selecteer **machine learning service-werk ruimte**.

1. Selecteer in het deel venster **service-werk ruimte** de optie **maken** om te beginnen.

1. Configureer uw nieuwe werk ruimte door de naam, het abonnement, de resource groep en de locatie van de werk ruimte op te geven.

    ![Werkruimte maken](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Veld|Description 
   ---|---
   Naam van de werkruimte |Voer een unieke naam in die uw werk ruimte identificeert. In dit voor beeld gebruiken we **docs-WS**. De namen moeten uniek zijn in de resource groep. Gebruik een naam die gemakkelijk kan worden ingetrokken en om onderscheid te maken tussen werk ruimten die door anderen zijn gemaakt.  
   Subscription |Selecteer het Azure-abonnement dat u wilt gebruiken.
   Resource group | Gebruik een bestaande resource groep in uw abonnement of voer een naam in om een nieuwe resource groep te maken. Een resource groep bevat gerelateerde resources voor een Azure-oplossing. In dit voor beeld gebruiken we **docs-AML**. 
   Location | Selecteer de locatie die het dichtst bij uw gebruikers ligt en de gegevens bronnen om uw werk ruimte te maken.

1. Nadat u klaar bent met het configureren van de werk ruimte, selecteert u **maken**. 

   Het kan even duren voordat de werk ruimte is gemaakt.

   Wanneer het proces is voltooid, wordt een bericht over een geslaagde implementatie weer gegeven. Het is ook aanwezig in de sectie meldingen. Als u de nieuwe werk ruimte wilt weer geven, selecteert u **Ga naar resource**.

   ![Status van het maken van de werk ruimte](./media/aml-create-in-portal/notifications.png)

1. Als uw code naar deze werk ruimte verwijst vanuit uw lokale omgeving, selecteert u **downloaden van config. json** in het gedeelte **overzicht** van de werk ruimte.  

   ![Config.json downloaden](./media/aml-create-in-portal/configure.png)

    Plaats dit JSON-bestand in de mapstructuur die uw python-scripts of Jupyter-notebooks bevat. Deze kan zich in dezelfde map bevindt, in een submap met de naam *. azureml*of in een bovenliggende map.

   Als u een [notebook-VM](../articles/machine-learning/service/tutorial-1st-experiment-sdk-setup.md#azure) wilt gebruiken, hoeft u dit bestand niet te downloaden, maar het wordt toegevoegd aan de juiste map op de virtuele machine.
