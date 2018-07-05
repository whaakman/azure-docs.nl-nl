---
title: Azure IoT-oplossingsversnellers en Azure Active Directory | Microsoft Docs
description: Hierin wordt beschreven hoe Azure IoT-oplossingsversnellers maakt gebruik van Azure Active Directory om machtigingen te beheren.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 676d5e553e2929ae09d447141ca315fd1cc448e3
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449822"
---
# <a name="permissions-on-the-azureiotsolutionscom-site"></a>Machtigingen op de site azureiotsolutions.com

## <a name="what-happens-when-you-sign-in"></a>Wat gebeurt er wanneer u zich aanmeldt

De eerste keer dat u zich aanmeldt bij [azureiotsuite.com][lnk-azureiotsolutions], de site bepaalt de machtigingsniveaus die u hebt op basis van de momenteel geselecteerde Azure Active Directory (AAD)-tenant en Azure-abonnement.

1. Eerst, om in te vullen in de lijst met tenants weergegeven naast uw gebruikersnaam, de site zoekt uit van Azure die u werkzaam bent bij AAD-tenants. De site kan op dit moment alleen gebruikerstokens voor één tenant verkrijgen op een tijdstip. Dus als u tenants met behulp van de vervolgkeuzelijst in de rechterbovenhoek, de site geregistreerd in die tenant om op te halen van de tokens voor de tenant.

2. De site zoekt vervolgens uit vanuit Azure welke abonnementen die u hebt gekoppeld aan de geselecteerde tenant. Bij het maken van een nieuwe oplossingsversnellers ziet u de beschikbare abonnementen.

3. Ten slotte haalt de site alle resources in de abonnementen en resourcegroepen gelabeld als oplossingsversnellers en vult de tegels op de startpagina.

De volgende secties worden de rollen die de toegang aan de Oplossingsverbeteringen.

## <a name="aad-roles"></a>AAD-rollen

De AAD-rollen beheren de mogelijkheid om te oplossingsversnellers inrichten, voor het beheren van gebruikers en bepaalde Azure-services in een oplossingsversnellers.

U vindt meer informatie over beheerdersrollen in AAD in [beheerdersrollen toewijzen in Azure AD][lnk-aad-admin]. Het huidige artikel richt zich op de **hoofdbeheerder** en de **gebruiker** maprollen gebruikt door de oplossingsversnellers.

### <a name="global-administrator"></a>Globale beheerder

Er zijn veel globale beheerders per AAD-tenant:

* Wanneer u een AAD-tenant maakt, bent u standaard de globale beheerder van deze tenant.
* De globale beheerder kan een basic en standard oplossingsversnellers (een eenvoudige implementatie maakt gebruik van een enkel Azure-Machine) inrichten.

### <a name="domain-user"></a>Domeingebruiker

Er zijn veel domeingebruikers per AAD-tenant:

* Een domeingebruiker kunt inrichten een eenvoudige oplossingsversnellers via de [azureiotsolutions.com] [ lnk-azureiotsolutions] site.
* Een domeingebruiker kunt maken van een eenvoudige oplossingsversnellers met behulp van de CLI.

### <a name="guest-user"></a>Gastgebruiker

Er zijn veel gastgebruikers ook kunnen per AAD-tenant. Gastgebruikers hebben een beperkte set rechten in de AAD-tenant. Gastgebruikers ook kunnen inrichten niet als gevolg hiervan een oplossingsversnellers in de AAD-tenant.

Zie de volgende bronnen voor meer informatie over gebruikers en rollen in AAD:

* [Gebruikers maken in Azure AD][lnk-create-edit-users]
* [Gebruikers toewijzen aan apps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Beheerdersrollen in Azure-abonnement

De Azure-beheerdersrollen beheren de mogelijkheid om een Azure-abonnement worden toegewezen aan een AAD-tenant.

Meer informatie over de Azure-beheerdersrollen in het artikel [toevoegen of wijzigen van Azure Medebeheerder, servicebeheerder en accountbeheerder][lnk-admin-roles].

## <a name="faq"></a>Veelgestelde vragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Ik ben een servicebeheerder en ik wil de directory-toewijzing tussen mijn abonnement en een specifieke AAD-tenant wijzigen. Hoe ik deze taak voltooid?

Zie [aan een bestaand abonnement toevoegen aan uw Azure AD-directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Ik wil een servicebeheerder of CO-beheerder wanneer u bent aangemeld met een organisatie-account wijzigen

Zie het ondersteuningsartikel [wijzigen van servicebeheerder en Medebeheerder wanneer u bent aangemeld met een organisatieaccount][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Waarom krijg ik deze fout te zien? 'Uw account beschikt niet over de juiste machtigingen om een oplossing te maken. Neem contact op met uw accountbeheerder of probeer met een ander account."

Bekijk het volgende diagram voor richtlijnen:

![][img-flowchart]

> [!NOTE]
> Als u doorgaat om te zien de fout na de validatie u een globale beheerder van de AAD-tenant en een CO-beheerder van het abonnement zijn, de beheerder van uw account de gebruiker te verwijderen en opnieuw toewijzen van benodigde machtiging in deze volgorde. Eerst de gebruiker als een globale beheerder toevoegen en voeg deze gebruiker als een CO-beheerder van de Azure-abonnement. Als de problemen zich blijven voordoen, neem dan contact op met [Help en ondersteuning][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Waarom krijg ik deze fout zien wanneer ik heb een Azure-abonnement? "Een Azure-abonnement is vereist om vooraf geconfigureerde oplossingen te maken. U kunt een gratis proefaccount maken in een paar minuten."

Als u zeker weet dat u hebt een Azure-abonnement, de tenanttoewijzing voor uw abonnement te valideren en controleer of dat de juiste tenant in de vervolgkeuzelijst is geselecteerd. Als u hebt gevalideerd, de gewenste tenant juist is, volgt u de voorgaande diagram en valideren van de toewijzing van uw abonnement en deze AAD-tenant.

## <a name="next-steps"></a>Volgende stappen
Om meer te leren over IoT-oplossingsversnellers, ziet u hoe u [een oplossingsversnellers aanpassen][lnk-customize].

[img-flowchart]: media/iot-accelerators-permissions/flowchart.png

[lnk-azureiotsolutions]: https://www.azureiotsolutions.com
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]:../active-directory/users-groups-roles/directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com
[lnk-create-edit-users]:../active-directory/fundamentals/active-directory-users-profile-azure-portal.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-accelerators-remote-monitoring-customize.md
