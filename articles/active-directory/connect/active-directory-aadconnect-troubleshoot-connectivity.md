---
title: 'Azure AD Connect: Verbindingsproblemen oplossen | Microsoft Docs'
description: Wordt uitgelegd hoe u verbindingsproblemen met Azure AD Connect oplossen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: d30006fae8a0d495909b9a53cf0bffb5cc824433
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38295393"
---
# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Verbindingsproblemen met Azure AD Connect oplossen
In dit artikel wordt uitgelegd hoe connectiviteit tussen Azure AD Connect en Azure AD werkt en het oplossen van problemen met de netwerkverbinding. Deze problemen zijn ondergebracht in een omgeving met een proxyserver worden weergegeven.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Problemen met verbindingen in de installatiewizard
Azure AD Connect maakt gebruik van moderne verificatie (met behulp van de ADAL-bibliotheek) voor verificatie. De installatiewizard en de juiste synchronisatie-engine moeten machine.config correct worden geconfigureerd omdat deze twee .NET-toepassingen.

In dit artikel laten we zien hoe Fabrikam verbinding maakt met Azure AD via de proxy. De proxy-server met de naam fabrikamproxy en poort 8080 is gebruiken.

Eerst moeten we om te controleren of [ **machine.config** ](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

> [!NOTE]
> In bepaalde niet-Microsoft-blogs, wordt dat de wijzigingen moeten worden aangebracht aan miiserver.exe.config in plaats daarvan beschreven. Dit bestand is echter overschreven bij elke upgrade dus zelfs dat als het niet werkt tijdens de initiële installatie, het systeem werkt niet meer bij eerst een upgrade. Om die reden is de aanbeveling om bij te werken machine.config in plaats daarvan.
>
>

De proxy-server moet ook de vereiste URL's die wordt geopend. De officiële lijst wordt beschreven in de [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

URL's is de volgende tabel het absolute minimum dat bare kunnen verbinding maken met Azure AD helemaal. Deze lijst bevat geen eventuele optionele functies, zoals het terugschrijven van wachtwoorden of Azure AD Connect Health. Dit wordt hier beschreven om te helpen bij het oplossen van problemen voor de eerste configuratie.

| URL | Poort | Beschrijving |
| --- | --- | --- |
| mscrl.microsoft.com |HTTP/80 |Voor het downloaden van de CRL-lijsten. |
| \*.verisign.com |HTTP/80 |Voor het downloaden van de CRL-lijsten. |
| \*.entrust.com |HTTP/80 |Voor het downloaden van een lijst met CRL's voor MFA. |
| \*.windows.net |HTTPS/443 |Gebruikt om aan te melden bij Azure AD. |
| secure.aadcdn.microsoftonline-p.com |HTTPS/443 |Gebruikt voor MFA. |
| \*.microsoftonline.com |HTTPS/443 |Gebruikt voor het configureren van uw Azure AD-directory en gegevens importeren/exporteren. |

## <a name="errors-in-the-wizard"></a>Fouten in de wizard
De installatiewizard maakt gebruik van twee andere beveiligingscontext. Op de pagina **verbinding maken met Azure AD**, wordt de momenteel aangemelde gebruiker gebruikt. Op de pagina **configureren**, deze wordt gewijzigd in de [account dat de service voor de synchronisatie-engine](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). Als er een probleem is, wordt deze weergegeven waarschijnlijk al op de **verbinding maken met Azure AD** pagina in de wizard omdat de proxyconfiguratie globale.

De volgende problemen zijn de meest voorkomende fouten die in de installatiewizard optreden.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>De installatiewizard is niet correct geconfigureerd
Deze fout treedt op wanneer de wizard zelf de proxy kan bereiken.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

* Als u deze fout ziet, controleert u of de [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.
* Als die er goed uitziet, volgt u de stappen in [Controleer de proxy verbinding](#verify-proxy-connectivity) om te zien als het probleem zich buiten de wizard ook.

### <a name="a-microsoft-account-is-used"></a>Een Microsoft-account wordt gebruikt
Als u een **Microsoft-account** in plaats van een **school of organisatie** -account, ziet u een algemene fout.  
![Een Microsoft-Account wordt gebruikt](./media/active-directory-aadconnect-troubleshoot-connectivity/unknownerror.png)

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Het MFA-eindpunt kan niet worden bereikt
Deze fout treedt op als het eindpunt **https://secure.aadcdn.microsoftonline-p.com** kan niet worden bereikt en de globale beheerder is voor MFA is ingeschakeld.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

* Als u deze fout ziet, controleert u of dat het eindpunt **secure.aadcdn.microsoftonline p.com** is toegevoegd aan de proxy.

### <a name="the-password-cannot-be-verified"></a>Het wachtwoord kan niet worden geverifieerd.
Als de installatiewizard voltooid is in de verbinding te maken met Azure AD, maar het wachtwoord zelf kan niet worden geverifieerd op dat deze fout wordt weergegeven:  
![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

* Het wachtwoord is een tijdelijk wachtwoord en moet worden gewijzigd? Is het daadwerkelijk het juiste wachtwoord? Probeer te melden bij https://login.microsoftonline.com (op een andere computer dan de Azure AD Connect-server) en controleer of het account kan worden gebruikt.

### <a name="verify-proxy-connectivity"></a>Controleer de proxy-verbinding
Om te controleren of de Azure AD Connect-server de werkelijke connectiviteit met de Proxy- en Internet heeft, gebruikt u PowerShell om te zien als de proxy webaanvragen of niet toestaat. Voer in een PowerShell-prompt `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technisch de eerste aanroep is https://login.microsoftonline.com en deze URI als goed werkt, maar de andere URI is sneller te reageren.)

PowerShell maakt gebruik van de configuratie in machine.config contact opnemen met de proxy. De instellingen van winhttp/netsh moeten geen invloed op deze cmdlets.

Als de proxy correct is geconfigureerd, moet u de status geslaagd: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Als u ontvangt **kan geen verbinding maken met de externe server**, klikt u vervolgens PowerShell probeert te maken van een directe aanroep zonder met behulp van de proxy of DNS is niet correct geconfigureerd. Zorg ervoor dat de **machine.config** bestand correct is geconfigureerd.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Als de proxy niet correct is geconfigureerd, krijgt u een fout opgetreden: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

| Fout | Fouttekst | Opmerking |
| --- | --- | --- |
| 403 |Verboden |De proxy is niet geopend voor de aangevraagde URL. Terug naar de proxyconfiguratie en zorg ervoor dat de [URL's](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) zijn geopend. |
| 407 |Proxyverificatie is vereist |De proxyserver vereist een aanmelding en is geen taaksequencer opgegeven. Als uw proxyserver verificatie is vereist, zorg ervoor dat u deze instelling is geconfigureerd in machine.config. Controleer ook of dat u domeinaccounts gebruikt voor de gebruiker die de wizard en voor het serviceaccount. |

### <a name="proxy-idle-timeout-setting"></a>Proxy-instelling voor time-out voor inactiviteit
Wanneer Azure AD Connect een aanvraag voor exporteren naar Azure AD verzendt, kan Azure AD verwerking van aanvragen vóór het genereren van een antwoord tot vijf minuten duren. Dit kan gebeuren met name als er een aantal groepsbeleidsobjecten met grote groepslidmaatschappen opgenomen in de dezelfde aanvraag voor exporteren. Zorg ervoor dat de time-out voor inactiviteit Proxy is geconfigureerd voor het niet groter zijn dan 5 minuten. Anders kan onregelmatige connectiviteitsprobleem met Azure AD worden waargenomen op de Azure AD Connect-server.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Het communicatiepatroon tussen Azure AD Connect en Azure AD
Als u alle voorgaande stappen hebt gevolgd en nog steeds geen verbinding kan maken, kunt u op dit moment start netwerklogboeken kijken. In deze sectie wordt een patroon van normaal en geslaagde verbinding documenteren. Het is ook algemene red herrings die kan worden genegeerd wanneer u de netwerklogboeken leest aanbieding.

* Er zijn aanroepen naar https://dc.services.visualstudio.com. Dit is niet vereist dat deze URL openen in de proxy voor de installatie te voltooien en deze aanroepen kunnen worden genegeerd.
* U ziet dat de werkelijke hosts moeten in de DNS-naam van ruimte nsatc.net en andere naamruimten niet onder microsoftonline.com een lijst met dns-omzetting. Maar er zijn niet alle web service-aanvragen op de namen van de werkelijke en u hoeft niet te deze URL's toevoegen aan de proxy.
* De eindpunten adminwebservice provisioningapi detectie-eindpunten en zijn gebruikt voor het vinden van de echt eindpunt te gebruiken. Deze eindpunten zijn verschillend, afhankelijk van uw regio.

### <a name="reference-proxy-logs"></a>Naslaginformatie over proxy-Logboeken
Hier volgt een dump uit een werkelijke proxy-logboek en op de pagina van de wizard installatie van waar die is gemaakt (hetzelfde eindpunt van dubbele items zijn verwijderd). In deze sectie kan worden gebruikt als uitgangspunt voor uw eigen logboeken proxy en het netwerk. De werkelijke eindpunten kunnen afwijken in uw omgeving (met name de URL's in *cursief*).

**Verbinding maken met Azure AD**

| Time | URL |
| --- | --- |
| 1/11/2016 8:31 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:31 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |connect://*bba800-anchor*.microsoftonline.com:443 |
| 1/11/2016 8:32 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:33 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |connect://*bwsc02-relay*.microsoftonline.com:443 |

**Configureren**

| Time | URL |
| --- | --- |
| 1/11/2016 8:43 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:43 |connect://*bba800-anchor*.microsoftonline.com:443 |
| 1/11/2016 8:43 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://*bba900-anchor*.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://*bba800-anchor*.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:46 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |connect://*bwsc02-relay*.microsoftonline.com:443 |

**Initiële synchronisatie**

| Time | URL |
| --- | --- |
| 1/11/2016 8:48 |connect://login.windows.net:443 |
| 1/11/2016 8:49 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |connect://*bba900-anchor*.microsoftonline.com:443 |
| 1/11/2016 8:49 |connect://*bba800-anchor*.microsoftonline.com:443 |

## <a name="authentication-errors"></a>Verificatiefouten
In deze sectie bevat informatie over fouten die kunnen worden geretourneerd van ADAL (de verificatiebibliotheek die worden gebruikt door Azure AD Connect) en PowerShell. De fout uitgelegd kunt u in de volgende stappen te begrijpen.

### <a name="invalid-grant"></a>Ongeldig verleend
Ongeldige gebruikersnaam of wachtwoord. Zie voor meer informatie, [het wachtwoord kan niet worden geverifieerd](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Onbekende gebruikerstype
Uw Azure AD-directory kan niet worden gevonden of is opgelost. Misschien wilt u zich aanmeldt met een gebruikersnaam in een niet-geverifieerd domein?

### <a name="user-realm-discovery-failed"></a>Gebruiker Realm detectie is mislukt
Netwerk- of proxyinstellingen configuratieproblemen. Het netwerk kan niet worden bereikt. Zie [oplossen van problemen met de netwerkverbinding in de installatiewizard](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Het gebruikerswachtwoord is verlopen
Uw referenties zijn verlopen. Uw wachtwoord wijzigen.

### <a name="authorizationfailure"></a>AuthorizationFailure
Onbekend probleem.

### <a name="authentication-cancelled"></a>Verificatie is geannuleerd
De uitdaging multi-factor authentication (MFA) is geannuleerd.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Verificatie is voltooid, maar er is een verificatieprobleem met Azure AD PowerShell.

### <a name="azurerolemissing"></a>AzureRoleMissing
Verificatie is voltooid. U bent geen globale beheerder.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Verificatie is voltooid. Privileged identity management is ingeschakeld en u bent momenteel niet een globale beheerder. Zie voor meer informatie, [Privileged Identity Management](../privileged-identity-management/pim-getting-started.md).

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Verificatie is voltooid. Kan de bedrijfsgegevens niet ophalen uit Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Verificatie is voltooid. Kan de domeingegevens niet ophalen uit Azure AD.

### <a name="unexpected-exception"></a>Onverwachte uitzondering
Onverwachte fout opgetreden in de installatiewizard weergegeven. Kan gebeuren als u probeert te gebruiken een **Microsoft-Account** in plaats van een **school-of organisatieaccount**.

## <a name="troubleshooting-steps-for-previous-releases"></a>Stappen voor probleemoplossing voor eerdere versies.
Met versies vanaf build-nummer 1.1.105.0 (uitgebracht februari 2016), de aanmeldhulp is buiten gebruik gesteld. In deze sectie en de configuratie mag niet langer vereist, maar wordt opgeslagen als referentie.

Voor de eenmalige aanmelding in assistent om te werken, moet winhttp worden geconfigureerd. Deze configuratie kunt u doen met [ **netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>De aanmeldhulp is niet correct geconfigureerd
Deze fout treedt op wanneer de aanmeldhulp de proxy niet bereiken kan of de proxy is niet toegestaan voor de aanvraag.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

* Als u deze fout ziet, bekijkt u de configuratie van de proxy in [netsh](active-directory-aadconnect-prerequisites.md#connectivity) en controleer of deze juist is.
  ![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
* Als die er goed uitziet, volgt u de stappen in [Controleer de proxy verbinding](#verify-proxy-connectivity) om te zien als het probleem zich buiten de wizard ook.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
