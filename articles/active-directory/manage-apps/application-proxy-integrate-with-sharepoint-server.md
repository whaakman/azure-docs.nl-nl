---
title: Externe toegang tot SharePoint met Azure AD-toepassingsproxy inschakelen | Microsoft Docs
description: Bevat de basisinformatie over het integreren van een on-premises SharePoint-server met Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 9b8ae85d1a5410677dd9299ebb947c2189a6b663
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166180"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Externe toegang tot SharePoint met Azure AD-toepassingsproxy inschakelen

In dit artikel wordt beschreven hoe u een on-premises SharePoint-server integreren met Azure Active Directory (Azure AD) Application Proxy.

Voor externe toegang tot SharePoint met Azure AD-toepassingsproxy, volgt u de secties in dit artikel stap voor stap.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u SharePoint 2013 of hoger al in uw omgeving hebt. Bovendien kunt u overwegen de volgende vereisten:

* SharePoint bevat systeemeigen ondersteuning van Kerberos. Daarom gebruikers die toegang hebben tot interne websites op afstand via Azure AD Application Proxy kunnen wordt ervan uitgegaan dat een ervaring voor eenmalige aanmelding (SSO).

* Dit scenario omvat configuratiewijzigingen naar uw SharePoint-server. We raden u aan met behulp van een faseringsomgeving. Op deze manier u kunt updates aanbrengen in uw testserver eerst, en vervolgens een testcyclus voordat u doorgaat naar de productie te vergemakkelijken.

* SSL op de gepubliceerde URL is vereist. SSL is ook vereist voor de interne URL om ervoor te zorgen dat koppelingen verzonden/toegewezen juist zijn.

## <a name="step-1-configure-kerberos-constrained-delegation-kcd"></a>Stap 1: Configureer Kerberos-beperkte overdracht (KCD)

Voor on-premises toepassingen die gebruikmaken van Windows-verificatie, kunt u eenmalige aanmelding (SSO) met het Kerberos-verificatieprotocol en een functie met de naam van Kerberos-beperkte delegatie (KCD) bereiken. KCD, wanneer geconfigureerd, kunt de Application Proxy-connector op een Windows-token voor een gebruiker, zelfs als de gebruiker nog niet is aangemeld bij Windows rechtstreeks. Zie voor meer informatie over KCD [Kerberos-beperkte overdracht overzicht](https://technet.microsoft.com/library/jj553400.aspx).

Als u KCD instelt voor een SharePoint-server, gebruik de procedures in de volgende sequentiële secties:

### <a name="ensure-that-sharepoint-web-application-is-running-under-a-domain-account"></a>Zorg ervoor dat SharePoint-webtoepassing wordt uitgevoerd onder een domeinaccount

Controleer eerst of de SharePoint-webtoepassing wordt uitgevoerd onder een domeinaccount--niet lokaal systeem, lokale service of NetworkService. Dit doen zodat u de service principal names (SPN's) aan dit account koppelen kunt. SPN's zijn hoe het Kerberos-protocol wordt aangegeven met verschillende services. En u moet het account later naar de KCD configureren.

> [!NOTE]
U moet beschikken over een eerder gemaakte Azure AD-account voor de service. Het is raadzaam dat u voor een automatische wachtwoordwijziging toestaan. Zie voor meer informatie over de volledige set van stappen en het oplossen van problemen, [automatisch veranderen van wachtwoorden in SharePoint configureren](https://technet.microsoft.com/library/ff724280.aspx).

Om ervoor te zorgen dat uw sites worden uitgevoerd onder een opgegeven service-account, moet u de volgende stappen uitvoeren:

1. Open de **Centraal beheer van SharePoint** site.
2. Ga naar **Security** en selecteer **service-accounts configureren**.
3. Selecteer **Web-groep van toepassingen - SharePoint - 80**. De opties mogelijk enigszins verschillen op basis van de naam van uw web-groep, of als de webpool voor SSL wordt standaard gebruikt.

  ![Opties voor het configureren van een service-account](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

4. Als **selecteert u een account voor dit onderdeel** veld is ingesteld op **lokale Service** of **netwerkservice**, moet u een account maken. Zo niet, u klaar bent en u kunt verplaatsen naar de volgende sectie.
5. Selecteer **nieuw beheerd account registreren**. Nadat uw account is gemaakt, moet u instellen **toepassingen** voordat u het account kunt gebruiken.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>Een service-principal-naam voor de SharePoint-serviceaccount instellen

Voordat u KCD configureert, moet u naar:

* Identificeer de domeinaccount die met de SharePoint-webtoepassing die Azure AD-Proxy wordt weergegeven.
* Kies een interne URL die wordt geconfigureerd in zowel Azure AD-toepassingsproxy en SharePoint. Deze interne URL mag niet worden gebruikt in de web-App en worden niet weergegeven in de webbrowser.

Ervan uitgaande dat de interne URL gekozen is <https://sharepoint>, dan is de SPN-naam:

```
HTTP/SharePoint
```

> [!NOTE]
> Respecteer de volgende aanbevelingen voor de interne URL:
> * Gebruik van HTTPS
> * Gebruik geen aangepaste poorten
> * In de DNS-server, maakt u een Host (A) aan, wijs SharePoint WFE (of een load balancer) en niet een Alias (CName)

Voer de volgende opdracht uit vanaf de opdrachtprompt als beheerder van het domein voor het registreren van deze SPN-naam:

```
setspn -S HTTP/SharePoint demo\spAppPoolAccount
```

Deze opdracht stelt u de SPN _HTTP/SharePoint_ voor het account voor toepassingsgroep SharePoint _demo\spAppPoolAccount_.

Vervang _HTTP/SharePoint_ met de SPN voor de interne URL en _demo\spAppPoolAccount_ met de account van de groep van toepassingen in uw omgeving. Het Setspn-opdracht wordt gezocht naar de SPN-naam voordat deze wordt toegevoegd. In deze al bestaat, ziet u een **dubbele SPN waarde** fout. In dit geval kunt u de bestaande SPN verwijderen als deze niet ingesteld onder de juiste account voor toepassingsgroep.

U kunt controleren of de SPN is toegevoegd door te voeren van de Setspn-opdracht met de optie -L. Zie voor meer informatie over deze opdracht, [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-trusted-for-delegation-to-the-spn-added-to-the-sharepoint-application-pool-account"></a>Zorg ervoor dat de connector vertrouwd voor overdracht naar de SPN toegevoegd aan de SharePoint-account voor de groep van toepassingen wordt

KCD zodanig configureren dat de service Azure AD-toepassingsproxy gebruikers-id's aan de SharePoint-account voor de groep van toepassing kan delegeren. KCD configureren door de Application Proxy-connector om op te halen van Kerberos-tickets voor uw gebruikers die zijn geverifieerd in Azure AD in te schakelen. En vervolgens die server wordt de context in dit geval aan de doeltoepassing of SharePoint doorgegeven.

Voor het configureren van de KCD, herhaalt u de volgende stappen uit voor elke connector-machine:

1. Meld u aan als domeinadministrator op een domeincontroller en open vervolgens **Active Directory: gebruikers en Computers**.
2. Zoek de computer waarop de connector wordt uitgevoerd. In dit voorbeeld is de dezelfde SharePoint-server.
3. Dubbelklik op de computer en klik vervolgens op de **delegering** tabblad.
4. Zorg ervoor dat de delegatie-instellingen zijn ingesteld op **deze computer mag delegeren aan de opgegeven services alleen**. Selecteer **elk verificatieprotocol gebruiken**.
5. Klik op de **toevoegen** klikken, klik op **gebruikers of Computers**, en Ga naar het SharePoint application pool-account, bijvoorbeeld _demo\spAppPoolAccount_.
6. Selecteer in de lijst van de SPN's die u eerder hebt gemaakt voor het serviceaccount.
7. Klik op **OK**. Klik op **OK** opnieuw de wijzigingen wilt opslaan.
  
  ![De delegatie-instellingen](./media/application-proxy-integrate-with-sharepoint-server/delegation-box2.png)

## <a name="step-2-configure-azure-ad-proxy"></a>Stap 2: Azure AD-toepassingsproxy configureren

Nu dat u KCD hebt geconfigureerd, bent u klaar om te configureren van Azure AD-toepassingsproxy.

1. Publiceer uw SharePoint-site met de volgende instellingen. Zie voor stapsgewijze instructies [toepassingen publiceren met behulp van Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md).
   * **Interne URL**: Interne URL van SharePoint die eerder is gekozen, zoals **<https://SharePoint/>**.
   * **Methode voor verificatie vooraf**: Azure Active Directory
   * **Vertalen van URL in de Headers**: NO

   >[!TIP]
   >SharePoint maakt gebruik van de _Host-Header_ waarde om te controleren of de site. Deze koppelingen op basis van deze waarde wordt ook gegenereerd. Het uiteindelijke resultaat is dat een koppeling die SharePoint genereert een gepubliceerde URL correct is ingesteld voor gebruik van de externe URL. Als de waarde instelt op **Ja** kunt u ook de connector voor het doorsturen van de aanvraag voor de back-endtoepassing. Als u echter de waarde instelt op **Nee** betekent dit dat de connector niet de naam van de interne wordt verzenden. De connector verzendt in plaats daarvan de host-header als de gepubliceerde URL naar de back-endtoepassing.

   ![SharePoint als toepassing publiceren](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

2. Zodra uw app is gepubliceerd, moet u de instellingen voor eenmalige aanmelding configureren met de volgende stappen uit:

   1. Selecteer op de toepassingspagina in de portal, **eenmalige aanmelding**.
   2. Selecteer voor Single Sign-on modus **geïntegreerde Windows-verificatie**.
   3. SPN voor interne toepassing ingesteld op de waarde die u eerder hebt ingesteld. In dit voorbeeld dat is **HTTP/SharePoint**.
   4. Selecteer in de "Gedelegeerde Aanmeldingsidentiteit", **On-premises SAM-accountnaam**.

   ![Geïntegreerde Windows-verificatie voor eenmalige aanmelding configureren](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

3. Voor het voltooien van het instellen van uw toepassing, gaat u naar de **gebruikers en groepen** sectie en gebruikers toegang krijgen tot deze toepassing toe te wijzen. 

## <a name="step-3-configure-sharepoint-to-use-kerberos-and-azure-ad-proxy-urls"></a>Stap 3: SharePoint voor het gebruik van Kerberos- en Azure AD-Proxy-URL's configureren

Volgende stap is het uitbreiden van de SharePoint-webtoepassing aan een nieuwe zone, geconfigureerd met Kerberos en de juiste alternatieve toegangstoewijzingen om toe te staan van SharePoint om binnenkomende aanvragen verzonden naar de interne URL te verwerken en direct reageren met de koppelingen die is gebouwd voor de externe URL.

1. Start de **SharePoint-beheershell**.
2. Voer het volgende script uit te breiden de web-App aan de zone met Extranet en Kerberos-verificatie inschakelen:

  ```powershell
  # Replace "http://spsites/" with the URL of your web application
  # Replace "https://sharepoint-f128.msappproxy.net/" with the External URL in your Azure AD proxy application
  $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
  Get-SPWebApplication "http://spsites/" | New-SPWebApplicationExtension -Name "SharePoint - AAD Proxy" -SecureSocketsLayer -Zone "Extranet" -Url "https://sharepoint-f128.msappproxy.net/" -AuthenticationProvider $winAp
  ```

3. Open de **Centraal beheer van SharePoint** site.
4. Onder **systeeminstellingen**, selecteer **alternatieve toegangstoewijzingen configureren**. Hiermee opent u het vak Alternatieve toewijzingen voor toegang.
5. Selecteer uw site, bijvoorbeeld **SharePoint - 80**. Voor dit moment geen Extranet zone de interne URL nog correct is ingesteld:

  ![Alternatieve toegangstoewijzingen vak](./media/application-proxy-integrate-with-sharepoint-server/alternate-access1.png)

6. Klik op **interne URL's toevoegen**.
7. In **URL-protocol, host en poort** tekstvak, type de **interne URL** geconfigureerd in Azure AD-proxy, bijvoorbeeld <https://SharePoint/>.
8. Selecteer Zone **Extranet** in de vervolgkeuzelijst.
9. Klik op **Opslaan**.
10. De alternatieve toegangstoewijzingen moet er nu als volgt uitzien:

  ![Alternatieve toegangstoewijzingen corrigeren](./media/application-proxy-integrate-with-sharepoint-server/alternate-access3.png)

## <a name="step-4-ensure-that-an-https-certificate-is-configured-for-the-iis-site-of-the-extranet-zone"></a>Stap 4: Zorg ervoor dat er een HTTPS-certificaat is geconfigureerd voor de IIS-site van het Extranet zone

SharePoint-configuratie is nu voltooid, maar omdat de interne URL van het Extranet zone <https://SharePoint/>, een certificaat moet worden ingesteld voor deze site.

1. Open Windows PowerShell-console.
2. Voer het volgende script om een zelfondertekend certificaat genereren en MY store toevoegen aan de computer:

  ```powershell
  # Replace "SharePoint" with the actual hostname of the Internal URL of your Azure AD proxy application
  New-SelfSignedCertificate -DnsName "SharePoint" -CertStoreLocation "cert:\LocalMachine\My"
  ```

  > [!NOTE]
  Zelfondertekende certificaten zijn alleen geschikt voor test-doeleinden. In een productieomgeving, is het raadzaam het gebruik van certificaten uitgegeven door een certificeringsinstantie in plaats daarvan.

3. Open de console van de 'Internet Information Services Manager'.
4. Vouw de server in de structuurweergave uit, vouw "Sites", selecteer de site 'SharePoint – AAD Proxy' en klik op **bindingen**.
5. Selecteer https-binding en klikt u op **bewerken...** .
6. Kies in het veld voor SSL-certificaat, **SharePoint** van het certificaat en klik op OK.

U kunt nu toegang tot de SharePoint-site extern via Azure AD-toepassingsproxy.

## <a name="next-steps"></a>Volgende stappen

* [Werken met aangepaste domeinen in Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md)
* [Meer informatie over Azure AD Application Proxy connectors](application-proxy-connectors.md)
