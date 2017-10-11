---
title: 'Azure AD Connect: Problemen met verbindingen | Microsoft Docs'
description: Legt uit hoe u problemen met verbindingen met Azure AD Connect.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: f9631e8a383b88421c55d9c42c8059df9e732800
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Oplossen van problemen met de netwerkverbinding met Azure AD Connect
Dit artikel wordt uitgelegd hoe de verbinding tussen Azure AD Connect en Azure AD werkt en het oplossen van problemen met de netwerkverbinding. Deze problemen zijn waarschijnlijk kunnen worden bekeken in een omgeving met een proxyserver.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Problemen met verbindingen in de installatiewizard
Azure AD Connect maakt gebruik van moderne verificatie (met behulp van de ADAL-bibliotheek) voor verificatie. De installatiewizard en de juiste synchronisatie-engine vereisen machine.config correct worden geconfigureerd omdat deze twee .NET-toepassingen.

In dit artikel, laten we zien hoe Fabrikam verbinding maakt met Azure AD via de proxy. De proxy-server met de naam fabrikamproxy en poort 8080 wordt gebruikt.

Eerst moet ervoor zorgen [ **machine.config** ](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.  
![machineconfig uit te voeren](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

> [!NOTE]
> In sommige blogs niet-Microsoft wordt dat moet worden gewijzigd miiserver.exe.config in plaats daarvan beschreven. Dit bestand is echter overschreven bij elke upgrade dus zelfs dat als dit tijdens de initiële installatie werkt, het systeem werkt niet bij een eerste upgrade. Aangeraden wordt om die reden machine.config in plaats daarvan bijwerken.
>
>

De proxy-server moet ook de gewenste URL's die worden geopend. De officiële lijst wordt beschreven in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

URL's is de volgende tabel absolute bare minimaal kunnen helemaal verbinden met Azure AD. Deze lijst bevat geen eventuele optionele functies, zoals wachtwoord terugschrijven of Azure AD Connect Health. Dit wordt hier beschreven om te helpen bij het oplossen van problemen voor de eerste configuratie.

| URL | Poort | Beschrijving |
| --- | --- | --- |
| mscrl.Microsoft.com |HTTP-/ 80 |Gebruikt voor het downloaden van de CRL-lijsten. |
| \*. verisign.com |HTTP-/ 80 |Gebruikt voor het downloaden van de CRL-lijsten. |
| \*. entrust.com |HTTP-/ 80 |Gebruikt voor het downloaden van een lijst met CRL voor MFA. |
| \*.windows.net |HTTPS/443 |Gebruikt om aan te melden bij Azure AD. |
| Secure.aadcdn.microsoftonline p.com |HTTPS/443 |Gebruikt voor MFA. |
| \*.microsoftonline.com |HTTPS/443 |Gebruikt voor het configureren van uw Azure AD-directory en gegevens voor importeren/exporteren. |

## <a name="errors-in-the-wizard"></a>Fouten in de wizard
De installatiewizard maakt gebruik van twee andere beveiligingscontext. Op de pagina **verbinding maken met Azure AD**, het gebruik van de momenteel aangemelde gebruiker. Op de pagina **configureren**, deze wordt gewijzigd in de [account dat de service voor de synchronisatie-engine](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). Als er een probleem is, verschijnt deze waarschijnlijk al op de **verbinding maken met Azure AD** pagina in de wizard omdat de proxyconfiguratie globale.

De volgende problemen zijn de meest voorkomende fouten die in de installatiewizard optreden.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>De installatiewizard is niet correct geconfigureerd
Deze fout treedt op wanneer de wizard zelf de proxy kan bereiken.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

* Als u deze fout aanhoudt, controleert u of de [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.
* Als die er goed uitziet, volgt u de stappen in [controleren of de proxy verbinding](#verify-proxy-connectivity) om te zien als het probleem niet met de wizard ook aanwezig is.

### <a name="a-microsoft-account-is-used"></a>Een Microsoft-account wordt gebruikt
Als u een **Microsoft-account** in plaats van een **school of organisatie** account, ziet u een algemene fout.  
![Een Microsoft-Account wordt gebruikt](./media/active-directory-aadconnect-troubleshoot-connectivity/unknownerror.png)

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Het MFA-eindpunt kan niet worden bereikt
Deze fout treedt op als het eindpunt **https://secure.aadcdn.microsoftonline-p.com** kan niet worden bereikt en de globale beheerder heeft MFA ingeschakeld.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

* Als u deze fout ziet, Controleer het eindpunt **secure.aadcdn.microsoftonline p.com** is toegevoegd aan de proxy.

### <a name="the-password-cannot-be-verified"></a>Het wachtwoord kan niet worden geverifieerd.
Als de installatiewizard voltooid is in de verbinding te maken met Azure AD, maar het wachtwoord zelf kan niet worden geverifieerd dat u deze fout te zien:  
![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

* Het wachtwoord is een tijdelijk wachtwoord en moet worden gewijzigd? Is het daadwerkelijk het juiste wachtwoord? Probeer te melden bij https://login.microsoftonline.com (op een andere computer dan de Azure AD Connect-server) en controleer of dat het account kan worden gebruikt.

### <a name="verify-proxy-connectivity"></a>Controleer de proxy-connectiviteit
Om te controleren of de Azure AD Connect-server de werkelijke connectiviteit met de Proxy- en Internet heeft, moet u enkele PowerShell gebruiken om te zien als de proxy webaanvragen of niet toestaat. Voer in een PowerShell-prompt `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technisch de eerste aanroep https://login.microsoftonline.com is en deze URI werkt ook, maar de overige URI is sneller reageren.)

PowerShell gebruikt de configuratie in machine.config contact opnemen met de proxy. De instellingen van winhttp/netsh moeten geen invloed op deze cmdlets.

Als de proxy correct is geconfigureerd, krijgt u de status geslaagd: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Als u krijgt **kan geen verbinding maken met de externe server**, vervolgens PowerShell probeert te maken van een directe aanroep zonder met behulp van de proxy- of DNS is niet correct geconfigureerd. Zorg ervoor dat de **machine.config** bestand correct is geconfigureerd.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Als de proxy is niet correct geconfigureerd, krijgt u een fout opgetreden: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

| Fout | Foutbericht | Opmerking |
| --- | --- | --- |
| 403 |Is niet toegestaan |De proxy is niet geopend voor de aangevraagde URL. Terugkeren naar de proxyconfiguratie en zorg ervoor dat de [URL's](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) zijn geopend. |
| 407 |Proxyverificatie is vereist |De proxyserver vereist een aanmelden en geen taaksequencer opgegeven. Als de proxyserver verificatie vereist, zorg ervoor dat u hebt deze instelling is geconfigureerd in machine.config. Controleer ook of dat u domeinaccounts gebruikt voor de gebruiker die de wizard uitvoert, en voor het serviceaccount. |

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Het communicatiepatroon tussen Azure AD Connect en Azure AD
Als u deze voorgaande stappen hebt uitgevoerd en nog steeds geen verbinding kunt maken, u mogelijk op dit moment gaan zo kijken netwerk Logboeken. Deze sectie wordt een patroon normaal en geslaagde verbinding documenteren. Het is ook algemene rood herrings die kan worden genegeerd wanneer u de logboeken netwerk lezen aanbieding.

* Er zijn https://dc.services.visualstudio.com aanroepen. Dit is niet vereist om deze URL is geopend in de proxy voor de installatie mislukt en deze aanroepen kunnen worden genegeerd.
* U ziet de werkelijke hosts in de DNS-naam ruimte nsatc.net en andere naamruimten niet onder microsoftonline.com een lijst met DNS-omzetting. Echter, er zijn webserviceaanvragen op de namen van de werkelijke en er geen URL's toevoegen aan de proxy.
* De eindpunten adminwebservice provisioningapi detectie eindpunten en gebruikt voor het zoeken van het werkelijke eindpunt te gebruiken. Deze eindpunten zijn verschillend afhankelijk van uw regio.

### <a name="reference-proxy-logs"></a>Verwijzing proxy-Logboeken
Hier volgt een dump uit een werkelijke proxy-logboek en op de pagina van de wizard installatie van waar deze is genomen (hetzelfde eindpunt van dubbele items zijn verwijderd). Deze sectie kan worden gebruikt als referentie voor uw eigen logboeken proxy en het netwerk. De werkelijke eindpunten kunnen afwijken in uw omgeving (met name de URL's in *cursief*).

**Verbinding maken met Azure AD**

| Time | URL |
| --- | --- |
| 1/11/2016 8:31 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:31 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |verbinding: / /*bba800 anker*. microsoftonline.com:443 |
| 1/11/2016 8:32 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:33 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |verbinding: / /*bwsc02 relay*. microsoftonline.com:443 |

**Configureren**

| Time | URL |
| --- | --- |
| 1/11/2016 8:43 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:43 |verbinding: / /*bba800 anker*. microsoftonline.com:443 |
| 1/11/2016 8:43 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |verbinding: / /*bba900 anker*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |verbinding: / /*bba800 anker*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:46 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |verbinding: / /*bwsc02 relay*. microsoftonline.com:443 |

**Initiële synchronisatie**

| Time | URL |
| --- | --- |
| 1/11/2016 8:48 |Connect://login.Windows.NET:443 |
| 1/11/2016 8:49 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |verbinding: / /*bba900 anker*. microsoftonline.com:443 |
| 1/11/2016 8:49 |verbinding: / /*bba800 anker*. microsoftonline.com:443 |

## <a name="authentication-errors"></a>Verificatiefouten
Deze sectie bevat informatie over fouten die kunnen worden geretourneerd van ADAL (de verificatiebibliotheek voor Azure AD Connect gebruikt) en PowerShell. De fout uitgelegd kunt u in de volgende stappen te begrijpen.

### <a name="invalid-grant"></a>Ongeldige verlenen
Ongeldige gebruikersnaam of wachtwoord. Zie voor meer informatie [het wachtwoord kan niet worden geverifieerd](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Onbekende gebruikerstype
Uw Azure AD-directory kan niet worden gevonden of opgelost. U probeert mogelijk aanmelden met een gebruikersnaam in een niet-geverifieerd domein?

### <a name="user-realm-discovery-failed"></a>Gebruiker Realm detectie is mislukt
Netwerk- of proxyinstellingen configuratieproblemen. Het netwerk kan niet worden bereikt. Zie [oplossen van problemen met de netwerkverbinding in de installatiewizard](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Gebruikerswachtwoord verlopen
Uw referenties zijn verlopen. Uw wachtwoord wijzigen.

### <a name="authorizationfailure"></a>AuthorizationFailure
Onbekend probleem.

### <a name="authentication-cancelled"></a>Verificatie is geannuleerd
De uitdaging multi-factor authentication (MFA) is geannuleerd.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Verificatie is gelukt, maar er is een verificatieprobleem met Azure AD PowerShell.

### <a name="azurerolemissing"></a>AzureRoleMissing
Verificatie is gelukt. U bent geen hoofdbeheerder.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Verificatie is gelukt. Beschermde identiteitsbeheer is ingeschakeld en u bent momenteel niet een globale beheerder. Zie voor meer informatie [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Verificatie is gelukt. Kan de bedrijfsgegevens niet ophalen uit Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Verificatie is gelukt. Kan de domeingegevens niet ophalen uit Azure AD.

### <a name="unexpected-exception"></a>Onverwachte uitzondering
Onverwachte fout opgetreden in de installatiewizard weergegeven. Kan gebeuren als u probeert te gebruiken een **Microsoft-Account** in plaats van een **school of organisatie-account**.

## <a name="troubleshooting-steps-for-previous-releases"></a>Stappen voor probleemoplossing voor eerdere versies.
Uitgaven vanaf de aanmeldhulp build-nummer 1.1.105.0 (februari 2016 uitgebracht), buiten gebruik werd gesteld. Deze sectie en de configuratie mag niet langer zijn vereist, maar wordt opgeslagen als referentie.

Voor de één-aanmeldhulp werken, moet winhttp worden geconfigureerd. Deze configuratie kunt u doen met [ **netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>De aanmeldhulp is niet correct geconfigureerd
Deze fout treedt op wanneer de aanmeldhulp de proxy niet bereiken kan of de proxy is niet toegestaan voor de aanvraag.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

* Als u deze fout ziet, bekijkt u in de configuratie van de proxy [netsh](active-directory-aadconnect-prerequisites.md#connectivity) en controleer of deze juist is.
  ![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
* Als die er goed uitziet, volgt u de stappen in [controleren of de proxy verbinding](#verify-proxy-connectivity) om te zien als het probleem niet met de wizard ook aanwezig is.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
