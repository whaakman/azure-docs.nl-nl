---
title: Het oplossen van problemen tijdens het back-ups van SAP HANA met behulp van Azure Backup | Microsoft Docs
description: Deze handleiding wordt uitgelegd hoe u veelvoorkomende fouten oplossen tijdens het back-SAP HANA-databases met Azure Backup.
services: backup
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: pullabhk
ms.openlocfilehash: 33f1b4674aad35d55ab014c45cd73753533931cb
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514180"
---
# <a name="troubleshoot-back-up-of-sap-hana-server-on-azure"></a>Oplossen van een back-up van SAP HANA-Server op Azure

Dit artikel bevat informatie over probleemoplossing voor de beveiliging van SAP HANA-databases op Azure Virtual Machines. Voordat u doorgaat met het oplossen van problemen, moeten we eerst begrijpen enkele belangrijke punten over de machtigingen en instellingen.

## <a name="understanding-pre-requisites"></a>Informatie over vereisten

Als onderdeel van [vereisten](backup-azure-sap-hana-database.md#prerequisites), het script vóór registratie moet worden uitgevoerd op de virtuele machine waarop HANA is geïnstalleerd voor het instellen van de juiste machtigingen.

### <a name="setting-up-permissions"></a>Machtigingen instellen

Wat het script vóór registratie doet:

1. AZUREWLBACKUPHANAUSER in HANA-systeem maakt en voegt de vereiste rollen en machtigingen, zoals hieronder vermeld:
    - BEHEERDER van de DATABASE - nieuwe databases maken tijdens het terugzetten
    - LEZEN van de CATALOGUS: lezen van de back-catalogus
    - SAP_INTERNAL_HANA_SUPPORT – voor toegang tot enkele privé-tabellen
2. Sleutel toegevoegd aan Hdbuserstore voor HANA-invoegtoepassing voor alle bewerkingen (onderzoek van de database, back-up configureren, uitvoeren van back-up, herstel uitvoeren)
   
   - Om te bevestigen dat de sleutel voor maken, voert u de opdracht HDBSQL binnen de HANA-machine uit met SIDADM referenties:

    ``` hdbsql
    hdbuserstore list
    ```
    
    Uitvoer van de opdracht moet de sleutel {SID} {DBNAME} met de gebruiker worden weergegeven als 'AZUREWLBACKUPHANAUSER'.

> [!NOTE]
> Zorg ervoor dat u hebt een unieke set van bestanden SSFS onder het pad ' / usr/sap/{SID}/home/.hdb/ '. Er mag slechts één map onder dit pad.

### <a name="setting-up-backint-parameters"></a>Het instellen van BackInt parameters

Als een database is gekozen voor back-up, wordt de Azure Backup-service backInt parameters configureren op het niveau van de DATABASE.

- [catalog_backup_using_backint:true]
- [enable_accumulated_catalog_backup:false]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Zorg ervoor dat deze parameters zijn niet aanwezig op het niveau van de HOST. Host niveau parameters overschrijft deze parameters en ander gedrag kunnen veroorzaken dan verwacht.

## <a name="understanding-common-user-errors"></a>Informatie over veelvoorkomende gebruikersfouten

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Kan geen verbinding maken met HANA system.check die uw systeem actief en werkend is.| Azure Backup-service kan geen verbinding maken met HANA omdat HANA DB niet actief is. Of HANA wordt uitgevoerd, maar Azure Backup-service om verbinding te maken is niet toegestaan | Controleer als de HANA-DB/service niet actief is. Als HANA DB/service actief is, controleert u of alle machtigingen zijn geconfigureerd zoals is vermeld [hier](#setting-up-permissions). Als de sleutel ontbreekt, moet u het script vóór registratie voor het maken van een nieuwe sleutel opnieuw uitvoeren. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Gedetecteerde ongeldig Backint configuratie. Beveiliging stoppen en opnieuw configureren van de database.| De backInt-parameters zijn onjuist opgegeven voor Azure Backup. | Controleer de parameters zijn zoals vermeld [hier](#setting-up-backint-parameters). Als backInt op basis van parameters aanwezig in de HOST zijn en verwijder ze. Als parameters niet aanwezig op de HOST zijn, maar handmatig op het databaseniveau van een hebt gewijzigd, klikt u vervolgens terugkeren ze op de juiste waarden zoals hierboven vermeld. Of 'stop de beveiliging met behoud van gegevens' van Azure portal en 'back-up hervatten' opnieuw.|
