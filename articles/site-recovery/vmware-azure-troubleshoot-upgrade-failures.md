---
title: Problemen met Microsoft Azure Site Recovery Provider-upgrade oplossen | Microsoft Docs
description: Begrijpen en
author: vDonGlover
manager: jarrettr
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 02/05/2019
ms.author: v-doglov
ms.openlocfilehash: fc50be2a960784895947f3f154a0251f41716fc7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58117299"
---
# <a name="troubleshoot-microsoft-azure-site-recovery-provider-upgrade-failures"></a>Problemen met het upgraden van de Microsoft Azure Site Recovery-provider oplossen

Dit artikel helpt u problemen die kunnen ontstaan tijdens een Microsoft Azure Site Recovery Provider-upgrade oplossen.

## <a name="the-upgrade-fails-reporting-that-the-latest-site-recovery-provider-is-already-installed"></a>De upgrade mislukt melden dat de meest recente versie van Site Recovery Provider al is geïnstalleerd

Bij het upgraden van Microsoft Azure Site Recovery Provider (DRA), wordt de geïntegreerde Setup van upgrade mislukt en problemen met het foutbericht weergegeven:

Upgrade wordt niet ondersteund als een hogere versie van de software al is geïnstalleerd.

Als u wilt bijwerken, gebruikt u de volgende stappen uit:

1. Download de Microsoft Azure Site Recovery van geïntegreerde Setup:
   1. In de sectie 'Koppelingen naar de momenteel ondersteunde updatepakketten' van de [Service-updates in de Azure Site Recovery](service-updates-how-to.md##links-to-currently-supported-update-rollups) artikel, selecteert u de provider die u wilt bijwerken.
   2. Zoek op de pagina updatepakket de **informatie bijwerken** sectie en downloaden van het updatepakket voor Microsoft Azure Site Recovery geïntegreerde Setup.

2. Open een opdrachtprompt en navigeer naar de map waarnaar u geïntegreerde Setup-bestand hebt gedownload. De setup-bestanden extraheren uit het downloaden met behulp van de volgende opdracht, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q/x:&lt;pad naar map voor de uitgepakte bestanden&gt;.
    
    Van de voorbeeldopdracht:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

3. Navigeer naar de map waarin u de bestanden hebt uitgepakt in de opdrachtprompt en voer de volgende installatieopdrachten:
   
    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART  UCX_SERVER_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /UPGRADE

1. Terug naar de map waarin u de geïntegreerde Setup gedownload en MicrosoftAzureSiteRecoveryUnifiedSetup.exe voor het voltooien van de upgrade worden uitgevoerd. 

## <a name="upgrade-failure-due-to-the-3rd-party-folder-being-renamed"></a>Upgrade mislukt vanwege de 3rd derden-map wordt gewijzigd

Voor de upgrade te voltooien, moet de map 3rd derden niet worden gewijzigd.

Het probleem kunt oplossen.

1. Start de Register-Editor (regedit.exe) en de vertakking HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\InMage Systems\Installed Products\10 openen.
1. Inspecteer de `Build_Version` sleutelwaarde. Als deze is ingesteld op de nieuwste versie, verlaagt u versienummer. Bijvoorbeeld als de meest recente versie is 9.22. \* en de `Build_Version` sleutel is ingesteld op deze waarde en vervolgens het verlagen naar 9.21.\*.
1. Download de nieuwste Azure Site Recovery geïntegreerde Setup van Microsoft:
   1. In de sectie 'Koppelingen naar de momenteel ondersteunde updatepakketten' van de [Service-updates in de Azure Site Recovery](service-updates-how-to.md##links-to-currently-supported-update-rollups) artikel, selecteert u de provider die u wilt bijwerken.
   2. Zoek op de pagina updatepakket de **informatie bijwerken** sectie en downloaden van het updatepakket voor Microsoft Azure Site Recovery geïntegreerde Setup.
1. Open een opdrachtprompt en navigeer naar de map die u hebt gedownload van geïntegreerde Setup-bestand en het extract de setup-bestanden van het downloaden met behulp van de volgende opdracht, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q/x:&lt;pad naar map voor de uitgepakte bestanden&gt;.

    Van de voorbeeldopdracht:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

1. Navigeer naar de map waarin u de bestanden hebt uitgepakt in de opdrachtprompt en voer de volgende installatieopdrachten:
   
    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART

1. Gebruik Taakbeheer om te controleren van de voortgang van de installatie. Wanneer het proces voor het CX_THIRDPARTY_SETUP. EXE is niet meer zichtbaar in Taakbeheer, gaat u verder met de volgende stap.
1. Controleer of dat C:\thirdparty bestaat en dat de map de bibliotheken RRD bevat.
1. Terug naar de map waarin u de geïntegreerde Setup gedownload en MicrosoftAzureSiteRecoveryUnifiedSetup.exe voor het voltooien van de upgrade worden uitgevoerd. 