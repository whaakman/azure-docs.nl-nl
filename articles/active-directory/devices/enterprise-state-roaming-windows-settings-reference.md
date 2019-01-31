---
title: Windows 10 roaming naslaginformatie | Microsoft Docs
description: Een volledige lijst van alle instellingen die worden verplaatste of een back-up in Windows 10.
services: active-directory
keywords: Enterprise state roaming, windows-cloud
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: markvi
ms.openlocfilehash: ff607ed44f19e915b6ba94c1d217367d1012f062
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55300231"
---
# <a name="windows-10-roaming-settings-reference"></a>Naslaginformatie over roaminginstellingen voor Windows 10
Hier volgt een volledige lijst van alle instellingen die worden verplaatste of een back-up in Windows 10. 

## <a name="devices-and-endpoints"></a>Apparaten en -eindpunten
Zie de volgende tabel voor een overzicht van de apparaten en accounttypen die worden ondersteund door de synchronisatie, back-up en herstel framework in Windows 10.

| Accounttype en bewerking | Bureaublad | Mobiele telefoon |
| --- | --- | --- |
| Azure Active Directory: synchronisatie |Ja |Nee |
| Azure Active Directory: back-up/herstellen |Nee |Nee |
| Microsoft-account: synchronisatie |Ja |Ja |
| Microsoft-account: back-up/herstellen |Nee |Ja |

## <a name="what-is-backup"></a>Wat is er back-up?
Windows-instellingen in het algemeen standaard synchroniseren, maar sommige instellingen zijn alleen back-up gemaakt, zoals de lijst met geïnstalleerde toepassingen op een apparaat. Back-up is voor mobiele apparaten alleen en momenteel niet beschikbaar voor gebruikers van Enterprise State Roaming. Back-up maakt gebruik van een Microsoft-account en de instellingen en toepassingsgegevens worden opgeslagen in OneDrive. Als een gebruiker wordt synchronisatie op het apparaat met behulp van de app instellingen uitgeschakeld, wordt toepassingsgegevens die normaal gesproken wordt gesynchroniseerd het back-up alleen. Back-upgegevens kunnen alleen worden geopend via de herstelbewerking tijdens de eerste ervaring van de uitvoering van een nieuw apparaat. Back-ups kunnen worden uitgeschakeld via de instellingen voor apparaten, en kunnen worden beheerd en verwijderd via een van de gebruiker OneDrive-account.

## <a name="windows-settings-overview"></a>Overzicht van Windows-instellingen
De volgende voor Instellingengroepen zijn beschikbaar voor eindgebruikers in-of uitschakelen synchronisatie van instellingen op Windows 10-apparaten.

* Thema: bureaubladachtergrond, gebruikerstegel, positie van de taakbalk, enzovoort. 
* Instellingen voor Internet Explorer: browsegeschiedenis, hebt getypt, URL's, Favorieten, enzovoort. 
* Wachtwoorden: Windows Referentiebeheer, met inbegrip van Wi-Fi-profielen 
* Taalvoorkeuren: woordenlijst, systeem-taalinstellingen 
* Toegankelijkheid: narrator, Schermtoetsenbord Vergrootglas 
* Andere instellingen voor Windows: Zie de details van Windows-instellingen
* Instellingen voor Microsoft Edge-browser: Microsoft Edge-Favorieten, leeslijst en andere instellingen

![Uw instellingen synchroniseren](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

Synchroniseren van de instelling van de browser Microsoft Edge-groep (Favorieten, leeslijst) kan worden ingeschakeld of uitgeschakeld door eindgebruikers via Microsoft Edge-browser optie instellingen.

![Account](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Voor Windows 10 versie 1803 of hoger, Internet Explorer instellingsgroep (Favorieten, opgegeven URL's) synchroniseren kan worden ingeschakeld of uitgeschakeld door eindgebruikers tot en met de menuoptie instellingen voor Internet Explorer. 

![Instellingen](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Details van Windows-instellingen
In de volgende tabel, andere vermeldingen in de kolom instellingengroep verwijst in de instellingen die kunnen worden uitgeschakeld via instellingen > Accounts > uw instellingen synchroniseren > andere Windows-instellingen. 

Interne vermeldingen in de instellingengroep kolom verwijzen naar instellingen en apps die alleen kunnen worden gesynchroniseerd binnen de app zelf of door het uitschakelen van synchronisatie uit voor het hele apparaat met behulp van mobile device management (MDM) of instellingen voor Groepsbeleid kunnen worden uitgeschakeld.
Instellingen die niet kunnen worden gebruikt of sync behoren niet tot een groep.

| Instellingen | Bureaublad | Mobiele telefoon | Groep |
| --- | --- | --- | --- |
| **Accounts**: accountafbeelding |Synchronisatie |X |Thema |
| **Accounts**: andere Accountinstellingen |X |X | |
| **Geavanceerde mobiele breedband**: Internetverbinding delen netwerknaam (automatische detectie van mobiele Wi-Fi-hotspots via Bluetooth kunnen) |X |X |Wachtwoorden |
| **App-gegevens**: afzonderlijke apps kunnen synchroniseren van gegevens |back-up van synchronisatie |back-up van synchronisatie |Interne |
| **Lijst met Apps**: lijst met geïnstalleerde apps |X |Back-up |Overige |
| **Bluetooth**: alle Bluetooth-instellingen |X |X | |
| **Opdrachtprompt**: Command prompt 'Standaard'-instellingen |Synchronisatie |X |Interne |
| **referenties**: Referentiekluis |Synchronisatie |Synchronisatie |wachtwoord |
| **Datum, tijd en de regio**: automatische tijd (tijdsynchronisatie Internet) |Synchronisatie |Synchronisatie |language |
| **Datum, tijd en de regio**: 24-uurs notatie |Synchronisatie |X |language |
| **Datum, tijd en de regio**: datum en tijd |Synchronisatie |X |language |
| **Datum, tijd en de regio**: tijdzone | |X |language |
| **Datum, tijd en de regio**: zomer-en wintertijd |Synchronisatie |X |language |
| **Datum, tijd en de regio**: land/regio |Synchronisatie |X |language |
| **Datum, tijd en de regio**: eerste dag van week |Synchronisatie |X |language |
| **Datum, tijd en de regio**: regio-indeling (landinstellingen) |Synchronisatie |X |language |
| **Datum, tijd en de regio**: korte datumnotatie |Synchronisatie |X |language |
| **Datum, tijd en de regio**: lange datum |Synchronisatie |X |language |
| **Datum, tijd en de regio**: korte tijd |Synchronisatie |X |language |
| **Datum, tijd en de regio**: lange tijd |Synchronisatie |X |language |
| **Bureaublad en persoonlijke instellingen**: bureaublad thema (achtergrond, systeemkleur, geluiden standaard, schermbeveiliging) |Synchronisatie |X |Thema |
| **Bureaublad en persoonlijke instellingen**: diavoorstelling achtergrond |Synchronisatie |X |Thema |
| **Bureaublad en persoonlijke instellingen**: Taakbalkinstellingen (positie, automatisch verbergen, enz.) |Synchronisatie |X |Thema |
| **Bureaublad en persoonlijke instellingen**: schermindeling van het start |X |Back-up | |
| **Apparaten**: gedeelde printers u met verbonden bent |X |X |andere |
| **Microsoft Edge-browser**: leeslijst |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: Favorieten |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: top sites <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: URL's opgegeven <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: instellingen voor favorieten <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: de startknop weergeven <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: pop-ups blokkeren <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: mij vragen wat te doen met elke download <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: bieden wachtwoorden op te slaan <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: verzenden aanvragen niet volgen <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: opslaan van formulier vermeldingen <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: zoek- en site suggesties weergeven als ik typ <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: cookies voorkeur <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: beveiligde medialicenties opslaan op mijn apparaat sites kunt <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Microsoft Edge-browser**: schermlezer instellen <sup> [[1]](#footnote-1)</sup> |Synchronisatie |Synchronisatie |Interne |
| **Hoog Contrast**: In- of uitschakelen |Synchronisatie |X |Betere toegankelijkheid |
| **Hoog contrast**: Instellingen voor thema 's |Synchronisatie |X |Betere toegankelijkheid |
| **Internet Explorer**: tabbladen (URL en de titel) openen |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: leeslijst |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: opgegeven URL's |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: browsegeschiedenis |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: Favorieten |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: uitgesloten van URL's |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: startpagina |Synchronisatie |Synchronisatie |Internet Explorer |
| **Internet Explorer**: domein suggesties |Synchronisatie |Synchronisatie |Internet Explorer |
| **Toetsenbord**: gebruikers kunnen inschakelen/uitschakelen op het scherm op het toetsenbord |Synchronisatie |X |Betere toegankelijkheid |
| **Toetsenbord**: inschakelen sticky Ja (standaard uitgeschakeld) |Synchronisatie |X |Betere toegankelijkheid |
| **Toetsenbord**: filter sleutels inschakelen (standaard uitgeschakeld) |Synchronisatie |X |Betere toegankelijkheid |
| **Toetsenbord**: in-/ uitschakelen sleutels inschakelen (standaard uitgeschakeld) |Synchronisatie |X |Betere toegankelijkheid |
| **Internet Explorer**: domein taal: Chinees (CHS) QWERTY - inschakelen zelf leren |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - inschakelen van de dynamische candidate rangorde |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - char-set vereenvoudigd Chinees |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - char-set Traditional Chinese |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - fuzzy pinyin |Synchronisatie |Back-up |Taal |
| **Taal**: CHS QWERTY - fuzzy pairs |Synchronisatie |Back-up |Taal |
| **Taal**: CHS QWERTY - full pinyin |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - double pinyin |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - automatische correctie lezen |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - sleutel voor C/E-switch, shift |Synchronisatie |X |Taal |
| **Taal**: CHS QWERTY - C/E schakelen Ctrl-toets |Synchronisatie |X |Taal |
| **Taal**: CHS WUBI - modus voor één teken invoer |Synchronisatie |X |Taal |
| **Taal**: CHS WUBI - tonen de resterende codering van de kandidaat |Synchronisatie |X |Taal |
| **Taal**: CHS WUBI - geluid wanneer 4-coding ongeldig is |Synchronisatie |X |Taal |
| **Taal**: CHT Bopomofo - CJK Ext-A opnemen |Synchronisatie |X |Taal |
| **Taal**: Japanse IME - voorspellende typen en aangepaste woorden |Synchronisatie |Synchronisatie |Taal |
| **Taal**: Koreaans (KOR) IME |X |X |Taal |
| **Taal**: handschriftherkenning |X |X |Taal |
| **Taal**: taal profiel |Synchronisatie |Back-up |Taal |
| **Taal**: spellingcontrole - spelfouten automatisch corrigeren en markeren |Synchronisatie |Back-up |Taal |
| **Taal**: lijst met toetsenborden |Synchronisatie |Back-up |Taal |
| **Scherm vergrendelen**: alle scherminstellingen van het vergrendelen |X |X | |
| **Vergrootglas**: in- of uitschakelen (master in-/ uitschakelen) |X |X |Toegankelijkheid |
| **Vergrootglas**: tekenomkering kleur inschakelen of uitschakelen (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Vergrootglas**: bijhouden - Volg de toetsenbordfocus |Synchronisatie |X |Toegankelijkheid |
| **Vergrootglas**: bijhouden - volgt u de muisaanwijzer |Synchronisatie |X |Toegankelijkheid |
| **Vergrootglas**: start wanneer gebruikers zich aanmelden (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Muis**: de grootte van de muisaanwijzer wijzigen |Synchronisatie |X |andere |
| **Muis**: de kleur van de muisaanwijzer wijzigen |Synchronisatie |X |andere |
| **Muis**: alle andere instellingen |X |X | |
| **Verteller**: snel starten |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: gebruikers kunnen Narrator spreken inspiratie wijzigen |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: gebruikers kunnen inschakelen of uitschakelen Narrator hints voor algemene artikelen lezen (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: gebruikers kunnen inschakelen of uitschakelen of getypte tekst kunnen horen (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: gebruikers kunnen inschakelen of uitschakelen of getypte woorden kunnen horen (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: insert-cursor Narrator te volgen hebben (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: visual markeren van Verteller cursor inschakelen (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: afspelen van geluid hints (op standaard) |Synchronisatie |X |Toegankelijkheid |
| **Verteller**: sleutels in het schermtoetsenbord activeren wanneer u uw vinger (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Betere toegankelijkheid**: de dikte van de knipperende cursor instellen |Synchronisatie |X |Toegankelijkheid |
| **Betere toegankelijkheid**: achtergrondafbeeldingen te verwijderen (standaard uitgeschakeld) |Synchronisatie |X |Toegankelijkheid |
| **Uitschakelen en slaapstand**: alle instellingen |X |X | |
| **Persoonlijke instellingen scherm Start**: accent kleur (alleen phone) |X |Synchronisatie |Thema |
| **Typen**: woordenlijst |Synchronisatie |Back-up |Taal |
| **Typen**: AutoCorrectie verkeerd gespeld woord |Synchronisatie |Back-up |Taal |
| **Typen**: verkeerd gespelde woorden markeren |Synchronisatie |Back-up |Taal |
| **Typen**: suggesties weergeven tijdens het typen |Synchronisatie |Back-up |Taal |
| **Typen**: Voeg een spatie nadat ik een tekstsuggestie kiezen |Synchronisatie |Back-up |Taal |
| **Typen**: een periode nadat ik Dubbeltik op de SPATIEBALK toevoegen |Synchronisatie |Back-up |Taal |
| **Typen**: de eerste letter van elk zin een hoofdletter |Synchronisatie |Back-up |Taal |
| **Typen**: alle hoofdletters gebruiken wanneer ik Dubbeltik shift-toets |Synchronisatie |Back-up |Taal |
| **Typen**: sleutel geluid afspelen tijdens het typen |Synchronisatie |Back-up |Taal |
| **Typen**: persoonlijke gegevens van Schermtoetsenbord |Synchronisatie |Back-up |Taal |
| **Wi-Fi**: Wi-Fi-profielen (alleen WPA) |Synchronisatie |Synchronisatie |Wachtwoorden |

###### <a name="footnote-1"></a>Voetnoot 1
Minimaal ondersteunde versie van het besturingssysteem van Windows Creators Update (15063 bouwen). 

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht [enterprise state roaming overzicht](enterprise-state-roaming-overview.md).

