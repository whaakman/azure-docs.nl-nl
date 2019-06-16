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
ms.openlocfilehash: 11280e1678f52ede928cb2a85ea83add222e15fa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66149213"
---
1. In de [Azure-portal](https://portal.azure.com), een lege logische app maken. 

2. Voer 'github' als filter in de ontwerper van logische Apps. 

3. Selecteer de GitHub-connector en de trigger die u wilt gebruiken.

   ![De GitHub-connector en een trigger selecteren](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Alle werkstromen voor logische app moeten beginnen met een trigger. Alleen wanneer uw werkstroom voor logische al met een trigger begint, kunt u acties selecteren. 

4. Als u eerder een verbinding hebt gemaakt, kiest u **aanmelden** , zodat u opgeven kunt wanneer u hierom wordt gevraagd uw GitHub-referenties.  

   ![Meld u aan met uw GitHub-referenties](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   Uw logische app maakt gebruik van deze referenties om verbinding te maken en openen van gegevens voor uw GitHub-account. 

5. Geef uw GitHub-gebruikersnaam en wachtwoord en Bevestig uw autorisatie.

   ![Geef referenties op en machtiging bevestigen](./media/connectors-create-api-github/github-connector-authorize.png)   

   De verbinding is nu gemaakt in Azure portal en is klaar voor gebruik.

6. Doorgaan met het definiëren van uw werkstroom voor logische Apps.

   ![Meer acties toevoegen aan uw werkstroom voor logische Apps](./media/connectors-create-api-github/github-connector-logic-app.png)

