---
title: Hand leiding voor het installeren en implementeren van Linux C-agent van Azure Security Center voor IoT-agent | Microsoft Docs
description: Meer informatie over het installeren van de Azure Security Center voor IoT-agent op zowel 32-bits als 64-bits Linux.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 7578811c37cd0bbe47821dadacce5fa5974f56cf
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812729"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Azure Security Center implementeren voor IoT C-gebaseerde beveiligings agent voor Linux

In deze hand leiding wordt uitgelegd hoe u de Azure Security Center voor IoT C-beveiligings agent op Linux installeert en implementeert.

In deze handleiding leert u het volgende: 
> [!div class="checklist"]
> * Installeren
> * Implementatie verifiëren
> * De agent verwijderen
> * Problemen oplossen 

## <a name="prerequisites"></a>Vereisten

Zie [de juiste beveiligings agent kiezen](how-to-deploy-agent.md)voor andere platforms en agents.

1. Voor het implementeren van de beveiligings agent zijn lokale beheerders rechten vereist op de computer waarop u wilt installeren (sudo).

1. [Maak een beveiligings module](quickstart-create-security-twin.md) voor het apparaat.

## <a name="installation"></a>Installatie 

Als u de beveiligings agent wilt installeren en implementeren, gebruikt u de volgende werk stroom:


1. Down load de meest recente versie van [github](https://aka.ms/iot-security-github-c)naar uw computer.

1. Pak de inhoud van het pakket uit en navigeer naar de map _/src/Installation_ .

1. Voeg actieve machtigingen toe aan het **script InstallSecurityAgent** door de volgende opdracht uit te voeren:
    
   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Voer vervolgens de volgende handelingen uit: 

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```
   
   Zie [verificatie configureren](concept-security-agent-authentication-methods.md) voor meer informatie over verificatie parameters.

Met dit script wordt de volgende functie uitgevoerd:

1. Hiermee worden vereisten geïnstalleerd.

2. Hiermee wordt een service gebruiker (met interactieve aanmelding uitgeschakeld) toegevoegd.

3. Installeert de agent als een **daemon** : veronderstelt dat het apparaat wordt gebruikt voor Service beheer.

4. Hiermee configureert u de agent met de opgegeven verificatie parameters. 

Voer het script uit met de para meter – Help voor meer informatie. 
    
    ./InstallSecurityAgent.sh --help

### <a name="uninstall-the-agent"></a>De agent verwijderen

Als u de agent wilt verwijderen, voert u het script uit met de para meter –-Uninstall:

    ./InstallSecurityAgent.sh -–uninstall

## <a name="troubleshooting"></a>Problemen oplossen
De implementatie status controleren door uit te voeren:

    systemctl status ASCIoTAgent.service


## <a name="next-steps"></a>Volgende stappen
- Lees het [overzicht](overview.md) van de Azure Security Center voor IOT-service
- Meer informatie over Azure Security Center voor IoT- [architectuur](architecture.md)
- De [service](quickstart-onboard-iot-hub.md) inschakelen
- Lees de [Veelgestelde vragen](resources-frequently-asked-questions.md)
- [Beveiligings waarschuwingen](concept-security-alerts.md) begrijpen
