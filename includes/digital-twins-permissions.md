---
title: bestand opnemen
description: bestand opnemen
services: azure-digital-twins
author: alinamstanciu
ms.service: azure-digital-twins
ms.topic: include
ms.date: 09/19/2018
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: d08b7a1256a26d1d7d39481ba15a8637339063ea
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322778"
---
1. Open in de [Azure-portal](https://portal.azure.com) **Azure Active Directory** in het navigatievenster aan de linkerkant en open vervolgens het deelvenster **Eigenschappen**. Kopieer de **Map-id** naar een tijdelijk bestand. U gebruikt deze waarde om de voorbeeldtoepassing in de volgende sectie te configureren.

    ![Map-id voor Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-tenant.png)

1. Open het deelvenster **App-registraties** en klik vervolgens op de knop **Nieuwe toepassing registreren**.
    
    ![Nieuwe app-registratie Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-start.png)

1. Geef een beschrijvende naam voor deze app-registratie op in het veld **Naam**. Kies voor **Toepassingstype** de optie **_Systeemeigen_** en voor **Omleidings-URI** **_https://microsoft.com_**. Klik op **Create**.

    ![App-registratie maken in Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-create.png)

1. Open de geregistreerde app en kopieer de waarde van het veld **Toepassings-id** naar een tijdelijk bestand; deze waarde identificeert uw Azure Active Directory-app. U gebruikt de toepassings-id om uw voorbeeldtoepassing in de volgende secties te configureren.

    ![Azure Active Directory-toepassings-id](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Open het deelvenster App-registratie, en klik op **Instellingen** > **Vereiste machtigingen**:
    - Klik linksboven op **Toevoegen** om het deelvenster **API-toegang toevoegen** te openen.
    - Klik op **Een API selecteren** en zoek naar **Azure Digital Twins**. Als de API met uw zoekopdracht niet wordt gevonden, zoekt u in plaats daarvan **Azure Smart Spaces**.
    - Selecteer de optie **Azure Digital Twins (Azure Smart Spaces-service)** en klik op **Selecteren**.
    - Klik op **Machtigingen selecteren**. Controleer het vak **Lees-/schrijftoegang** voor gedelegeerde machtigingen en klik op **Selecteren**.
    - Klik op **Gereed** in het deelvenster **API-toegang toevoegen**.
    - Klik in het deelvenster **Vereiste machtigingen**op de knop **Machtigingen verlenen** en accepteer de bevestiging die wordt weergegeven.

       ![API toevoegen in Azure Active Directory-app-registratie](./media/digital-twins-permissions/aad-app-req-permissions.png)
