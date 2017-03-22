---
title: 'Aan de slag: Azure AD-wachtwoordbeheer | Microsoft Docs'
description: Stel gebruikers in staat om hun eigen wachtwoorden opnieuw in te stellen, ontdek wat de vereisten zijn voor het opnieuw instellen van wachtwoorden en schakel Wachtwoord terugschrijven in om on-premises wachtwoorden te beheren in Active Directory.
services: active-directory
keywords: wachtwoordbeheer Active Directory, wachtwoordbeheer, Azure AD-wachtwoord opnieuw instellen
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: joflore
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 441caf3cc9a3b9074bd263f4a4c45763967fa580
ms.lasthandoff: 03/15/2017


---
# <a name="getting-started-with-password-management"></a>Aan de slag met wachtwoordbeheer
> [!IMPORTANT]
> **Bent u hier terechtgekomen omdat u problemen ondervindt met het aanmelden?** Als dat het geval is, vindt u hier meer informatie over het [wijzigen en opnieuw instellen van uw eigen wachtwoord](active-directory-passwords-update-your-own-password.md#how-to-reset-your-password).
>
>

Er hoeven slechts enkele eenvoudige stappen te worden uitgevoerd om uw gebruikers in staat te stellen hun eigen wachtwoorden voor Azure Active Directory in de cloud of on-premises te beheren. Wanneer u hebt gecontroleerd of u aan een aantal eenvoudige vereisten voldoet, is het wijzigen van wachtwoorden en het opnieuw instellen van wachtwoorden voor uw volledige organisatie voor u het weet ingeschakeld. In dit artikel leest u meer over de volgende concepten:

* [**Handige tips van onze klanten voordat u begint**](#top-tips-from-our-customers-to-read-before-you-begin)
 * [**BELANGRIJKSTE TIP: NAVIGEREN IN DOCUMENTATIE** - Gebruik onze inhoudsopgave en de zoekfunctie van uw browser om antwoord op uw vragen te vinden](#top-tip-documentation-navigation---use-our-table-of-contents-and-your-browsers-find-feature-to-find-answers)
 * [**Tip 1: LICENTIEVERLENING** - Zorg ervoor dat de licentievereisten duidelijk zijn](#tip-1-licensing---make-sure-you-understand-the-licensing-requirements)
 * [**Tip 2: TESTEN** - Test met een eindgebruiker, niet een beheerder, en voer een pilot uit met een kleine groep gebruikers](#tip-2-testing---test-with-a-end-user-not-an-administrator-and-pilot-with-a-small-set-of-users)
 * [**Tip 3: IMPLEMENTATIE** - Vul vooraf gegevens in voor uw gebruikers, zodat ze zich niet hoeven te registreren](#tip-3-deployment---pre-populate-data-for-your-users-so-they-dont-have-to-register)
 * [**Tip 4: Implementatie** - Gebruik de functie voor wachtwoordherstel, zodat het niet meer nodig is om tijdelijke wachtwoorden te verstrekken](#tip-4-deployment---use-password-reset-to-obviate-the-need-to-communicate-temporary-passwords)
 * [**Tip 5: TERUGSCHRIJVEN** - Raadpleeg het toepassingsgebeurtenislogboek op uw computer met AAD Connect om problemen met het terugschrijven van wachtwoorden op te lossen](#tip-5-writeback---look-at-the-application-event-log-on-your-aad-connect-machine-to-troubleshoot-password-writeback)
 * [**Tip 6: TERUGSCHRIJVEN** - Zorg ervoor dat u de juiste machtigingen, firewallregels en verbindingsinstellingen inschakelt voor het terugschrijven van wachtwoorden](#tip-6-writeback---ensure-you-enable-the-correct-permissions-firewall-rules-and-connection-settings-for-password-writeback)
 * [**Tip 7: RAPPORTAGE** - Raadpleeg de auditlogboeken van Azure AD SSPR om te kijken wie er wachtwoorden registreert of opnieuw instelt](#tip-7-reporting---see-who-is-registering-or-resetting-passwords-with-the-azure-ad-sspr-audit-logs)
 * [**Tip 8: PROBLEMEN OPLOSSEN** - Lees onze gids voor probleemoplossing en de veelgestelde vragen om een antwoord te vinden voor een probleem](#tip-8-troubleshoot---read-our-troubleshooting-guide-and-faq-to-solve-many-issues)
 * [**Tip 9: PROBLEMEN OPLOSSEN** - Als u toch nog hulp nodig hebt, verzamel dan voldoende informatie voor ons om u te kunnen helpen](#tip-9-troubleshoot---if-you-still-need-help-include-enough-information-for-us-to-assist-you)
* [**Gebruikers in staat stellen om hun Azure Active Directory-wachtwoorden opnieuw in te stellen**](#enable-users-to-reset-their-azure-ad-passwords)
 * [Vereisten voor de selfservice voor wachtwoordherstel](#prerequisites)
 * [Stap 1: Een beleid voor het opnieuw instellen van wachtwoorden configureren](#step-1-configure-password-reset-policy)
 * [Stap 2: Contactgegevens toevoegen voor uw testgebruiker](#step-2-add-contact-data-for-your-test-user)
 * [Stap 3: Uw wachtwoord opnieuw instellen als gebruiker](#step-3-reset-your-azure-ad-password-as-a-user)
* [**Gebruikers in staat stellen om hun on-premises Active Directory-wachtwoorden te wijzigen of opnieuw in te stellen**](#enable-users-to-reset-or-change-their-ad-passwords)
 * [Vereisten voor het terugschrijven van wachtwoorden](#writeback-prerequisites)
 * [Stap 1: De nieuwste versie van Azure AD Connect downloaden](#step-1-download-the-latest-version-of-azure-ad-connect)
 * [Stap 2: Het terugschrijven van wachtwoorden in Azure AD Connect inschakelen via de gebruikersinterface of PowerShell en controles uitvoeren](#step-2-enable-password-writeback-in-azure-ad-connect)
 * [Stap 3: Uw firewall configureren](#step-3-configure-your-firewall)
 * [Stap 4: De juiste machtigingen instellen](#step-4-set-up-the-appropriate-active-directory-permissions)
 * [Stap 5: Uw AD-wachtwoord als gebruiker opnieuw instellen en controleren](#step-5-reset-your-ad-password-as-a-user)

## <a name="top-tips-from-our-customers-to-read-before-you-begin"></a>Belangrijke tips van onze klanten die u moet lezen voordat u begint
Hieronder vindt u enkele van de tips die in de praktijk zeer nuttig zijn gebleken voor klanten die wachtwoordbeheer hebben geïmplementeerd in hun organisatie.

* [**BELANGRIJKSTE TIP: NAVIGEREN IN DOCUMENTATIE** - Gebruik onze inhoudsopgave en de zoekfunctie van uw browser om antwoord op uw vragen te vinden](#top-tip-documentation-navigation---use-our-table-of-contents-and-your-browsers-find-feature-to-find-answers)
* [**Tip 1: LICENTIEVERLENING** - Zorg ervoor dat de licentievereisten duidelijk zijn](#tip-1-licensing---make-sure-you-understand-the-licensing-requirements)
* [**Tip 2: TESTEN** - Test met een eindgebruiker, niet een beheerder, en voer een pilot uit met een kleine groep gebruikers](#tip-2-testing---test-with-a-end-user-not-an-administrator-and-pilot-with-a-small-set-of-users)
* [**Tip 3: IMPLEMENTATIE** - Vul vooraf gegevens in voor uw gebruikers, zodat ze zich niet hoeven te registreren](#tip-3-deployment---pre-populate-data-for-your-users-so-they-dont-have-to-register)
* [**Tip 4: Implementatie** - Gebruik de functie voor wachtwoordherstel, zodat het niet meer nodig is om tijdelijke wachtwoorden te verstrekken](#tip-4-deployment---use-password-reset-to-obviate-the-need-to-communicate-temporary-passwords)
* [**Tip 5: TERUGSCHRIJVEN** - Raadpleeg het toepassingsgebeurtenislogboek op uw computer met AAD Connect om problemen met het terugschrijven van wachtwoorden op te lossen](#tip-5-writeback---look-at-the-application-event-log-on-your-aad-connect-machine-to-troubleshoot-password-writeback)
* [**Tip 6: TERUGSCHRIJVEN** - Zorg ervoor dat u de juiste machtigingen, firewallregels en verbindingsinstellingen inschakelt voor het terugschrijven van wachtwoorden](#tip-6-writeback---ensure-you-enable-the-correct-permissions-firewall-rules-and-connection-settings-for-password-writeback)
* [**Tip 7: RAPPORTAGE** - Raadpleeg de auditlogboeken van Azure AD SSPR om te kijken wie er wachtwoorden registreert of opnieuw instelt](#tip-7-reporting---see-who-is-registering-or-resetting-passwords-with-the-azure-ad-sspr-audit-logs)
* [**Tip 8: PROBLEMEN OPLOSSEN** - Lees onze gids voor probleemoplossing en de veelgestelde vragen om een antwoord te vinden voor een probleem](#tip-8-troubleshoot---read-our-troubleshooting-guide-and-faq-to-solve-many-issues)
* [**Tip 9: PROBLEMEN OPLOSSEN** - Als u toch nog hulp nodig hebt, verzamel dan voldoende informatie voor ons om u te kunnen helpen](#tip-9-troubleshoot---if-you-still-need-help-include-enough-information-for-us-to-assist-you)

### <a name="top-tip-documentation-navigation---use-our-table-of-contents-and-your-browsers-find-feature-to-find-answers"></a>BELANGRIJKSTE TIP: NAVIGEREN IN DOCUMENTATIE - Gebruik onze inhoudsopgave en de zoekfunctie van uw browser om antwoord op uw vragen te vinden
Als u gebruikmaakt van onze documentatie, zult u zien dat we hebben geprobeerd om in de inhoudsopgave zo veel mogelijk quicklinks op te nemen naar alle interessante onderwerpen voor beheerders. 

U vindt de inhoudsopgave via deze koppeling: 
* [Azure AD Password Reset: Documentation Table of Contents](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) (Azure AD-wachtwoord opnieuw instellen: inhoudsopgave van documentatie)

### <a name="tip-1-licensing---make-sure-you-understand-the-licensing-requirements"></a>Tip 1: LICENTIEVERLENING - Zorg ervoor dat de licentievereisten duidelijk zijn
De functie voor wachtwoordherstel van Azure AD werkt alleen als er ten minste één licentie is toegewezen in uw organisatie. Er wordt geen licentie per gebruiker afgedwongen voor de ervaring Wachtwoordherstel zelf. Als u de functie echter gebruikt zonder dat er een licentie is toegewezen aan een gebruiker, voldoet u niet aan de voorwaarden van uw Microsoft-licentieovereenkomst en zult u licenties moeten toewijzen aan die gebruikers.

Dit zijn enkele documenten die u kunt lezen om te begrijpen welke licenties zijn vereist voor de functie voor wachtwoordherstel.
* [General password reset licensing information]() (Algemene licentiegegevens voor wachtwoordherstel)
* [Per-feature password reset licensing information]() (Licentiegegevens voor wachtwoordherstel per functie)
* [Scenarios supported for password writeback]() (Scenario's die worden ondersteund voor terugschrijven van wachtwoorden)

### <a name="tip-2-testing---test-with-an-end-user-not-an-administrator-and-pilot-with-a-small-set-of-users"></a>Tip 2: TESTEN - Test met een eindgebruiker, niet een beheerder, en voer een pilot uit met een kleine groep gebruikers
Wanneer u test met een beheerder, is het beleid voor wachtwoordherstel voor beheerders van kracht. Dit beleid wordt hieronder toegelicht.  Dit betekent dat u NIET de verwachte resultaten ziet van het beleid dat u hebt geconfigureerd voor uw eindgebruikers.

De beleidsregels die zijn geconfigureerd in de beheerinterface, zijn ALLEEN van toepassing op eindgebruikers, niet op beheerders. Microsoft hanteert voor uw beheerders standaard een sterk beleid voor wachtwoordherstel om ervoor te zorgen dat uw organisatie optimaal beveiligd blijft. Deze beleidsregels kunnen afwijken van de beleidsregels die u instelt voor uw eindgebruikers.

#### <a name="administrator-password-reset-policy"></a>Beleid voor opnieuw instellen van beheerderswachtwoorden
* **Van toepassing op:** elke beheerdersrol (globale beheerder, helpdeskbeheerder, wachtwoordbeheerder, enzovoort)
* **Beleid met één gate geldt...**
 * ...gedurende de eerste 30 dagen nadat een proefabonnement is gestart **OF**
 * ...als er geen vanity domain wordt gebruikt **EN** er geen identiteiten worden gesynchroniseerd in Azure AD Connect
 * **_Vereist_**: dat **één** van deze opties is opgegeven: E-mailadres voor authenticatie, Alternatief e-mailadres, Telefoon voor authenticatie, Mobiele telefoon of Telefoon (werk)
* **Beleid met twee gates geldt...** 
 * ...als de eerste 30 dagen van een proefabonnement zijn verstreken **OF**
 * ...bij aanwezigheid van een vanity domain **OF** 
 * ...als u in Azure AD Connect hebt ingesteld dat identiteiten vanuit de on-premises omgeving moeten worden gesynchroniseerd
 * _**Vereist**_: dat **twee** van deze opties zijn opgegeven: E-mailadres voor authenticatie, Alternatief e-mailadres, Telefoon voor authenticatie, Mobiele telefoon of Telefoon (werk)

### <a name="tip-3-deployment---pre-populate-data-for-your-users-so-they-dont-have-to-register"></a>Tip 3: IMPLEMENTATIE - Vul vooraf gegevens in voor uw gebruikers, zodat ze zich niet hoeven te registreren
Veel mensen weten niet dat gebruikers zich niet hoeven te registreren voor de functie voor wachtwoordherstel om de functie te kunnen gebruiken.  Als u vooraf eigenschappen van een telefoon of e-mailadres instelt voor uw gebruikers, kunt u de functie voor wachtwoordherstel in één keer implementeren voor de hele organisatie, **zonder dat uw gebruikers iets hoeven te doen!**

Raadpleeg de volgende documentatie als u wilt weten hoe u dit doet met een API, PowerShell of Azure AD Connect:
* [Deploying password reset without requiring end user registration](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#deploying-password-reset-without-requiring-end-user-registration) (Wachtwoordherstel implementeren zonder registratie van eindgebruikers)
* [What data is used by password reset?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-data-is-used-by-password-reset) (Welke gegevens worden gebruikt tijdens wachtwoordherstel?)

### <a name="tip-4-deployment---use-password-reset-to-obviate-the-need-to-communicate-temporary-passwords"></a>Tip 4: IMPLEMENTATIE - Gebruik de functie voor wachtwoordherstel, zodat het niet nodig is om tijdelijke wachtwoorden te verstrekken
Deze tip is een vervolg op tip 3. Laten we ervan uitgaan dat al uw gebruikers vooraf zijn geconfigureerd voor wachtwoordherstel en dat er door uw bedrijf een nieuwe werknemer wordt aangenomen. In plaats van een tijdelijk wachtwoord naar deze persoon te sturen, hoeft u hem alleen maar te verwijzen naar de [portal voor het opnieuw instellen van wachtwoorden van Azure AD](https://passwordreset.microsoftonline.com), met het verzoek een nieuw wachtwoord in te stellen.

Als de gebruiker een [apparaat met Windows 10 gebruikt dat lid is van een Azure AD-domein](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy), is dit zelfs mogelijk vanuit het eerste aanmeldingsscherm van Windows 10 dat hij te zien krijgt. Op deze manier kan de gebruiker dus toegang krijgen tot een gloednieuwe pc zonder dat u iets hoeft te doen!

Raadpleeg de volgende documentatie als u wilt weten hoe u dit doet met een API, PowerShell of Azure AD Connect. Als de vereiste gegevens vooraf zijn ingevuld, stuurt u uw gebruikers een bericht met het verzoek hun wachtwoord opnieuw in te stellen. De gebruikers hebben dan direct toegang tot hun account:
* [Deploying password reset without requiring end user registration](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#deploying-password-reset-without-requiring-end-user-registration) (Wachtwoordherstel implementeren zonder registratie van eindgebruikers)
* [What data is used by password reset?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-data-is-used-by-password-reset) (Welke gegevens worden gebruikt tijdens wachtwoordherstel?)

### <a name="tip-5-writeback---look-at-the-application-event-log-on-your-aad-connect-machine-to-troubleshoot-password-writeback"></a>Tip 5: TERUGSCHRIJVEN - Raadpleeg het toepassingsgebeurtenislogboek op uw computer met AAD Connect om problemen met het terugschrijven van wachtwoorden op te lossen
Het toepassingsgebeurtenislogboek van Azure AD Connect bevat waardevolle informatie over veel van de activiteiten die verband houden met de service voor het terugschrijven van wachtwoorden. Het logboek wordt in real-time bijgehouden. Volg de onderstaande stappen om toegang te krijgen tot dit logboek:

1. Meld u aan bij de computer met **Azure AD Connect**.
2. Open **Logboeken van Windows** door op **Start** te klikken en **Logboeken** te typen.
3. Open het gebeurtenislogboek **Toepassing**.
4. Zoek naar gebeurtenissen uit de bronnen **PasswordResetService** en **ADSync** om informatie te verzamelen over het probleem dat zich mogelijk voordoet.

Raadpleeg de volgende onderwerpen voor een volledige lijst van gebeurtenissen die kunnen worden vastgelegd in dit logboek, evenals aanvullende informatie over het oplossen van problemen met het terugschrijven van wachtwoorden:
* [Troubleshoot Password Writeback](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback) (Problemen met het terugschrijven van wachtwoorden oplossen)
* [Password Writeback event log error codes](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#password-writeback-event-log-error-codes) (Foutcodes in het gebeurtenislogboek voor het terugschrijven van wachtwoorden)
* [Troubleshoot Password Writeback connectivity](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity) (Problemen oplossen met connectiviteit voor het terugschrijven van wachtwoorden)
* [Stap 3: Uw firewall configureren](#step-3-configure-your-firewall)
* [Stap 4: De juiste machtigingen instellen](#step-4-set-up-the-appropriate-active-directory-permissions)

### <a name="tip-6-writeback---ensure-you-enable-the-correct-permissions-firewall-rules-and-connection-settings-for-password-writeback"></a>Tip 6: TERUGSCHRIJVEN - Zorg ervoor dat u de juiste machtigingen, firewallregels en verbindingsinstellingen inschakelt voor het terugschrijven van wachtwoorden
Het terugschrijven van wachtwoorden werkt alleen als aan deze voorwaarden wordt voldaan:

1. De juiste **Active Directory-machtigingen** zijn ingesteld voor gebruikers die de functie voor het terugschrijven van wachtwoorden gebruiken, zodat ze beschikken over rechten om hun eigen wachtwoorden en ontgrendelingsvlaggen voor accounts te wijzigen in AD.
2. De juiste **firewallpoorten** zijn geopend, zodat de service voor het terugschrijven van wachtwoorden op een veilige manier via een uitgaande verbinding kan communiceren met de buitenwereld.
3. De juiste **firewalluitzonderingen** zijn ingesteld voor essentiële URL's voor de service voor het opnieuw instellen van wachtwoorden, zoals Service Bus.
4. **Niet-actieve uitgaande verbindingen worden niet beëindigd door de proxy- en firewallinstellingen**; het wordt aangeraden ze 10 minuten of langer in stand te houden.

Raadpleeg deze onderwerpen voor een volledig overzicht van informatie voor probleemoplossing en specifieke instructies voor het configureren van machtigingen en firewallregels om terugschrijven van wachtwoorden mogelijk te maken:
* [Troubleshoot Password Writeback](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback) (Problemen met het terugschrijven van wachtwoorden oplossen)
* [Password Writeback event log error codes](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#password-writeback-event-log-error-codes) (Foutcodes in het gebeurtenislogboek voor het terugschrijven van wachtwoorden)
* [Troubleshoot Password Writeback connectivity](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity) (Problemen oplossen met connectiviteit voor het terugschrijven van wachtwoorden)
* [Stap 3: Uw firewall configureren](#step-3-configure-your-firewall)
* [Stap 4: De juiste machtigingen instellen](#step-4-set-up-the-appropriate-active-directory-permissions)

### <a name="tip-7-reporting---see-who-is-registering-or-resetting-passwords-with-the-azure-ad-sspr-audit-logs"></a>Tip 7: RAPPORTAGE - Raadpleeg de auditlogboeken van Azure AD SSPR om te kijken wie er wachtwoorden registreert of opnieuw instelt 
Als wachtwoordherstel eenmaal is geïmplementeerd en werkt, is de volgende logische stap te kijken hoe het functioneert en te analyseren wie zich alsnog moet registreren. Ook kunt u dan zien welke problemen uw gebruikers regelmatig tegenkomen bij het opnieuw instellen van wachtwoorden en welke voordelen de functie oplevert.

Met behulp van de auditlogboeken van de functie voor wachtwoordherstel van Azure AD kunt u dit en nog veel meer doen vanuit Azure Portal, Power BI, de API Reporting Events van Azure AD of PowerShell.  Zie deze onderwerpen voor meer informatie over het gebruik van deze rapportagefuncties:
* [Overview of password management reports](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#overview-of-password-management-reports) (Overzicht van rapporten voor wachtwoordbeheer)
* [How to view Password Management Reports](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#how-to-view-password-management-reports) (Rapporten voor wachtwoordbeheer bekijken in Azure Portal)
* [Self-service Password Management activity types in the Azure Portal](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#self-service-password-management-activity-types-in-the-new-azure-portal) (Activiteitstypen van de self-service voor wachtwoordbeheer in Azure Portal)
* [How to retrieve password management events from the Azure AD Reports and Events API](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api) (Gebeurtenissen van wachtwoordbeheer ophalen uit de API Reports and Events van Azure AD)
* [How to download password reset registration events quickly with PowerShell](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-get-insights#how-to-download-password-reset-registration-events-quickly-with-powershell) (Gebeurtenissen van registratie met wachtwoordherstel snel downloaden met PowerShell)

### <a name="tip-8-troubleshoot---read-our-troubleshooting-guide-and-faq-to-solve-many-issues"></a>Tip 8: PROBLEMEN OPLOSSEN - Lees onze gids voor probleemoplossing en de veelgestelde vragen om een antwoord te vinden voor een probleem
Er is heel veel informatie beschikbaar voor het oplossen van problemen met de functie voor wachtwoordherstel, waaronder antwoorden op veelgestelde vragen. De kans is dan ook groot dat u het antwoord op een vraag via de onderstaande koppelingen kunt vinden.

Daarnaast biedt de blade **Ondersteuning en probleemoplossing** in [Azure Portal](https://portal.azure.com) toegang tot een grote hoeveelheid informatie over probleemoplossing. Gebruikers met beheerrechten kunnen deze blade weergeven door in het navigatievenster aan de linkerkant via **Azure Active Directory** -> **Gebruikers en groepen** -> **Wachtwoordherstel** -> **Ondersteuning en probleemoplossing** te kiezen.

Koppelingen naar informatie en veelgestelde vragen over problemen met wachtwoordherstel:
* [How to troubleshoot Password Management](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot) (Problemen met wachtwoordbeheer oplossen)
* [Password management frequently asked questions](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-faq) (Wachtwoordbeheer - Veelgestelde vragen)

### <a name="tip-9-troubleshoot---if-you-still-need-help-include-enough-information-for-us-to-assist-you"></a>Tip 9: PROBLEMEN OPLOSSEN - Als u toch nog hulp nodig hebt, verzamel dan voldoende informatie voor ons om u te kunnen helpen
Als het nog steeds niet is gelukt om het probleem op te lossen, willen we u natuurlijk graag helpen. U kunt hiervoor een ondersteuningsaanvraag openen of rechtstreeks contact met ons opnemen. We zijn er om u te helpen.

We kunnen u alleen snel helpen als u **alle onderstaande gegevens bij de hand hebt** wanneer u contact opneemt.
* [Benodigde informatie om snel te worden geholpen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#information-to-include-when-you-need-help)

#### <a name="ways-to-provide-password-reset-feedback"></a>Manieren om feedback te geven over de functie voor wachtwoordherstel
* [Functieaanvragen of probleemoplossing - berichten posten op MSDN-forums over Azure AD](https://social.msdn.microsoft.com/Forums/azure/home?forum=WindowsAzureAD)
* [Functieaanvragen of probleemoplossing - berichten posten op StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
* [Functieaanvragen of probleemoplossing - tweet naar @azuread!](https://twitter.com/azuread)
* [Alleen functieaanvragen - bericht achterlaten op UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory)

## <a name="enable-users-to-reset-their-azure-ad-passwords"></a>Gebruikers in staat stellen om hun Azure AD-wachtwoorden opnieuw in te stellen
In dit gedeelte vindt u meer informatie over het inschakelen van de selfservice voor wachtwoordherstel voor uw AAD-clouddirectory, over het registreren van gebruikers voor de selfservice voor wachtwoordherstel en het als gebruiker uitvoeren van een testwachtwoordherstel via de selfservice.

* [Vereisten voor de selfservice voor wachtwoordherstel](#prerequisites)
* [Stap 1: Een beleid voor het opnieuw instellen van wachtwoorden configureren](#step-1-configure-password-reset-policy)
* [Stap 2: Contactgegevens toevoegen voor uw testgebruiker](#step-2-add-contact-data-for-your-test-user)
* [Stap 3: Uw wachtwoord opnieuw instellen als gebruiker](#step-3-reset-your-azure-ad-password-as-a-user)

### <a name="prerequisites"></a>Vereisten
Voordat u het gebruik van de selfservice voor wachtwoordherstel kunt inschakelen, moet u aan de volgende vereisten voldoen:

* Maak een AAD-tenant. Zie [Aan de slag met Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/) voor meer informatie.
* Zorg voor een Azure-abonnement. Zie [Wat is een Azure AD-tenant?](active-directory-administer.md#what-is-an-azure-ad-tenant) voor meer informatie.
* Koppel uw AAD-tenant aan uw Azure-abonnement. Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx) voor meer informatie.
* Voer een upgrade uit naar Azure AD Premium of Basic of gebruik een betaalde O365-licentie. Zie [Azure Active Directory-edities](https://azure.microsoft.com/pricing/details/active-directory/) voor meer informatie.

  > [!NOTE]
  > Als u de selfservice voor wachtwoordherstel wilt inschakelen voor cloudgebruikers, moet u upgraden naar Azure AD Premium, Azure AD Basic of een betaalde O365-licentie.  Als u de selfservice voor wachtwoordherstel wilt inschakelen voor on-premises gebruikers, moet u upgraden naar Azure AD Premium. Zie [Azure Active Directory-edities](https://azure.microsoft.com/pricing/details/active-directory/) voor meer informatie. Deze informatie bevat gedetailleerde informatie over hoe u zich registreert voor Azure AD Premium of Basic, hoe u uw licentie activeert en hoe u toegang tot Azure AD activeert. U vindt hier ook informatie over het toewijzen van toegang aan beheerders- en gebruikersaccounts.
  >
  >
* Maak ten minste één beheerdersaccount en één gebruikersaccount in uw AAD-directory.
* Wijs een AAD Premium-, Basic- of betaalde O365-licentie toe aan de beheerder en het gebruikersaccount dat u hebt gemaakt.

### <a name="step-1-configure-password-reset-policy"></a>Stap 1: een beleid voor het opnieuw instellen van wachtwoorden configureren
Als u beleid voor het opnieuw instellen van wachtwoorden door gebruikers wilt configureren, volgt u de volgende stappen:

1. Open een browser naar keuze en ga naar de [klassieke Azure Portal](https://manage.windowsazure.com).
2. In de [klassieke Azure Portal](https://manage.windowsazure.com) gaat u naar de **Active Directory-extensie** op de navigatiebalk aan de linkerzijde.

   ![Wachtwoordbeheer in Azure AD][001]
3. Op het tabblad **Directory** klikt u op de directory waarin u het beleid voor het configureren van gebruikerswachtwoorden, zoals Wingtip Toys, wilt wijzigen.

    ![][002]
4. Klik op het tabblad **Configureren**.

   ![][003]

5. Op het tabblad **Configureren** gaat u naar het gedeelte **Beleid voor het opnieuw instellen van gebruikerswachtwoorden**.  Hier kunt u alle aspecten van het beleid voor het opnieuw instellen van gebruikerswachtwoorden configureren voor een bepaalde directory. *Als u het tabblad Configureren niet ziet, controleert u of u zich hebt geregistreerd voor Azure Active Directory Premium of Basic en of u __een licentie hebt toegewezen__ aan het beheerdersaccount waarmee deze functie wordt geconfigureerd.*  

   > [!NOTE]
   > **Het beleid dat u instelt, geldt alleen voor eindgebruikers in uw organisatie en niet voor beheerders**. Uit veiligheidsoverwegingen bepaalt Microsoft het beleid voor wachtwoordherstel voor beheerders. Met het huidige beleid voor beheerders zijn twee uitdagingen vereist: mobiel nummer en e-mailadres.

   ![][004]
6. Als u het beleid voor het opnieuw instellen voor gebruikerswachtwoorden wilt configureren, stelt u de knop **Gebruikers mogen wachtwoorden opnieuw instellen** in op **Ja**.  U krijgt dan meer besturingselementen te zien waarmee u kunt configureren hoe deze functie werkt in uw directory.  U kunt het opnieuw instellen van wachtwoorden aanpassen zoals u dat wilt.  Als u meer wilt weten over wat elk besturingselement van het beleid voor het opnieuw instellen van wachtwoorden doet, ziet u [Aanpassen: Azure AD-wachtwoordbeheer](active-directory-passwords-customize.md).

   ![][005]
7. Wanneer u het beleid voor het opnieuw instellen van wachtwoorden naar wens hebt geconfigureerd voor uw tenant, klikt u op **Opslaan**, onder aan het scherm.

   > [!NOTE]
   > Het beleid met twee uitdagingen wordt aanbevolen; dan kunt u zien hoe de functionaliteit in het meest complexe geval werkt.
   >
   >

   ![][006]

   > [!NOTE]
   > **Het beleid dat u instelt, geldt alleen voor eindgebruikers in uw organisatie en niet voor beheerders**. Uit veiligheidsoverwegingen bepaalt Microsoft het beleid voor wachtwoordherstel voor beheerders. Met het huidige beleid voor beheerders zijn twee uitdagingen vereist: mobiel nummer en e-mailadres.
   >
   >

### <a name="step-2-add-contact-data-for-your-test-user"></a>Stap 2: contactgegevens toevoegen voor uw testgebruiker
U hebt verschillende mogelijkheden voor het opgeven van gegevens van gebruikers in uw organisatie voor het uitvoeren van wachtwoordherstel.

* Gebruikers bewerken in de [klassieke Azure Portal](https://manage.windowsazure.com) of de [Office 365-beheerportal](https://portal.microsoftonline.com)
* Gebruik AAD Connect om gebruikerseigenschappen met Azure AD te synchroniseren via een on-premises Active Directory-domein
* Windows PowerShell gebruiken om gebruikerseigenschappen te bewerken
* Stel gebruikers in staat om hun eigen gegevens te registreren door ze naar de registratieportal te leiden op [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
* Vereis dat gebruikers zich registreren voor wachtwoordherstel wanneer ze zich aanmelden bij het deelvenster Toegang [http://myapps.microsoft.com](http://myapps.microsoft.com); dit doet u door de SSPR-configuratieoptie **Vereisen dat gebruikers zich registreren wanneer ze zich aanmelden bij het deelvenster Toegang** in te stellen op **Ja**.

Als u meer wilt weten over welke gegevens worden gebruikt tijdens het opnieuw instellen van een wachtwoord, en welke indelingsvereisten er gelden voor deze gegevens, ziet u [Welke gegevens worden gebruikt voor het opnieuw instellen van wachtwoorden?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

#### <a name="to-add-user-contact-data-via-the-user-registration-portal"></a>Contactgegevens van een gebruiker toevoegen via de gebruikersregistratieportal
1. Als u de registratieportal voor wachtwoordherstel wilt gebruiken, moet u de gebruikers in uw organisatie een koppeling sturen naar deze pagina ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) of de optie inschakelen zodat wordt vereist dat gebruikers zich automatisch registreren.  Wanneer ze op deze koppeling klikken, wordt hen gevraagd om zich aan te melden met hun bedrijfsaccount.  Wanneer ze dat hebt gedaan, krijgen ze de volgende pagina te zien:

   ![][007]
2. Hier kunnen gebruikers hun mobiele telefoonnummer opgeven en controleren en hun e-mailadres en/of beveiligingsvragen wijzigen.  Het controleren van een mobiel telefoonnummer gaat als volgt.

   ![][008]
3. Wanneer een gebruiker deze informatie opgeeft, wordt de pagina bijgewerkt om aan te geven dat de informatie geldig is (deze wordt hieronder verborgen).  De gebruiker gaat door op **Voltooien** of **Annuleren** te klikken naar het deelvenster Toegang.

   ![][009]
4. Wanneer een gebruiker beide gegevens heeft geverifieerd, wordt zijn of haar profiel bijgewerkt met de opgegeven gegevens.  In dit voorbeeld is het nummer **Telefoon (werk)** handmatig opgegeven zodat de gebruiker dat nummer ook als contactmethode kan gebruiken voor het opnieuw instellen van zijn of haar wachtwoord.

   ![][010]

### <a name="step-3-reset-your-azure-ad-password-as-a-user"></a>Stap 3: uw Azure AD-wachtwoord als gebruiker opnieuw instellen
Nu u een beleid voor het opnieuw instellen van wachtwoorden door gebruikers hebt geconfigureerd en u contactgegevens hebt opgegeven voor uw gebruiker, kan deze gebruiker de selfservice voor wachtwoordherstel gebruiken.

#### <a name="to-perform-a-self-service-password-reset"></a>De selfservice voor wachtwoordherstel gebruiken
1. Wanneer u naar een site als [**portal.microsoftonline.com**](http://portal.microsoftonline.com) gaat, ziet u een aanmeldingsscherm zoals dat hieronder.  Klik op de koppeling **Geen toegang tot uw account?** om de gebruikersinterface voor het opnieuw instellen van wachtwoorden te testen.

   ![][011]
2. Wanneer u op **Geen toegang tot uw account?** hebt geklikt, gaat u naar een nieuwe pagina waarop om de **gebruikersnaam** wordt gevraagd waarvoor u het wachtwoord opnieuw wilt instellen.  Voer uw test**gebruikersnaam** in, voer de CAPTCHA in en klik op **Volgende**.

   ![][012]
3. Omdat de gebruiker een **zakelijk telefoonnummer**, **mobiele telefoonnummer** en een **alternatief e-mailadres** heeft opgegeven, ziet u dat hij of zij al die opties heeft om de eerste uitdaging te doorstaan.

   ![][013]
4. Kies er in dat geval voor om eerst te **bellen** naar het **zakelijke telefoonnummer**.  Wanneer de telefoonmethode wordt geselecteerd, wordt gebruikers gevraagd om hun telefoonnummer **te verifiëren** voordat ze hun wachtwoord opnieuw instellen.  Dit is om te voorkomen dat kwaadwillende personen gebruikers in uw organisatie gaan lastigvallen via hun telefoonnummers.

   ![][014]
5. Wanneer de gebruiker zijn of haar telefoonnummer heeft bevestigd en op Bellen heeft geklikt, wordt er een spinner weergegeven en gaat de telefoon over.  Er wordt een bericht afgespeeld wanneer hij of zij de telefoon opneemt. De gebruiker moet **op # klikken** om zijn of haar account te verifiëren.  Wanneer op deze toets wordt gedrukt, wordt automatisch geverifieerd dat de gebruiker de eerste uitdaging heeft doorstaan en gaat deze in de gebruikersinterface door naar de tweede verificatiestap.

   ![][015]
6. Wanneer u de eerste uitdaging hebt doorstaan, wordt de gebruikersinterface automatisch bijgewerkt zodat deze niet meer in de lijst opties van de gebruiker staat.  Omdat u het **zakelijke telefoonnummer** al hebt gebruikt, zijn alleen het **mobiele telefoonnummer** en het **alternatieve e-mailadres** nog beschikbaar als opties voor de tweede verificatiestap.  Klik op de optie **Stuur e-mail naar mijn alternatieve e-mailadres**.  Wanneer u dat hebt gedaan, wordt er een e-mail naar het geregistreerde alternatieve e-mailadres gestuurd wanneer u op E-mailen klikt.

   ![][016]
7. Hier volgt een voorbeeld van een e-mailbericht dat gebruikers mogelijk zien (let op de tenantvormgeving):

   ![][017]
8. Wanneer het e-mailbericht is ontvangen, wordt de pagina bijgewerkt en kunt u de verificatiecode invoeren die in de e-mail staat. Dat doet u in het invoervak, zoals hieronder wordt weergegeven.  Wanneer de juiste code is ingevoerd, wordt de knop Volgende beschikbaar en hebt u de tweede verificatiestap voltooid.

   ![][018]
9. Als u aan de vereisten van het organisatiebeleid voldoet, kunt u een nieuw wachtwoord kiezen.  Het wachtwoord wordt gevalideerd als het voldoet aan de AAD-vereisten voor een sterk wachtwoord (zie [Wachtwoordbeleid in Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx)). In het scherm is te zien of het wachtwoord dat de gebruiker invoert aan het beleid voldoet of niet.

   ![][019]
10. Wanneer u overeenkomende wachtwoorden hebt opgegeven die voldoen aan het organisatiebeleid, wordt uw wachtwoord opnieuw ingesteld en u kunt zich al direct aanmelden met uw nieuwe wachtwoord.

    ![][020]

## <a name="enable-users-to-reset-or-change-their-ad-passwords"></a>Gebruikers in staat stellen om hun AD-wachtwoorden opnieuw in te stellen of te wijzigen
In dit gedeelte vindt u meer informatie over het configureren van het opnieuw instellen van wachtwoorden, zodat ze worden teruggeschreven naar een on-premises Active Directory.

* [Vereisten voor het terugschrijven van wachtwoorden](#writeback-prerequisites)
* [Stap 1: De nieuwste versie van Azure AD Connect downloaden](#step-1-download-the-latest-version-of-azure-ad-connect)
* [Stap 2: Het terugschrijven van wachtwoorden in Azure AD Connect inschakelen via de gebruikersinterface of PowerShell en controles uitvoeren](#step-2-enable-password-writeback-in-azure-ad-connect)
* [Stap 3: Uw firewall configureren](#step-3-configure-your-firewall)
* [Stap 4: De juiste machtigingen instellen](#step-4-set-up-the-appropriate-active-directory-permissions)
* [Stap 5: Uw AD-wachtwoord als gebruiker opnieuw instellen en controleren](#step-5-reset-your-ad-password-as-a-user)

### <a name="writeback-prerequisites"></a>Vereisten voor terugschrijven
Voordat u het terugschrijven van wachtwoorden kunt inschakelen, moet u aan de volgende vereisten voldoen:

* U hebt een Azure AD-tenant waarvoor Azure AD Premium is ingeschakeld.  Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie.
* Het opnieuw instellen van wachtwoorden is geconfigureerd en ingeschakeld in uw tenant.  Zie [Gebruikers in staat stellen om hun Azure AD-wachtwoorden opnieuw in te stellen](#enable-users-to-reset-their-azure-ad-passwords) voor meer informatie
* U hebt ten minste één beheerdersaccount en één testgebruikersaccount nodig in combinatie met een Azure AD Premium-licentie waarmee u deze functie kunt testen.  Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie.

  > [!NOTE]
  > Zorg ervoor dat het beheerdersaccount dat u gebruikt om Wachtwoord terugschrijven in te schakelen, een cloudbeheerdersaccount is (gemaakt in Azure AD) en geen gefedereerd account (gemaakt in on-premises AD en gesynchroniseerd met Azure AD).
  >
  >
* U hebt een on-premises AD-implementatie met één of meerdere forests met Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 of Windows Server 2012 R2 waarop de nieuwste servicepacks zijn geïnstalleerd.

  > [!NOTE]
  > Als u een oudere versie van Windows Server 2008 or 2008 R2 gebruikt, kunt u deze functie ook gebruiken, maar dan moet u [KB 2386717 downloaden en installeren](https://support.microsoft.com/kb/2386717), anders kunt u uw lokale AD-wachtwoordbeleid niet toepassen in de cloud.
  >
  >
* U hebt het Azure AD Connect-hulpprogramma geïnstalleerd en u hebt uw AD-omgeving voorbereid op synchronisatie naar de cloud.  Zie [Uw on-premises infrastructuur voor identiteiten in de cloud gebruiken](connect/active-directory-aadconnect.md) voor meer informatie.

  > [!NOTE]
  > Voordat u Wachtwoord terugschrijven test, voert u een volledige importeerbewerking en een volledige synchronisatie uit vanuit AD en Azure AD in Azure AD Connect.
  >
  >
* Als u Azure AD Sync of Azure AD Connect gebruikt, moet in sommige gevallen de uitgaande poort **TCP 443** (en in sommige gevallen **TCP 9350-9354**) open zijn.  Zie [Stap 3: uw firewall configureren](#step-3-configure-your-firewall) voor meer informatie. Het gebruik van DirSync wordt in dit scenario niet meer ondersteund.  Als u nog steeds DirSync gebruikt, voert u een upgrade uit naar de nieuwste versie van Azure AD Connect voordat u het terugschrijven van wachtwoorden implementeert.

  > [!NOTE]
  > We raden iedereen die de hulpprogramma’s Azure AD Sync en DirSync gebruikt om te upgraden naar de nieuwste versie van Azure AD Connect voor de best mogelijke gebruikerservaring en nieuwe functies (zodra ze beschikbaar worden gemaakt).
  >
  >

### <a name="step-1-download-the-latest-version-of-azure-ad-connect"></a>Stap 1: de nieuwste versie van Azure AD Connect downloaden
Wachtwoord terugschrijven is beschikbaar in de releases van Azure AD Connect of in het Azure AD Sync-hulpprogramma met versienummer **1.0.0419.0911** of hoger.  Wachtwoord terugschrijven met automatische accountontgrendeling is beschikbaar in de releases van Azure AD Connect of in het Azure AD Sync-hulpprogramma met versienummer **1.0.0485.0222** of hoger. Als u een oudere versie gebruikt, voert u een upgrade uit naar ten minste deze versie voordat u doorgaat. [Klik hier om de nieuwste versie van Azure AD Connect te downloaden](connect/active-directory-aadconnect.md#install-azure-ad-connect).

#### <a name="to-check-the-version-of-azure-ad-sync"></a>De versie van Azure AD Sync controleren
1. Ga naar **%ProgramFiles%\Azure Active Directory Sync\**.
2. Zoek het uitvoerbare bestand **ConfigWizard.exe**.
3. Klik met de rechtermuisknop op het uitvoerbare bestand en selecteer de optie **Eigenschappen** in het contextmenu.
4. Klik op het tabblad **Details**.
5. Zoek het veld **Bestandsversie**.

   ![][021]

Als het versienummer hoger is dan of gelijk is aan **1.0.0419.0911** of als u Azure AD Connect installeert, kunt u doorgaan naar [Stap 2: het terugschrijven van wachtwoorden in Azure AD Connect inschakelen via de gebruikersinterface of PowerShell en controles uitvoeren](#step-2-enable-password-writeback-in-azure-ad-connect).

> [!NOTE]
> Als dit de eerste keer is dat u het Azure AD Connect-hulpprogramma installeert, wordt aangeraden dat u enkele aanbevolen procedures uitvoert om uw omgeving voor te bereiden op de directorysynchronisatie.  Voordat u het hulpprogramma Azure AD Connect installeert, moet u directorysynchronisatie inschakelen via de [Office 365-beheerportal](https://portal.microsoftonline.com) of de [klassieke Azure Portal](https://manage.windowsazure.com).  Zie [Azure AD Connect beheren](active-directory-aadconnect-whats-next.md) voor meer informatie.
>
>

### <a name="step-2-enable-password-writeback-in-azure-ad-connect"></a>Stap 2: wachtwoord terugschrijven inschakelen in Azure AD Connect
Nu u het Azure AD Connect-hulpprogramma hebt gedownload, kunt u Wachtwoord terugschrijven inschakelen.  U kunt dit op twee manieren doen.  U kunt Wachtwoord terugschrijven inschakelen via het scherm Optionele functies van de Azure AD Connect-installatiewizard, of u kunt het inschakelen via Windows PowerShell.

#### <a name="to-enable-password-writeback-in-the-configuration-wizard"></a>Wachtwoord terugschrijven inschakelen via de configuratiewizard
1. Op uw **Directory Sync-computer** opent u de **Azure AD Connect**-configuratiewizard.
2. Klik door de stappen totdat u het configuratiescherm **Optionele functies** bereikt.
3. Schakel de optie **Wachtwoord terugschrijven** in.

   ![][022]
4. Voltooi de wizard. Op de laatste pagina wordt een overzicht weergegeven van de wijzigingen. In dit overzicht staat ook dat de configuratie van Wachtwoord terugschrijven is gewijzigd.

> [!NOTE]
> U kunt Wachtwoord terugschrijven te allen tijde weer uitschakelen door deze wizard opnieuw uit te voeren en de optie uit te schakelen, of door **Wachtwoorden terugschrijven naar on-premises directory** in te stellen op **Nee** in het gedeelte **Beleid voor het opnieuw instellen van wachtwoorden van gebruikers** van het tabblad **Configureren** van uw directory in de [klassieke Azure Portal](https://manage.windowsazure.com).  Zie [Aanpassen: Azure AD-wachtwoordbeheer](active-directory-passwords-customize.md) voor meer informatie over het aanpassen van de ervaring bij het opnieuw instellen van wachtwoorden.
>
>

#### <a name="to-enable-password-writeback-using-windows-powershell"></a>Wachtwoord terugschrijven inschakelen met Windows PowerShell
1. Op uw **Directory Sync-computer** opent u een nieuw **verhoogd Windows PowerShell-venster**.
2. Als de module nog niet is geladen, typt u in de `import-module ADSync`-opdracht om de Azure AD Connect-cmdlets naar uw huidige sessie te laden.
3. Haal de lijst Azure AD-connectors in uw systeem op door de cmdlet `Get-ADSyncConnector` uit te voeren en de resultaten op te slaan in `$aadConnectorName`, zoals `$connectors = Get-ADSyncConnector|where-object {$\_.name -like "\*AAD"}`
4. De huidige status van terugschrijven voor de huidige connector ophalen door de volgende cmdlet uit te voeren: `Get-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name`
5. Wachtwoord terugschrijven inschakelen door de cmdlet uit te voeren: `Set-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name –Enable $true`

> [!NOTE]
> Als u wordt gevraagd om een referentie, zorgt u ervoor dat het beheerdersaccount dat u opgeeft voor AzureADCredential een **cloudbeheerdersaccount is (gemaakt in Azure AD)** en geen gefedereerd account (gemaakt in on-premises AD en gesynchroniseerd met Azure AD).
>
> [!NOTE]
> U kunt Wachtwoord terugschrijven uitschakelen via PowerShell door de bovenstaande instructies uit te voeren, maar door `$false` over te slaan of door de instelling **Wachtwoorden terugschrijven naar on-premises directory** in te stellen op **Nee** in het gedeelte **Beleid voor het opnieuw instellen van wachtwoorden van gebruikers** van het tabblad **Configureren** van uw directory in de [klassieke Azure Portal](https://manage.windowsazure.com).
>
>

#### <a name="verify-that-the-configuration-was-successful"></a>Controleren of de configuratie is voltooid
Wanneer de configuratie is voltooid, krijgt u het bericht Terugschrijven voor wachtwoord opnieuw instellen is ingeschakeld te zien in het Windows PowerShell-venster, of u krijgt een voltooiingsbericht te zien in de gebruikersinterface van de configuratie.

U kunt ook controleren of de service correct is geïnstalleerd door Logboeken te openen, naar het toepassingsgebeurtenislogboek te gaan en te zoeken naar de gebeurtenis **31005 - OnboardingEventSuccess**, van de bron **PasswordResetService**.

  ![][023]

### <a name="step-3-configure-your-firewall"></a>Stap 3: uw firewall configureren
Als u Wachtwoord terugschrijven hebt ingeschakeld, moet u ervoor zorgen dat de machine met Azure AD Connect de Microsoft-cloudservices kan bereiken om op die manier aanvragen voor het terugschrijven van wachtwoorden te ontvangen. Deze stap omvat het bijwerken van de verbindingsregels in uw netwerkapparaten (proxyservers, firewalls enz.), zodat uitgaande verbindingen met bepaalde [URL's en IP-adressen van Microsoft](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US) worden toegestaan via bepaalde netwerkpoorten. Deze wijzigingen kunnen variëren op basis van de versie van het Azure AD Connect-hulpprogramma. Voor aanvullende context leest u meer over [hoe het terugschrijven van wachtwoorden werkt](active-directory-passwords-learn-more.md#how-password-writeback-works) en over [het beveiligingsmodel voor het terugschrijven van wachtwoorden](active-directory-passwords-learn-more.md#password-writeback-security-model).

#### <a name="why-do-i-need-to-do-this"></a>Waarom moet ik dit doen?

Als u wilt dat het terugschrijven van wachtwoorden goed werkt, moet de machine waarop Azure AD Connect wordt uitgevoerd, een uitgaande HTTPS-verbinding kunnen maken met **.servicebus.windows.net* en bepaalde IP-adressen die worden gebruikt door Azure. Deze worden gedefinieerd in de [Microsoft Azure-lijst met datacenter-IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).

Voor Azure AD Connect-versie **1.1.443.0** (nieuwste) en hoger:

- De meest recente versie van het hulpprogramma Azure AD Connect moet **uitgaande HTTPS-toegang** hebben tot:
    - *passwordreset.microsoftonline.com*
    - *servicebus.windows.net*

Voor Azure AD Connect-versie **1.0.8667.0** tot **1.1.380.0**:

- **Optie 1:** alle uitgaande HTTPS-verbindingen toestaan via poort 443 (met een URL of IP-adres).
    - Wanneer u deze optie gebruikt:
        - Gebruik deze optie als u een zo eenvoudig mogelijke configuratie wilt gebruiken die niet hoeft te worden bijgewerkt wanneer de Azure-datacenter-IP-bereiken veranderen.
    - Vereiste stappen:
        - Sta alle uitgaande HTTPS-verbindingen via poort 443 toe met een URL of IP-adres.
<br><br>
- **Optie 2:** uitgaande HTTPS-verbindingen met specifieke IP-bereiken en URL's toestaan
    - Wanneer u deze optie gebruikt:
        - Gebruik deze optie als u in een beperkte netwerkomgeving werkt of om andere redenen uitgaande verbindingen niet wilt toestaan.
        - Als u wilt dat het terugschrijven van wachtwoorden bij deze configuratie blijft werken, moet u ervoor zorgen dat uw netwerkapparaten elke week worden bijgewerkt met de nieuwste IP-adressen van de Microsoft Azure-lijst met datacenter-IP-bereiken. Deze IP-bereiken zijn beschikbaar in een XML-bestand dat elke woensdag (Pacific Time) wordt bijgewerkt en de daarop volgende maandag (Pacific Time) van kracht wordt.
    - Vereiste stappen:
        - Alle uitgaande HTTPS-verbindingen met *.servicebus.windows.net toestaan
        - Sta alle uitgaande HTTPS-verbindingen met de IP-adressen in de Microsoft Azure-lijst met datacenter-IP-bereiken toe en werk deze configuratie elke week bij.

> [!NOTE]
> Als u het terugschrijven van wachtwoorden hebt geconfigureerd door de bovenstaande instructies te volgen en u geen fouten ziet in het Azure AD Connect-gebeurtenislogboek, maar er wel connectiviteitsproblemen optreden bij het testen, kan het zijn dat een netwerkapparaat in uw omgeving HTTP-verbindingen met IP-adressen blokkeert. Als verbinding met *https://*.servicebus.windows.net* bijvoorbeeld is toegestaan, kan het zijn dat de verbinding met een bepaald IP-adres binnen dat bereik wordt geblokkeerd. U kunt dit oplossen door uw netwerkomgeving zodanig te configureren dat uitgaande HTTPS-verbindingen via poort 443 met alle URL's en IP-adressen (optie 1 hierboven) worden toegestaan. Als alternatief kunt u ook contact opnemen met uw netwerkteam om HTTPS-verbindingen met specifieke IP-adressen toe te staan (optie 2 hierboven).

**Voor oudere versies:**

- Uitgaande TCP-verbindingen via poort 443, 9350-9354 en poort 5671 toestaan
- Uitgaande verbindingen met *https://ssprsbprodncu-sb.accesscontrol.windows.net/* toestaan

> [!NOTE]
> Als u een oudere versie van Azure AD Connect gebruikt dan 1.0.8667.0, raden we u aan te upgraden naar de [meest recente versie van Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594). Deze omvat een aantal verbeteringen met betrekking tot terugschrijven binnen netwerken die de configuratie vereenvoudigen.

Nadat de netwerkapparaten zijn geconfigureerd, moet u de machine met het Azure AD Connect-hulpprogramma opnieuw opstarten.

#### <a name="idle-connections-on-azure-ad-connect-114430-and-up"></a>Niet-actieve verbindingen op Azure AD Connect (1.1.443.0 en hoger)
Het hulpprogramma Azure AD Connect verzendt periodieke pings/keepalives naar ServiceBus-eindpunten om ervoor te zorgen dat de verbindingen actief blijven. Als het hulpprogramma detecteert dat er te veel verbindingen worden afgesloten, wordt de frequentie van pings naar het eindpunt automatisch verhoogd. De laagste pingintervallen worden verlaagd naar 1 ping per 60 seconden. **We raden echter aan dat proxy's/firewalls verbindingen toestaan om ten minste 2-3 minuten niet-actief te zijn.** \*Voor oudere versies raden we vier minuten of meer aan.

### <a name="step-4-set-up-the-appropriate-active-directory-permissions"></a>Stap 4: de juiste Active Directory-machtigingen instellen
Bij elke forest die gebruikers bevat waarvan de wachtwoorden opnieuw worden ingesteld en waarbij X het account is dat voor die forest is opgegeven in de configuratiewizard (tijdens de eerste configuratie), moet X de machtigingen **Wachtwoord opnieuw instellen**, **Wachtwoord wijzigen**, **Schrijfmachtigingen** voor `lockoutTime` en **Schrijfmachtigingen** voor `pwdLastSet` krijgen. Daarbij moet X ook uitgebreide rechten krijgen voor het hoofdobject van elk domein in die forest OF voor de organisatie-eenheid van gebruikers die in scope moet zijn voor SSPR. In het laatste geval kunt u trouwens ook meerdere organisatie-eenheden opgeven.  Gebruik de laatste optie als u de scope van de machtigingen voor wachtwoordherstel wilt beperken tot een specifieke set gebruikersobjecten, omdat het niet wenselijk is om het hoofdobject van het domein op te nemen in de scope. Het recht moet worden gemarkeerd als 'overgenomen door alle gebruikersobjecten'.  

Als u niet zeker welk account hierboven wordt bedoeld, opent u de gebruikersinterface van de Azure Active Directory Connect-configuratie en klikt u op **Uw oplossing controleren**.  Het account waar u machtigingen voor moet toevoegen, is rood onderstreept in de onderstaande schermafbeelding.

**<font color="red">Zorg ervoor dat deze machtiging voor elk domein in elk forest van uw systeem wordt ingesteld, anders werkt Wachtwoord terugschrijven niet goed.</font>**

  ![][032]

  Als u deze machtigingen instelt, kan het MA-serviceaccount voor elk forest de wachtwoorden beheren namens de gebruikersaccounts in dat forest. Als u deze machtigingen niet toewijst, lijkt het alsof terugschrijving correct is geconfigureerd, maar kan het zijn er fouten optreden bij gebruikers wanneer ze hun on-premises wachtwoorden vanuit de cloud proberen te wijzigen. Hier volgen gedetailleerde stappen op basis waarvan u dit kunt doen met de **Active Directory-beheermodule Gebruikers en computers**:

> [!NOTE]
> Het kan een uur duren voordat de machtigingen zijn gerepliceerd naar alle objecten in uw directory.
>
>

#### <a name="to-set-up-the-right-permissions-for-writeback-to-occur"></a>De juiste machtigingen instellen voor terugschrijving
1. Open **Active Directory: gebruikers en computers** met een account met de juiste machtigingen voor domeinbeheer.
2. Zorg ervoor dat bij **Weergavemenu** de optie **Geavanceerde functies** is ingeschakeld.
3. In het linkerdeelvenster klikt u met de rechtermuisknop op het object dat de hoofdmap van het domein vertegenwoordigt.
4. Klik op het tabblad **Beveiliging**.
5. Klik vervolgens op **Geavanceerd**.

   ![][024]
6. Klik op het tabblad **Machtigingen** op **Toevoegen**.

   ![][025]
7. Selecteer het account waar u machtigingen aan wilt toewijzen (hetzelfde account dat is opgegeven tijdens het instellen van de synchronisatie voor dat forest).
8. Selecteer in de vervolgkeuzelijst bovenaan de optie **Onderliggende gebruikersobjecten**.
9. In het dialoogvenster **Vermelding van machtiging** dat wordt weergegeven schakelt u de volgende selectievakjes in: **Wachtwoord opnieuw instellen**, **Wachtwoord wijzigen**, **Schrijfmachtigingen** voor `lockoutTime` en **Schrijfmachtigingen** voor `pwdLastSet`.

   ![][026]
   ![][027]
   ![][028]
10. Klik vervolgens op **Toepassen/OK** in alle open dialoogvensters.

### <a name="step-5-reset-your-ad-password-as-a-user"></a>Stap 5: uw AD-wachtwoord opnieuw instellen als gebruiker
Nu Wachtwoord terugschrijven is ingeschakeld, kunt u testen of dit werkt. Hiervoor stelt u het wachtwoord van een gebruiker opnieuw in waarvan het account is gesynchroniseerd met uw cloudtenant.

#### <a name="to-verify-password-writeback-is-working-properly"></a>Controleren of Wachtwoord terugschrijven goed werkt
1. Ga naar [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com) of naar een ander aanmeldingsscherm van een organisatie en klik op de koppeling **Geen toegang tot uw account?**.

   ![][029]
2. U krijgt nu een nieuwe pagina te zien waarop om de gebruikers-id wordt gevraagd waarvoor u het wachtwoord opnieuw wilt instellen. Voer uw testgebruikers-id in en volg de stappen voor het opnieuw instellen van het wachtwoord.
3. Wanneer u uw wachtwoord opnieuw hebt ingesteld, krijgt u een scherm te zien dat er ongeveer als volgt uitziet. Dit betekent dat u uw wachtwoord opnieuw hebt ingesteld voor uw on-premises en/of clouddirectory.

   ![][030]
4. Als u wilt controleren of de bewerking is voltooid of om fouten op te sporten, gaat u naar uw **Directory Sync-computer**, opent u **Logboeken**, gaat u naar het **toepassingsgebeurtenislogboek** en zoek naar de gebeurtenis **31002 - PasswordResetSuccess** van de bron **PasswordResetService** van uw testgebruiker.

   ![][031]



## <a name="next-steps"></a>Volgende stappen
Hieronder vindt u koppelingen naar alle Azure AD-documentatiepagina’s over wachtwoordherstel:

* **Bent u hier terechtgekomen omdat u problemen ondervindt met het aanmelden?** Als dat het geval is, vindt u hier meer informatie over het [wijzigen en opnieuw instellen van uw eigen wachtwoord](active-directory-passwords-update-your-own-password.md#how-to-reset-your-password).
* [**Hoe werkt het?**](active-directory-passwords-how-it-works.md): meer informatie over de zes verschillende onderdelen van de service en wat elke service biedt
* [**Aanpassen**](active-directory-passwords-customize.md): informatie over het aanpassen van de weergave en het gedrag van de service om aan de behoeften van uw organisatie te voldoen
* [**Aanbevolen procedures**](active-directory-passwords-best-practices.md): informatie over het snel implementeren en effectief beheren van wachtwoorden in uw organisatie
* [**Inzichten verkrijgen**](active-directory-passwords-get-insights.md): meer informatie over onze geïntegreerde rapportagemogelijkheden
* [**Veelgestelde vragen**](active-directory-passwords-faq.md): verkrijg antwoord op veelgestelde vragen
* [**Probleemoplossing**](active-directory-passwords-troubleshoot.md): informatie over het snel oplossen van problemen met de service
* [**Meer informatie**](active-directory-passwords-learn-more.md): meer informatie over de technische details van de werking van de service

[001]: ./media/active-directory-passwords-getting-started/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-getting-started/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-getting-started/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-getting-started/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-getting-started/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-getting-started/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-getting-started/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-getting-started/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-getting-started/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-getting-started/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-getting-started/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-getting-started/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-getting-started/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-getting-started/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-getting-started/015.jpg "Image_015.jpg"
[016]: ./media/active-directory-passwords-getting-started/016.jpg "Image_016.jpg"
[017]: ./media/active-directory-passwords-getting-started/017.jpg "Image_017.jpg"
[018]: ./media/active-directory-passwords-getting-started/018.jpg "Image_018.jpg"
[019]: ./media/active-directory-passwords-getting-started/019.jpg "Image_019.jpg"
[020]: ./media/active-directory-passwords-getting-started/020.jpg "Image_020.jpg"
[021]: ./media/active-directory-passwords-getting-started/021.jpg "Image_021.jpg"
[022]: ./media/active-directory-passwords-getting-started/022.jpg "Image_022.jpg"
[023]: ./media/active-directory-passwords-getting-started/023.jpg "Image_023.jpg"
[024]: ./media/active-directory-passwords-getting-started/024.jpg "Image_024.jpg"
[025]: ./media/active-directory-passwords-getting-started/025.jpg "Image_025.jpg"
[026]: ./media/active-directory-passwords-getting-started/026.jpg "Image_026.jpg"
[027]: ./media/active-directory-passwords-getting-started/027.jpg "Image_027.jpg"
[028]: ./media/active-directory-passwords-getting-started/028.jpg "Image_028.jpg"
[029]: ./media/active-directory-passwords-getting-started/029.jpg "Image_029.jpg"
[030]: ./media/active-directory-passwords-getting-started/030.jpg "Image_030.jpg"
[031]: ./media/active-directory-passwords-getting-started/031.jpg "Image_031.jpg"
[032]: ./media/active-directory-passwords-getting-started/032.jpg "Image_032.jpg"

