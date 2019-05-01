---
title: Goedkeuren of weigeren van toegang in Azure AD-recht beheer (Preview) - Azure Active Directory
description: Meer informatie over het gebruik van de portal mijn toegang goedkeuren of weigeren van aanvragen voor een pakket toegang in Azure Active Directory waar u recht op management (Preview).
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
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b2d07638f6c6f153ee3640273fbee5e56df0ab2
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541523"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management-preview"></a>Goedkeuren of weigeren van toegang in Azure AD waar u recht op beheer (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Met Azure AD recht management, kunt u beleid configureren voor goedkeuring vereisen voor toegang tot pakketten en kiest u een of meer fiatteurs. Dit artikel wordt beschreven hoe aangewezen goedkeurders kan goedkeuren of weigeren van aanvragen voor toegang tot pakketten.

## <a name="open-request"></a>Open aanvraag

De eerste stap voor het goedkeuren of weigeren van toegang is om te zoeken en openen van de aanvraag in afwachting van goedkeuring. Er zijn twee manieren om de toegang te vragen.

**Vereiste rol:** Goedkeurder

1. Zoek naar een e-mailbericht van Microsoft Azure die u wilt goedkeuren of weigeren van een aanvraag wordt gevraagd. Hier volgt een voorbeeld van de e-mailbericht:

    ![Aanvraag voor toegang tot e-mail van pakket goedkeuren](./media/entitlement-management-shared/email-approve-request.png)

1. Klik op de **goedkeuren of weigeren van aanvraag** koppeling om de toegang te vragen.

1. Aanmelden bij de portal mijn toegang.

Als u het e-mailbericht niet hebt, kunt u de toegangsaanvragen vinden moeten worden goedgekeurd door de volgende stappen.

1. Meld u aan bij de portal My Access op [ https://myaccess.microsoft.com ](https://myaccess.microsoft.com).

1. Klik in het menu links op **goedkeuringen** voor een overzicht van aanvragen in afwachting van goedkeuring.

1. Op de **in behandeling** tabblad, het vinden van de aanvraag.

## <a name="approve-or-deny-request"></a>Goedkeuren of weigeren van aanvraag

Nadat u een aanvraag voor toegang tot in afwachting van goedkeuring opent, ziet u informatie waarmee u bij het maken van een goedkeuren of weigeren van de beslissing.

**Vereiste rol:** Goedkeurder

1. Klik op de **weergave** koppeling om de toegang tot aanvraag deelvenster te openen.

1. Klik op **Details** voor informatie over de aanvraag.

    De details van de gebruiker de naam, organisatie, toegang tot begin- en datum indien opgegeven, zakelijke reden, wanneer de aanvraag is ingediend en verloopt de aanvraag.

1. Klik op **goedkeuren** of **weigeren**.

1. Voer een reden, indien nodig.

    ![Mijn toegangsportal - aanvraag voor toegang tot](./media/entitlement-management-shared/my-access-approve-request.png)

1. Klik op **indienen** om in te dienen uw beslissing.

    Als een beleid is geconfigureerd met meerdere goedkeurders, moet slechts één fiatteur een beslissing nemen over de goedkeuring in behandeling. Nadat een goedkeurder de beslissing om de aanvraag voor toegang tot verzonden heeft, wordt de aanvraag is voltooid en is niet meer beschikbaar voor de andere goedkeurders goedkeuren of weigeren van de aanvraag. De beslissing van de aanvraag en de besluitvormer in de portal mijn toegang tot zien de andere goedkeurders. Op dit moment wordt alleen één fase goedkeuring ondersteund.

    Als geen van de geconfigureerde goedkeurders kan goedkeuren of weigeren van de aanvraag voor toegang tot zijn, wordt de aanvraag verloopt na de duur van de geconfigureerde aanvraag. De gebruiker opgehaald op de hoogte gesteld dat hun access-aanvraag is verlopen en dat ze moeten de aanvraag opnieuw indienen.

## <a name="next-steps"></a>Volgende stappen

- [Aanvraag voor toegang tot een package toegang](entitlement-management-request-access.md)
- [Aanvraag verwerken en e-mailmeldingen](entitlement-management-process.md)
