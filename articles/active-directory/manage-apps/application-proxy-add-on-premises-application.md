---
title: Een on-premises app toevoegen - Application Proxy in Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) heeft een Application Proxy-service waarmee gebruikers toegang krijgen tot on-premises toepassingen door zich aan te melden met hun Azure AD-account. In deze zelfstudie leert u hoe u uw omgeving kunt voorbereiden op het gebruik van Application Proxy. Vervolgens wordt de Azure-portal gebruikt om een on-premises toepassing toe te voegen aan uw Azure AD-tenant.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: barbkess
ms.reviewer: japere
ms.openlocfilehash: 1f6962f0313b6b0ca187512185c990606789da6e
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55661952"
---
# <a name="tutorial-add-an-on-premises-application-for-remote-access-through-application-proxy-in-azure-active-directory"></a>Zelfstudie: Een on-premises toepassing voor externe toegang toevoegen via Application Proxy in Azure Active Directory

Azure Active Directory (Azure AD) heeft een Application Proxy-service waarmee gebruikers toegang krijgen tot on-premises toepassingen door zich aan te melden met hun Azure AD-account. In deze zelfstudie wordt uw omgeving voorbereid voor gebruik van Application Proxy. Zodra uw omgeving gereed is, gebruikt u de Azure-portal om een on-premises toepassing toe te voegen aan uw Azure AD-tenant.

Deze zelfstudie:

> [!div class="checklist"]
> * Opent poorten voor uitgaand verkeer en verschaft toegang tot specifieke URL's
> * Installeert de connector op uw Windows-server en registreert deze bij Application Proxy
> * Controleert of de connector juist is geïnstalleerd en geregistreerd
> * Voegt een on-premises toepassing toe aan uw Azure AD-tenant
> * Controleert of een testgebruiker zich kan aanmelden bij de toepassing met behulp van een Azure AD-account

## <a name="before-you-begin"></a>Voordat u begint

Als u een toepassing wilt toevoegen aan uw tenant, hebt u het volgende nodig:

* Een [Microsoft Azure AD Basic- of premium-abonnement](https://azure.microsoft.com/pricing/details/active-directory). 
* Een Administrator-account voor de toepassing.

### <a name="windows-server"></a>Windows-server
Als u Application Proxy wilt gebruiken, hebt u een Windows-server nodig waarop Windows Server 2012 R2 of hoger wordt uitgevoerd. U installeert de Application Proxy-connector op de server. Deze connectorserver moet verbinding maken met de Application Proxy-service in Azure en met de on-premises toepassingen die u van plan bent te publiceren.

Om een hoge beschikbaarheid in uw productieomgeving te realiseren wordt aangeraden meer dan één Windows-server te gebruiken. Voor deze zelfstudie is één Windows-server toereikend.

**Aanbevelingen voor de connectorserver**

1. Plaats de connectorserver fysiek dicht bij de toepassingsservers om de prestaties tussen de connector en de toepassing te optimaliseren. Zie [Overwegingen bij de netwerktopologie](application-proxy-network-topology.md) voor meer informatie.

2. De connectorserver en de webtoepassingsservers moeten deel uitmaken van hetzelfde Active Directory-domein. De servers moeten zich in hetzelfde domein bevinden om gebruik te kunnen maken van eenmalige aanmelding (SSO) met Geïntegreerde Windows-verificatie en beperkte delegatie van Kerberos. Als de connectorserver en de webtoepassingsservers zich in verschillende Active Directory-domeinen bevinden, moet u delegatie op basis van resources gebruiken voor eenmalige aanmelding. Zie [KCD voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md) voor meer informatie.

**Softwarevereisten**

Voor de Windows-connectorserver moet TLS 1.2 zijn ingeschakeld voordat u de Application Proxy-connector installeert. Bestaande connectors met een versie lager dan 1.5.612.0 blijven tot nader order werken in eerdere versies van TLS. 

TLS 1.2 inschakelen:

1. Stel de volgende registersleutels in:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. Start de server opnieuw

  
## <a name="prepare-your-on-premises-environment"></a>Bereid uw on-premises omgeving voor
Als u uw omgeving wilt voorbereiden voor Application Proxy van Azure AD, moet u eerst communicatie met Azure-datacenters inschakelen. Als er op het pad een firewall is, zorgt u ervoor dat deze openstaat zodat de connector HTTPS-aanvragen (TCP) kan versturen naar de toepassingsproxy.

### <a name="open-ports"></a>Poorten openen

Open de volgende poorten voor **uitgaand** verkeer. 

   | Poortnummer | Hoe dat wordt gebruikt |
   | --- | --- |
   | 80 | Het downloaden van certificaatintrekkingslijsten (CRL's) tijdens het valideren van het SSL-certificaat |
   | 443 | Alle uitgaande communicatie met de Application Proxy-service |

Als met uw firewall verkeer wordt afgedwongen op basis van de herkomst van gebruikers, open dan ook poorten 80 en 443 voor verkeer dat afkomstig is van Windows-services die als netwerkservice worden uitgevoerd.

Als u Application Proxy al gebruikt, is er mogelijk een oudere versie van de connector geïnstalleerd.  Volg deze zelfstudie voor het installeren van de meest recente versie van de connector. Voor versies ouder dan 1.5.132.0 moeten ook de volgende poorten zijn geopend: 5671, 8080, 9090-9091, 9350, 9352, 10100–10120. 

### <a name="allow-access-to-urls"></a>Toegang tot URL's toestaan

Sta toegang tot de volgende URL's toe:

| URL | Hoe dat wordt gebruikt |
| --- | --- |
| \*.msappproxy.net<br>\*.servicebus.windows.net | Communicatie tussen de connector en de Application Proxy-cloudservice |
| mscrl.microsoft.com:80<br>crl.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Azure gebruikt deze URL's om certificaten te controleren |
| login.windows.net<br>login.microsoftonline.com | De connector gebruikt deze URL's tijdens het registratieproces. |

Als uw firewall of proxyserver opname in een DNS-whitelist toestaat, kunt u verbindingen met \*msappproxy.net en \* in de whitelist opnemen. Als dat niet het geval is, moet u toegang toestaan tot de [IP-bereiken van Azure DataCenter](https://www.microsoft.com/download/details.aspx?id=41653), die overigens elke week worden bijgewerkt.

## <a name="install-and-register-a-connector"></a>Een connector installeren en registreren
Voor het gebruik van Application Proxy moet u een connector installeren op elke Windows-server die u wilt gebruiken met de Application Proxy-service. De connector is een agent die de uitgaande verbinding vanaf de on-premises toepassingsservers naar Application Proxy in Azure AD beheert. U kunt een connector installeren op servers waarop ook andere verificatie-agents zijn geïnstalleerd, zoals Azure AD Connect.

De connector installeren:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als een toepassingsbeheerder van de map die gebruikmaakt van Application Proxy. Als het domein van de tenant bijvoorbeeld contoso.com is, moet de beheerder admin@contoso.com of een andere beheerdersalias in dat domein zijn.
2. De huidige map wordt weergegeven onder uw gebruikersnaam in de rechterbovenhoek. Controleer of u bent aangemeld in de map die gebruikmaakt van Application Proxy. Als u van map wilt veranderen, selecteert u dat pictogram.
3. Klik in de linkerblade op **Azure Active Directory** en vervolgens op **Application Proxy**.
4. Klik op **Service-connector downloaden**.
5. Lees de servicevoorwaarden.  Wanneer u klaar bent, klikt u op **Voorwaarden accepteren en downloaden**.
6. Onder in het venster ziet u een prompt voor het downloaden van **AADApplicationProxyConnectorInstaller.exe**. Klik op **Uitvoeren** om de connector te installeren. Er wordt een installatiewizard geopend. 
7. Volg de instructies in de wizard om de toepassing te installeren. Wanneer u wordt gevraagd de connector te registreren voor de Application Proxy voor uw Azure AD-tenant, geeft u uw gegevens als toepassingsbeheerder op.
    - Voor Internet Explorer (IE) geldt dat als **Verbeterde beveiliging van Internet Explorer** is ingesteld op **Aan**, het registratiescherm niet wordt weergegeven. Volg de instructies in het foutbericht om toegang te krijgen. Zorg ervoor dat Verbeterde beveiliging van Internet Explorer is ingesteld op **Uit**.

### <a name="general-remarks"></a>Algemene opmerkingen

Als u al een connector hebt geïnstalleerd, voert u een nieuwe installatie uit met de meest recente versie.

Als u meer dan één Windows-server voor uw on-premises toepassingen wilt, moet u de connector op elke server installeren en registreren. U kunt de connectors onderverdelen in connectorgroepen. Zie [Connectorgroepen](application-proxy-connector-groups.md) voor meer informatie. 

Als uw organisatie proxyservers gebruikt om verbinding met internet te maken, moet u ze configureren voor Application Proxy.  Zie [Werken met bestaande on-premises proxyservers](application-proxy-configure-connectors-with-proxy-servers.md) voor meer informatie. 

Zie [Informatie over Azure AD Application Proxy-connectors](application-proxy-connectors.md) voor meer informatie over connectors, capaciteitsplanning en hoe connectors up-to-date blijven. 

Als u de toepassing Qlik Sense gebruikt, moet u altijd de meest recente versie van de connector installeren. Qlik Sense maakt gebruik van WebSockets, dat alleen wordt ondersteund in connectorversies 1.5.612.0 of hoger.


## <a name="verify-the-connector-installed-and-registered-correctly"></a>Controleer of de connector juist is geïnstalleerd en geregistreerd

U kunt de Azure-portal of uw Windows-server gebruiken om te bevestigen dat er een nieuwe connector correct is geïnstalleerd.

### <a name="verify---azure-portal"></a>Controleren - Azure-portal
Controleren of de connector juist is geïnstalleerd en geregistreerd:

1. Meld u aan bij uw tenantmap in de [Azure-portal](https://portal.azure.com).
2. Klik op **Azure Active Directory** en vervolgens op **Application Proxy**. Al uw connectors en connectorgroepen worden op deze pagina weergegeven. 
3. Selecteer een connector om de details ervan te controleren. Een actief groen label geeft aan dat de connector verbinding kan maken met de service. Maar ook al is het label groen, toch kan een netwerkprobleem er nog steeds voor zorgen dat de connector geen berichten ontvangt. 

    ![Azure AD Application Proxy-connectors](./media/application-proxy-connectors/app-proxy-connectors.png)

Zie [Problemen bij het installeren van een Application Proxy-connector](application-proxy-connector-installation-problem.md) voor meer hulp bij het installeren van een connector.

### <a name="verify---windows-server"></a>Controleren - Windows Server
Controleren of de connector juist is geïnstalleerd en geregistreerd:

1. Open Windows Services Manager door te klikken op de **Windows**-toets en *services.msc* in te voeren.
2. Controleer of de status van de volgende twee services **Wordt uitgevoerd** is.
   - Met **Microsoft AAD Application Proxy Connector** wordt de connectiviteit mogelijk gemaakt
   - **Microsoft AAD Application Proxy Connector Updater** is een automatische updateservice. De updater controleert op nieuwe versies van de connector en werkt de connector bij als dat nodig is.

    ![Connectorservices voor toepassingsproxy - schermafbeelding](./media/application-proxy-enable/app_proxy_services.png)

3. Als de status van de services niet **Wordt uitgevoerd** is, klikt u met de rechtermuisknop op elke service en kiest u **Starten**. 

## <a name="add-an-on-premises-app-to-azure-ad"></a>Een on-premises app toevoegen aan Azure AD

Nu u uw omgeving hebt voorbereid en een connector hebt geïnstalleerd, kunt u on-premises toepassingen gaan toevoegen aan Azure AD.  

1. Meld u aan als beheerder in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **Azure Active Directory** > **Bedrijfstoepassingen** > **Nieuwe toepassing**.

    ![Een bedrijfstoepassing toevoegen](./media/application-proxy-publish-azure-portal/add-app.png)

3. Selecteer **Alles** en selecteer **On-premises toepassing**.  

    ![Uw eigen toepassing toevoegen](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Geef op de blade **Uw eigen on-premises toepassing toevoegen** de volgende gegevens op voor uw toepassing:

    ![Uw toepassing configureren](./media/application-proxy-publish-azure-portal/configure-app.png)

    | Veld | Beschrijving |
    | :---- | :---------- |
    | **Naam** | De naam van de toepassing die wordt weergegeven in het toegangsvenster en in de Azure-portal. |
    | **Interne URL** | Dit is de URL voor toegang tot de toepassing vanuit uw particuliere netwerk. U kunt voor het publiceren een specifiek pad opgeven op de back-endserver, terwijl de rest van de server ongepubliceerd blijft. Op deze manier kunt u verschillende sites op dezelfde server als verschillende apps publiceren en elk daarvan een eigen naam en toegangsregels geven.<br><br>Als u een pad publiceert, moet u ervoor zorgen dat dit alle benodigde installatiekopieën, scripts en opmaakmodellen voor uw toepassing bevat. Als uw app zich bijvoorbeeld bevindt op https://yourapp/app en gebruikmaakt van installatiekopieën op https://yourapp/media, moet u https://yourapp/ publiceren als het pad. Deze interne URL hoeft niet de bestemmingspagina te zijn die uw gebruikers te zien krijgen. Zie [Een aangepaste startpagina voor gepubliceerde apps instellen](application-proxy-configure-custom-home-page.md) voor meer informatie. |
    | **Externe URL** | Het adres voor gebruikers om toegang te krijgen tot de app van buiten uw netwerk. Als u het standaarddomein voor Application Proxy niet wilt gebruiken, lees dan de informatie over [aangepaste domeinen in Azure AD Application Proxy](application-proxy-configure-custom-domain.md).|
    | **Verificatie vooraf** | De manier waarop gebruikers door Application Proxy worden geverifieerd voordat ze toegang krijgen tot uw toepassing.<br><br>**Azure Active Directory**: de gebruikers worden omgeleid door Application Proxy zodat ze zich kunnen aanmelden met Azure AD. Hierbij worden hun machtigingen geverifieerd voor de map en de toepassing. Het is raadzaam deze optie standaard ingesteld te houden, zodat u gebruik kunt maken van Azure AD-beveiligingsfuncties zoals voorwaardelijke toegang en meervoudige verificatie. **Azure Active Directory** is vereist voor het bewaken van de toepassing met Microsoft Cloud Application Security.<br><br>**Passthrough**: gebruikers hoeven geen verificatie te doorlopen in Azure Active Directory om toegang te krijgen tot de toepassing. U kunt nog steeds verificatievereisten op de back-end instellen. |
    | **Connectorgroep** | Connectors verwerken de externe toegang tot uw toepassing en met connectorgroepen kunt u connectors en toepassingen indelen per regio, netwerk of doel. Als u nog geen connectorgroepen hebt gemaakt, wordt uw toepassing toegewezen als **Standaard**.<br><br>Als uw toepassing gebruikmaakt van WebSockets om verbinding te maken, moeten alle connectors in de groep versie 1.5.612.0 of hoger hebben.|

5. Configureer zo nodig**aanvullende instellingen**. Voor de meeste toepassingen moet u voor deze instellingen de standaardwaarden behouden. 

    | Veld | Beschrijving |
    | :---- | :---------- |
    | **Toepassingstime-out voor de back-end** | Stel deze waarde alleen in op **Lang** als uw toepassing traag is met verifiëren en verbinding maken. |
    | **Alleen-HTTP-cookies gebruiken** | Stel deze waarde in op **Ja** om ervoor te zorgen dat Application Proxy-cookies de HTTPOnly-vlag in de HTTP-antwoordheader bevatten. Als u Extern bureaublad-services gebruikt, stelt u deze waarde in op **Nee**.|
    | **Beveiligde cookies gebruiken**| Stel deze waarde in op **Ja** om cookies te verzenden via een beveiligd kanaal, zoals een versleutelde HTTPS-aanvraag.
    | **Permanente cookies gebruiken**| Houd deze waarde ingesteld op **Nee**. Deze instelling mag alleen worden gebruikt voor toepassingen die cookies niet kunnen delen tussen processen. Zie [Cookie-instellingen voor toegang tot on-premises toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings) voor meer informatie over cookie-instellingen
    | **URL's in headers vertalen** | Laat deze waarde op **Ja** staan, tenzij voor uw toepassing de oorspronkelijke host-header in de verificatieaanvraag moet zijn opgenomen. |
    | **URL's vertalen in de hoofdtekst van de toepassing** | Laat deze waarde op **Nee** staan, tenzij u hardcoded HTML-koppelingen naar andere on-premises toepassingen hebt en geen aangepaste domeinen gebruikt. Zie [Vertaling koppelen aan Application Proxy](application-proxy-configure-hard-coded-link-translation.md) voor meer informatie.<br><br>Stel deze waarde in op **Ja** als u van plan bent om deze toepassing te bewaken met Microsoft Cloud App Security (MCAS). Meer informatie vindt u in [Configuratie van realtime bewaking van toegang tot toepassingen met Microsoft Cloud App Security en Azure Active Directory](application-proxy-integrate-with-microsoft-cloud-application-security.md). |
   


6. Selecteer **Toevoegen**.

## <a name="test-the-application"></a>De toepassing testen

U kunt nu testen of de toepassing correct is toegevoegd. Hiervoor gaat u in de volgende stappen een gebruikersaccount toevoegen aan de toepassing en u vervolgens proberen aan te melden.

### <a name="add-a-user-for-testing"></a>Een gebruiker toevoegen voor het testen
Controleer voordat u een gebruiker aan de toepassing toevoegt of het gebruikersaccount al machtigingen heeft voor toegang tot de toepassing vanuit het bedrijfsnetwerk.

Een testgebruiker toevoegen:

1. Ga weer naar de blade **Quickstart** en selecteer **Een gebruiker toewijzen voor de test**.

    ![Een gebruiker toewijzen voor de test](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Selecteer op de blade **Gebruikers en groepen** de optie **Gebruiker toevoegen**.

    ![Een gebruiker of groep toevoegen](./media/application-proxy-publish-azure-portal/add-user.png)

3. Op de blade **Toewijzing toevoegen** selecteert u **Gebruikers en groepen** en kiest u het account dat u wilt toevoegen. 
4. Selecteer **Toewijzen**.

### <a name="test-the-sign-on"></a>De aanmelding testen

De aanmelding voor de toepassing testen:

1. Navigeer in uw browser naar de externe URL die u hebt geconfigureerd tijdens de stap voor het publiceren. 
2. Als het goed is, ziet u nu het startscherm.
3. Probeer u aan te melden als de gebruiker die u in het vorige gedeelte hebt gemaakt.

    ![Uw gepubliceerde toepassing testen](./media/application-proxy-publish-azure-portal/test-app.png)

Zie [Problemen en foutberichten met Application Proxy oplossen](application-proxy-troubleshoot.md) voor het oplossen van problemen.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u uw on-premises omgeving voorbereid om te werken met Application Proxy en hebt u vervolgens de Application Proxy-connector geïnstalleerd en geregistreerd. Vervolgens hebt u een toepassing toegevoegd aan uw Azure AD-tenant. U hebt ook gecontroleerd of een gebruiker zich kan aanmelden bij de toepassing met behulp van een Azure AD-account

U hebt het volgende gedaan:
> [!div class="checklist"]
> * Poorten geopend voor uitgaand verkeer en toegang verschaft tot specifieke URL 's
> * De connector geïnstalleerd op uw Windows-server en deze geregistreerd bij Application Proxy
> * Gecontroleerd of de connector juist is geïnstalleerd en geregistreerd
> * Een toepassing toegevoegd aan uw Azure AD-tenant
> * Gecontroleerd of een testgebruiker zich kan aanmelden bij de toepassing met behulp van een Azure AD-account

U kunt de toepassing nu gaan configureren voor eenmalige aanmelding. Gebruik de volgende koppeling om een methode voor eenmalige aanmelding te kiezen en om de zelfstudies voor eenmalige aanmelding te vinden. 

> [!div class="nextstepaction"]
>[Eenmalige aanmelding configureren](what-is-single-sign-on.md#choosing-a-single-sign-on-method)





