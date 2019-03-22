---
title: Azure multi-factor Authentication gebruiker verzamelen van gegevens - Azure Active Directory
description: Welke informatie wordt gebruikt om te verifiëren van gebruikers met Azure multi-factor Authentication?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0c22d4421aa984a9862f83b9be1095d548e5841
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314474"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Azure multi-factor Authentication-gebruiker gegevens verzamelen

Dit document wordt uitgelegd dat het zoeken naar gebruikersgegevens verzameld door de Azure multi-factor Authentication-Server (MFA-Server) en Azure MFA (cloudservice) in het geval dat u wilt verwijderen.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Gegevens die worden verzameld

MFA-Server, de NPS-extensie en de Windows Server 2016 Azure MFA AD FS-Adapter verzamelen en opslaan van de volgende informatie voor 90 dagen.

Verificatiepogingen (gebruikt voor rapportage en probleemoplossing):

- Tijdstempel
- Gebruikersnaam
- Voornaam
- Achternaam
- E-mailadres
- Gebruikersgroep
- Verificatiemethode (telefoongesprek, SMS-bericht, mobiele App, OATH-Token)
- Modus voor Telefoonoproep (Standard, PIN)
- Richting voor tekstberichten (één richting, twee richtingen)
- Modus voor SMS-bericht (OTP, OTP + PINCODE)
- Modus voor mobiele App (Standard, PIN)
- Modus OATH-Token (Standard, PIN)
- Authenticatietype
- Naam van toepassing
- De aanroep van de primaire landcode
- Aanroep van het primaire telefoonnummer
- De aanroep van de primaire extensie
- Primaire aanroep geverifieerd
- Primaire Gespreksresultaat
- Landcode de aanroep van de back-up
- Telefoonnummer van de back-aanroep
- De aanroep van de back-extensie
- Back-aanroep geverifieerd
- Back-up Gespreksresultaat
- Algemene geverifieerd
- Het uiteindelijke resultaat
- Resultaten
- Geverifieerd
- Resultaat
- Initiërende IP-adres
- Apparaten
- Apparaattoken
- Apparaattype
- Mobiele App-versie
- Versie van het besturingssysteem
- Resultaat
- Gebruikte controleren op meldingen

Activeringen (pogingen om een account in de mobiele Microsoft Authenticator-app te activeren):
- Gebruikersnaam
- Accountnaam
- Tijdstempel
- Resultaat van de activering Code ophalen
- Succes activeren
- Fout bij activeren
- Activering Status resultaat
- De naam van apparaat
- Apparaattype
- Appversie
- OATH-Token is ingeschakeld

Blokken (die wordt gebruikt om geblokkeerde status te bepalen en voor rapporten):

- Tijdstempel blokkeren
- Blokkeren door gebruikersnaam
- Gebruikersnaam
- Landnummer
- Telefoonnummer
- Indeling telefoonnummer
- Toestelnummer
- Opschonen van extensie
- Geblokkeerd
- Blokkeringsreden
- Timestamp van voltooiing
- Reden voor voltooiing
- Accountvergrendeling
- Fraudewaarschuwing
- Fraudewaarschuwing niet geblokkeerd
- Taal

Omleidingen (gebruikt voor het melden van):

- Bypass-Timestamp
- Bypass-seconden
- Bypass van gebruikersnaam
- Gebruikersnaam
- Landnummer
- Telefoonnummer
- Indeling telefoonnummer
- Toestelnummer
- Opschonen van extensie
- Bypass-reden
- Timestamp van voltooiing
- Reden voor voltooiing
- Bypass gebruikt

De wijzigingen (gebruikt voor het synchroniseren van wijzigingen van de gebruiker MFA-Server of AAD):

- Tijdstempel wijzigen
- Gebruikersnaam
- Nieuwe landcode
- Nieuw telefoonnummer
- Nieuw toestel
- Nieuwe alternatieve landcode
- Nieuwe back-telefoonnummer
- Nieuwe Backup-extensie
- Nieuwe pincode
- Wijziging pincode vereist
- Oude Apparaattoken
- Nieuw apparaattoken

## <a name="gather-data-from-mfa-server"></a>Verzamelen van gegevens van MFA-Server

Voor MFA-Server versie 8.0 of hoger beheerders het volgende proces alle gegevens voor gebruikers te exporteren:

- Meld u aan bij uw MFA-Server, gaat u naar de **gebruikers** tabblad, selecteert u de gebruiker in kwestie en klikt u op de **bewerken** knop. Schermopnamen (Alt-PrtScn) van elk tabblad voor de gebruiker hun huidige MFA-instellingen.
- Uitvoeren vanaf de opdrachtregel van de MFA-Server, de volgende opdracht wijzigen van het pad op basis van uw installatie `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` opgemaakt bestand voor het produceren van een JSON.
- Beheerders kunnen ook gebruik van de Web Service SDK GetUserGdpr bewerking als een optie voor het exporteren van alle MFA cloud service-gegevens die worden verzameld voor een bepaalde gebruiker of opnemen in een grotere rapportageoplossing.
- Search `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` en een back-ups voor '<username>' (inclusief de aanhalingstekens in de zoekopdracht) om te zoeken naar alle exemplaren van de record van de gebruiker wordt toegevoegd of gewijzigd.
   - Deze records kunnen hiervoor schakelt worden beperkt tot (maar niet opgeheven) **"Meld gebruikerswijzigingen"** in de MFA Server UX, sectie logboekregistratie, het tabblad logboekbestanden.
   - Als u syslog is geconfigureerd, en **"Meld gebruikerswijzigingen"** is ingeschakeld in de MFA Server UX, sectie logboekregistratie, Syslog-tabblad, en vervolgens de logboekvermeldingen worden verzameld bij syslog in plaats daarvan.
- Andere instanties van de gebruikersnaam in MultiFactorAuthSvc.log en andere MFA-Server-logboekbestanden met betrekking tot verificatie pogingen worden beschouwd als operationele en vormen van vermenigvuldigde tot de informatie die is opgegeven met behulp van MultiFactorAuthGdpr.exe exporteren of webservice-SDK GetUserGdpr.

## <a name="delete-data-from-mfa-server"></a>Gegevens uit de MFA-Server verwijderen

Uitvoeren vanaf de opdrachtregel van de MFA-Server, de volgende opdracht wijzigen van het pad op basis van uw installatie `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` alle MFA cloud service om informatie te verwijderen die worden verzameld voor deze gebruiker.

- Gegevens die zijn opgenomen in de uitvoer in realtime wordt verwijderd, maar het duurt maximaal 30 dagen voor gegevens van operationele of vormen van vermenigvuldigde moet volledig worden verwijderd.
- Beheerders kunnen ook de bewerking Web Service SDK DeleteUserGdpr gebruiken als een optie om alle MFA cloud service-informatie verzameld voor een bepaalde gebruiker te verwijderen of opnemen in een grotere rapportageoplossing.

## <a name="gather-data-from-nps-extension"></a>Verzamelen van gegevens van de NPS-extensie

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te maken van een aanvraag voor exporteren.

- MFA-informatie is opgenomen in de uitvoer, die duurt uren of dagen kwijt bent om te voltooien.
- Instanties van de gebruikersnaam in de AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh en AzureMfa/AuthZ/AuthZOptCh gebeurtenislogboeken worden beschouwd als de operationele en vormen van vermenigvuldigde naar de informatie in de uitvoer.

## <a name="delete-data-from-nps-extension"></a>Gegevens uit de NPS-extensie verwijderen

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te vragen voor Account dicht bij het verwijderen van alle MFA cloud servicegegevens worden verzameld voor deze gebruiker.

- Het duurt maximaal 30 dagen voor gegevens volledig worden verwijderd.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Verzamelen van gegevens van Windows Server 2016 AD FS-Adapter Azure MFA

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te maken van een aanvraag voor exporteren. 

- MFA-informatie is opgenomen in de uitvoer, die duurt uren of dagen kwijt bent om te voltooien.
- Instanties van de gebruikersnaam in de gebeurtenislogboeken van de AD FS tracering/Debug (indien ingeschakeld) worden beschouwd als de operationele en vormen van vermenigvuldigde naar de informatie in de uitvoer.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Gegevens uit Windows Server 2016 Azure MFA AD FS-Adapter verwijderen

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te vragen voor Account dicht bij het verwijderen van alle MFA cloud servicegegevens worden verzameld voor deze gebruiker.

- Het duurt maximaal 30 dagen voor gegevens volledig worden verwijderd.

## <a name="gather-data-for-azure-mfa"></a>Verzamelen van gegevens voor Azure MFA

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te maken van een aanvraag voor exporteren.

- MFA-informatie is opgenomen in de uitvoer, die duurt uren of dagen kwijt bent om te voltooien.

## <a name="delete-data-for-azure-mfa"></a>Verwijderen van gegevens voor Azure MFA

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te vragen voor Account dicht bij het verwijderen van alle MFA cloud servicegegevens worden verzameld voor deze gebruiker.

- Het duurt maximaal 30 dagen voor gegevens volledig worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

[Melden van MFA-Server](howto-mfa-reporting.md)
