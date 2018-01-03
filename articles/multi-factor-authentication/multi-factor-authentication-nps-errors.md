---
title: Foutcodes voor de extensie Azure MFA NPS oplossen | Microsoft Docs
description: Hulp bij het oplossen van problemen met de NPS-extensie voor Azure multi-factor Authentication met specifieke oplossingen voor algemene foutberichten
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 7960a398ac25ad0192300632dd6d5add94fd4a7c
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="resolve-error-messages-from-the-nps-extension-for-azure-multi-factor-authentication"></a>Foutberichten van de NPS-extensie voor Azure multi-factor Authentication oplossen

Als u fouten met de extensie NPS voor Azure multi-factor Authentication optreden, gebruik dit artikel proberen op te lossen sneller. 

## <a name="troubleshooting-steps-for-common-errors"></a>Probleemoplossing voor veelvoorkomende fouten

| Foutcode | Stappen voor het oplossen van problemen |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Neem contact op met ondersteuning](#contact-microsoft-support), en vermeld in de lijst met stappen voor het verzamelen van Logboeken. Geef zoveel mogelijk gegevens over wat er gebeurd voordat de fout is, met inbegrip van tenant-id en de UPN (user Principal name). |
| **CLIENT_CERT_INSTALL_ERROR** | Mogelijk zijn er een probleem met het certificaat van de client is geïnstalleerd of die zijn gekoppeld aan uw tenant. Volg de instructies in [probleemoplossing van de extensie MFA NPS](multi-factor-authentication-nps-extension.md#troubleshooting) om client cert problemen te onderzoeken. |
| **ESTS_TOKEN_ERROR** | Volg de instructies in [probleemoplossing van de extensie MFA NPS](multi-factor-authentication-nps-extension.md#troubleshooting) client cert en ADAL token u voor het onderzoeken van problemen. |
| **HTTPS_COMMUNICATION_ERROR** | De NPS-server is niet ontvangen van reacties van Azure MFA. Controleer of de firewalls geopend bidirectioneel voor verkeer van en naar https://adnotifications.windowsazure.com zijn |
| **HTTP_CONNECT_ERROR** | Op de server waarop de NPS-extensie wordt uitgevoerd, controleert u of u https://adnotifications.windowsazure.com en https://login.microsoftonline.com/ kunt bereiken. Als deze sites niet laden, controleert u de verbinding op die server. |
| **REGISTRY_CONFIG_ERROR** | Er ontbreekt een sleutel in het register voor de toepassing, wat mogelijk is de [PowerShell-script](multi-factor-authentication-nps-extension.md#install-the-nps-extension) na de installatie is niet uitgevoerd. Het foutbericht moet de ontbrekende sleutel bevatten. Zorg ervoor dat u beschikt over de sleutel onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa. |
| **REQUEST_FORMAT_ERROR** <br> RADIUS-aanvraag ontbreken verplichte userName\Identifier Radius-kenmerk. Controleren of NPS RADIUS-aanvragen wordt ontvangen | Deze fout duidt normaal gesproken een installatieprobleem. De NPS-uitbreiding moet worden geïnstalleerd in de NPS-servers die RADIUS-aanvragen kunnen ontvangen. NPS-servers die zijn geïnstalleerd als de afhankelijkheden voor services zoals RDG en RRAS geen radius-aanvragen worden ontvangen. Uitbreiding van NPS werkt niet als geïnstalleerd via dergelijke installaties en -fouten uit omdat de details van de authenticatie-aanvraag kan niet worden gelezen. |
| **REQUEST_MISSING_CODE** | Zorg ervoor dat het wachtwoord versleutelingsprotocol tussen de NPS- en NAS-servers de secundaire verificatie-methode die u ondersteunt. **PAP** ondersteunt de verificatiemethoden van Azure MFA in de cloud: telefoonoproep, eenzijdige SMS-bericht, mobiele-appmelding en verificatiecode mobiele app. **CHAPv2** en **EAP** telefoongesprek en mobiele-appmelding ondersteunen. |
| **USERNAME_CANONICALIZATION_ERROR** | Verifieer dat de gebruiker aanwezig is in uw lokale Active Directory-exemplaar en of de NPS-Service is gemachtigd om toegang tot de map. Als u de vertrouwensrelaties tussen forests, [contact op met ondersteuning](#contact-microsoft-support) voor verdere assistentie. |


   

### <a name="alternate-login-id-errors"></a>Alternatieve aanmeldings-ID-fouten

| Foutcode | Bericht invoeren | Stappen voor het oplossen van problemen |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Fout: userObjectSid opzoeken is mislukt | Controleer of de gebruiker bestaat in uw lokale Active Directory-exemplaar. Als u de vertrouwensrelaties tussen forests, [contact op met ondersteuning](#contact-microsoft-support) voor verdere assistentie. |
| **ALTERNATE_LOGIN_ID_ERROR** | Fout: Er is een alternatieve LoginId lookup mislukt | Controleren of LDAP_ALTERNATE_LOGINID_ATTRIBUTE is ingesteld op een [geldig active directory-kenmerk](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> Als LDAP_FORCE_GLOBAL_CATALOG is ingesteld op True of LDAP_LOOKUP_FORESTS is geconfigureerd met een niet-lege waarde, controleert u of dat u een globale catalogus hebt geconfigureerd en dat het kenmerk AlternateLoginId hieraan toegevoegd. <br><br> Als LDAP_LOOKUP_FORESTS is geconfigureerd met een niet-lege waarde, moet u controleren of de waarde juist is. Als er meer dan één naam van het forest, moeten de namen worden gescheiden met puntkomma's, geen spaties. <br><br> Als deze stappen het probleem niet oplost [contact op met ondersteuning](#contact-microsoft-support) voor meer informatie. |
| **ALTERNATE_LOGIN_ID_ERROR** | Fout: Alternatieve LoginId waarde is leeg | Controleren of het kenmerk AlternateLoginId is geconfigureerd voor de gebruiker. |


## <a name="errors-your-users-may-encounter"></a>Uw gebruikers kunnen ondervinden fouten

| Foutcode | Bericht invoeren | Stappen voor het oplossen van problemen |
| ---------- | ------------- | --------------------- |
| **AccessDenied** | Tenant van de aanroeper heeft geen machtigingen voor toegang tot Authentication uitvoeren voor de gebruiker | Controleer of de tenant en het domein van de UPN (user Principal name) hetzelfde zijn. Bijvoorbeeld, zorg ervoor dat user@contoso.com is proberen te verifiëren bij de Contoso-tenant. De UPN vertegenwoordigt een geldige gebruiker voor de tenant in Azure. |
| **AuthenticationMethodNotConfigured** | De opgegeven verificatiemethode is niet geconfigureerd voor de gebruiker | Een gebruiker toevoegen of controleren van hun verificatiemethoden volgens de instructies in [beheren van uw instellingen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Opgegeven verificatiemethode wordt niet ondersteund. | Alle logboeken met deze fout te verzamelen en [contact op met ondersteuning](#contact-microsoft-support). Wanneer u contact op met ondersteuning, geef de gebruikersnaam en de secundaire verificatiemethode die de fout heeft veroorzaakt. |
| **BecAccessDenied** | MSODS Bec aanroep heeft geretourneerd van de toegang is geweigerd, de gebruikersnaam is waarschijnlijk niet gedefinieerd in de tenant | De gebruiker aanwezig is in Active Directory lokale maar is niet gesynchroniseerd naar Azure AD door AD Connect. Of de gebruiker ontbreekt voor de tenant. De gebruiker toevoegen aan Azure AD en toe te voegen hun verificatiemethoden volgens de instructies in [beheren van uw instellingen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** of **StrongAuthenticationServiceInvalidParameter** | Het telefoonnummer is in een niet-herkende indeling | Hebben de gebruikers hun telefoonnummers verificatie te corrigeren. |
| **InvalidSession** | De opgegeven sessie is ongeldig of verlopen | De sessie heeft genomen meer dan drie minuten duren. Controleer of dat de gebruiker de verificatiecode invoeren of reageert niet op de appmelding binnen drie minuten na het initiëren van de verificatieaanvraag. Als het probleem niet wordt verholpen, Controleer of er geen netwerkvertragingen tussen de client, NAS-Server, NPS-Server en de Azure MFA-eindpunt.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Er is geen standaardverificatiemethode is geconfigureerd voor de gebruiker | Een gebruiker toevoegen of controleren van hun verificatiemethoden volgens de instructies in [beheren van uw instellingen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md). Controleer of dat de gebruiker heeft ervoor een standaardmethode voor verificatie gekozen en geconfigureerd die methode voor hun rekening. |
| **OathCodePinIncorrect** | Onjuiste code en pincode ingevoerd. | Deze fout wordt niet verwacht in de NPS-uitbreiding. Als de gebruiker tegenkomt, [contact op met ondersteuning](#contact-microsoft-support) voor hulp bij probleemoplossing. |
| **ProofDataNotFound** | Bewijs gegevens is niet geconfigureerd voor de opgegeven verificatiemethode. | Een gebruiker probeert een andere verificatiemethode of Voeg een nieuwe verificatiemethoden volgens de instructies in [beheren van uw instellingen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md). Als de gebruiker deze fout te zien blijft als u bevestigd dat hun verificatiemethode correct is ingesteld [contact op met ondersteuning](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Onjuiste code en pincode ingevoerd. (OneWaySMS) | Deze fout wordt niet verwacht in de NPS-uitbreiding. Als de gebruiker tegenkomt, [contact op met ondersteuning](#contact-microsoft-support) voor hulp bij probleemoplossing. |
| **TenantIsBlocked** | Tenant is geblokkeerd | [Neem contact op met ondersteuning](#contact-microsoft-support) met map-ID van de eigenschappenpagina van Azure AD in de Azure portal. |
| **UserNotFound** | De opgegeven gebruiker is niet gevonden. | De tenant is niet langer zichtbaar als actief in Azure AD. Controleer of uw abonnement actief is en u de vereiste hebt eigen apps. Controleer ook of de tenant in het certificaatonderwerp zoals verwacht en het certificaat is nog steeds geldig en geregistreerd onder de service-principal. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Berichten die uw gebruikers kunnen ondervinden die zich niet fouten

Uw gebruikers kunnen soms berichten ophalen van multi-factor Authentication omdat hun authenticatie-aanvraag is mislukt. Deze fouten in het product van de configuratie niet, maar zijn opzettelijk waarschuwingen waarin wordt uitgelegd waarom een verificatieaanvraag is geweigerd.

| Foutcode | Bericht invoeren | Aanbevolen stappen | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Onjuiste code entered\OATH Code is onjuist | Niet door een fout gebruiker heeft onjuiste code ingevoerd. | De gebruiker heeft onjuiste code ingevoerd. Hebben ze opnieuw proberen door een nieuwe code aanvragen of meldt u zich opnieuw. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | Maximale toegestane code opnieuw proberen is bereikt | De gebruiker is het lastig voor de verificatie te vaak mislukt. Afhankelijk van uw instellingen, kunnen ze moeten naar nu door een beheerder worden gedeblokkeerd.  |
| **SMSAuthFailedWrongCodeEntered** | Code verkeerd ingevoerde/tekst bericht OTP onjuist | De gebruiker heeft onjuiste code ingevoerd. Hebben ze opnieuw proberen door een nieuwe code aanvragen of meldt u zich opnieuw. |

## <a name="errors-that-require-support"></a>Fouten die ondersteuning nodig hebt

Als u een van deze fouten optreden, raden we u [contact op met ondersteuning](#contact-microsoft-support) voor diagnostische informatie. Er is geen standaard ingesteld van de stappen die deze fouten kunt oplossen. Wanneer u Neem contact op met ondersteuning, zorg er dan voor dat opnemen als veel mogelijke informatie over de stappen die hebben geleid tot een fout en uw tenant.

| Foutcode | Bericht invoeren |
| ---------- | ------------- |
| **InvalidParameter** | Aanvraag mag niet null zijn |
| **InvalidParameter** | Object-id mag geen null of leeg zijn voor ReplicationScope: {0} |
| **InvalidParameter** | De lengte van NaamBedrijf \{0} \ langer is dan de maximaal toegestane lengte {1} |
| **InvalidParameter** | UserPrincipalName moet niet null of leeg zijn |
| **InvalidParameter** | De opgegeven TenantId bevindt zich niet in de juiste indeling |
| **InvalidParameter** | Sessie-id mag geen null of leeg zijn |
| **InvalidParameter** | Kan ProofData van aanvraag of Msods niet omzetten. De ProofData is onbekend |
| **InternalError** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Volgende stappen

### <a name="troubleshoot-user-accounts"></a>Gebruikersaccounts oplossen

Als uw gebruikers zijn [problemen hebt met verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-troubleshoot.md), helpen ze problemen diagnosticeren zelf. 

### <a name="contact-microsoft-support"></a>Neem contact op met Microsoft ondersteuning

Als u meer hulp nodig hebt, neem dan contact op met een supportmedewerker via [ondersteuning van Azure multi-factor Authentication-Server](https://support.microsoft.com/oas/default.aspx?prid=14947). Wanneer u contact met ons, is het handig als u zo veel mogelijk gegevens over uw probleem mogelijk kunt opnemen. Informatie die u kunt opgeven bevat de pagina waar u de fout, de specifieke foutcode hebt gezien de specifieke sessie-ID, de ID van de gebruiker die de fout hebt gezien en logboeken voor foutopsporing.

Gebruik de volgende stappen uit op de NPS-server voor het verzamelen van Logboeken voor foutopsporing voor ondersteuning van diagnostische gegevens:

1. Open de Register-Editor en blader naar HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa set **VERBOSE_LOG** naar **TRUE**
2. Open een opdrachtprompt als Administrator en voer deze opdrachten uit:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

3. Reproduceer het probleem

4. Stop de tracering met de volgende opdrachten:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

5. Open de Register-Editor en blader naar HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa set **VERBOSE_LOG** naar **FALSE**
6. De inhoud van de map C:\NPS ZIP en koppelt u het ZIP-bestand aan de ondersteuningsaanvraag.


