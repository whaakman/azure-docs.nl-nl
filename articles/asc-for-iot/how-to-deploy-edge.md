---
title: Azure Security Center implementeren voor IoT Edge-module | Microsoft Docs
description: Meer informatie over het implementeren van een Azure Security Center voor IoT security-agent op IoT Edge.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/1/2019
ms.author: mlottner
ms.openlocfilehash: 40f771e97b61c28229b0eff29191247ef2fef695
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862842"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>Een beveiligingsmodule op uw IoT Edge-apparaat implementeren

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

**Azure Security Center (ASC) voor IoT** module biedt een uitgebreide oplossing voor uw IoT Edge-apparaat.
Security module verzamelt, verzamelt en analyseert onbewerkte beveiligingsgegevens van uw besturingssysteem en de Container-systeem in de praktijk toepasbare aanbevelingen en waarschuwingen.
Zie voor meer informatie, [Security-module voor IoT Edge](security-edge-architecture.md).

In deze handleiding leert u hoe u een security-module op uw IoT Edge-apparaat implementeren.

## <a name="deploy-security-module"></a>Beveiligingsmodule implementeren

Gebruik de volgende stappen voor het implementeren van een ASC voor IoT-beveiliging-module voor IoT Edge.

### <a name="prerequisites"></a>Vereisten

- Controleer of uw apparaat is in uw IoT-Hub [geregistreerd als een IoT Edge-apparaat](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).

- ASC voor IoT Edge-module vereist [AuditD framework](https://linux.die.net/man/8/auditd) op het IoT Edge-apparaat is geïnstalleerd.

    - Installeer framework door te voeren van de volgende opdracht uit op uw IoT Edge-apparaat:
   
      `sudo apt-get install auditd audispd-plugins`
   
    - Controleer of dat auditd actief is door het uitvoeren van de volgende opdracht uit:
   
      `sudo systemctl status auditd`
      
        Het verwachte antwoord is `active (running)`. 

### <a name="deployment-using-azure-portal"></a>Implementatie met behulp van Azure portal

1. Open in Azure portal, **Marketplace**.

1. Selecteer **Internet of Things**, zoek vervolgens naar **Azure Security Center voor IoT** en selecteer deze.

   ![Selecteer het Azure Beveiligingscentrum voor IoT](media/howto/edge-onboarding-8.png)

1. Klik op **maken** om de implementatie te configureren. 

1. Kies het Azure **abonnement** van uw IoT-Hub, selecteer uw **IoT-Hub**.<br>Selecteer **implementeren op een apparaat** gericht op één apparaat of een keuze **implementeren op schaal** wilt richten op meerdere apparaten, en klik op **maken**. Zie voor meer informatie over het implementeren van op grote schaal [over het implementeren van](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >Als u hebt geselecteerd **implementeren op schaal**, de apparaatnaam en gegevens toevoegen voordat u verdergaat met de **Modules toevoegen** tabblad in de volgende instructies.     

Er zijn drie stappen voor het maken van een IoT Edge-implementatie voor Azure Security Center voor IoT. De volgende secties helpen bij elkaar. 

#### <a name="step-1-add-modules"></a>Stap 1: Modules toevoegen

1. Uit de **Modules toevoegen** tabblad **implementatie Modules** gebied, klikt u op **AzureSecurityCenterforIoT**. 
   
1. Wijzig de **naam** naar **azureiotsecurity**.
1. Wijzig de naam van **URI installatiekopie** naar **mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.1**
      
1. Controleer **de gewenste eigenschappen van de moduledubbel Set** is geselecteerd en de configuratieobject te wijzigen:
      
    ``` json
      "properties.desired": {
        "azureiot*com^securityAgentConfiguration^1*0*0": {
        }
      }
      ```

1. Klik op **Opslaan**.
1. Ga naar de onderkant van het tabblad en selecteer **geavanceerde instellingen voor Edge-Runtime configureren**.
   
  >[!Note]
  > Voer **niet** AMQP-communicatie voor de IoT Edge Hub uitschakelen.
  > Azure Security Center voor IoT-module vereist AMQP-communicatie met IoT Edge Hub.
   
1. Wijzig de **installatiekopie** onder **Edge Hub** naar **mcr.microsoft.com/ascforiot/edgehub:1.05-preview**.
      
1. Controleer of **opties maken** is ingesteld op: 
         
    ``` json
    {
      "HostConfig": {
        "PortBindings": {
          "8883/tcp": [{"HostPort": "8883"}],
          "443/tcp": [{"HostPort": "443"}],
          "5671/tcp": [{"HostPort": "5671"}]
        }
      }
    }
    ```
      
1. Klik op **Opslaan**.
   
1. Klik op **volgende**.

#### <a name="step-2-specify-routes"></a>Stap 2: Routes opgeven 

1. In de **Routes opgeven** tabblad, stelt u de **ASCForIoTToIoTHub** routeren naar **' uit/berichten/modules/azureiotsecurity/\* in $upstream '**, en klikt u op  **Volgende**.

   ![Routes opgeven](media/howto/edge-onboarding-9.png)

#### <a name="step-3-review-deployment"></a>Stap 3: Implementatie controleren

1. In de **implementatie bekijken** tabblad, uw implementatie-informatie en selecteer vervolgens **indienen** om uit te voeren van de implementatie.

## <a name="diagnostic-steps"></a>Diagnostische stappen

Als u een probleem ondervindt, zijn de logboeken voor containers de beste manier om meer informatie over de status van een IoT Edge module beveiligingsapparaat. Gebruik de opdrachten en hulpprogramma's in deze sectie om informatie te verzamelen.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Controleer of dat de vereiste containers zijn geïnstalleerd en werkt zoals verwacht

1. Voer de volgende opdracht uit op uw IoT Edge-apparaat:
    
     `sudo docker ps`
   
1. Controleer of de volgende containers worden uitgevoerd:
   
   | Name | AFBEELDING |
   | --- | --- |
   | azureIoTSecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:0.0.1 |
   | edgeHub | asotcontainerregistry.azurecr.io/edgehub:1.04-preview |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0 |
   
   Als de minimaal containers niet aanwezig zijn vereiste, controleert u als het manifest van uw IoT Edge-implementatie wordt uitgelijnd met de aanbevolen instellingen. Zie voor meer informatie, [implementeren IoT Edge-module](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>De module Logboeken op fouten controleren
   
1. Voer de volgende opdracht uit op uw IoT Edge-apparaat:

   `sudo docker logs azureiotsecurity`
   
1. Voor meer uitgebreide logboeken toevoegen de volgende omgevingsvariabele **azureiotsecurity** implementatie van beleidsmodule: `logLevel=Debug`.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over configuratie-opties, blijven de handleiding voor de configuratie van de module. 
> [!div class="nextstepaction"]
> [Module configuratie gebruiksaanwijzing](./how-to-agent-configuration.md)
