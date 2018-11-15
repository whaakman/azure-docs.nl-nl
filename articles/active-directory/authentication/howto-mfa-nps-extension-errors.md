---
title: Foutcodes voor de Azure MFA NPS-extensie oplossen | Microsoft Docs
description: Hulp bij het oplossen van problemen met de NPS-extensie voor Azure multi-factor Authentication
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 3820aae1e926e51ffa88fabc94e3572b286162de
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634223"
---
# <a name="resolve-error-messages-from-the-nps-extension-for-azure-multi-factor-authentication"></a>Foutberichten van de NPS-extensie voor Azure multi-factor Authentication oplossen

Als er fouten met de NPS-extensie voor Azure multi-factor Authentication optreden, gebruikt u in dit artikel sneller een oplossing is bereikt. NPS-extensie Logboeken vindt u in Logboeken onder **aangepaste weergaven** > **serverfuncties** > **Network Policy and Access Services** op de de server waarop de NPS-extensie is geïnstalleerd.

## <a name="troubleshooting-steps-for-common-errors"></a>Stappen voor probleemoplossing voor algemene fouten

| Foutcode | Stappen voor probleemoplossing |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Neem contact op met ondersteuning voor](#contact-microsoft-support), en de lijst met stappen voor het verzamelen van Logboeken worden vermeld. Zo veel mogelijk informatie over wat is er gebeurd voordat de fout optrad, met inbegrip van de tenant-id en de principal-naam van gebruiker (UPN) kunt bieden. |
| **CLIENT_CERT_INSTALL_ERROR** | Het is mogelijk dat er een probleem met het certificaat van de client is geïnstalleerd of die zijn gekoppeld aan uw tenant. Volg de instructies in [het oplossen van de MFA NPS-extensie](howto-mfa-nps-extension.md#troubleshooting) om client cert problemen te onderzoeken. |
| **ESTS_TOKEN_ERROR** | Volg de instructies in [het oplossen van de MFA NPS-extensie](howto-mfa-nps-extension.md#troubleshooting) clientcertificaat en ADAL token u voor het onderzoeken van problemen. |
| **HTTPS_COMMUNICATION_ERROR** | De NPS-server is niet antwoorden krijgen van Azure MFA. Controleer of uw firewalls zijn open bidirectioneel gesynchroniseerd voor verkeer naar en uit https://adnotifications.windowsazure.com |
| **HTTP_CONNECT_ERROR** | Controleer of u kunt bereiken op de server waarop de NPS-extensie wordt uitgevoerd, https://adnotifications.windowsazure.com en https://login.microsoftonline.com/. Als deze sites niet laden, controleert u de verbinding op die server. |
| **NPS-extensie voor Azure MFA:** <br> NPS-extensie voor Azure MFA voert alleen uit voor secundaire verificatie voor RADIUS-aanvragen in AccessAccept staat. Aanvraag voor de gebruikersnaam van gebruiker met de reactiestatus AccessReject, negeert aanvraag ontvangen. | Deze fout geeft meestal een verificatiefout opgetreden in AD of dat de NPS-server kan geen antwoorden krijgen van Azure AD. Controleren of uw firewalls openen bidirectioneel gesynchroniseerd voor verkeer naar en van https://adnotifications.windowsazure.com en https://login.microsoftonline.com met behulp van de poorten 80 en 443. Het is ook belangrijk om te controleren dat op het tabblad DIAL-IN van de toegangsmachtigingen netwerk de instelling is ingesteld op 'toegang via NPS-netwerkbeleid beheren'. |
| **REGISTRY_CONFIG_ERROR** | Er ontbreekt een sleutel in het register voor de toepassing, die mogelijk doordat de [PowerShell-script](howto-mfa-nps-extension.md#install-the-nps-extension) na de installatie is niet uitgevoerd. Het foutbericht moet de ontbrekende sleutel bevatten. Zorg ervoor dat u hebt de sleutel onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa. |
| **REQUEST_FORMAT_ERROR** <br> RADIUS-aanvraag verplichte userName\Identifier Radius-kenmerk ontbreekt. Controleer of dat NPS RADIUS-aanvragen is ontvangen | Deze fout geeft meestal een probleem met installatie. De NPS-extensie moet zijn geïnstalleerd in de NPS-servers die de RADIUS-aanvragen kunnen ontvangen. NPS-servers die zijn geïnstalleerd als de afhankelijkheden voor services zoals RDG- en RRAS geen radius-aanvragen ontvangen. NPS-extensie werkt niet als geïnstalleerd via deze-installaties en -fouten af omdat dit kan de details van de verificatieaanvraag niet lezen. |
| **REQUEST_MISSING_CODE** | Zorg ervoor dat de wachtwoord-versleutelingsprotocol tussen de NPS- en NAS-servers biedt ondersteuning voor de secundaire verificatiemethode die u gebruikt. **PAP** biedt ondersteuning voor alle verificatiemethoden van Azure MFA in de cloud: telefonische oproep, eenzijdige SMS-bericht, mobiele app-meldingen en verificatiecode via mobiele app. **CHAPv2** en **EAP** ondersteuning bellen en mobiele app-meldingen. |
| **USERNAME_CANONICALIZATION_ERROR** | Controleer of de gebruiker aanwezig zijn in uw on-premises Active Directory-exemplaar, en dat de NPS-Service beschikt over machtigingen voor toegang tot de map. Als u vertrouwensrelaties tussen forests, [contact op met ondersteuning](#contact-microsoft-support) voor verdere assistentie. |

### <a name="alternate-login-id-errors"></a>Alternatieve aanmeldings-ID-fouten

| Foutcode | Foutbericht | Stappen voor probleemoplossing |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Fout: userObjectSid-zoekactie is mislukt | Controleer of de gebruiker bestaat in uw on-premises Active Directory-exemplaar. Als u vertrouwensrelaties tussen forests, [contact op met ondersteuning](#contact-microsoft-support) voor verdere assistentie. |
| **ALTERNATE_LOGIN_ID_ERROR** | Fout: Alternatieve LoginId-zoekactie is mislukt | Controleren of LDAP_ALTERNATE_LOGINID_ATTRIBUTE is ingesteld op een [geldig active directory-kenmerk](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> Als LDAP_FORCE_GLOBAL_CATALOG is ingesteld op True of LDAP_LOOKUP_FORESTS is geconfigureerd met een niet-lege waarde, verifieert u dat u een globale catalogus hebt geconfigureerd en dat het kenmerk AlternateLoginId wordt toegevoegd aan het. <br><br> Als LDAP_LOOKUP_FORESTS is geconfigureerd met een niet-lege waarde, moet u controleren of de waarde juist is. Als er meer dan één naam van het forest, moeten de namen worden gescheiden door puntkomma's, geen spaties. <br><br> Als deze stappen het probleem niet oplost [contact op met ondersteuning](#contact-microsoft-support) voor meer informatie. |
| **ALTERNATE_LOGIN_ID_ERROR** | Fout: Alternatieve LoginId waarde is leeg | Controleren of het kenmerk AlternateLoginId is geconfigureerd voor de gebruiker. |

## <a name="errors-your-users-may-encounter"></a>Fouten met uw gebruikers optreden kunnen

| Foutcode | Foutbericht | Stappen voor probleemoplossing |
| ---------- | ------------- | --------------------- |
| **AccessDenied** | Tenant van de oproepende functie heeft geen machtigingen voor toegang tot het doen van verificatie voor de gebruiker | Controleer of het domein van de tenant en het domein van de UPN (User Principal Name) hetzelfde zijn. Bijvoorbeeld, zorg ervoor dat user@contoso.com probeert te verifiëren bij de Contoso-tenant. De UPN is een geldige gebruiker voor de tenant in Azure. |
| **AuthenticationMethodNotConfigured** | De opgegeven verificatiemethode is niet geconfigureerd voor de gebruiker | Laat de gebruiker toevoegen of controleren van de verificatiemethoden op basis van de instructies in [uw instellingen beheren voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Opgegeven verificatiemethode wordt niet ondersteund. | Alle logboeken die deze fout te verzamelen en [contact op met ondersteuning](#contact-microsoft-support). Wanneer u contact op met ondersteuning, geef de gebruikersnaam en de secundaire verificatiemethode die de fout heeft veroorzaakt. |
| **BecAccessDenied** | Aanroep van MSODS Bee geretourneerd toegang is geweigerd, de gebruikersnaam is waarschijnlijk niet gedefinieerd in de tenant | De gebruiker aanwezig is in on-premises Active Directory, maar is niet gesynchroniseerd naar Azure AD door AD Connect. Of de gebruiker is niet gevonden voor de tenant. De gebruiker toevoegen aan Azure AD en toe te voegen van de verificatiemethoden op basis van de instructies in [uw instellingen beheren voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** of **StrongAuthenticationServiceInvalidParameter** | Het telefoonnummer heeft een onherkenbare indeling | Laat de gebruiker die verhelpen hun telefoonnummers verificatie. |
| **InvalidSession** | De opgegeven sessie is ongeldig of verlopen | De sessie heeft genomen meer dan drie minuten om te voltooien. Controleer of de gebruiker is de verificatiecode invoeren, of reageert op de app-melding, binnen drie minuten na het starten van de verificatieaanvraag. Als het probleem daarmee niet wordt verholpen, controleert u dat er geen netwerkvertragingen tussen de client, NAS-Server, NPS-Server en het eindpunt van de Azure MFA zijn.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Er is geen standaardmethode voor verificatie is geconfigureerd voor de gebruiker | Laat de gebruiker toevoegen of controleren van de verificatiemethoden op basis van de instructies in [uw instellingen beheren voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-manage-settings.md). Controleer of dat de gebruiker heeft ervoor een standaardmethode voor verificatie gekozen en geconfigureerd die methode voor hun rekening. |
| **OathCodePinIncorrect** | Onjuiste code en pincode ingevoerd. | Deze fout wordt niet verwacht in de NPS-extensie. Als de gebruiker dit [contact op met ondersteuning](#contact-microsoft-support) voor oplossingen voor problemen. |
| **ProofDataNotFound** | Bewijs gegevens is niet geconfigureerd voor de opgegeven verificatiemethode. | Laat de gebruiker probeer een andere verificatiemethode of toevoegen van een nieuwe verificatiemethoden op basis van de instructies in [uw instellingen beheren voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-manage-settings.md). Als de gebruiker heeft nog steeds deze fout ziet nadat u hebt bevestigd dat de verificatiemethode correct is ingesteld [contact op met ondersteuning](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Onjuiste code en pincode ingevoerd. (OneWaySMS) | Deze fout wordt niet verwacht in de NPS-extensie. Als de gebruiker dit [contact op met ondersteuning](#contact-microsoft-support) voor oplossingen voor problemen. |
| **TenantIsBlocked** | Tenant is geblokkeerd | [Neem contact op met ondersteuning voor](#contact-microsoft-support) met Directory-ID van de eigenschappenpagina van de Azure AD in Azure portal. |
| **UserNotFound** | De opgegeven gebruiker is niet gevonden | De tenant is niet langer zichtbaar als actief in Azure AD. Controleer of uw abonnement actief is en u de vereiste hebt eigen apps. Controleer ook of de tenant in het certificaatonderwerp is zoals verwacht en het certificaat is nog steeds geldig en geregistreerd onder de service-principal. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Berichten die uw gebruikers kunnen ondervinden die niet zijn fouten

Uw gebruikers kunnen soms berichten ophalen van multi-factor Authentication omdat de verificatieaanvraag is mislukt. Deze fouten in het product van de configuratie niet, maar zijn opzettelijke waarschuwingen waarin wordt uitgelegd waarom een verificatieaanvraag is geweigerd.

| Foutcode | Foutbericht | Aanbevolen stappen | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Verkeerde code entered\OATH Code onjuist | Geen fout, de gebruiker heeft verkeerde code ingevoerd. | De gebruiker heeft de verkeerde code ingevoerd. Vraagt u deze opnieuw om door een nieuwe code aanvragen of meldt u zich opnieuw. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | Maximale toegestane code opnieuw proberen is bereikt | De gebruiker is te vaak mislukt de verificatie-uitdaging. Afhankelijk van uw instellingen, ze mogelijk moeten worden geblokkeerd door een beheerder nu.  |
| **SMSAuthFailedWrongCodeEntered** | Onjuist code ingevoerd/tekst bericht OTP onjuist | De gebruiker heeft de verkeerde code ingevoerd. Vraagt u deze opnieuw om door een nieuwe code aanvragen of meldt u zich opnieuw. |

## <a name="errors-that-require-support"></a>Fouten die ondersteuning vereisen

Als u een van deze fouten ondervindt, raden we u [contact op met ondersteuning](#contact-microsoft-support) voor diagnostische informatie. Er is geen standaard ingesteld met de stappen die in deze fouten voorzien kan. Wanneer u Neem contact op met ondersteuning, zorg ervoor dat moeten worden opgenomen als veel informatie mogelijk over de stappen die hebben geleid tot een fout en de tenantgegevens van uw.

| Foutcode | Foutbericht |
| ---------- | ------------- |
| **InvalidParameter** | Aanvraag mag niet null zijn |
| **InvalidParameter** | Object-id mag geen null of leeg zijn voor ReplicationScope:{0} |
| **InvalidParameter** | De lengte van CompanyName \{0} \ langer is dan de maximaal toegestane lengte {1} |
| **InvalidParameter** | UserPrincipalName moet niet null of leeg zijn |
| **InvalidParameter** | De opgegeven tenant-id is niet in de juiste indeling |
| **InvalidParameter** | Sessie-id mag geen null of leeg zijn |
| **InvalidParameter** | Kan ProofData van aanvraag of Msods niet omzetten. De ProofData is onbekend |
| **Activiteits-id** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Volgende stappen

### <a name="troubleshoot-user-accounts"></a>Accounts van gebruikers oplossen

Als uw gebruikers zich [problemen hebt met verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-troubleshoot.md), help ze zelf diagnose problemen.

### <a name="contact-microsoft-support"></a>Contact opnemen met Microsoft Ondersteuning

Als u meer hulp nodig hebt, neem dan contact op met een ondersteuningsmedewerker via [ondersteuning voor Azure multi-factor Authentication-Server](https://support.microsoft.com/oas/default.aspx?prid=14947). Wanneer u contact met ons opneemt, is het handig als u zo veel mogelijk informatie over uw probleem mogelijk kunt opnemen. Informatie u opgeven kunt, inclusief de pagina waar u het foutbericht, de specifieke foutcode, gezien de specifieke sessie-ID, de ID van de gebruiker die de fout hebt gezien en fouten opsporen in Logboeken.

Voor het verzamelen van Logboeken voor foutopsporing voor ondersteuning van diagnostische gegevens, gebruikt u de volgende stappen uit op de NPS-server:

1. Open de Register-Editor en blader naar HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa set **VERBOSE_LOG** naar **TRUE**
2. Open een opdrachtprompt als Administrator en voer deze opdrachten uit:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

3. Het probleem te reproduceren

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
6. De inhoud van de map C:\NPS ZIP en het ZIP-bestand koppelen aan de ondersteuningsaanvraag.
