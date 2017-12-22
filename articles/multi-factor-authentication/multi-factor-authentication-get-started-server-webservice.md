---
title: Webservice voor mobiele apps voor de Azure MFA-server | Microsoft Docs
description: De Microsoft verificator-app biedt een extra optie voor verificatie buiten de band.  Hierdoor kan de MFA-server pushmeldingen naar gebruikers sturen.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 83b04e48dd528881097bcf16bc03e1a18ea20c43
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Authenticatie met de mobiele app integreren met Azure Multi-Factor Authentication-server

De Microsoft verificator-app biedt een extra optie voor verificatie buiten de band. In plaats van tijdens het aanmelden een automatisch telefoongesprek met de gebruiker tot stand te brengen of hem een sms-bericht te sturen, verzendt Azure Multi-Factor Authentication een melding naar de Microsoft verificator-app op de smartphone of tablet van de gebruiker. De gebruiker hoeft in de app enkel op **Verifiëren** te tikken (of een pincode in te voeren en op Verifiëren te tikken) om de aanmelding te voltooien.

Het gebruik van een mobiele app voor verificatie in twee stappen wordt aanbevolen wanneer het telefonische bereik onbetrouwbaar is. Als u de app als een OATH-tokengenerator gebruikt, hebt u geen netwerk- of internetverbinding nodig.

Afhankelijk van uw omgeving kan het raadzaam zijn om de webservice voor mobiele apps te implementeren op dezelfde server als de Azure Multi-Factor Authentication-server of op een andere internetgerichte server.

## <a name="requirements"></a>Vereisten

Als u de Microsoft verificator-app wilt gebruiken, is het volgende vereist om de app correct te laten communiceren met de webservice voor mobiele apps:

* Azure Multi-Factor Authentication-server versie 6.0 of hoger.
* De webservice voor mobiele apps moet zijn geïnstalleerd op een internetgerichte webserver waarop Microsoft® [Internet Information Services (IIS) 7.x of hoger](http://www.iis.net/) wordt uitgevoerd.
* ASP.NET versie 4.0.30319 moet zijn geïnstalleerd en geregistreerd en zijn ingesteld op Toegestaan.
* Vereiste functieservices omvatten ASP.NET en compatibiliteit met IIS 6-metabase.
* Webservice voor mobiele apps moet toegankelijk zijn via een openbare URL.
* Webservice voor mobiele apps moet met een SSL-certificaat zijn beveiligd.
* De webservice-SDK van Azure Multi-Factor Authentication moet zijn geïnstalleerd in IIS 7.x of hoger op **dezelfde server als de Azure Multi-Factor Authentication-server**.
* De webservice-SDK van Azure Multi-Factor Authentication moet zijn beveiligd met een SSL-certificaat.
* De webservice voor mobiele apps moet via SSL verbinding kunnen maken met de webservice-SDK van Azure Multi-Factor Authentication.
* De webservice voor mobiele apps moet zich bij de webservice-SDK van Azure MFA kunnen verifiëren met behulp van de referenties van een serviceaccount dat lid is van de beveiligingsgroep PhoneFactor Admins. Dit serviceaccount en deze groep bevinden zich in Active Directory als de Azure Multi-Factor Authentication-server lid is van een server die aan een domein is gekoppeld. Dit serviceaccount en deze groep bevinden zich lokaal op de Azure Multi-Factor Authentication-server als deze niet aan een domein is gekoppeld.

## <a name="install-the-mobile-app-web-service"></a>De webservice voor mobiele apps installeren

Voordat u de webservice voor mobiele apps installeert, moet u op de volgende details letten:

* U moet een serviceaccount hebben dat deel uitmaakt van de groep 'PhoneFactor Admins'. Dit account mag hetzelfde zijn als het account dat is gebruikt voor de installatie van de gebruikersportal.
* Het is handig om een webbrowser te openen op de internetgerichte webserver en te navigeren naar de URL van de webservice-SDK die in het bestand web.config is ingevoerd. Als de browser de webservice kan bereiken, wordt u normaal om referenties gevraagd. Geef de gebruikersnaam en het wachtwoord op precies zoals deze in het bestand web.config zijn ingevoerd. Controleer of er geen certificaatwaarschuwingen of -fouten worden weergegeven.
* Als zich vóór de webserver van de webservice voor mobiele apps een omgekeerde proxy of firewall bevindt en SSL-offloading wordt uitvoert, kunt u het bestand web.config van de webservice voor mobiele apps bewerken, zodat de webservice voor mobiele apps http in plaats van https kan gebruiken. SSL is nog steeds vereist vanuit de mobiele app naar de firewall/omgekeerde proxy. Voeg de volgende sleutel toe aan de sectie \<appSettings\>:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-web-service-sdk"></a>De webservice-SDK installeren

Als de SDK van de webservice Azure Multi-Factor Authentication nog **niet** is geïnstalleerd op de Azure MFA-server (Multi-Factor Authentication), geldt voor beide scenario's dat u de volgende stappen moet uitvoeren.

1. Open de console van de Multi-Factor Authentication-server.
2. Ga naar **Webservice-SDK** en selecteer **Webservice-SDK installeren**.
3. Voltooi de installatie met de standaardinstellingen, tenzij u een speciale reden hebt om een andere instelling te kiezen.
4. Bind een SSL-certificaat aan de site in IIS.

Als u vragen hebt over het configureren van een SSL-certificaat op een IIS-server, raadpleegt u het artikel [How to Set Up SSL on IIS 7](https://docs.microsoft.com/iis/manage/configuring-security/how-to-set-up-ssl-on-iis) (SSL configureren in IIS 7).

De webservice-SDK moet met een SSL-certificaat worden beveiligd. Voor dit doel kan een zelfondertekend certificaat worden gebruikt. Importeer het certificaat in het certificaatarchief Vertrouwde basiscertificeringsinstanties van het account Lokale computer op de webserver van de gebruikersportal, zodat daar het certificaat wordt vertrouwd wanneer de SSL-verbinding tot stand wordt gebracht.

![Configuratie van MFA-server - webservice-SDK installeren](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

### <a name="install-the-service"></a>De service installeren

1. Blader **op de MFA-Server** naar het installatiepad.
2. Navigeer naar de map waarin de Azure MFA-server is geïnstalleerd. Standaard is dit de map **C:\Program Files\Azure Multi-Factor Authentication**.
3. Zoek het installatiebestand **MultiFactorAuthenticationMobileAppWebServiceSetup64**. Als de server **niet** internetgericht is, kopieert u het installatiebestand naar de internetgerichte server.
4. Als de MFA-server **niet** internetgericht is, schakelt u over naar de **internetgerichte server**.
5. Voer het installatiebestand **MultiFactorAuthenticationMobileAppWebServiceSetup64** uit als beheerder, wijzig de site desgewenst en wijzig de naam van de virtuele map eventueel in een korte naam.
6. Nadat de installatie is voltooid, bladert u naar **C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService** (of de overeenkomstige map op basis van de naam van de virtuele map) en bewerkt u het bestand Web.Config.

   * Zoek de sleutel **'WEB_SERVICE_SDK_AUTHENTICATION_USERNAME'** en wijzig **value=''** in **value='DOMAIN\User'** waarbij DOMAIN\User een serviceaccount is dat deel uitmaakt van de groep 'PhoneFactor Admins'.
   * Zoek de sleutel **'WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD'** en wijzig **value=''** in **value='Password"** waarbij Password het wachtwoord is voor het serviceaccount dat is ingevoerd op de vorige regel.
   * Zoek de instelling **pfMobile App Web Service_pfwssdk_PfWsSdk** en wijzig de waarde van **http://localhost:4898/PfWsSdk.asmx** in de URL van de webservice-SDK (voorbeeld: https://mfa.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).
   * Sla het bestand Web.Config op en sluit Kladblok.

   > [!NOTE]
   > Omdat voor deze verbinding SSL wordt gebruikt, moet u naar de webservice-SDK verwijzen met behulp van de **FQDN** en niet met het **IP-adres**. Er moet een SSL-certificaat voor de FQDN zijn uitgegeven en de URL moet overeenkomen met de naam op het certificaat.

7. Als de website waaronder de webservice voor mobiele apps is geïnstalleerd, nog niet is verbonden met een openbaar ondertekend certificaat, installeert u het certificaat op de server. Vervolgens opent u IIS-beheer en verbindt u het certificaat met de website.
8. Open op een computer een webbrowser en navigeer naar de URL waar de webservice Mobile Apps is geïnstalleerd (bijvoorbeeld: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService). Controleer of er geen certificaatwaarschuwingen of -fouten worden weergegeven.
9. Zie het Help-bestand van de MFA-server voor meer informatie over de methoden die beschikbaar zijn in de SDK voor webservices.

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De instellingen voor de webservice voor mobiele apps op de Azure Multi-Factor Authentication-server configureren

Nu de webservice voor mobiele apps is geïnstalleerd, moet u de Azure Multi-Factor Authentication-server voor gebruik met de gebruikersportal configureren.

1. Klik in de console van de Multi-Factor Authentication-server op het pictogram Gebruikersportal. Als gebruikers zelf een verificatiemethode mogen kiezen, schakelt u op het tabblad Instellingen onder **Toestaan dat gebruikers de methode selecteren** de optie **Mobiele app** in. Als deze functie niet is ingeschakeld, moeten eindgebruikers contact opnemen met uw helpdesk om de activering van de mobiele app te voltooien.
2. Schakel het selectievakje **Toestaan dat gebruikers de mobiele app activeren** in.
3. Schakel het selectievakje **Registreren van gebruikers toestaan** in.
4. Klik op het pictogram **Mobiele app**.
5. Voer in het veld **Webservice-URL mobiele app:** de URL in die wordt gebruikt met de virtuele map die is gemaakt bij het installeren van MultiFactorAuthenticationMobileAppWebServiceSetup64 (voorbeeld: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService/).
6. Voer in het veld **Accountnaam** de naam van het bedrijf of de organisatie in die moet worden weergegeven in de mobiele toepassing voor dit account.
   ![Configuratie van MFA-server - instellingen voor mobiele app](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)

## <a name="next-steps"></a>Volgende stappen

- [Geavanceerde scenario's met Azure Multi-Factor Authentication en VPN's van derden](multi-factor-authentication-advanced-vpn-configurations.md).
