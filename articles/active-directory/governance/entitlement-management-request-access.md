---
title: Aanvraag voor toegang tot een package toegang in Azure AD-recht beheer (Preview) - Azure Active Directory
description: Meer informatie over het gebruik van de portal mijn toegang voor het aanvragen van toegang tot een package toegang in Azure Active Directory waar u recht op management (Preview).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39a50240b4360c5b4adcd6020c2b80b0f06315f7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64541553"
---
# <a name="request-access-to-an-access-package-in-azure-ad-entitlement-management-preview"></a>Toegang vragen tot een package toegang in Azure AD waar u recht op beheer (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="sign-in-to-the-my-access-portal"></a>Aanmelden bij de portal My Access

De eerste stap is het aanmelden bij de portal voor mijn toegang waar u toegang tot een package toegang kunt aanvragen.

**Vereiste rol:** Aanvrager

1. Zoeken naar een e-mailbericht of een bericht uit de project- of business manager die u met werkt. Het e-mailbericht moet een koppeling naar de access-pakket u toegang tot moet bevatten. De koppeling wordt gestart met:

    `https://myaccess.microsoft.com`

1. Open de koppeling.

1. Aanmelden bij de portal mijn toegang.

    Zorg ervoor dat u uw organisatie-account gebruiken. Als u niet zeker weet, neem contact op met uw project- of business-beheer.

## <a name="request-an-access-package"></a>Een pakket toegang aanvragen

Als u het pakket toegang hebt gevonden in de portal mijn toegang, kunt u een aanvraag indienen.

**Vereiste rol:** Aanvrager

1. Klik op het vinkje om te selecteren van het pakket toegang.

    ![Mijn toegangsportal - toegang tot pakketten](./media/entitlement-management-shared/my-access-access-packages.png)

1. Klik op **toegang aanvragen** om de aanvraag voor toegang tot deelvenster te openen.

1. Als de **zakelijke reden** wordt weergegeven, typt u een reden voor hoeven toegang.

1. Als **aanvragen voor die specifieke periode?** is ingeschakeld, selecteert **Ja** of **Nee**.

1. Indien nodig, geef de begindatum en einddatum.

    ![Mijn toegangsportal - aanvraag voor toegang](./media/entitlement-management-shared/my-access-request-access.png)

1. Wanneer u klaar bent, klikt u op **indienen** uw aanvraag indienen.

1. Klik op **geschiedenis van de aanvraag** voor een overzicht van de aanvragen en de status.

    Als het pakket toegang goedkeuring vereist, is de aanvraag nu in een status in afwachting van goedkeuring.

## <a name="cancel-a-request"></a>Een aanvraag annuleren

Als u een aanvraag voor toegang indienen en de aanvraag is nog steeds in de **in afwachting van goedkeuring** staat, kunt u de aanvraag annuleren.

**Vereiste rol:** Aanvrager

1. Klik in de portal mijn toegang aan de linkerkant op **geschiedenis van de aanvraag** voor een overzicht van de aanvragen en de status.

1. Klik op de **weergave** koppeling voor de aanvraag die u wilt annuleren.

1. Als de aanvraag nog steeds in de **in afwachting van goedkeuring** staat, kunt u klikken op **verzoek tot annuleren** annuleren van de aanvraag.

    ![Mijn toegangsportal - verzoek tot annuleren](./media/entitlement-management-request-access/my-access-cancel-request.png)

1. Klik op **geschiedenis van de aanvraag** om te bevestigen van de aanvraag is geannuleerd.

## <a name="select-a-policy"></a>Beleid selecteren

Als u toegang tot een access-pakket met meerdere beleidsregels die van toepassing zijn die u aanvraagt, u mogelijk gevraagd om te selecteren van een beleid. Bijvoorbeeld configureert een Pakketbeheer toegang een access-pakket met twee beleidsregels voor twee groepen interne werknemers. Het eerste beleid mogelijk toegang toestaan tot 60 dagen en goedkeuring vereisen. De tweede beleidsregel mogelijk toegang toestaan voor twee dagen en geen goedkeuring vereist. Als u dit scenario, moet u het beleid dat u wilt gebruiken.

**Vereiste rol:** Aanvrager

## <a name="next-steps"></a>Volgende stappen

- [Goedkeuren of weigeren van toegangsaanvragen](entitlement-management-request-approve.md)
- [Aanvraag verwerken en e-mailmeldingen](entitlement-management-process.md)
