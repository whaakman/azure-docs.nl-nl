---
title: Azure IoT Suite en Azure Active Directory | Microsoft Docs
description: Hierin wordt beschreven hoe Azure IoT Suite maakt gebruik van Azure Active Directory om machtigingen te beheren.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: e5804cda921e9d598d0ed02c4fafccdb40fbe7a5
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Machtigingen op de site azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Wat gebeurt er wanneer u zich aanmeldt

De eerste keer dat u zich aanmeldt bij [azureiotsuite.com][lnk-azureiotsuite], de site bepaalt aan de machtigingsniveaus die u hebt op basis van de momenteel geselecteerde tenant van Azure Active Directory (AAD) en de Azure-abonnement.

1. Eerst om in te vullen in de lijst met tenants gezien naast uw gebruikersnaam, vindt de site uit van Azure welke AAD tenants u deel uitmaakt. Op dit moment wordt de site, kan alleen gebruikerstokens verkrijgen voor één tenant tegelijk. Daarom wanneer u tenants met behulp van de vervolgkeuzelijst in de rechterbovenhoek overschakelt, registreert de site u naar die tenant verkrijgen van de tokens voor deze tenant.

2. De site vindt vervolgens uit vanuit Azure welke abonnementen u hebt gekoppeld aan de geselecteerde tenant. Wanneer u een nieuwe vooraf geconfigureerde oplossing maakt ziet u de beschikbare abonnementen.

3. Ten slotte wordt haalt de site alle bronnen in de abonnementen en resourcegroepen gelabeld als vooraf geconfigureerde oplossingen en de tegels op de startpagina wordt gevuld.

De volgende secties worden de functies waarmee de toegang tot de vooraf geconfigureerde oplossingen.

## <a name="aad-roles"></a>AAD-functies

De AAD-rollen beheren de mogelijkheid inrichten van de vooraf geconfigureerde oplossingen en beheren van gebruikers in een vooraf geconfigureerde oplossing.

U vindt meer informatie over beheerdersrollen in AAD in [beheerdersrollen toewijzen in Azure AD][lnk-aad-admin]. Het huidige artikel is gericht op de **hoofdbeheerder** en de **gebruiker** directory-functies gebruikt door de vooraf geconfigureerde oplossingen.

### <a name="global-administrator"></a>Globale beheerder

Er zijn veel globale beheerders per AAD-tenant:

* Wanneer u een AAD-tenant maakt, bent u standaard de globale beheerder van deze tenant.
* De globale beheerder kan een basis en standaard, vooraf geconfigureerde oplossingen inrichten.

### <a name="domain-user"></a>Domeingebruiker

Er zijn veel domeingebruikers per AAD-tenant:

* Een domeingebruiker kunt inrichten een basic vooraf geconfigureerde oplossing via de [azureiotsuite.com] [ lnk-azureiotsuite] site.
* Een domeingebruiker kunt maken van een eenvoudige vooraf geconfigureerde oplossing met behulp van de CLI.

### <a name="guest-user"></a>Gastgebruiker

Er zijn veel gastgebruikers per AAD-tenant. Gastgebruikers hebben een beperkte set rechten in de AAD-tenant. Gastgebruikers kunnen niet als gevolg hiervan voorzien in een vooraf geconfigureerde oplossing in de AAD-tenant.

Zie voor meer informatie over gebruikers en rollen in AAD, de volgende bronnen:

* [Gebruikers maken in Azure AD][lnk-create-edit-users]
* [Gebruikers toewijzen aan apps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Beheerdersrollen van Azure-abonnement

De Azure-beheerdersrollen bepalen de mogelijkheid een Azure-abonnement toewijzen aan een AD-tenant.

Meer informatie over de Azure-beheerdersrollen in het artikel [toevoegen of wijzigen Medebeheerder van Azure, servicebeheerder en accountbeheerder][lnk-admin-roles].

## <a name="faq"></a>Veelgestelde vragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Ik ben een servicebeheerder en ik wil de directory toewijzing tussen mijn abonnement en een specifieke AAD-tenant wijzigen. Hoe worden deze taak voltooien?

Zie [een bestaand abonnement toevoegen aan uw Azure AD-directory](../active-directory/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Ik wil een servicebeheerder of Medebeheerder wanneer aangemeld met een organisatorische account wijzigen

Zie het ondersteuningsartikel [wijzigen servicebeheerder en Medebeheerder wanneer aangemeld met een account organisatorische][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Waarom krijg ik deze fout te zien? 'Uw account heeft niet de juiste machtigingen om een oplossing te maken. Neem contact op met uw accountbeheerder of probeer met een ander account."

Bekijk het volgende diagram voor richtlijnen:

![][img-flowchart]

> [!NOTE]
> Als u doorgaat om te zien de fout na de validatie u een globale beheerder op de AAD-tenant en medebeheerder voor het abonnement zijn, de beheerder van uw account de gebruiker te verwijderen en opnieuw toewijzen van benodigde machtiging in deze volgorde hebben. Eerst de gebruiker als een globale beheerder toevoegen en vervolgens gebruiker toevoegen als medebeheerder van het Azure-abonnement. Als de problemen zich blijven voordoen, neem dan contact op met [Help en ondersteuning voor][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Waarom krijg ik deze fout zien wanneer ik een Azure-abonnement hebben? "Een Azure-abonnement is vereist om vooraf geconfigureerde oplossingen te maken. U kunt een gratis proefaccount maken binnen een paar minuten."

Als u zeker weet dat u hebt een Azure-abonnement valideren van de huurder toewijzen voor uw abonnement en zorg ervoor dat de juiste tenant is geselecteerd in de vervolgkeuzelijst. Als u hebt gevalideerd de gewenste tenant juist is, volgt u de voorgaande diagram en valideren van de toewijzing van uw abonnement en deze AAD-tenant.

## <a name="next-steps"></a>Volgende stappen
Zie hoe u kunt om door te gaan leren over IoT Suite [aanpassen van een vooraf geconfigureerde oplossing][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
