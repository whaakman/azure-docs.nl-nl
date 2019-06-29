---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 06/26/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: f03ee57867185eac946be5b596477bf942643587
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459226"
---
>[!NOTE]
>Let erop dat de verouderde AAD app-registratie, die wordt hier gebruikt binnenkort worden afgebroken. In deze sectie wordt bijgewerkt zodra Azure digitale dubbels is volledig geïntegreerd met de [nieuwe AAD-app-registratie](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app). U kunt in de tussentijd experimenteren met nieuwe AAD-app-registratie. Houd er rekening mee dat u wilt gebruiken [openbare client (mobiele en Desktop)](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types#mobile-and-native-apps) voor de *omleidings-URI*. 

1. Open in de [Azure-portal](https://portal.azure.com) **Azure Active Directory** in linkerdeelvenster en open vervolgens het deelvenster **Eigenschappen**. Kopieer de **Map-id** naar een tijdelijk bestand. U gebruikt deze waarde om de voorbeeldtoepassing in de volgende sectie te configureren.

    ![Map-id voor Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-tenant.png)

1. In de [Azure-portal](https://portal.azure.com)Open **Azure Active Directory** in het linkerdeelvenster en open vervolgens de **App-registraties (verouderd)** deelvenster. Selecteer de **nieuwe toepassing registreren** knop.

1. Geef een beschrijvende naam voor deze app-registratie op in het vak **Naam**. Kies voor **Toepassingstype** de optie **Systeemeigen** en voor **Omleidings-URI** `https://microsoft.com`. Selecteer **Maken**.

    ![Deelvenster maken](./media/digital-twins-permissions/aad-app-reg-create.png)

1.  Open de geregistreerde app en kopieer de waarde in het veld **Toepassings-id** naar een tijdelijk bestand. Deze waarde identificeert uw Azure Active Directory-app. U gebruikt de toepassings-id om uw voorbeeldtoepassing in de volgende secties te configureren.

    ![Azure Active Directory-toepassings-id](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Open het deelvenster voor app-registratie. Selecteer **Instellingen** > **Vereiste machtigingen** en ga als volgt te werk:

   a. Selecteer linksboven **Toevoegen** om het deelvenster **API-toegang toevoegen** te openen.

   b. Selecteer **Een API selecteren** en zoek **Azure Digital Twins**. Als de API met uw zoekopdracht niet wordt gevonden, zoekt u in plaats daarvan **Azure Smart Spaces**.

   c. Selecteer de optie **Azure Digital Twins (Azure Smart Spaces-service)** en kies **Selecteren**.

   d. Kies **Machtigingen selecteren**. Schakel het selectievakje **Lees-/schrijftoegang** voor gedelegeerde machtigingen in en klik op **Selecteren**.

   e. Selecteer **Gereed** in het deelvenster **API-toegang toevoegen**.

   f. Selecteer in het deelvenster **Vereiste machtigingen** de knop **Machtigingen verlenen** en accepteer de bevestiging die wordt weergegeven. Als de machtiging is niet verleend voor deze API, contact op met uw beheerder.

      ![Deelvenster Vereiste machtigingen](./media/digital-twins-permissions/aad-app-req-permissions.png)

 