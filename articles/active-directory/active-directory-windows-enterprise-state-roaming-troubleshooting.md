---
title: Het oplossen van Enterprise State Roaming instellingen in Azure Active Directory | Microsoft Docs
description: Biedt antwoorden op enkele vragen IT-beheerders wellicht over de instellingen en synchroniseren van app-gegevens.
services: active-directory
keywords: Enterprise state roaming instellingen windows cloud, veelgestelde vragen op enterprise state roaming
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: f45d0515-99f7-42ad-94d8-307bc0d07be5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: tanning
ms.custom: it-pro
ms.openlocfilehash: 8ee3b523baf562b06bd5f7d652a431e1d4553d5c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Enterprise State Roaming instellingen voor probleemoplossing in Azure Active Directory

Dit onderwerp bevat informatie over het oplossen en analyseren van problemen met Enterprise State Roaming en bevat een lijst met bekende problemen.

## <a name="preliminary-steps-for-troubleshooting"></a>Voorbereidende stappen voor probleemoplossing 
Voordat u begint met het oplossen van problemen, moet u controleren of de gebruiker en apparaat correct zijn geconfigureerd en dat alle vereisten van het Enterprise State Roaming door het apparaat en de gebruiker wordt voldaan. 

1. Windows 10, is met de meest recente updates en een minimale versie 1511 (Build van 10586 of hoger) geïnstalleerd op het apparaat. 
2. Het apparaat is Azure AD die lid zijn van of hybride Azure AD die lid is. Zie voor meer informatie [het ophalen van een apparaat onder het beheer van Azure AD](device-management-introduction.md).
3. Zorg ervoor dat **Enterprise State Roaming** is ingeschakeld voor de tenant in Azure AD, zoals beschreven in [Enterprise State Roaming inschakelen](active-directory-windows-enterprise-state-roaming-enable.md). U kunt inschakelen voor alle gebruikers of voor een geselecteerde groep gebruikers roaming.
4. De gebruiker moet al een Azure Active Directory Premium-licentie worden toegewezen.  
25. Het apparaat moet opnieuw worden opgestart en de gebruiker moet opnieuw aanmelden bij de toegang tot de functies voor Enterprise State Roaming.

## <a name="information-to-include-when-you-need-help"></a>Informatie moet worden opgenomen wanneer u hulp nodig hebt
Als u niet op uw probleem met de onderstaande richtlijnen oplossen kunt, kunt u contact opnemen met onze ondersteuningsmedewerkers. Wanneer u verbinding maken met deze, zijn de volgende informatie:

* **Algemene beschrijving van de fout**: zijn er foutberichten gezien door de gebruiker? Als er geen foutbericht, wordt het onverwachte gedrag, in detail opgemerkt beschreven. Welke functies zijn ingeschakeld voor synchronisatie en wat is de gebruiker om te synchroniseren wordt verwacht? Meerdere functies niet synchroniseert of is deze in een geïsoleerd?
* **Betrokken gebruikers** – sync Is werkende/mislukt voor een of meer gebruikers? Het aantal apparaten per gebruiker betrokken zijn? Zijn ze niet synchroniseert alle of enkele ervan synchroniseren en sommige niet synchroniseren?
* **Informatie over de gebruiker** – welke identiteit van de gebruiker met aan te melden bij het apparaat is? Hoe wordt de gebruiker aangemeld bij het apparaat? Zijn ze deel uitmaken van een geselecteerde beveiligingsgroep kunnen synchroniseren? 
* **Informatie over het apparaat** – dit apparaat Azure AD-lid of het domein? Welke build is het apparaat op? Wat zijn de meest recente updates?
- **Datum / tijd / tijdzone** – wat is de exacte datum en tijd die u hebt gezien de fout (inclusief de tijdzone)?

Met inbegrip van deze informatie helpt ons zo snel mogelijk het probleem kunt oplossen.

## <a name="troubleshooting-and-diagnosing-issues"></a>Het oplossen van problemen en het onderzoeken van problemen
Deze sectie vindt u suggesties over het oplossen en analyseren van problemen met betrekking tot Enterprise State Roaming.

## <a name="verify-sync-and-the-sync-your-settings-settings-page"></a>Controleer of de synchronisatie en de instellingenpagina "Uw instellingen synchroniseren" 

1. Nadat u uw Windows 10-PC lid aan een domein dat is geconfigureerd voor het toestaan van Enterprise State Roaming, moet u zich aanmelden met uw werkaccount. Ga naar **instellingen** > **Accounts** > **uw synchronisatie-instellingen** en bevestigt u dat synchronisatie en de afzonderlijke instellingen op en de bovenkant van de instellingenpagina geeft aan dat u met uw werkaccount synchroniseert. Hetzelfde account wordt ook gebruikt als uw aanmeldingsaccount in bevestigen **instellingen** > **Accounts** > **uw Info**. 
2. Controleer of de synchronisatie over meerdere machines werkt door een aantal wijzigingen op de oorspronkelijke computer, zoals het verplaatsen van de taakbalk op de bovenste of rechts van het scherm. Bekijk de wijziging wordt doorgegeven aan de tweede computer binnen vijf minuten. 
  * Vergrendelen en ontgrendelen van het scherm (Win + L) kan helpen bij het activeren van een synchronisatie.
  * U moet aanmelden met hetzelfde account op beide computers voor synchronisatie om te werken, zoals Enterprise State Roaming is gekoppeld aan het gebruikersaccount en niet de computeraccount.

**Potentiële probleem**: als de besturingselementen in de **instellingen** pagina zijn niet beschikbaar en u ziet het bericht "enkele Windows-onderdelen zijn alleen beschikbaar als u een Microsoft-account of werkaccount gebruikt." Dit probleem zich voordoen voor apparaten die zijn ingesteld voor domein- en geregistreerde naar Azure AD, maar het apparaat niet nog is geverifieerd met Azure AD. Een mogelijke oorzaak is dat het apparaatbeleid moet worden toegepast, maar deze toepassing verloopt asynchroon en een paar uur vertraging kan optreden. 

### <a name="verify-the-device-registration-status"></a>Controleer of de status van het apparaat-registratie
Enterprise State Roaming vereist dat het apparaat moet worden geregistreerd met Azure AD. Hoewel specifieke Enterprise State Roaming, de onderstaande instructies kunt bevestigen dat de Windows 10-Client is geregistreerd en Bevestig de status van de vingerafdruk van instellingen voor de URL van Azure AD, NGC en andere informatie.

1.  Open de opdrachtprompt verlaagde bevoegdheden. U doet dit in Windows, opent u het venster uitvoeren starten (Win + R) en typ 'cmd' te openen.
2.  Zodra de opdrachtprompt geopend is, typt u "*dsregcmd.exe/status*'.
3.  Voor de verwachte uitvoer de **AzureAdJoined** veldwaarde moet 'Ja' **WamDefaultSet** veldwaarde moet 'Ja', en de **WamDefaultGUID** veldwaarde moet een GUID met '(AzureAd)' aan het einde.

**Potentiële probleem**: **WamDefaultSet** en **AzureAdJoined** zowel "Nee" in de veldwaarde, het apparaat is lid van een domein en geregistreerd bij Azure AD en het apparaat wordt niet gesynchroniseerd. Als het dit weergegeven wordt, moet het apparaat mogelijk wacht op beleid moet worden toegepast of de verificatie voor het apparaat is mislukt bij het verbinden met Azure AD. De gebruiker wellicht wacht enkele uren voor het beleid moet worden toegepast. Andere stappen voor probleemoplossing, omvat mogelijk automatische registratie door afmelden en weer opnieuw, of de taak in de Taakplanner wordt gestart. In sommige gevallen kan met '*dsregcmd.exe /leave*' in een opdrachtpromptvenster, opnieuw opstarten en het opnieuw proberen registratie kunnen dit probleem oplossen.


**Potentiële probleem**: het veld voor **AzureAdSettingsUrl** leeg is en het apparaat wordt niet gesynchroniseerd. De gebruiker mag hebben laatst heeft aangemeld op het apparaat voordat Enterprise State Roaming is ingeschakeld in de Azure Active Directory-Portal. Het apparaat opnieuw starten en de gebruikersaanmelding hebben. Probeer eventueel met de IT-beheerder uitschakelen en opnieuw inschakelen van gebruikers kunnen synchronisatie-instellingen en Enterprise-App-gegevens in de portal. Eenmaal weer wordt ingeschakeld, het apparaat opnieuw starten en de gebruikersaanmelding hebben. Als het probleem hiermee niet is opgelost **AzureAdSettingsUrl** mag niet leeg zijn in het geval van een certificaat met onjuiste apparaat. In dit geval wordt uitgevoerd '*dsregcmd.exe /leave*' in een opdrachtpromptvenster, opnieuw opstarten en het opnieuw proberen registratie kunnen dit probleem oplossen.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Enterprise State Roaming en meervoudige verificatie 
Onder bepaalde omstandigheden kan Enterprise State Roaming niet worden het synchroniseren van gegevens als Azure multi-factor Authentication is geconfigureerd. Raadpleeg het ondersteuningsdocument voor meer informatie over deze problemen [KB3193683](https://support.microsoft.com/kb/3193683). 

**Potentiële probleem**: als uw apparaat is geconfigureerd voor multi-factor Authentication vereisen in de Azure Active Directory-portal, mislukken u instellingen synchroniseren tijdens het aanmelden bij een Windows 10-apparaat met een wachtwoord. Dit type configuratie van multi-factor Authentication is bedoeld voor het beveiligen van een Azure-administrator-account. Beheergebruikers nog steeds mogelijk om te synchroniseren met het aanmelden bij hun Windows 10-apparaten met hun Microsoft Passport voor Work PINCODE of door multi-factor Authentication in te voeren bij het openen van andere Azure-services zoals Office 365.

**Potentiële probleem**: synchronisatie kan mislukken als de beheerder het voorwaardelijke toegangsbeleid van Active Directory Federation Services multi-factor Authentication configureert en het toegangstoken op het apparaat is verlopen. Zorg ervoor dat u aanmelden en afmelden met behulp van de Microsoft Passport voor Work PINCODE of multi-factor Authentication voltooid tijdens het openen van andere Azure-services zoals Office 365.

###<a name="event-viewer"></a>Logboeken
Voor geavanceerde probleemoplossing kan Logboeken worden gebruikt voor fouten gevonden. Deze worden beschreven in de onderstaande tabel. De gebeurtenissen kunnen worden gevonden in Logboeken > Logboeken toepassingen en Services > **Microsoft** > **Windows** > **SettingSync** en voor problemen met het synchroniseren van identiteitsgerelateerde **Microsoft** > **Windows** > **Azure AD**.


## <a name="known-issues"></a>Bekende problemen

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Synchronisatie werkt niet op apparaten met apps ' side-loaded met behulp van de MDM-software

Is van invloed op apparaten waarop de Update voor Windows 10 Verjaardag (versie 1607) wordt uitgevoerd. De gebeurtenis-ID 6013 met fout 80070259 is vaak zichtbaar in Logboeken onder logboeken van de SettingSync Azure.

**Aanbevolen actie**  
Zorg ervoor dat de client Windows 10-v1607 23 augustus 2016 cumulatieve Update ([KB3176934](https://support.microsoft.com/kb/3176934) OS bouwen 14393.82). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Favorieten in Internet Explorer synchroniseert geen

Is van invloed op apparaten waarop de Update voor Windows 10 November (versie 1511) wordt uitgevoerd.

**Aanbevolen actie**  
Zorg ervoor dat de Windows 10 v1511-client heeft de juli 2016 cumulatieve Update ([KB3172985](https://support.microsoft.com/kb/3172985) OS bouwen 10586.494).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Thema synchroniseert niet, evenals gegevens die worden beveiligd met Windows Information Protection 

Om te voorkomen dat gegevens worden gelekt, gegevens die worden beveiligd met [Windows Information Protection](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) wordt niet meer gesynchroniseerd tot en met Enterprise State Roaming voor apparaten met behulp van de Update voor Windows 10 verjaardag.



**Aanbevolen actie**  
Geen. Toekomstige updates voor Windows kunnen dit probleem oplossen.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Instellingen voor datum, tijd en regio Synchroniseer niet op domein-apparaat 
  
Apparaten die lid zijn van een domein niet krijgen met synchronisatie voor de instelling van de datum en tijd regio: automatische tijd. Met behulp van de automatische tijd kan onderdrukken van de andere instellingen voor datum, tijd en de regio en ervoor zorgen dat deze instellingen niet te synchroniseren. 

**Aanbevolen actie**  
Geen. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>UAC wordt gevraagd wanneer wachtwoorden synchroniseren

Is van invloed op apparaten met Windows 10 November Update (versie 1511) met een draadloze NIC die is geconfigureerd voor het synchroniseren van wachtwoorden.

**Aanbevolen actie**  
Zorg ervoor dat de Windows 10 v1511-client heeft de cumulatieve Update ([KB3140743](https://support.microsoft.com/kb/3140743) OS bouwen 10586.494).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Synchronisatie werkt niet op apparaten die gebruikmaken van de smartcard voor aanmelding
Als u probeert aan te melden bij uw Windows-apparaat met een smartcard of virtuele smartcard, stopt de synchronisatie van instellingen werken.     

**Aanbevolen actie**  
Geen. Toekomstige updates voor Windows kunnen dit probleem oplossen.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Domein-apparaat wordt niet gesynchroniseerd na het verlaten van bedrijfsnetwerk     
Domein-apparaten die zijn geregistreerd bij Azure AD kunnen synchronisatiefout optreden als het apparaat buiten het bedrijf voor langere tijd is en domeinverificatie kan niet worden voltooid.

**Aanbevolen actie**  
Verbind het apparaat met een bedrijfsnetwerk, zodat de synchronisatie kan worden hervat.

---

 ### <a name="azure-ad-joined-device-is-not-syncing-and-the-user-has-a-mixed-case-user-principal-name"></a>Azure AD join-apparaat niet wordt gesynchroniseerd en de gebruiker heeft een gemengde case User Principal-naam.
 Als de gebruiker een gemengde aanvraag UPN (bv UserName in plaats van de gebruikersnaam heeft) en de gebruiker zich op een Azure AD join-apparaat dat is bijgewerkt van Windows 10-Build 10586 naar 14393, mislukken apparaat van de gebruiker om te synchroniseren. 

**Aanbevolen actie**  
De gebruiker moet loskoppelen van en het apparaat naar de cloud weer. Dit doet, meld u aan als de gebruiker van de lokale beheerder en loskoppelen van het apparaat door te gaan naar **instellingen** > **System** > **over** en selecteer ' beheren of Verbreek de verbinding van werk of school '. Opschonen van de bestanden die hieronder en Azure AD Join van het apparaat opnieuw in **instellingen** > **System** > **over** en 'Verbinden naar werk of School' te selecteren. Doorgaan met het apparaat toevoegen aan Azure Active Directory en voltooien van de stroom.

Klik in de stap opschonen opruimen van de volgende bestanden:
- Settings.dat in`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Alle bestanden onder de map`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>Gebeurtenis-ID 6065:80070533 die deze gebruiker zich niet aanmelden omdat dit account is momenteel uitgeschakeld.  
In Logboeken onder logboeken voor SettingSync/foutopsporing, kan deze fout worden weergegeven wanneer de gebruiker referenties zijn verlopen. Het kan ook optreden wanneer de tenant is automatisch geen AzureRMS ingericht. 

**Aanbevolen actie**  
In het eerste geval is, hebben de gebruikers hun referenties en meld u aan het apparaat met de nieuwe referenties bijwerken. U lost het probleem AzureRMS, gaat u verder met de stappen in [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>Gebeurtenis-ID 1098: Fout: 0xCAA5001C Token broker-bewerking is mislukt  
In Logboeken onder logboeken van de AAD/operationeel voor deze fout kan worden gezien met gebeurtenis 1104: aanroep van de invoegtoepassing AAD Cloud Azië Get-token heeft een fout geretourneerd: 0xC000005F. Dit probleem treedt op als er machtigingen of mist eigendom kenmerken.  

**Aanbevolen actie**  
Doorgaan met de hier vermelde stappen [KB3196528](https://support.microsoft.com/kb/3196528).  



## <a name="next-steps"></a>Volgende stappen

- Gebruik de [User Voice-forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/158658-enterprise-state-roaming) naar uw feedback en suggesties voor het verbeteren van Enterprise State Roaming.

- Zie voor meer informatie de [Enterprise State Roaming overzicht](active-directory-windows-enterprise-state-roaming-overview.md). 

## <a name="related-topics"></a>Verwante onderwerpen
* [Overzicht van zwervende Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
* [Enterprise-status roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md)
* [Instellingen en veelgestelde vragen voor dataroaming](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Groep beleid en de MDM-instellingen voor synchronisatie van instellingen](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 zwervende naslaginformatie](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
