---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 06/28/2019
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: 324f41055cf333081f308a3ff533ff7df6b33038
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "67479179"
---
>[!NOTE]
>Deze sectie vindt u instructies voor het [nieuwe Azure AD-app-registratie](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app). Als u nog steeds oude systeemeigen app-registratie hebt, kan u deze gebruiken, zolang het wordt ondersteund. Als de nieuwe manier van app registation om een bepaalde reden niet in de instellingen werkt is, moet u proberen een oudere systeemeigen AAD-app maken. Lezen [uw digitale dubbels Azure-app registreren bij Azure Active Directory legacy](../articles/digital-twins/how-to-use-legacy-aad.md) voor meer instructies. 

1. In de [Azure-portal](https://portal.azure.com)Open **Azure Active Directory** in het linkerdeelvenster en open vervolgens de **App-registraties** deelvenster. Selecteer de **registratie van nieuwe** knop.

    ![Geregistreerde App](./media/digital-twins-permissions/aad-app-register.png)

1. Geef een beschrijvende naam voor deze app-registratie op in het vak **Naam**. Onder de **omleidings-URI (optioneel)** sectie **openbare client (mobiele en desktop)** in de vervolgkeuzelijst aan de linkerkant en voer `https://microsoft.com` in het tekstvak aan de rechterkant. Selecteer **Registreren**.

    ![Deelvenster maken](./media/digital-twins-permissions/aad-app-reg-create.png)

1. Om ervoor te zorgen dat [de app is geregistreerd als een *systeemeigen app*](https://docs.microsoft.com/azure/active-directory/develop/scenario-desktop-app-registration), open de **verificatie** deelvenster voor uw app-registratie en schuift u omlaag in dit deelvenster. In de **standaard clienttype** sectie **Ja** voor **toepassing behandelen als een openbare client**. 

    ![Standaard systeemeigen](./media/digital-twins-permissions/aad-app-default-native.png)

1.  Open de **overzicht** deelvenster van de geregistreerde app, en kopieer de waarden van de volgende entiteiten in een tijdelijk bestand. U gebruikt deze waarden om te configureren van de voorbeeldtoepassing in de volgende secties.

    - **ID van de toepassing (client)**
    - **Directory (tenant)-ID**

    ![Azure Active Directory-toepassings-id](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Open de **API-machtigingen** deelvenster voor de registratie van uw app. Selecteer **toevoegen van een machtiging** knop. In de **aanvragen API-machtigingen** venster de **API's maakt gebruik van mijn organisatie** tabblad en zoek vervolgens **Azure slimme spaties**. Selecteer de **Azure Smart opslagruimten Service** API.

    ![API voor zoeken](./media/digital-twins-permissions/aad-app-search-api.png)

1. De geselecteerde API wordt weergegeven als **Azure digitale dubbels** in dezelfde **aanvragen API-machtigingen** deelvenster. Selecteer de **lezen (1)** vervolgkeuzelijst en selecteer vervolgens **Read.Write** selectievakje. Selecteer de **machtigingen toevoegen** knop.

    ![API-machtigingen toevoegen](./media/digital-twins-permissions/aad-app-req-permissions.png)

1. Afhankelijk van instellingen van uw organisatie moet u mogelijk extra stappen om beheerderstoegang te verlenen tot deze API. Neem contact op met uw beheerder voor meer informatie. Zodra de beheerderstoegang is goedgekeurd, de **-beheerder toestemming vereist** kolom in de **API-machtigingen** deelvenster worden weergegeven die vergelijkbaar is met de volgende opties voor uw API's:

    ![API-machtigingen toevoegen](./media/digital-twins-permissions/aad-app-admin-consent.png)

