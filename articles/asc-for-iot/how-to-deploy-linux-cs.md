---
title: Handleiding voor het installeren en implementeren van Linux C# agent van Azure Security Center voor IoT-Preview | Microsoft Docs
description: Informatie over het installeren van de Azure Security Center voor IoT-agent op zowel 32-bits en 64-bits Linux.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: be4c663d3a1e99ef67cbbbc2f39b315f1080125c
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758336"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Azure Security Center implementeren voor IoT C#-security-agent voor Linux gebaseerd

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze handleiding wordt uitgelegd hoe u te installeren en implementeren van de Azure Security Center (ASC) voor IoT C#-security-agent op Linux gebaseerd.

In deze handleiding leert u het volgende: 
> [!div class="checklist"]
> * Installeren
> * Implementatie verifiëren
> * De agent verwijderen
> * Problemen oplossen 

## <a name="prerequisites"></a>Vereisten

Zie voor andere platforms en versies van de agent [kiest u de juiste security agent](how-to-deploy-agent.md).

1. Voor het implementeren van de security-agent zijn lokale beheerdersrechten vereist op de computer die u installeren wilt op. 

1. [Maken van een security module](quickstart-create-security-twin.md) voor het apparaat.

## <a name="installation"></a>Installatie 

Voor het implementeren van de security-agent, het volgende doen:

1. De meest recente versie downloaden op de computer van [Github](https://aka.ms/iot-security-github-cs).

1. Pak de inhoud van het pakket uit en navigeer naar de _/Install_ map.

1. Actieve Voeg machtigingen toe aan de **InstallSecurityAgent script** door uit te voeren `chmod +x InstallSecurityAgent.sh` 

1. Voer vervolgens: 

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```
   
   Zie [verificatie configureren](concept-security-agent-authentication-methods.md) voor meer informatie over parameters voor verificatie.

Dit script doet het volgende:

- Vereiste onderdelen installeert.

- Hiermee voegt u een servicegebruiker toe (met interactieve aanmelding uitgeschakeld).

- Installeert de agent als een **Daemon** -dit wordt ervan uitgegaan dat het apparaat gebruikmaakt van **systemd** voor service management.

- Hiermee configureert u **sudoers** waarmee de agent voor het uitvoeren van bepaalde taken als root.

- Hiermee configureert u de agent met de verificatieparameters opgegeven.


Voer het script voor extra hulp met de parameter Help-informatie: `./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>De agent verwijderen

De agent verwijderen, kunt u het script uitvoeren met de parameter – u: `./InstallSecurityAgent.sh -u`. 

> [!NOTE]
> Verwijder eventuele ontbrekende vereiste onderdelen die zijn geïnstalleerd tijdens de installatie niet verwijderd.

## <a name="troubleshooting"></a>Problemen oplossen  

1. Controleer de implementatiestatus door uit te voeren:

    `systemctl status ASCIoTAgent.service`

2. Logboekregistratie inschakelen.  
   Als de agent niet kan worden gestart, schakelt u het logboek voor meer informatie.

   De logboekregistratie van inschakelen:

   1. Open het configuratiebestand voor het bewerken van in een Linux-editor:

        `vi /var/ASCIoTAgent/General.config`

   1. Bewerk de volgende waarden: 

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" /> 
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```
       De **logFilePath** waarde kan worden geconfigureerd. 

       > [!NOTE]
       > Het is raadzaam logboekregistratie inschakelen **uit** na het oplossen van problemen is voltooid. Logboekregistratie verlaten **op** toeneemt melden bestand grootte en gegevensgebruik.

   1. Start de agent opnieuw door te voeren:

       `systemctl restart ASCIoTAgent.service`

   1. Het logboekbestand voor meer informatie over de fout te bekijken.  

       Locatie van het logboekbestand is: `/var/ASCIoTAgent/IotAgentLog.log`

       Wijzigen van het bestandspad van de locatie op basis van de naam die u hebt gekozen voor de **logFilePath** in stap 2. 

## <a name="next-steps"></a>Volgende stappen

- Lees de ASC voor IoT-service [overzicht](overview.md)
- Meer informatie over ASC voor IoT [architectuur](architecture.md)
- Schakel de [service](quickstart-onboard-iot-hub.md)
- Lees de [Veelgestelde vragen](resources-frequently-asked-questions.md)
- Inzicht in [waarschuwingen](concept-security-alerts.md)