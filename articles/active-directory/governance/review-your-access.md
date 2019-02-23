---
title: Toegang beoordelen voor uzelf met groepen of toepassingen in Azure AD-Toegangsbeoordelingen | Microsoft Docs
description: Leer hoe u uw eigen toegang met groepen of toepassingen in Azure Active Directory-Toegangsbeoordelingen kunnen beoordelen.
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
ms.openlocfilehash: 76f90a5aa3f201fa5d1578ac63526be26377aedf
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56731205"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Toegang beoordelen voor uzelf met groepen of toepassingen in Azure AD-Toegangsbeoordelingen

Azure Active Directory (Azure AD) vereenvoudigt de manier waarop ondernemingen toegang tot groepen of toepassingen beheren in Azure AD en andere Microsoft Online Services met een functie voor Azure AD-Toegangsbeoordelingen.

In dit artikel wordt beschreven hoe u uw eigen toegang tot een groep of een toepassing bekijken.

## <a name="open-the-access-review"></a>Open de toegangsbeoordeling

De eerste stap bij het uitvoeren van een toegangsbeoordeling is om te zoeken en openen van de toegangsbeoordeling.

1. Zoek naar een e-mailbericht van Microsoft waarmee u wordt gevraagd te toegang beoordelen. Hier volgt een voorbeeld van de e-mail om te controleren van de toegang tot een groep.

    ![Controleer toegang tot e-mail](./media/review-your-access/access-review-email.png)

1. Klik op de **toegang beoordelen** koppeling naar het openen van de toegangsbeoordeling.

Als u het e-mailbericht niet hebt, kunt u uw in behandeling zijnde toegangsbeoordelingen door de volgende stappen kunt vinden.

1. Meld u aan bij de MyApps-portal op [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![MyApps-portal](./media/review-your-access/myapps-access-panel.png)

1. Klik op het symbool gebruiker, die uw organisatie naam en een standaardwaarde weergegeven in de rechterbovenhoek van de pagina. Als meer dan één organisatie wordt weergegeven, selecteert u de organisatie die een toegangsbeoordeling aangevraagd.

1. Aan de rechterkant van de pagina, klikt u op de **Toegangsbeoordelingen** tegel voor een overzicht van de in behandeling zijnde toegangsbeoordelingen.

    Als de tegel niet zichtbaar is, er zijn geen toegangsbeoordelingen om uit te voeren voor deze organisatie en is er op dit moment geen actie nodig.

    ![Lijst met toegangsbeoordelingen](./media/review-your-access/access-reviews-list.png)

1. Klik op de **controle beginnen** koppeling voor de toegangsbeoordeling die u wilt uitvoeren.

## <a name="perform-the-access-review"></a>De toegangscontrole uitvoeren

Nadat u de toegangsbeoordeling hebt geopend, ziet u de toegang.

1. Uw toegang controleren en beslissen of u nog steeds toegang nodig.

    Als de aanvraag om te controleren van toegang voor anderen, ziet de pagina er anders. Zie voor meer informatie, [toegang met groepen of toepassingen beoordelen](perform-access-review.md).

    ![Toegangscontrole uitvoeren](./media/review-your-access/perform-access-review.png)

1. Klik op **Ja** te houden van uw toegang of klik op **geen** om de toegang te verwijderen.

1. Als u klikt op **Ja**, moet u mogelijk Geef een reden in de **reden** vak.

    ![Toegangscontrole uitvoeren](./media/review-your-access/perform-access-review-submit.png)

1. Klik op **Submit**

    Uw selectie wordt verzonden en keert u terug naar de MyApps-portal.

    Als u wijzigen van uw antwoord wilt, de toegang tot beoordelingen pagina opnieuw openen en bijwerken van uw antwoord. U kunt uw antwoord wijzigen op elk gewenst moment totdat de toegangsbeoordeling is beëindigd.

    > [!NOTE]
    > Als u hebt aangegeven dat u toegang niet meer nodig hebt, worden niet u onmiddellijk verwijderd. U worden verwijderd wanneer de beoordeling is voltooid of wanneer een beheerder de controle wordt gestopt.

## <a name="next-steps"></a>Volgende stappen

- [Een toegangscontrole van groepen of toepassingen voltooien](complete-access-review.md)
