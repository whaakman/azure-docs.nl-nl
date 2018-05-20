---
title: Identiteit voor Azure-app maken in de portal | Microsoft Docs
description: Beschrijft hoe u maakt een nieuwe Azure Active Directory-toepassing en service-principal die kan worden gebruikt met de op rollen gebaseerde toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2018
ms.author: tomfitz
ms.openlocfilehash: e5d93963dddb4acb1147042ae338b32cb5d7646f
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Portal gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot

Wanneer u code die behoeften openen of wijzigen van resources hebt, moet u een Azure Active Directory (AD)-toepassing instellen. U kunt de vereiste machtigingen toewijzen aan AD-toepassing. Deze aanpak is voorkeur boven het uitvoeren van de app met uw eigen referenties, omdat u machtigingen kunt toewijzen aan de identiteit van de app die anders zijn dan uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot precies wat de app moet doen.

Dit artikel laat zien hoe u deze stappen uitvoert via de portal. Het is gericht op een één-tenant-toepassing waarin de toepassing is bedoeld om uit te voeren binnen één organisatie. Doorgaans gebruikt toepassingen voor één tenant voor line-of-business-toepassingen die worden uitgevoerd binnen uw organisatie.

> [!IMPORTANT]
> In plaats van een service-principal maken, kunt u overwegen Azure AD beheerde Service-identiteit voor de identiteit van uw toepassingen. Azure AD-MSI is een openbare preview-functie van Azure Active Directory die vereenvoudigt het maken van een identiteit voor de code. Als uw code wordt uitgevoerd op een service die ondersteuning biedt voor Azure AD MSI en toegang tot bronnen die ondersteuning bieden voor Azure Active Directory-verificatie, is Azure AD MSI een betere optie voor u. Zie voor meer informatie over Azure AD MSI, met inbegrip van welke services momenteel ondersteuning [Service-identiteit voor Azure-resources beheerd](../active-directory/managed-service-identity/overview.md).

## <a name="required-permissions"></a>Vereiste machtigingen

Als u dit artikel, moet u voldoende rechten hebt voor een toepassing registreren met uw Azure AD-tenant en de toepassing toewijzen aan een rol in uw Azure-abonnement. Zorg ervoor dat de juiste machtigingen deze stappen uit te voeren.

### <a name="check-azure-active-directory-permissions"></a>Controleer de machtigingen voor Azure Active Directory

1. Selecteer **Azure Active Directory**.

   ![azure active directory selecteren](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecteer in Azure Active Directory, **gebruikersinstellingen**.

   ![gebruikersinstellingen voor de selecteren](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Controleer de **App registraties** instelling. Indien ingesteld op **Ja**, niet-beheerders kunnen zich registreren AD-apps. Deze instelling betekent dat elke gebruiker in de Azure AD-tenant een app kunt registreren. U kunt verdergaan naar [controleren Azure-abonnement machtigingen](#check-azure-subscription-permissions).

   ![app-registraties weergeven](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Als de app-registraties instelling is ingesteld op **Nee**, alleen [globale beheerders](../active-directory/active-directory-assign-admin-roles-azure-portal.md) apps kunt registreren. Controleer of uw account een beheerder voor de Azure AD-tenant. Selecteer **overzicht** en bekijk uw gebruikersgegevens. Als uw account is toegewezen aan de gebruikersrol, maar de registratie van app-instelling (van de vorige stap) beperkt tot beheergebruikers is, vraag uw beheerder op een toewijzen aan de rol globale beheerder of waarmee gebruikers om apps te registreren.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Controleer de machtigingen van de Azure-abonnement

In uw Azure-abonnement, uw account moet hebben `Microsoft.Authorization/*/Write` toegang tot een AD-app aan een rol toewijzen. Deze actie wordt verleend via de [eigenaar](../role-based-access-control/built-in-roles.md#owner) rol of [beheerder voor gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator) rol. Als uw account is toegewezen aan de **Inzender** rol, u hebt niet de juiste machtiging. U ontvangt een fout opgetreden bij een poging de service-principal toewijzen aan een rol.

Uw abonnement om machtigingen te controleren:

1. Selecteer uw account in de rechterbovenhoek en selecteer **mijn machtigingen**.

   ![gebruikersmachtigingen selecteren](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Selecteer het abonnement in de vervolgkeuzelijst. Selecteer **Klik hier voor volledige toegang tot de gegevens voor dit abonnement**.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/view-details.png)

1. Bekijk uw toegewezen rollen en bepalen of u onvoldoende machtigingen hebt voor het toewijzen van een AD-app aan een rol. Als dat niet het geval is, vraagt de abonnementsbeheerder van uw u toe te voegen aan de rol beheerder voor gebruikerstoegang. In de volgende afbeelding wordt de gebruiker toegewezen aan de rol van eigenaar, wat betekent dat die de gebruiker heeft onvoldoende machtigingen.

   ![machtigingen weergeven](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

1. Aanmelden bij uw Azure-Account via de [Azure-portal](https://portal.azure.com).
1. Selecteer **Azure Active Directory**.

   ![azure active directory selecteren](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecteer **App-registraties**.

   ![app-registraties selecteren](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Selecteer **Nieuwe toepassing registreren**.

   ![toepassing toevoegen](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Geef een naam en URL voor de toepassing op. Selecteer **Web-app/API** voor het type toepassing dat u wilt maken. U kunt geen referenties voor maken een [systeemeigen toepassing](../active-directory/manage-apps/application-proxy-configure-native-client-application.md); daarom type werkt niet voor een geautomatiseerde toepassing. Na het instellen van de waarden, selecteer **maken**.

   ![toepassing een naam geven](./media/resource-group-create-service-principal-portal/create-app.png)

U kunt uw toepassing hebt gemaakt.

## <a name="get-application-id-and-authentication-key"></a>Ophalen van de sleutel-ID en verificatie van toepassing

Wanneer u zich programmatisch aanmeldt, hebt u de id voor uw toepassing en een verificatiesleutel nodig. U kunt deze waarden als volgt ophalen:

1. Selecteer uw toepassing bij **App-registraties** in Azure Active Directory.

   ![toepassing selecteren](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. Sommige [voorbeeldtoepassingen](#log-in-as-the-application) verwijzen naar deze waarde als de client-id.

   ![client-id](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Selecteer **Instellingen** om een verificatiesleutel te genereren.

   ![instellingen selecteren](./media/resource-group-create-service-principal-portal/select-settings.png)

1. Selecteer **Sleutels** om een verificatiesleutel te genereren.

   ![sleutels selecteren](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Geef een beschrijving van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

   ![sleutel opslaan](./media/resource-group-create-service-principal-portal/save-key.png)

   Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Kopieer deze waarde, want u kunt de sleutel later niet meer ophalen. U geeft de sleutelwaarde samen met de toepassings-id op om u aan te melden met de toepassing. Sla de sleutelwaarde op waar uw toepassing deze kan ophalen.

   ![opgeslagen sleutel](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Tenant-id ophalen

Wanneer u zich programmatisch aanmeldt, moet u de tenant-id samen met uw verificatieaanvraag doorgeven.

1. Selecteer **Azure Active Directory**.

   ![azure active directory selecteren](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Haal de tenant-id op door **Eigenschappen** voor uw Azure AD-tenant te selecteren.

   ![Azure AD-eigenschappen selecteren](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopieer de **Directory-id**. Deze waarde is uw tenant-id.

   ![tenant-id](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Toepassing toewijzen aan rol

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol vertegenwoordigt de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van het bereik. Bijvoorbeeld, een toepassing met de rol Lezer voor een resourcegroep toe te voegen dat kan de resourcegroep en alle resources die deze bevat gelezen.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing. Selecteer bijvoorbeeld als een rol bij het abonnementsbereik wilt toewijzen, **abonnementen**. U kunt in plaats daarvan een resourcegroep of resource selecteren.

   ![Abonnement selecteren](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Selecteer het bepaald abonnement (resourcegroep of resource) de toepassing toewijzen aan.

   ![Selecteer het abonnement voor de toewijzing](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Selecteer **toegangsbeheer (IAM)**.

   ![Selecteer toegang](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. Selecteer **Toevoegen**.

   ![Selecteer toevoegen](./media/resource-group-create-service-principal-portal/select-add.png)

1. Selecteer de rol die u wilt toewijzen aan de toepassing. De volgende afbeelding toont de **lezer** rol.

   ![rol selecteren](./media/resource-group-create-service-principal-portal/select-role.png)

1. Azure Active Directory-toepassingen worden niet standaard weergegeven in de beschikbare opties. Als u wilt zoeken in uw toepassing, moet u de naam van het in het zoekveld opgeven. Selecteer deze.

   ![Zoek app](./media/resource-group-create-service-principal-portal/search-app.png)

1. Selecteer **opslaan** voltooid de rol toe te wijzen. Ziet u uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

## <a name="next-steps"></a>Volgende stappen
* Als u een multitenant-toepassing instelt, Zie [Ontwikkelaarshandleiding voor autorisatie met de Azure Resource Manager-API](resource-manager-api-authentication.md).
* Zie voor meer informatie over het opgeven van beveiligingsbeleid [toegangsbeheer op basis van rollen in Azure](../role-based-access-control/role-assignments-portal.md).  
* Zie voor een lijst met beschikbare acties die kunnen worden verleend of geweigerd voor gebruikers, [Resourceprovider van Azure Resource Manager operations](../role-based-access-control/resource-provider-operations.md).
