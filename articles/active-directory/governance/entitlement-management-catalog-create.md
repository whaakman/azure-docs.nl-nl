---
title: Een catalogus maken en beheren in azure AD rechten beheer (preview)-Azure Active Directory
description: Meer informatie over het maken van een nieuwe container met resources en toegangs pakketten in Azure Active Directory rechten beheer (preview).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: HANKI
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e563d86abe3817e4c77cc0d5c8df928e41563f8
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489076"
---
# <a name="create-and-manage-a-catalog-in-azure-ad-entitlement-management-preview"></a>Een catalogus maken en beheren in azure AD-rechts beheer (preview-versie)

> [!IMPORTANT]
> Azure Active Directory (Azure AD)-rechts beheer is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="create-a-catalog"></a>Een catalogus maken

Een catalogus is een container met resources en toegangs pakketten. U maakt een catalogus wanneer u gerelateerde resources en toegangs pakketten wilt groeperen. Degene die de catalogus maakt, wordt de eerste eigenaar van de catalogus. Een catalogus eigenaar kan extra catalogus eigenaren toevoegen.

**Vereiste rol:** Gebruikers beheerder of catalogus Maker

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Klik op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het menu links op **catalogi**.

    ![Rechten beheer catalogi in de Azure Portal](./media/entitlement-management-catalog-create/catalogs.png)

1. Klik op **nieuwe catalogus**.

1. Voer een unieke naam in voor de catalogus en geef een beschrijving op.

    Gebruikers zien deze informatie in de details van een toegangs pakket.

1. Als u wilt dat de toegangs pakketten in deze catalogus beschikbaar zijn voor gebruikers om aan te vragen zodra ze zijn gemaakt, stelt u **ingeschakeld** in op **Ja**.

1. Als u wilt toestaan dat gebruikers in geselecteerde externe directory's toegangs pakketten kunnen aanvragen in deze catalogus, stelt u **ingeschakeld voor externe gebruikers** in op **Ja**.

    ![Deel venster nieuwe catalogus](./media/entitlement-management-catalog-create/new-catalog.png)

1. Klik op **maken** om de catalogus te maken.

## <a name="add-resources-to-a-catalog"></a>Resources toevoegen aan een catalogus

Als u resources wilt toevoegen aan een toegangs pakket, moeten de resources aanwezig zijn in een catalogus. De typen resources die u kunt toevoegen, zijn groepen, toepassingen en share point online-sites. De groepen kunnen bestaan uit in de Cloud gemaakte Office 365-groepen of in de Cloud gemaakte Azure AD-beveiligings groepen. De toepassingen kunnen Azure AD-bedrijfs toepassingen zijn, met inbegrip van zowel SaaS-toepassingen als uw eigen toepassingen die federatieve zijn voor Azure AD. De sites kunnen share point online-sites of share point online-site verzamelingen zijn.

**Vereiste rol:** De [vereiste rollen weer geven om resources toe te voegen aan een catalogus](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)

1. Klik in de Azure Portal op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus waaraan u resources wilt toevoegen.

1. Klik in het linkermenu op **resources**.

1. Klik op **resources toevoegen**.

1. Klik op een resource type: **Groepen**, **toepassingen**of **share point-sites**.

    Als u een resource die u wilt toevoegen niet ziet of u geen resource kunt toevoegen, moet u ervoor zorgen dat u beschikt over de vereiste functie voor Azure AD-Directory en rechten beheer. Mogelijk moet u iemand met de vereiste rollen hebben om de resource toe te voegen aan uw catalogus. Zie [vereiste rollen voor het toevoegen van resources aan een catalogus](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)voor meer informatie.

1. Selecteer een of meer resources van het type dat u wilt toevoegen aan de catalogus.

1. Wanneer u klaar bent, klikt u op **toevoegen**.

    Deze resources kunnen nu worden opgenomen in toegangs pakketten in de catalogus.

## <a name="remove-resources-from-a-catalog"></a>Resources uit een catalogus verwijderen

U kunt resources verwijderen uit een catalogus. Een resource kan alleen uit een catalogus worden verwijderd als deze niet wordt gebruikt in een van de toegangs pakketten van de catalogus.

**Vereiste rol:** De [vereiste rollen weer geven om resources toe te voegen aan een catalogus](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)

1. Klik in de Azure Portal op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus waaruit u resources wilt verwijderen.

1. Klik in het linkermenu op **resources**.

1. Selecteer de resources die u wilt verwijderen.

1. Klik op **verwijderen** (of klik op het weglatings teken ( **...** ) en klik vervolgens op **resource verwijderen**.

## <a name="edit-a-catalog"></a>Een catalogus bewerken

U kunt de naam en beschrijving voor een catalogus bewerken. Gebruikers zien deze informatie in de details van een toegangs pakket.

**Vereiste rol:** Gebruikers beheerder of catalogus eigenaar

1. Klik in de Azure Portal op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt bewerken.

1. Klik op de pagina **overzicht** van de catalogus op **bewerken**.

1. Bewerk de naam of beschrijving van de catalogus.

1. Klik op **Opslaan**.

## <a name="delete-a-catalog"></a>Een catalogus verwijderen

U kunt een catalogus verwijderen, maar alleen als deze geen toegangs pakketten heeft.

**Vereiste rol:** Gebruikers beheerder of catalogus eigenaar

1. Klik in de Azure Portal op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt verwijderen.

1. Klik op **verwijderen**in het **overzicht**van de catalogus.

1. Klik op **Ja**in het bericht venster dat wordt weer gegeven.

## <a name="next-steps"></a>Volgende stappen

- [Een maker van de catalogus toevoegen](entitlement-management-delegate.md#add-a-catalog-creator)
- [Een toegangs pakket maken en beheren](entitlement-management-access-package-create.md)
