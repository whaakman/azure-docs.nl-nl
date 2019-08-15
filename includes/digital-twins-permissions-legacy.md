---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: 9f4bf6fb92b590e274e8880b5f900e5469f85727
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012104"
---
1. Open in de [Azure-portal](https://portal.azure.com) **Azure Active Directory** in linkerdeelvenster en open vervolgens het deelvenster **Eigenschappen**. Kopieer de **Map-id** naar een tijdelijk bestand. U gebruikt deze waarde om de voorbeeldtoepassing in de volgende sectie te configureren.

    ![Map-id voor Azure Active Directory](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)

1. Open in de [Azure Portal](https://portal.azure.com) **Azure Active Directory** in het linkerdeel venster en open vervolgens het deel venster **app-registraties (verouderd)** . Selecteer de knop **nieuwe toepassing registreren** .

1. Geef een beschrijvende naam voor deze app-registratie op in het vak **Naam**. Kies voor **Toepassingstype** de optie **Systeemeigen** en voor **Omleidings-URI** `https://microsoft.com`. Selecteer **Maken**.

    ![Deelvenster maken](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)

1.  Open de geregistreerde app en kopieer de waarde in het veld **Toepassings-id** naar een tijdelijk bestand. Deze waarde identificeert uw Azure Active Directory-app. U gebruikt de toepassings-id om uw voorbeeldtoepassing in de volgende secties te configureren.

    ![Azure Active Directory-toepassings-id](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)

1. Open het deelvenster voor app-registratie. Selecteer **Instellingen** > **Vereiste machtigingen** en ga als volgt te werk:

   a. Selecteer linksboven **Toevoegen** om het deelvenster **API-toegang toevoegen** te openen.

   b. Selecteer **Een API selecteren** en zoek **Azure Digital Twins**. Als de API met uw zoekopdracht niet wordt gevonden, zoekt u in plaats daarvan **Azure Smart Spaces**.

   c. Selecteer de optie **Azure Digital Twins (Azure Smart Spaces-service)** en kies **Selecteren**.

   d. Kies **Machtigingen selecteren**. Schakel het selectievakje **Lees-/schrijftoegang** voor gedelegeerde machtigingen in en klik op **Selecteren**.

   e. Selecteer **Gereed** in het deelvenster **API-toegang toevoegen**.

   f. Selecteer in het deelvenster **Vereiste machtigingen** de knop **Machtigingen verlenen** en accepteer de bevestiging die wordt weergegeven. Neem contact op met de beheerder als de machtiging voor deze API niet is toegekend.

      ![Deelvenster Vereiste machtigingen](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)

 