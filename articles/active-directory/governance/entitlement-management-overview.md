---
title: Wat is Azure AD recht management? (Preview) - Azure Active Directory
description: Bekijk een overzicht van Azure Active Directory waar u recht op beheer en hoe u deze kunt gebruiken voor het beheren van toegang tot groepen, toepassingen en SharePoint Online-sites voor interne en externe gebruikers.
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
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: efd3ff8a6e7ddf2aa6242cc322d8a6536a6bd26b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66474061"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>Wat is Azure AD recht management? (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Werknemers in organisaties hebben toegang tot verschillende groepen, toepassingen en websites om uit te voeren van hun werk nodig. Deze toegang beheren met zich mee. In de meeste gevallen is er geen geordende lijst van alle resources die een gebruiker nodig voor een project heeft. Door de projectmanager is een goed begrip van de resources die nodig zijn, de personen die betrokken zijn en hoe lang het project duurt. Echter, door de projectmanager doorgaans heeft geen machtigingen voor het goedkeuren of verlenen van toegang tot andere. In dit scenario wordt meer gecompliceerde wanneer u probeert te werken met externe personen of bedrijven.

Beheer van de rechten van een Azure Active Directory (Azure AD) kunt u toegang tot groepen, toepassingen en SharePoint Online-sites voor interne gebruikers en ook gebruikers buiten uw organisatie beheren.

## <a name="why-use-entitlement-management"></a>Waarom gebruiken waar u recht op management?

Enterprise-organisaties wordt vaak uitdagingen bij het beheren van toegang tot resources, zoals:

- Gebruikers kunnen niet weten wat ze moeten hebben toegang
- Gebruikers hebben mogelijk problemen bij het zoeken naar de juiste personen of de juiste resources
- Zodra gebruikers vinden en toegang tot een resource krijgen, kunnen ze hebben op voor toegang tot meer dan nodig is voor zakelijke doeleinden

Deze problemen worden samengesteld voor gebruikers die toegang vanuit een andere map, zoals externe gebruikers die afkomstig van supply chain organisaties of andere zakelijke partners zijn nodig hebben. Bijvoorbeeld:

- Organisaties kunnen niet alle van de specifieke personen in andere directory's kunnen uitnodigen ze kent
- Zelfs als organisaties deze gebruikers uitnodigen zijn, kunnen organisaties niet onthouden alle van de gebruiker toegang consistent beheren

Beheer van de rechten van een Azure AD kunt deze problemen aan te pakken.

## <a name="what-can-i-do-with-entitlement-management"></a>Wat kan ik doen met rechten management?

Hier volgen enkele van de beheermogelijkheden van waar u recht op:

- Pakketten van gerelateerde resources die gebruikers kunnen aanvragen maken
- Definieer regels voor informatie over aanvragen voor resources en wanneer toegang is verlopen
- De levenscyclus van toegang voor zowel interne als externe gebruikers beheren
- Beheer van resources delegeren
- Fiatteurs verzoeken goed te keuren aanwijzen
- Maak rapporten om bij te houden van geschiedenis

Bekijk de volgende video van de 2018 Ignite-Conferentie voor een overzicht van identiteitsbeheer en het beheer van rechten:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>Welke bronnen kan ik beheren?

Dit zijn de typen resources die u toegang tot met beheer van rechten kunt beheren:

- Azure AD-beveiligingsgroepen
- Office 365-groepen
- Azure AD bedrijfstoepassingen, inclusief SaaS-toepassing en geïntegreerd met aangepaste toepassingen die ondersteuning bieden voor Federatie of inrichten
- SharePoint Online-siteverzamelingen en sites

U kunt ook toegang tot andere resources die afhankelijk van Azure AD-beveiligingsgroepen of Office 365-groepen zijn beheren.  Bijvoorbeeld:

- U kunt gebruikerslicenties voor Microsoft Office 365 geven met behulp van een Azure AD-beveiligingsgroep in een pakket toegang en configureren van [Groepslicenties](../users-groups-roles/licensing-groups-assign.md) voor die groep
- U kunt gebruikers toegang geven tot Azure-resources beheren met behulp van een Azure AD-beveiligingsgroep in een pakket toegang en het maken van een [Azure roltoewijzing](../../role-based-access-control/role-assignments-portal.md) voor die groep

## <a name="what-are-access-packages-and-policies"></a>Wat zijn de toegang tot pakketten en -beleid?

Recht management introduceert het concept van een *toegang pakket*. Een pakket toegang is een bundel van alle resources die een gebruiker nodig heeft om te werken aan een project of hun taak uit te voeren. De resources hebben toegang tot groepen, toepassingen of websites. Toegang tot pakketten worden gebruikt om te bepalen de toegang voor uw interne werknemers en ook gebruikers buiten uw organisatie. Toegang tot pakketten zijn gedefinieerd in containers met de naam *catalogi*.

Toegang tot pakketten bevatten ook een of meer *beleid*. Een beleid definieert de regels of guardrails voor toegang tot een package toegang. Als u een beleid wordt afgedwongen dat alleen de juiste gebruikers toegang, met de juiste resources en voor de juiste hoeveelheid tijd worden verleend.

![Access-pakket en het beleid](./media/entitlement-management-overview/elm-overview-access-package.png)

Met een access-pakket en het beleid, wordt de toegang tot package manager gedefinieerd:

- Resources
- Rollen de gebruikers nodig hebt voor de resources
- Interne gebruikers en externe gebruikers die in aanmerking komen om toegang te vragen
- Goedkeuringsproces en de gebruikers die u kunnen goedkeuren of weigeren van toegang
- Duur van de toegang van gebruiker

Het volgende diagram toont een voorbeeld van de verschillende elementen in het beheer van rechten. Hier ziet u twee voorbeeld toegang tot pakketten.

- **Toegang tot pakket, 1** bevat één groep als een resource. Toegang wordt gedefinieerd met een beleid waarmee een set gebruikers in de map om toegang te vragen.
- **Toegang tot het pakket 2** bestaat uit een groep, een toepassing en een SharePoint Online-site als resources. Toegang wordt gedefinieerd met twee verschillende soorten beleid. Het eerste beleid kunt een groep gebruikers in de map om toegang te vragen. De tweede beleidsregel kan gebruikers in een externe directory om toegang te vragen.

![Overzicht van rechten](./media/entitlement-management-overview/elm-overview.png)

## <a name="external-users"></a>Externe gebruikers

Wanneer u de [Azure AD business-to-business (B2B)](../b2b/what-is-b2b.md) uitnodigen ervaring, moet u het e-mailadressen van de externe gastgebruikers die u wilt voldoen aan de directory van uw resource en werken met al weten. Dit werkt prima als u aan een project kleiner of op korte termijn werkt en u alle deelnemers al kent, maar dit is het moeilijker om te beheren, hebt u veel gebruikers die u werken wilt met of als de deelnemers verloop van tijd veranderen.  Bijvoorbeeld, u mogelijk werken met een andere organisatie en één punt van contact met deze organisatie hebt, maar het na verloop van tijd meer gebruikers van deze organisatie ook toegang nodig voor.

U kunt een beleid waarmee gebruikers van organisaties die u opgeeft, die ook van Azure AD gebruikmaken om aanvragen van een pakket toegang te kunnen definiëren met beheer van rechten. U kunt opgeven of de goedkeuring is vereist en een vervaldatum voor toegang tot de. Als goedkeuring vereist is, kunt u ook opgeven als een goedkeurder een of meer gebruikers van de externe organisatie die u eerder hebt uitgenodigd - omdat ze waarschijnlijk om te weten welke externe gebruikers van hun organisatie toegang nodig hebben. Nadat u het pakket toegang hebt geconfigureerd, kunt u een koppeling naar het pakket toegang verzenden naar de contactpersoon van de externe organisatie. Deze contactpersoon in de externe organisatie kunt delen met andere gebruikers en ze kunnen deze koppeling gebruiken om aan te vragen van de access-pakket.  Gebruikers van die organisatie die al in uw directory zijn uitgenodigd kunnen die koppeling ook gebruiken.

Wanneer een aanvraag is goedgekeurd, wordt de gebruiker met de benodigde toegang, waaronder de gebruiker uitnodigen als ze niet al in uw directory inrichten in beheer van rechten. Azure AD maakt automatisch een B2B-account voor hen.  Houd er rekening mee dat een beheerder eerder welke organisaties zijn toegestaan voor samenwerking beperkte mogelijk, door in te stellen een [B2B toestaan of weigeren van lijst](../b2b/allow-deny-list.md) uitnodigingen toestaan of blokkeren op andere organisaties.  Als de gebruiker is niet toegestaan door de lijst met toegestane of geblokkeerde, en vervolgens worden ze niet worden uitgenodigd.

Omdat u niet dat de externe gebruiker toegang wilt tot voorgoed, geeft u een vervaldatum in het beleid, zoals 180 dagen. Na 180 dagen als u de toegang niet verlengt, beheer van rechten wordt alle toegang verwijderen die zijn gekoppeld aan dit pakket toegang.  Als de gebruiker die is uitgenodigd via beheer van rechten geen andere pakket toegangstoewijzingen heeft, vervolgens wanneer zij hun laatste toewijzing verliezen hun B2B account wordt geen toegang tot aanmelding voor 30 dagen, en vervolgens verwijderd.  Dit voorkomt dat de toename van onnodige accounts.  

## <a name="terminology"></a>Terminologie

Voor meer informatie over waar u recht op beheer en de bijbehorende documentatie, moet u de volgende voorwaarden controleren.

| Term of concept | Description |
| --- | --- |
| beheer van rechten | Een service die wordt toegewezen, trekt en beheert de toegang tot pakketten. |
| Access-pakket | Een verzameling van machtigingen en het beleid in de bronnen die gebruikers kunnen aanvragen. Een pakket toegang altijd zich in een catalogus. |
| aanvraag voor toegang tot | Een aanvraag voor toegang tot een package toegang. Een aanvraag wordt gewoonlijk verwerkt via een werkstroom. |
| policy | Een reeks regels waarin de levenscyclus van toegang, zoals hoe gebruikers toegang krijgen, die kunnen goedkeuren en hoe lang gebruikers toegang hebben. Voorbeelden van beleidsregels zijn toegang van werknemers en externe toegang. |
| catalog | Een container met gerelateerde resources en toegang tot pakketten. |
| Algemene catalogus | Een ingebouwde catalogus die altijd beschikbaar is. U kunt resources toevoegen aan de algemene catalogus nodig bepaalde machtigingen. |
| resource | Een asset of service (zoals een groep, toepassing of site) die een gebruiker kan machtigingen worden verleend. |
| Resourcetype | Het type resource, met inbegrip van groepen, toepassingen en SharePoint Online-sites. |
| resource-rollen | Een verzameling van de machtigingen die zijn gekoppeld aan een resource. |
| resource-map | Een map met een of meer resources te delen. |
| toegewezen gebruikers | Een toewijzing van een pakket toegang aan een gebruiker of groep. |
| Inschakelen | Het proces van het maken van een access-pakket beschikbaar voor gebruikers om aan te vragen. |

## <a name="roles-and-permissions"></a>Rollen en machtigingen

Beheer van rechten heeft op basis van functie met verschillende rollen.

| Rol | Description |
| --- | --- |
| [Gebruikerbeheerder](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) | Alle aspecten van waar u recht op management beheren.<br/>Gebruikers en groepen maken. |
| Maker van de catalogus | Maken en beheren van catalogussen. Normaal gesproken een IT-beheerder of resource-eigenaar. De persoon die u een catalogus automatisch maakt wordt het eerste cataloguseigenaar van de catalogus. |
| De eigenaar van catalogus | Bewerken en beheren van bestaande catalogussen. Normaal gesproken een IT-beheerder of resource-eigenaar. |
| Toegang tot package manager | Bewerken en beheren van alle bestaande toegang pakketten in een catalogus. |
| Goedkeurder | Aanvragen voor toegang tot pakketten goedkeuren. |
| Aanvrager | Aanvragen van toegang tot pakketten. |

De volgende tabel bevat de machtigingen voor elk van deze rollen.

| Taak | Gebruikerbeheerder | Maker van de catalogus | De eigenaar van catalogus | Toegang tot package manager | Goedkeurder |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Maak een nieuwe toegang-pakket in de algemene catalogus](entitlement-management-access-package-create.md) | :heavy_check_mark: |  :heavy_check_mark: |  |  |  |
| [Maak een nieuwe toegang-pakket in een catalogus](entitlement-management-access-package-create.md) | :heavy_check_mark: |   | :heavy_check_mark: |  |  |
| [Resource-rollen naar/van een pakket toegang toevoegen/verwijderen](entitlement-management-access-package-edit.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Opgeven wie een pakket toegang kunnen vragen](entitlement-management-access-package-edit.md#add-a-new-policy) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een gebruiker rechtstreeks toewijzen aan een pakket toegang](entitlement-management-access-package-edit.md#directly-assign-a-user) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Weergave die een toewijzing aan een pakket toegang heeft](entitlement-management-access-package-edit.md#view-who-has-an-assignment) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een access-pakket aanvragen weergeven](entitlement-management-access-package-edit.md#view-requests) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een aanvraag voor de levering van fouten weergeven](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een aanvraag in behandeling annuleren](entitlement-management-access-package-edit.md#cancel-a-pending-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een pakket toegang verbergen](entitlement-management-access-package-edit.md#change-the-hidden-setting) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een pakket toegang verwijderen](entitlement-management-access-package-edit.md#delete) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Een aanvraag voor toegang tot goedkeuren](entitlement-management-request-approve.md) |  |  |  |  | :heavy_check_mark: |
| [Een catalogusitemgroep maken](entitlement-management-catalog-create.md) | :heavy_check_mark: | :heavy_check_mark: |  |  |  |
| [Resources van de algemene catalogus toevoegen/verwijderen](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  |  |  |  |
| [Resources uit een catalogus toevoegen/verwijderen](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Catalogus eigenaren toevoegen of toegang tot Pakketbeheer](entitlement-management-catalog-create.md#add-catalog-owners-or-access-package-managers) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Een catalogus voor bewerken/verwijderen](entitlement-management-catalog-create.md#edit-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |

## <a name="license-requirements"></a>Licentievereisten

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Gespecialiseerde clouds, zoals Azure Government, Azure Duitsland en Azure China 21Vianet, zijn momenteel niet beschikbaar voor gebruik in deze Preview-versie.

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Uw eerste toegang-pakket maken](entitlement-management-access-package-first.md)
- [Algemene scenario's](entitlement-management-scenarios.md)
