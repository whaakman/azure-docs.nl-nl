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
ms.date: 10/19/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 0f1b46176ba440a11d1584846019859c63d2f263
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135777"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Externe toegang tot SharePoint met Azure AD-toepassingsproxy inschakelen

In dit artikel wordt beschreven hoe u een on-premises SharePoint-server integreren met Azure Active Directory (Azure AD) Application Proxy.

Voor externe toegang tot SharePoint met Azure AD-toepassingsproxy, volgt u de secties in dit artikel stap voor stap.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u SharePoint 2013 of hoger al in uw omgeving hebt. Bovendien kunt u overwegen de volgende vereisten:

* SharePoint bevat systeemeigen ondersteuning van Kerberos. Daarom gebruikers die toegang hebben tot interne websites op afstand via Azure AD Application Proxy kunnen wordt ervan uitgegaan dat een ervaring voor eenmalige aanmelding (SSO).

* Dit scenario omvat configuratiewijzigingen naar uw SharePoint-server. We raden u aan met behulp van een faseringsomgeving. Op deze manier u kunt updates aanbrengen in uw testserver eerst, en vervolgens een testcyclus voordat u doorgaat naar de productie te vergemakkelijken.

* SSL op de gepubliceerde URL is vereist. U moet SSL is ingeschakeld op uw interne site om ervoor te zorgen dat koppelingen verzonden/toegewezen juist zijn. Als u dit nog niet hebt geconfigureerd voor SSL, Zie [SSL configureren voor SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) voor instructies. Zorg er ook voor dat het certificaat dat u de opdracht wordt vertrouwd door de connector-machine. (Het certificaat hoeft niet te openbaar worden uitgegeven.)

## <a name="step-1-set-up-single-sign-on-to-sharepoint"></a>Stap 1: Van single sign-on bij SharePoint instellen

Voor on-premises toepassingen die gebruikmaken van Windows-verificatie, kunt u eenmalige aanmelding (SSO) met het Kerberos-verificatieprotocol en een functie met de naam van Kerberos-beperkte delegatie (KCD) bereiken. KCD, wanneer geconfigureerd, kunt de Application Proxy-connector op een Windows-token voor een gebruiker, zelfs als de gebruiker nog niet is aangemeld bij Windows rechtstreeks. Zie voor meer informatie over KCD [Kerberos-beperkte overdracht overzicht](https://technet.microsoft.com/library/jj553400.aspx).

Als u KCD instelt voor een SharePoint-server, gebruik de procedures in de volgende sequentiële secties:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>Zorg ervoor dat SharePoint wordt uitgevoerd onder een serviceaccount

Controleer eerst of SharePoint wordt uitgevoerd onder een gedefinieerde serviceaccount--niet lokaal systeem, lokale service of NetworkService. Dit doen zodat u de service principal names (SPN's) aan een geldig account toevoegen kunt. SPN's zijn hoe het Kerberos-protocol wordt aangegeven met verschillende services. En u moet het account later naar de KCD configureren.

> [!NOTE]
U moet beschikken over een eerder gemaakte Azure AD-account voor de service. Het is raadzaam dat u voor een automatische wachtwoordwijziging toestaan. Zie voor meer informatie over de volledige set van stappen en het oplossen van problemen, [configureert automatisch veranderen van wachtwoorden in SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).

Om ervoor te zorgen dat uw sites worden uitgevoerd onder een opgegeven service-account, moet u de volgende stappen uitvoeren:

1. Open de **Centraal beheer van SharePoint 2013** site.
2. Ga naar **Security** en selecteer **service-accounts configureren**.
3. Selecteer **Web-groep van toepassingen - SharePoint - 80**. De opties mogelijk enigszins verschillen op basis van de naam van uw web-groep, of als de webpool voor SSL wordt standaard gebruikt.

  ![Opties voor het configureren van een service-account](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

4. Als **selecteert u een account voor dit onderdeel** veld is ingesteld op **lokale Service** of **netwerkservice**, moet u een account maken. Zo niet, u klaar bent en u kunt verplaatsen naar de volgende sectie.
5. Selecteer **nieuw beheerd account registreren**. Nadat uw account is gemaakt, moet u instellen **toepassingen** voordat u het account kunt gebruiken.

### <a name="configure-sharepoint-for-kerberos"></a>SharePoint configureren voor Kerberos

U KCD gebruiken om uit te voeren single sign-on bij de SharePoint-server.

Het configureren van uw SharePoint-site voor Kerberos-verificatie:

1. Open de **Centraal beheer van SharePoint 2013** site.
2. Ga naar **Toepassingsbeheer**, selecteer **beheren van webtoepassingen**, en selecteer uw SharePoint-site. In dit voorbeeld is het **SharePoint - 80**.

  ![De SharePoint-site selecteren](./media/application-proxy-integrate-with-sharepoint-server/manage-web-applications.png)

3. Klik op **verificatieproviders** op de werkbalk.
4. In de **verificatieproviders** Klik **standaard Zone** om de instellingen weer te geven.
5. In de **verificatie bewerken** dialoogvenster vak, schuif omlaag totdat u ziet **Claims verificatietypen**. Zorg ervoor dat beide **Windows-verificatie inschakelen** en **geïntegreerde Windows-verificatie** zijn geselecteerd.
6. Controleer of in de vervolgkeuzelijst voor het veld geïntegreerde Windows-verificatie, **onderhandelen (Kerberos)** is geselecteerd.

  ![In het dialoogvenster voor verificatie bewerken](./media/application-proxy-integrate-with-sharepoint-server/service-edit-authentication.png)

7. Aan de onderkant van de **verificatie bewerken** in het dialoogvenster, klikt u op **opslaan**.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>Een service-principal-naam voor de SharePoint-serviceaccount instellen

Voordat u KCD configureert, moet u voor het identificeren van de SharePoint-service die wordt uitgevoerd als het serviceaccount dat u hebt geconfigureerd. Identificeren van de service door in te stellen van een SPN-naam. Zie voor meer informatie, [Service Principal Names](https://technet.microsoft.com/library/cc961723.aspx).

De SPN-indeling is:

```
<service class>/<host>:<port>
```

In de indeling van de SPN-naam:

* _Service-klasse_ is een unieke naam voor de service. Voor SharePoint, gebruikt u **HTTP**.

* _host_ is de FQDN of NetBIOS-naam van de host waarop de service wordt uitgevoerd op. Voor een SharePoint-site moet deze tekst mogelijk worden de URL van de site, afhankelijk van de versie van IIS die u gebruikt.

* _poort_ is optioneel.

Als de FQDN-naam van de SharePoint-server:

```
sharepoint.demo.o365identity.us
```

Dan is de SPN-naam:

```
HTTP/sharepoint.demo.o365identity.us demo
```

Mogelijk moet u ook de SPN's voor specifieke sites ingesteld op uw server. Zie voor meer informatie, [Configureer Kerberos-verificatie](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Aandacht besteedt aan het gedeelte 'Service-Principal-naam voor uw webtoepassingen met behulp van Kerberos-verificatie maken'.

De eenvoudigste manier voor het instellen van de SPN's is de SPN-indelingen die mogelijk al aanwezig zijn voor uw sites volgen. Kopieer deze SPN's om u te registreren op basis van het serviceaccount. Om dit te doen:

1. Blader naar de site met de SPN-naam van een andere computer.
 Wanneer u dit doet, wordt de betreffende set Kerberos-tickets in cache op de machine. Deze tickets bevatten de SPN-naam van de doelsite die u hebt gebladerd.

2. U kunt de SPN voor die site ophalen met behulp van een hulpprogramma met de naam [Klist uit](https://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html). In een opdrachtvenster die in dezelfde context als de gebruiker die toegang krijgen de site in de browser tot wordt uitgevoerd, moet u de volgende opdracht uitvoeren:
```
Klist
```
Klist uit retourneert vervolgens de set van de doel-SPN's. In dit voorbeeld is de gemarkeerde waarde de SPN die u nodig hebt:

  ![Voorbeeld Klist uit resultaten](./media/application-proxy-integrate-with-sharepoint-server/remote-sharepoint-target-service.png)

4. Nu dat u de SPN-naam hebt, zorg ervoor dat deze juist geconfigureerd op het serviceaccount dat u voor de webtoepassing eerder hebt ingesteld. Voer de volgende opdracht uit vanaf de opdrachtprompt als beheerder van het domein:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Deze opdracht stelt u de SPN voor de SharePoint-serviceaccount die wordt uitgevoerd als _demo\sp_svc_.

 Vervang _http/sharepoint.demo.o365identity.us_ met de SPN-naam voor uw server en _demo\sp_svc_ met de serviceaccount in uw omgeving. Het Setspn-opdracht wordt gezocht naar de SPN-naam voordat deze wordt toegevoegd. In dit geval ziet u mogelijk een **dubbele SPN waarde** fout. Als u deze fout ziet, zorg ervoor dat de waarde gekoppeld aan het serviceaccount is.

U kunt controleren of de SPN is toegevoegd door te voeren van de Setspn-opdracht met de optie -l. Zie voor meer informatie over deze opdracht, [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-set-as-a-trusted-delegate-to-sharepoint"></a>Zorg ervoor dat de connector is ingesteld als een vertrouwde gemachtigde naar SharePoint

KCD zodanig configureren dat de service Azure AD Application Proxy gebruikers-id's aan de SharePoint-service delegeren kan. KCD configureren door de Application Proxy-connector om op te halen van Kerberos-tickets voor uw gebruikers die zijn geverifieerd in Azure AD in te schakelen. En vervolgens die server wordt de context in dit geval aan de doeltoepassing of SharePoint doorgegeven.

Voor het configureren van de KCD, herhaalt u de volgende stappen uit voor elke connector-machine:

1. Meld u aan als domeinadministrator op een domeincontroller en open vervolgens **Active Directory: gebruikers en Computers**.
2. Zoek de computer waarop de connector wordt uitgevoerd. In dit voorbeeld is de dezelfde SharePoint-server.
3. Dubbelklik op de computer en klik vervolgens op de **delegering** tabblad.
4. Zorg ervoor dat de delegatie-instellingen zijn ingesteld op **deze computer mag delegeren aan de opgegeven services alleen**. Selecteer **elk verificatieprotocol gebruiken**.

  ![De delegatie-instellingen](./media/application-proxy-integrate-with-sharepoint-server/delegation-box.png)

5. Klik op de **toevoegen** klikken, klik op **gebruikers of Computers**, en zoek de serviceaccount.

  ![De SPN voor de serviceaccount toevoegen](./media/application-proxy-integrate-with-sharepoint-server/users-computers.png)

6. Selecteer in de lijst van de SPN's die u eerder hebt gemaakt voor het serviceaccount.
7. Klik op **OK**. Klik op **OK** opnieuw de wijzigingen wilt opslaan.

## <a name="step-2-enable-remote-access-to-sharepoint"></a>Stap 2: Extern toegang tot SharePoint inschakelen

Nu dat u SharePoint hebt ingeschakeld voor Kerberos- en geconfigureerde KCD, u kunt voor het publiceren van de SharePoint-farm voor externe toegang via Azure AD-toepassingsproxy.

1. Publiceer uw SharePoint-site met de volgende instellingen. Zie voor stapsgewijze instructies [toepassingen publiceren met behulp van Azure AD-toepassingsproxy](application-proxy-add-on-premises-application.md). 
   - **Interne URL**: de URL van de SharePoint-site intern, zoals **https://SharePoint/**. Zorg ervoor dat u in dit voorbeeld **https**
   - **Methode voor verificatie vooraf**: Azure Active Directory
   - **Vertalen van URL in de Headers**: Nee

   >[!TIP]
   >SharePoint maakt gebruik van de _Host-Header_ waarde om te controleren of de site. Deze koppelingen op basis van deze waarde wordt ook gegenereerd. Het uiteindelijke resultaat is dat een koppeling die SharePoint genereert een gepubliceerde URL correct is ingesteld voor gebruik van de externe URL. Als de waarde instelt op **Ja** kunt u ook de connector voor het doorsturen van de aanvraag voor de back-endtoepassing. Als u echter de waarde instelt op **Nee** betekent dit dat de connector niet de naam van de interne wordt verzenden. De connector verzendt in plaats daarvan de host-header als de gepubliceerde URL naar de back-endtoepassing.

   ![SharePoint als toepassing publiceren](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

2. Zodra uw app is gepubliceerd, moet u de instellingen voor eenmalige aanmelding configureren met de volgende stappen uit:

   1. Selecteer op de toepassingspagina in de portal, **eenmalige aanmelding**.
   2. Selecteer voor Single Sign-on modus **geïntegreerde Windows-verificatie**.
   3. SPN voor interne toepassing ingesteld op de waarde die u eerder hebt ingesteld. In dit voorbeeld dat is **http/sharepoint.demo.o365identity.us**.

   ![Geïntegreerde Windows-verificatie voor eenmalige aanmelding configureren](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

3. Voor het voltooien van het instellen van uw toepassing, gaat u naar de **gebruikers en groepen** sectie en gebruikers toegang krijgen tot deze toepassing toe te wijzen. 

## <a name="step-3-ensure-that-sharepoint-knows-about-the-external-url"></a>Stap 3: Zorg ervoor dat SharePoint op de hoogte van de externe URL

De laatste stap is om ervoor te zorgen dat SharePoint met de site op basis van de externe URL, vinden kunt zodat deze koppelingen op basis van deze externe URL wordt weergegeven. U doen dit door het configureren van alternatieve toegangstoewijzingen voor de SharePoint-site.

1. Open de **Centraal beheer van SharePoint 2013** site.
2. Onder **systeeminstellingen**, selecteer **alternatieve toegangstoewijzingen configureren**. Hiermee opent u het vak Alternatieve toewijzingen voor toegang.

  ![Alternatieve toegangstoewijzingen vak](./media/application-proxy-integrate-with-sharepoint-server/alternate-access1.png)

3. In de vervolgkeuzelijst naast **alternatieve toegang toewijzen verzameling**, selecteer **wijziging alternatieve toegang toewijzen verzameling**.
4. Selecteer uw site--bijvoorbeeld **SharePoint - 80**.
5. U kunt de gepubliceerde URL toevoegen als een interne URL of een openbare URL. Dit voorbeeld wordt een openbare URL als het extranet. Als u een aangepaste poort-Zorg ervoor dat u uw aangepaste poort in de URL bevatten.
6. Klik op **openbare URL's bewerken** in de **Extranet** pad, en voer vervolgens de externe URL die is gemaakt toen u de toepassing hebt gepubliceerd. Voer bijvoorbeeld **https://sharepoint-iddemo.msappproxy.net**.

  ![Het pad invoeren](./media/application-proxy-integrate-with-sharepoint-server/alternate-access3.png)

7. Klik op **Opslaan**.

U kunt nu toegang tot de SharePoint-site extern via Azure AD-toepassingsproxy.

## <a name="next-steps"></a>Volgende stappen

- [Werken met aangepaste domeinen in Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md)
- [Meer informatie over Azure AD Application Proxy connectors](application-proxy-connectors.md)

