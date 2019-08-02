---
title: Windows-installatie van Azure Security Center voor IoT-agent | Microsoft Docs
description: Meer informatie over het installeren van Azure Security Center voor IoT-agent op 32-bits of 64-bits Windows-apparaten.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: acc99f260931de7fd8c7566a3ff6daf43f34c5ef
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597206"
---
# <a name="deploy-an-azure-security-center-for-iot-c-based-security-agent-for-windows"></a>Een Azure Security Center implementeren voor IoT C#-gebaseerde beveiligings agent voor Windows

In deze hand leiding wordt uitgelegd hoe u de Azure Security Center C#voor IOT-gebaseerde beveiligings Agent installeert in Windows.

In deze handleiding leert u het volgende: 
> [!div class="checklist"]
> * Installeren
> * Implementatie verifiëren
> * De agent verwijderen
> * Problemen oplossen 

## <a name="prerequisites"></a>Vereisten

Zie [de juiste beveiligings agent kiezen](how-to-deploy-agent.md)voor andere platforms en agents.

1. Lokale beheerders rechten op de computer waarop u wilt installeren. 

1. [Maak een beveiligings module](quickstart-create-security-twin.md) voor het apparaat.

## <a name="installation"></a>Installatie 

Als u de beveiligings agent wilt installeren, gebruikt u de volgende werk stroom:

1. Installeer de Azure Security Center voor IoT Windows C# -agent op het apparaat. Down load de meest recente versie naar uw computer vanuit de Azure Security Center voor IoT [github-opslag plaats](https://github.com/Azure/Azure-IoT-Security-Agent-CS).

1. Pak de inhoud van het pakket uit en navigeer naar de map/install.

1. Open Windows Power shell als Administrator. 
1. Voer de volgende handelingen uit om actieve machtigingen toe te voegen aan het InstallSecurityAgent-script:<br>
    ```
    Unblock-File .\InstallSecurityAgent.ps1
    ```
    
    Voer vervolgens de volgende handelingen uit:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```
    
    Bijvoorbeeld:
    
    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```
    
    Zie [verificatie configureren](concept-security-agent-authentication-methods.md)voor meer informatie over verificatie parameters.

Met dit script worden de volgende acties uitgevoerd:

- Hiermee worden vereisten geïnstalleerd.

- Hiermee wordt een service gebruiker (met interactieve aanmelding uitgeschakeld) toegevoegd.

- Installeert de agent als een **systeem service**.

- Hiermee configureert u de agent met de opgegeven verificatie parameters.


Gebruik de opdracht Get-Help in Power shell voor meer informatie. <br>Get-Help-voor beeld:  
    ```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Implementatie status controleren

- Controleer de implementatie status van de agent door uit te voeren:<br>
    ```sc.exe query "ASC IoT Agent"```

### <a name="uninstall-the-agent"></a>De agent verwijderen

De agent verwijderen:

1. Voer het volgende Power shell-script uit met de para meter **-mode** ingesteld op **verwijderen**.  

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ``` 

## <a name="troubleshooting"></a>Problemen oplossen

Als de agent niet kan worden gestart, schakelt u logboek registratie in (logboek registratie is standaard *uitgeschakeld* ) om meer informatie te krijgen.

Logboek registratie inschakelen:

1. Open het configuratie bestand (General. config) voor bewerking met een standaard bestands editor.

1. Bewerk de volgende waarden:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/> 
   <add key="diagnosticVerbosityLevel" value="Some" /> 
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Het is raadzaam om de logboek registratie **uit** te scha kelen nadat de probleem oplossing is voltooid. Wanneer de logboek registratie is ingeschakeld, neemt de grootte **van** het logboek bestand en het gegevens gebruik toe. 

1. Start de agent opnieuw door de volgende Power shell of opdracht regel uit te voeren:

    **Powershell**
     ```
     Restart-Service "ASC IoT Agent"
     ```
     
   of

    **CMD**
     ```
     sc.exe stop "ASC IoT Agent" 
     sc.exe start "ASC IoT Agent" 
     ```

1. Raadpleeg het logboek bestand voor meer informatie over de fout.

   Locatie van logboek bestand:`%WinDir%/System32/IoTAgentLog.log`


## <a name="next-steps"></a>Volgende stappen
- Lees het [overzicht](overview.md) van de Azure Security Center voor IOT-service
- Meer informatie over Azure Security Center voor IoT- [architectuur](architecture.md)
- De [service](quickstart-onboard-iot-hub.md) inschakelen
- Lees de [Veelgestelde vragen](resources-frequently-asked-questions.md)
- Meer informatie over [waarschuwingen](concept-security-alerts.md)