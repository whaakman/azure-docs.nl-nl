---
title: Zelfstudie voor het installeren en implementeren van Linux C-agent voor IoT-agent Preview van ASC | Microsoft Docs
description: Informatie over het installeren van de ASC voor IoT-agent op zowel 32-bits en 64-bits Linux.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.service: ascforiot
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 04b41791915946820c9c7a1666ab10e880731e4b
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541558"
---
# <a name="tutorial-deploy-asc-for-iot-c-based-security-agent-for-linux"></a>Zelfstudie: ASC voor beveiliging op basis van IoT C-agent voor Linux implementeren

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In deze zelfstudie wordt uitgelegd hoe te installeren en implementeren van de ASC voor beveiliging op basis van IoT C-agent op Linux.

In deze zelfstudie leert u het volgende: 
> [!div class="checklist"]
> * Installeren
> * Implementatie verifiëren
> * De agent verwijderen
> * Problemen oplossen 

## <a name="prerequisites"></a>Vereisten

Zie voor andere platforms en agent aroma's [kiest u de juiste security agent](select-deploy-agent.md).

1. Voor het implementeren van de security-agent zijn lokale beheerdersrechten vereist op de computer die u installeren wilt op.

1. [Maken van een security module](quickstart-create-security-twin.md) voor het apparaat.

## <a name="installation"></a>Installatie 

Als u wilt installeren en implementeren van de security-agent, het volgende doen:

1. De meest recente versie downloaden op de computer van [Github](https://aka.ms/iot-security-github-cs).

1. Pak de inhoud van het pakket uit en navigeer naar de _/installation_ map.

1. Actieve Voeg machtigingen toe aan de **InstallSecurityAgent script** door het uitvoeren van de volgende:
    
    `chmod +x InstallSecurityAgent.sh` 

1. Voer vervolgens: 

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```
   
   Zie [verificatie configureren](concept-security-agent-authentication-methods.md) voor meer informatie over parameters voor verificatie.

Dit script doet het volgende:

1. Vereiste onderdelen installeert.

2. Hiermee voegt u een servicegebruiker toe (met interactieve aanmelding uitgeschakeld).

3. Installeert de agent als een **Daemon** -wordt ervan uitgegaan dat het apparaat gebruikmaakt van **systemd** voor service management.

4. Hiermee configureert u **sudoers** waarmee de agent voor het uitvoeren van bepaalde taken als root.

5. Hiermee configureert u de agent met de opgegeven verificatieparameters. 

Voer het script voor extra hulp met de parameter Help-informatie: 
    
    `./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>De agent verwijderen

Voer het script met de parameter – u voor het verwijderen van de agent:

    `./InstallSecurityAgent.sh -u`. 

```
 .\InstallSecurityAgent.sh –uninstall / -u
``` 

## <a name="troubleshooting"></a>Problemen oplossen
Controleer de implementatiestatus door uit te voeren:

```
systemctl status ASCIoTAgent.service
```


## <a name="next-steps"></a>Volgende stappen
- Lees de ASC voor IoT-service [overzicht](overview.md)
- Meer informatie over ASC voor IoT [architectuur](architecture.md)
- Schakel de [service](quickstart-onboard-iot-hub.md)
- Lees de [Veelgestelde vragen](resources-frequently-asked-questions.md)
- Inzicht in [beveiligingswaarschuwingen](concept-security-alerts.md)