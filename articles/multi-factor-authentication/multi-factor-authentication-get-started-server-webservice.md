---
title: Webservice voor mobiele apps voor de Azure MFA-server | Microsoft Docs
description: De Microsoft verificator-app biedt een extra optie voor verificatie buiten de band.  Hierdoor kan de MFA-server pushmeldingen naar gebruikers sturen.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017,it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 20afeb3ba290ddf728d2b52c076c7a57fadc77c6
ms.openlocfilehash: 4014bf0217e25ea9bc8473ef2383279e5eb79b87
ms.contentlocale: nl-nl
ms.lasthandoff: 02/28/2017

---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Authenticatie met de mobiele app integreren met Azure Multi-Factor Authentication-server

De Microsoft verificator-app biedt een extra optie voor verificatie buiten de band. In plaats van tijdens het aanmelden een automatisch telefoongesprek met de gebruiker tot stand te brengen of hem een sms-bericht te sturen, verzendt Azure Multi-Factor Authentication een melding naar de Microsoft verificator-app op de smartphone of tablet van de gebruiker. De gebruiker hoeft in de app enkel op **Verifiëren** te tikken (of een pincode in te voeren en op Verifiëren te tikken) om de aanmelding te voltooien.

Het gebruik van een mobiele app voor verificatie in twee stappen wordt aanbevolen wanneer het telefonische bereik onbetrouwbaar is. Als u de app als een OATH-tokengenerator gebruikt, hebt u geen netwerk- of internetverbinding nodig.

Voor de installatie van de gebruikersportal op een andere server dan de Azure Multi-Factor Authentication-server moeten de volgende stappen worden uitgevoerd:

1. De webservice-SDK installeren
2. De webservice voor mobiele apps installeren
3. De instellingen voor de webservice voor mobiele apps op de Azure Multi-Factor Authentication-server configureren
4. De Microsoft verificator-app voor eindgebruikers activeren

## <a name="requirements"></a>Vereisten

Als u de Microsoft verificator-app wilt gebruiken, is het volgende vereist om de app correct te laten communiceren met de webservice voor mobiele apps:

* Azure Multi-Factor Authentication-server versie 6.0 of hoger.
* De webservice voor mobiele apps moet zijn geïnstalleerd op een internetgerichte webserver waarop Microsoft® [Internet Information Services (IIS) 7.x of hoger](http://www.iis.net/) wordt uitgevoerd.
* ASP.NET versie 4.0.30319 moet zijn geïnstalleerd en geregistreerd en zijn ingesteld op Toegestaan.
* Vereiste functieservices omvatten ASP.NET en compatibiliteit met IIS 6-metabase.
* Webservice voor mobiele apps moet toegankelijk zijn via een openbare URL.
* Webservice voor mobiele apps moet met een SSL-certificaat zijn beveiligd.
* De webservice-SDK van Azure Multi-Factor Authentication moet zijn geïnstalleerd in IIS 7.x of hoger op dezelfde server als de Azure Multi-Factor Authentication-server.
* De webservice-SDK van Azure Multi-Factor Authentication moet zijn beveiligd met een SSL-certificaat.
* De webservice voor mobiele apps moet via SSL verbinding kunnen maken met de webservice-SDK van Azure Multi-Factor Authentication.
* De webservice voor mobiele apps moet zich bij de webservice-SDK van Azure MFA kunnen verifiëren met behulp van de referenties van een serviceaccount dat lid is van de beveiligingsgroep PhoneFactor Admins. Dit serviceaccount en deze groep bevinden zich in Active Directory als de Azure Multi-Factor Authentication-server lid is van een server die aan een domein is gekoppeld. Dit serviceaccount en deze groep bevinden zich lokaal op de Azure Multi-Factor Authentication-server als deze niet aan een domein is gekoppeld.


## <a name="install-the-web-service-sdk"></a>De webservice-SDK installeren
Als de webservice-SDK van de Azure Multi-Factor Authentication nog niet op de Azure MFA-server (Multi-Factor Authentication) is geïnstalleerd, gaat u naar die server en opent u de Azure MFA-server.

1. Klik op het pictogram van de webservice-SDK.
2. Klik op **Webservice-SDK installeren** en volg de aanwijzingen die worden weergegeven.

De webservice-SDK moet met een SSL-certificaat worden beveiligd. Voor dit doel kan een zelfondertekend certificaat worden gebruikt. Importeer het certificaat in het certificaatarchief Vertrouwde basiscertificeringsinstanties van het account Lokale computer op de webserver van de gebruikersportal, zodat daar het certificaat wordt vertrouwd wanneer de SSL-verbinding tot stand wordt gebracht.

![Instellen](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

## <a name="install-the-mobile-app-web-service"></a>De webservice voor mobiele apps installeren
Voordat u de webservice voor mobiele apps installeert, moet u op de volgende details letten:

* Als de gebruikersportal van Azure MFA al is geïnstalleerd op de internetgerichte server, kunnen de gebruikersnaam, het wachtwoord en de URL uit het bestand web.config van de gebruikersportal naar de webservice-SDK worden gekopieerd.
* Het is handig om een webbrowser te openen op de internetgerichte webserver en te navigeren naar de URL van de webservice-SDK die in het bestand web.config is ingevoerd. Als de browser de webservice kan bereiken, wordt u normaal om referenties gevraagd. Geef de gebruikersnaam en het wachtwoord op precies zoals deze in het bestand web.config zijn ingevoerd. Controleer of er geen certificaatwaarschuwingen of -fouten worden weergegeven.
* Als zich vóór de webserver van de webservice voor mobiele apps een omgekeerde proxy of firewall bevindt en SSL-offloading wordt uitvoert, kunt u het bestand web.config van de webservice voor mobiele apps bewerken, zodat de webservice voor mobiele apps http in plaats van https kan gebruiken. SSL is nog steeds vereist vanuit de mobiele app naar de firewall/omgekeerde proxy. Voeg de volgende sleutel toe aan de sectie \<appSettings\>:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-service"></a>De service installeren

1. Open Windows Verkenner op de Azure Multi-Factor Authentication-server en navigeer naar de map waarin de Azure MFA-server is geïnstalleerd (gewoonlijk C:\Program Files\Azure Multi-Factor Authentication). Kies de 32-bits of 64-bits versie van het Multi-Factor AuthenticationPhoneAppWebServiceSetup-installatiebestand. Kopieer het installatiebestand naar de internetgerichte server.

2. Voer op de internetgerichte webserver het installatiebestand uit met beheerdersrechten. Open als beheerder een opdrachtprompt en navigeer naar de locatie waarnaar het installatiebestand is gekopieerd.

3. Voer het installatiebestand MultiFactorAuthenticationMobileAppWebServiceSetup uit, waarbij u desgewenst de locatie kunt wijzigen en de naam van de virtuele map kunt wijzigen in een korte naam, bijvoorbeeld 'PA'.

  De korte naam voor de virtuele map wordt aanbevolen omdat gebruikers tijdens de activering de URL voor de webservice voor mobiele apps op hun mobiele apparaat moeten invoeren.

4. Wanneer het Azure-installatiebestand MultiFactorAuthenticationMobileAppWebServiceSetup is geïnstalleerd, bladert u naar C:\inetpub\wwwroot\PA (of de overeenkomstige map op basis van de naam van de virtuele map) en bewerkt u het bestand web.config.

5. Zoek de sleutels WEB_SERVICE_SDK_AUTHENTICATION_USERNAME en WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD. Stel de waarde voor de gebruikersnaam en het wachtwoord in van het serviceaccount dat lid is van de veiligheidsgroep PhoneFactor Admins. Dit kan hetzelfde account zijn dat ook is gebruikt als de identiteit van de gebruikersportal van Azure Multi-Factor Authentication, als die eerder is geïnstalleerd. Zorg ervoor dat u de gebruikersnaam en het wachtwoord invoert tussen de aanhalingstekens aan het einde van de regel (value=””/>). Gebruik een gekwalificeerde gebruikersnaam, bijvoorbeeld domein\gebruikersnaam of computer\gebruikersnaam.  

6. Zoek de instelling Service_pfwssdk_PfWsSdk van PF Mobile App Web. Wijzig de waarde van *http://localhost:4898/PfWsSdk.asmx* in de URL van de webservice-SDK die wordt uitgevoerd op de Azure Multi-Factor Authentication-server (bijvoorbeeld https://computer1.domein.lokaal/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).

  Omdat voor deze verbinding SSL wordt gebruikt, moet u naar de webservice-SDK verwijzen met behulp van de servernaam en niet met het IP-adres. Er moet een SSL-certificaat voor de servernaam zijn uitgegeven en de URL moet overeenkomen met de naam op het certificaat. Mogelijk wordt de servernaam niet vanuit de internetgerichte server in een IP-adres omgezet. In dat geval voegt u een vermelding toe aan het hostbestand op die server, om de naam van de Azure Multi-Factor Authentication-server toe te wijzen aan het IP-adres ervan. Sla het bestand web.config op nadat de wijzigingen zijn aangebracht.

7. Als de website waaronder de webservice voor mobiele apps is geïnstalleerd, nog niet is verbonden met een openbaar ondertekend certificaat, installeert u het certificaat op de server. Vervolgens opent u IIS-beheer en verbindt u het certificaat met de website.

8. Open een webbrowser op een computer en navigeer naar de URL waar de webservice voor mobiele apps is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/PA). Controleer of er geen certificaatwaarschuwingen of -fouten worden weergegeven.

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De instellingen voor de webservice voor mobiele apps op de Azure Multi-Factor Authentication-server configureren
Nu de webservice voor mobiele apps is geïnstalleerd, moet u de Azure Multi-Factor Authentication-server voor gebruik met de gebruikersportal configureren.

1. Klik in de Azure MFA-server op het pictogram Gebruikersportal. Als gebruikers zelf een verificatiemethode mogen kiezen, schakelt u op het tabblad Instellingen onder **Toestaan dat gebruikers de methode selecteren** de optie **Mobiele app** in. Als deze functie niet is ingeschakeld, moeten eindgebruikers contact opnemen met uw helpdesk om de activering van de mobiele App te voltooien.
2. Schakel het selectievakje **Toestaan dat gebruikers de mobiele app activeren** in.
3. Schakel het selectievakje **Registreren van gebruikers toestaan** in.
4. Klik op het pictogram Mobiele app.
5. Voer de URL in die wordt gebruikt met de virtuele map die is gemaakt bij de installatie van de Azure- MultiFactorAuthenticationMobileAppWebServiceSetup. U kunt een accountnaam invoeren in het daarvoor bestemde vak. Deze bedrijfsnaam wordt weergegeven in de mobiele toepassing. Als u dit vak leeg laat, wordt de naam weergegeven van de provider voor Multi-Factor Authentication die in de klassieke Azure-portal is gemaakt.

<center>![Instellen](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>

