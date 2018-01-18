---
title: Selfservice voor wachtwoordherstel voor probleemoplossing - Azure Active Directory
description: Het oplossen van problemen Azure AD selfservice voor wachtwoordherstel
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: c038a9ec682a5971a5f79b9fe36e667493702cbd
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-self-service-password-reset"></a>Selfservice voor wachtwoordherstel oplossen

Zijn er een probleem met Azure Active Directory (Azure AD) selfservice voor wachtwoordherstel (SSPR)? De volgende informatie kunt u alles werkt het opnieuw.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-might-see"></a>Selfservice voor wachtwoordherstel opnieuw instellen van fouten die een gebruiker ziet oplossen

| Fout | Details | Technische details |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Het spijt ons, dat u het wachtwoord op dit moment niet herstellen omdat de beheerder voor wachtwoordherstel voor uw organisatie is uitgeschakeld. Er is geen verdere actie die u nemen kunt voor het oplossen van deze situatie. Neem contact op met uw beheerder en vraag of deze functie in te schakelen. Zie voor meer informatie, [hulp nodig hebt, ik ben mijn Azure AD-wachtwoord vergeten](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: We hebben vastgesteld dat wachtwoord opnieuw instellen is niet ingeschakeld door de beheerder. Neem contact op met uw beheerder en vraag ze om in te schakelen voor wachtwoordherstel voor uw organisatie. |
| WritebackNotEnabled = 10 |Het spijt ons, dat u kunt het wachtwoord op dit moment niet herstellen omdat de beheerder is niet ingeschakeld voor een vereiste service voor uw organisatie. Er is geen verdere actie die u nemen kunt voor het oplossen van deze situatie. Neem contact op met uw beheerder en vraag ze om te controleren van de configuratie van uw organisatie. Zie voor meer informatie over deze service nodig, [wachtwoord terugschrijven configureren](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-writeback#configure-password-writeback). | SSPR_0010: We hebben vastgesteld dat wachtwoord terugschrijven is niet ingeschakeld. Neem contact op met uw beheerder en vraag of wachtwoord terugschrijven inschakelen. |
| SsprNotEnabledInUserPolicy = 11 | Het spijt ons, dat u kunt het wachtwoord op dit moment niet herstellen omdat de beheerder is niet geconfigureerd voor wachtwoord opnieuw instellen voor uw organisatie. Er is geen verdere actie die u nemen kunt voor het oplossen van deze situatie. Neem contact op met uw beheerder en vraag ze om te configureren voor wachtwoordherstel. Zie voor meer informatie over het wachtwoord opnieuw instellen van configuratie [snel aan de slag: Azure AD selfservice voor wachtwoordherstel](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Uw organisatie is niet gedefinieerd voor een beleid voor wachtwoordherstel. Neem contact op met uw beheerder en vraag of voor het definiëren van een beleid voor wachtwoordherstel. |
| UserNotLicensed = 12 | Het spijt ons, dat u kunt het wachtwoord op dit moment niet herstellen omdat vereiste licenties ontbreekt in uw organisatie. Er is geen verdere actie die u nemen kunt voor het oplossen van deze situatie. Neem contact op met uw beheerder en vraag ze om te controleren van uw licentietoewijzing. Zie voor meer informatie over licentieverlening, [Licensing vereisten voor Azure AD zelf uw wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Uw organisatie heeft niet de vereiste licenties nodig zijn voor wachtwoordherstel. Neem contact op met uw beheerder en vraag ze om te controleren van het toewijzen van licenties. |
| UserNotMemberOfScopedAccessGroup = 13 | Het spijt ons, dat u kunt het wachtwoord op dit moment niet herstellen omdat de beheerder is niet geconfigureerd voor uw account wilt gebruiken voor wachtwoordherstel. Er is geen verdere actie die u nemen kunt voor het oplossen van deze situatie. Neem contact op met uw beheerder en vraag of uw account voor wachtwoordherstel te configureren. Zie voor meer informatie over de configuratie van de account voor wachtwoordherstel, [voor wachtwoordherstel voor gebruikers implementeert](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012: U geen lid bent van een groep die is ingeschakeld voor wachtwoordherstel. Neem contact op met uw beheerder en de aanvraag moet worden toegevoegd aan de groep. |
| UserNotProperlyConfigured = 14 | Het spijt ons, dat u kunt het wachtwoord op dit moment niet herstellen omdat de vereiste informatie ontbreekt in uw account is. Er is geen verdere actie die u nemen kunt voor het oplossen van deze situatie. Contact met u beheerder en vraag ze uw wachtwoord opnieuw instellen voor u. Nadat u toegang tot je account opnieuw hebt, moet u de benodigde informatie te registreren. Om te registreren gegevens, volg de stappen in de [registreren voor selfservice voor wachtwoordherstel](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register) artikel. | SSPR_0014: Extra beveiligingsgegevens is nodig om uw wachtwoord opnieuw instellen. Neem contact op met uw beheerder en vraag ze uw wachtwoord opnieuw instellen om door te gaan. Nadat u toegang tot je account hebt, kunt u aanvullende beveiligingsgegevens op https://aka.ms/ssprsetup registreren. De beheerder kan gegevens voor extra beveiliging toevoegen aan uw account door de stappen in [Set en lezen verificatiegegevens voor wachtwoordherstel](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired = 29 | Helaas is dat er op dit moment het wachtwoord niet herstellen vanwege een probleem met de configuratie van uw organisatie wachtwoord opnieuw instellen. Er is geen verdere actie die u nemen kunt voor het oplossen van deze situatie. Neem contact op met uw beheerder en vraag deze persoon te onderzoeken. Zie voor meer informatie over het eventuele probleem, [wachtwoord terugschrijven oplossen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: We kunnen uw wachtwoord vanwege een fout in uw on-premises-configuratie opnieuw instellen. Neem contact op met uw beheerder en vraag deze persoon te onderzoeken. |
| OnPremisesConnectivityError = 30 | Helaas is dat er op dit moment het wachtwoord niet herstellen wegens verbindingsproblemen zijn met uw organisatie. Er is geen actie te ondernemen nu, maar het probleem mogelijk worden opgelost als u later opnieuw proberen. Als het probleem zich blijft voordoen, neem contact op met uw beheerder en vraag deze persoon te onderzoeken. Zie voor meer informatie over problemen met de netwerkverbinding, [wachtwoord terugschrijven connectiviteit oplossen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Er kan geen uw wachtwoord opnieuw instellen als gevolg van een slechte verbinding met uw on-premises omgeving. Neem contact op met uw beheerder en vraag ze om te onderzoeken.|


## <a name="troubleshoot-the-password-reset-configuration-in-the-azure-portal"></a>De configuratie van wachtwoord opnieuw instellen in de Azure portal oplossen

| Fout | Oplossing |
| --- | --- |
| Ik zie niet in de **wachtwoordherstel** sectie onder Azure AD in de Azure portal. | Dit kan gebeuren als er geen een Azure AD Premium of Basic licentie is toegewezen aan de beheerder de bewerking wordt uitgevoerd. <br> <br> Een licentie toewijzen aan de desbetreffende administrator-account. U kunt de stappen in de [toewijzen, controleren en oplossen van problemen met licenties](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artikel.|
| De optie voor een specifieke configuratie wordt niet weergegeven. | Veel van de UI-elementen worden verborgen totdat ze nodig zijn. Kunt u het inschakelen van de opties die u wilt zien. |
| Ik zie niet in de **On-premises integratie** tabblad. | Deze optie wordt alleen weergegeven als u Azure AD Connect hebt gedownload en wachtwoord terugschrijven hebt geconfigureerd. Zie voor meer informatie [aan de slag met Azure AD Connect met behulp van de snelle instellingen](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Wachtwoord opnieuw instellen van reporting oplossen

| Fout | Oplossing |
| --- | --- |
| Ik zie niet alle typen wachtwoord management activiteiten in de **Self-Service wachtwoordbeheer** audit gebeurteniscategorie. | Dit kan gebeuren als er geen een Azure AD Premium of Basic licentie is toegewezen aan de beheerder de bewerking wordt uitgevoerd. <br> <br> U kunt dit probleem oplossen door een licentie toewijzen aan het beheerdersaccount in kwestie. Volg de stappen in de [toewijzen, controleren en oplossen van problemen met licenties](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artikel. |
| Registraties van de gebruiker weergeven meerdere keren. | Wanneer een gebruiker registreert, meld we op dit moment wordt elke afzonderlijke gegevensitem dat geregistreerd als een afzonderlijke gebeurtenis. <br> <br> Als u wilt deze gegevens aggregeert en hebt meer flexibiliteit in hoe u deze kunt bekijken, kunt u downloaden van het rapport en opent de gegevens als een draaitabel in Excel.

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Problemen met de registratieportal voor wachtwoordherstel

| Fout | Oplossing |
| --- | --- |
| De map is niet ingeschakeld voor wachtwoordherstel. **De beheerder heeft het gebruik van deze functie niet ingeschakeld.** | Switch de **selfservice voor wachtwoordherstel ingeschakeld** markering **geselecteerde** of **alle** en selecteer vervolgens **opslaan**. |
| De gebruiker beschikt niet over een Azure AD Premium of Basic licentie is toegewezen. **De beheerder heeft het gebruik van deze functie niet ingeschakeld.** | Dit kan gebeuren als er geen een Azure AD Premium of Basic licentie is toegewezen aan de beheerder de bewerking wordt uitgevoerd. <br> <br> U kunt dit probleem oplossen door een licentie toewijzen aan het beheerdersaccount in kwestie. Volg de stappen in de [toewijzen, controleren en oplossen van problemen met licenties](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artikel.|
| Er is een fout bij het verwerken van de aanvraag. | Dit kan worden veroorzaakt door veel problemen, maar doorgaans deze fout wordt veroorzaakt door een onderbreking van deze service of een configuratieprobleem. Als u deze fout te zien en heeft dit gevolgen voor uw bedrijf, moet u contact op met Microsoft ondersteuning voor assistentie. |

## <a name="troubleshoot-the-password-reset-portal"></a>Problemen met de portal voor wachtwoord opnieuw instellen

| Fout | Oplossing |
| --- | --- |
| De map is niet ingeschakeld voor wachtwoordherstel. | Switch de **selfservice voor wachtwoordherstel ingeschakeld** markering **geselecteerde** of **alle** en selecteer vervolgens **opslaan**. |
| De gebruiker beschikt niet over een Azure AD Premium of Basic licentie is toegewezen. | Dit kan gebeuren als er geen een Azure AD Premium of Basic licentie is toegewezen aan de beheerder de bewerking wordt uitgevoerd. <br> <br> Als u een licentie aan de desbetreffende administrator-account toewijzen, kunt u dit probleem oplossen. Volg de stappen in de [toewijzen, controleren en oplossen van problemen met licenties](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) artikel. |
| De map voor het wachtwoord opnieuw instellen is ingeschakeld, maar de gebruiker heeft een ontbrekend of onjuist gevormd verificatiegegevens. | Voordat u doorgaat, zorg ervoor dat de dat gebruiker heeft contact op met gegevens in het bestand in de map goed gevormd. Zie voor meer informatie [gegevens die worden gebruikt door Azure AD zelf uw wachtwoord opnieuw instellen](active-directory-passwords-data.md). |
| De map voor het wachtwoord opnieuw instellen is ingeschakeld, maar de gebruiker heeft slechts één stukje contact op met gegevens in het bestand als het beleid is ingesteld op het vereisen van twee verificatiemethoden. | Zorg ervoor dat de gebruiker ten minste twee contactmethoden correct is geconfigureerd heeft voordat u verdergaat. Een voorbeeld heeft zowel een mobiel telefoonnummer *en* een office-telefoonnummer. |
| De map voor het wachtwoord opnieuw instellen is ingeschakeld en de gebruiker correct is geconfigureerd, maar de gebruiker is niet bereikbaar. | Dit kan het gevolg zijn van een tijdelijke servicefout of als er zijn onjuiste contact op met gegevens die we niet correct worden gedetecteerd. <br> <br> Als de gebruiker 10 seconden wacht, 'opnieuw proberen' en 'contact op met uw beheerder' koppelingen worden weergegeven. Als de gebruiker selecteert 'Probeer het opnieuw', maar het opnieuw probeert de aanroep. Als de gebruiker selecteert 'contact op met uw beheerder', stuurt een formulier per e-mail naar hun beheerders aanvragen van een wachtwoord opnieuw instellen om te worden uitgevoerd voor die account. |
| De gebruiker ontvangt nooit de wachtwoordherstel SMS of telefoongesprek. | Dit is het resultaat van een onjuist telefoonnummer in de map. Zorg ervoor dat het telefoonnummer is in de notatie '+ CC xxxyyyzzzzXeeee'. <br> <br> Wachtwoordherstel ondersteunt geen extensies, zelfs als u een in de map opgeven. De extensies zijn verwijderd voordat de aanroep is verzonden. Een waarde zonder extensie te gebruiken of de uitbreiding integreren met het telefoonnummer in uw persoonlijke vertakking exchange (PBX). |
| Nooit krijgt de gebruiker het wachtwoord opnieuw instellen van e-mailbericht. | De meest voorkomende oorzaak voor dit probleem is dat het bericht is geweigerd door een spamfilter. Controleer uw spam, de map Ongewenste e-mail of verwijderde items voor het e-mailbericht. <br> <br> Zorg er ook voor dat u het juiste e-mailaccount voor het bericht bent controleren. |
| Ik heb een beleid voor wachtwoordherstel ingesteld, maar wanneer een beheerdersaccount voor wachtwoordherstel gebruikt, wordt dat beleid wordt niet toegepast. | Microsoft beheert en besturingselementen het administrator-wachtwoord opnieuw ingesteld beleid om te controleren of het hoogste niveau van beveiliging. |
| De gebruiker wordt voorkomen dat een wachtwoord opnieuw instellen te vaak in een dag geprobeerd. | We implementeren een automatische bandbreedtebeperking voorkomen dat gebruikers uit hun wachtwoord opnieuw instellen te vaak in een korte periode wilt. Beperking van deze gebeurtenis treedt op wanneer: <br><ul><li>De gebruiker probeert een telefoonnummer valideren 5 maal binnen een uur.</li><li>De gebruiker probeert te gebruiken van de beveiliging vragen gate 5 maal binnen een uur.</li><li>De gebruiker probeert een wachtwoord voor de dezelfde gebruikersaccount 5 maal binnen een uur opnieuw instellen.</li></ul>Dit probleem kunt oplossen, moet de gebruiker moet worden gewacht 24 uur na de laatste poging. De gebruiker kan vervolgens hun wachtwoord opnieuw instellen. |
| De gebruiker ziet een fout opgetreden bij het valideren van hun telefoonnummer. | Deze fout treedt op wanneer het telefoonnummer hebt ingevoerd komt niet overeen met het telefoonnummer in het bestand. Controleer of dat de gebruiker de volledige telefoonnummer, met inbegrip van de code gebied en land als ze proberen met een telefonische methode voor opnieuw instellen van wachtwoorden is ingevoerd. |
| Er is een fout bij het verwerken van de aanvraag. | Dit kan worden veroorzaakt door veel problemen, maar doorgaans deze fout wordt veroorzaakt door een onderbreking van deze service of een configuratieprobleem. Als u deze fout te zien en heeft dit gevolgen voor uw bedrijf, moet u contact op met Microsoft ondersteuning voor assistentie. |

## <a name="troubleshoot-password-writeback"></a>Wachtwoord terugschrijven oplossen

| Fout | Oplossing |
| --- | --- |
| Het wachtwoord opnieuw instellen-service lokale niet wordt gestart. Fout 6800 wordt weergegeven in het logboek voor toepassingsgebeurtenissen van de Azure AD Connect-machine. <br> <br> Nadat de voorbereiding, kunnen federatieve of wachtwoord-hash-gesynchroniseerde gebruikers hun wachtwoorden niet opnieuw instellen. | Wanneer u wachtwoord terugschrijven is ingeschakeld, roept de synchronisatie-engine de Write-back-bibliotheek voor het uitvoeren van de configuratie (voorbereiden) door te communiceren met de cloudservice voor onboarding. Eventuele fouten aangetroffen tijdens de voorbereiding of tijdens het starten van het Windows Communication Foundation (WCF)-eindpunt voor de resultaten van wachtwoord terugschrijven in fouten in het gebeurtenislogboek in uw Azure AD Connect-machine. <br> <br> Tijdens het opnieuw opstarten van de service Azure AD Sync (ADSync) als write-back is geconfigureerd, het WCF-eindpunt wordt gestart. Maar als het opstarten van het eindpunt is mislukt, wordt gebeurtenis 6800 geregistreerd en kan de synchronisatieservice starten. De aanwezigheid van deze gebeurtenis betekent dat het eindpunt van de Write-back van wachtwoord niet is opgestart. Details van het gebeurtenislogboek voor deze gebeurtenis 6800, samen met de vermeldingen die zijn gegenereerd door het onderdeel PasswordResetService gebeurtenislogboek geven aan waarom u het eindpunt niet starten. Bekijk deze fouten gebeurtenislogboek en probeer het opnieuw opstarten van de Azure AD Connect als write-back van wachtwoord nog steeds niet werkt. Als het probleem zich blijft voordoen, kunt u proberen uit te schakelen en wachtwoord terugschrijven opnieuw inschakelen.
| Wanneer een gebruiker probeert een wachtwoord opnieuw instellen of een account ontgrendelen met wachtwoord terugschrijven is ingeschakeld, mislukt de bewerking. <br> <br> Bovendien ziet u een gebeurtenis in het gebeurtenislogboek van Azure AD Connect dat bevat: "synchronisatie-Engine heeft een fout hr geretourneerd = 800700CE, bericht = de bestandsnaam of extensie is te lang ' nadat de ontgrendelingsbewerking heeft plaatsgevonden. | Het Active Directory-account voor Azure AD Connect vinden en het wachtwoord opnieuw instellen zodat deze niet meer dan 127 tekens bevat. Open vervolgens de **synchronisatieservice** van de **Start** menu. Blader naar **Connectors** en zoek de **Active Directory-Connector**. Selecteer deze en selecteer vervolgens **eigenschappen**. Blader naar de **referenties** pagina en voer het nieuwe wachtwoord. Selecteer **OK** om de pagina te sluiten. |
| De laatste stap van het installatieproces van Azure AD Connect ziet u een foutbericht dat aangeeft dat wachtwoord terugschrijven kan niet worden geconfigureerd. <br> <br> Het logboek voor toepassingsgebeurtenissen Azure AD Connect bevat fout 32009 met de tekst "Fout bij het ophalen auth-token." | Deze fout treedt op in de volgende twee gevallen: <br><ul><li>U hebt opgegeven dat een onjuist wachtwoord voor het account globale beheerder opgegeven aan het begin van het installatieproces van Azure AD Connect.</li><li>U hebt geprobeerd een federatieve gebruiker gebruiken voor het account globale beheerder opgegeven aan het begin van het installatieproces van Azure AD Connect.</li></ul> U kunt dit probleem oplossen door ervoor te zorgen dat u gebruikt geen gefedereerd account voor de globale beheerder die u hebt opgegeven aan het begin van het installatieproces. Zorg er ook voor dat het opgegeven wachtwoord juist is. |
| Het gebeurtenislogboek van de Azure AD Connect-machine bevat fout 32002 die is gegenereerd door het uitvoeren van PasswordResetService. <br> <br> Leest de volgende fout: "Fout bij verbinding maken met Service bus. De tokenprovider kan niet worden geleverd een beveiligingstoken." | Uw on-premises omgeving is niet verbinding kunnen maken met de Azure Service Bus-eindpunt in de cloud. Deze fout wordt meestal veroorzaakt door een firewallregel voor het blokkeren van een uitgaande verbinding aan een specifieke poort of web-adres. Zie [connectiviteit vereisten](./connect/active-directory-aadconnect-prerequisites.md) voor meer informatie. Nadat u deze regels hebt bijgewerkt, de Azure AD Connect-computer opnieuw op en wachtwoord terugschrijven moet aan de slag opnieuw. |
| Nadat u enige tijd kunnen federatieve of wachtwoord-hash-gesynchroniseerde gebruikers hun wachtwoorden niet opnieuw instellen. | In sommige gevallen kan de Write-back van wachtwoord service mislukken om te starten wanneer u Azure AD Connect opnieuw is opgestart. In dergelijke gevallen moet u eerst controleren of wachtwoord terugschrijven ingeschakelde lijkt on-premises worden. U kunt controleren met behulp van de Azure AD Connect-wizard of de PowerShell (Zie de vorige HowTos sectie). Als de functie ingeschakeld is, probeer in- of uitschakelen van de functie opnieuw via de gebruikersinterface of PowerShell. Als dit niet werkt, proberen een volledig verwijderen en opnieuw installeren van Azure AD Connect. |
| Federatieve of wachtwoord-hash-gesynchroniseerde gebruikers die hun wachtwoord opnieuw wilt zien een fout na het probeert te verzenden hun wachtwoord. De fout geeft aan dat er een serviceprobleem is. <br ><br> Naast dit probleem tijdens de bewerkingen voor wachtwoord opnieuw instellen, ziet u mogelijk een foutbericht dat de beheeragent in uw lokale gebeurtenislogboeken toegang werd geweigerd. | Als u deze fouten in het gebeurtenislogboek zien, controleert u of de Active Directory Management Agent (ADMA)-account dat is opgegeven in de wizard op het moment van de configuratie van de vereiste machtigingen voor write-back van wachtwoord heeft. <br> <br> Duren nadat deze machtiging is opgegeven, kan maximaal één uur om de machtigingen voor doorsijpelen omlaag de `sdprop` achtergrondtaak op de domeincontroller (DC). <br> <br> Voor wachtwoord opnieuw instellen om te werken, moet de machtiging worden geautoriseerd op de security descriptor van het gebruikersobject waarvan het wachtwoord opnieuw wordt ingesteld. Totdat deze machtiging wordt weergegeven op het gebruikersobject, blijft wachtwoordherstel mislukken met een toegang geweigerd-bericht. |
| Federatieve of wachtwoord-hash-gesynchroniseerde gebruikers die hun wachtwoord opnieuw wilt instellen er een foutbericht wanneer ze hun wachtwoord indienen. De fout geeft aan dat er een serviceprobleem is. <br> <br> Naast dit probleem tijdens de bewerkingen voor wachtwoord opnieuw instellen, ziet u mogelijk een fout in de gebeurtenislogboeken van de service Azure AD Connect is een fout 'Kan Object niet gevonden' waarmee wordt aangegeven. | Deze fout geeft meestal aan dat de synchronisatie-engine kan het gebruikersobject in de ruimte van Azure AD-connector of de gekoppelde metaverse (MV) of Azure AD-connector space-object vinden. <br> <br> U kunt dit probleem oplossen door ervoor te zorgen dat de gebruiker is inderdaad gesynchroniseerd van on-premises naar Azure AD via het huidige exemplaar van Azure AD Connect en de status van de objecten in de connectorspaces en MV controleren. Bevestig dat het object Active Directory Certificate Services (AD CS) is verbonden met het MV-object via de regel 'Microsoft.InfromADUserAccountEnabled.xxx'.|
| Federatieve of wachtwoord-hash-gesynchroniseerde gebruikers die hun wachtwoord opnieuw wilt instellen er een foutbericht wanneer ze hun wachtwoord indienen. De fout geeft aan dat er een serviceprobleem is. <br> <br> Naast dit probleem tijdens de bewerkingen voor wachtwoord opnieuw instellen, ziet u mogelijk een fout in de gebeurtenislogboeken van de Azure AD Connect-service die aangeeft dat er een 'Meerdere overeenkomsten gevonden'-fout is opgetreden. | Hiermee wordt aangegeven dat de synchronisatie-engine gedetecteerd dat het MV-object is verbonden met meer dan één AD CS-object via 'Microsoft.InfromADUserAccountEnabled.xxx'. Dit betekent dat de gebruiker een ingeschakeld-account in meer dan één forest heeft. Houd er rekening mee dat dit scenario wordt niet ondersteund voor write-back van wachtwoord. |
| Wachtwoord bewerkingen mislukken met een configuratiefout. Het logboek voor toepassingsgebeurtenissen bevat Azure AD Connect fout 6329 met de tekst '0x8023061f (de bewerking is mislukt omdat de Wachtwoordsynchronisatie is niet ingeschakeld op deze Agent Management)'. | Deze fout treedt op dat als de Azure AD Connect-configuratie voor het toevoegen van een nieuw Active Directory-forest (of als u wilt verwijderen en opnieuw toevoegen van een bestaand forest) is gewijzigd na de functie wachtwoord terugschrijven is al ingeschakeld. Wachtwoord bewerkingen voor gebruikers in deze forests mislukken onlangs hebt toegevoegd. U kunt het probleem oplossen door uitschakelen en de functie van de Write-back van wachtwoord vervolgens opnieuw inschakelen nadat de wijzigingen in de forest zijn voltooid. |

## <a name="password-writeback-event-log-error-codes"></a>Wachtwoord terugschrijven gebeurtenislogboek foutcodes

Er is een best practice bij het oplossen van problemen met terugschrijven van wachtwoorden wilt controleren van het logboek voor toepassingsgebeurtenissen op uw Azure AD Connect-computer. Dit logboek bevat gebeurtenissen die uit twee bronnen van belang voor write-back van wachtwoord. De bron PasswordResetService beschrijving van bewerkingen en problemen met betrekking tot de werking van wachtwoord terugschrijven. De ADSync bron beschrijft bewerkingen en problemen met betrekking tot het instellen van wachtwoorden in uw Active Directory-omgeving.

### <a name="if-the-source-of-the-event-is-adsync"></a>Als de bron van de gebeurtenis ADSync

| Code | Naam of het bericht | Beschrijving |
| --- | --- | --- |
| 6329 | ONDERSTEUNEN: MMS(4924) 0x80230619: "een beperking voorkomt u dat het wachtwoord wordt gewijzigd in de huidige versie opgegeven." | Deze gebeurtenis treedt op wanneer de Write-back van wachtwoord service probeert een wachtwoord instellen op uw lokale directory die niet aan de leeftijd van wachtwoord, geschiedenis, complexiteit of filteren van de vereisten van het domein voldoet. <br> <br> Als u de leeftijd van een minimumlengte van een wachtwoord hebt en het wachtwoord in dat venster tijd onlangs hebt gewijzigd, bent u niet kunt wijzigen van het wachtwoord opnieuw totdat de opgegeven leeftijd in uw domein is bereikt. De minimale ouderdom voor testdoeleinden moet worden ingesteld op 0. <br> <br> Als er vereisten voor wachtwoordgeschiedenis ingeschakeld en vervolgens moet u een wachtwoord dat niet is gebruikt in de laatste *N* tijden waar *N* is de instelling van de geschiedenis van wachtwoord. Als u een wachtwoord dat is gebruikt in de afgelopen selecteert *N* time-out, en vervolgens in dit geval er een fout. Voor testdoeleinden, moet de wachtwoordgeschiedenis worden ingesteld op 0. <br> <br> Als u de vereisten voor wachtwoordcomplexiteit hebt, worden deze allemaal afgedwongen wanneer de gebruiker probeert te wijzigen of opnieuw instellen van een wachtwoord. <br> <br> Als er wachtwoordfilters ingeschakeld en een gebruiker een wachtwoord selecteert die niet voldoet aan de filtercriteria, klikt u vervolgens het opnieuw instellen of wijzigingsbewerking is mislukt. |
| 6329 | MMS(3040): admaexport.cpp(2837): de server bevat geen LDAP-wachtwoordbeheer beleid. | Dit probleem treedt op als LDAP_SERVER_POLICY_HINTS_OID control (1.2.840.113556.1.4.2066) is niet ingeschakeld op de DC's. Voor het gebruik van de functie wachtwoord terugschrijven, moet u het besturingselement inschakelen. Om dit te doen, moet de DC's in Windows Server 2008 (met de meest recente SP) of later. Als uw DC's op 2008 (pre-R2 worden), dan moet u hotfix toepassen [KB2386717](http://support.microsoft.com/kb/2386717). |
| HR 8023042 | Synchronisatie-Engine heeft een fout hr = 80230402, bericht = een poging een object is mislukt omdat er dubbele vermeldingen met hetzelfde anker. | Deze fout treedt op wanneer dezelfde gebruikers-ID is ingeschakeld in meerdere domeinen. Een voorbeeld is als u account- en forests synchroniseren bent en dezelfde gebruikers-ID aanwezig en ingeschakeld in elk forest hebben. <br> <br> Deze fout kan ook optreden als u een niet-unieke ankerkenmerk gebruikt, zoals een alias of UPN en twee gebruikers die dezelfde ankerkenmerk delen. <br> <br> Zorg ervoor dat er geen dubbele gebruikers binnen uw domeinen en dat u een unieke ankerkenmerk voor elke gebruiker voor dit probleem. |

### <a name="if-the-source-of-the-event-is-passwordresetservice"></a>Als de bron van de gebeurtenis PasswordResetService

| Code | Naam of het bericht | Beschrijving |
| --- | --- | --- |
| 31001 | PasswordResetStart | Deze gebeurtenis geeft aan dat de on-premises service een aanvraag voor een federatieve of wachtwoord-hash-gesynchroniseerde gebruiker dat afkomstig van de cloud is voor wachtwoordherstel gedetecteerd. Deze gebeurtenis is de eerste gebeurtenis in elke bewerking Write-back van wachtwoord opnieuw instellen. |
| 31002 | PasswordResetSuccess | Deze gebeurtenis geeft aan dat een gebruiker een nieuw wachtwoord geselecteerd tijdens een bewerking voor wachtwoord opnieuw instellen. We bepaald dat dit wachtwoord voldoet aan zakelijke wachtwoordvereisten. Het wachtwoord is geschreven terug naar de lokale Active Directory-omgeving. |
| 31003 | PasswordResetFail | Deze gebeurtenis geeft aan dat een gebruiker een wachtwoord geselecteerd en het wachtwoord is ontvangen op de on-premises omgeving. Maar bij het instellen van het wachtwoord in de lokale Active Directory-omgeving, is een fout opgetreden. Deze fout kan zich voordoen om verschillende redenen: <br><ul><li>Wachtwoord van de gebruiker niet voldoen aan de leeftijd, geschiedenis, complexiteit, of filteren van de vereisten voor het domein. U lost dit probleem, moet u een nieuw wachtwoord maken.</li><li>De ADMA-serviceaccount heeft niet de juiste machtigingen voor het nieuwe wachtwoord instellen voor de desbetreffende gebruikersaccount.</li><li>Het gebruikersaccount is in een beveiligde groep, zoals de domein- of ondernemingsadministrator admin-groep, wat niet toegestaan door de set-bewerkingen voor wachtwoord.</li></ul>|
| 31004 | OnboardingEventStart | Deze gebeurtenis treedt op als u wachtwoord terugschrijven met Azure AD Connect inschakelen en we onboarding van uw organisatie met de webservice voor write-back van wachtwoord hebt gestart. |
| 31005 | OnboardingEventSuccess | Deze gebeurtenis geeft aan dat het voorbereidingsproces geslaagd is en de mogelijkheid van wachtwoord terugschrijven is klaar voor gebruik. |
| 31006 | ChangePasswordStart | Deze gebeurtenis geeft aan dat de on-premises service een wijzigingsaanvraag wachtwoord voor een federatieve of wachtwoord-hash-gesynchroniseerde gebruiker dat afkomstig van de cloud is gedetecteerd. Deze gebeurtenis is de eerste gebeurtenis in de terugschrijfbewerking voor elke wijziging van het wachtwoord. |
| 31007 | ChangePasswordSuccess | Deze gebeurtenis geeft aan dat een gebruiker een nieuw wachtwoord tijdens een wachtwoordwijziging geselecteerd, hebben we bepaald dat het wachtwoord voldoet aan de zakelijke wachtwoordvereisten en dat het wachtwoord is geschreven terug naar de lokale Active Directory-omgeving. |
| 31008 | ChangePasswordFail | Deze gebeurtenis geeft aan dat een gebruiker een wachtwoord geselecteerd en dat het wachtwoord is ontvangen op de on-premises-omgeving, maar bij het instellen van het wachtwoord in de lokale Active Directory-omgeving, is een fout opgetreden. Deze fout kan zich voordoen om verschillende redenen: <br><ul><li>Wachtwoord van de gebruiker niet voldoen aan de leeftijd, geschiedenis, complexiteit, of filteren van de vereisten voor het domein. U lost dit probleem, moet u een nieuw wachtwoord maken.</li><li>De ADMA-serviceaccount heeft niet de juiste machtigingen voor het nieuwe wachtwoord instellen voor de desbetreffende gebruikersaccount.</li><li>Het gebruikersaccount is in een beveiligde groep, zoals de domein- of enterprise-beheerders, die niet toegestaan door de set-bewerkingen voor wachtwoord.</li></ul> |
| 31009 | ResetUserPasswordByAdminStart | De on-premises service gedetecteerd dat een aanvraag voor een federatieve of wachtwoord-hash-gesynchroniseerde gebruiker die afkomstig zijn van de beheerder namens een gebruiker voor wachtwoordherstel. Deze gebeurtenis is de eerste gebeurtenis in elke bewerking Write-back van wachtwoord opnieuw instellen die door een beheerder is gestart. |
| 31010 | ResetUserPasswordByAdminSuccess | De beheerder een nieuw wachtwoord tijdens een bewerking voor wachtwoordherstel beheerder geïnitieerde geselecteerd. We bepaald dat dit wachtwoord voldoet aan zakelijke wachtwoordvereisten. Het wachtwoord is geschreven terug naar de lokale Active Directory-omgeving. |
| 31011 | ResetUserPasswordByAdminFail | De beheerder geselecteerd een wachtwoord namens een gebruiker. Het wachtwoord is aangekomen is naar de on-premises omgeving. Maar bij het instellen van het wachtwoord in de lokale Active Directory-omgeving, is een fout opgetreden. Deze fout kan zich voordoen om verschillende redenen: <br><ul><li>Wachtwoord van de gebruiker niet voldoen aan de leeftijd, geschiedenis, complexiteit, of filteren van de vereisten voor het domein. Probeer een nieuw wachtwoord voor dit probleem.</li><li>De ADMA-serviceaccount heeft niet de juiste machtigingen voor het nieuwe wachtwoord instellen voor de desbetreffende gebruikersaccount.</li><li>Het gebruikersaccount is in een beveiligde groep, zoals de domein- of enterprise-beheerders, die niet toegestaan door de set-bewerkingen voor wachtwoord.</li></ul>  |
| 31012 | OffboardingEventStart | Deze gebeurtenis treedt op als u terugschrijven van wachtwoorden met Azure AD Connect uitschakelen en geeft aan dat we offboarding van uw organisatie met de webservice voor write-back van wachtwoord gestart. |
| 31013| OffboardingEventSuccess| Deze gebeurtenis geeft aan dat het proces offboarding geslaagd is en dat wachtwoord terugschrijven mogelijkheid is uitgeschakeld. |
| 31014| OffboardingEventFail| Deze gebeurtenis geeft aan dat het proces offboarding niet gelukt is. Dit wordt mogelijk vanwege een machtigingsfout op de cloud of on-premises administrator-account opgegeven tijdens de configuratie. De fout kan ook optreden als u probeert een globale beheerder van de federatieve cloud gebruiken bij het wachtwoord terugschrijven uitschakelen. U kunt dit probleem, Controleer de beheermachtigingen en zorg ervoor dat u een federatieve-account niet gebruikt tijdens het configureren van de mogelijkheid van de Write-back van wachtwoord.|
| 31015| WriteBackServiceStarted| Deze gebeurtenis geeft aan dat het wachtwoord terugschrijven-service is gestart. Het is gereed om te accepteren wachtwoord management-aanvragen uit de cloud.|
| 31016| WriteBackServiceStopped| Deze gebeurtenis geeft aan dat de wachtwoord-Write-back-service is gestopt. Een wachtwoord management-aanvragen uit de cloud kunnen niet worden voltooid.|
| 31017| AuthTokenSuccess| Deze gebeurtenis geeft aan dat we een verificatietoken voor de globale beheerder opgegeven tijdens de installatie van Azure AD Connect de offboarding of onboarding-proces starten is opgehaald.|
| 31018| KeyPairCreationSuccess| Deze gebeurtenis geeft aan dat we de coderingssleutel van het wachtwoord is gemaakt. Deze sleutel wordt gebruikt voor het versleutelen van wachtwoorden vanuit de cloud worden verzonden naar uw on-premises omgeving.|
| 32000| UnknownError| Deze gebeurtenis geeft aan dat een onbekende fout opgetreden tijdens een bewerking voor het beheer van wachtwoord. Bekijk de tekst van de uitzondering in de gebeurtenis voor meer informatie. Als u problemen ondervindt, kunt u uitschakelen en Write-back van wachtwoord opnieuw in te schakelen. Als dit niet helpt, een kopie van het gebeurtenislogboek samen met de tracering opnemen ID opgegeven insider naar uw ondersteuningsmedewerker.|
| 32001| ServiceError| Deze gebeurtenis geeft aan er is een fout bij verbinding met het wachtwoord van de cloud-service opnieuw instellen. Deze fout treedt meestal op wanneer de on-premises service kon geen verbinding maken met de webservice voor wachtwoord opnieuw instellen.|
| 32002| ServiceBusError| Deze gebeurtenis geeft aan er is een fout opgetreden bij het verbinding maken met Service Bus-exemplaar van uw tenant. Dit kan gebeuren als u uitgaande verbindingen in uw on-premises omgeving blokkeren bent. Controleer uw firewall om ervoor te zorgen dat verbindingen via TCP 443 en naar https://ssprsbprodncu-sb.accesscontrol.windows.net/ toestaan en probeer het opnieuw. Als u steeds problemen ondervindt nog, kunt u uitschakelen en Write-back van wachtwoord opnieuw in te schakelen.|
| 32003| InPutValidationError| Deze gebeurtenis geeft aan dat de invoer doorgegeven aan onze API-webservice ongeldig is. Probeer het opnieuw.|
| 32004| DecryptionError| Deze gebeurtenis geeft aan dat er is een fout opgetreden bij het ontsleutelen van het wachtwoord dat is ontvangen vanuit de cloud. Dit wordt mogelijk vanwege een ontsleuteling sleutel komt niet overeen tussen de cloudservice en uw on-premises omgeving. U lost dit probleem, uitschakelen en terugschrijven van wachtwoorden in uw on-premises-omgeving opnieuw inschakelen.|
| 32005| ConfigurationError| Tijdens de voorbereiding opslaan we tenantspecifieke informatie in een configuratiebestand in uw on-premises omgeving. Deze gebeurtenis geeft aan dat er is een fout opgetreden bij het opslaan van dit bestand of dat wanneer de service is gestart, er is een fout bij het lezen van het bestand. U kunt dit probleem oplossen door probeert uit te schakelen en wachtwoord terugschrijven om af te dwingen een herschrijven van het configuratiebestand opnieuw in te schakelen.|
| 32007| OnBoardingConfigUpdateError| Tijdens de voorbereiding verzenden we gegevens vanuit de cloud naar on-premises wachtwoord opnieuw instellen. Deze gegevens vervolgens geschreven naar een bestand in het geheugen voordat deze wordt verzonden naar de synchronisatieservice worden veilig opgeslagen op schijf. Deze gebeurtenis geeft aan dat er een probleem met het schrijven van of bijwerken van die gegevens in het geheugen. U kunt dit probleem oplossen door probeert uit te schakelen en wachtwoord terugschrijven om af te dwingen een herschrijven van dit configuratiebestand opnieuw in te schakelen.|
| 32008| ValidationError| Deze gebeurtenis geeft aan dat we een ongeldige reactie ontvangen van de webservice voor wachtwoord opnieuw instellen. U kunt dit probleem oplossen door probeert uit te schakelen en Write-back van wachtwoord opnieuw in te schakelen.|
| 32009| AuthTokenError| Deze gebeurtenis geeft aan dat er kan niet een autorisatie-token voor het account globale beheerder is opgegeven tijdens de installatie van Azure AD Connect ophalen. Deze fout kan worden veroorzaakt door een onjuiste gebruikersnaam of wachtwoord dat is opgegeven voor het account globale beheerder. Deze fout kan ook optreden als het opgegeven account voor de globale beheerder is gefedereerd. U kunt dit probleem oplossen door de configuratie met de juiste gebruikersnaam en wachtwoord opnieuw uitvoeren en zorg ervoor dat de beheerder een beheerde account (alleen in de cloud of het wachtwoord is gesynchroniseerd).|
| 32010| CryptoError| Deze gebeurtenis geeft aan er is een fout opgetreden bij het genereren van de wachtwoordversleutelingssleutel of ontsleutelen van een wachtwoord dat uit de cloudservice binnenkomt. Deze fout waarschijnlijk duidt op een probleem met uw omgeving. Bekijk de details van het gebeurtenislogboek voor meer informatie over het oplossen van dit probleem. U kunt ook proberen uit te schakelen en de service van de Write-back van wachtwoord opnieuw in te schakelen.|
| 32011| OnBoardingServiceError| Deze gebeurtenis geeft aan dat de on-premises service correct kan niet met de webservice van wachtwoord opnieuw instellen communiceren voor het initiëren van het voorbereidingsproces. Dit kan gebeuren als gevolg van een firewallregel of als er een probleem opgetreden bij het ophalen van een verificatie-token voor uw tenant. U kunt dit probleem oplossen door ervoor te zorgen dat u bent niet worden geblokkeerd uitgaande verbindingen via TCP 443 en TCP 9350-9354 of naar https://ssprsbprodncu-sb.accesscontrol.windows.net/. Ook controleren of u wordt niet vrijgeven federatieve van Azure AD-admin-account.|
| 32013| OffBoardingError| Deze gebeurtenis geeft aan dat de on-premises service correct kan niet met de webservice van wachtwoord opnieuw instellen communiceren voor het initiëren van het proces offboarding. Dit kan gebeuren als gevolg van een firewallregel of als er een probleem opgetreden bij het ophalen van een vergunning token voor uw tenant. U kunt dit probleem oplossen door ervoor te zorgen dat u bent niet worden geblokkeerd uitgaande verbindingen via 443 of naar https://ssprsbprodncu-sb.accesscontrol.windows.net/ en dat de account voor Azure Active Directory-beheerder u voor offboard gebruikt is niet federatieve.|
| 32014| ServiceBusWarning| Deze gebeurtenis geeft aan dat we moesten opnieuw verbinding met uw tenant Service Bus-exemplaar te maken. Onder normale omstandigheden, mag dit geen probleem, maar als u deze gebeurtenis te in veel gevallen zien kunt u controleren of de netwerkverbinding met Service Bus, vooral als het een hoge latentie of een lage bandbreedte verbinding.|
| 32015| ReportServiceHealthError| Om de status van uw wachtwoord terugschrijven service bewaken, we heartbeat gegevens verzenden naar onze wachtwoordherstel webservice om de vijf minuten. Deze gebeurtenis geeft aan dat er een fout opgetreden is bij het verzenden van deze health-informatie terug naar de cloud-webservice. Deze informatie omvat geen een object-identificatiegegevens (OII) of de persoonsgegevens (PII) gegevens en is uitsluitend een heartbeat en statistieken van de basic-service zodat we de statusgegevens van de service in de cloud kunnen opgeven.|
| 33001| ADUnKnownError| Deze gebeurtenis geeft aan dat er een onbekende fout geretourneerd door Active Directory is. Controleer het gebeurtenislogboek voor Azure AD Connect-server voor gebeurtenissen van de bron ADSync voor meer informatie.|
| 33002| ADUserNotFoundError| Deze gebeurtenis geeft aan dat de gebruiker die probeert te stellen of te wijzigen van een wachtwoord niet in de on-premises directory gevonden is. Deze fout kan optreden als de gebruiker verwijderd op lokale is maar niet in de cloud. Deze fout kan ook optreden als er een probleem met het synchroniseren van is. Controleer uw synchronisatielogboeken en de laatste paar synchronisatie uitvoeren details voor meer informatie.|
| 33003| ADMutliMatchError| Wanneer u een wachtwoord opnieuw instellen of de wijzigingsaanvraag is afkomstig uit de cloud, gebruiken we het anker cloud is opgegeven tijdens de installatie van Azure AD Connect te bepalen die aanvraag weer aan een gebruiker in uw on-premises-omgeving koppelen. Deze gebeurtenis geeft aan dat er twee gebruikers in uw on-premises directory met hetzelfde kenmerk voor de cloud anker gevonden. Controleer uw synchronisatielogboeken en de laatste paar synchronisatie uitvoeren details voor meer informatie.|
| 33004| ADPermissionsError| Deze gebeurtenis geeft aan dat het Active Directory Management Agent (ADMA)-serviceaccount niet de juiste machtigingen hebben op het account in kwestie een nieuw wachtwoord instellen. Zorg ervoor dat de ADMA-account in de forest van de gebruiker is opnieuw ingesteld en machtigingen voor alle objecten in het forest wachtwoord wijzigen. Zie voor meer informatie over het instellen van de machtigingen stap 4: de juiste Active Directory-machtigingen instellen.|
| 33005| ADUserAccountDisabled| Deze gebeurtenis geeft aan dat er heeft geprobeerd te stellen of te wijzigen van een wachtwoord voor een account dat uitgeschakeld op lokale is. Het account inschakelen en probeer het opnieuw.|
| 33006| ADUserAccountLockedOut| Deze gebeurtenis geeft aan dat er heeft geprobeerd te stellen of te wijzigen van een wachtwoord voor een account dat lokale is vergrendeld. Vergrendelingen kunnen optreden wanneer een gebruiker heeft geprobeerd een wijziging of opnieuw instellen van wachtwoord opnieuw te vaak in een korte periode. Het account ontgrendelen en probeer het opnieuw.|
| 33007| ADUserIncorrectPassword| Deze gebeurtenis geeft aan dat de gebruiker een onjuist wachtwoord voor de huidige opgeven bij het uitvoeren van een wachtwoord bewerking wijzigen. Geef het juiste wachtwoord voor de huidige en probeer het opnieuw.|
| 33008| ADPasswordPolicyError| Deze gebeurtenis treedt op wanneer de Write-back van wachtwoord service probeert een wachtwoord instellen op uw lokale directory die niet aan de leeftijd van wachtwoord, geschiedenis, complexiteit of filteren van de vereisten van het domein voldoet. <br> <br> Als u de leeftijd van een minimumlengte van een wachtwoord hebt en het wachtwoord in dat venster tijd onlangs hebt gewijzigd, bent u niet kunt wijzigen van het wachtwoord opnieuw totdat de opgegeven leeftijd in uw domein is bereikt. De minimale ouderdom voor testdoeleinden moet worden ingesteld op 0. <br> <br> Als er vereisten voor wachtwoordgeschiedenis ingeschakeld en vervolgens moet u een wachtwoord dat niet is gebruikt in de laatste *N* tijden waar *N* is de instelling van de geschiedenis van wachtwoord. Als u een wachtwoord dat is gebruikt in de afgelopen selecteert *N* time-out, en vervolgens in dit geval er een fout. Voor testdoeleinden, moet de wachtwoordgeschiedenis worden ingesteld op 0. <br> <br> Als u de vereisten voor wachtwoordcomplexiteit hebt, worden deze allemaal afgedwongen wanneer de gebruiker probeert te wijzigen of opnieuw instellen van een wachtwoord. <br> <br> Als er wachtwoordfilters ingeschakeld en een gebruiker een wachtwoord selecteert die niet voldoet aan de filtercriteria, klikt u vervolgens het opnieuw instellen of wijzigingsbewerking is mislukt.|
| 33009| ADConfigurationError| Deze gebeurtenis geeft aan er is een probleem schrijven die een wachtwoord terug naar uw on-premises directory vanwege een configuratieprobleem met Active Directory. Controleer de Azure AD Connect-machine logboek voor toepassingsgebeurtenissen voor berichten van de service ADSync voor meer informatie op die de fout is opgetreden.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Wachtwoord terugschrijven connectiviteit oplossen

Als u serviceonderbrekingen met het onderdeel van de Write-back van wachtwoord van Azure AD Connect ondervindt, moet u hier enkele snelle stappen die u nemen kunt voor het oplossen van dit probleem zijn:

* [Controleer de netwerkverbinding](#confirm-network-connectivity)
* [De service Azure AD Connect-synchronisatie opnieuw starten](#restart-the-azure-ad-connect-sync-service)
* [Schakel uit en schakelt u de functie wachtwoord terugschrijven](#disable-and-re-enable-the-password-writeback-feature)
* [De meest recente versie van Azure AD Connect installeren](#install-the-latest-azure-ad-connect-release)
* [Wachtwoord terugschrijven oplossen](#troubleshoot-password-writeback)

In het algemeen als u wilt uw service op de snelste manier herstelt, wordt aangeraden dat u deze stappen in de volgorde die eerder besproken uitvoeren.

### <a name="confirm-network-connectivity"></a>Controleer de netwerkverbinding

Het meest voorkomende storingspunt of de firewall is en of proxy-poorten en niet-actieve time-outs zijn onjuist geconfigureerd. 

Voor Azure AD Connect versie 1.1.443.0 en hoger, u moet uitgaande HTTPS toegang tot het volgende:

   - passwordreset.microsoftonline.com
   - servicebus.windows.net

Verwijst naar de bijgewerkte lijst met meer granulatie [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653) elke woensdag bijgewerkt en de volgende tot stand is gebracht maandag.

Raadpleeg voor meer informatie de connectiviteit-vereisten in de [vereisten voor Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) artikel.



### <a name="restart-the-azure-ad-connect-sync-service"></a>De service Azure AD Connect-synchronisatie opnieuw starten

Los problemen met de netwerkconnectiviteit of andere tijdelijke problemen met de service door de Azure AD Connect-synchronisatie-service opnieuw te starten:

   1. Als beheerder, selecteer **Start** op de server met Azure AD Connect.
   2. Voer **services.msc** in het zoekveld en selecteer **Enter**.
   3. Zoek naar de **Microsoft Azure AD Sync** vermelding.
   4. Met de rechtermuisknop op de servicevermelding, selecteert u **opnieuw**, en wacht tot de bewerking te voltooien.

   ![De Azure AD Sync-service opnieuw starten][Service restart]

Deze stappen opnieuw verbinding met de cloudservice en los eventuele onderbrekingen die u mogelijk ondervindt. Als de ADSync-service opnieuw start uw probleem niet is opgelost, raden wij aan dat u probeert uit te schakelen en de functie van de Write-back van wachtwoord opnieuw inschakelen.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Schakel uit en schakelt u de functie wachtwoord terugschrijven

U lost problemen met de netwerkverbinding, uitschakelen en de functie wachtwoord terugschrijven opnieuw inschakelen:

   1. Open de wizard Azure AD Connect-configuratie als een beheerder.
   2. In **verbinding maken met Azure AD**, Voer uw referenties van de globale beheerder Azure AD.
   3. In **verbinding maken met AD DS**, Voer uw beheerdersreferenties AD Domain Services.
   4. In **een unieke id van uw gebruikers**, selecteer de **volgende** knop.
   5. In **optionele functies**, schakelt de **wachtwoord terugschrijven** selectievakje.
   6. Selecteer **volgende** via de resterende dialoogvenster's zonder iets te wijzigen totdat u de **klaar om te configureren** pagina.
   7. Zorg ervoor dat de **gereed voor configuratie van de pagina** toont de **wachtwoord terugschrijven** als optie **uitgeschakeld** en selecteer vervolgens de groene **configureren** knop uw wijzigingen.
   8. In **voltooid**, schakelt de **nu synchroniseren** optie en selecteer vervolgens **voltooien** om de wizard te sluiten.
   9. Open de wizard Azure AD Connect-configuratie.
   10. Herhaal stap 2-8, maar zorg ervoor dat u selecteert de **wachtwoord terugschrijven** kiezen op de **optionele functies** pagina opnieuw inschakelen van de service.

Deze stappen opnieuw verbinding met de cloudservice en los eventuele onderbrekingen die u mogelijk ondervindt.

Als het probleem blijft uitschakelen en de functie van de Write-back van wachtwoord opnieuw in te schakelen voordoen zich, wordt u aangeraden dat u Azure AD Connect opnieuw installeren.

### <a name="install-the-latest-azure-ad-connect-release"></a>De meest recente versie van Azure AD Connect installeren

Azure AD Connect opnieuw kunt oplossen, configuratie en problemen met de netwerkverbinding tussen de cloud-services en uw lokale Active Directory-omgeving.

Het is raadzaam dat u deze stap uitvoeren nadat u de eerste twee stappen dat eerder is beschreven probeert.

> [!WARNING]
> Als u de regels out-of-the-box-synchronisatie hebt aangepast *back-up voordat u doorgaat met de upgrade en handmatig implementeren nadat u klaar bent.*

   1. Download de nieuwste versie van Azure AD Connect uit de [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
   2. Omdat u Azure AD Connect al hebt geïnstalleerd, moet u een in-place upgrade voor uw Azure AD Connect-installatie bijwerken naar de nieuwste versie uitvoeren.
   3. Het gedownloade pakket uitvoeren en volg de aanwijzingen op het scherm instructies voor het bijwerken van uw Azure AD Connect-machine.

De vorige stappen moeten opnieuw verbinding met de cloudservice en los eventuele onderbrekingen die u mogelijk ondervindt.

Als u de nieuwste versie van de Azure AD Connect-server installeert uw probleem niet is opgelost, raden wij aan dat u probeert uit te schakelen en opnieuw in te schakelen wachtwoord terugschrijven als laatste stap nadat u de meest recente versie hebt geïnstalleerd.

## <a name="verify-that-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Controleer of Azure AD Connect heeft de vereiste machtiging voor write-back van wachtwoord

Azure AD Connect is de Active Directory vereist **wachtwoord opnieuw instellen** toestemming voor het terugschrijven van wachtwoorden uitvoeren. Als u wilt weten of Azure AD Connect de vereiste machtiging voor een opgegeven lokale Active Directory-gebruikersaccount heeft, kunt u de effectieve machtigingen van Windows-functie:

   1. Aanmelden bij de Azure AD Connect-server en start de **Synchronization Service Manager** door te selecteren **Start** > **synchronisatieservice**.
   2. Onder de **Connectors** tabblad, selecteert u de on-premises **Active Directory Domain Services** connector en selecteer vervolgens **eigenschappen**.  

   ![Effectieve machtiging - stap 2](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
  
   3. Selecteer in het pop-upvenster **verbinding maken met Active Directory-Forest** en noteer de **gebruikersnaam** eigenschap. Deze eigenschap is de AD DS-account gebruikt met Azure AD Connect directorysynchronisatie wordt uitgevoerd. Voor Azure AD Connect wachtwoord terugschrijven uitvoeren, de AD DS-account moet hebben opnieuw ingesteld wachtwoord machtiging.  
   
   ![Effectieve machtiging - stap 3](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
  
   4. Aanmelden bij een domeincontroller voor lokale en start de **Active Directory: gebruikers en Computers** toepassing.
   5. Selecteer **weergave** en zorg ervoor dat de **geavanceerde functies** optie is ingeschakeld.  
   
   ![Effectieve machtiging - stap 5](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
  
   6. Zoek naar de Active Directory-gebruikersaccount dat u wilt controleren. Met de rechtermuisknop op de accountnaam en selecteer **eigenschappen**.  
   
   ![Effectieve machtiging - stap 6](./media/active-directory-passwords-troubleshoot/checkpermission04.png) 

   7. In het pop-upvenster, gaat u naar de **beveiliging** tabblad en selecteer **Geavanceerd**.  
   
   ![Effectieve machtiging - stap 7](./media/active-directory-passwords-troubleshoot/checkpermission05.png) 
   
   8. In de **geavanceerde beveiligingsinstellingen voor de beheerder** pop-upvenster, gaat u naar de **effectieve toegang** tabblad.
   9. Selecteer **selecteert u een gebruiker**, selecteert u de AD DS-account gebruikt door Azure AD Connect (Zie stap 3) en selecteer vervolgens **effectieve toegang weergeven**.  
   
   ![Effectieve machtiging - stap 9](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
  
   10. Schuif naar beneden en zoekt u naar **wachtwoord opnieuw instellen**. Als de vermelding ingeschakeld is, heeft de AD DS-account machtigingen voor het herstellen van het wachtwoord van de geselecteerde Active Directory-gebruikersaccount.  
   
   ![Effectieve machtiging - stap 10](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Azure AD-forums

Als er een algemene vraag over Azure AD en Self-service voor wachtwoordherstel, kunt u vragen de community om hulp op de [Azure AD-forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Leden van de community opnemen engineers, productmanagers, MVP's en andere IT-professionals.

## <a name="contact-microsoft-support"></a>Neem contact op met Microsoft ondersteuning

Als u het antwoord op een probleem niet kunt vinden, zijn er altijd onze ondersteuningsteams beschikbaar om u te helpen verder.

We vragen zo gedetailleerd mogelijk te bieden bij het openen van een aanvraag goed om u te helpen. Deze gegevens omvatten:

* **Algemene beschrijving van de fout**: Wat is de fout? Wat is het gedrag op dat is opgemerkt? Hoe kunnen we de fout optreedt? Geef zo gedetailleerd mogelijk.
* **Pagina**: welke pagina was u op wanneer u de fout opgemerkt? De URL als u en een schermafbeelding van de pagina bevatten.
* **Ondersteuning voor code**: Wat is de ondersteuningscode die is gegenereerd toen de fout hebt gezien de gebruiker?
    * Deze code vindt de fout optreedt, en selecteer vervolgens de **code ondersteund** koppelen aan de onderkant van het scherm en de GUID die het resultaat van de ondersteuningstechnicus verzenden.

    ![Zoek de ondersteuningscode aan de onderkant van het scherm][Support code]

    * Als u op een pagina zonder de ondersteuningscode van een onder, F12 en zoek naar de SID en de CID selecteren en de twee resultaten verzenden naar de ondersteuningstechnicus.
* **Datum, tijd en tijdzone**: omvatten de exacte datum en tijd *met de tijdzone* die de fout is opgetreden.
* **Gebruikers-ID**: die de gebruiker die de fout hebt gezien is? Een voorbeeld is  *user@contoso.com* .
    * Is dit een federatieve gebruiker?
    * Is dit een wachtwoord-hash-gesynchroniseerde gebruiker?
    * Is dit een gebruiker alleen in de cloud?
* **Licentieverlening**: de gebruiker beschikt over een Azure AD Premium of Azure AD Basic licentie toegewezen?
* **Logboek voor toepassingsgebeurtenissen**: als u terugschrijven van wachtwoord en de fout in uw on-premises infrastructuur is, omvatten een gecomprimeerde kopie van het logboek voor toepassingsgebeurtenissen van de Azure AD Connect-server.



[Service restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "De Azure AD Sync-service opnieuw starten"
[Support code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "De ondersteuningscode bevindt zich in de rechterbenedenhoek van het venster"

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen bevatten aanvullende informatie over het wachtwoord opnieuw instellen via Azure AD:

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md)
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)
