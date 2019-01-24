---
title: Tenanttoepassingen weergeven - Azure Active Directory | Microsoft Docs
description: Gebruik de Azure-portal om de toepassingen in uw Azure AD-tenant (Azure Active Directory) weer te geven.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/25/2018
ms.author: barbkess
ms.reviewer: arvinh
ms.custom: it-pro
ms.openlocfilehash: ce998cd23ac3606ca9269e1e3316e6679fe0a7af
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54462858"
---
# <a name="view-your-azure-active-directory-tenant-applications"></a>Azure Active Directory-tenanttoepassingen weergeven

In deze quickstart wordt de Azure-portal gebruikt om de toepassingen in uw Azure AD-tenant (Azure Active Directory) weer te geven.

## <a name="before-you-begin"></a>Voordat u begint

Uw Azure AD-tenant moet over minstens één toepassing beschikken om resultaten te zien. Raadpleeg de quickstart [Een toepassing toevoegen](add-application-portal.md) om een toepassing toe te voegen.

Meld u bij de [Azure-portal](https://portal.azure.com) aan als globale beheerder van uw Azure AD-tenant, als beheerder van cloudtoepassingen of als toepassingsbeheerder.

## <a name="find-the-list-of-tenant-applications"></a>De lijst met tenanttoepassingen zoeken

Azure AD-tenanttoepassingen worden weergegeven in de sectie **Bedrijfsapps** van de Azure-portal.

Ga als volgt te werk om de tenanttoepassingen te zoeken:

1. Klik in de **[Azure-portal](https://portal.azure.com)** in het navigatiepaneel aan de linkerkant op **Azure Active Directory**. 

2. Klik op de blade Azure Active Directory op **Bedrijfstoepassingen**. 

3. Selecteer in de vervolgkeuzelijst **Toepassingstype** de optie **Alle toepassingen** en klik op **Toepassen**. Er wordt een willekeurige selectie uit uw tenanttoepassingen weergegeven.

    ![Enterprise-apps](media/view-applications-portal/open-enterprise-apps.png)
   
4. Als u meer toepassingen wilt zien, klikt u onder aan de lijst op **Meer weergeven**. Afhankelijk van het aantal toepassingen in de tenant is het welllicht gemakkelijker om te [zoeken naar een bepaalde toepassing](#search-for-a-tenant-application) in plaats van door de lijst te schuiven.

## <a name="select-viewing-options"></a>Weergaveopties selecteren

Selecteer in deze sectie de opties op basis van wat u zoekt.

1. U kunt de toepassingen weergeven op basis van opties voor **Toepassingstype**, **Toepassingsstatus** en **Zichtbaarheid van toepassing**. 

    ![Opties voor zoeken](media/view-applications-portal/search-options.png)

2. Kies onder **Toepassingstype** een van de volgende opties:

    - **Bedrijfstoepassingen** geeft niet-Microsoft-toepassingen weer.
    - **Microsoft-toepassingen** geeft Microsoft-toepassingen weer.
    - **Alle toepassingen** geeft zowel niet-Microsoft- als Microsoft-toepassingen weer.

3. Kies onder **Toepassingsstatus** een van de volgende opties: **Alle**, **Uitgeschakeld** of **Ingeschakeld**. De optie **Alle** omvat zowel de uitgeschakelde als de ingeschakelde toepassingen.

4. Kies onder **Zichtbaarheid van toepassing** de optie **Alle** of **Verborgen**. De optie **Verborgen** geeft toepassingen weer die zich in de tenant bevinden maar die niet zichtbaar zijn voor gebruikers.

5. Als u de gewenste opties hebt gekozen, klikt u op **Toepassen**.
 

## <a name="search-for-a-tenant-application"></a>Zoeken naar een tenanttoepassing

Ga als volgt te werk om naar een bepaalde toepassing te zoeken:

1. Selecteer in het menu **Toepassingstype** de optie **Alle toepassingen** en klik op **Toepassen**.

2. Voer de naam in van de toepassing die u zoekt. Als de toepassing is toegevoegd aan uw Azure AD-tenant, wordt deze weergegeven in de zoekresultaten. In dit voorbeeld ziet u dat GitHub niet is toegevoegd aan de tenanttoepassingen.

    ![Zoeken naar een toepassing](media/view-applications-portal/search-for-tenant-application.png)

3. Voer de eerste paar letters van een toepassingsnaam in.  In dit voorbeeld ziet u dat alle toepassingen beginnen met **Verkoop**.

    ![Zoeken met een voorvoegsel](media/view-applications-portal/search-by-prefix.png)

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u geleerd hoe u de toepassingen in uw Azure AD-tenant kunt weergeven en hoe u de lijst met toepassingen kunt filteren op type, status en zichtbaarheid van de toepassing. U hebt ook geleerd hoe u kunt zoeken naar een bepaalde toepassing.

Nu u de toepassing hebt gevonden die u zocht, kunt u [meer toepassingen toevoegen aan de tenant](add-application-portal.md) of op de toepassing klikken om de eigenschappen en configuratieopties te bekijken of te bewerken. U kunt bijvoorbeeld ook eenmalige aanmelding configureren. 

> [!div class="nextstepaction"]
> [Eenmalige aanmelding configureren](configure-single-sign-on-portal.md)


