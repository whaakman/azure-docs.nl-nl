---
title: Avere vFXT cluster afstemmen - Azure
description: Overzicht van aangepaste instellingen voor het optimaliseren van prestaties in Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 640c550e82c1b883970a3ea7a374a85989cd5e21
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633944"
---
# <a name="cluster-tuning"></a>Cluster afstemmen


De meeste vFXT clusters kunnen profiteren van aangepaste prestatie-instellingen. Deze instellingen helpen bij het cluster geschikt zijn voor uw specifieke werkstroom, gegevensset en hulpprogramma's. 

Deze aanpassing moet samen met een ondersteuningsmedewerker worden uitgevoerd, omdat dat normaal gesproken omvat het configureren van de functies die niet beschikbaar in het Configuratiescherm Avere.

Deze sectie wordt uitgelegd enkele van de aangepaste afstemmen die kan worden gedaan.

<!-- 
[ xxx keep or not? \/ research this xxx ]

> [!TIP]
> The VDBench utility can be helfpul in generating I/O workloads to test a vFXT cluster. Read [Measuring vFXT Performance](vdbench.md) to learn more.

-->

## <a name="general-optimizations"></a>Algemene optimalisaties

Deze wijzigingen kunnen worden aanbevolen op basis van kenmerken van de gegevensset of werkstroom-stijl.

* Als de werkbelasting schrijfintensief, verhoogt u de grootte van de schrijf-cache van de standaardwaarde van 20%. 
* Als de gegevensset veel kleine bestanden omvat, vergroot u de limiet voor het aantal van de cache van de cluster-bestand. 
* Als het werk omvat kopiëren of verplaatsen van gegevens tussen twee opslagplaatsen, het aantal threads die worden gebruikt voor het verplaatsen van gegevens aanpassen: 
  * Om te verkorten, kunt u het aantal parallelle threads gebruikt verhogen.
  * Als het back-end opslagvolume wordt overbelast, moet u mogelijk verkleinen het aantal parallelle threads die worden gebruikt.
* Als het cluster gegevens voor een core filer die gebruikmaakt van NFSv4-ACL's slaat, toegangsmodus opslaan in cache voor het stroomlijnen van bestandsverificatie voor bepaalde clients inschakelen

## <a name="cloud-nas-or-cloud-gateway-optimizations"></a>NAS in de cloud of in de cloud optimalisaties voor gateway

Als u wilt profiteren van hogere snelheden tussen de vFXT cluster en cloud-opslag in een cloud NAS of de gateway-scenario (waar het cluster vFXT NAS-stijl toegang tot een cloudcontainer biedt), wijzigen van instellingen zoals deze meer van uw vertegenwoordiger aanbevelen gegevens naar het opslagvolume doelbewust pushen uit de cache:

* Vergroot het aantal TCP-verbindingen tussen het cluster en de storage-container
* Verkleinen van de REST-time-outwaarde voor de communicatie tussen het cluster en de opslag schrijfbewerkingen eerder wordt uitgevoerd als ze niet direct worden voltooid  
* Het segment wordt vergroot zodat elk back-end segment overdrachten schrijven een segment 8 MB aan gegevens in plaats van 1 MB

## <a name="cloud-bursting-or-hybrid-wan-optimizations"></a>Cloudbursting of hybride WAN-optimalisatie

In een cloud bursting scenario of hybride opslag WAN-optimalisatie scenario (waar het cluster vFXT biedt integratie tussen de cloud en on-premises hardware opslag), kunnen het nuttig zijn deze wijzigingen:

* Het aantal TCP-verbindingen toegestaan tussen het cluster en de filer core verhogen
* Schakel de WAN-optimalisatie-instelling voor de externe core filer (deze instelling kan worden gebruikt voor een externe lokale filer of een cloud core filer in een andere Azure-regio).
* Verhoog de grootte van de TCP-Sockets (afhankelijk van de werkbelasting en prestatiebehoeften voldoen)
* Schakel de instelling "altijd forward" te verminderen van de bestanden toch toe in de cache (afhankelijk van de werkbelasting en prestatiebehoeften voldoen)

## <a name="help-optimizing-your-avere-vfxt-for-azure"></a>Help uw vFXT Avere optimaliseren voor Azure

Gebruik de procedure wordt beschreven in [hulp met uw systeem](avere-vfxt-open-ticket.md) contact opnemen met ondersteuningsmedewerkers over deze optimalisaties. 