---
title: Azure Active Directory Identity Protection risico gebeurtenissen-verwijzing | Microsoft Docs
description: Azure Active Directory Identity Protection risicogebeurtenissen naar verwijzen.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d009022d4ec03990d3ed0321ebcb13c72c674ed8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58516549"
---
# <a name="azure-active-directory-identity-protection-risk-events-reference"></a>Azure Active Directory Identity Protection risicogebeurtenissen verwijzen naar

De meeste van de beveiliging terugdringen plaatsvinden als aanvallers toegang krijgen tot een omgeving door het stelen van de identiteit van een gebruiker. Verdachte identiteiten detecteren, is geen eenvoudige taak. Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodieken voor het detecteren van verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Elke gedetecteerde verdachte actie wordt opgeslagen in een record met de naam risicogebeurtenis.


## <a name="anonymous-ip-address"></a>Anoniem IP-adres

**Detectietype:** Realtime  
**Oude naam:** Aanmeldingen vanaf anonieme IP-adres


Dit type risicogebeurtenis geeft aanmeldingen vanaf anonieme IP-adres (bijvoorbeeld Tor browser, anonymizer VPN's).
Deze IP-adressen worden meestal gebruikt door actoren die hun aanmelding-telemetrie (IP-adres, locatie, apparaat, enzovoort) voor mogelijk kwade bedoelingen te verbergen.


## <a name="atypical-travel"></a>Ongewoon traject

**Detectietype:** Offline  
**Oude naam:** Onmogelijke reis naar ongewone locaties


Dit type risicogebeurtenis identificeert twee aanmeldingen die afkomstig zijn van geografisch verafgelegen locaties, waarbij ten minste een van de locaties mogelijk ook ongewone voor de gebruiker, krijgt het verleden gedrag. Tussen verschillende andere factoren rekening dit algoritme voor machine learning gehouden met de tijd tussen de twee aanmeldingen en de tijd die het voor de gebruiker voor een van de eerste locatie reis naar de tweede, waarmee wordt aangegeven dat een andere gebruiker is met behulp van dezelfde zou hebben geduurd de referenties.

Het algoritme wordt genegeerd voor de hand liggende 'valse positieven' bijdragen aan de voorwaarden onmogelijk traject, zoals VPN-verbindingen en de locaties die regelmatig worden gebruikt door andere gebruikers in de organisatie. Het systeem heeft een eerste leerperiode van de vroegste van 14 dagen of 10 aanmeldingen, waarbij het aanmelden gedrag van een nieuwe gebruiker leert.


## <a name="leaked-credentials"></a>Gelekte referenties

**Detectietype:** Offline  
**Oude naam:** Gebruikers van wie de referenties zijn gelekt


Dit type risicogebeurtenis geeft aan dat van de gebruiker geldige referenties zijn gelekt.
Bij cybercriminelen geldige wachtwoorden van legitieme gebruikers binnendringen, delen de criminelen vaak deze referenties. Dit gebeurt meestal door ze te posten openbaar op de donkere web of plakt u sites of door trading of de referenties op de zwarte markt verkopen. De Microsoft gelekte referenties service verkrijgt gebruikersnaam / wachtwoord paren van openbare en donkere websites bewaken en door te werken met:

- Onderzoekers

- Justitie en politie

- Beveiligingsteams bij Microsoft

- Andere vertrouwde bronnen

Wanneer de service gebruikersreferenties van de web-donker, plakken sites of de bovenstaande bronnen verkrijgt, worden ze gecontroleerd op basis van Azure AD-gebruikers huidige geldige referenties naar geldige overeenkomsten worden gevonden.


## <a name="malware-linked-ip-address"></a>Aan malware gekoppeld IP-adres

**Detectietype:** Offline  
**Oude naam:** Aanmeldingen vanaf geïnfecteerde apparaten


Dit type risicogebeurtenis geeft aanmeldingen vanaf IP-adressen die zijn geïnfecteerd met malware die bekend is actief communiceren met een bot-server. Dit wordt bepaald door de IP-adressen van het apparaat van de gebruiker op basis van IP-adressen die verbonden met een bot-server, zijn terwijl de bot-server actief is.


## <a name="unfamiliar-sign-in-properties"></a>Onbekende aanmeldingseigenschappen

**Detectietype:** Realtime  
**Oude naam:** Aanmeldingen vanaf onbekende locaties

Dit type risicogebeurtenis rekening gehouden met na aanmelding geschiedenis (IP, breedtegraad / lengtegraad en ASN) om te zoeken naar afwijkende aanmeldingen. Het systeem wordt informatie opgeslagen over de voorgaande locaties die worden gebruikt door een gebruiker en deze 'vertrouwde' locaties overweegt. De risicogebeurtenis wordt geactiveerd wanneer de aanmelding plaatsvindt vanaf een locatie die nog niet in de lijst met vertrouwde sites. Nieuwe gebruikers bevindt zich in 'learning modus' voor een bepaalde periode in welke eigenschappen voor onbekende aanmelden risicogebeurtenissen wordt uitgeschakeld terwijl het gedrag van de gebruiker door onze algoritmen voor meer informatie. De learning dynamisch is de duur van de modus en is afhankelijk van de op hoe lang het duurt voordat het algoritme voor het verzamelen van voldoende gegevens over van de gebruiker aanmelden patronen. De minimale duur is vijf dagen. Een gebruiker kunt teruggaan naar het learning-modus na een lange periode van inactiviteit. Aanmeldingen vanaf bekende apparaten en de locaties die geografisch dicht bij een vertrouwde locatie wordt ook negeert door het systeem. 

We ook uitvoeren deze detectie voor basisverificatie (of verouderde protocollen). Omdat deze protocollen geen moderne eigenschappen zoals client-ID hebt, is er beperkte prestatietelemetrie waarmee u fout-positieven. We raden onze klanten om te verplaatsen naar moderne verificatie.

