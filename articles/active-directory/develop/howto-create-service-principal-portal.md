---
title: ID voor Azure-app in de portal maken | Microsoft Docs
description: Beschrijft hoe u een nieuwe Azure Active Directory-toepassing en service-principal die kan worden gebruikt met de op rollen gebaseerd toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen te maken.
services: active-directory
documentationcenter: na
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2018
ms.author: celested
ms.reviewer: tomfitz
ms.openlocfilehash: b60be3db14a756407e62540bb89f3b0924597067
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49960355"
---
# <a name="how-to-use-the-portal-to-create-an-azure-ad-application-and-service-principal-that-can-access-resources"></a>Hoe: Gebruik de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources

Wanneer u de code die u moet toegang tot of het wijzigen van resources hebt, kunt u een identiteit voor de app kunt maken. Deze identiteit staat bekend als een service-principal. U kunt vervolgens de vereiste machtigingen toewijzen aan de service-principal. Dit artikel leest u hoe het gebruik van de portal om de serviceprincipal te maken. Dit artikel gaat over de toepassing van een één tenant waar de toepassing is bedoeld om uit te voeren binnen één organisatie. Doorgaans gebruikt u één tenant toepassingen voor line-of-business-toepassingen die worden uitgevoerd binnen uw organisatie.

> [!IMPORTANT]
> In plaats van het maken van een service-principal, kunt u overwegen beheerde identiteiten voor Azure-resources voor de identiteit van uw toepassingen. Als uw code wordt uitgevoerd op een service die ondersteuning biedt voor beheerde identiteiten en toegang tot bronnen die ondersteuning bieden voor verificatie van Azure Active Directory (Azure AD), zijn beheerde identiteiten een betere optie voor u. Zie voor meer informatie over beheerde identiteiten voor een Azure-resources, met inbegrip van welke services momenteel, [wat is beheerde identiteiten voor Azure-resources?](../managed-identities-azure-resources/overview.md).

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

We gaan direct naar het maken van de identiteit. Als u een probleem ondervindt, controleert u de [vereiste machtigingen](#required-permissions) om ervoor te zorgen dat uw account de identiteit kunt maken.

1. Meld u aan met uw Azure-Account via de [Azure-portal](https://portal.azure.com).
1. Selecteer **Azure Active Directory**.
1. Selecteer **App-registraties**.

   ![app-registraties selecteren](./media/howto-create-service-principal-portal/select-app-registrations.png)

1. Selecteer **Nieuwe toepassing registreren**.

   ![App toevoegen](./media/howto-create-service-principal-portal/select-add-app.png)

1. Geef een naam en URL op voor de toepassing. Selecteer **Web-app/API** voor het type toepassing dat u wilt maken. U kunt geen referenties voor de maken een [systeemeigen toepassing](../manage-apps/application-proxy-configure-native-client-application.md). U kunt dit type niet gebruiken voor een geautomatiseerde toepassing. Na het instellen van de waarden, selecteer **maken**.

   ![toepassing een naam geven](./media/howto-create-service-principal-portal/create-app.png)

U hebt uw Azure AD-toepassing en service-principal gemaakt.

## <a name="assign-the-application-to-a-role"></a>De toepassing aan een rol toewijzen

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol biedt de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../../role-based-access-control/built-in-roles.md).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Bijvoorbeeld een toepassing toevoegt aan de rol van lezer voor een resourcegroep betekent dit dat het vindt de resourcegroep en alle resources die deze bevat.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **alle services** en **abonnementen**.

   ![Abonnement selecteren](./media/howto-create-service-principal-portal/select-subscription.png)

1. Selecteer het specifieke abonnement toewijzen van de toepassing.

   ![abonnement voor toewijzing selecteren](./media/howto-create-service-principal-portal/select-one-subscription.png)

   Als u het abonnement dat u zoekt niet ziet, selecteert u **filter globale abonnementen**. Zorg ervoor dat het gewenste abonnement is geselecteerd voor de portal. 

1. Selecteer **toegangsbeheer (IAM)**.
1. Selecteer **Toevoegen**.

   ![Selecteer toevoegen](./media/howto-create-service-principal-portal/select-add.png)

1. Selecteer de rol die u wilt toewijzen aan de toepassing. Om toe te staan van de toepassing voor het uitvoeren van acties zoals **opnieuw opstarten**, **start** en **stoppen** instanties, selecteer de **Inzender** rol. Azure AD-toepassingen worden niet standaard weergegeven in de beschikbare opties. Als u uw toepassing zoekt, zoeken naar de naam en selecteer deze.

   ![Rol selecteren](./media/howto-create-service-principal-portal/select-role.png)

1. Selecteer **opslaan** voltooien van de rol toe te wijzen. U ziet uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

Uw service-principal is ingesteld. U kunt beginnen met het uw scripts of apps uit te voeren. De volgende sectie wordt beschreven hoe om waarden die nodig zijn bij het aanmelden via een programma te verkrijgen.

## <a name="get-values-for-signing-in"></a>Waarden ophalen voor het aanmelden

### <a name="get-tenant-id"></a>Tenant-id ophalen

Wanneer u zich programmatisch aanmeldt, moet u de tenant-ID met uw verificatieaanvraag doorgeven.

1. Selecteer **Azure Active Directory**.
1. Selecteer **eigenschappen**.

   ![Azure AD-eigenschappen selecteren](./media/howto-create-service-principal-portal/select-ad-properties.png)

1. Kopieer de **map-ID** om op te halen van uw tenant-ID.

   ![Tenant-id](./media/howto-create-service-principal-portal/copy-directory-id.png)

### <a name="get-application-id-and-authentication-key"></a>Toepassing-ID en verificatiesleutel sleutel ophalen

U moet ook de ID voor uw toepassing en een verificatiesleutel nodig. U kunt deze waarden als volgt ophalen:

1. Van **App-registraties** in Azure AD, selecteer uw toepassing.

   ![Toepassing selecteren](./media/howto-create-service-principal-portal/select-app.png)

1. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode.

   ![Client-id](./media/howto-create-service-principal-portal/copy-app-id.png)

1. Selecteer **instellingen**.

   ![instellingen selecteren](./media/howto-create-service-principal-portal/select-settings.png)

1. Selecteer **Sleutels**.
1. Geef een beschrijving op van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

   ![sleutel opslaan](./media/howto-create-service-principal-portal/save-key.png)

   Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Deze waarde niet kopiëren omdat u niet kunnen ophalen van de sleutel later opnieuw. U geeft de sleutelwaarde samen met de toepassings-ID aan te melden als de toepassing. Bewaar de sleutelwaarde op een locatie waar de toepassing deze kan ophalen.

   ![opgeslagen sleutel](./media/howto-create-service-principal-portal/copy-key.png)

## <a name="required-permissions"></a>Vereiste machtigingen

U moet voldoende rechten hebt voor een toepassing registreren met uw Azure AD-tenant en de toepassing toewijzen aan een rol in uw Azure-abonnement.

### <a name="check-azure-ad-permissions"></a>Azure AD-machtigingen controleren

1. Selecteer **Azure Active Directory**.
1. Houd er rekening mee uw rol. Als u hebt de **gebruiker** rol, moet u ervoor zorgen dat niet-beheerders kunnen toepassingen registreren.

   ![gebruiker zoeken](./media/howto-create-service-principal-portal/view-user-info.png)

1. Selecteer **gebruikersinstellingen**.

   ![selecteert u gebruikersinstellingen](./media/howto-create-service-principal-portal/select-user-settings.png)

1. Controleer de **App-registraties** instelling. Deze waarde kan alleen worden ingesteld door een beheerder. Indien ingesteld op **Ja**, een app kunt registreren door een gebruiker in de Azure AD-tenant.

   ![app-registraties weergeven](./media/howto-create-service-principal-portal/view-app-registrations.png)

Als de instelling app-registraties is ingesteld op **Nee**, alleen [globale beheerders](../users-groups-roles/directory-assign-admin-roles.md) apps kunt registreren. Als uw account is toegewezen aan de gebruikersrol, maar de app-instelling voor registratie beperkt tot gebruikers van de beheerder is, vraagt u uw beheerder om een toewijzen aan de rol globale beheerder of zodat gebruikers om apps te registreren.

### <a name="check-azure-subscription-permissions"></a>Controleer de machtigingen van de Azure-abonnement

In uw Azure-abonnement, moet uw account beschikt over `Microsoft.Authorization/*/Write` toegang tot een AD-app toewijst aan een rol. Deze toegang wordt verleend via de rol [Eigenaar](../../role-based-access-control/built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](../../role-based-access-control/built-in-roles.md#user-access-administrator). Als uw account is toegewezen aan de **Inzender** rol, u hebt geen machtiging voldoende. U ontvangt een foutmelding wanneer u probeert de service-principal toewijzen aan een rol.

Uw om abonnementsmachtigingen te controleren:

1. Selecteer uw account in de rechterbovenhoek en selecteer **mijn machtigingen**.

   ![gebruikersmachtigingen selecteren](./media/howto-create-service-principal-portal/select-my-permissions.png)

1. Selecteer het abonnement dat u wilt dat de service-principal in maken in de vervolgkeuzelijst. Selecteer **Klik hier voor volledige toegang tot gegevens voor dit abonnement**.

   ![gebruiker zoeken](./media/howto-create-service-principal-portal/view-details.png)

1. De toegewezen rollen kunt weergeven en bepalen of u voldoende machtigingen voor een AD-app toewijst aan een rol hebt. Als dat niet het geval is, vraagt de beheerder van het abonnement u toe te voegen aan de rol Administrator voor gebruikerstoegang. In de volgende afbeelding wordt de gebruiker toegewezen aan de rol van eigenaar, wat betekent dat de gebruiker de juiste machtigingen heeft.

   ![machtigingen weergeven](./media/howto-create-service-principal-portal/view-user-role.png)

## <a name="next-steps"></a>Volgende stappen

* Als u een toepassing met meerdere tenants instelt, Zie [Developer's guide voor autorisatie met de Azure Resource Manager API](../../azure-resource-manager/resource-manager-api-authentication.md).
* Zie voor meer informatie over het opgeven van beleidsregels voor veiligheid, [Azure Role-based Access Control](../../role-based-access-control/role-assignments-portal.md).  
* Zie voor een lijst van beschikbare acties die kunnen worden verleend of geweigerd voor gebruikers, [Resourceprovider van Azure Resource Manager-bewerkingen](../../role-based-access-control/resource-provider-operations.md).
