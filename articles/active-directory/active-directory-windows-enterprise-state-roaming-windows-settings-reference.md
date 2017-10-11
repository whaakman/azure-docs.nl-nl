---
title: Windows 10 roaming naslaginformatie | Microsoft Docs
description: Een volledige lijst van alle instellingen die worden verplaatste of een back-up in Windows 10.
services: active-directory
keywords: Enterprise status tijdens roaming windows cloud
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 5082ed8d2f41e72fa979b978e2ac0b0840fdcdac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="windows-10-roaming-settings-reference"></a>Naslaginformatie over roaminginstellingen voor Windows 10
Hier volgt een volledige lijst van alle instellingen die worden verplaatste of een back-up in Windows 10. 

## <a name="devices-and-endpoints"></a>Apparaten en -eindpunten
Zie de volgende tabel voor een overzicht van de apparaten en accounttypen die worden ondersteund door de back-up, synchronisatie en herstel van framework in Windows 10.

| Accounttype en bewerking | Bureaublad | Mobiele telefoon |
| --- | --- | --- |
| Azure Active Directory: synchronisatie |Ja |Nee |
| Azure Active Directory: back-up/herstel |Nee |Nee |
| Microsoft-account: synchronisatie |Ja |Ja |
| Microsoft-account: back-up/herstel |Nee |Ja |

## <a name="what-is-backup"></a>Wat is het back-up?
Windows-instellingen in het algemeen standaard synchroniseren, maar sommige instellingen zijn alleen een back-up, zoals de lijst met geïnstalleerde toepassingen op een apparaat. Back-up is voor mobiele apparaten alleen en die momenteel niet beschikbaar voor gebruikers Enterprise State Roaming. Back-up maakt gebruik van een Microsoft-account en de instellingen en toepassingsgegevens opgeslagen in OneDrive. Als een gebruiker wordt synchronisatie op het apparaat met de app instellingen uitgeschakeld, wordt toepassingsgegevens die normaal gesproken wordt gesynchroniseerd het back-up alleen. Back-upgegevens kan alleen worden benaderd via de herstelbewerking tijdens de first-run experience van een nieuw apparaat. Back-ups kunnen worden uitgeschakeld via de instellingen voor apparaten en kan worden beheerd en verwijderd met de OneDrive-account van de gebruiker.

## <a name="windows-settings-overview"></a>Overzicht van Windows-instellingen
De volgende Instellingengroepen zijn beschikbaar voor eindgebruikers van synchronisatie van instellingen op Windows 10-apparaten in-of uitschakelen.

* Thema: bureaubladachtergrond, gebruikerstegel, taakbalk positie, enzovoort. 
* Instellingen voor Internet Explorer: browsegeschiedenis, hebt getypt, URL's, Favorieten, enzovoort. 
* Wachtwoorden: [Windows referentiekluis](https://technet.microsoft.com/library/jj554668.aspx), met inbegrip van Wi-Fi-profielen 
* Het taalvoorkeuren: woordenlijst, systeem-en taalinstellingen 
* Toegankelijkheid: narrator, Schermtoetsenbord Vergrootglas 
* Andere Windows-instellingen: Zie details van de Windows-instellingen

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-individual-sync-settings.png)

Rand browser instelling groep (Favorieten, leeslijst zijn vermeld) synchroniseren kan ingeschakeld of uitgeschakeld door eindgebruikers via Edge-browser optie instellingen.

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-sync-content.png)

## <a name="windows-settings-details"></a>Details van Windows-instellingen
In de volgende tabel andere vermeldingen in de kolom instellingengroep verwijst naar instellingen die kunnen worden uitgeschakeld door te gaan naar Instellingen > Accounts > uw instellingen synchroniseren > andere Windows-instellingen. 

Interne vermeldingen in de kolom instellingengroep verwijzen naar instellingen en apps die alleen wordt gesynchroniseerd binnen de app zelf of door het uitschakelen van de synchronisatie voor het hele apparaat met beheer van mobiele apparaten (MDM) of instellingen voor Groepsbeleid kunnen worden uitgeschakeld.
Instellingen die niet kunnen worden gebruikt of de synchronisatie wordt behoort niet tot een groep.

| Instellingen | Bureaublad | Mobiele telefoon | Groep |
| --- | --- | --- | --- |
| **Accounts**: accountafbeelding |Synchronisatie |X |Thema |
| **Accounts**: andere Accountinstellingen |X |X | |
| **Geavanceerde mobiele breedband**: gedeelde internetverbinding netwerknaam (automatische detectie van mobiele Wi-Fi-hotspots via Bluetooth kunnen) |X |X |Wachtwoorden |
| **App-gegevens**: afzonderlijke apps kunnen synchroniseren van gegevens |synchronisatie back-up |synchronisatie back-up |interne |
| **Lijst met Apps**: lijst met geïnstalleerde apps |X |Back-up |Overige |
| **Bluetooth**: alle Bluetooth-instellingen |X |X | |
| **Opdrachtprompt**: opdrachtprompt 'Standaard' instellingen |Synchronisatie |X | |
| **Referenties**: Referentiekluis van referenties |Synchronisatie |Synchronisatie |wachtwoord |
| **Datum, tijd en regio**: automatische tijd (tijdsynchronisatie Internet) |Synchronisatie |Synchronisatie |taal |
| **Datum, tijd en regio**: 24-uurs klok |Synchronisatie |X |taal |
| **Datum, tijd en regio**: datum en tijd |Synchronisatie |X |taal |
| **Datum, tijd en regio**: tijdzone | |X |taal |
| **Datum, tijd en regio**: wintertijd |Synchronisatie |X |taal |
| **Datum, tijd en regio**: land/regio |Synchronisatie |X |taal |
| **Datum, tijd en regio**: eerste dag van week |Synchronisatie |X |taal |
| **Datum, tijd en regio**: regio-indeling (landinstelling) |Synchronisatie |X |taal |
| **Datum, tijd en regio**: korte datumnotatie |Synchronisatie |X |taal |
| **Datum, tijd en regio**: lange datumnotatie |Synchronisatie |X |taal |
| **Datum, tijd en regio**: korte tijd |Synchronisatie |X |taal |
| **Datum, tijd en regio**: lange tijd |Synchronisatie |X |taal |
| **Bureaubladinstellingen**: bureaublad-thema (achtergrond, systeemkleur, geluiden standaard, schermbeveiliging) |Synchronisatie |X |Thema |
| **Bureaubladinstellingen**: diavoorstelling achtergrond |Synchronisatie |X |Thema |
| **Bureaubladinstellingen**: Taakbalkinstellingen (positie, automatisch verbergen, enz.) |Synchronisatie |X |Thema |
| **Bureaubladinstellingen**: indeling startscherm starten |X |Back-up | |
| **Apparaten**: gedeelde printers die u hebt verbonden met |X |X |andere |
| **Edge-browser**: lijst lezen |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: Favorieten |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: top sites <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: URL's hebt getypt <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: Favorieten balk instellingen <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: de knop Start weergeven <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: pop-ups blokkeren <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: mij vragen wat te doen met elke download <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: bieden wachtwoorden op te slaan <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: verzenden aanvragen niet volgen <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: opslaan van de invoer in formulieren <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: zoek- en suggesties weergeven tijdens het typen <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: cookies voorkeur <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: sites beveiligde medialicenties opslaan op mijn apparaat laten <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Edge-browser**: schermlezer instelling <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |interne |
| **Hoog Contrast**: in- of uitschakelen |Synchronisatie |X |betere toegankelijkheid |
| **Hoog contrast**: thema-instellingen |Synchronisatie |X |betere toegankelijkheid |
| **Internet Explorer**: open tabbladen (URL en titel) |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: lijst lezen |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: getypt URL's |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: browsegeschiedenis |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: Favorieten |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: uitgesloten van URL's |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: startpagina |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: suggesties voor domein |Synchronisatie |Synchronisatie |Internet Explorer |
| **Toetsenbord**: gebruikers kunnen inschakelen/uitschakelen Schermtoetsenbord |Synchronisatie |X |betere toegankelijkheid |
| **Toetsenbord**: inschakelen op een tijdelijke Ja (standaard uitgeschakeld) |Synchronisatie |X |betere toegankelijkheid |
| **Toetsenbord**: filter sleutels inschakelen (standaard uitgeschakeld) |Synchronisatie |X |betere toegankelijkheid |
| **Toetsenbord**: sleutels wisselknop inschakelen (standaard uitgeschakeld) |Synchronisatie |X |betere toegankelijkheid |
| **Internet Explorer**: domein talen: Chinees (CHS) QWERTY - inschakelen zelf leren |Synchronisatie |X |Taal |
| **Taal**: QWERTY CHS - enable dynamische candidate rangschikking |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - char-set vereenvoudigd Chinees |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - char-set traditioneel Chinees |Synchronisatie |X |Taal |
| **Taal**: QWERTY CHS - fuzzy pinyin |Synchronisatie |Back-up |Taal |
| **Taal**: QWERTY CHS - fuzzy paren |Synchronisatie |Back-up |Taal |
| **Taal**: QWERTY CHS - volledige pinyin |Synchronisatie |X |Taal |
| **Taal**: QWERTY CHS - dubbele pinyin |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - automatische correctie lezen |Synchronisatie |X |Taal |
| **Taal**: QWERTY CHS - sleutel voor C/E-switch, shift |Synchronisatie |X |Taal |
| **Taal**: QWERTY CHS - sleutel voor C/E-switch, Ctrl |Synchronisatie |X |Taal |
| **Taal**: CHS WUBI - invoermodus één teken |Synchronisatie |X |Taal |
| **Taal**: CHS WUBI - weergeven met de codering van de uiteindelijke resterende |Synchronisatie |X |Taal |
| **Taal**: CHS WUBI - piepgeluid laat horen wanneer het coderen van 4 is ongeldig |Synchronisatie |X |Taal |
| **Taal**: CHT Bopomofo - CJK Ext A opnemen |Synchronisatie |X |Taal |
| **Taal**: Japanse IME - voorspeld typen en aangepaste woorden |Synchronisatie |Synchronisatie |Taal |
| **Taal**: Koreaans (KOR) IME |X |X |Taal |
| **Taal**: handschriftherkenning |X |X |Taal |
| **Taal**: taal profiel |Synchronisatie |Back-up |Taal |
| **Taal**: spellingcontrole - typefouten AutoCorrectie en markeren |Synchronisatie |Back-up |Taal |
| **Taal**: lijst met toetsenborden |Synchronisatie |Back-up |Taal |
| **Scherm vergrendelen**: alle scherminstellingen vergrendelen |X |X | |
| **Vergrootglas**: in- of uitschakelen (master in-of uitschakelen) |X |X |Toegankelijkheid |
| **Vergrootglas**: omkering kleur inschakelen of uitschakelen (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Vergrootglas**: bijhouden - volgt u de toetsenbordfocus |Synchronisatie |X |Toegankelijkheid |
| **Vergrootglas**: bijhouden - volgt u de muisaanwijzer |Synchronisatie |X |Toegankelijkheid |
| **Vergrootglas**: starten wanneer gebruikers zich aanmelden (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Muis**: de grootte van de muisaanwijzer wijzigen |Synchronisatie |X |andere |
| **Muis**: de kleur van de muisaanwijzer wijzigen |Synchronisatie |X |andere |
| **Muis**: alle andere instellingen |X |X | |
| **Narrator**: snel starten |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: gebruikers kunnen Narrator spreken presentatie wijzigen |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: gebruikers kunnen inschakelen of uitschakelen Narrator lezen hints voor algemene items (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: gebruikers kunnen inschakelen of uitschakelen of ze getypte tekens kunnen horen (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: gebruikers kunnen inschakelen of uitschakelen of ze getypte woorden kunnen horen (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: insert cursor na Narrator hebben (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: Schakel visuele markering Narrator cursor (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: afspelen audio hints (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Narrator**: sleutels op het schermtoetsenbord activeren wanneer u uw vinger (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Betere toegankelijkheid**: de dikte van de knipperende cursor instellen |Synchronisatie |X |Toegankelijkheid |
| **Betere toegankelijkheid**: verwijderen van installatiekopieën van de achtergrond (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Stroom en slaapstand**: alle instellingen |X |X | |
| **Scherm personalisatie Start**: accent kleur (alleen phone) |X |Synchronisatie |Thema |
| **Typen**: woordenlijst |Synchronisatie |Back-up |Taal |
| **Typen**: AutoCorrectie gespelde woorden |Synchronisatie |Back-up |Taal |
| **Typen**: verkeerd gespelde woorden markeren |Synchronisatie |Back-up |Taal |
| **Typen**: suggesties weergeven tijdens het typen |Synchronisatie |Back-up |Taal |
| **Typen**: een spatie toegevoegd nadat ik een suggestie tekst kiezen |Synchronisatie |Back-up |Taal |
| **Typen**: een periode nadat ik double-tikt u op de SPATIEBALK toevoegen |Synchronisatie |Back-up |Taal |
| **Typen**: beginhoofdletter van elke zin |Synchronisatie |Back-up |Taal |
| **Typen**: hoofdletters gebruiken wanneer ik double-tikt u op shift-toets |Synchronisatie |Back-up |Taal |
| **Typen**: key geluid afspelen tijdens het typen |Synchronisatie |Back-up |Taal |
| **Typen**: personalisatiegegevens voor Schermtoetsenbord |Synchronisatie |Back-up |Taal |
| **Wi-Fi**: Wi-Fi-profielen (alleen WPA) |Synchronisatie |Synchronisatie |Wachtwoorden |

###### <a name="footnote-1"></a>Voetnoot 1
Minimaal ondersteunde versie van het besturingssysteem van Windows auteurs Update (15063 bouwen). 

## <a name="related-topics"></a>Verwante onderwerpen
* [Overzicht van zwervende Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
* [Enterprise-status roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md)
* [Instellingen en veelgestelde vragen voor dataroaming](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Groep beleid en de MDM-instellingen voor synchronisatie van instellingen](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Problemen oplossen](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
