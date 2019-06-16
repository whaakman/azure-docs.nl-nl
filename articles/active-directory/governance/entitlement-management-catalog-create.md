---
title: Maken en beheren van een catalogus in Azure AD-recht beheer (Preview) - Azure Active Directory
description: Informatie over het maken van een nieuwe container van resources en toegang tot pakketten in beheer van de rechten van een Azure Active Directory (Preview).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: HANKI
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6d9220cd2162b4c8cb77c1e7abd0372052f5454
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64541613"
---
# <a name="create-and-manage-a-catalog-in-azure-ad-entitlement-management-preview"></a>Maken en beheren van een catalogus in Azure AD waar u recht op beheer (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="create-a-catalog"></a>Een catalogus maken

Een catalogus is een container van resources en toegang tot pakketten. U kunt een catalogus maken als u wilt groeperen van gerelateerde resources en toegang tot pakketten. Degene die de catalogus wordt gemaakt, wordt de eigenaar van de eerste catalogus. De eigenaar van een catalogus kunt eigenaren van aanvullende catalogussen toevoegen.

**Vereiste rol:** Gebruikerbeheerder of maker van de catalogus

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Klik op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **catalogi**.

    ![Recht management catalogi in Azure portal](./media/entitlement-management-catalog-create/catalogs.png)

1. Klik op **nieuwe catalogus**.

1. Voer een unieke naam voor de catalogus en geef een beschrijving.

    Gebruikers zien deze informatie in details van een access-pakket.

1. Als u wilt dat de toegang tot pakketten in deze catalogus moet beschikbaar zijn voor gebruikers om aan te vragen, zodra ze zijn gemaakt, stelt u **ingeschakeld** naar **Ja**.

1. Als u toestaan dat gebruikers in geselecteerde externe mappen wilt kunnen aanvragen van toegang tot pakketten in deze catalogus, stelt u de **ingeschakeld voor externe gebruikers** naar **Ja**.

    ![Nieuwe catalogus deelvenster](./media/entitlement-management-catalog-create/new-catalog.png)

1. Klik op **maken** om de catalogus te maken.

## <a name="add-resources-to-a-catalog"></a>Resources toevoegen aan een catalogus

Zodat resources in een pakket toegang moeten de resources zich in een catalogus. De typen resources die u kunt toevoegen, zijn groepen, toepassingen en SharePoint Online-sites.

**Vereiste rol:** Gebruikerbeheerder of eigenaar van catalogus

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt toevoegen van resources.

1. Klik in het menu links op **Resources**.

1. Klik op **resources toevoegen**.

1. Klik op een resourcetype: **Groepen**, **toepassingen**, of **SharePoint-sites**.

    Als u de maker van de catalogus bent, kunt u een Office 365-groep of Azure AD-beveiligingsgroep eigenaar van uw catalogus kunt toevoegen. Als er is een groep die u wilt toewijzen aan gebruikers, maar u geen eigenaar van de groep, moet u dat de Gebruikersbeheerder van een die groep toevoegen aan de catalogus.

    Als u de maker van de catalogus bent, kunt u een Azure AD-bedrijfstoepassing u eigenaar bent, met inbegrip van zowel SaaS-toepassingen en uw eigen toepassingen die zijn gefedereerd met Azure AD, in uw catalogus kunt toevoegen. Als er een toepassing die u wilt toewijzen van gebruikers, maar niet de eigenaar bent, moet u de Gebruikersbeheerder van een die toepassing toevoegen aan uw catalogus hebt. Zodra de toepassing deel van de catalogus uitmaakt, kunt u een van de rollen van de toepassing selecteren in een pakket toegang.

1. Selecteer een of meer resources van het type dat u wilt toevoegen aan de catalogus.

1. Wanneer u klaar bent, klikt u op **toevoegen**.

    Deze resources kunnen nu worden opgenomen in de toegang tot pakketten in de catalogus.

## <a name="remove-resources-from-a-catalog"></a>Resources uit een catalogus verwijderen

U kunt resources verwijderen uit een catalogus. Een resource kan alleen worden verwijderd uit een catalogus als deze niet wordt gebruikt in een van de access-pakketten van de catalogus.

**Vereiste rol:** Gebruikerbeheerder of eigenaar van catalogus

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt verwijderen van resources uit.

1. Klik in het menu links op **Resources**.

1. Selecteer de resources die u wilt verwijderen.

1. Klik op **verwijderen** (of klik op het weglatingsteken ( **...** ) en klik vervolgens op **resource verwijderen**).

## <a name="add-catalog-owners-or-access-package-managers"></a>Catalogus eigenaren toevoegen of toegang tot Pakketbeheer

Als u delegeren van beheer van de catalogus of de toegang tot pakketten in de catalogus wilt, kunt u catalogus eigenaren toevoegen of toegang tot Pakketbeheer. Degene die een catalogus wordt gemaakt, wordt de eigenaar van de eerste catalogus.

**Vereiste rol:** Gebruikerbeheerder of eigenaar van catalogus

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt toevoegen van beheerders.

1. Klik in het menu links op **rollen en beheerders**.

1. Klik op **eigenaren toevoegen** of **Pakketbeheer toegang toevoegen** om de leden van deze rollen te selecteren.

1. Klik op **Selecteer** deze leden toevoegen.

## <a name="edit-a-catalog"></a>Een catalogus bewerken

U kunt de naam en beschrijving voor een catalogus bewerken. Gebruikers zien deze informatie in details van een access-pakket.

**Vereiste rol:** Gebruikerbeheerder of eigenaar van catalogus

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt bewerken.

1. Op van de catalogus **overzicht** pagina, klikt u op **bewerken**.

1. Bewerk de naam of beschrijving van de catalogus.

1. Klik op **Opslaan**.

## <a name="delete-a-catalog"></a>Een catalogus verwijderen

U kunt een catalogus, maar alleen verwijderen als er geen er toegang tot pakketten.

**Vereiste rol:** Gebruikerbeheerder of eigenaar van catalogus

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt verwijderen.

1. Op van de catalogus **overzicht**, klikt u op **verwijderen**.

1. In het vak van het bericht dat wordt weergegeven, klikt u op **Ja**.

## <a name="next-steps"></a>Volgende stappen

- [Maken en beheren van een access-pakket](entitlement-management-access-package-create.md)
