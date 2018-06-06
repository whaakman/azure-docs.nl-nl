---
title: Azure IoT-oplossing accelerators en Azure Active Directory | Microsoft Docs
description: Hierin wordt beschreven hoe Azure IoT-oplossing accelerators Azure Active Directory gebruikt om machtigingen te beheren.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 51d55d20f905c3677bdb98cb25d305f5152aa52c
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34735881"
---
# <a name="permissions-on-the-azureiotsolutionscom-site"></a>Machtigingen op de site azureiotsolutions.com

## <a name="what-happens-when-you-sign-in"></a>Wat gebeurt er wanneer u zich aanmeldt

De eerste keer dat u zich aanmeldt bij [azureiotsuite.com][lnk-azureiotsolutions], de site bepaalt aan de machtigingsniveaus die u hebt op basis van de momenteel geselecteerde tenant van Azure Active Directory (AAD) en de Azure-abonnement.

1. Eerst om in te vullen in de lijst met tenants gezien naast uw gebruikersnaam, vindt de site uit van Azure welke AAD tenants u deel uitmaakt. Op dit moment wordt de site, kan alleen gebruikerstokens verkrijgen voor één tenant tegelijk. Daarom wanneer u tenants met behulp van de vervolgkeuzelijst in de rechterbovenhoek overschakelt, registreert de site u naar die tenant verkrijgen van de tokens voor deze tenant.

2. De site vindt vervolgens uit vanuit Azure welke abonnementen u hebt gekoppeld aan de geselecteerde tenant. Bij het maken van een nieuwe oplossingsverbetering ziet u de beschikbare abonnementen.

3. Ten slotte wordt haalt de site alle bronnen in de abonnementen en resourcegroepen gelabeld als oplossing accelerators en de tegels op de startpagina wordt gevuld.

De volgende secties worden de functies waarmee de toegang aan de oplossing accelerators aan te schaffen.

## <a name="aad-roles"></a>AAD-functies

De AAD-rollen beheren de mogelijkheid om te richten oplossing accelerators, voor het beheren van gebruikers en bepaalde Azure-services in een oplossingsverbetering.

U vindt meer informatie over beheerdersrollen in AAD in [beheerdersrollen toewijzen in Azure AD][lnk-aad-admin]. Het huidige artikel is gericht op de **hoofdbeheerder** en de **gebruiker** directory-functies gebruikt door de oplossing accelerators aan te schaffen.

### <a name="global-administrator"></a>Globale beheerder

Er zijn veel globale beheerders per AAD-tenant:

* Wanneer u een AAD-tenant maakt, bent u standaard de globale beheerder van deze tenant.
* De globale beheerder kunt inrichten van een basis en standaard oplossing accelerators (een eenvoudige implementatie maakt gebruik van één Azure virtuele Machine).

### <a name="domain-user"></a>Domeingebruiker

Er zijn veel domeingebruikers per AAD-tenant:

* Een domeingebruiker kunt inrichten een basic oplossingsverbetering via de [azureiotsolutions.com] [ lnk-azureiotsolutions] site.
* Een domeingebruiker kunt maken van een eenvoudige oplossingsverbetering met behulp van de CLI.

### <a name="guest-user"></a>Gastgebruiker

Er zijn veel gastgebruikers per AAD-tenant. Gastgebruikers hebben een beperkte set rechten in de AAD-tenant. Gastgebruikers kunnen als gevolg hiervan een oplossingsverbetering in de tenant van AAD niet inrichten.

Zie voor meer informatie over gebruikers en rollen in AAD, de volgende bronnen:

* [Gebruikers maken in Azure AD][lnk-create-edit-users]
* [Gebruikers toewijzen aan apps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Beheerdersrollen van Azure-abonnement

De Azure-beheerdersrollen bepalen de mogelijkheid een Azure-abonnement toewijzen aan een AAD-tenant.

Meer informatie over de Azure-beheerdersrollen in het artikel [toevoegen of wijzigen Medebeheerder van Azure, servicebeheerder en accountbeheerder][lnk-admin-roles].

## <a name="faq"></a>Veelgestelde vragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Ik ben een servicebeheerder en ik wil de directory toewijzing tussen mijn abonnement en een specifieke AAD-tenant wijzigen. Hoe worden deze taak voltooien?

Zie [een bestaand abonnement toevoegen aan uw Azure AD-directory](../active-directory/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Ik wil een servicebeheerder of Medebeheerder wanneer aangemeld met een organisatie-account wijzigen

Zie het ondersteuningsartikel [wijzigen servicebeheerder en Medebeheerder wanneer aangemeld met een organisatieaccount][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Waarom krijg ik deze fout te zien? 'Uw account heeft niet de juiste machtigingen om een oplossing te maken. Neem contact op met uw accountbeheerder of probeer met een ander account."

Bekijk het volgende diagram voor richtlijnen:

![][img-flowchart]

> [!NOTE]
> Als u doorgaat om te zien de fout na de validatie u een globale beheerder van de AAD-tenant en medebeheerder van het abonnement zijn, de beheerder van uw account de gebruiker te verwijderen en opnieuw toewijzen van benodigde machtiging in deze volgorde hebben. Eerst de gebruiker als een globale beheerder toevoegen en vervolgens gebruiker toevoegen als medebeheerder van het Azure-abonnement. Als de problemen zich blijven voordoen, neem dan contact op met [Help en ondersteuning voor][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Waarom krijg ik deze fout zien wanneer ik een Azure-abonnement hebben? "Een Azure-abonnement is vereist om vooraf geconfigureerde oplossingen te maken. U kunt een gratis proefaccount maken binnen een paar minuten."

Als u zeker weet dat u hebt een Azure-abonnement valideren van de huurder toewijzen voor uw abonnement en zorg ervoor dat de juiste tenant is geselecteerd in de vervolgkeuzelijst. Als u hebt gevalideerd de gewenste tenant juist is, volgt u de voorgaande diagram en valideren van de toewijzing van uw abonnement en deze AAD-tenant.

## <a name="next-steps"></a>Volgende stappen
Zie hoe u kunt om door te gaan leren over IoT-oplossing accelerators [aanpassen van een oplossingsverbetering][lnk-customize].

[img-flowchart]: media/iot-accelerators-permissions/flowchart.png

[lnk-azureiotsolutions]: https://www.azureiotsolutions.com
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles-azure-portal.md
[lnk-portal]: https://portal.azure.com
[lnk-create-edit-users]: ../active-directory/active-directory-users-profile-azure-portal.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-accelerators-remote-monitoring-customize.md
