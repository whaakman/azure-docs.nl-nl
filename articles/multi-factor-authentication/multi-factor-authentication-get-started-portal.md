---
title: Gebruikersportal voor Azure MFA-server | Microsoft Docs
description: Dit is de Azure Multi-Factor Authentication-pagina waarop wordt beschreven hoe u met Azure MFA en de gebruikersportal aan de slag kunt gaan.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/24/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 97845748ef44469d1ec6a91ff8184288dc6350d9
ms.contentlocale: nl-nl
ms.lasthandoff: 04/28/2017


---
# <a name="deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>De gebruikersportal implementeren voor de Azure Multi-Factor Authentication-server
De gebruikersportal is een IIS-website waar gebruikers zich kunnen registreren bij Azure Multi-Factor Authentication en hun account kunnen beheren. Een gebruiker kan bij de volgende aanmelding zijn telefoonnummer en pincode wijzigen of verificatie in twee stappen overslaan.

Gebruikers kunnen zich bij de gebruikersportal aanmelden met hun gebruikelijke gebruikersnaam en wachtwoord en zullen een telefoonoproep voor verificatie in twee stappen aannemen of beveiligingsvragen beantwoorden om hun verificatie te voltooien. Als gebruikersregistratie is toegestaan, configureren gebruikers hun telefoonnummer en pincode de eerste keer dat ze zich aanmelden bij de gebruikersportal.

Er kunnen beheerders voor de gebruikersportal worden ingesteld. Aan hen kan ook een machtiging worden verleend om nieuwe gebruikers toe te voegen en bestaande gebruikers bij te werken.

<center>![Instellen](./media/multi-factor-authentication-get-started-portal/install.png)</center>

>[!NOTE] 
>De gebruikersportal is alleen beschikbaar met Multi-Factor Authentication Server. Als u Multi-Factor Authentication in de cloud gebruikt, verwijst u uw gebruikers naar [Uw account instellen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-first-time.md) of [Uw instellingen beheren voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md).


## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>De gebruikersportal op dezelfde server implementeren als de Azure Multi-Factor Authentication-server
Als u de gebruikersportal op dezelfde server als de Azure Multi-Factor Authentication-server wilt installeren, moet aan de volgende vereisten worden voldaan:

* IIS inclusief asp.net en compatibiliteit met IIS 6-metabase (voor IIS 7 of hoger)
* Een account met beheerdersrechten voor de computer en het domein, indien van toepassing. Het account heeft machtigingen nodig om Active Directory-beveiligingsgroepen te maken.

Voer de volgende stappen uit om de gebruikersportal te implementeren:

1. Klik op de Azure Multi-Factor Authentication-server in het menu links op het **Gebruikersportal**-pictogram en daarna op **Gebruikersportal installeren**.
2. Klik op **Volgende**.
3. Klik op **Volgende**.
4. Als de computer lid is van een domein en Active Directory niet is geconfigureerd voor het beveiligen van de communicatie tussen de gebruikersportal en de Azure Multi-Factor Authentication-service, wordt de Active Directory-stap weergegeven. Klik op de knop **Volgende** om deze configuratie automatisch te voltooien.
5. Klik op **Volgende**.
6. Klik op **Volgende**.
7. Klik op **Sluiten**.
8. Open op een computer een webbrowser en navigeer naar de URL waar de gebruikersportal is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/MultiFactorAuth). Controleer of er geen certificaatwaarschuwingen of -fouten worden weergegeven.

<center>![Instellen](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploy-the-user-portal-on-a-separate-server"></a>De gebruikersportal implementeren op een afzonderlijke server
Als de server waarop Azure Multi-Factor Authentication wordt uitgevoerd niet internetgericht is, moet u de gebruikersportal installeren op een afzonderlijke server die internetgericht is. 

Als uw organisatie gebruikmaakt van de Microsoft Authenticator-app als een van de verificatiemethoden en de gebruikersportal op een eigen server wil implementeren, moet u aan de volgende vereisten voldoen: 

* U moet versie 6.0 of een recentere versie van de Azure Multi-Factor Authentication-server gebruiken.
* De gebruikersportal moet zijn geïnstalleerd op een internetgerichte webserver waarop Microsoft Internet Information Services (IIS) 6.x of een recentere versie wordt uitgevoerd.
* Wanneer u IIS 6.x gebruikt, zorgt u ervoor dat ASP.NET v2.0.50727 is geïnstalleerd, is geregistreerd en is ingesteld op **Toegestaan**.
* Bij het gebruik van IIS 7.x of een recentere versie zijn ASP.NET en compatibiliteit met IIS 6-metabase-functieservices vereist.
* De gebruikersportal moet zijn beveiligd met een SSL-certificaat.
* De webservice-SDK van Azure Multi-Factor Authentication moet zijn beveiligd met een SSL-certificaat.
* Zorg dat de gebruikersportal via SSL verbinding kan maken met de webservice-SDK van Azure Multi-Factor Authentication.
* Zorg dat de gebruikersportal bij de webservice-SDK van Azure Multi-Factor Authentication kan verifiëren met behulp van de referenties van een serviceaccount in de beveiligingsgroep PhoneFactor Admins. Dit serviceaccount en deze groep bevinden zich in Active Directory als de Azure Multi-Factor Authentication-server wordt uitgevoerd op een server die aan een domein is gekoppeld. Dit serviceaccount en deze groep bevinden zich lokaal op de Azure Multi-Factor Authentication-server als deze niet aan een domein is gekoppeld.

Voor de installatie van de gebruikersportal op een andere server dan de Azure Multi-Factor Authentication-server moeten de volgende drie stappen worden uitgevoerd:

1. Installeer de webservice-SDK op dezelfde server als de Azure Multi-Factor Authentication-server
2. De gebruikersportal installeren
3. De instellingen voor de gebruikersportal configureren in de Azure Multi-Factor Authentication-server

### <a name="step-1-install-the-web-service-sdk"></a>Stap 1: de webservice-SDK installeren
Als de webservice-SDK van de Azure Multi-Factor Authentication nog niet op de Azure Multi-Factor Authentication-server is geïnstalleerd, gaat u naar die server en opent u de Azure Multi-Factor Authentication-server. Klik op het pictogram voor de **webservice-SDK** en vervolgens op **Webservice-SDK installeren...**. Volg de aanwijzingen die worden gegeven. 

De webservice-SDK moet met een SSL-certificaat worden beveiligd. Een zelfondertekend certificaat volstaat hiervoor, maar het moet dan wel worden geïmporteerd in het certificaatarchief Vertrouwde basiscertificeringsinstanties van het account Lokale computer op de server. De webservice-SDK is nu in staat dat certificaat te vertrouwen wanneer deze de SSL-verbinding tot stand brengt.

<center>![Instellen](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="step-2-install-the-user-portal"></a>Stap 2: de gebruikersportal installeren
Voordat u de gebruikersportal op een aparte server installeert, houdt u rekening met de volgende aanbevolen procedures:

* Het is handig om een webbrowser te openen op de internetgerichte webserver en te navigeren naar de URL van de webservice-SDK die in het bestand web.config is ingevoerd. Als de browser de webservice kan bereiken, wordt u normaal om referenties gevraagd. Geef de gebruikersnaam en het wachtwoord op precies zoals deze in het bestand web.config zijn ingevoerd. Controleer of er geen certificaatwaarschuwingen of -fouten worden weergegeven.
* Als zich voor de webserver van de gebruikersportal een omgekeerde proxy of firewall bevindt en deze SSL-offloading uitvoert, kunt u het bestand web.config van de gebruikersportal bewerken en de volgende sleutel toevoegen aan de sectie `<appSettings>`, zodat de gebruikersportal http in plaats van https kan gebruiken:

    `<add key="SSL_REQUIRED" value="false"/>`

#### <a name="to-install-the-user-portal"></a>De gebruikersportal installeren
1. Open Windows Verkenner op de Azure MFA-server en navigeer naar de map waarin de Azure Multi-Factor Authentication-server is geïnstalleerd (gewoonlijk C:\Program Files\Multi-Factor Authentication Server). Kopieer de 32-bits of 64-bits versie van het MultiFactorAuthenticationUserPortalSetup-installatiebestand naar de internetgerichte server.
2. Voer op de internetgerichte webserver het bestand uit met beheerdersrechten. De eenvoudigste manier om dit te doen is een opdrachtprompt openen als een beheerder en navigeren naar de locatie waar het installatiebestand is gekopieerd.
3. Voer het MultiFactorAuthenticationUserPortalSetup64-installatiebestand uit en wijzig de naam van de site en de virtuele map, indien gewenst.
4. Nadat de installatie van de gebruikersportal is voltooid, bladert u naar C:\inetpub\wwwroot\MultiFactorAuth (of de overeenkomstige map op basis van de naam van de virtuele map) en bewerkt u het bestand web.config.
5. Zoek de sleutel USE_WEB_SERVICE_SDK en wijzig de waarde van **false** (onwaar) in **true** (waar). 
6. Ga naar de sleutel WEB_SERVICE_SDK_AUTHENTICATION_USERNAME en stel de waarde voor de gebruikersnaam van het serviceaccount in de PhoneFactor Admins-beveiligingsgroep in. Gebruik een gekwalificeerde gebruikersnaam, bijvoorbeeld domein\gebruikersnaam of computer\gebruikersnaam. 
7. Ga naar de sleutel WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD en stel de waarde voor het wachtwoord van het serviceaccount in de PhoneFactor Admins-beveiligingsgroep in.
8. Zoek de instelling pfup_pfwssdk_PfWsSdk en wijzig de waarde van http://localhost:4898/PfWsSdk.asmx in de URL van de webservice-SDK die wordt uitgevoerd op de Azure Multi-Factor Authentication-server (bijvoorbeeld https://computer1.domein.lokaal/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Aangezien voor deze verbinding SSL wordt gebruikt, verwijst u naar de webservice-SDK met de servernaam, en niet het IP-adres, omdat het SSL-certificaat is uitgegeven voor de servernaam. Als de naam van de server niet wordt omgezet in een IP-adres van de internetgerichte server, kunt u een vermelding toevoegen aan het hostbestand op die server, om de naam van de Azure Multi-Factor Authentication-server toe te wijzen aan het IP-adres ervan. Sla het bestand web.config op nadat de wijzigingen zijn aangebracht.

    Ga naar [Secure your resources using Azure Multi-Factor Authentication Server with AD FS](multi-factor-authentication-get-started-adfs-w2k12.md#edit-the-multifactorauthenticationadfsadapterconfig-file) (Uw resources beveiligen met behulp van de Azure Multi-Factor Authentication-server met AD FS) voor meer informatie over het bewerken van het configuratiebestand.

7. Als de website waaronder de gebruikersportal is geïnstalleerd (bijvoorbeeld Standaardwebsite) nog niet is verbonden met een openbaar ondertekend certificaat, installeert u het certificaat op de server, opent u IIS-beheer en verbindt u het certificaat aan de website.
8. Open op een computer een webbrowser en navigeer naar de URL waar de gebruikersportal is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/MultiFactorAuth). Controleer of er geen certificaatwaarschuwingen of -fouten worden weergegeven.

### <a name="step-3-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Stap 3: de instellingen voor de gebruikersportal op de Azure Multi-Factor Authentication-server configureren
Nu de gebruikersportal is geïnstalleerd, dient u de Azure Multi-Factor Authentication-server te configureren voor gebruik met de portal.

De Azure Multi-Factor Authentication-server biedt verschillende opties voor de gebruikersportal.  De volgende tabel geeft een lijst van deze opties en een verklaring van de taak waarvoor ze worden gebruikt.

| Instellingen van gebruikersportal | Beschrijving |
|:--- |:--- |
| URL gebruikersportal | Hiermee kunt u de URL invoeren van de locatie waar de portal wordt gehost. |
| Primaire authenticatie | Hiermee kunt u opgeven welk verificatietype moet worden gebruikt bij de aanmelding bij de portal.  Windows-, Radius- of LDAP-authenticatie. |
| Aanmelden van gebruikers toestaan | Hiermee kunnen gebruikers een gebruikersnaam en wachtwoord invoeren op de aanmeldingspagina voor de gebruikersportal.  Als deze instelling niet is ingeschakeld, worden de vakken grijs weergegeven. |
| Registreren van gebruikers toestaan | Hiermee kunnen gebruikers zich registreren voor Multi-Factor Authentication doordat ze naar een installatiescherm worden geleid waar om aanvullende informatie zoals een telefoonnummer wordt gevraagd. Bij Vragen om alternatief telefoonnummer kunnen gebruikers een tweede telefoonnummer opgeven. Bij Vragen om OATH-token van derden kunnen gebruikers een OATH-token van een derde opgeven. |
| Gebruikers toestaan om eenmalige toegang te initiëren | Hiermee kunt u gebruikers toestaan om eenmalige toegang te initiëren.  Deze optie wordt van kracht wanneer de gebruiker zich de volgende keer aanmeldt. De optie Vragen om bypass-seconden biedt gebruikers een invoervak, waarin zij de standaardwaarde van 300 seconden kunnen wijzigen. Anders duurt de eenmalige toegang slechts 300 seconden. |
| Toestaan dat gebruikers de methode selecteren | Hiermee kunnen gebruikers hun primaire contactmethode opgeven.  Dit kan zijn: telefoonoproep, sms-bericht, mobiele app of OATH-token. |
| Toestaan dat gebruikers de taal selecteren | Hiermee kunnen gebruikers de taal wijzigen die wordt gebruikt voor de telefoonoproep, het sms-bericht, de mobiele app of het OATH-token. |
| Toestaan dat gebruikers de mobiele app activeren | Hiermee kunnen gebruikers een activeringscode genereren om het activeringsproces van de mobiele app te voltooien dat bij de server wordt gebruikt.  U kunt ook instellen op hoeveel apparaten zij dit kunnen activeren, met een aantal van 1 t/m 10. |
| Beveiligingsvragen gebruiken als terugvaloptie | Hiermee kunt u beveiligingsvragen toestaan als verificatie in twee stappen is mislukt.  U kunt opgeven hoeveel beveiligingsvragen juist moeten worden beantwoord. |
| Toestaan dat gebruikers OATH-token van derden koppelen | Hiermee kunnen gebruikers een OATH-token van derden opgeven. |
| OATH-token gebruiken als terugvaloptie | Hiermee kunt u het gebruik van een OATH-token toestaan als verificatie in twee stappen is mislukt. U kunt ook de sessietime-out in minuten opgeven. |
| Logboekregistratie inschakelen | Hiermee schakelt u de logboekregistratie in op de gebruikersportal. De logboekbestanden bevinden zich op: C:\Program Files\Multi-Factor Authentication Server\Logs. |

De meeste van deze instellingen zijn zichtbaar voor de gebruiker zodra deze zijn ingeschakeld en de gebruiker zich bij de gebruikersportal heeft aangemeld.

![Instellingen gebruikersportal](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>De instellingen voor de gebruikersportal configureren op de Azure Multi-Factor Authentication-server
1. Klik op de Azure Multi-Factor Authentication-server op het pictogram **Gebruikersportal**. Voer op het tabblad Instellingen in het tekstvak **URL gebruikersportal** de URL naar de gebruikersportal in. Als de e-mailfunctionaliteit is ingeschakeld, zal deze URL worden ingevoegd in e-mailberichten die worden verzonden naar gebruikers wanneer ze worden geïmporteerd op de Azure Multi-Factor Authentication-server.
2. Kies de instellingen die u in de gebruikersportal wilt gebruiken. Als u bijvoorbeeld wilt toestaan dat gebruikers hun verificatiemethoden kiezen, zorgt u ervoor dat de optie **Toestaan dat gebruikers de methode selecteren** wordt ingeschakeld, samen met de methoden waaruit ze kunnen kiezen.
3. Klik rechtsboven op de **Help**-koppeling voor meer informatie over de weergegeven instellingen.

<center>![Instellen](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Tabblad Beheerders
Op het tabblad **Beheerders** kunt u gebruikers toevoegen die over beheerdersrechten zullen beschikken.  Wanneer u een beheerder toevoegt, kunt u de machtigingen die zij krijgen exact instellen. Klik op de knop **Toevoegen** en selecteer een gebruiker en de bijbehorende machtigingen. Klik vervolgens op **Toevoegen**.

![Beheerders van gebruikersportal](./media/multi-factor-authentication-get-started-portal/admin.png)

## <a name="security-questions"></a>Beveiligingsvragen
Op het tabblad **Beveiligingsvragen** kunt u de beveiligingsvragen opgeven die gebruikers juist moeten beantwoorden als de optie **Beveiligingsvragen gebruiken als terugvaloptie** is geselecteerd.  Azure Multi-Factor Authentication-server wordt geleverd met standaardvragen die u kunt gebruiken. U kunt de volgorde wijzigen of uw eigen vragen toevoegen.  Als u uw eigen vragen toevoegt, kunt u ook de taal opgeven waarin u die vragen wilt stellen.

![Beveiligingsvragen van gebruikersportal](./media/multi-factor-authentication-get-started-portal/secquestion.png)

## <a name="saml"></a>SAML
Gebruik het tabblad **SAML** om te configureren dat de gebruikersportal claims accepteert van een id-provider die SAML gebruikt.  U kunt hier ook de time-out voor een SAML-sessie, het verificatiecertificaat en de Omleidings-URL voor afmelden opgeven.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Goedgekeurde IP-adressen
Op het tabblad **Goedgekeurde IP-adressen** kunt u een IP-adres of een IP-adresbereik opgeven dat kan worden toegevoegd zodat verificatie in twee stappen wordt overgeslagen als een gebruiker zich aanmeldt vanaf een van deze IP-adressen.

![Goedgekeurde IP-adressen gebruikersportal](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Selfservice voor gebruikersregistratie
Als u wilt dat uw gebruikers zich kunnen aanmelden en registreren, selecteert u op het tabblad Instellingen de opties **Aanmelden van gebruikers toestaan** en **Registreren van gebruikers toestaan**. Houd er rekening mee dat de instellingen die u selecteert, invloed hebben op de gebruikerservaring bij het aanmelden.

Wanneer bijvoorbeeld een gebruiker zich voor het eerst bij de gebruikersportal aanmeldt, wordt de gebruiker naar de pagina met gebruikersinstellingen van Azure Multi-Factor Authentication geleid.  Afhankelijk van hoe u Azure Multi-Factor Authentication hebt geconfigureerd, kan een gebruiker al dan niet de verificatiemethode selecteren.  

Als de gebruiker de verificatiemethode Spraakoproep selecteert of als de gebruiker vooraf is geconfigureerd om die methode te gebruiken, wordt de gebruiker gevraagd zijn of haar primaire telefoonnummer en, indien van toepassing, zijn of haar toestelnummer op te geven.  Mogelijk kan de gebruiker ook een alternatief telefoonnummer invoeren.  

![Goedgekeurde IP-adressen gebruikersportal](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Als de gebruiker bij de verificatie een pincode moet gebruiken, wordt de gebruiker ook gevraagd een pincode te maken.  Nadat de gebruiker zijn of haar telefoonnummer(s) en pincode (indien van toepassing) heeft ingevoerd, klikt de gebruiker op de knop **Bel me nu om te authenticeren**.  Azure Multi-Factor Authentication zal een verificatie door middel van een spraakoproep uitvoeren met behulp van het primaire telefoonnummer van de gebruiker.  De gebruiker moet de spraakoproep beantwoorden en zijn of haar pincode invoeren (indien van toepassing) en daarna op # drukken om door te gaan naar de volgende stap van het zelfregistratieproces.   

Als de gebruiker de verificatiemethode Sms-bericht selecteert of vooraf is geconfigureerd om die methode te gebruiken, wordt de gebruiker gevraagd zijn of haar mobiele telefoonnummer op te geven.  Als de gebruiker bij de verificatie een pincode moet gebruiken, wordt de gebruiker ook gevraagd een pincode op te geven.  Nadat de gebruiker zijn of haar telefoonnummer en pincode (indien van toepassing) heeft ingevoerd, klikt de gebruiker op de knop **Sms me nu om te authenticeren**.  Azure Multi-Factor Authentication zal een sms-verificatie uitvoeren met behulp van de mobiele telefoon van de gebruiker.  De gebruiker ontvangt een sms-bericht met een eenmalige-wachtwoordcode (OTP). Vervolgens beantwoordt de gebruiker het bericht met die OTP plus de pincode (indien van toepassing).

![Sms gebruikersportal](./media/multi-factor-authentication-get-started-portal/text.png)   

Als de gebruiker de verificatiemethode Mobiele app selecteert, wordt de gebruiker op de pagina gevraagd om de Microsoft Authenticator-app op zijn of haar apparaat te installeren en een activeringscode te genereren.  Na de installatie van de app klikt de gebruiker op de knop Activeringscode genereren.    

> [!NOTE]
> De gebruiker dient pushmeldingen voor het apparaat in te schakelen om de Microsoft Authenticator-app te gebruiken.

Op de pagina worden vervolgens een activeringscode en een URL weergegeven, samen met een afbeelding van een streepjescode.  Als de gebruiker bij de verificatie een pincode moet gebruiken, wordt de gebruiker ook gevraagd een pincode op te geven.  De gebruiker voert de activeringscode en URL in de Microsoft Authenticator-app in of gebruikt de streepjescodescanner om de afbeelding van de streepjescode te scannen en klikt op de knop Activeren.    

Nadat de activering is voltooid, klikt de gebruiker op de knop **Nu verifiëren**.  Azure Multi-Factor Authentication zal een verificatie uitvoeren met behulp van de mobiele app van de gebruiker.  De gebruiker moet in de mobiele app de pincode invoeren (indien van toepassing) en op de knop Verifiëren drukken om door te gaan naar de volgende stap van het zelfregistratieproces.  

Als de beheerders de Azure Multi-Factor Authentication-server hebben geconfigureerd om beveiligingsvragen en antwoorden te verzamelen, wordt de gebruiker hierna naar de pagina Beveiligingsvragen geleid.  De gebruiker moet vier beveiligingsvragen selecteren en deze juist beantwoorden.    

![Beveiligingsvragen van gebruikersportal](./media/multi-factor-authentication-get-started-portal/secq.png)  

De zelfregistratie van de gebruiker is nu voltooid en de gebruiker is aangemeld bij de gebruikersportal.  Gebruikers kunnen zich later op elk gewenst moment opnieuw aanmelden bij de gebruikersportal om hun telefoonnummers, pincodes, verificatiemethoden en beveiligingsvragen te wijzigen als dat door de beheerders is toegestaan.


