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
ms.date: 01/16/2018
ms.author: tomfitz
ms.openlocfilehash: 89f2f834c84cd5ae50b2d5b9d46e792065080d8a
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Portal gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot

Wanneer u een toepassing die behoeften openen of wijzigen van resources hebt, moet u een Azure Active Directory (AD)-toepassing instellen en de vereiste machtigingen toewijzen aan deze. Deze aanpak is voorkeur boven het uitvoeren van de app met uw eigen referenties, omdat:

* U kunt machtigingen toewijzen aan de identiteit van de app die anders zijn dan uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot precies wat de app moet doen.
* U hoeft niet te wijzigen van de app referenties als uw verantwoordelijkheden wijzigt. 
* U kunt een certificaat gebruiken voor het automatiseren van verificatie bij het uitvoeren van een onbewaakt script.

Dit artikel laat zien hoe u deze stappen uitvoert via de portal. Het is gericht op een één-tenant-toepassing waarin de toepassing is bedoeld om uit te voeren binnen één organisatie. Doorgaans gebruikt toepassingen voor één tenant voor line-of-business-toepassingen die worden uitgevoerd binnen uw organisatie.

## <a name="required-permissions"></a>Vereiste machtigingen

Als u dit artikel, moet u voldoende rechten hebt voor een toepassing registreren met uw Azure AD-tenant en de toepassing toewijzen aan een rol in uw Azure-abonnement. Zorg ervoor dat de juiste machtigingen deze stappen uit te voeren.

### <a name="check-azure-active-directory-permissions"></a>Controleer de machtigingen voor Azure Active Directory

1. Aanmelden bij uw Azure-Account via de [Azure-portal](https://portal.azure.com).

1. Selecteer **Azure Active Directory**.

   ![Selecteer azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecteer in Azure Active Directory, **gebruikersinstellingen**.

   ![gebruikersinstellingen voor de selecteren](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Controleer de **App registraties** instelling. Indien ingesteld op **Ja**, niet-beheerders kunnen zich registreren AD-apps. Deze instelling betekent dat elke gebruiker in de Azure AD-tenant een app kunt registreren. U kunt verdergaan naar [controleren Azure-abonnement machtigingen](#check-azure-subscription-permissions).

   ![app-registraties weergeven](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Als de app-registraties instelling is ingesteld op **Nee**, alleen de beheerder gebruikers apps kunnen registreren. Controleer of uw account een beheerder voor de Azure AD-tenant. Selecteer **overzicht** en **een gebruiker zoeken** van snelle taken.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/find-user.png)

1. Zoek uw account en selecteren wanneer u deze vinden.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/show-user.png)

1. Selecteer voor uw account **functie Directory**.

   ![Directory-rol](./media/resource-group-create-service-principal-portal/select-directory-role.png)

1. De functie toegewezen directory weergeven in Azure AD. Als uw account is toegewezen aan de gebruikersrol, maar de registratie van app-instelling (van de voorgaande stappen) beperkt tot beheergebruikers is, vraag uw beheerder op een toewijzen aan een administrator-rol of waarmee gebruikers om apps te registreren.

   ![de rol weergeven](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a>Controleer de machtigingen van de Azure-abonnement

In uw Azure-abonnement, uw account moet hebben `Microsoft.Authorization/*/Write` toegang tot een AD-app aan een rol toewijzen. Deze actie wordt verleend via de [eigenaar](../active-directory/role-based-access-built-in-roles.md#owner) rol of [beheerder voor gebruikerstoegang](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) rol. Als uw account is toegewezen aan de **Inzender** rol, u hebt niet de juiste machtiging. U ontvangt een fout opgetreden bij een poging de service-principal toewijzen aan een rol.

Uw abonnement om machtigingen te controleren:

1. Als u niet al uw Azure AD-account van de voorgaande stappen hebt uitgevoerd bekijkt, selecteert u **Azure Active Directory** in het linkerdeelvenster.

1. Uw Azure AD-account vinden. Selecteer **overzicht** en **een gebruiker zoeken** van snelle taken.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/find-user.png)

1. Zoek uw account en selecteren wanneer u deze vinden.

   ![gebruiker zoeken](./media/resource-group-create-service-principal-portal/show-user.png)

1. Selecteer **Azure-resources**.

   ![Selecteer resources](./media/resource-group-create-service-principal-portal/select-azure-resources.png)

1. Bekijk uw toegewezen rollen en bepalen of u onvoldoende machtigingen hebt voor het toewijzen van een AD-app aan een rol. Als dat niet het geval is, vraagt de abonnementsbeheerder van uw u toe te voegen aan de rol beheerder voor gebruikerstoegang. In de volgende afbeelding wordt de gebruiker toegewezen aan de rol van eigenaar voor twee abonnementen, wat betekent dat die de gebruiker heeft onvoldoende machtigingen.

   ![machtigingen weergeven](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

1. Aanmelden bij uw Azure-Account via de [Azure-portal](https://portal.azure.com).
1. Selecteer **Azure Active Directory**.

   ![Selecteer azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Selecteer **App registraties**.

   ![Selecteer de app-registraties](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Selecteer **registratie van de nieuwe toepassing**.

   ![app toevoegen](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Geef een naam en de URL voor de toepassing. Selecteer **Web-app / API** voor het type van de toepassing die u wilt maken. U kunt geen referenties voor maken een **systeemeigen** toepassing in, dat type werkt daarom niet voor een geautomatiseerde toepassing. Na het instellen van de waarden, selecteer **maken**.

   ![de naam van toepassing](./media/resource-group-create-service-principal-portal/create-app.png)

U kunt uw toepassing hebt gemaakt.

## <a name="get-application-id-and-authentication-key"></a>Ophalen van de sleutel-ID en verificatie van toepassing

Wanneer u programmatisch zich aanmeldt, moet u de ID voor uw toepassing en een verificatiesleutel. Als u deze waarden, gebruikt u de volgende stappen uit:

1. Van **App registraties** in Azure Active Directory, selecteer uw toepassing.

   ![toepassing selecteren](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopieer de **toepassings-ID** en op te slaan in uw toepassingscode. Sommige [voorbeeldtoepassingen](#log-in-as-the-application) verwijzen naar deze waarde als de client-ID.

   ![Client-ID](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Selecteer voor het genereren van een verificatiesleutel **sleutels**.

   ![Selecteer sleutels](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Geef een beschrijving van de sleutel en een duur voor de sleutel. Wanneer u klaar bent, selecteer **opslaan**.

   ![sleutel opslaan](./media/resource-group-create-service-principal-portal/save-key.png)

   Na het opslaan van de sleutel, wordt de waarde van de sleutel weergegeven. Deze waarde niet kopiëren omdat u niet kan ophalen van de sleutel later. U kunt de sleutelwaarde opgeven met de toepassings-ID aan te melden als de toepassing. Slaat de waarde van de sleutel waar uw toepassing deze kan worden opgehaald.

   ![sleutel wordt opgeslagen](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Tenant-ID ophalen

Wanneer u programmatisch zich aanmeldt, moet u de tenant-ID met de verificatieaanvraag doorgeven.

1. Selecteer **Azure Active Directory**.

   ![Selecteer azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Als u de tenant-ID, selecteer **eigenschappen** voor uw Azure AD-tenant.

   ![Azure AD-eigenschappen selecteren](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopieer de **map-ID**. Deze waarde is uw tenant-ID.

   ![Tenant-ID](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Toepassing toewijzen aan rol

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol vertegenwoordigt de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md).

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

1. Zoeken naar uw toepassing en selecteer deze.

   ![Zoek app](./media/resource-group-create-service-principal-portal/search-app.png)

1. Selecteer **opslaan** voltooid de rol toe te wijzen. Ziet u uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

## <a name="log-in-as-the-application"></a>Meld u aan als de toepassing

Uw toepassing is nu ingesteld in Azure Active Directory. U hebt een ID en -sleutel moet worden gebruikt voor aanmelden als de toepassing. De toepassing is toegewezen aan een rol die hieraan bepaalde acties die kan uitvoeren. Zie voor informatie over logboekregistratie in als de toepassing via verschillende platforms:

* [PowerShell](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [Azure-CLI](resource-group-authenticate-service-principal-cli.md)
* [REST](/rest/api/#create-the-request)
* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Volgende stappen
* Als u een multitenant-toepassing instelt, Zie [Ontwikkelaarshandleiding voor autorisatie met de Azure Resource Manager-API](resource-manager-api-authentication.md).
* Zie voor meer informatie over het opgeven van beveiligingsbeleid [toegangsbeheer op basis van rollen in Azure](../active-directory/role-based-access-control-configure.md).  
* Zie voor een lijst met beschikbare acties die kunnen worden verleend of geweigerd voor gebruikers, [Resourceprovider van Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).
