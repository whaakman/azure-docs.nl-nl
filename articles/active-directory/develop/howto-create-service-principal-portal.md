---
title: ID voor Azure-app in de portal maken | Microsoft Docs
description: Beschrijft hoe u een nieuwe Azure Active Directory-toepassing en service-principal die kan worden gebruikt met de op rollen gebaseerd toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen te maken.
services: active-directory
documentationcenter: na
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2019
ms.author: ryanwi
ms.reviewer: tomfitz
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5bd1534b3f966051104a3f3ee389fb047ab258fc
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482811"
---
# <a name="how-to-use-the-portal-to-create-an-azure-ad-application-and-service-principal-that-can-access-resources"></a>Procedure: De portal gebruiken om een Azure AD-toepassing en service-principal die toegang hebben tot resources te maken

In dit artikel wordt beschreven hoe u een nieuwe Azure Active Directory (Azure AD)-toepassing en service-principal die kan worden gebruikt met de op rollen gebaseerd toegangsbeheer te maken. Wanneer u de code die u moet toegang tot of het wijzigen van resources hebt, kunt u een identiteit voor de app kunt maken. Deze identiteit staat bekend als een service-principal. U kunt vervolgens de vereiste machtigingen toewijzen aan de service-principal. Dit artikel leest u hoe het gebruik van de portal om de serviceprincipal te maken. Dit artikel gaat over de toepassing van een één tenant waar de toepassing is bedoeld om uit te voeren binnen één organisatie. Doorgaans gebruikt u één tenant toepassingen voor line-of-business-toepassingen die worden uitgevoerd binnen uw organisatie.

> [!IMPORTANT]
> In plaats van het maken van een service-principal, kunt u overwegen beheerde identiteiten voor Azure-resources voor de identiteit van uw toepassingen. Als uw code wordt uitgevoerd op een service die ondersteuning biedt voor beheerde identiteiten en toegang tot bronnen die ondersteuning bieden voor Azure AD-verificatie, zijn beheerde identiteiten een betere optie voor u. Zie voor meer informatie over beheerde identiteiten voor een Azure-resources, met inbegrip van welke services momenteel, [wat is beheerde identiteiten voor Azure-resources?](../managed-identities-azure-resources/overview.md).

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

We gaan direct naar het maken van de identiteit. Als u een probleem ondervindt, controleert u de [vereiste machtigingen](#required-permissions) om ervoor te zorgen dat uw account de identiteit kunt maken.

1. Meld u aan met uw Azure-Account via de [Azure-portal](https://portal.azure.com).
1. Selecteer **Azure Active Directory**.
1. Selecteer **App-registraties**.
1. Selecteer **registratie van nieuwe**.
1. Noem de toepassing. Selecteer een ondersteunde-account hebt getypt, waarmee wordt bepaald wie de toepassing kan gebruiken. Onder **omleidings-URI**, selecteer **Web** voor het type van de toepassing die u wilt maken. Voer de URI waar wordt naar het toegangstoken verzonden. U kunt geen referenties voor de maken een [systeemeigen toepassing](../manage-apps/application-proxy-configure-native-client-application.md). U kunt dit type niet gebruiken voor een geautomatiseerde toepassing. Na het instellen van de waarden, selecteer **registreren**.

   ![Typ een naam voor uw toepassing](./media/howto-create-service-principal-portal/create-app.png)

U hebt uw Azure AD-toepassing en service-principal gemaakt.

## <a name="assign-the-application-to-a-role"></a>De toepassing aan een rol toewijzen

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol biedt de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../../role-based-access-control/built-in-roles.md).

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Bijvoorbeeld een toepassing toevoegt aan de rol van lezer voor een resourcegroep betekent dit dat het vindt de resourcegroep en alle resources die deze bevat.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **alle services** en **abonnementen**.

   ![Een rol op het abonnementsbereik bijvoorbeeld toewijzen](./media/howto-create-service-principal-portal/select-subscription.png)

1. Selecteer het specifieke abonnement toewijzen van de toepassing.

   ![abonnement voor toewijzing selecteren](./media/howto-create-service-principal-portal/select-one-subscription.png)

   Als u het abonnement dat u zoekt niet ziet, selecteert u **filter globale abonnementen**. Zorg ervoor dat het gewenste abonnement is geselecteerd voor de portal.

1. Selecteer **toegangsbeheer (IAM)** .
1. Selecteer **roltoewijzing toevoegen**.
1. Selecteer de rol die u wilt toewijzen aan de toepassing. Om toe te staan van de toepassing voor het uitvoeren van acties zoals **opnieuw opstarten**, **start** en **stoppen** instanties, selecteer de **Inzender** rol. Azure AD-toepassingen worden niet standaard weergegeven in de beschikbare opties. Als u uw toepassing zoekt, zoeken naar de naam en selecteer deze.

   ![Selecteer de rol toewijzen aan de toepassing](./media/howto-create-service-principal-portal/select-role.png)

1. Selecteer **opslaan** voltooien van de rol toe te wijzen. U ziet uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

Uw service-principal is ingesteld. U kunt beginnen met het uw scripts of apps uit te voeren. De volgende sectie wordt beschreven hoe om waarden die nodig zijn bij het aanmelden via een programma te verkrijgen.

## <a name="get-values-for-signing-in"></a>Waarden ophalen voor het aanmelden

Wanneer u zich programmatisch aanmeldt, moet u de tenant-ID met uw verificatieaanvraag doorgeven. U moet ook de ID voor uw toepassing en een verificatiesleutel nodig. U kunt deze waarden als volgt ophalen:

1. Selecteer **Azure Active Directory**.
1. Van **App-registraties** in Azure AD, selecteer uw toepassing.
1. Kopieer de map (tenant)-ID en op te slaan in de code van uw toepassing.

    ![Kopieer de map (tenant-ID) en op te slaan in uw app-code](./media/howto-create-service-principal-portal/copy-tenant-id.png)

1. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode.

   ![Kopieer de ID van de toepassing (client)](./media/howto-create-service-principal-portal/copy-app-id.png)

## <a name="certificates-and-secrets"></a>Certificaten en geheimen
Daemon-toepassingen kunnen twee soorten referenties gebruiken om te verifiëren met Azure AD: certificaten en toepassingsgeheimen.  Wordt geadviseerd om een certificaat, maar u kunt ook een nieuwe toepassingsgeheim maken.

### <a name="upload-a-certificate"></a>Een certificaat uploaden

U kunt een bestaand certificaat gebruiken als u die hebt.  Desgewenst kunt u een zelfondertekend certificaat voor testdoeleinden. Open PowerShell en voer [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) met de volgende parameters voor het maken van een zelfondertekend certificaat in het certificaatarchief van de gebruiker op uw computer: `$cert=New-SelfSignedCertificate -Subject "CN=DaemonConsoleCert" -CertStoreLocation "Cert:\CurrentUser\My"  -KeyExportPolicy Exportable -KeySpec Signature`.  Exporteren van dit certificaat met de [gebruikerscertificaat beheren](/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) MMC-module toegankelijk is vanaf de Windows-Configuratiescherm.

Het certificaat uploaden:

1. Selecteer **certificaten en geheimen**.
1. Selecteer **certificaat uploaden** en selecteer het certificaat (een bestaand certificaat of de zelf-ondertekend certificaat u geëxporteerd).

    ![Certificaat uploaden selecteren en selecteer de gewenste om toe te voegen](./media/howto-create-service-principal-portal/upload-cert.png)

1. Selecteer **Toevoegen**.

Na de registratie van het certificaat met uw toepassing in de portal voor appregistratie, moet u om in te schakelen van de client-toepassingscode om het certificaat te gebruiken.

### <a name="create-a-new-application-secret"></a>Een nieuwe toepassingsgeheim maken

Als u ervoor kiest om niet te gebruiken een certificaat, kunt u een nieuwe toepassingsgeheim maken.

1. Selecteer **certificaten en geheimen**.
1. Selecteer **geheimen van de Client -> Nieuw clientgeheim**.
1. Geef een beschrijving van de geheime sleutel en een duur. Wanneer u klaar bent, selecteert u **toevoegen**.

   Na het opslaan van het clientgeheim, wordt de waarde van het clientgeheim weergegeven. Deze waarde niet kopiëren omdat u niet kunnen ophalen van de sleutel later opnieuw. U geeft de sleutelwaarde samen met de toepassings-ID aan te melden als de toepassing. Bewaar de sleutelwaarde op een locatie waar de toepassing deze kan ophalen.

   ![Kopieer de geheime waarde, omdat u dit later niet ophalen](./media/howto-create-service-principal-portal/copy-secret.png)

## <a name="required-permissions"></a>Vereiste machtigingen

U moet voldoende rechten hebt voor een toepassing registreren met uw Azure AD-tenant en de toepassing toewijzen aan een rol in uw Azure-abonnement.

### <a name="check-azure-ad-permissions"></a>Azure AD-machtigingen controleren

1. Selecteer **Azure Active Directory**.
1. Houd er rekening mee uw rol. Als u hebt de **gebruiker** rol, moet u ervoor zorgen dat niet-beheerders kunnen toepassingen registreren.

   ![Zoek uw rol. Als u een gebruiker bent, controleert u of niet-beheerders kunnen apps registreren](./media/howto-create-service-principal-portal/view-user-info.png)

1. Selecteer **gebruikersinstellingen**.
1. Controleer de **App-registraties** instelling. Deze waarde kan alleen worden ingesteld door een beheerder. Indien ingesteld op **Ja**, een app kunt registreren door een gebruiker in de Azure AD-tenant.

Als de instelling app-registraties is ingesteld op **Nee**mogen alleen gebruikers met een beheerdersrol dergelijke toepassingen kunnen registreren. Zie [beschikbare rollen](../users-groups-roles/directory-assign-admin-roles.md#available-roles) en [rolmachtigingen](../users-groups-roles/directory-assign-admin-roles.md#role-permissions) voor meer informatie over beschikbare beheerdersrollen en de specifieke machtigingen in Azure AD die worden toegewezen aan elke rol. Als uw account is toegewezen aan de gebruikersrol, maar de app-instelling voor registratie beperkt tot gebruikers van de beheerder is, vraagt u uw beheerder om u toewijzen aan een van de beheerdersrollen die kunnen maken en alle aspecten van app-registraties en om gebruikers te beheren apps registreren.

### <a name="check-azure-subscription-permissions"></a>Controleer de machtigingen van de Azure-abonnement

In uw Azure-abonnement, moet uw account beschikt over `Microsoft.Authorization/*/Write` toegang tot een AD-app toewijst aan een rol. Deze toegang wordt verleend via de rol [Eigenaar](../../role-based-access-control/built-in-roles.md#owner) of [Administrator voor gebruikerstoegang](../../role-based-access-control/built-in-roles.md#user-access-administrator). Als uw account is toegewezen aan de **Inzender** rol, u hebt geen machtiging voldoende. U ontvangt een foutmelding wanneer u probeert de service-principal toewijzen aan een rol.

Uw om abonnementsmachtigingen te controleren:

1. Selecteer uw account in de rechterbovenhoek en selecteer **... -> Mijn machtigingen**.

   ![Selecteer uw account en de machtigingen van de gebruiker](./media/howto-create-service-principal-portal/select-my-permissions.png)

1. Selecteer het abonnement dat u wilt dat de service-principal in maken in de vervolgkeuzelijst. Selecteer **Klik hier voor volledige toegang tot gegevens voor dit abonnement**.

   ![Selecteer het abonnement dat u wilt dat de service-principal in maken](./media/howto-create-service-principal-portal/view-details.png)

1. Selecteer **roltoewijzingen** weergeven van de toegewezen rollen en bepalen of u voldoende machtigingen voor een AD-app toewijst aan een rol hebt. Als dat niet het geval is, vraagt de beheerder van het abonnement u toe te voegen aan de rol Administrator voor gebruikerstoegang. In de volgende afbeelding wordt de gebruiker toegewezen aan de rol van eigenaar, wat betekent dat de gebruiker de juiste machtigingen heeft.

   ![In dit voorbeeld ziet dat de gebruiker is toegewezen aan de rol van eigenaar](./media/howto-create-service-principal-portal/view-user-role.png)

## <a name="next-steps"></a>Volgende stappen

* Als u een toepassing met meerdere tenants instelt, Zie [Developer's guide voor autorisatie met de Azure Resource Manager API](../../azure-resource-manager/resource-manager-api-authentication.md).
* Zie voor meer informatie over het opgeven van beleidsregels voor veiligheid, [Azure Role-based Access Control](../../role-based-access-control/role-assignments-portal.md).  
* Zie voor een lijst van beschikbare acties die kunnen worden verleend of geweigerd voor gebruikers, [Resourceprovider van Azure Resource Manager-bewerkingen](../../role-based-access-control/resource-provider-operations.md).
