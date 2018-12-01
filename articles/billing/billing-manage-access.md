---
title: Beheer de toegang tot Azure-facturering | Microsoft Docs
description: ''
services: ''
documentationcenter: ''
author: vikramdesai01
manager: amberb
editor: ''
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: cwatson
ms.openlocfilehash: 00b29ddf29ae334154d41eedd0bf9e66556e09ad
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52720116"
---
# <a name="manage-access-to-billing-information-for-azure"></a>Beheer de toegang tot factureringsgegevens voor Azure

U kunt voor de meeste abonnementen, facturering Informatietoegang geven tot leden van uw team van **abonnementen** in Azure portal. Als u een Azure-klant met een Enterprise Agreement (EA-klanten bent) en de Enterprise-beheerder bent, kunt u machtigingen geven voor de afdeling beheerders en eigenaren van een Account in de Enterprise portal.

## <a name="give-access-to-billing"></a>Toegang verlenen tot facturering

Alles behalve EA-klanten kunnen toegang verlenen tot Azure-Factureringsinformatie door een van de volgende gebruikersrollen toewijzen aan leden van uw team:

- Accountbeheerder
- Servicebeheerder
- Co-beheerder
- Eigenaar
- Inzender
- Lezer
- Lezer voor facturering

Als u wilt toewijzen van rollen, Zie [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

Deze rollen hebben toegang tot factureringsgegevens in de [Azure-portal](https://portal.azure.com/). Personen die deze rollen zijn toegewezen, kunnen ook gebruiken de [facturerings-API's](billing-usage-rate-card-overview.md) om op te halen via een programma facturen en informatie over het gebruik. Zie voor meer informatie, [rollen in Azure RBAC](../role-based-access-control/built-in-roles.md).

### <a name="opt-in"></a> Gebruikers toestaan om te downloaden van facturen

Nadat u evalueren toewijzen die de juiste rollen voor leden van uw team, de accountbeheerder moeten inschakelen om te downloaden van facturen in Azure portal. Facturen die ouder zijn dan December 2016 zijn beschikbaar alleen naar de accountbeheerder.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Als de accountbeheerder, selecteer uw abonnement uit de [blade abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.

1. Selecteer **facturen** en vervolgens **toegang tot facturen**.

    ![Schermafbeelding ziet u hoe u toegang tot facturen delegeren](./media/billing-manage-access/AA-optin.png)

1. Selecteer **op** en op te slaan.

    ![Schermafbeelding ziet u in-uitschakelen voor toegang tot de factuur delegeren](./media/billing-manage-access/AA-optinAllow.png)

De accountbeheerder kan de instellingen ook zo configureren dat facturen via e-mail worden verzonden. Raadpleeg [Uw factuur per e-mail ontvangen](billing-download-azure-invoice-daily-usage-date.md) voor meer informatie.

## <a name="give-read-only-access-to-billing"></a>Alleen-lezen toegang geven tot facturering

De rol factureren voor lezer toewijzen aan iemand die alleen-lezen toegang tot de factureringsgegevens van het abonnement, maar niet de mogelijkheid om te beheren of maken van Azure-services nodig heeft. Deze rol is geschikt voor gebruikers in een organisatie die verantwoordelijk voor het beheer van financiële en de kosten voor Azure-abonnementen zijn.

Als u een EA-klant bent, kunt een accounteigenaar of beheerder van de afdeling de rol van lezer facturering toewijzen aan leden van een team. Maar voor deze factureren voor lezer om informatie over facturering voor de afdeling of het account weer te geven, de Enterprise-beheerder moet inschakelen **kosten weergeven die door de AO** of **DA weergave kosten** beleid in de Enterprise portal.

De functie factureren voor lezer is in preview en biedt niet-globale clouds nog geen ondersteuning.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer uw abonnement op de [blade Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in de Azure-portal.

1. Selecteer **toegangsbeheer (IAM)**.
1. Selecteer **roltoewijzingen** om de roltoewijzingen voor dit abonnement weer te geven.
1. Selecteer **roltoewijzing toevoegen**.
1. In de **rol** vervolgkeuzelijst Kies **factureren voor lezer**.
1. In de **Selecteer** tekstvak, typ de naam of e-voor de gebruiker die u wilt toevoegen.
1. Selecteer de gebruiker.
1. Selecteer **Opslaan**.
1. Na enkele ogenblikken wordt de gebruiker de rol van lezer facturering op het abonnementsbereik toegewezen.
1. Het factureren voor lezer ontvangt een e-mailbericht met een koppeling aan te melden.

    ![Schermopname die laat zien wat de factureren voor lezer kan zien in Azure portal](./media/billing-manage-access/billing-reader-view.png)

## <a name="allow-department-administrator-or-account-owner-billing-access"></a>Afdeling beheerder of eigenaar van Account facturering toegang toestaan

De Enterprise-beheerder kunt toestaan dat de beheerders van de afdeling en eigenaars om weer te geven informatie over het gebruik en de kosten die zijn gekoppeld aan de verschillende afdelingen en Accounts die ze beheren.

1. Als de Enterprise-beheerder aanmelden bij de [EA-portal](https://ea.azure.com/).
1. Selecteer **beheren**.
1. Onder **inschrijving**, wijzigt de **DA weergave kosten** naar **ingeschakeld** voor de beheerder van de afdeling gebruik en kosten kunt bekijken.
1. Wijziging **kosten weergeven die door de AO** naar **ingeschakeld** voor de eigenaar van het gebruik en kosten kunt bekijken.


Zie voor meer informatie, [beheerdersrollen in Azure begrijpen Azure Enterprise overeenkomst](billing-understand-ea-roles.md).

## <a name="only-account-admins-can-access-account-center"></a>Alleen de accountbeheerders hebben toegang tot Accountcentrum

De accountbeheerder is de juridische eigenaar van het abonnement. De persoon die zich heeft aangemeld of het Azure-abonnement hebt gekocht is de accountbeheerder, tenzij de [abonnement eigendom is overgedragen](billing-subscription-transfer.md) aan iemand anders. De accountbeheerder kunt maken van abonnementen, abonnementen annuleren, het factuuradres voor een abonnement wijzigen en beheren van toegangsbeleid voor het abonnement van de [Accountcentrum](https://account.azure.com/Subscriptions).

## <a name="next-steps"></a>Volgende stappen

- Gebruikers in andere rollen, zoals eigenaar of bijdrager, hebben toegang tot niet alleen factureringsgegevens, maar ook Azure services. Zie voor het beheren van deze rollen, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).
- Zie voor meer informatie over rollen [ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
