---
title: Aanbevelingen voor beveiliging voor Azure Marketplace-installatiekopieën | Microsoft Docs
description: In dit artikel bevat aanbevelingen voor installatiekopieën die zijn opgenomen op de markt
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: barclayn
ms.openlocfilehash: 17372076f06cbaa833c437121a01f0dcaaf0e757
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243627"
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Aanbevelingen voor beveiliging voor Azure Marketplace-installatiekopieën

U wordt aangeraden dat elke oplossing aan de volgende aanbevelingen voor beveiliging-configuratie voldoet. Dit helpt het onderhouden van een hoog beveiligingsniveau voor installatiekopieën van de partner-oplossing in de Azure Marketplace.

Deze aanbevelingen kunnen ook nuttig zijn voor organisaties die geen afbeeldingen in de Azure marketplace. U wilt controleren, Windows en Linux-installatiekopie configuraties van uw bedrijf op basis van de richtlijnen die zijn gevonden in de volgende tabellen:

## <a name="open-source-based-images"></a>Open Source-installatiekopieën

|||
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Categorie**                                                 | **Selectievakje**                                                                                                                                                                                                                                                                              |
| Beveiliging                                                     | De meest recente beveiligingsupdate patches voor de Linux-distributie worden geïnstalleerd.                                                                                                                                                                                                              |
| Beveiliging                                                     | Richtlijnen voor het beveiligen van de VM-installatiekopie voor de specifieke Linux-distributie branche zijn gevolgd.                                                                                                                                                                                     |
| Beveiliging                                                     | Beperk de kwetsbaarheid voor aanvallen door te bewaren van minimale voetafdruk met alleen die nodig zijn Windows Server-functies, onderdelen, services en poorten netwerken.                                                                                                                                               |
| Beveiliging                                                     | Broncode en resulterende VM-installatiekopie voor schadelijke software gescand.                                                                                                                                                                                                                                   |
| Beveiliging                                                     | De VHD-installatiekopie bevat alleen de benodigde vergrendelde accounts waarvoor geen standaard wachtwoorden dat interactieve aanmelding leiden zou; Er is geen back-deuren.                                                                                                                                           |
| Beveiliging                                                     | Firewall-regels zijn uitgeschakeld, tenzij de toepassing functioneel afhankelijk is, zoals een firewall-apparaat.                                                                                                                                                                             |
| Beveiliging                                                     | Alle gevoelige informatie is verwijderd uit de VHD-installatiekopie, zoals test SSH-sleutels, bekende hosts-bestand, logboekbestanden en onnodige certificaten.                                                                                                                                       |
| Beveiliging                                                     | Het verdient aanbeveling dat LVM niet moet worden gebruikt.                                                                                                                                                                                                                                            |
| Beveiliging                                                     | Meest recente versies van de vereiste bibliotheken moeten worden opgenomen: </br> -OpenSSL v1.0 of hoger </br> -Python 2.5 of hoger (Python 2.6 + wordt sterk aanbevolen) </br> -Python pyasn1-pakket als nog niet is geïnstalleerd </br> -d.OpenSSL v 1.0 of hoger                                                                |
| Beveiliging                                                     | Bash-Shell geschiedenisvermeldingen moeten worden gewist                                                                                                                                                                                                                                             |
| Netwerken                                                   | SSH-server moet worden opgenomen in de standaardinstelling. SSH keep alive ingesteld op ' sshd config ' met de volgende optie: ' ClientAliveInterval 180 '                                                                                                                                                        |
| Netwerken                                                   | Installatiekopie mag geen aangepaste netwerkconfiguratie bevatten. De resolv.conf verwijderen: `rm /etc/resolv.conf`                                                                                                                                                                                |
| Implementatie                                                   | Meest recente Azure Linux Agent moet worden geïnstalleerd </br> -De agent moet worden geïnstalleerd met het RPM- of Deb-pakket.  </br> -U kunt ook het proces voor handmatige installatie, maar de installer-pakketten worden aanbevolen en de voorkeur. </br> -Als de agent handmatig installeren vanuit de GitHub-opslagplaats, kopieert u eerst de `waagent` van het bestand in `/usr/sbin` en uit te voeren (als root): </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Het configuratiebestand van de agent wordt geplaatst op `/etc/waagent.conf`.    |
| Implementatie                                                   | Zorgt ervoor dat Azure Support onze partners met de seriële console-uitvoer wanneer die nodig zijn en gepaste time-out voor OS-schijf koppelen vanuit de cloudopslag kan bieden. De installatiekopie moet de volgende parameters hebben toegevoegd aan de Kernel Boot Line: `console=ttyS0 earlyprintk=ttyS0 rootdelay=300` |
| Implementatie                                                   | Er is geen swap-partitie op de besturingssysteemschijf. Wisselen kan voor het maken van op de lokale bronschijf worden aangevraagd door de Linux-Agent.         |
| Implementatie                                                   | Het verdient aanbeveling dat een enkele hoofdpartitie voor de besturingssysteemschijf is gemaakt.      |
| Implementatie                                                   | 64-bits besturingssysteem alleen.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server-installatiekopieën

|||
|-------------| -------------------------|
| **Categorie**                                                     | **Selectievakje**                                                                                                                                                                |
| Beveiliging                                                         | Gebruik een beveiligde OS-basisinstallatiekopie. De VHD die wordt gebruikt voor de bron van elke afbeelding op basis van Windows-Server moet zijn van de installatiekopieën van Windows Server-besturingssysteem geleverd via Microsoft Azure. |
| Beveiliging                                                         | Installeer alle meest recente beveiligingsupdates.                                                                                                                                     |
| Beveiliging                                                         | Toepassingen moeten niet afhankelijk zijn van een op de namen van de gebruiker met beperkte rechten zoals Administrator, root en -beheerder.                                                                |
| Beveiliging                                                         | BitLocker-stationsversleuteling wordt niet ondersteund op de harde schijf van het besturingssysteem. BitLocker kan worden gebruikt op gegevensschijven met.                                                            |
| Beveiliging                                                         | De kwetsbaarheid voor aanvallen door te bewaren van minimale voetafdruk met alleen die nodig zijn Windows Server-functies, onderdelen, services en poorten ingeschakeld netwerken beperken.                         |
| Beveiliging                                                         | Broncode en resulterende VM-installatiekopie voor schadelijke software gescand.                                                                                                                     |
| Beveiliging                                                         | Stel de beveiligingsupdate van Windows Server-installatiekopieën voor automatisch bijwerken.                                                                                                                |
| Beveiliging                                                         | De VHD-installatiekopie bevat alleen de benodigde vergrendelde accounts waarvoor geen standaard wachtwoorden dat interactieve aanmelding leiden zou; Er is geen back-deuren.                             |
| Beveiliging                                                         | Firewall-regels zijn uitgeschakeld, tenzij de toepassing functioneel afhankelijk is, zoals een firewall-apparaat.                                                               |
| Beveiliging                                                         | Alle gevoelige informatie is verwijderd uit de VHD-installatiekopie. Bijvoorbeeld, moeten HOSTS-bestand, logboekbestanden en onnodige certificaten worden verwijderd.                                              |
| Implementatie                                                       | 64-bits besturingssysteem alleen.                            |
