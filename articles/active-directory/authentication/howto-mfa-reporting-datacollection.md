---
title: Azure multi-factor Authentication-gebruiker gegevens verzamelen
description: Welke informatie wordt gebruikt om te verifiëren van gebruikers door de Azure multi-factor Authentication?
services: multi-factor-authentication
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.service: multi-factor-authentication
ms.workload: identity
ms.topic: article
ms.date: 05/01/2018
ms.openlocfilehash: 2281a35d1616aa88b0c646fb96cb9f95c3272536
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264365"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Azure multi-factor Authentication-gebruiker gegevens verzamelen

Dit document wordt uitgelegd hoe u kunt zoeken naar gebruikersgegevens verzameld door de Azure multi-factor Authentication-Server (MFA-Server) en Azure MFA (Cloud-gebaseerde) in het geval u wilt verwijderen.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Gegevens die worden verzameld

MFA-Server, de NPS-uitbreiding en de Windows Server 2016 Azure MFA AD FS-Adapter verzamelen en opslaan van de volgende informatie voor 90 dagen.

Verificatiepogingen (gebruikt voor rapportage en probleemoplossing):

- Timestamp
- Gebruikersnaam
- Voornaam
- Achternaam
- E-mailadres
- Gebruikersgroep
- Verificatiemethode (telefoonoproep, SMS-bericht, mobiele App, OATH-Token)
- Telefoongesprekmodus (Standard, PIN)
- Richting voor tekstberichten (één richting, wederzijdse)
- Tekstberichtmodus (OTP OTP en PINCODE)
- Mobiele-Appmodus (Standard, PIN)
- Modus OATH-Token (Standard, PIN)
- Authenticatietype
- Toepassingsnaam
- De aanroep van de primaire landcode
- De aanroep van de primaire telefoonnummer
- De aanroep van de primaire-uitbreiding
- Primaire aanroep geverifieerd
- Primaire Oproepresultaat
- Back-aanroep landcode
- Telefoonnummer van de back-aanroep
- De aanroep van de back-extensie
- Back-aanroep geverifieerd
- Resultaat van de back-aanroep
- De algemene geverifieerd
- Het uiteindelijke resultaat
- Resultaten
- Geverifieerd
- Resultaat
- IP-adres starten
- Apparaten
- Apparaattoken
- Apparaattype
- Mobiele App-versie
- Versie van het besturingssysteem
- Resultaat
- Gebruikte controle voor melding

Activeringen (pogingen om een account in de Microsoft Authenticator mobiele app activeren):
- Gebruikersnaam
- Accountnaam
- Timestamp
- Activering Code resultaat
- Geslaagde activeren
- Fout activeren
- Statusresultaat van activering
- Apparaatnaam
- Apparaattype
- Appversie
- OATH-Token is ingeschakeld

Blokken (gebruikt om te bepalen van de geblokkeerde status en voor rapporten):

- Tijdstempel blokkeren
- Blok door gebruikersnaam
- Gebruikersnaam
- Landnummer
- Telefoonnummer
- Telefoonnummer geformatteerd
- Toestelnummer
- Extensie opschonen
- Geblokkeerd
- Blokkeringsreden
- De tijdstempel is voltooid
- Reden voor voltooiing
- Accountvergrendeling
- Fraudewaarschuwing
- Fraudewaarschuwing niet geblokkeerd
- Taal

Omleidingen (gebruikt voor het melden van):

- Tijdstempel overslaan
- Bypass-seconden
- Bypass door gebruikersnaam
- Gebruikersnaam
- Landnummer
- Telefoonnummer
- Telefoonnummer geformatteerd
- Toestelnummer
- Extensie opschonen
- Bypass-reden
- De tijdstempel is voltooid
- Reden voor voltooiing
- Bypass gebruikt

Wijzigingen (gebruikt voor het synchroniseren van wijzigingen van gebruikergegevens MFA-Server of AAD):

- Tijdstempel wijzigen
- Gebruikersnaam
- Nieuwe landcode
- Nieuw telefoonnummer
- Nieuw toestel
- Nieuwe Reservelandcode
- Nieuwe reservetelefoonnummer
- Nieuwe Backup-extensie
- Nieuwe pincode
- Wijziging pincode vereist
- Oude Apparaattoken
- Nieuw apparaattoken

## <a name="gather-data-from-mfa-server"></a>Verzamelen van gegevens van MFA-Server

Voor MFA-Server versie 8.0 of hoger beheerders het volgende proces om alle gegevens voor gebruikers te exporteren:

- Aanmelden bij uw MFA-Server, gaat u naar de **gebruikers** tabblad, selecteert u de betrokken gebruiker en klik op de **bewerken** knop. Schermafbeeldingen (Alt PrtScn) van elk tabblad hun huidige MFA-instellingen voor de gebruiker opgeven.
- Voer de volgende opdracht voor het wijzigen van het pad op basis van uw installatie vanaf de opdrachtregel van de MFA-Server `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` voor het produceren van een JSON geformatteerd bestand.
- Beheerders kunnen ook de bewerking Web Service SDK GetUserGdpr gebruiken als een optie om alle MFA cloud servicegegevens verzameld voor een bepaalde gebruiker te exporteren of opnemen in een grotere rapportageoplossing.
- Search `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` en een back-ups voor '<username>' (de aanhalingstekens in de zoekopdracht opnemen) om alle exemplaren van de gebruikersrecord wordt toegevoegd of gewijzigd te vinden.
   - Deze records kunnen worden beperkt (, maar niet geëlimineerd) uitschakelen door het **'gebruikerswijzigingen vastleggen'** in de MFA-Server UX, sectie logboekregistratie, tabblad logboekbestanden.
   - Als syslog is geconfigureerd, en **'gebruikerswijzigingen vastleggen'** is ingeschakeld in de MFA-Server UX, sectie logboekregistratie, tabblad Syslog, en vervolgens de logboekvermeldingen kunnen worden verzameld uit syslog in plaats daarvan.
- Andere exemplaren van de gebruikersnaam in MultiFactorAuthSvc.log en andere MFA-Server de logboekbestanden met betrekking tot verificatie pogingen worden beschouwd als operationeel is en de informatie die wordt geleverd met MultiFactorAuthGdpr.exe exporteren of Web Service SDK herhaaldelijke GetUserGdpr.

## <a name="delete-data-from-mfa-server"></a>Gegevens uit de MFA-Server verwijderen

Voer de volgende opdracht voor het wijzigen van het pad op basis van uw installatie vanaf de opdrachtregel van de MFA-Server `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` alle MFA cloud service om informatie te verwijderen die worden verzameld voor deze gebruiker.

- Gegevens die zijn opgenomen in de uitvoer in realtime worden verwijderd, maar het duurt maximaal 30 dagen voor de operationele of herhaaldelijke gegevens volledig worden verwijderd.
- Beheerders kunnen ook de bewerking Web Service SDK DeleteUserGdpr gebruiken als een optie om alle MFA cloud service informatie verzameld voor een bepaalde gebruiker te verwijderen of opnemen in een grotere rapportageoplossing.

## <a name="gather-data-from-nps-extension"></a>Verzamelen van gegevens van de NPS-uitbreiding

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om een aanvraag voor exporteren.

- MFA-informatie is opgenomen in de uitvoer, dit kan uren of dagen om te voltooien.
- Instanties van de gebruikersnaam in de AuthN-AzureMfa/AuthNOptCh, AuthZ-AzureMfa/AuthZAdminCh en AuthZ-AzureMfa/AuthZOptCh gebeurtenislogboeken worden beschouwd als operationeel is en herhaaldelijke naar de informatie in de export.

## <a name="delete-data-from-nps-extension"></a>Gegevens uit de NPS-extensie verwijderen

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te vragen voor Account dicht bij het verwijderen van alle MFA cloudservicegegevens verzameld voor deze gebruiker.

- Het duurt maximaal 30 dagen voor gegevens volledig worden verwijderd.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Verzamelen van gegevens van Windows Server 2016 Azure MFA AD FS-Adapter

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om een aanvraag voor exporteren. 

- MFA-informatie is opgenomen in de uitvoer, dit kan uren of dagen om te voltooien.
- Instanties van de gebruikersnaam in de gebeurtenislogboeken van AD FS tracering/Debug (indien ingeschakeld) worden beschouwd als operationeel is en herhaaldelijke naar de informatie in de export.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Gegevens uit Windows Server 2016 Azure MFA AD FS-Adapter verwijderen

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te vragen voor Account dicht bij het verwijderen van alle MFA cloudservicegegevens verzameld voor deze gebruiker.

- Het duurt maximaal 30 dagen voor gegevens volledig worden verwijderd.

## <a name="gather-data-for-azure-mfa"></a>Verzamelen van gegevens voor Azure MFA

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om een aanvraag voor exporteren.

- MFA-informatie is opgenomen in de uitvoer, dit kan uren of dagen om te voltooien.

## <a name="delete-data-for-azure-mfa"></a>Gegevens verwijderen voor Azure MFA

Gebruik de [Microsoft Privacy Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) om te vragen voor Account dicht bij het verwijderen van alle MFA cloudservicegegevens verzameld voor deze gebruiker.

- Het duurt maximaal 30 dagen voor gegevens volledig worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

[MFA-Server melden](howto-mfa-reporting.md)
