---
title: Toegang met groepen of toepassingen in toegangsbeoordelingen - Azure Active Directory controleren | Microsoft Docs
description: Meer informatie over het bekijken van de groepsleden of toepassing toegang in Azure Active Directory-toegangsbeoordelingen.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02c12a8cc8163fa70008b21995ddb35aec57dfcf
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579679"
---
# <a name="review-access-to-groups-or-applications-in-azure-ad-access-reviews"></a>Controleer toegang tot groepen of toepassingen in Azure AD-toegangsbeoordelingen

Azure Active Directory (Azure AD) vereenvoudigt hoe bedrijven toegang aan groepen en toepassingen beheren in Azure AD en andere Microsoft Online Services met een functie met de naam van de toegang van Azure AD-toegangsbeoordelingen.

Dit artikel wordt beschreven hoe een aangewezen revisor voert een toegangscontrole uit voor leden van een groep of gebruikers met toegang tot een toepassing.

## <a name="open-the-access-review"></a>Open de toegangsbeoordeling

De eerste stap bij het uitvoeren van een toegangsbeoordeling is om te zoeken en openen van de toegangsbeoordeling.

1. Zoek naar een e-mailbericht van Microsoft waarmee u wordt gevraagd te toegang beoordelen. Hier volgt een voorbeeld van de e-mail om te controleren van de toegang voor een groep.

    ![Controleer toegang tot e-mail](./media/perform-access-review/access-review-email.png)

1. Klik op de **revisie starten** koppeling naar het openen van de toegangsbeoordeling.

Als u het e-mailbericht niet hebt, kunt u uw in behandeling zijnde toegangsbeoordelingen door de volgende stappen kunt vinden.

1. Meld u aan bij de MyApps-portal op [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![MyApps-portal](./media/perform-access-review/myapps-access-panel.png)

1. Klik in de rechterbovenhoek van de pagina op het gebruikerssymbool, waar uw naam en standaardorganisatie worden weergegeven. Als er meerdere organisaties vermeld staan, selecteert u de organisatie waarvoor een toegangsbeoordeling is aangevraagd.

1. Aan de rechterkant van de pagina, klikt u op de **Toegangsbeoordelingen** tegel voor een overzicht van de in behandeling zijnde toegangsbeoordelingen.

    Als de tegel niet zichtbaar is, zijn er geen toegangsbeoordelingen die voor die organisatie moeten worden uitgevoerd en hoeft er momenteel geen actie te worden ondernomen.

    ![Lijst met toegangsbeoordelingen](./media/perform-access-review/access-reviews-list.png)

1. Klik op de **controle beginnen** koppeling voor de toegangsbeoordeling die u wilt uitvoeren.

## <a name="perform-the-access-review"></a>De toegangscontrole uitvoeren

Nadat u de toegangsbeoordeling hebt geopend, ziet u de namen van gebruikers die moeten worden gecontroleerd.

Als de aanvraag is uw eigen toegang controleren, ziet de pagina er anders. Zie voor meer informatie, [toegang beoordelen zelf aan groepen of toepassingen](review-your-access.md).

![Toegangscontrole uitvoeren](./media/perform-access-review/perform-access-review.png)

Er zijn twee manieren waarop u kunt goedkeuren of weigeren van toegang:

- U kunt goedkeuren of weigeren van elke aanvraag afzonderlijk, of
- U kunt de systeemaanbevelingen, dit is de eenvoudigste en snelste manier accepteren.

### <a name="approve-or-deny-access-for-each-request"></a>Goedkeuren of weigeren van toegang voor elke aanvraag

1. Bekijk de lijst van gebruikers om te bepalen of u wilt goedkeuren of weigeren van de blijvende toegang.

1. Als u wilt goedkeuren of afwijzen elke aanvraag, klikt u op de rij om het te openen om op te geven van de actie te ondernemen.

1. Klik op **goedkeuren** of **weigeren**. Als u niet zeker weet, klikt u op **weet niet**. In dat geval zal leiden tot de gebruiker die het beheren van zijn/haar toegang, maar de selectie, worden weergegeven in de auditlogboeken.

    ![Toegangscontrole uitvoeren](./media/perform-access-review/approve-deny.png)

    De beheerder van de toegangsbeoordeling mogelijk dat u een reden voor het goedkeuren van blijvende toegang of lidmaatschap van groep opgeven.

1. Nadat u de actie te ondernemen hebt opgegeven, klikt u op **opslaan**.

    Als u uw antwoord wijzigen wilt, selecteert u de rij en bijwerken van het antwoord. Bijvoorbeeld, kunt u een eerder geweigerde gebruiker goedkeuren of weigeren van een eerder goedgekeurde gebruiker. U kunt uw antwoord wijzigen op elk gewenst moment totdat de toegangsbeoordeling is beëindigd.

    Als er meerdere recensenten, wordt de laatst ingediende antwoord wordt vastgelegd. Bekijk een voorbeeld waarin een beheerder twee revisoren – Els en Bob wijst. Els wordt de toegangsbeoordeling eerst geopend en keurt deze goed toegang. Voordat de beoordeling is beëindigd, wordt Bob opent de toegangsbeoordeling en de toegang weigert. De laatste weigeren antwoord is wat wordt vastgelegd.

    > [!NOTE]
    > Als een gebruiker toegang wordt geweigerd, worden niet ze onmiddellijk verwijderd. Ze worden verwijderd wanneer de beoordeling is voltooid of wanneer een beheerder de controle wordt gestopt.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Goedkeuren of weigeren van toegang op basis van aanbevelingen

Als u wilt maken met toegangsbeoordelingen sneller en gemakkelijker voor u, bieden we ook aanbevelingen die u kunt accepteren met één klik. De aanbevelingen zijn gegenereerd op basis van de aanmeldingsactiviteiten van de gebruiker.

1. Klik in de blauwe balk aan de onderkant van de pagina op **aanbevelingen accepteren**.

    ![Aanbevelingen accepteren](./media/perform-access-review/accept-recommendations.png)

    U ziet een overzicht van de aanbevolen acties uitvoert.

    ![Samenvatting aanbevelingen accepteren](./media/perform-access-review/accept-recommendations-summary.png)

1. Klik op **Ok** te accepteren van de aanbevelingen.

## <a name="next-steps"></a>Volgende stappen

- [Een toegangscontrole van groepen of toepassingen voltooien](complete-access-review.md)
