---
title: Taken in Azure AD-recht beheer (Preview) - Azure Active Directory delegeren
description: Meer informatie over de functies die u toewijzen kunt aan taken in Azure Active Directory waar u recht op beheer delegeren.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/07/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fa0be8e2af7644564ba27e6d58fda09b1ae7bc7
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191497"
---
# <a name="delegate-tasks-in-azure-ad-entitlement-management-preview"></a>Taken delegeren in Azure AD waar u recht op beheer (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Standaard kunnen globale beheerders en beheerders van gebruikers maken en beheren van alle aspecten van Azure AD waar u recht op. De gebruikers in deze rollen kunnen echter niet weet voor alle scenario's waarbij toegang tot pakketten vereist zijn. Dit is meestal gebruikers binnen de afdelingen die weten die nodig hebben om samen te werken.

In plaats van onbeperkte machtigingen verlenen aan de niet-beheerders, verleent u gebruikers de minimale machtigingen die ze nodig hebben voor hun taak uitvoeren en Vermijd het maken van conflicterende of onjuiste toegangsrechten. Dit artikel beschrijft de functies die u toewijzen kunt aan verschillende taken in waar u recht op beheer delegeren.

## <a name="delegate-example-for-departmental-adoption"></a>Voorbeeld van de gemachtigde voor afdelingen acceptatie

Het helpt om te begrijpen hoe u taken in het beheer van rechten kan delegeren, bekijk een voorbeeld. Stel dat uw organisatie heeft de volgende vijf gebruikers:

| Gebruiker | Afdeling | Opmerkingen |
| --- | --- | --- |
| Els | IT | Globale beheerder |
| Bob | Research | Bob is ook eigenaar van een onderzoek-groep |
| Carole | Research |  |
| Dave | Marketing |  |
| Elisa | Marketing | Elisa is ook eigenaar van een toepassing Marketing |

De onderzoeks- en marketingafdeling wilt gebruiken van beheer van rechten voor hun gebruikers. Els is nog niet gereed voor andere afdelingen de mogelijkheid om het beheer van rechten. Hier volgt één manier dat Alice kan taken delegeren aan de onderzoeks- en marketingafdeling.

1. Alice maakt u een nieuwe Azure AD-beveiligingsgroep groeperen voor makers van catalogus en Bob, Carola Dave en Elisa toegevoegd als leden van die groep.

1. Alice maakt gebruik van de instellingen voor rechten die groep toevoegen aan de rol van de makers van catalogus.

1. Carola maakt een **onderzoek** catalogus en Bob toegevoegd als co-eigenaar van deze catalogus. Bob voegt het onderzoek-groep hij eigenaar is van aan de catalogus als een resource, zodat deze kan worden gebruikt in een pakket toegang voor onderzoek samenwerking.

1. Dave maakt een **Marketing** catalogus en Elisa toegevoegd als co-eigenaar van deze catalogus. Elisa toegevoegd de marketing toepassing zij eigenaar is van aan de catalogus als een resource, zodat deze kan worden gebruikt in een pakket toegang voor samenwerking marketing.

Nu kunnen de onderzoeks- en marketing afdelingen gebruikmaken van beheer van rechten. Bob, Carola Dave en Elisa kunnen maken en beheren van toegang tot pakketten in hun respectieve catalogussen.

![Voorbeeld van waar u recht op beheer delegeren](./media/entitlement-management-delegate/elm-delegate.png)

## <a name="entitlement-management-roles"></a>Recht beheerrollen

Beheer van rechten heeft de volgende functies die specifiek voor het beheer van rechten zijn.

| Rol | Description |
| --- | --- |
| Maker van de catalogus | Maken en beheren van catalogussen. Normaal gesproken een IT-beheerder die geen globale beheerder of een resource-eigenaar voor een verzameling resources. De persoon die een catalogus automatisch worden gemaakt, wordt het eerste cataloguseigenaar van de catalogus en eigenaren van aanvullende catalogus kunt toevoegen. |
| De eigenaar van catalogus | Bewerken en beheren van bestaande catalogussen. Normaal gesproken een IT-beheerder of resource-eigenaren of een gebruiker die eigenaar van de catalogus is toegewezen. |
| Toegang tot package manager | Bewerken en beheren van alle bestaande toegang pakketten in een catalogus. |

Bovendien een aangewezen fiatteur en een aanvrager van een pakket toegang beschikt over andere rechten, hoewel ze geen rollen zijn.
 
* Fiatteur: Geautoriseerd door een beleid voor het goedkeuren of weigeren van aanvragen voor toegang tot pakketten, hoewel ze niet wijzigen de pakketdefinities toegang.
* Aanvrager: Geautoriseerd door een beleid van een pakket toegang om aan te vragen die toegang-pakket.

De volgende tabel bevat de taken die deze functies kunnen uitvoeren.

| Taak | Maker van de catalogus | De eigenaar van catalogus | Toegang tot package manager | Goedkeurder |
| --- | :---: | :---: | :---: | :---: |
| [Een nieuwe catalogus maken](entitlement-management-catalog-create.md) | :heavy_check_mark: |  |  |  |
| [Een resource toevoegen aan een catalogus](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | | :heavy_check_mark: | | |
| [Een catalogus bewerken](entitlement-management-catalog-create.md#edit-a-catalog) |  | :heavy_check_mark: |  |  |
| [Een catalogus verwijderen](entitlement-management-catalog-create.md#delete-a-catalog) |  | :heavy_check_mark: |  |  |
| [De eigenaar van een catalogus of een pakket access manager toevoegen aan een catalogus](#add-a-catalog-owner-or-an-access-package-manager) |  | :heavy_check_mark: |  |  |
| [Maak een nieuwe toegang-pakket in een catalogus](entitlement-management-access-package-create.md) |  | :heavy_check_mark: |  |  |
| [Resourcerollen in een pakket toegang beheren](entitlement-management-access-package-edit.md) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Opgeven wie een pakket toegang kunnen vragen](entitlement-management-access-package-edit.md#add-a-new-policy) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een gebruiker rechtstreeks toewijzen aan een pakket toegang](entitlement-management-access-package-edit.md#directly-assign-a-user) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Weergave die een toewijzing aan een pakket toegang heeft](entitlement-management-access-package-edit.md#view-who-has-an-assignment) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een access-pakket aanvragen weergeven](entitlement-management-access-package-edit.md#view-requests) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een aanvraag voor de levering van fouten weergeven](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een aanvraag in behandeling annuleren](entitlement-management-access-package-edit.md#cancel-a-pending-request) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een pakket toegang verbergen](entitlement-management-access-package-edit.md#change-the-hidden-setting) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een pakket toegang verwijderen](entitlement-management-access-package-edit.md#delete) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een aanvraag voor toegang tot goedkeuren](entitlement-management-request-approve.md) |  |  |  | :heavy_check_mark: |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Vereiste functies op bronnen toevoegen aan een catalogus

Een globale beheerder kunt toevoegen of verwijderen van een groep (gemaakt met een cloud-beveiligingsgroepen of Office 365-groepen cloud gemaakt), toepassing of SharePoint Online-site in een catalogus. De Gebruikersbeheerder van een kunt toevoegen of verwijderen van een groep of toepassing in een catalogus.

Voor een gebruiker die geen globale beheerder of Gebruikersbeheerder heeft, groepen, toepassingen of SharePoint Online-sites toevoegen aan een catalogus van die gebruiker moet hebben *beide* de Azure AD directory-rol en catalog eigenaar recht vereist van de beheerrol. De volgende tabel bevat de rolcombinaties die nodig zijn voor resources toevoegen aan een catalogus. Als resources wilt verwijderen uit een catalogus, moet u dezelfde rollen hebben.

| Azure AD-directory-rol | Recht beheerrol | Beveiligingsgroep kunt toevoegen | Office 365-groep kunt toevoegen | App kunt toevoegen | SharePoint Online-site kunt toevoegen |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Globale beheerder](../users-groups-roles/directory-assign-admin-roles.md) | N.v.t. |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Gebruikerbeheerder](../users-groups-roles/directory-assign-admin-roles.md) | N.v.t. |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Intune-beheerder](../users-groups-roles/directory-assign-admin-roles.md) | De eigenaar van catalogus | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Exchange-beheerder](../users-groups-roles/directory-assign-admin-roles.md) | De eigenaar van catalogus |  | :heavy_check_mark: |  |  |
| [Teams-servicebeheerder](../users-groups-roles/directory-assign-admin-roles.md) | De eigenaar van catalogus |  | :heavy_check_mark: |  |  |
| [SharePoint-beheerder](../users-groups-roles/directory-assign-admin-roles.md) | De eigenaar van catalogus |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Toepassingsbeheerder](../users-groups-roles/directory-assign-admin-roles.md) | De eigenaar van catalogus |  |  | :heavy_check_mark: |  |
| [Beheerder van de cloudtoepassing](../users-groups-roles/directory-assign-admin-roles.md) | De eigenaar van catalogus |  |  | :heavy_check_mark: |  |
| Gebruiker | De eigenaar van catalogus | Alleen als de eigenaar van groep | Alleen als de eigenaar van groep | Alleen als eigenaar van de app |  |

## <a name="add-a-catalog-creator"></a>Maker van de catalogus toevoegen

Als u delegeren catalogus worden gemaakt wilt, kunt u gebruikers toevoegen aan de rol van de maker van catalogus.  U kunt afzonderlijke gebruikers toevoegen of voor uw gemak bedoeld; een groep, waarvan de leden hebben dan catalogi maken kunt toevoegen. Volg deze stappen voor een gebruiker toewijzen aan de rol van de maker van catalogus.

**Vereiste rol:** Globale beheerder of Gebruikerbeheerder

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. In het menu links in de **recht management** sectie, klikt u op **instellingen**.

1. Klik op **Bewerken**.

1. In de **recht beheer delegeren** sectie, klikt u op **makers van de catalogus toevoegen** om te selecteren van de gebruikers of groepen die de leden voor deze rechten van een management-rol.

1. Klik op **Selecteren**.

1. Klik op **Opslaan**.

## <a name="add-a-catalog-owner-or-an-access-package-manager"></a>De eigenaar van een catalogus of een pakket access manager toevoegen

Als u delegeren van beheer van een catalogus of toegang tot pakketten in de catalogus wilt, kunt u gebruikers toevoegen aan de eigenaar van de catalogus of toegang package manager-rollen. Degene die een catalogus wordt gemaakt, wordt de eigenaar van de eerste catalogus. Volg deze stappen voor een gebruiker toewijzen aan de eigenaar van de catalogus of toegang package manager-rol.

**Vereiste rol:** Gebruikerbeheerder of eigenaar van catalogus

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **catalogi** en open vervolgens de catalogus die u wilt toevoegen van beheerders.

1. Klik in het menu links op **rollen en beheerders**.

1. Klik op **eigenaren toevoegen** of **Pakketbeheer toegang toevoegen** om de leden van deze rollen te selecteren.

1. Klik op **Selecteer** deze leden toevoegen.

## <a name="next-steps"></a>Volgende stappen

- [Fiatteurs toevoegen](entitlement-management-access-package-edit.md#policy-request)
- [Resources toevoegen aan een catalogus](entitlement-management-catalog-create.md#add-resources-to-a-catalog)
