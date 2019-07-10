---
title: Een on-premises app toevoegen - Application Proxy in Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) heeft een Application Proxy-service waarmee gebruikers toegang krijgen tot on-premises toepassingen door zich aan te melden met hun Azure AD-account. Deze zelfstudie leert u hoe u uw omgeving voorbereidt voor gebruik met Application Proxy. Vervolgens wordt de Azure portal om toe te voegen een on-premises toepassing naar uw Azure AD-tenant gebruikt.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: tutorial
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecff60d1a1f808c4021476d136fe014175451672
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723956"
---
# <a name="tutorial-add-an-on-premises-application-for-remote-access-through-application-proxy-in-azure-active-directory"></a>Zelfstudie: Een on-premises toepassing voor externe toegang toevoegen via Application Proxy in Azure Active Directory

Azure Active Directory (Azure AD) heeft een Application Proxy-service waarmee gebruikers toegang krijgen tot on-premises toepassingen door zich aan te melden met hun Azure AD-account. In deze zelfstudie wordt uw omgeving voorbereid voor gebruik van Application Proxy. Zodra uw omgeving gereed is, gebruikt u de Azure-portal om een on-premises toepassing toe te voegen aan uw Azure AD-tenant.

Deze zelfstudie:

> [!div class="checklist"]
> * Opent poorten voor uitgaand verkeer en verschaft toegang tot specifieke URL 's
> * Installeert de connector op uw Windows-server en registreert deze bij Application Proxy
> * Controleert of de connector juist is geïnstalleerd en geregistreerd
> * Voegt een toepassing toe aan uw Azure AD-tenant
> * Controleert of dat een testgebruiker kunt aanmelden bij de toepassing met behulp van een Azure AD-account

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt toevoegen van een on-premises toepassing naar Azure AD, hebt u het volgende nodig:

* Een [Microsoft Azure AD basic of premium-abonnement](https://azure.microsoft.com/pricing/details/active-directory)
* Een toepassing administrator-account
* Gebruikers-id's moeten worden gesynchroniseerd vanuit een on-premises directory of die rechtstreeks in uw Azure AD-tenants zijn gemaakt. Identiteitssynchronisatie kunt vooraf om gebruikers te verifiëren voordat ze toegang verlenen tot de App Proxy toepassingen gepubliceerde en dat de id-gegevens van de gebruiker nodig om uit te voeren eenmalige aanmelding (SSO) met Azure AD.

### <a name="windows-server"></a>Windows server

Als u Application Proxy wilt gebruiken, hebt u een Windows-server nodig waarop Windows Server 2012 R2 of hoger wordt uitgevoerd. U installeert de Application Proxy-connector op de server. Deze connectorserver moet verbinding maken met de Application Proxy-service in Azure en met de on-premises toepassingen die u van plan bent te publiceren.

Om een hoge beschikbaarheid in uw productieomgeving te realiseren wordt aangeraden meer dan één Windows-server te gebruiken. Voor deze zelfstudie is één Windows-server toereikend.

#### <a name="recommendations-for-the-connector-server"></a>Aanbevelingen voor de connectorserver

1. Plaats de connectorserver fysiek dicht bij de toepassingsservers om de prestaties tussen de connector en de toepassing te optimaliseren. Zie [Overwegingen bij de netwerktopologie](application-proxy-network-topology.md) voor meer informatie.
1. De connectorserver en de webservers van toepassingen moeten deel uitmaken van hetzelfde Active Directory-domein of vertrouwde domeinen. De servers die in hetzelfde domein of domeinen vertrouwen is vereist voor het gebruik van eenmalige aanmelding (SSO) met geïntegreerde Windows-verificatie (IWA) en Kerberos-beperkte delegatie (KCD). Als de connectorserver en de webtoepassingsservers zich in verschillende Active Directory-domeinen bevinden, moet u delegatie op basis van resources gebruiken voor eenmalige aanmelding. Zie [KCD voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md) voor meer informatie.

#### <a name="tls-requirements"></a>TLS-vereisten

Voor de Windows-connectorserver moet TLS 1.2 zijn ingeschakeld voordat u de Application Proxy-connector installeert.

TLS 1.2 inschakelen:

1. De volgende registersleutels instellen:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. De server opnieuw opstarten.

> [!IMPORTANT]
> Voor de best mogelijke codering voor onze klanten, stellen we updates voor de Application Proxy-service de toegang beperken tot alleen de protocollen TLS 1.2. Op basis van de gereedheid van de klant wijzigingen wordt geleidelijk aan worden geïmplementeerd op klanten die alleen gebruikmaakt van TLS 1.2-protocollen en eventuele gevolgen van deze wijziging niet meer te zien. TLS 1.0 en 1.1 afschaffing wordt voltooid op 31 augustus 2019 en klanten ontvangt van tevoren om voor te bereiden voor deze wijziging. Om voor te bereiden voor deze wijziging Zorg ervoor dat die alle combinaties van client / server- en browser-server bijgewerkt met het gebruik van TLS 1.2 verbinding met de service voor toepassingsproxy onderhouden. Het gaat hierbij om clients die uw gebruikers gebruiken voor toegang tot toepassingen die zijn gepubliceerd via toepassingsproxy. Zie voorbereidingen voor [TLS 1.2 in Office 365](https://support.microsoft.com/help/4057306/preparing-for-tls-1-2-in-office-365) voor handige naslaginformatie en bronnen.

## <a name="prepare-your-on-premises-environment"></a>Bereid uw on-premises omgeving voor

Begin met het inschakelen van communicatie met Azure-datacenters in uw omgeving voorbereiden voor Azure AD-toepassingsproxy. Als er een firewall in het pad, zorgt u ervoor dat deze open. Een open-firewall kunt de connector voor HTTPS (TCP) aanvragen naar de toepassingsproxy.

### <a name="open-ports"></a>Poorten openen

Open de volgende poorten voor **uitgaand** verkeer.

   | Poortnummer | Hoe dat wordt gebruikt |
   | --- | --- |
   | 80 | Het downloaden van certificaatintrekkingslijsten (CRL's) tijdens het valideren van het SSL-certificaat |
   | 443 | Alle uitgaande communicatie met de Application Proxy-service |

Als met uw firewall verkeer wordt afgedwongen op basis van de herkomst van gebruikers, open dan ook poorten 80 en 443 voor verkeer dat afkomstig is van Windows-services die als netwerkservice worden uitgevoerd.

Mogelijk hebt u een oudere versie van de connector is geïnstalleerd als u al Application Proxy. Volg deze zelfstudie voor het installeren van de meest recente versie van de connector. Voor versies ouder dan 1.5.132.0 moeten ook de volgende poorten zijn geopend: 5671, 8080, 9090-9091, 9350, 9352, 10100–10120.

### <a name="allow-access-to-urls"></a>Toegang tot URL's toestaan

Sta toegang tot de volgende URL's toe:

| URL | Hoe dat wordt gebruikt |
| --- | --- |
| \*.msappproxy.net<br>\*.servicebus.windows.net | Communicatie tussen de connector en de Application Proxy-cloudservice |
| mscrl.microsoft.com:80<br>crl.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Azure maakt gebruik van deze URL's om te controleren of certificaten. |
| login.windows.net<br>login.microsoftonline.com<br>secure.aadcdn.microsoftonline-p.com  | De connector gebruikt deze URL's tijdens het registratieproces. |

U kunt verbindingen met \*. msappproxy.net en \*. servicebus.windows.net als u uw firewall of proxy configureren van DNS toestaan lijsten. Als dat niet het geval is, moet u toegang toestaan tot de [IP-bereiken van Azure DataCenter](https://www.microsoft.com/download/details.aspx?id=41653), die overigens elke week worden bijgewerkt.

## <a name="install-and-register-a-connector"></a>Een connector installeren en registreren

Voor het gebruik van Application Proxy, door een connector te installeren op elke Windows-server die u met de Application Proxy-service. De connector is een agent die de uitgaande verbinding vanaf de on-premises toepassingsservers naar Application Proxy in Azure AD beheert. U kunt een connector installeren op servers waarop ook andere verificatie-agents zijn geïnstalleerd, zoals Azure AD Connect.

De connector installeren:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als een toepassingsbeheerder van de map die gebruikmaakt van Application Proxy. Als het domein van de tenant bijvoorbeeld contoso.com is, moet de beheerder admin@contoso.com of een andere beheerdersalias in dat domein zijn.
1. Selecteer uw gebruikersnaam in de rechterbovenhoek. Controleer of dat u bent aangemeld bij een map die gebruikmaakt van de toepassingsproxy. Als u wijzigen van mappen wilt, selecteert u **schakelen tussen mappen** en kiest u een map die gebruikmaakt van de toepassingsproxy.
1. Selecteer in het linkernavigatievenster, **Azure Active Directory**.
1. Onder **beheren**, selecteer **toepassingsproxy**.
1. Selecteer **service-connector downloaden**.

    ![Service-connector om te zien van de servicevoorwaarden downloaden](./media/application-proxy-add-on-premises-application/application-proxy-download-connector-service.png)

1. Lees de servicevoorwaarden. Wanneer u klaar bent, selecteert u **voorwaarden accepteren en downloaden**.
1. Selecteer aan de onderkant van het venster **uitvoeren** om de connector te installeren. Er wordt een installatiewizard geopend.
1. Volg de instructies in de wizard voor het installeren van de service. Wanneer u wordt gevraagd de connector te registreren voor de Application Proxy voor uw Azure AD-tenant, geeft u uw gegevens als toepassingsbeheerder op.
    - Voor Internet Explorer (IE) geldt dat als **Verbeterde beveiliging van Internet Explorer** is ingesteld op **Aan**, het registratiescherm niet wordt weergegeven. Volg de instructies in het foutbericht om toegang te krijgen. Zorg ervoor dat **verbeterde beveiliging van Internet Explorer** is ingesteld op **uit**.

### <a name="general-remarks"></a>Algemene opmerkingen

Als u al een connector hebt geïnstalleerd, voert u een nieuwe installatie uit met de meest recente versie. Voor informatie over de eerder uitgebrachte versies en welke wijzigingen ze omvatten, Zie [Application Proxy: Versiegeschiedenis van Release](application-proxy-release-version-history.md).

Als u meer dan één Windows-server voor uw on-premises toepassingen wilt, moet u de connector op elke server installeren en registreren. U kunt de connectors onderverdelen in connectorgroepen. Zie [Connectorgroepen](application-proxy-connector-groups.md) voor meer informatie.

Als uw organisatie proxyservers gebruikt om verbinding met internet te maken, moet u ze configureren voor Application Proxy.  Zie [Werken met bestaande on-premises proxyservers](application-proxy-configure-connectors-with-proxy-servers.md) voor meer informatie. 

Zie [Informatie over Azure AD Application Proxy-connectors](application-proxy-connectors.md) voor meer informatie over connectors, capaciteitsplanning en hoe connectors up-to-date blijven.

## <a name="verify-the-connector-installed-and-registered-correctly"></a>Controleer of de connector juist is geïnstalleerd en geregistreerd

U kunt de Azure-portal of uw Windows-server gebruiken om te bevestigen dat er een nieuwe connector correct is geïnstalleerd.

### <a name="verify-the-installation-through-azure-portal"></a>Controleer of de installatie via Azure portal

Controleren of de connector juist is geïnstalleerd en geregistreerd:

1. Meld u aan bij uw tenantmap in de [Azure-portal](https://portal.azure.com).
1. Selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**, en selecteer vervolgens **Application Proxy** onder de **beheren** sectie. Al uw connectors en connectorgroepen worden op deze pagina weergegeven.
1. Een connector om te controleren of de details ervan bekijken. De connectors zijn standaard uitgevouwen. Als de connector die u wilt weergeven, niet is uitgevouwen, vouwt u de connector om de details weer te geven. Een actief groen label geeft aan dat de connector verbinding kan maken met de service. Maar ook al is het label groen, toch kan een netwerkprobleem er nog steeds voor zorgen dat de connector geen berichten ontvangt.

    ![Azure AD Application Proxy Connectors](./media/application-proxy-connectors/app-proxy-connectors.png)

Zie voor meer hulp bij het installeren van een connector [problemen bij het installeren van de Connector voor toepassingsproxy](application-proxy-connector-installation-problem.md).

### <a name="verify-the-installation-through-your-windows-server"></a>Controleer of de installatie via uw Windows-server

Controleren of de connector juist is geïnstalleerd en geregistreerd:

1. Open Windows Services Manager door te klikken op de **Windows**-toets en *services.msc* in te voeren.
1. Controleer of de status van de volgende twee services **Wordt uitgevoerd** is.
   - **Microsoft AAD Application Proxy Connector** maakt de connectiviteit mogelijk.
   - **Microsoft AAD Application Proxy Connector Updater** is een automatische updateservice. De updater controleert op nieuwe versies van de connector en werkt de connector bij als dat nodig is.

     ![Connectorservices voor toepassingsproxy - schermafbeelding](./media/application-proxy-enable/app_proxy_services.png)

1. Als de status van de services niet **met**, klik met de rechtermuisknop voor het selecteren van elke service en kies **Start**.

## <a name="add-an-on-premises-app-to-azure-ad"></a>Een on-premises app toevoegen aan Azure AD

Nu u uw omgeving hebt voorbereid en een connector hebt geïnstalleerd, kunt u on-premises toepassingen gaan toevoegen aan Azure AD.  

1. Meld u aan als beheerder in de [Azure-portal](https://portal.azure.com/).
1. Selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.
1. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **nieuwe toepassing**.
1. Selecteer **On-premises toepassing**.  
1. In de **toevoegen van uw eigen on-premises toepassing** sectie, geef de volgende informatie over uw toepassing:

    | Veld | Description |
    | :---- | :---------- |
    | **Name** | De naam van de toepassing die wordt weergegeven in het toegangsvenster en in de Azure-portal. |
    | **Interne URL** | Dit is de URL voor toegang tot de toepassing vanuit uw particuliere netwerk. U kunt voor het publiceren een specifiek pad opgeven op de back-endserver, terwijl de rest van de server ongepubliceerd blijft. Op deze manier kunt u verschillende sites op dezelfde server als verschillende apps publiceren en elk daarvan een eigen naam en toegangsregels geven.<br><br>Als u een pad publiceert, moet u ervoor zorgen dat dit alle benodigde installatiekopieën, scripts en opmaakmodellen voor uw toepassing bevat. Bijvoorbeeld, als uw app naar https:\//yourapp /-app en maakt gebruik van installatiekopieën te vinden op https:\//yourapp/media, dan hebt u https moet publiceren:\//yourapp/ als het pad. Deze interne URL hoeft niet de bestemmingspagina te zijn die uw gebruikers te zien krijgen. Zie [Een aangepaste startpagina voor gepubliceerde apps instellen](application-proxy-configure-custom-home-page.md) voor meer informatie. |
    | **Externe URL** | Het adres voor gebruikers om toegang te krijgen tot de app van buiten uw netwerk. Als u het standaarddomein voor Application Proxy niet wilt gebruiken, lees dan de informatie over [aangepaste domeinen in Azure AD Application Proxy](application-proxy-configure-custom-domain.md).|
    | **Verificatie vooraf** | De manier waarop gebruikers door Application Proxy worden geverifieerd voordat ze toegang krijgen tot uw toepassing.<br><br>**Azure Active Directory**: de gebruikers worden omgeleid door Application Proxy zodat ze zich kunnen aanmelden met Azure AD. Hierbij worden hun machtigingen geverifieerd voor de map en de toepassing. We raden deze optie als de standaard zodat u van Azure AD-beveiligingsfuncties, zoals voorwaardelijke toegang en multi-factor Authentication profiteren kunt. **Azure Active Directory** is vereist voor het bewaken van de toepassing met Microsoft Cloud Application Security.<br><br>**Passthrough** -gebruikers niet hoeven te verifiëren bij Azure AD voor toegang tot de toepassing. U kunt nog steeds verificatievereisten op de back-end instellen. |
    | **Connectorgroep** | Connectors verwerken de externe toegang tot uw toepassing en met connectorgroepen kunt u connectors en toepassingen indelen per regio, netwerk of doel. Als u nog geen connectorgroepen hebt gemaakt, wordt uw toepassing toegewezen als **Standaard**.<br><br>Als uw toepassing gebruikmaakt van WebSockets om verbinding te maken, moeten alle connectors in de groep versie 1.5.612.0 of hoger hebben.|

1. Configureer zo nodig**aanvullende instellingen**. Voor de meeste toepassingen moet u voor deze instellingen de standaardwaarden behouden. 

    | Veld | Description |
    | :---- | :---------- |
    | **Toepassingstime-out voor de back-end** | Stel deze waarde alleen in op **Lang** als uw toepassing traag is met verifiëren en verbinding maken. |
    | **Alleen-HTTP-cookies gebruiken** | Stel deze waarde in op **Ja** om ervoor te zorgen dat Application Proxy-cookies de HTTPOnly-vlag in de HTTP-antwoordheader bevatten. Als u Extern bureaublad-services gebruikt, stelt u deze waarde in op **Nee**.|
    | **Beveiligde cookies gebruiken**| Stel deze waarde in op **Ja** om cookies te verzenden via een beveiligd kanaal, zoals een versleutelde HTTPS-aanvraag.
    | **Permanente cookies gebruiken**| Houd deze waarde ingesteld op **Nee**. Gebruik deze instelling alleen voor toepassingen die cookies tussen processen niet delen. Zie voor meer informatie over cookie-instellingen, [Cookie-instellingen voor toegang tot on-premises toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).
    | **URL's in headers vertalen** | Laat deze waarde op **Ja** staan, tenzij voor uw toepassing de oorspronkelijke host-header in de verificatieaanvraag moet zijn opgenomen. |
    | **URL's vertalen in de hoofdtekst van de toepassing** | Deze waarde als houden **Nee** , tenzij u vastgelegd HTML-koppelingen naar andere on-premises toepassingen hebt en aangepaste domeinen niet gebruiken. Zie [Vertaling koppelen aan Application Proxy](application-proxy-configure-hard-coded-link-translation.md) voor meer informatie.<br><br>Stel deze waarde in op **Ja** als u van plan bent om deze toepassing te bewaken met Microsoft Cloud App Security (MCAS). Zie voor meer informatie, [realtime toegang toepassingsbewaking configureren met Microsoft Cloud App Security en Azure Active Directory](application-proxy-integrate-with-microsoft-cloud-application-security.md). |

1. Selecteer **Toevoegen**.

## <a name="test-the-application"></a>De toepassing testen

U kunt nu testen of de toepassing correct is toegevoegd. Hiervoor gaat u in de volgende stappen een gebruikersaccount toevoegen aan de toepassing en u vervolgens proberen aan te melden.

### <a name="add-a-user-for-testing"></a>Een gebruiker toevoegen voor het testen

Controleer voordat u een gebruiker aan de toepassing toevoegt of het gebruikersaccount al machtigingen heeft voor toegang tot de toepassing vanuit het bedrijfsnetwerk.

Een testgebruiker toevoegen:

1. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens de toepassing die u wilt testen.
1. Selecteer **aan de slag**, en selecteer vervolgens **een gebruiker toewijzen voor het testen van**.
1. Onder **gebruikers en groepen**, selecteer **gebruiker toevoegen**.
1. Onder **toewijzing toevoegen**, selecteer **gebruikers en groepen**. De **gebruikers en groepen** sectie wordt weergegeven.
1. Kiezen welk account die u wilt toevoegen.
1. Kies **Selecteer**, en selecteer vervolgens **toewijzen**.

### <a name="test-the-sign-on"></a>De aanmelding testen

Voor het testen van de aanmelding bij de toepassing:

1. Navigeer in uw browser naar de externe URL die u hebt geconfigureerd tijdens de stap voor het publiceren. Als het goed is, ziet u nu het startscherm.
1. Meld u aan als de gebruiker die u in de vorige sectie hebt gemaakt.

Zie [Problemen en foutberichten met Application Proxy oplossen](application-proxy-troubleshoot.md) voor het oplossen van problemen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u uw on-premises omgeving voorbereid om te werken met Application Proxy en hebt u vervolgens de Application Proxy-connector geïnstalleerd en geregistreerd. Vervolgens hebt u een toepassing toegevoegd aan uw Azure AD-tenant. U hebt ook gecontroleerd of een gebruiker zich kan aanmelden bij de toepassing met behulp van een Azure AD-account

U hebt het volgende gedaan:
> [!div class="checklist"]
> * Poorten geopend voor uitgaand verkeer en toegang verschaft tot specifieke URL 's
> * De connector geïnstalleerd op uw Windows-server en deze geregistreerd bij Application Proxy
> * Gecontroleerd of de connector juist is geïnstalleerd en geregistreerd
> * Een toepassing toegevoegd aan uw Azure AD-tenant
> * Geverifieerd met dat een testgebruiker kunt aanmelden bij de toepassing met behulp van een Azure AD-account

U kunt de toepassing nu gaan configureren voor eenmalige aanmelding. Gebruik de volgende koppeling om te kiezen van een methode voor eenmalige aanmelding en zelfstudies voor eenmalige aanmelding.

> [!div class="nextstepaction"]
> [Eenmalige aanmelding configureren](what-is-single-sign-on.md#choosing-a-single-sign-on-method)
