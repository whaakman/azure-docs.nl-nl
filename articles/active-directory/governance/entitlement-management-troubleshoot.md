---
title: Problemen met Azure AD recht management (Preview) - Azure Active Directory oplossen
description: Meer informatie over sommige items dat moet u controleren bij het beheer van de rechten van een Azure Active Directory (Preview) oplossen.
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
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2526ef10c3080dae1b32881a109a9436a0fd390
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473822"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Problemen met Azure AD recht management (Preview) oplossen

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Dit artikel beschrijft enkele items die u moet controleren om te helpen bij het oplossen van Azure Active Directory (Azure AD) waar u recht op beheer.

## <a name="checklist-for-entitlement-management-administration"></a>Controlelijst voor het beheer van rechten

* Als u een bericht toegang geweigerd bij het configureren van beheer van rechten en u een globale beheerder bent, zorgt u ervoor dat uw directory heeft een [licentie voor Azure AD Premium P2 (of EMS E5)](entitlement-management-overview.md#license-requirements).  
* Als u een toegang geweigerd bericht krijgt bij het maken of weergeven, toegang tot pakketten, en u lid zijn van een Catalogusgroep Maker, moet u een catalogus vóór het maken van uw eerste toegang-pakket maken.

## <a name="checklist-for-adding-a-resource"></a>Controlelijst voor het toevoegen van een resource

* Voor een toepassing moet een resource in een pakket toegang moet hebben ten minste één resourcerol die kan worden toegewezen. De rollen die zijn gedefinieerd door de toepassing zelf, en worden beheerd in Azure AD. Houd er rekening mee dat door de Azure-portal ook service-principals voor services die niet worden geselecteerd als toepassingen kan worden weergegeven.  In het bijzonder **Exchange Online** en **SharePoint Online** Services, zijn geen toepassingen waarvoor resource-rollen in de directory, zodat ze in een pakket toegang kunnen niet worden opgenomen.  Gebruik in plaats daarvan Groepslicenties tot stand brengen van een juiste licentie voor een gebruiker die toegang tot die services nodig heeft.

* Voor een groep moet een resource in een pakket toegang, moet het mogelijk zijn om te worden gewijzigd in Azure AD.  Groepen die afkomstig uit een on-premises Active Directory zijn kunnen niet als resources worden toegewezen omdat de eigenaar of lidkenmerken kunnen niet worden gewijzigd in Azure AD.  

* SharePoint Online-documentbibliotheken en afzonderlijke documenten kunnen niet worden toegevoegd als resources.  In plaats daarvan een Azure AD-beveiligingsgroep maken, die groep en een siterol opnemen in het pakket toegang en in SharePoint Online kunt u die groep gebruiken voor het beheren van toegang tot de documentbibliotheek of het document.

* Als er gebruikers die al zijn toegewezen aan een resource die u wilt beheren met een pakket toegang zijn, moet u dat de gebruikers zijn toegewezen aan het pakket met toegang tot een juiste beleid. U wilt bijvoorbeeld een groep opnemen in een access-pakket dat al gebruikers in de groep. Als de gebruikers in de groep vereisen toegang blijvende, moeten ze een juiste beleid voor de toegang tot pakketten hebben zodat ze hun toegang aan de groep houden blijven. Door een van beide waarin wordt gevraagd de gebruikers om aan te vragen van de access-pakket met die bron, of door ze rechtstreeks toe te wijzen aan het pakket toegang, kunt u het pakket toegang toewijzen. Zie voor meer informatie, [bewerken en beheren van een bestaand pakket met toegang tot](entitlement-management-access-package-edit.md).

## <a name="checklist-for-providing-external-users-access"></a>Controlelijst voor externe gebruikers toegang verlenen

* Als er een B2B [acceptatielijst](../b2b/allow-deny-list.md), en vervolgens gebruikers waarvan mappen zijn niet toegestaan niet mogelijk om toegang te vragen.

* Zorg ervoor dat er geen [beleid voor voorwaardelijke toegang](../conditional-access/require-managed-devices.md) die zou voorkomen dat externe gebruikers kunnen aanvragen van toegang of de mogelijkheid voor het gebruik van de toepassingen in de access-pakketten.

## <a name="checklist-for-request-issues"></a>Controlelijst voor aanvraag-problemen

* Wanneer een gebruiker wil de toegang tot een access-pakket, zorg ervoor dat ze gebruiken de **koppeling naar de mijn toegang** voor het pakket toegang. Zie voor meer informatie, [koppeling naar de kopie mijn toegang](entitlement-management-access-package-edit.md#copy-my-access-portal-link).

* Wanneer een gebruiker zich aanmeldt bij de portal mijn toegang om aan te vragen van een pakket toegang, moet u verificatie met behulp van hun organisatie-account. De organisatie-account mag een account in de resource-map of in een map die is opgenomen in een van de beleidsregels van het pakket toegang. Als het gebruikersaccount niet een organisatie-account is of de map niet in het beleid opgenomen is, klikt u vervolgens de gebruiker ziet niet het pakket toegang. Zie voor meer informatie, [aanvragen van toegang tot een package toegang](entitlement-management-request-access.md).

* Als een gebruiker is geblokkeerd bij de resource-map, is ze niet mogelijk om toegang in de portal mijn toegang te vragen. Voordat de gebruiker toegang aanvragen kan, moet u het blok aanmelden verwijderen uit het profiel van de gebruiker. Als u wilt verwijderen van het blok aanmelden in Azure portal, klikt u op **Azure Active Directory**, klikt u op **gebruikers**, klikt u op de gebruiker en klik vervolgens op **profiel**. Bewerken de **instellingen** sectie en wijzig **aanmelden blokkeren** naar **Nee**. Zie voor meer informatie, [toevoegen of bijwerken van de profielgegevens van een gebruiker met behulp van Azure Active Directory](../fundamentals/active-directory-users-profile-azure-portal.md).  U kunt ook controleren of de gebruiker is geblokkeerd vanwege een [Identity Protection-beleid](../identity-protection/howto-unblock-user.md).

* In de portal mijn toegang als een gebruiker zowel een aanvrager en de fiatteur is, zien ze de aanvraag voor een pakket toegang op de **goedkeuringen** pagina. Dit gedrag is opzettelijk: een gebruiker niet hun eigen aanvraag goedkeuren. Zorg ervoor dat het pakket met toegang tot dat ze aanvragen is extra fiatteurs geconfigureerd op het beleid. Zie voor meer informatie, [een bestaande beleidsregel bewerken](entitlement-management-access-package-edit.md#edit-an-existing-policy).

* Als een nieuwe externe gebruiker, dat niet eerder ondertekend in uw directory, ontvangt een access-pakket met inbegrip van een SharePoint Online-site, wordt hun toegang pakket weergeven die niet volledig worden geleverd totdat hun account is ingericht in SharePoint Online.

## <a name="next-steps"></a>Volgende stappen

- [Rapporten van hoe gebruikers toegang in beheer van rechten kregen weergeven](entitlement-management-reports.md)
