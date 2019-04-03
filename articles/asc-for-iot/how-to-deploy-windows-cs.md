---
title: Windows-installatie van Azure Security Center voor IoT-agent Preview | Microsoft Docs
description: Meer informatie over het installeren van Azure Security Center voor IoT-agent op 32-bits of 64-bits Windows-apparaten.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: mlottner
ms.openlocfilehash: b45d64f56653ad6d0baabef4a63351ef14f24c42
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58861952"
---
# <a name="deploy-an-azure-security-center-for-iot-c-based-security-agent-for-windows"></a>Een Azure Security Center implementeren voor IoT C#-op basis van de security-agent voor Windows

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze handleiding wordt uitgelegd hoe u voor het installeren van de Azure Security Center (ASC) voor IoT C#-security-agent op Windows gebaseerd.

In deze handleiding leert u het volgende: 
> [!div class="checklist"]
> * Installeren
> * Implementatie verifiëren
> * De agent verwijderen
> * Problemen oplossen 

## <a name="prerequisites"></a>Vereisten

Zie voor andere platforms en agent aroma's [kiest u de juiste security agent](how-to-deploy-agent.md).

1. Lokale administratorrechten op de computer die u installeren wilt op. 

1. [Maken van een security module](quickstart-create-security-twin.md) voor het apparaat.

## <a name="installation"></a>Installatie 

Voor het installeren van de security-agent, het volgende doen:

1. Het installeren van de ASC voor IoT Windows C# -agent op het apparaat de meest recente versie downloaden op uw computer van de ASC voor IoT [GitHub-opslagplaats](https://github.com/Azure/Azure-IoT-Security-Agent-CS).

2. Pak de inhoud van het pakket uit en navigeer naar de map/Install.

3. Open Windows PowerShell als beheerder. 
    1. Actieve machtigingen toevoegen aan het script InstallSecurityAgent door uit te voeren ```Unblock-File .\InstallSecurityAgent.ps1```
    
        en uitvoeren:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```
    
    Bijvoorbeeld:
    
    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```
    
    Zie [verificatie configureren](concept-security-agent-authentication-methods.md) voor meer informatie over parameters voor verificatie.

Dit script doet het volgende:

- Vereiste onderdelen installeert.

- Hiermee voegt u een servicegebruiker toe (met interactieve aanmelding uitgeschakeld).

- Installeert de agent als een **systeemservice**.

- Hiermee configureert u de agent met de verificatieparameters opgegeven.


Als u meer hulp nodig hebt, gebruikt de opdracht Get-Help in PowerShell <br>Get-Help-voorbeeld:  
    ```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Controleer of de status van de implementatie

- Controleer de implementatiestatus van de agent door uit te voeren:<br>
    ```sc.exe query "ASC IoT Agent" ```

### <a name="uninstall-the-agent"></a>De agent verwijderen

De agent verwijderen:

1. Voer het volgende PowerShell-script met de **-modus** parameter ingesteld op **verwijderen**.  

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ``` 

## <a name="troubleshooting"></a>Problemen oplossen

Als de agent niet kan worden gestart, logboekregistratie inschakelen (logboekregistratie is *uit* standaard) voor meer informatie.

Logboekregistratie inschakelen:

1. Open het configuratiebestand (General.config) voor het bewerken met behulp van een standaardbestand-editor.

1. Bewerk de volgende waarden:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/> 
   <add key="diagnosticVerbosityLevel" value="Some" /> 
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Het is raadzaam logboekregistratie inschakelen **uit** na het oplossen van problemen is voltooid. Logboekregistratie verlaten **op** toeneemt melden bestand grootte en gegevensgebruik. 

1. De agent opnieuw starten door het uitvoeren van de volgende PowerShell of vanaf de opdrachtregel:

    **PowerShell**
     ```
     Restart-Service "ASC IoT Agent"
     ```
     
   of

    **CMD**
     ```
     sc.exe stop "ASC IoT Agent" 
     sc.exe start "ASC IoT Agent" 
     ```

1. Controleer het logboekbestand voor meer informatie over de fout.

   Locatie van logboekbestand: `%WinDir%/System32/IoTAgentLog.log`


## <a name="next-steps"></a>Volgende stappen
- Lees de ASC voor IoT-service [overzicht](overview.md)
- Meer informatie over ASC voor IoT [architectuur](architecture.md)
- Schakel de [service](quickstart-onboard-iot-hub.md)
- Lees de [Veelgestelde vragen](resources-frequently-asked-questions.md)
- Inzicht in [waarschuwingen](concept-security-alerts.md)