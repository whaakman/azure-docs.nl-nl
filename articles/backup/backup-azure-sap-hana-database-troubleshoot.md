---
title: Problemen oplossen bij het maken van back-ups van SAP HANA-data bases met behulp van Azure Backup | Microsoft Docs
description: Hierin wordt beschreven hoe u veelvoorkomende fouten oplost die zich kunnen voordoen wanneer u Azure Backup gebruikt om back-ups te maken van SAP HANA-data bases.
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: pullabhk
ms.openlocfilehash: 221b669c141681749709d6a5a406c78499f21032
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465475"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Problemen met back-ups van SAP HANA-data bases in azure oplossen

Dit artikel bevat informatie over het oplossen van problemen met het maken van back-ups van SAP HANA-data bases op Azure virtual machines.

## <a name="prerequisites"></a>Vereisten

Zorg er als onderdeel van de [vereisten](backup-azure-sap-hana-database.md#prerequisites)voor dat het script voor de voorafgaande registratie is uitgevoerd op de virtuele machine waarop Hana is geïnstalleerd.

### <a name="setting-up-permissions"></a>Machtigingen instellen

Wat is het voor registratie script:

1. Maakt AZUREWLBACKUPHANAUSER in het HANA-systeem en voegt deze vereiste rollen en machtigingen toe:
    - DATABASE beheerder: Maak tijdens het terugzetten een nieuwe Db's.
    - CATALOGUS gelezen: voor het lezen van de back-catalogus.
    - SAP_INTERNAL_HANA_SUPPORT: voor toegang tot een paar persoonlijke tabellen.
2. Voegt een sleutel toe aan Hdbuserstore voor de HANA-invoeg toepassing voor het afhandelen van alle bewerkingen (database query's, herstel bewerkingen, het configureren en uitvoeren van back-ups).
   
   U kunt controleren of de sleutel is gemaakt door de opdracht HDBSQL uit te voeren op de HANA-computer met SIDADM-referenties:

    ``` hdbsql
    hdbuserstore list
    ```
    
    De uitvoer van de opdracht moet de sleutel {SID} {DBNAME} bevatten, waarbij de gebruiker als AZUREWLBACKUPHANAUSER wordt weer gegeven.

> [!NOTE]
> Zorg ervoor dat u een unieke set SSFS-bestanden hebt onder **/usr/sap/{sid}/Home/.HDB/** . Dit pad mag slechts één map bevatten.

### <a name="setting-up-backint-parameters"></a>BackInt-para meters instellen

Nadat een Data Base is gekozen voor back-up, configureert de Azure Backup-Service backInt-para meters op DATABASE niveau:

- [catalog_backup_using_backint: True]
- [enable_accumulated_catalog_backup: False]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Zorg ervoor dat deze para meters *niet* aanwezig zijn op het niveau van de host. Met para meters op hostniveau worden deze para meters overschreven en kan dit leiden tot onverwacht gedrag.

## <a name="common-user-errors"></a>Veelvoorkomende gebruikers fouten

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

data| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Kan geen verbinding maken met het HANA-systeem. Controleer of uw systeem actief is.| De Azure Backup-service kan geen verbinding maken met HANA omdat de HANA-data base niet beschikbaar is. Of HANA wordt uitgevoerd, maar staat niet toe dat de Azure Backup-service verbinding maakt. | Controleer of de HANA-data base of service niet beschikbaar is. Als de HANA-data base of-service actief is, controleert u of [alle machtigingen zijn ingesteld](#setting-up-permissions). Als de sleutel ontbreekt, voert u het script voor de voorafgaande registratie opnieuw uit om een nieuwe sleutel te maken. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Ongeldige Backint-configuratie gedetecteerd. Stop de beveiliging en configureer de data base opnieuw.| De backInt-para meters zijn onjuist opgegeven voor Azure Backup. | Controleer of [de para meters zijn ingesteld](#setting-up-backint-parameters). Als de backInt-para meters aanwezig zijn op de HOST, verwijdert u deze. Als de para meters niet aanwezig zijn op het niveau van de HOST, maar hand matig zijn gewijzigd op database niveau, moet u deze herstellen naar de juiste waarden zoals eerder beschreven. Of voer de **beveiliging stoppen uit en behoud back-upgegevens** van de Azure Portal en selecteer vervolgens **back-up hervatten**.|
