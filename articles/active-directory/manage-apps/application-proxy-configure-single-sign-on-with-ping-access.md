---
title: Verificatie op basis van een koptekst met PingAccess voor Azure AD-toepassingsproxy | Microsoft Docs
description: Publiceren van toepassingen met PingAccess en App-Proxy voor de ondersteuning van verificatie op basis van een koptekst.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 365f017fe7d71500c17d0a9ccd9c5a0a26a78b75
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989625"
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Koptekst gebaseerde verificatie voor eenmalige aanmelding met de toepassingsproxy en PingAccess

Toepassingsproxy van Azure Active Directory (Azure AD) is een partnerschap aangegaan met PingAccess zodat uw klanten van Azure AD toegang hebben tot informatie van uw toepassingen. PingAccess breidt de [bestaande Application Proxy-aanbiedingen](application-proxy.md) om op te nemen van eenmalige aanmelding toegang tot toepassingen die headers voor verificatie gebruiken.

## <a name="whats-pingaccess-for-azure-ad"></a>Wat is PingAccess voor Azure AD?

Met PingAccess voor Azure AD, kunt u gebruikerstoegang en eenmalige aanmelding (SSO) kunt geven tot toepassingen die headers voor verificatie gebruiken. Application Proxy behandelt deze toepassingen, zoals een andere, met behulp van Azure AD verifiëren van toegang en vervolgens verkeer via de connector-service. PingAccess bevindt zich voor de toepassingen en zet het toegangstoken van Azure AD in een header. De toepassing wordt vervolgens ontvangt de verificatie in de indeling die kan gelezen.

Uw gebruikers wordt niet ziet u iets anders wanneer ze zich aanmelden bij het gebruik van uw zakelijke toepassingen. Ze kunnen nog steeds overal werken vanaf op elk apparaat. De Application Proxy connectors rechtstreeks extern verkeer naar alle apps, ongeacht het verificatietype, zodat ze u nog steeds geladen automatisch saldo.

## <a name="how-do-i-get-access"></a>Hoe krijg ik toegang?

Aangezien dit scenario afkomstig van een verbinding tussen Azure Active Directory en PingAccess is, moet u licenties voor beide services. Azure Active Directory Premium-abonnementen hebben echter een basic PingAccess-licentie die betrekking heeft op maximaal 20 toepassingen. Als u meer dan 20 headers gebaseerde toepassingen publiceert wilt, kunt u een extra licentie van PingAccess kopen.

Zie [Azure Active Directory-edities](../fundamentals/active-directory-whatis.md) voor meer informatie.

## <a name="publish-your-application-in-azure"></a>Uw toepassing publiceren in Azure

Dit artikel is voor gebruikers voor het publiceren van een toepassing met dit scenario voor het eerst. Behalve met gedetailleerde informatie over de stappen publiceren, deze beschrijft hoe u aan de slag met zowel de toepassingsproxy en PingAccess. Als u beide services al hebt geconfigureerd, maar een opfrisser over het publiceren stappen wilt, gaat u naar de [uw toepassing toevoegen aan Azure AD met Application Proxy](#add-your-application-to-azure-ad-with-application-proxy) sectie.

> [!NOTE]
> Omdat dit scenario een partnerschap tussen Azure AD is en PingAccess, enkele van de instructies aanwezig zijn op de Ping Identity-site.

### <a name="install-an-application-proxy-connector"></a>Een Application Proxy-connector installeren

Als u hebt ingeschakeld Application Proxy ingeschakeld en een connector al hebt geïnstalleerd, kunt u deze sectie overslaan en gaat u naar [uw toepassing toevoegen aan Azure AD met Application Proxy](#add-your-application-to-azure-ad-with-application-proxy).

De Application Proxy-connector is een Windows Server-service die zorgt ervoor het verkeer van uw externe medewerkers naar uw gepubliceerde toepassingen dat. Zie voor meer installatie-instructies gedetailleerde, [zelfstudie: Toevoegen van een on-premises toepassing voor externe toegang via Application Proxy in Azure Active Directory](application-proxy-add-on-premises-application.md).

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/) als een beheerder van de toepassing. De **Azure Active Directory-beheercentrum** pagina wordt weergegeven.
2. Selecteer **Azure Active Directory** > **toepassingsproxy** > **service-connector downloaden**. De **Application Proxy Connector downloaden** pagina wordt weergegeven.

   ![Application proxy-connector downloaden](./media/application-proxy-configure-single-sign-on-with-ping-access/application-proxy-connector-download.png)
3. Volg de installatie-instructies.

Downloaden van de connector moet automatisch toepassingsproxy inschakelen voor uw directory, maar als dit niet het geval is, kunt u **Enable Application Proxy**.

### <a name="add-your-application-to-azure-ad-with-application-proxy"></a>Uw toepassing toevoegen aan Azure AD met Application Proxy

Er zijn twee acties die u moet uitvoeren in Azure portal. Eerst moet u uw toepassing met Application Proxy publiceren. Vervolgens moet u enkele gegevens over de toepassing die u tijdens de stappen PingAccess gebruiken kunt verzamelen.

#### <a name="publish-your-application"></a>Uw toepassing publiceren

Eerst hebt u uw toepassing wilt publiceren. Deze actie omvat:

- Uw on-premises toepassing naar Azure AD toevoegen
- Een gebruiker toewijzen voor het testen van de toepassing en koptekst gebaseerde SSO kiezen
- Instellen van de omleidings-URL van de toepassing
- Verlenen van machtigingen voor gebruikers en andere toepassingen bij het gebruik van uw on-premises toepassing

Uw eigen on-premises toepassing publiceren:

1. Als u niet hebt gedaan in de laatste sectie, aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/) als een beheerder van de toepassing.
2. Selecteer **bedrijfstoepassingen** > **nieuwe toepassing** > **On-premises toepassing**. De **toevoegen van uw eigen on-premises toepassing** pagina wordt weergegeven.

   ![Uw eigen on-premises toepassing toevoegen](./media/application-proxy-configure-single-sign-on-with-ping-access/add-your-own-on-premises-application.png)
3. Vul de vereiste velden met informatie over uw nieuwe toepassing. Gebruik de richtlijnen hieronder voor de instellingen.

   > [!NOTE]
   > Zie voor een meer gedetailleerd overzicht van deze stap, [een on-premises app toevoegen aan Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).

   1. **Interne URL**: Normaal gesproken bieden u de URL die u naar de aanmeldingspagina van de app gaat wanneer u zich in het bedrijfsnetwerk bevinden. Voor dit scenario moet de connector de proxy PingAccess behandelen als de front-pagina van de toepassing. Gebruik de volgende notatie: `https://<host name of your PingAccess server>:<port>`. De poort is 3000 standaard, maar u kunt deze configureren in PingAccess.

      > [!WARNING]
      > Voor dit type van eenmalige aanmelding, de interne URL moet gebruiken `https` en kan niet worden gebruikt `http`.

   2. **Methode voor verificatie vooraf**: Choose **Azure Active Directory**.
   3. **Vertalen van URL in de Headers**: Kies **Nee**.

   > [!NOTE]
   > Als dit uw eerste toepassing, moet u poort 3000 gebruiken om te starten en keert u terug naar het bijwerken van deze instelling als u de configuratie van uw PingAccess wijzigt. Voor de volgende toepassingen moet de poort overeenkomen met de Listener die u hebt geconfigureerd in PingAccess. Meer informatie over [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).
4. Selecteer **Toevoegen**. De overzichtspagina voor de nieuwe toepassing wordt weergegeven.

Nu een gebruiker voor het testen van toepassingen toewijzen en kies headers gebaseerde eenmalige aanmelding:

1. Selecteer in de zijbalk toepassing **gebruikers en groepen** > **gebruiker toevoegen** > **gebruikers en groepen (\<nummer > geselecteerde)** . Er verschijnt een lijst van gebruikers en groepen voor u om uit te kiezen.

   ![Gebruikers en groepen](./media/application-proxy-configure-single-sign-on-with-ping-access/users-and-groups.png)
2. Selecteer een gebruiker voor het testen van toepassingen en selecteer **Selecteer**. Zorg ervoor dat deze testaccount toegang heeft tot de on-premises toepassing.
3. Selecteer **Toewijzen**.
4. Selecteer in de zijbalk toepassing **eenmalige aanmelding** > **headers gebaseerde**.

   > [!TIP]
   > Als dit de eerste keer is met behulp van headers gebaseerde eenmalige aanmelding, moet u PingAccess installeren. Als u wilt controleren of dat uw Azure-abonnement wordt automatisch gekoppeld aan uw PingAccess-installatie, gebruikt u de koppeling op deze pagina voor eenmalige aanmelding voor het downloaden van PingAccess. U kunt nu openen van de site voor het downloaden of keert u terug naar deze pagina later opnieuw.

   ![Headers gebaseerde aanmelding](./media/application-proxy-configure-single-sign-on-with-ping-access/sso-header.png)
5. Selecteer **Opslaan**.

Controleer vervolgens of uw Omleidings-URL is ingesteld op de externe URL:

1. Uit de **Azure Active Directory-beheercentrum** zijbalk Selecteer **Azure Active Directory** > **App-registraties**. Er wordt een lijst weergegeven van toepassingen.

   ![App-registraties](./media/application-proxy-configure-single-sign-on-with-ping-access/app-registrations.png)
2. Selecteer uw toepassing.
3. Selecteer de koppeling naast **omleidings-URI's**, met het nummer van omleidings-URI's voor web- en openbare clients instellen. De  **\<toepassingsnaam >-verificatie** pagina wordt weergegeven.
4. Controleer of de externe URL die u hebt toegewezen aan uw toepassing eerder de **omleidings-URI's** lijst. Als dit niet, de externe URL nu toevoegen, met behulp van een omleidings-URI type **Web**, en selecteer **opslaan**.

Ten slotte uw on-premises toepassing zo instellen dat gebruikers leestoegang hebben en andere toepassingen toegang voor lezen/schrijven hebben:

1. Uit de **App-registraties** zijbalk voor uw toepassing, selecteer **API-machtigingen** > **toevoegen van een machtiging**  >   **Microsoft-API's** > **Microsoft Graph**. De **aanvragen API-machtigingen** pagina voor **Microsoft Graph** wordt weergegeven, waarin de API's voor Windows Azure Active Directory.

   ![API-machtigingen aanvragen](./media/application-proxy-configure-single-sign-on-with-ping-access/required-permissions.png)
2. Selecteer **overgedragen machtigingen** > **gebruiker** > **User.Read**.
3. Selecteer **Toepassingsmachtigingen** > **toepassing** > **Application.ReadWrite.All**.
4. Selecteer **machtigingen toevoegen**.
5. In de **API-machtigingen** weergeeft, schakelt **beheerder toestemming voor \<de directorynaam van uw >**.

#### <a name="collect-information-for-the-pingaccess-steps"></a>Voor de stappen PingAccess informatie verzamelen

U moet verzamelen van deze drie soorten informatie (alle GUID's) voor het instellen van uw toepassing met PingAccess:

| Naam van Azure AD-veld | Naam van PingAccess veld | Gegevensindeling |
| --- | --- | --- |
| **ID van de toepassing (client)** | **Client ID** | GUID |
| **Directory (tenant)-ID** | **Verlener** | GUID |
| `PingAccess key` | **Client Secret** | Willekeurige tekenreeks |

Voor het verzamelen van deze informatie:

1. Uit de **Azure Active Directory-beheercentrum** zijbalk Selecteer **Azure Active Directory** > **App-registraties**. Er wordt een lijst weergegeven van toepassingen.
2. Selecteer uw toepassing. De **App-registraties** pagina voor uw toepassing wordt weergegeven.

   ![Registratieoverzicht van de voor een toepassing](./media/application-proxy-configure-single-sign-on-with-ping-access/registration-overview-for-an-application.png)
3. Naast de **(client) toepassings-ID** waarde, selecteer de **naar Klembord kopiëren** pictogram, kopieer en bewaar de. U opgeven deze waarde later als PingAccess van client-ID.
4. Volgende de **map (tenant)-ID** waarde, selecteert u ook **naar Klembord kopiëren**en kopieer en bewaar de. Deze waarde die u als uitgever van PingAccess later opgeven.
5. In de zijbalk van de **App-registraties** voor uw toepassing, selecteert u **certificaten en geheimen** > **nieuwe clientgeheim**. De **toevoegen van een clientgeheim** pagina wordt weergegeven.

   ![Een clientgeheim toevoegen](./media/application-proxy-configure-single-sign-on-with-ping-access/add-a-client-secret.png)
6. In **beschrijving**, type `PingAccess key`.
7. Onder **verloopt**, kiezen hoe u de sleutel PingAccess instellen: **Na één jaar**, **na twee jaar**, of **nooit**.
8. Selecteer **Toevoegen**. De PingAccess sleutel wordt weergegeven in de tabel van geheimen van de client, met een willekeurige tekenreeks die autofills in de **waarde** veld.
9. Naast de PingAccess-sleutel **waarde** veld, selecteer de **naar Klembord kopiëren** pictogram, kopieer en bewaar de. U opgeven deze waarde later als het clientgeheim van PingAccess.

### <a name="update-graphapi-to-send-custom-fields-optional"></a>Update GraphAPI voor het verzenden van aangepaste velden (optioneel)

Zie voor een lijst van beveiligingstokens die Azure AD wordt verzonden voor verificatie, [Microsoft identity-platform-ID-tokens](../develop/id-tokens.md). Als u een aangepaste claim waarmee andere tokens worden verzonden, stel de `acceptMappedClaims` Toepassingsveld `True`. U kunt Graph Explorer of een manifest van de toepassing van de Azure AD-portal om deze wijziging te maken.

In dit voorbeeld maakt gebruik van Graph Explorer:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application>

{
  "acceptMappedClaims":true
}
```

In dit voorbeeld wordt de [Azure Active Directory-portal](https://aad.portal.azure.com/) om bij te werken de `acceptMappedClaims` veld:

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/) als een beheerder van de toepassing.
2. Selecteer **Azure Active Directory** > **App-registraties**. Er wordt een lijst weergegeven van toepassingen.
3. Selecteer uw toepassing.
4. In de zijbalk van de **App-registraties** -pagina voor uw toepassing, selecteert **Manifest**. Het manifest JSON-code voor de registratie van uw toepassing wordt weergegeven.
5. Zoek de `acceptMappedClaims` veld en wijzig de waarde in `True`.
6. Selecteer **Opslaan**.

### <a name="use-a-custom-claim-optional"></a>Gebruik een aangepaste claim (optioneel)

Als u wilt dat uw toepassing met een aangepaste claim en aanvullende velden opnemen, zorg ervoor dat u hebt ook [een aangepaste claims toewijzen van beleid gemaakt en toegewezen aan de toepassing](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

> [!NOTE]
> Voor het gebruik van een aangepaste claim, moet u ook een aangepast beleid gedefinieerd en toegewezen aan de toepassing hebben. Dit beleid moet alle vereiste aangepaste kenmerken bevatten.
>
> U kunt doen beleidsdefinitie en de toewijzing via PowerShell, Azure AD Graph Explorer of Microsoft Graph. Als u ze in PowerShell doet, moet u mogelijk eerst met `New-AzureADPolicy` en deze vervolgens toewijzen aan de toepassing met `Add-AzureADServicePrincipalPolicy`. Zie voor meer informatie, [Claims toewijzing beleidstoewijzing](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-application"></a>PingAccess downloaden en configureren van uw toepassing

Nu dat u alle stappen van de Azure Active Directory-instelling hebt voltooid, kunt u met het configureren van PingAccess op verplaatsen.

De gedetailleerde stappen voor het PingAccess die deel uitmaken van dit scenario blijven in de documentatie van Ping Identity. Volg de instructies in [PingAccess voor Azure AD om te beveiligen, toepassingen die zijn gepubliceerd met behulp van Microsoft Azure AD-toepassingsproxy configureren](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html) op de Ping Identity-website.

Deze stappen helpen u PingAccess installeren en instellen van een PingAccess-account (als u nog een hebt). Vervolgens, een Azure AD OIDC (OpenID Connect) om verbinding te maken, stelt u een token provider met de **map (tenant)-ID** waarde die u hebt gekopieerd uit de Azure AD-portal. Vervolgens voor het maken van een websessie op PingAccess, gebruikt u de **(client) toepassings-ID** en `PingAccess key` waarden. U kunt hierna Identiteitstoewijzing instellen en maken van een virtuele host, de site en de toepassing.

### <a name="test-your-application"></a>Uw toepassing testen

Als u al deze stappen hebt voltooid, moet uw toepassing actief en werkend. Als u wilt testen, open een browser en navigeer naar de externe URL die u hebt gemaakt toen u de toepassing in Azure hebt gepubliceerd. Meld u aan met de testaccount dat u hebt toegewezen aan de toepassing.

## <a name="next-steps"></a>Volgende stappen

- [PingAccess voor Azure AD om te beveiligen, toepassingen die zijn gepubliceerd met behulp van Microsoft Azure AD-toepassingsproxy configureren](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Eenmalige aanmelding bij toepassingen in Azure Active Directory](what-is-single-sign-on.md)
- [Oplossen van problemen met Application Proxy- en foutberichten](application-proxy-troubleshoot.md)
