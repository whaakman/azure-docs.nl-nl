---
title: Toevoegen of wijzigen van de Azure-abonnement beheerdersrollen | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure Medebeheerder, servicebeheerder en accountbeheerder toevoegen of wijzigen
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/30/2017
ms.author: genli
ms.openlocfilehash: d78174cd968c0f918a07027daf1e59665d6b6c1e
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Toevoegen of wijzigen van de Azure-beheerdersrollen die het abonnement of de services beheren

U kunt de Azure-beheerder die uw Azure-abonnement beheert of de Azure-services in uw abonnement beheert wijzigen. Azure factureringsgegevens bekijken en beheren van abonnementen, u moet zich aanmelden met het Account Center als de accountbeheerder. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Een beheerder RBAC-eigenaar voor een abonnement toevoegen in Azure-portal 

Als u wilt toevoegen iemand als beheerder voor een abonnement in de Azure-portal, het is raadzaam zodat ze een [RBAC](../active-directory/role-based-access-control-configure.md) rol van eigenaar. De rol van eigenaar kan de resources in het abonnement dat u toegewezen en heeft geen toegangsrechten naar andere abonnementen beheren. De eigenaar u via toevoegen de [Azure-portal](https://portal.azure.com) niet beheren resource in de [klassieke Azure-portal](https://manage.windowsazure.com).

1. Aanmelden bij de [abonnementen weergeven in de Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Het abonnement dat u wilt dat de beheerder om toegang te selecteren.
1. Selecteer **toegangsbeheer (IAM)** in het menu.
1. Selecteer **toevoegen** > **rol** > **eigenaar**. Typ de e-mailadres van de gebruiker die u wilt toevoegen als eigenaar, selecteert u de gebruiker en selecteer vervolgens **opslaan**.

    ![Schermafbeelding van de rol van eigenaar geselecteerd](./media/billing-add-change-azure-subscription-administrator/add-role.png)

## <a name="add-or-change-co-administrator"></a>Toevoegen of wijzigen medebeheerder

Alleen een eigenaar kan worden toegevoegd als medebeheerder. Andere gebruikers met functies zoals Inzender en lezer kunnen niet worden toegevoegd als medebeheerders.

1. Als u nog niet gedaan hebt, kunt u iemand als eigenaar van een van bovenstaande instructies toevoegen.
2. **Met de rechtermuisknop op** de gebruiker de eigenaar u zojuist hebt toegevoegd, en selecteer vervolgens **toevoegen als medebeheerder**. Als u niet ziet de **toevoegen als medebeheerder** optie vers de pagina of probeer een andere internetbrowser. 

     ![Schermafbeelding van medebeheerder voegt](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >U moet de 'Eigenaar'-account toevoegen als medebeheerder als de gebruiker voor het beheren van de Azure-services in [klassieke Azure-portal](https://manage.windowsazure.com/).

    Verwijderen van de machtiging medebeheerder **met de rechtermuisknop op** 'CO-beheerder' gebruiker en selecteer vervolgens **medebeheerder verwijderen**.

    ![Schermopname die medebeheerder verwijdert](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>De servicebeheerder voor een Azure-abonnement wijzigen

Alleen de accountbeheerder kan de servicebeheerder voor een abonnement kunt wijzigen. Standaard, wanneer u zich aanmeldt, is de servicebeheerder hetzelfde als de accountbeheerder.

1. Zorg ervoor dat uw scenario wordt ondersteund door het controleren van de [limieten voor het wijzigen van servicebeheerders](#limits).
1. Aanmelden bij [Accountcentrum](https://account.windowsazure.com/subscriptions) als accountbeheerder.
1. Selecteer een abonnement.
1. Selecteer aan de rechterkant **abonnementsgegevens bewerken**.

    ![Schermopname van de knop bewerken abonnement in Accountcentrum](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. In de **SERVICEBEHEERDER** Voer het e-mailadres van de nieuwe Service-beheerder.

    ![Schermafbeelding van het selectievakje in om te wijzigen van het e-mailbericht servicebeheerder](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Beperkingen voor het wijzigen van servicebeheerders

* Elk abonnement is gekoppeld aan een Azure Active directory. Als u de map die het abonnement is gekoppeld aan zoekt, gaat u naar de [klassieke Azure-portal](https://manage.windowsazure.com/), selecteer **instellingen** > **abonnementen**. Controleer de abonnements-ID voor de map zoeken.
* Als u bent aangemeld bij werk of School-account, kunt u andere accounts in uw organisatie als servicebeheerder kunt toevoegen. Bijvoorbeeld: abby@contoso.com kunt toevoegen bob@contoso.com als servicebeheerder kan niet worden toegevoegd, maar john@notcontoso.com tenzij john@notcontoso.com aanwezigheid heeft in de map contoso.com. Gebruikers die zijn aangemeld met werk of School accounts kunnen worden voortgezet als servicebeheerder voor gebruikers van Microsoft-Account toevoegen.

  | Aanmeldingsmethode | Microsoft-Account gebruiker toevoegen als SA? | Account voor werk of School binnen dezelfde organisatie als SA toevoegen? | Werk of School-account in een andere organisatie als SA toevoegen? |
  | --- | --- | --- | --- |
  |  Microsoft-account |Ja |Nee |Nee |
  |  Werk of Schoolaccount |Ja |Ja |Nee |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>De accountbeheerder voor een Azure-abonnement wijzigen

Zie het wijzigen van de accountbeheerder van een abonnement [eigendom van een Azure-abonnement overdragen aan een ander account](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Weet u niet zeker wie de accountbeheerder is?** Volg deze stappen:

1. Aanmelden bij de [abonnementen weergeven in de Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren, en ga vervolgens naar **instellingen**.
1. Selecteer **eigenschappen**. De accountbeheerder van het abonnement wordt weergegeven in de **accountbeheerder** vak.  

## <a name="types-of-azure-admin-accounts"></a>Typen Azure beheerdersaccounts

 Accountbeheerder servicebeheerder en medebeheerder zijn de drie soorten beheerdersrollen in Microsoft Azure. De volgende tabel beschrijft het verschil tussen deze drie beheerdersrollen.

| Administratieve rol | Limiet | Beschrijving |
| --- | --- | --- |
| De accountbeheerder (AA) |1 per Azure-account |Dit is de persoon die zich heeft aangemeld of Azure-abonnementen hebt gekocht en is geautoriseerd voor toegang tot de [Accountcentrum](https://account.azure.com/Subscriptions) en verschillende beheertaken uit te voeren. Hierbij kunnen maken van abonnementen, abonnementen annuleren, de facturering voor een abonnement te wijzigen en wijzigen van de servicebeheerder. |
| Servicebeheerder (SA) |1 per Azure-abonnement |Deze rol is geautoriseerd voor het beheren van services in de [Azure-portal](https://portal.azure.com). De accountbeheerder is voor een nieuw abonnement wordt standaard ook de servicebeheerder. |
| Medebeheerder (CA) in de [klassieke Azure-portal](https://manage.windowsazure.com) |200 per abonnement |Deze rol heeft dezelfde toegangsrechten als de Servicebeheerder, maar kan de koppeling van abonnementen aan Azure-directory's niet wijzigen. |

Azure Active Directory-rol gebaseerde toegangsbeheer (RBAC) kunnen gebruikers worden toegevoegd aan meerdere rollen. Zie voor meer informatie [toegangsbeheer op basis van een functie van Azure Active Directory](../active-directory/role-based-access-control-configure.md).


## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Meer informatie over toegangsbeheer voor resource en Active Directory

* Zie voor meer informatie over hoe de toegang tot bronnen in Microsoft Azure wordt beheerd, [informatie over toegang tot bronnen in Azure](../active-directory/active-directory-understanding-resource-access.md).
* Zie voor meer informatie over Azure Active Directory, [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) en [beheerdersrollen toewijzen in Azure Active Directory](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
