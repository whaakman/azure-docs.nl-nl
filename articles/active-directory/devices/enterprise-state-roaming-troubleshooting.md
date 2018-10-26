---
title: Enterprise State Roaming-instellingen in Azure Active Directory oplossen | Microsoft Docs
description: Vindt u antwoorden op enkele vragen die IT-beheerders mogelijk over de instellingen en app-gegevens synchroniseren.
services: active-directory
keywords: Enterprise state roaming-instellingen, windows-cloud, veelgestelde vragen over enterprise state roaming
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: f45d0515-99f7-42ad-94d8-307bc0d07be5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: markvi
ms.reviewer: tanning
ms.custom: it-pro
ms.openlocfilehash: ef10f45cbe02bd364563be4b0d62f9c491848452
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50097868"
---
# <a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Enterprise State Roaming-instellingen in Azure Active Directory oplossen

Dit onderwerp bevat informatie over het oplossen en diagnosticeren van problemen met het Enterprise State Roaming, en geeft een lijst van bekende problemen.

## <a name="preliminary-steps-for-troubleshooting"></a>Voorbereidende stappen voor probleemoplossing 

Voordat u begint met het oplossen van problemen, Controleer of dat de gebruiker en apparaat correct hebt geconfigureerd en dat aan alle vereisten van het Enterprise State Roaming wordt voldaan door het apparaat en de gebruiker. 

1. Windows 10, is met de meest recente updates en een minimale versie 1511 (OS Build 10586 of hoger) geïnstalleerd op het apparaat. 
1. Het apparaat is Azure AD een domein of toegevoegd aan hybrid Azure AD. Zie voor meer informatie, [over het verkrijgen van een apparaat onder het beheer van Azure AD](overview.md).
1. Zorg ervoor dat **Enterprise State Roaming** is ingeschakeld voor de tenant in Azure AD, zoals beschreven in [om in te schakelen Enterprise State Roaming](enterprise-state-roaming-enable.md). U kunt inschakelen voor alle gebruikers of voor een geselecteerde groep gebruikers voor roaming.
1. De gebruiker moet al een Azure Active Directory Premium-licentie worden toegewezen.  
1. Het apparaat moet opnieuw worden opgestart en de gebruiker moet opnieuw aanmelden voor toegang tot functies Enterprise State Roaming.

## <a name="information-to-include-when-you-need-help"></a>Gegevens op te nemen wanneer u hulp nodig hebt
Als u uw probleem met de onderstaande richtlijnen niet kan oplossen, kunt u contact op met onze ondersteuningsmedewerkers. Wanneer u contact met hen opnemen, zijn onder andere de volgende informatie:

* **Algemene beschrijving van de fout**: zijn er foutberichten worden gezien door de gebruiker? Als er geen foutbericht is, wordt het onverwachte gedrag, in detail gezien beschreven. Welke functies zijn ingeschakeld voor synchronisatie en wat is de gebruiker om te synchroniseren wordt verwacht? Worden meerdere functies niet is gesynchroniseerd of wordt deze aan een geïsoleerd?
* **Betrokken gebruikers** – Is sync werken/mislukte voor één gebruiker of meerdere gebruikers? Het aantal apparaten per gebruiker zijn betrokken? Zijn al deze niet gesynchroniseerd of zijn sommige van deze synchroniseren en sommige niet synchroniseren?
* **Informatie over de gebruiker** : welke identiteit van de gebruiker die zich aanmeldt bij het apparaat is? Hoe wordt de gebruiker aanmelden bij het apparaat? Zijn ze deel uitmaken van een geselecteerde beveiligingsgroep kan synchroniseren? 
* **Informatie over het apparaat** – dit apparaat Azure AD join of domein? Welke versie is het apparaat op? Wat zijn de meest recente updates?
- **Datum / tijd / tijdzone** : Wat is de exacte datum en tijd die u hebt gezien de fout (inclusief de tijdzone)?

Deze gegevens kan we het probleem zo snel mogelijk kunt oplossen.

## <a name="troubleshooting-and-diagnosing-issues"></a>Het oplossen en diagnosticeren van problemen
In deze sectie bevat suggesties over het oplossen en diagnosticeren van problemen met betrekking tot het Enterprise State Roaming.

## <a name="verify-sync-and-the-sync-your-settings-settings-page"></a>Controleer of de synchronisatie en de instellingenpagina "Uw instellingen synchroniseren" 

1. Nadat u uw Windows 10-pc's toevoegen aan een domein dat is geconfigureerd voor het Enterprise State Roaming toestaan, moet u zich aanmelden met uw werkaccount. Ga naar **instellingen** > **Accounts** > **uw synchronisatie-instellingen** en controleer of synchronisatie en de afzonderlijke instellingen op, en dat de bovenkant van de instellingenpagina geeft aan dat u met uw werkaccount synchroniseert. Controleer of hetzelfde account wordt ook gebruikt als uw account voor aanmelding in **instellingen** > **Accounts** > **je Info**. 
1. Controleer of synchroniseren tussen meerdere computers werkt door enkele wijzigingen doorgevoerd op de oorspronkelijke computer, zoals het verplaatsen van de taakbalk op de bovenste of rechts van het scherm. Bekijk de wijziging wordt doorgegeven aan de tweede machine binnen vijf minuten. 

  * Vergrendelen en ontgrendelen van het scherm (Win + L) kan helpen bij het activeren van een synchronisatie.
  * U moet aanmelden met hetzelfde account op beide computers voor synchronisatie moet werken, zoals Enterprise State Roaming is gekoppeld aan het gebruikersaccount en niet het computeraccount.

**Potentiële problemen**: als de besturingselementen in de **instellingen** pagina zijn niet beschikbaar en u ziet het bericht "Sommige Windows-functies zijn alleen beschikbaar als u een Microsoft-account of werkaccount." Dit probleem kan optreden voor apparaten die zijn ingesteld op domein en geregistreerd bij Azure AD, maar het apparaat is niet nog is geverifieerd bij Azure AD. Een mogelijke oorzaak is dat het apparaatbeleid moet worden toegepast, maar deze toepassing verloopt asynchroon en door een paar uur kan worden vertraagd. 

### <a name="verify-the-device-registration-status"></a>Status van de apparaatregistratie controleren

Enterprise State Roaming vereist dat het apparaat worden geregistreerd bij Azure AD. Hoewel niet specifiek zijn voor Enterprise State Roaming, volgens de onderstaande instructies kunt bevestigen dat de Windows 10-Client is geregistreerd, en doet dit door de vingerafdruk, URL voor Azure AD-instellingen, NGC, en andere informatie.

1.  Open de opdrachtprompt verlaagde bevoegdheden. Om dit te doen in Windows, opent u het uitvoeren starten (Win + R) en typ "cmd" om te openen.
2.  Zodra de opdrachtprompt geopend is, typt u "*dsregcmd.exe status*'.
3.  Voor de verwachte uitvoer, de **AzureAdJoined** veldwaarde moet 'Ja', **WamDefaultSet** veldwaarde moet 'Ja', en de **WamDefaultGUID** veldwaarde moet een GUID met '(AzureAd)' aan het einde.

**Potentiële problemen**: **WamDefaultSet** en **AzureAdJoined** zowel "Nee" hebben in de veldwaarde, het apparaat is domein en geregistreerd bij Azure AD en het apparaat wordt niet gesynchroniseerd. Als het dit wordt weergegeven, wordt het apparaat moet mogelijk na afloop van beleid moet worden toegepast of de verificatie voor het apparaat is mislukt bij het verbinden met Azure AD. De gebruiker mogelijk Wacht een paar uur voor het beleid moet worden toegepast. Andere stappen voor probleemoplossing, omvat mogelijk automatische registratie door afmelden en weer opnieuw of start de taak in Task Scheduler. In sommige gevallen wordt uitgevoerd '*dsregcmd.exe /leave*' in het venster opdrachtprompt met verhoogde bevoegdheid opnieuw wordt opgestart, en probeer het opnieuw registreren bij dit probleem kunnen helpen.


**Potentiële problemen**: het veld voor **SettingsUrl** leeg is en het apparaat wordt niet gesynchroniseerd. De gebruiker kan hebben laatst vastgelegd in op het apparaat voordat Enterprise State Roaming is ingeschakeld in de Azure Active Directory-Portal. Het apparaat opnieuw starten en de gebruikersaanmelding hebben. Probeer (optioneel) dat de IT-beheerder uitschakelen en opnieuw inschakelen van gebruikers kunnen synchronisatie-instellingen en Enterprise-App-gegevens in de portal. Eenmaal opnieuw ingeschakeld, start het apparaat opnieuw en de gebruikersaanmelding hebben. Als het probleem hiermee niet is opgelost **SettingsUrl** mag niet leeg zijn in het geval van een certificaat voor beschadigde apparaten. In dit geval wordt uitgevoerd '*dsregcmd.exe /leave*' in het venster opdrachtprompt met verhoogde bevoegdheid opnieuw wordt opgestart, en probeer het opnieuw registreren bij dit probleem kunnen helpen.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Enterprise State Roaming en meervoudige verificatie 

Onder bepaalde omstandigheden, kan Enterprise State Roaming niet kunnen synchroniseren van gegevens als Azure multi-factor Authentication is geconfigureerd. Raadpleeg het ondersteuningsdocument over voor meer informatie over deze problemen [KB3193683](https://support.microsoft.com/kb/3193683). 

**Potentiële problemen**: als uw apparaat is geconfigureerd voor meervoudige verificatie vereisen in de Azure Active Directory-portal, schakelt u mogelijk naar instellingen synchroniseren tijdens het aanmelden bij een Windows 10-apparaat met een wachtwoord. Dit type configuratie van multi-factor Authentication is bedoeld voor het beveiligen van een Azure-beheerdersaccount. Beheerder gebruikers nog steeds mogelijk om te synchroniseren met het aanmelden bij hun Windows 10-apparaten met hun Microsoft Passport voor Work PINCODE of door te voeren van multi-factor Authentication bij het openen van andere Azure-services zoals Office 365.

**Potentiële problemen**: synchronisatie kan mislukken als de beheerder het voorwaardelijke toegangsbeleid van Active Directory Federation Services multi-factor Authentication configureert en het toegangstoken op het apparaat is verlopen. Zorg ervoor dat u zich aanmelden en meld u af met de Microsoft Passport for Work-PINCODE of multi-Factor Authentication voltooid tijdens het openen van andere Azure-services zoals Office 365.

### <a name="event-viewer"></a>Logboeken

Voor geavanceerde probleemoplossing, kan de logboeken worden gebruikt om fouten gevonden. Deze worden beschreven in de onderstaande tabel. De gebeurtenissen kunnen u vinden onder Logboeken > Logboeken toepassingen en Services > **Microsoft** > **Windows** > **SettingSync Azure** en voor identiteit-problemen met synchronisatie **Microsoft** > **Windows** > **AAD**.

## <a name="known-issues"></a>Bekende problemen

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Synchronisatie werkt niet op apparaten met apps ' side-loaded met behulp van MDM-software

Is van invloed op apparaten met Windows 10 Jubileumupdate (versie 1607). In Logboeken onder de SettingSync-Azure-Logboeken, is vaak de gebeurtenis-ID 6013 met fout 80070259 gezien.

**Aanbevolen actie**  
Zorg ervoor dat de client van Windows 10 v1607 heeft 23 augustus 2016 cumulatieve Update ([KB3176934](https://support.microsoft.com/kb/3176934) OS Build 14393.82). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Favorieten in Internet Explorer worden niet gesynchroniseerd

Is van invloed op apparaten met Windows 10 November Update (versie 1511).

**Aanbevolen actie**  
Zorg ervoor dat de Windows 10 v1511-client heeft de juli 2016 cumulatieve Update ([KB3172985](https://support.microsoft.com/kb/3172985) OS Build 10586.494).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Thema wordt niet gesynchroniseerd, en de gegevens die worden beschermd met Windows Information Protection 

Om te voorkomen dat gegevens worden gelekt, gegevens die worden beveiligd met [Windows Information Protection](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) worden niet gesynchroniseerd via Enterprise State Roaming voor apparaten die gebruikmaken van de Windows 10 Verjaardag Update.

**Aanbevolen actie**  
Geen. Toekomstige updates voor Windows kunnen dit probleem te verhelpen.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Datum, tijd en de regio-instellingen worden niet gesynchroniseerd op een apparaat lid is van een domein 
  
Apparaten die domein zijn niet ervaren synchronisatie uit voor de instelling van de datum, tijd en de regio: automatische tijd. Met behulp van automatische tijd kan de andere datum, tijd en de regio instellingen overschrijven en ervoor zorgen dat deze instellingen niet te synchroniseren. 

**Aanbevolen actie**  
Geen. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>Gebruikersaccountbeheer gevraagd bij het synchroniseren van wachtwoorden

Is van invloed op apparaten met Windows 10 November Update (versie 1511) met een draadloos NIC die is geconfigureerd voor het synchroniseren van wachtwoorden.

**Aanbevolen actie**  
Zorg ervoor dat de Windows 10 v1511-client heeft de cumulatieve Update ([KB3140743](https://support.microsoft.com/kb/3140743) OS Build 10586.494).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Synchronisatie werkt niet op apparaten die gebruikmaken van de smartcard voor aanmelding

Als u probeert aan te melden met uw Windows-apparaat via een smartcard of virtuele smartcard, zal werken niet meer instellingen synchroniseren.     

**Aanbevolen actie**  
Geen. Toekomstige updates voor Windows kunnen dit probleem te verhelpen.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Aan domein toegevoegd apparaat wordt niet gesynchroniseerd na het bedrijfsnetwerk verlaten     

Domein-apparaten die zijn geregistreerd bij Azure AD kunnen synchronisatie mislukt optreden als het apparaat buiten het bedrijf voor langere tijd is en domeinverificatie kan niet worden voltooid.

**Aanbevolen actie**  
Verbind het apparaat met een bedrijfsnetwerk, zodat de synchronisatie kan worden hervat.

---

### <a name="azure-ad-joined-device-is-not-syncing-and-the-user-has-a-mixed-case-user-principal-name"></a>Azure AD join-apparaat niet wordt gesynchroniseerd en de gebruiker heeft een gemengde case User Principal Name.

Als de gebruiker een gemengde aanvraag UPN (bijvoorbeeld de gebruikersnaam in plaats van gebruikersnaam heeft) en de gebruiker zich op een Azure AD join-apparaat dat is bijgewerkt van Windows 10 bouwen 10586 naar 14393, kan het apparaat van de gebruiker niet kunnen synchroniseren. 

**Aanbevolen actie**  
De gebruiker moet loskoppelen en opnieuw lid worden van het apparaat naar de cloud. Dit doet, meld u aan als de lokale beheerder-gebruiker en loskoppelen van het apparaat door te gaan naar **instellingen** > **System** > **over** en selecteer 'beheren of werk of school verbreken'. Opschonen van de volgende bestanden en Azure AD Join van het apparaat opnieuw in **instellingen** > **System** > **over** en selecteer ' verbinden met werk of School'. Doorgaan met het apparaat toevoegen aan Azure Active Directory en de stroom voltooien.

In de stap opruimen, opschonen van de volgende bestanden:
- Settings.dat in `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Alle bestanden onder de map `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>Gebeurtenis-ID 6065:80070533 die deze gebruiker kan niet aanmelden omdat dit account is momenteel uitgeschakeld.  

In Logboeken onder de SettingSync/Debug-Logboeken, kan deze fout worden weergegeven wanneer de referenties van de gebruiker zijn verlopen. Het kan ook optreden wanneer de tenant is automatisch geen AzureRMS ingericht. 

**Aanbevolen actie**  
In het eerste geval is, hebben de gebruikers hun referenties en meld u aan op het apparaat met de nieuwe referenties bijwerken. U lost het probleem AzureRMS, gaat u verder met de stappen in [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>Gebeurtenis-ID 1098: Fout: 0xCAA5001C Token broker-bewerking is mislukt  

In Logboeken onder logboeken van de AAD/operationeel voor deze fout kan worden weergegeven met gebeurtenis 1104: AAD Cloud Azië en Stille Oceaan-invoegtoepassing aanroep Get-token heeft fout geretourneerd: 0xC000005F. Dit probleem doet zich voor als er niet beschikken over machtigingen of eigendom kenmerken.  

**Aanbevolen actie**  
Doorgaan met de hier vermelde stappen [KB3196528](https://support.microsoft.com/kb/3196528).  

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht [enterprise state roaming overzicht](enterprise-state-roaming-overview.md).
