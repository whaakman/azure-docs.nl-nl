---
title: Azure IoT Suite en Azure Active Directory | Microsoft Docs
description: Hierin wordt beschreven hoe Azure IoT Suite maakt gebruik van Azure Active Directory om machtigingen te beheren.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: a56d535ee06a097c7c18bcd507c25708f6a33f91
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296921"
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Machtigingen op de site azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Wat gebeurt er wanneer u zich aanmeldt

De eerste keer dat u zich aanmeldt bij [azureiotsuite.com][lnk-azureiotsuite], de site bepaalt de machtigingsniveaus die u hebt op basis van de momenteel geselecteerde Azure Active Directory (AAD)-tenant en Azure-abonnement.

1. Eerst, om in te vullen in de lijst met tenants weergegeven naast uw gebruikersnaam, de site zoekt uit van Azure die u werkzaam bent bij AAD-tenants. De site kan op dit moment alleen gebruikerstokens voor één tenant verkrijgen op een tijdstip. Dus als u tenants met behulp van de vervolgkeuzelijst in de rechterbovenhoek, de site geregistreerd in die tenant om op te halen van de tokens voor de tenant.

2. De site zoekt vervolgens uit vanuit Azure welke abonnementen die u hebt gekoppeld aan de geselecteerde tenant. Wanneer u een nieuwe, vooraf geconfigureerde oplossing maken ziet u de beschikbare abonnementen.

3. Ten slotte haalt de site alle resources in de abonnementen en resourcegroepen gelabeld als vooraf geconfigureerde oplossingen en vult de tegels op de startpagina.

De volgende secties worden de rollen die de toegang tot de vooraf geconfigureerde oplossingen.

## <a name="aad-roles"></a>AAD-rollen

De AAD-rollen beheren de mogelijkheid inrichten vooraf geconfigureerde oplossingen en beheren van gebruikers in een vooraf geconfigureerde oplossing.

U vindt meer informatie over beheerdersrollen in AAD in [beheerdersrollen toewijzen in Azure AD][lnk-aad-admin]. Het huidige artikel richt zich op de **hoofdbeheerder** en de **gebruiker** directory-rollen zoals de vooraf geconfigureerde oplossingen wordt gebruikt.

### <a name="global-administrator"></a>Globale beheerder

Er zijn veel globale beheerders per AAD-tenant:

* Wanneer u een AAD-tenant maakt, bent u standaard de globale beheerder van deze tenant.
* De globale beheerder van een vooraf geconfigureerde oplossing kunt inrichten en krijgt een **Admin** rol voor de toepassing binnen hun AAD-tenant.
* Als een andere gebruiker in de dezelfde AAD-tenant een toepassing maakt, is het de standaardrol verleend aan de globale beheerder **ReadOnly**.
* Een globale beheerder gebruikers toewijzen aan rollen voor toepassingen die gebruikmaken van de [Azure-portal][lnk-portal].

### <a name="domain-user"></a>Domeingebruiker

Er zijn veel domeingebruikers per AAD-tenant:

* Een domeingebruiker kunt inrichten van een vooraf geconfigureerde oplossing via de [azureiotsuite.com] [ lnk-azureiotsuite] site. Standaard wordt de domeingebruiker verleend de **Admin** rol in de ingerichte toepassing.
* Een domeingebruiker kan een toepassing maken met het script build.cmd in de [azure-iot-remote-monitoring][lnk-rm-github-repo], [azure-iot-predictive-maintenance] [ lnk-pm-github-repo], of [azure-iot-connected-factory] [ lnk-cf-github-repo] opslagplaats. De standaardrol verleend aan de domeingebruiker is echter **ReadOnly**, omdat een domeingebruiker is niet gemachtigd het toewijzen van rollen.
* Als een andere gebruiker in de AAD-tenant een toepassing maakt, de domeingebruiker is toegewezen de **ReadOnly** rol standaard voor de toepassing.
* Een domeingebruiker kan geen rollen toewijzen voor toepassingen. Daarom toevoegen een domeingebruiker niet of rollen voor gebruikers voor een toepassing, zelfs als ze ingericht.

### <a name="guest-user"></a>Gastgebruiker

Er zijn veel gastgebruikers ook kunnen per AAD-tenant. Gastgebruikers hebben een beperkte set rechten in de AAD-tenant. Gastgebruikers kunnen niet als gevolg hiervan een vooraf geconfigureerde oplossing in de AAD-tenant inricht.

Zie de volgende bronnen voor meer informatie over gebruikers en rollen in AAD:

* [Gebruikers maken in Azure AD][lnk-create-edit-users]
* [Gebruikers toewijzen aan apps][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Beheerdersrollen in Azure-abonnement

De Azure-beheerdersrollen beheren de mogelijkheid om een Azure-abonnement worden toegewezen aan een AD-tenant.

Meer informatie over de Azure-beheerdersrollen in het artikel [toevoegen of wijzigen Azure-abonnementbeheerders][lnk-admin-roles].

## <a name="application-roles"></a>Toepassingsrollen

De rollen van de toepassing beheren toegang tot apparaten in uw vooraf geconfigureerde oplossing.

Er zijn twee gedefinieerde rollen en één impliciete rol gedefinieerd in een ingerichte toepassing:

* **De beheerder:** heeft volledig beheer toevoegen, beheren, apparaten verwijderen en instellingen wijzigen.
* **Alleen-lezen:** apparaten, regels, acties, taken en telemetrie kunt weergeven.

U vindt de machtigingen worden toegewezen aan elke rol in de [RolePermissions.cs] [ lnk-resource-cs] bronbestand.

### <a name="changing-application-roles-for-a-user"></a>Toepassingsrollen voor een gebruiker wijzigen

U kunt de volgende procedure gebruiken zodat een beheerder van uw vooraf geconfigureerde oplossing voor een gebruiker in uw Active Directory.

U moet een globale beheerder van de AAD functies voor een gebruiker te wijzigen:

1. Ga naar [Azure Portal][lnk-portal].
2. Selecteer **Azure Active Directory**.
3. Zorg ervoor dat u de map die u hebt gekozen bij het inrichten van uw oplossing op azureiotsuite.com werkt. Als u meerdere mappen die zijn gekoppeld aan uw abonnement hebt, kunt u schakelen tussen deze als u klikt op de naam van uw account in de rechterbovenhoek van de portal.
4. Klik op **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen**.
4. Weergeven **alle toepassingen** met **eventuele** status. Zoek vervolgens naar een toepassing met de naam van uw vooraf geconfigureerde oplossing.
5. Klik op de naam van de toepassing die overeenkomt met de naam van uw vooraf geconfigureerde oplossing.
6. Klik op **Gebruikers en groepen**.
7. Selecteer de gebruiker die u wilt overschakelen van rollen.
8. Klik op **toewijzen** en selecteer de rol (zoals **Admin**) u wilt toewijzen aan de gebruiker, klikt u op het vinkje hebt geklikt.

## <a name="faq"></a>Veelgestelde vragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Ik ben een servicebeheerder en ik wil de directory-toewijzing tussen mijn abonnement en een specifieke AAD-tenant wijzigen. Hoe ik deze taak voltooid?

Zie [aan een bestaand abonnement toevoegen aan uw Azure AD-directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ik ben een gebruiker/lid van domein van de AAD-tenant en ik heb een vooraf geconfigureerde oplossing hebt gemaakt. Hoe ik ophalen een bepaalde rol voor mijn toepassing?

Vraag een globale beheerder moet u een globale beheerder op de AAD-tenant en vervolgens rollen toewijzen aan gebruikers zelf. U kunt ook vragen een globale beheerder om toe te wijzen u rechtstreeks een rol. Zie de volgende vraag als u wilt wijzigen van de AAD-tenant uw vooraf geconfigureerde oplossing is geïmplementeerd.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Hoe verander ik mijn vooraf geconfigureerde oplossing voor externe controle en de toepassing worden toegewezen aan de AAD-tenant?

U kunt een cloudimplementatie van uitvoeren <https://github.com/Azure/azure-iot-remote-monitoring> en opnieuw implementeren met een nieuwe AAD-tenant. Omdat u bent, standaard een globale beheerder bij het maken van een AAD-tenant, hebt u machtigingen voor het toevoegen van gebruikers en rollen toewijzen aan deze gebruikers.

1. Maak een AAD-map in de [Azure-portal][lnk-portal].
2. Ga naar <https://github.com/Azure/azure-iot-remote-monitoring>.
3. Voer `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (bijvoorbeeld `build.cmd cloud debug myRMSolution`)
4. Wanneer u hierom wordt gevraagd, de **tenantid** moet uw nieuwe tenant in plaats van uw vorige tenant.

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Ik wil een servicebeheerder of CO-beheerder wanneer u bent aangemeld met een organisatorische account wijzigen

Zie het ondersteuningsartikel [wijzigen van servicebeheerder en Medebeheerder wanneer u bent aangemeld met een organisatorische account][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Waarom krijg ik deze fout te zien? 'Uw account beschikt niet over de juiste machtigingen om een oplossing te maken. Neem contact op met uw accountbeheerder of probeer met een ander account."

Bekijk het volgende diagram voor richtlijnen:

![][img-flowchart]

> [!NOTE]
> Als u doorgaat om te zien de fout na de validatie u een globale beheerder voor de AAD-tenant en een CO-beheerder van het abonnement zijn, de beheerder van uw account de gebruiker te verwijderen en opnieuw toewijzen van benodigde machtiging in deze volgorde. Eerst de gebruiker toevoegen als een globale beheerder en gebruiker vervolgens als een CO-beheerder van het Azure-abonnement toevoegen. Als de problemen zich blijven voordoen, neem dan contact op met [Help en ondersteuning][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Waarom krijg ik deze fout zien wanneer ik heb een Azure-abonnement? "Een Azure-abonnement is vereist om vooraf geconfigureerde oplossingen te maken. U kunt een gratis proefaccount maken in een paar minuten."

Als u zeker weet dat u hebt een Azure-abonnement, de tenanttoewijzing voor uw abonnement te valideren en controleer of dat de juiste tenant in de vervolgkeuzelijst is geselecteerd. Als u hebt gevalideerd, de gewenste tenant juist is, volgt u de voorgaande diagram en valideren van de toewijzing van uw abonnement en deze AAD-tenant.

## <a name="next-steps"></a>Volgende stappen
Om meer te leren over IoT Suite, ziet u hoe u [een vooraf geconfigureerde oplossing aanpassen][lnk-customize].

[img-flowchart]: media/iot-suite-v1-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
