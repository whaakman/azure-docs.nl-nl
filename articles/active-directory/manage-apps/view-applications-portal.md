---
title: Tenanttoepassingen weergeven - Azure Active Directory | Microsoft Docs
description: Gebruik de Azure-portal om de toepassingen in uw Azure AD-tenant (Azure Active Directory) weer te geven.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/25/2018
ms.author: celested
ms.reviewer: arvinh
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ca57746d1b13d2020f78d3c527e7d0c7a444769
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918396"
---
# <a name="view-your-azure-active-directory-tenant-applications"></a>Azure Active Directory-tenanttoepassingen weergeven

In deze quickstart wordt de Azure-portal gebruikt om de toepassingen in uw Azure AD-tenant (Azure Active Directory) weer te geven.

## <a name="before-you-begin"></a>Voordat u begint

Uw Azure AD-tenant moet over minstens één toepassing beschikken om resultaten te zien. Raadpleeg de quickstart [Een toepassing toevoegen](add-application-portal.md) om een toepassing toe te voegen.

Meld u bij de [Azure-portal](https://portal.azure.com) aan als globale beheerder van uw Azure AD-tenant, als beheerder van cloudtoepassingen of als toepassingsbeheerder.

## <a name="find-the-list-of-tenant-applications"></a>De lijst met tenanttoepassingen zoeken

Azure AD-tenanttoepassingen worden weergegeven in de sectie **Bedrijfsapps** van de Azure-portal.

Ga als volgt te werk om de tenanttoepassingen te zoeken:

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**. 

2. In de **Azure Active Directory** venster **bedrijfstoepassingen**. 

3. Uit de **toepassingstype** vervolgkeuzelijst in het menu **alle toepassingen**, en kies **toepassen**. Er wordt een willekeurige selectie uit uw tenanttoepassingen weergegeven.
   
4. Als u meer toepassingen, selecteer **meer laden** aan de onderkant van de lijst. Afhankelijk van het aantal toepassingen in de tenant is het welllicht gemakkelijker om te [zoeken naar een bepaalde toepassing](#search-for-a-tenant-application) in plaats van door de lijst te schuiven.

## <a name="select-viewing-options"></a>Weergaveopties selecteren

Selecteer de opties op basis van wat u zoekt.

1. U kunt de toepassingen door bekijken **toepassingstype**, **toepassingsstatus**, en **zichtbaarheid van toepassing**. 

2. Kies onder **Toepassingstype** een van de volgende opties:

    - **Bedrijfstoepassingen** geeft niet-Microsoft-toepassingen weer.
    - **Microsoft-toepassingen** geeft Microsoft-toepassingen weer.
    - **Alle toepassingen** geeft zowel niet-Microsoft- als Microsoft-toepassingen weer.

3. Kies onder **Toepassingsstatus** een van de volgende opties: **Alle**, **Uitgeschakeld** of **Ingeschakeld**. De optie **Alle** omvat zowel de uitgeschakelde als de ingeschakelde toepassingen.

4. Kies onder **Zichtbaarheid van toepassing** de optie **Alle** of **Verborgen**. De **verborgen** optie worden de toepassingen die in de tenant zijn, maar zijn niet zichtbaar voor gebruikers weergegeven.

5. Nadat u de gewenste opties is gekozen, selecteert u **toepassen**.
 

## <a name="search-for-a-tenant-application"></a>Zoeken naar een tenanttoepassing

Om te zoeken naar een bepaalde toepassing:

1. In de **toepassingstype** in het menu **alle toepassingen**, en kies **toepassen**.

2. Voer de naam in van de toepassing die u zoekt. Als de toepassing is toegevoegd aan uw Azure AD-tenant, worden deze weergegeven in de lijst met zoekresultaten. In dit voorbeeld laat zien dat GitHub nog niet is toegevoegd aan de tenant-toepassingen.

    ![Zoeken naar een toepassing](media/view-applications-portal/search-for-tenant-application.png)

3. Voer de eerste paar letters van een toepassingsnaam in. In dit voorbeeld ziet u dat alle toepassingen beginnen met **Verkoop**.

    ![Zoeken met een voorvoegsel](media/view-applications-portal/search-by-prefix.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u de toepassingen in uw Azure AD-tenant te bekijken. Hebt u geleerd hoe u filtert de lijst met toepassingen door het toepassingstype, status en zichtbaarheid. U hebt ook geleerd hoe u kunt zoeken naar een bepaalde toepassing.

Nu dat u de toepassing hebt gevonden u zijn zoekt, kunt u doorgaan met [meer toepassingen toevoegen aan uw tenant](add-application-portal.md). Of u de toepassing weergeven of bewerken van eigenschappen en configuratie-opties kunt selecteren. U kunt bijvoorbeeld ook eenmalige aanmelding configureren. 

> [!div class="nextstepaction"]
> [Eenmalige aanmelding configureren](configure-single-sign-on-portal.md)


