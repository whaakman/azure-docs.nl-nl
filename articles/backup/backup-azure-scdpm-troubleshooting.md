---
title: Problemen met System Center Data Protection Manager met Azure Backup | Microsoft Docs
description: Oplossen van problemen in System Center Data Protection Manager.
services: backup
documentationcenter: 
author: adigan
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;adigan
ms.openlocfilehash: 2244a39217f54eb5906d05990f19fc8ca2fdeb0e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Problemen met System Center Data Protection Manager

U vindt de meest recente Release-opmerkingen voor SC DPM [hier](https://docs.microsoft.com/en-us/system-center/dpm/dpm-release-notes?view=sc-dpm-2016).
De ondersteuningsmatrix voor de beveiliging kan worden gevonden [hier](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).

## <a name="replica-is-inconsistent"></a>De replica is inconsistent

Deze fout kan optreden voor diverse redenen zoals - taak maken van replica is mislukt, problemen met het wijzigingslogboek, fouten in de bitmap voor het filter op niveau van volume, bronmachine is onverwacht afgesloten, overloop van het synchronisatielogboek of de replica's echt inconsistent is. Volg deze stappen om dit probleem te verhelpen:
- Een consistentiecontrole handmatig uitvoeren of dagelijks consistentiecontrole plannen voor het verwijderen van de inconsistente status.
- Zorg ervoor dat u zich op de meest recente versie van de Server mal of System Center DPM
- Zorg ervoor dat automatische consistentiecontrole is ingeschakeld
- Probeer het opnieuw starten van de services vanaf een opdrachtprompt ('net stop dpmra' gevolgd door 'net start dpmra')
- Zorg ervoor dat de netwerk-verbindingen en bandbreedte vereisten wordt voldaan
- Controleer of de bronmachine onverwacht is afgesloten
- Zorg ervoor dat de schijf in orde is en er voldoende ruimte voor de replica
- Zorg ervoor dat er geen dubbele back-uptaak tegelijkertijd worden uitgevoerd

## <a name="online-recovery-point-creation-failed"></a>Online herstelpunt is mislukt

Volg deze stappen om dit probleem te verhelpen:
- Zorg ervoor dat u zich op de meest recente versie van Azure Backup Agent
- Maak handmatig een herstelpunt in het taakgebied beveiliging
- Zorg ervoor dat u een consistentiecontrole op gegevensbron uitvoeren
- Zorg ervoor dat de netwerk-verbindingen en bandbreedte vereisten wordt voldaan
- Gerepliceerde gegevens heeft inconsistente status. Maak een schijfherstelpunt van deze gegevensbron
- Zorg ervoor dat de replica is aanwezig zijn en niet ontbreekt
- Replica heeft onvoldoende ruimte om te maken van USN-logboek

## <a name="unable-to-configure-protection"></a>Kan geen beveiliging configureren

Deze fout wordt weergegeven als de DPM-server kan geen verbinding met de beveiligde server. Volg deze stappen om dit probleem te verhelpen:
- Zorg ervoor dat u op de meest recente versie van Azure Backup Agent
- Zorg dat er connectiviteit (firewall-netwerk/proxy) tussen uw DPM-server en de beveiligde server
- Als u SQL Server beveiligt, moet u controleren NT AUTHORITY\SYSTEM sysadmin vanuit Aanmeldingseigenschappen ingeschakeld

## <a name="this-server-is-not-registered-to-the-vault-specified-by-the-vault-credential"></a>Deze server is niet geregistreerd bij de kluis die is opgegeven bij de kluisreferentie

Deze fout treedt op wanneer het kluisreferentiebestand geselecteerd niet tot de Recovery Services-kluis die is gekoppeld aan de System Center DPM behoort / Azure Backup-Server waarop het herstel wordt uitgevoerd. Volg deze stappen om dit probleem te verhelpen:
- Download het kluisreferentiebestand van de Recovery Services-kluis waarop de System Center DPM / Azure Backup-Server is geregistreerd.
- Probeer de server met de kluis met behulp van de meest recente gedownloade kluisreferentiebestand te registreren.

## <a name="either-the-recoverable-data-is-not-available-or-the-selected-server-is-not-a-dpm-server"></a>Ofwel de herstelbare gegevens is niet beschikbaar of de geselecteerde server is niet een DPM-server
Deze fout treedt op wanneer er geen andere System Center DPM zijn / Azure Backup-Servers geregistreerd voor de Recovery Services-kluis of de servers nog niet de metagegevens zijn geüpload, of de geselecteerde server niet een System Center DPM is / Azure Backup-Server.
- Als er andere System Center DPM zijn / Azure back-up-Servers geregistreerd in de Recovery Services-kluis, zorg ervoor dat de meest recente Azure Backup-agent is geïnstalleerd.
- Als er andere System Center DPM zijn / Azure back-up-Servers geregistreerd in de Recovery Services-kluis, wacht u totdat een dag na de installatie van het herstelproces te starten. De nachtelijke taak uploadt u de metagegevens voor alle beveiligde back-ups in de cloud. De gegevens zijn beschikbaar voor herstel.

## <a name="the-encryption-passphrase-provided-does-not-match-with-passphrase-associated-with-the-following-server"></a>De opgegeven wachtwoordzin voor versleuteling komt niet overeen met de wachtwoordzin die bij de volgende server hoort

> [!NOTE]
>Als je vergeten/niet langer de wachtwoordzin voor versleuteling, klik is er geen optie om de gegevens te herstellen. De enige optie is de wachtwoordzin te genereren en gebruiken voor toekomstige back-upgegevens te versleutelen.
>
>

Deze fout wordt weergegeven wanneer de wachtwoordzin voor versleuteling gebruikt voor het versleutelen van de gegevens van de System Center DPM / gegevens met Azure Backup-Server die wordt hersteld komt niet overeen met de opgegeven wachtwoordzin voor versleuteling. De agent is niet de gegevens ontsleutelen. Daarom wordt het herstel mislukt. Volg deze stappen om dit probleem te verhelpen:
- Geef de exacte dezelfde wachtwoordzin voor versleuteling die zijn gekoppeld aan de System Center DPM / Azure Backup-Server waarvan gegevens worden hersteld. 
