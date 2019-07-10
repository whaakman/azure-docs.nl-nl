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
ms.date: 05/21/2019
ms.openlocfilehash: a33eb98525ea857ee52ad2fffa5937207504909d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720514"
---
1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met behulp van de referenties voor het Azure-abonnement u gebruikt. 

   ![Azure Portal](./media/aml-create-in-portal/portal-dashboard-05-2019.png)

1. Selecteer in de linkerbovenhoek van de portal **een resource maken**.

   ![Een resource maken in Azure Portal](./media/aml-create-in-portal/portal-create-a-resource-07-2019.png)

1. Voer in de zoekbalk typt, **Machine Learning**. Selecteer de **werkruimte van Machine Learning-service** zoekresultaat.

   ![Zoeken naar een werkruimte](./media/aml-create-in-portal/allservices-search.png)

1. In de **ML-werkruimte in service** venster **maken** om te beginnen.

    ![Knop maken](./media/aml-create-in-portal/portal-create-button.png)

1. In de **ML-werkruimte in service** in het deelvenster voor het configureren van uw werkruimte.

   Veld|Description
   ---|---
   Naam van de werkruimte |Voer een unieke naam ter identificatie van uw werkruimte. In dit voorbeeld gebruiken we **docs-ws**. Namen moeten uniek zijn in de resourcegroep. Gebruik een naam die gemakkelijk te trekken en onderscheiden van werkruimten die door anderen zijn gemaakt.  
   Subscription |Selecteer het Azure-abonnement dat u wilt gebruiken.
   Resource group | Een bestaande resourcegroep gebruiken in uw abonnement, of voer een naam in om een nieuwe resourcegroep te maken. Een resourcegroep bevat verwante resources voor een Azure-oplossing. In dit voorbeeld gebruiken we **docs-aml**. 
   Location | Selecteer de locatie die het dichtst bij uw gebruikers en de gegevensresources. Deze locatie is waar de werkruimte is gemaakt.

1. Selecteer eerst het maakproces **revisie + maken**.

    ![Werkruimte maken](./media/aml-create-in-portal/workspace-create-main-tab.png)

1. Controleer de Werkruimteconfiguratie van uw. Als deze juist is, selecteert u **maken**. Het kan even duren om de werkruimte te maken.

1. Als u wilt controleren op de status van de implementatie, selecteer het pictogram meldingen **bell**, op de werkbalk.

1. Wanneer het proces is voltooid, wordt er een bericht voor de implementatie weergegeven. Het is ook aanwezig in de sectie meldingen. Als u de nieuwe werkruimte, selecteert u **naar de resource gaan**.

   ![Status van het maken van werkruimte](./media/aml-create-in-portal/notifications.png)
