---
title: Diagnose van prestatieproblemen van afbeeldingen in extern bureaublad - Azure
description: In dit artikel wordt beschreven hoe u RemoteFX grafische tellers in Remote remote desktop protocol-sessies gebruiken voor het vaststellen van prestatieproblemen met afbeeldingen in een virtuele Windows-bureaublad.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 05/23/2019
ms.author: v-chjenk
ms.openlocfilehash: 0b4113f1e0024415135aa99d1fb4e881efe448a3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66499262"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>Prestatieproblemen van afbeeldingen in extern bureaublad

Wanneer het systeem niet werken zoals verwacht, is het belangrijk dat u de bron van het probleem te identificeren. Dit artikel helpt u identificeren en oplossen van knelpunten met betrekking tot afbeeldingen tijdens Remote Desktop Protocol (RDP)-sessies.

## <a name="find-your-remote-session-name"></a>De naam van uw externe sessie worden gevonden

U moet de naam van uw externe sessie voor het identificeren van de prestatiemeteritems van afbeeldingen. Volg de instructies in deze sectie om de naam van de externe sessie Windows Virtual Desktop Preview te identificeren.

1. De Windows-opdrachtprompt openen vanuit de externe sessie.
2. Voer de **qwinsta** opdracht.
    - Als uw sessie wordt gehost in een sessie met meerdere virtuele machine (VM): Het achtervoegsel voor de naam van het prestatiemeteritem is het achtervoegsel van de dezelfde in de sessienaam van uw, zoals "rdp-TCP-37."
    - Als uw sessie wordt gehost in een virtuele machine die ondersteuning biedt voor virtual Graphics Processing Units (vGPU): De items die zijn opgeslagen op de server in plaats van in uw virtuele machine. De teller instanties omvatten de naam van de VM in plaats van het nummer in de sessienaam van de, zoals ' Win8 Enterprise VM. "

>[!NOTE]
> Hoewel tellers RemoteFX in hun namen hebben, bevatten ze grafisch voor extern bureaublad in ook vGPU-scenario's.

## <a name="access-performance-counters"></a>Prestatiemeteritems voor toegang

Prestatiemeteritems in RemoteFX afbeeldingen zodat u kunt knelpunten detecteren door te helpen zaken zoals frame codering tijd bij te houden en overgeslagen frames.

Nadat u de naam van uw externe sessie hebt vastgesteld, volg deze instructies voor het verzamelen van de prestatiemeteritems voor RemoteFX afbeeldingen voor de externe sessie.

1. Selecteer **Start** > **Systeembeheer** > **Prestatiemeter**.
2. In de **Prestatiemeter** dialoogvenster Vouw **controlehulpprogramma's**, selecteer **Prestatiemeter**, en selecteer vervolgens **toevoegen**.
3. In de **items toevoegen** in het dialoogvenster van de **beschikbare items** lijst uit en vouw prestatiemeteritem-object voor RemoteFX afbeeldingen.
4. Selecteer de items die moeten worden bewaakt.
5. In de **exemplaren van het geselecteerde object** , selecteert u de specifieke exemplaren voor de geselecteerde items worden bewaakt en selecteer vervolgens **toevoegen**. Als u alle exemplaren van de beschikbare prestatiemeteritems selecteren, schakelt **alle exemplaren**.
6. Nadat u hebt de meteritems toe te voegen, selecteert u **OK**.

De geselecteerde prestatiemeteritems worden weergegeven op het scherm van de Prestatiemeter.

>[!NOTE]
>Elke actieve sessie op een host heeft een eigen instantie van elk prestatiemeteritem.

## <a name="diagnosis"></a>Diagnosis

Grafische afbeeldingen met betrekking tot prestatieproblemen kunnen in het algemeen worden onderverdeeld in vier categorieën:

- Lage framesnelheid
- Willekeurige vertragingen
- Hoge latentie voor invoer
- Slechte frame kwaliteit

Gestart door aan te pakken lage framesnelheid, willekeurige vertragingen en hoge latentie voor invoer. De volgende sectie ziet u welke prestatiemeteritems meten elke categorie.

### <a name="performance-counters"></a>Prestatiemeteritems

In deze sectie helpt u bepalen knelpunten.

Controleer eerst de teller uitvoer Frames/Second. Deze meet het aantal frames beschikbaar gesteld aan de client. Als deze waarde kleiner dan de teller invoer Frames/Second is, wordt frames overgeslagen. Gebruik de Frames overgeslagen/seconde tellers voor het identificeren van het knelpunt.

Er zijn drie typen Frames overgeslagen/seconde items:

- Frames overgeslagen per seconde (onvoldoende netwerkbronnen)
- Frames overgeslagen per seconde (Client onvoldoende Resources)
- Frames overgeslagen per seconde (onvoldoende serverresources)

Een hoge waarde voor het gebruik van de Frames overgeslagen/seconde tellers impliceert dat het probleem is gerelateerd aan de bron de teller houdt. Bijvoorbeeld, als de client niet decoderen en aanwezig frames hetzelfde tarief de-server biedt de frames, is de teller Frames overgeslagen per seconde (onvoldoende Client Resources) hoog.

Als de teller uitvoer Frames/Second overeenkomt met de teller invoer Frames/Second, nog steeds hebt ongebruikelijke vertraging of gestopt, het probleem wordt mogelijk de gemiddelde tijd voor codering. Codering is een synchrone proces dat op de server in het scenario voor één-sessie (vGPU) en op de virtuele machine in het scenario voor meerdere sessie plaatsvindt. Gemiddelde tijd voor codering moet dan 33 ms. Als de gemiddelde tijd van de codering dan 33 ms is, maar u nog steeds problemen met prestaties hebt, is er mogelijk een probleem met de app of het besturingssysteem dat u gebruikt.

Zie voor meer informatie over het oplossen van problemen met betrekking tot app [gebruiker invoer vertraging prestatiemeteritems](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters).

Aangezien RDP een gemiddelde tijd van de codering van 33 ms ondersteunt, ondersteunt deze een invoer framesnelheid maximaal 30 frames per seconde. Houd er rekening mee dat 33 ms is de maximale ondersteunde framesnelheid. In veel gevallen worden de ervaren door de gebruiker framesnelheid lager, afhankelijk van hoe vaak een frame voor RDP door de bron is opgegeven. Bijvoorbeeld taken zoals het bekijken van een video vereisen een volledige invoer framesnelheid van 30 frames per seconde, maar die minder resource-intensief-taken, zoals weinig bewerken van een word-document een hoge frequentie van invoer frames per seconde niet vereisen voor een goede gebruikerservaring.

Gebruik de teller Frame kwaliteit frame kwaliteitsproblemen op te sporen. Deze teller wordt de kwaliteit van het kader van de uitvoer uitgedrukt als een percentage van de kwaliteit van het kader van de bron. Het kwaliteitsverlies kan worden veroorzaakt door RemoteFX of kan het zijn inherent zijn aan de bron voor afbeeldingen. Als RemoteFX het kwaliteitsverlies veroorzaakt, is het probleem mogelijk een gebrek aan resources netwerk of de server voor het verzenden van hogere kwaliteit van zowel inhoud.

## <a name="mitigation"></a>Oplossing

Als de server-bronnen zijn de bottleneck veroorzaakt, probeert u een van de volgende dingen om prestaties te verbeteren:

- Verminder het aantal sessies per host.
- Vergroot de hoeveelheid geheugen en compute-resources op de server.
- De resolutie van de verbinding verwijderen.

Als de netwerkbronnen zijn de bottleneck veroorzaakt, probeert u een van de volgende dingen die verbeterd kunnen netwerkbeschikbaarheid per sessie:

- Verminder het aantal sessies per host.
- De resolutie van de verbinding verwijderen.
- Gebruik een hogere bandbreedte-netwerk.

Als de client resources zijn de bottleneck veroorzaakt, kunt u een of beide van de volgende dingen om prestaties te verbeteren:

- Installeer de meest recente extern bureaublad-client.
- Meer geheugen en compute-resources op de clientcomputer.

> [!NOTE]
> Wordt de teller bron Frames/Second momenteel niet ondersteund. Op dit moment wordt de teller bron Frames/Second altijd zijn ingesteld op 0.

## <a name="next-steps"></a>Volgende stappen

- Zie voor het maken van een Azure-machine met geoptimaliseerde GPU [grafische versnelling op GPU (unit) voor Windows Virtual Desktop Preview-omgeving configureren](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu).
- Zie voor een overzicht van de probleemoplossing en escalatie nummers [overzicht, feedback en ondersteuning voor probleemoplossing](https://docs.microsoft.com/azure/virtual-desktop/troubleshoot-set-up-overview).
- Zie voor meer informatie over de Preview-service, [Windows Desktop Preview-omgeving](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
