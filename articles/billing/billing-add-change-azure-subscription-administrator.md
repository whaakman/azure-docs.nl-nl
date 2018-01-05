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
ms.date: 01/04/2018
ms.author: genli
ms.openlocfilehash: dc09f29fec78d408e1560bfa0a943f16ab50c760
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="add-or-change-azure-subscription-administrators"></a>Toevoegen of wijzigen van de beheerders van Azure-abonnement

Beheerders van de klassieke Azure-abonnement en Azure [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-what-is.md) zijn de twee systemen voor het beheren van toegang tot Azure-resources:

* Klassieke abonnement beheerdersrollen basic toegangsbeheer bieden en zijn accountbeheerder servicebeheerder en Medebeheerders.
    * Wanneer u zich voor een nieuwe Azure-abonnement aanmeldt, wordt uw account standaard ingesteld als de accountbeheerder en de servicebeheerder.
    * Medebeheerders kunnen worden toegevoegd na aanmelding.
* RBAC is een nieuwere systeem die Geavanceerd toegangsbeheer met veel ingebouwde rollen, de flexibiliteit van een bereik en aangepaste rollen biedt.
    * Gebruikers met alleen RBAC-rollen en geen beheerdersrollen klassieke abonnement kunnen echter niet Azure klassieke implementaties beheren.

Om ervoor te zorgen betere controle en voor het vereenvoudigen van beheer van toegang, wordt u aangeraden gebruikmaken van RBAC voor alle access management-behoeften. Indien mogelijk is het raadzaam dat u de configuratie van bestaande beleidsregels voor toegang met RBAC. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Een beheerder RBAC-eigenaar voor een abonnement toevoegen in Azure-portal 

Als u wilt toevoegen iemand als beheerder voor het beheer van de service Azure-abonnement, ze geven de rol van een RBAC eigenaar aan het abonnement. De rol van eigenaar kan de resources in het abonnement dat u toegewezen en heeft geen toegangsrechten naar andere abonnementen beheren.

1. Ga naar [ **abonnementen** in Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Selecteer het abonnement dat u toegang wilt geven.
3. Selecteer **toegangsbeheer (IAM)** in het menu.
4. In de **rol** de optie **eigenaar**. 
5. In de **toewijzen van toegang tot** de optie **Azure AD-gebruiker, groep of toepassing**. 
6. In de **Selecteer** typt u het e-mailadres van de gebruiker die u wilt toevoegen als eigenaar. Selecteer de gebruiker en selecteer vervolgens **opslaan**.

    ![Schermafbeelding van de rol van eigenaar geselecteerd](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Dit geeft de volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren. Ga naar het menu IAM voor deze scope om toegang aan een ander bereik, zoals een resourcegroep te verlenen. 

## <a name="add-or-change-co-administrator"></a>Toevoegen of wijzigen medebeheerder

Alleen een eigenaar kan worden toegevoegd als medebeheerder. Andere gebruikers met functies zoals Inzender en lezer kunnen niet worden toegevoegd als medebeheerders.

> [!TIP]
> Alleen moet u de 'Eigenaar'-account toevoegen als medebeheerder als de gebruiker voor het beheren van Azure klassieke implementaties. U wordt aangeraden met het RBAC voor alle andere doeleinden.

1. Als u nog niet gedaan hebt, kunt u iemand als eigenaar van een van bovenstaande instructies toevoegen.
2. **Met de rechtermuisknop op** de gebruiker de eigenaar u zojuist hebt toegevoegd, en selecteer vervolgens **toevoegen als medebeheerder**. Als u niet ziet de **toevoegen als medebeheerder** optie, de pagina vernieuwen of probeer een andere internetbrowser. 

    ![Schermafbeelding van medebeheerder voegt](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Verwijderen van de machtiging medebeheerder **met de rechtermuisknop op** 'CO-beheerder' gebruiker en selecteer vervolgens **medebeheerder verwijderen**.

    ![Schermopname die medebeheerder verwijdert](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>De servicebeheerder voor een Azure-abonnement wijzigen

Alleen de accountbeheerder kan de servicebeheerder voor een abonnement kunt wijzigen. Standaard, wanneer u zich aanmeldt, is de servicebeheerder hetzelfde als de accountbeheerder. Als de servicebeheerder is gewijzigd naar een andere gebruiker, verliest de accountbeheerder toegang tot Azure portal. De accountbeheerder kunt altijd Account Center gebruiken om te wijzigen van de servicebeheerder naar zichzelf.

1. Zorg ervoor dat uw scenario wordt ondersteund door het controleren van de [limieten voor het wijzigen van servicebeheerders](#limits).
1. Aanmelden bij [Accountcentrum](https://account.windowsazure.com/subscriptions) als accountbeheerder.
1. Selecteer een abonnement.
1. Selecteer aan de rechterkant **abonnementsgegevens bewerken**.

    ![Schermopname van de knop bewerken abonnement in Accountcentrum](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. In de **SERVICEBEHEERDER** Voer het e-mailadres van de nieuwe Service-beheerder.

    ![Schermafbeelding van het selectievakje in om te wijzigen van het e-mailbericht servicebeheerder](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Beperkingen voor het wijzigen van servicebeheerders

* Elk abonnement is gekoppeld aan een Azure Active directory. Als u de map die het abonnement is gekoppeld aan zoekt, gaat u naar [ **abonnementen**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecteert u een abonnement om te zien van de map.
* Als u bent aangemeld bij werk of School-account, kunt u andere accounts in uw organisatie als servicebeheerder kunt toevoegen. Bijvoorbeeld: abby@contoso.com kunt toevoegen bob@contoso.com als servicebeheerder kan niet worden toegevoegd, maar john@notcontoso.com tenzij john@notcontoso.com aanwezigheid heeft in de map contoso.com. Gebruikers die zijn aangemeld met werk of School accounts kunnen worden voortgezet als servicebeheerder voor gebruikers van Microsoft-Account toevoegen.

  | Aanmeldingsmethode | Microsoft-Account gebruiker toevoegen als SA? | Account voor werk of School binnen dezelfde organisatie als SA toevoegen? | Werk of School-account in een andere organisatie als SA toevoegen? |
  | --- | --- | --- | --- |
  |  Microsoft-account |Ja |Nee |Nee |
  |  Werk of Schoolaccount |Ja |Ja |Nee |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>De accountbeheerder voor een Azure-abonnement wijzigen

De accountbeheerder is de gebruiker die in eerste instantie aangemeld voor het Azure-abonnement en is verantwoordelijk als eigenaar van het abonnement van de facturering. Zie het wijzigen van de accountbeheerder van een abonnement [eigendom van een Azure-abonnement overdragen aan een ander account](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Weet u niet zeker wie de accountbeheerder is?** Volg deze stappen:

1. Ga naar [ **abonnementen** in Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren, en ga vervolgens naar **instellingen**.
1. Selecteer **eigenschappen**. De accountbeheerder van het abonnement wordt weergegeven in de **accountbeheerder** vak.  

## <a name="types-of-classic-subscription-admins"></a>Klassieke abonnementsbeheerders typen

 Accountbeheerder servicebeheerder en medebeheerder zijn de drie soorten beheerdersrollen klassieke abonnement in Azure. Het account dat wordt gebruikt om u te registreren voor Azure wordt automatisch ingesteld als de accountbeheerder en de servicebeheerder. Aanvullende Medebeheerders kunnen vervolgens worden toegevoegd. De volgende tabel beschrijft de exacte verschillen tussen deze drie beheerdersrollen. 

> [!TIP]
> Voor een betere controle en beheer van fijnmazig toegang, wordt u aangeraden Azure rollen gebaseerd toegangsbeheer (RBAC), waarmee gebruikers worden toegevoegd aan meerdere rollen. Zie voor meer informatie, [toegangsbeheer op basis van een functie van Azure Active Directory](../active-directory/role-based-access-control-what-is.md).

| Klassiek abonnement beheerder | Limiet | Beschrijving |
| --- | --- | --- |
| De accountbeheerder (AA) |1 per Azure-account |Dit is de gebruiker die heeft aangemeld voor het Azure-abonnement en is geautoriseerd voor toegang tot de [Accountcentrum](https://account.azure.com/Subscriptions) en verschillende beheertaken uit te voeren. Hierbij kunnen maken van nieuwe abonnementen, abonnementen annuleren, de facturering voor een abonnement te wijzigen en wijzigen van de servicebeheerder. De accountbeheerder is conceptueel gezien de facturering eigenaar van het abonnement. De accountbeheerder is niet in RBAC, een rol worden toegewezen.|
| Servicebeheerder (SA) |1 per Azure-abonnement |Deze rol is geautoriseerd voor het beheren van services in de [Azure-portal](https://portal.azure.com). De accountbeheerder is voor een nieuw abonnement wordt standaard ook de servicebeheerder. De rol van eigenaar in RBAC krijgt de servicebeheerder voor het abonnementsbereik.|
| Medebeheerder (CA) |200 per abonnement |Deze rol heeft dezelfde toegangsrechten als de Servicebeheerder, maar kan de koppeling van abonnementen aan Azure-directory's niet wijzigen. Bij RBAC, wordt de rol van eigenaar gegeven aan de CO-beheerder bij het abonnementsbereik.|

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Meer informatie over toegangsbeheer voor resource en Active Directory

* Zie voor meer informatie over hoe de toegang tot bronnen in Microsoft Azure wordt beheerd, [informatie over toegang tot bronnen in Azure](../active-directory/active-directory-understanding-resource-access.md).
* Zie voor meer informatie over Azure Active Directory, [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) en [beheerdersrollen toewijzen in Azure Active Directory](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
