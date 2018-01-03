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
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: a032fc4332c697748e658ad2615ed5b0915c56c1
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
* De globale beheerder kan een vooraf geconfigureerde oplossing inricht en krijgt een **Admin** rol voor de toepassing binnen de AAD-tenant.
* Als een andere gebruiker in dezelfde AAD-tenant wordt gemaakt van een toepassing, de standaard-rol te krijgen tot de globale beheerder is **ReadOnly**.
* Een globale beheerder gebruikers toewijzen aan rollen voor toepassingen die gebruikmaken van de [Azure-portal][lnk-portal].

### <a name="domain-user"></a>Domeingebruiker

Er zijn veel domeingebruikers per AAD-tenant:

* Een domeingebruiker kan inrichten met een vooraf geconfigureerde oplossing via de [azureiotsuite.com] [ lnk-azureiotsuite] site. Standaard de domeingebruiker is verleend de **Admin** rol in de betreffende toepassing.
* Een domeingebruiker kan een toepassing maken met het script build.cmd in de [azure-iot-remote-monitoring][lnk-rm-github-repo], [azure-iot-predictive-maintenance][lnk-pm-github-repo], of [azure-iot-verbonden-factory] [ lnk-cf-github-repo] opslagplaats. De standaard-rol te krijgen tot de domeingebruiker is echter **ReadOnly**, omdat een domeingebruiker geen machtiging heeft voor het toewijzen van rollen.
* Als een andere gebruiker in de AAD-tenant wordt gemaakt van een toepassing, de domeingebruiker is toegewezen de **ReadOnly** rol standaard voor die toepassing.
* Rollen voor toepassingen, kan niet worden toegewezen door een domeingebruiker Daarom toevoegen een domeingebruiker niet gebruikers of rollen voor gebruikers voor een toepassing zelfs als ze deze ingericht.

### <a name="guest-user"></a>Gastgebruiker

Er zijn veel gastgebruikers per AAD-tenant. Gastgebruikers hebben een beperkte set rechten in de AAD-tenant. Gastgebruikers kunnen niet als gevolg hiervan voorzien in een vooraf geconfigureerde oplossing in de AAD-tenant.

Zie voor meer informatie over gebruikers en rollen in AAD, de volgende bronnen:

* [Gebruikers maken in Azure AD][lnk-create-edit-users]
* [Gebruikers toewijzen aan apps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Beheerdersrollen van Azure-abonnement

De Azure-beheerdersrollen bepalen de mogelijkheid een Azure-abonnement toewijzen aan een AD-tenant.

Meer informatie over de Azure-beheerdersrollen in het artikel [toevoegen of wijzigen Medebeheerder van Azure, servicebeheerder en accountbeheerder][lnk-admin-roles].

## <a name="application-roles"></a>Toepassingsrollen

De rollen van de toepassing toegang tot apparaten in uw vooraf geconfigureerde oplossing.

Er zijn twee gedefinieerde rollen en één impliciete rol gedefinieerd in een ingerichte toepassing:

* **Admin:** heeft volledig beheer wilt toevoegen, beheren, verwijderen van apparaten en instellingen wijzigen.
* **Alleen-lezen:** apparaten, regels, taken, acties en telemetrie kunt weergeven.

U vindt de machtigingen worden toegewezen aan elke rol in de [RolePermissions.cs] [ lnk-resource-cs] bronbestand.

### <a name="changing-application-roles-for-a-user"></a>Het wijzigen van de rollen van de toepassing voor een gebruiker

U kunt de volgende procedure om een gebruiker in uw Active Directory een beheerder van uw vooraf geconfigureerde oplossing maken.

U moet een globale beheerder van de AAD functies voor een gebruiker te wijzigen:

1. Ga naar [Azure Portal][lnk-portal].
2. Selecteer **Azure Active Directory**.
3. Zorg ervoor dat u de map die u hebt gekozen bij het inrichten van uw oplossing op azureiotsuite.com werkt. Als u meerdere mappen die zijn gekoppeld aan uw abonnement hebt, kunt u schakelen tussen deze als u de naam van uw account in de rechterbovenhoek van de portal op.
4. Klik op **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.
4. Weergeven **alle toepassingen** met **eventuele** status. Zoek vervolgens naar een toepassing met de naam van uw vooraf geconfigureerde oplossing.
5. Klik op de naam van de toepassing die overeenkomt met de naam van uw vooraf geconfigureerde oplossing.
6. Klik op **gebruikers en groepen**.
7. Selecteer de gebruiker die u wilt overschakelen van rollen.
8. Klik op **toewijzen** en selecteer de rol (zoals **Admin**) u wilt toewijzen aan de gebruiker, klikt u op het vinkje.

## <a name="faq"></a>Veelgestelde vragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Ik ben een servicebeheerder en ik wil de directory toewijzing tussen mijn abonnement en een specifieke AAD-tenant wijzigen. Hoe worden deze taak voltooien?

Zie [een bestaand abonnement toevoegen aan uw Azure AD-directory](../active-directory/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ik ben gebruiker/lid van een domein op de AAD-tenant en ik een vooraf geconfigureerde oplossing hebt gemaakt. Hoe ik ophalen een bepaalde rol voor de toepassing?

Vraag een globale beheerder moet u een globale beheerder op de AAD-tenant en vervolgens rollen toewijzen aan gebruikers zelf. U kunt ook vragen om toe te wijzen u rechtstreeks een rol globale beheerder. Als u de AAD-tenant wijzigen uw vooraf geconfigureerde oplossing is geïmplementeerd als u wilt, Zie de volgende vraag.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Hoe schakel ik mijn vooraf geconfigureerde oplossing voor externe controle en de toepassing zijn toegewezen aan de AAD-tenant?

U kunt een cloudimplementatie uit uitvoeren <https://github.com/Azure/azure-iot-remote-monitoring> en implementeren met een nieuwe AAD-tenant. Omdat u bent, standaard een globale beheerder bij het maken van een AAD-tenant, hebt u machtigingen voor het toevoegen van gebruikers en rollen voor deze gebruikers toewijst.

1. Maak een AAD-directory in de [Azure-portal][lnk-portal].
2. Ga naar <https://github.com/Azure/azure-iot-remote-monitoring>.
3. Voer `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (bijvoorbeeld `build.cmd cloud debug myRMSolution`)
4. Als u wordt gevraagd, stelt de **tenantid** worden de nieuwe tenant in plaats van uw vorige tenant.

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

[img-flowchart]: media/iot-suite-v1-permissions/flowchart.png

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
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
