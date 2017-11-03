---
title: "Aanbevelingen voor beveiliging voor Azure Marketplace-installatiekopieën | Microsoft Docs"
description: "In dit artikel bevat aanbevelingen voor de installatiekopieën die zijn opgenomen op de markt"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: barclayn
ms.openlocfilehash: 4ae36f87c29975c82bb99f713893a9dc78a249e6
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Aanbevelingen voor beveiliging voor Azure Marketplace-installatiekopieën

We raden aan dat elke oplossing voldoet aan de volgende aanbevelingen voor de configuratie van beveiliging. Dit helpt onderhouden van een hoog niveau van beveiliging voor installatiekopieën van de partner-oplossing in Azure Marketplace.

Deze aanbevelingen kunnen ook nuttig voor organisaties die geen afbeeldingen in de Azure marketplace zijn. U wilt controleren, Windows en Linux-installatiekopie configuraties van uw bedrijf op basis van de richtlijnen in de onderstaande tabellen gevonden.

## <a name="open-source-based-images"></a>Open Source-installatiekopieën

|||
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Categorie**                                                 | **Selectievakje**                                                                                                                                                                                                                                                                              |
| Beveiliging                                                     | De meest recente beveiligingspatches voor de Linux-distributie zijn geïnstalleerd.                                                                                                                                                                                                              |
| Beveiliging                                                     | Industriële richtlijnen voor het beveiligen van de VM-installatiekopie voor de specifieke Linux-distributie zijn gevolgd.                                                                                                                                                                                     |
| Beveiliging                                                     | De kwetsbaarheid voor aanvallen met minimale footprint te houden met alleen de benodigde Windows Server-functies, onderdelen, services en poorten netwerken beperkt.                                                                                                                                               |
| Beveiliging                                                     | Broncode en resulterende VM-installatiekopie op schadelijke software gescand.                                                                                                                                                                                                                                   |
| Beveiliging                                                     | De VHD-installatiekopie bevat alleen de benodigde vergrendelde accounts waarvoor geen standaard wachtwoorden waarmee interactieve aanmelding; Er is geen back-deuren.                                                                                                                                           |
| Beveiliging                                                     | Firewallregels zijn uitgeschakeld, tenzij de toepassing functioneel afhankelijk is, zoals een firewall-apparaat.                                                                                                                                                                             |
| Beveiliging                                                     | Alle gevoelige informatie is verwijderd uit de VHD-installatiekopie, zoals test SSH-sleutels, bekend hosts-bestand, logboekbestanden en onnodige certificaten.                                                                                                                                       |
| Beveiliging                                                     | Het verdient aanbeveling LVM mag niet gebruikt.                                                                                                                                                                                                                                            |
| Beveiliging                                                     | Meest recente versies van de vereiste bibliotheken moeten worden opgenomen: </br> -OpenSSL v1.0 of hoger </br> -Python 2.5 of hoger (Python 2.6 + wordt ten zeerste aanbevolen) </br> -Python pyasn1 pakket indien nog niet geïnstalleerd </br> -d.OpenSSL v 1.0 of hoger                                                                |
| Beveiliging                                                     | Bash-Shell geschiedenisvermeldingen moeten worden gewist.                                                                                                                                                                                                                                             |
| Netwerken                                                   | SSH-server moet worden opgenomen in de standaardinstelling. SSH behouden actief is ingesteld op sshd config met de volgende optie: ClientAliveInterval 180                                                                                                                                                        |
| Netwerken                                                   | Installatiekopie mag geen eventuele aangepaste netwerkconfiguratie bevatten. Verwijder de resolv.conf:`rm /etc/resolv.conf`                                                                                                                                                                                |
| Implementatie                                                   | Meest recente Azure Linux Agent moet worden geïnstalleerd. </br> -De agent moet worden geïnstalleerd met behulp van het pakket RPM of Deb.  </br> -U kunt ook het proces voor handmatige installatie, maar de installer-pakketten zijn aanbevolen en de voorkeur. </br> -Als de agent handmatig installeren van de github-opslagplaats, kopieert u eerst de `waagent` van het bestand in `/usr/sbin` en worden uitgevoerd (als root): </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Het configuratiebestand van de agent wordt geplaatst op `/etc/waagent.conf`.    |
| Implementatie                                                   | Zorgt ervoor dat ondersteuning van Azure onze partners met de seriële console van uitvoer wanneer nodig en geef voldoende time-out voor OS-schijf koppelen van cloud-opslag kan bieden. Afbeelding moet de volgende parameters hebben toegevoegd aan de Kernel opstarten regel:`console=ttyS0 earlyprintk=ttyS0 rootdelay=300` |
| Implementatie                                                   | Er is geen partitie wisselen op de schijf met het besturingssysteem. Wisseling kan voor het maken van op de lokale resource schijf worden aangevraagd door de Linux-Agent.         |
| Implementatie                                                   | Het is raadzaam dat een partitie met één hoofdmap is gemaakt voor de besturingssysteemschijf.      |
| Implementatie                                                   | 64-bits besturingssysteem alleen.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server-installatiekopieën

|||
|-------------| -------------------------|
| **Categorie**                                                     | **Selectievakje**                                                                                                                                                                |
| Beveiliging                                                         | Gebruik een veilige basis installatiekopie van het besturingssysteem. De VHD die is gebruikt voor de bron van een installatiekopie op basis van Windows Server moet zijn van de installatiekopieën van het besturingssysteem van Windows Server geboden via Microsoft Azure. |
| Beveiliging                                                         | Alle meest recente beveiligingsupdates installeren.                                                                                                                                     |
| Beveiliging                                                         | Toepassingen mogen geen een afhankelijkheid op de namen van de gebruiker met beperkte rechten zoals beheerder, basis en -beheerder.                                                                |
| Beveiliging                                                         | BitLocker-stationsversleuteling wordt niet ondersteund op de harde schijf van het besturingssysteem. BitLocker kan worden gebruikt op gegevensschijven met.                                                            |
| Beveiliging                                                         | Beperk de kwetsbaarheid voor aanvallen met minimale footprint met alleen de benodigde Windows Server-functies, onderdelen, services en netwerken poorten ingeschakeld te houden.                         |
| Beveiliging                                                         | Broncode en resulterende VM-installatiekopie op schadelijke software gescand.                                                                                                                     |
| Beveiliging                                                         | Windows Server-installatiekopieën-beveiligingsupdate voor automatische updates ingesteld.                                                                                                                |
| Beveiliging                                                         | De VHD-installatiekopie bevat alleen de benodigde vergrendelde accounts waarvoor geen standaard wachtwoorden waarmee interactieve aanmelding; Er is geen back-deuren.                             |
| Beveiliging                                                         | Firewallregels zijn uitgeschakeld, tenzij de toepassing functioneel afhankelijk is, zoals een firewall-apparaat.                                                               |
| Beveiliging                                                         | Alle gevoelige informatie is verwijderd uit de VHD-installatiekopie. Bijvoorbeeld, moeten HOSTS-bestand, logboekbestanden en onnodige certificaten worden verwijderd.                                              |
| Implementatie                                                       | 64-bits besturingssysteem alleen.                            |
