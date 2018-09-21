---
title: ID voor Azure-app in de portal maken | Microsoft Docs
description: Beschrijft hoe u een nieuwe Azure Active Directory-toepassing en service-principal die kan worden gebruikt met de op rollen gebaseerd toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen te maken.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2018
ms.author: tomfitz
ms.openlocfilehash: 2f053f6dd98b9f4e97d69e51bce933a003633277
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497940"
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Portal gebruiken om een Azure Active Directory-toepassing en service-principal die toegang hebben tot resources te maken

Wanneer u de code die u moet toegang tot of het wijzigen van resources hebt, moet u een Azure Active Directory (AD)-toepassing instellen. U kunt vervolgens de vereiste machtigingen toewijzen aan de AD-toepassing. Deze aanpak is beter de app onder uw eigen referenties te voeren, omdat u machtigingen kunt toewijzen aan de identiteit van de app die anders dan uw eigen machtigingen. Deze machtigingen worden meestal beperkt tot de functies die met de app moeten kunnen worden uitgevoerd.

Dit artikel leest u hoe u deze stappen uitvoert via de portal. Dit artikel gaat over de toepassing van een één tenant waar de toepassing is bedoeld om uit te voeren binnen één organisatie. Doorgaans gebruikt u één tenant toepassingen voor line-of-business-toepassingen die worden uitgevoerd binnen uw organisatie.

> [!IMPORTANT]
> In plaats van het maken van een service-principal, kunt u overwegen beheerde identiteiten voor Azure-resources voor de identiteit van uw toepassingen. Als uw code wordt uitgevoerd op een service die ondersteuning biedt voor beheerde identiteiten en toegang tot bronnen die ondersteuning bieden voor Azure Active Directory-verificatie, zijn beheerde identiteiten een betere optie voor u. Zie voor meer informatie over beheerde identiteiten voor een Azure-resources, met inbegrip van welke services momenteel, [wat is beheerde identiteiten voor Azure-resources?](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="required-permissions"></a>Vereiste machtigingen

Als u wilt in dit artikel hebt voltooid, moet u voldoende rechten hebt voor een toepassing registreren met uw Azure AD-tenant en de toepassing toewijzen aan een rol in uw Azure-abonnement. Zorg ervoor dat u hebt de juiste machtigingen deze stappen uitvoert.

### <a name="check-azure-active-directory-permissions"></a>Machtigingen voor Azure Active Directory controleren

1. Selecteer **Azure Active Directory**.

   ![azure active directory selecteren](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecteer in Azure Active Directory, **Gebruikersinstellingen**.

   ![selecteert u gebruikersinstellingen](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Controleer de **App-registraties** instelling. Indien ingesteld op **Ja**, niet-beheerders kunnen zich registreren AD-apps. Deze instelling betekent dat elke gebruiker in de Azure AD-tenant een app kan registreren. U kunt doorgaan met [Azure controleren abonnementsmachtigingen](#check-azure-subscription-permissions).

   ![app-registraties weergeven](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Als de instelling app-registraties is ingesteld op **Nee**, alleen [globale beheerders](../active-directory/users-groups-roles/directory-assign-admin-roles.md) apps kunt registreren. Controleer of uw account een beheerder is voor de Azure AD-tenant is. Selecteer **overzicht** en bekijk uw gebruikersgegevens. Als uw account is toegewezen aan de gebruikersrol, maar de app-registratie-instelling (van de vorige stap) beperkt tot gebruikers van de beheerder is, vraagt u uw beheerder om een toewijzen aan de rol globale beheerder of zodat gebruikers om apps te registreren.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Controleer de machtigingen van de Azure-abonnement

In uw Azure-abonnement, moet uw account beschikt over `Microsoft.Authorization/*/Write` toegang tot een AD-app toewijst aan een rol. Deze toegang wordt verleend via de rol [Eigenaar](../role-based-access-control/built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator). Als uw account is toegewezen aan de **Inzender** rol, u hebt geen machtiging voldoende. U ontvangt een foutmelding wanneer u probeert de service-principal toewijzen aan een rol.

Uw om abonnementsmachtigingen te controleren:

1. Selecteer uw account in de rechterbovenhoek en selecteer **mijn machtigingen**.

   ![gebruikersmachtigingen selecteren](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Selecteer het abonnement in de vervolgkeuzelijst. Selecteer **Klik hier voor volledige toegang tot gegevens voor dit abonnement**.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/view-details.png)

1. De toegewezen rollen kunt weergeven en bepalen of u voldoende machtigingen voor een AD-app toewijst aan een rol hebt. Als dat niet het geval is, vraagt de beheerder van het abonnement u toe te voegen aan de rol Administrator voor gebruikerstoegang. In de volgende afbeelding wordt de gebruiker toegewezen aan de rol van eigenaar, wat betekent dat de gebruiker de juiste machtigingen heeft.

   ![machtigingen weergeven](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

1. Meld u aan met uw Azure-Account via de [Azure-portal](https://portal.azure.com).
1. Selecteer **Azure Active Directory**.

   ![azure active directory selecteren](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecteer **App-registraties**.

   ![app-registraties selecteren](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Selecteer **Nieuwe toepassing registreren**.

   ![toepassing toevoegen](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Geef een naam en URL op voor de toepassing. Selecteer **Web-app/API** voor het type toepassing dat u wilt maken. U kunt geen referenties voor de maken een [systeemeigen toepassing](../active-directory/manage-apps/application-proxy-configure-native-client-application.md); daarom dat type niet voor een geautomatiseerde toepassing werkt. Na het instellen van de waarden, selecteer **maken**.

   ![toepassing een naam geven](./media/resource-group-create-service-principal-portal/create-app.png)

U kunt uw toepassing hebt gemaakt.

## <a name="get-application-id-and-authentication-key"></a>Toepassing-ID en verificatiesleutel sleutel ophalen

Wanneer u zich programmatisch aanmeldt, hebt u de id voor uw toepassing en een verificatiesleutel nodig. U kunt deze waarden als volgt ophalen:

1. Selecteer uw toepassing bij **App-registraties** in Azure Active Directory.

   ![toepassing selecteren](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. Sommige [voorbeeldtoepassingen](#log-in-as-the-application) verwijzen naar deze waarde als de client-id.

   ![client-id](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Selecteer **Instellingen** om een verificatiesleutel te genereren.

   ![instellingen selecteren](./media/resource-group-create-service-principal-portal/select-settings.png)

1. Selecteer **Sleutels** om een verificatiesleutel te genereren.

   ![sleutels selecteren](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Geef een beschrijving op van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

   ![sleutel opslaan](./media/resource-group-create-service-principal-portal/save-key.png)

   Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Deze waarde niet kopiëren omdat u niet kunnen ophalen van de sleutel later opnieuw. U geeft de sleutelwaarde samen met de toepassings-ID aan te melden als de toepassing. Bewaar de sleutelwaarde op een locatie waar de toepassing deze kan ophalen.

   ![opgeslagen sleutel](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Tenant-id ophalen

Wanneer u zich programmatisch aanmeldt, moet u de tenant-id samen met uw verificatieaanvraag doorgeven.

1. Selecteer **Azure Active Directory**.

   ![azure active directory selecteren](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Haal de tenant-id op door **Eigenschappen** voor uw Azure AD-tenant te selecteren.

   ![Azure AD-eigenschappen selecteren](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopieer de **Map-id**. Deze waarde is uw tenant-id.

   ![tenant-id](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Toepassing toewijzen aan rol

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol staat voor de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Bijvoorbeeld een toepassing toevoegt aan de rol van lezer voor een resourcegroep betekent dit dat het vindt de resourcegroep en alle resources die deze bevat.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **abonnementen**. U kunt in plaats daarvan een resourcegroep of resource selecteren.

   ![abonnement selecteren](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Selecteer het specifieke abonnement (resourcegroep of resource) om de toepassing te wijzen aan.

   ![abonnement voor toewijzing selecteren](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Selecteer **toegangsbeheer (IAM)**.

   ![Selecteer toegang](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. Selecteer **Toevoegen**.

   ![Selecteer toevoegen](./media/resource-group-create-service-principal-portal/select-add.png)

1. Selecteer de rol die u wilt toewijzen aan de toepassing. Als u wilt toestaan dat de toepassing uitvoeren van acties zoals **opnieuw opstarten**, **start** en **stoppen** instanties, die u nodig hebt om de rol selecteren **Inzender**. De volgende afbeelding toont de **lezer** rol.

   ![rol selecteren](./media/resource-group-create-service-principal-portal/select-role.png)

1. Azure Active Directory-toepassingen worden niet standaard weergegeven in de beschikbare opties. Als u uw toepassing zoekt, moet u de naam ervan in het zoekveld opgeven. Selecteer deze.

   ![zoeken naar app](./media/resource-group-create-service-principal-portal/search-app.png)

1. Selecteer **opslaan** voltooien van de rol toe te wijzen. U ziet uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

## <a name="next-steps"></a>Volgende stappen
* Als u een toepassing met meerdere tenants instelt, Zie [Developer's guide voor autorisatie met de Azure Resource Manager API](resource-manager-api-authentication.md).
* Zie voor meer informatie over het opgeven van beleidsregels voor veiligheid, [Azure Role-based Access Control](../role-based-access-control/role-assignments-portal.md).  
* Zie voor een lijst van beschikbare acties die kunnen worden verleend of geweigerd voor gebruikers, [Resourceprovider van Azure Resource Manager-bewerkingen](../role-based-access-control/resource-provider-operations.md).
