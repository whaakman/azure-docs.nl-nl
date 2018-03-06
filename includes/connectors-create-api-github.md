---
title: bestand opnemen
description: bestand opnemen
services: logic-apps
author: MandiOhlinger
ms.service: logic-apps
ms.topic: include
ms.date: 03/02/2018
ms.author: mandia
ms.custom: include file
ms.openlocfilehash: ec5b3ca9ccd139cbdf17768056eb1d835336e7a7
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
1. In de [Azure-portal](https://portal.azure.com), een lege logische app maken. 

2. Voer 'github' als filter in de ontwerpfunctie voor Logic Apps. 

3. Selecteer de GitHub-connector en de trigger die u wilt gebruiken.

   ![Selecteer de GitHub-connector en een trigger](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Alle logische app werkstromen moeten beginnen met een trigger. Alleen wanneer de werkstroom logica al met een trigger begint, kunt u acties selecteren. 

4. Als u een verbinding eerder hebt gemaakt, kiest u **aanmelden** zodat u uw referenties in GitHub wanneer u wordt gevraagd kan leveren.  

   ![Meld u aan met uw GitHub-referenties](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   Uw logische app gebruikt deze referenties om verbinding te maken en toegang tot gegevens voor uw GitHub-account autoriseren. 

5. Geef uw GitHub-gebruikersnaam en het wachtwoord en bevestig vervolgens uw autorisatie.

   ![Geef referenties op en Bevestig autorisatie](./media/connectors-create-api-github/github-connector-authorize.png)   

   De verbinding is nu gemaakt in de Azure portal en is klaar voor gebruik.

6. Blijven uw logische app werkstroom definiëren.

   ![Meer acties toevoegen aan uw logische app-werkstroom](./media/connectors-create-api-github/github-connector-logic-app.png)

