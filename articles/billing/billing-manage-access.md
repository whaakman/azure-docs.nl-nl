---
title: Beheer de toegang tot Azure-facturering | Microsoft Docs
description: Leer hoe u toegang geven tot uw Azure-Factureringsinformatie voor leden van uw team.
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
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 6726c876d0895f9488aa2ae5c225a6b2ac19e69f
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491051"
---
# <a name="manage-access-to-billing-information-for-azure"></a>Beheer de toegang tot factureringsgegevens voor Azure

U kunt andere gebruikers toegang bieden tot de factureringsgegevens voor uw account in Azure portal. Het type van de facturering rollen en de instructies voor toegang tot de factureringsgegevens variëren per type van uw factureringsrekening. Zie het vaststellen van het type van uw factureringsrekening [controleert u het type van uw factureringsrekening](#check-the-type-of-your-billing-account).

Het artikel is van toepassing op klanten met accounts van Microsoft Online Service-programma. Als u een Azure-klant met een Enterprise Agreement (EA) en de Enterprise-beheerder bent, kunt u machtigingen geven voor de afdeling beheerders en eigenaren van een Account in de Enterprise portal. Zie voor meer informatie, [beheerdersrollen in Azure begrijpen Azure Enterprise overeenkomst](billing-understand-ea-roles.md). Als u een Microsoft-KLANTOVEREENKOMST-klant bent, ziet, [KLANTOVEREENKOMST Microsoft informatie over beheerdersrollen in Azure](billing-understand-mca-roles.md). 

## <a name="account-administrators-for-microsoft-online-service-program-accounts"></a>Accountbeheerders voor Microsoft Online Service programma-accounts

Een beheerder is de enige eigenaar voor een programma van de Microsoft Online Service-account voor facturering. De rol wordt toegewezen aan een persoon die zich hebben aangemeld voor Azure. Accountbeheerders bent gemachtigd om uit te voeren van verschillende facturering taken zoals het maken van abonnementen, facturen weergeven of wijzigen van de facturering voor een abonnement.

## <a name="give-others-access-to-view-billing-information"></a>Anderen toegang geven tot factureringsgegevens weergeven

Accountbeheerder kan anderen toegang verlenen tot Azure-Factureringsinformatie door toe te wijzen op een van de volgende rollen op een abonnement in hun account.

- Servicebeheerder
- Co-beheerder
- Eigenaar
- Inzender
- Lezer
- Facturering voor lezer

Deze rollen hebben toegang tot factureringsgegevens in de [Azure-portal](https://portal.azure.com/). Personen die deze rollen zijn toegewezen, kunnen ook gebruiken de [facturerings-API's](billing-usage-rate-card-overview.md) om op te halen via een programma facturen en informatie over het gebruik.

Als u wilt toewijzen van rollen, Zie [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

** Als u een EA-klant bent, kunt eigenaar van een Account de bovenstaande rol toewijzen aan andere gebruikers van uw team. Maar voor deze gebruikers om factuurgegevens weer te geven, moet de Enterprise-beheerder de kosten van de door de AO weergeven in de Enterprise portal inschakelen.


### <a name="opt-in"></a> Gebruikers toestaan om te downloaden van facturen

Nadat een accountbeheerder de juiste rollen aan andere gebruikers toegewezen heeft, moeten ze toegang tot het downloaden van facturen in Azure portal inschakelen. Facturen die ouder zijn dan December 2016 zijn beschikbaar alleen naar de accountbeheerder.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/), als een accountbeheerder

1. Zoek naar **kostenbeheer en facturering**.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-manage-access/billing-search-cost-management-billing.png)
 
1. Selecteer **abonnementen** in het linkerdeelvenster. Afhankelijk van uw toegang mogelijk moet u een facturering bereik selecteren en selecteer vervolgens **abonnementen**.
 
    ![Schermafbeelding waarin de abonnementen te selecteren](./media/billing-manage-access/billing-select-subscriptions.png)

1. Selecteer **facturen** en vervolgens **toegang tot de factuur**.

    ![Schermafbeelding ziet u hoe u toegang tot facturen delegeren](./media/billing-manage-access/AA-optin.png)

1. Selecteer **op** en op te slaan.

    ![Schermafbeelding ziet u in-uitschakelen voor toegang tot de factuur delegeren](./media/billing-manage-access/AA-optinAllow.png)

De accountbeheerder kan de instellingen ook zo configureren dat facturen via e-mail worden verzonden. Raadpleeg [Uw factuur per e-mail ontvangen](billing-download-azure-invoice-daily-usage-date.md) voor meer informatie.

## <a name="give-read-only-access-to-billing"></a>Alleen-lezen toegang geven tot facturering

De rol factureren voor lezer toewijzen aan iemand die alleen-lezen toegang tot de factureringsgegevens van het abonnement, maar niet de mogelijkheid om te beheren of maken van Azure-services nodig heeft. Deze rol is geschikt voor gebruikers in een organisatie die verantwoordelijk voor het beheer van financiële en de kosten voor Azure-abonnementen zijn.

De functie factureren voor lezer is in preview en biedt niet-globale clouds nog geen ondersteuning.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/), als een accountbeheerder

1. Zoek naar **kostenbeheer en facturering**.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-manage-access/billing-search-cost-management-billing.png)

1. Selecteer **abonnementen** in het linkerdeelvenster. Afhankelijk van uw toegang mogelijk moet u een facturering bereik selecteren en selecteer vervolgens **abonnementen**.
 
    ![Schermafbeelding waarin de abonnementen te selecteren](./media/billing-manage-access/billing-select-subscriptions.png)

1. Selecteer **toegangsbeheer (IAM)** .
1. Selecteer **toevoegen** vanaf de bovenkant van de pagina.

    ![Schermafbeelding van te klikken op roltoewijzing toevoegen](./media/billing-manage-access/billing-click-add-role-assignment.png)

1. In de **rol** vervolgkeuzelijst Kies **factureren voor lezer**.
1. In de **Selecteer** tekstvak, typ de naam of e-voor de gebruiker die u wilt toevoegen.
1. Selecteer de gebruiker.
1. Selecteer **Opslaan**.
    ![Schermafbeelding van te klikken op roltoewijzing toevoegen](./media/billing-manage-access/billing-save-role-assignment.png)

1. Na enkele ogenblikken wordt de gebruiker de rol van lezer facturering voor het abonnement toegewezen.

** Als u een EA-klant bent, kunt een accounteigenaar of beheerder van de afdeling de rol van lezer facturering toewijzen aan leden van een team. Maar voor deze factureren voor lezer om informatie over facturering voor de afdeling of het account weer te geven, de Enterprise-beheerder moet inschakelen **kosten weergeven die door de AO** of **DA weergave kosten** beleid in de Enterprise portal.

## <a name="check-the-type-of-your-billing-account"></a>Het type van uw factureringsrekening controleren
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

## <a name="next-steps"></a>Volgende stappen

- Gebruikers in andere rollen, zoals eigenaar of bijdrager, hebben toegang tot niet alleen factureringsgegevens, maar ook Azure services. Zie voor het beheren van deze rollen, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).
- Zie voor meer informatie over rollen [ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).
