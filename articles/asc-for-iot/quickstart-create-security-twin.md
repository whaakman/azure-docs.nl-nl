---
title: Maken van een security moduledubbel voor ASC voor IoT-Preview | Microsoft Docs
description: Meer informatie over het maken van een ASC voor IoT-moduledubbel voor gebruik met ASC voor IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: c782692e-1284-4c54-9d76-567bc13787cc
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 89802a638944ec220186e943d5fdc33524b2d4e9
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541645"
---
# <a name="quickstart-create-an-asc-for-iot-module-twin"></a>Quickstart: Maken van een ASC voor IoT-moduledubbel

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview. Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze snelstartgids uitleg over het maken van afzonderlijke ASC voor IoT-moduledubbels voor nieuwe apparaten of batch-maken moduledubbels voor alle apparaten in een IoT-Hub.  

## <a name="understanding-asc-for-iot-module-twins"></a>Inzicht krijgen in ASC voor IoT-moduledubbels 

Apparaatdubbels spelen voor IoT-oplossingen die zijn gemaakt in Azure, een belangrijke rol in Apparaatbeheer en automatisering van bedrijfsprocessen. 

ASC voor IoT biedt volledige integratie met uw bestaande IoT platform voor Apparaatbeheer, zodat u kunt de beveiligingsstatus van uw apparaat te beheren en maken gebruik van bestaande mogelijkheden voor toegangsbeheer van apparaat. ASC voor IoT-integratie wordt bereikt met behulp van de IoT-Hub twin mechanisme.  

Zie [IoT-Hub moduledubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) voor meer informatie over het algemeen concept van moduledubbels in Azure IoT Hub. 
 
ASC voor IoT maakt gebruik van het mechanisme voor module dubbele en onderhoudt een moduledubbel beveiliging voor elk van uw apparaten. De moduledubbel beveiliging bevat alle informatie die relevant zijn voor de beveiliging van apparaten voor elk van uw apparaten. 
 
Volledige om gebruik te maken van ASC voor IoT-functies, moet u het maken, configureren en gebruiken van deze moduledubbels beveiliging voor elk apparaat in de service.  

## <a name="create-asc-for-iot-module-twin"></a>ASC maken voor IoT-moduledubbel 

ASC voor IoT-moduledubbels kan worden gemaakt in de batchmodus met behulp van de standaardconfiguratie of afzonderlijk met specifieke configuraties voor elk apparaat. Batch maken voor nieuwe apparaten of apparaten zonder een moduledubbel, gebruikt u de [Module batchscript](https://aka.ms/iot-security-github-create-module). 

>[!NOTE] 
> Met behulp van de batch-methode, worden bestaande moduledubbels niet overschreven. Met behulp van de batch-methode maakt alleen nieuwe moduledubbels voor apparaten die nog geen een moduledubbel. 

Zie [wijzigen van een security moduledubbel](how-to-modify-security-module-twin.md) voor informatie over het wijzigen of de configuratie van een bestaande moduledubbel wijzigen. 

Een nieuwe ASC voor IoT-moduledubbel voor een apparaat gebruiken voor het maken van de volgende instructies: 

1. Zoek en selecteer het apparaat dat u wilt een beveiligings-moduledubbel voor maken van uw IoT-Hub in uw IoT-Hub. 
1. In de **naam van de Microsoft-identiteit** veld **ascforiotsecurity**.
1. Klik op **Opslaan**. 

## <a name="verify-creation-of-a-module-twin"></a>Controleer of het maken van een moduledubbel

Om te controleren of er een dubbel van de module beveiliging voor een specifiek apparaat bestaat:

1. Selecteer in uw Azure-IoT-Hub, **IoT-apparaten** uit de **Explorers** menu.    
1. De apparaat-ID invoeren, of Selecteer een optie in de **apparaat queryveld** en klikt u op **query uitvoeren op apparaten**. 
    ![Query-apparaten](./media/quickstart/verify-security-module-twin.png)
1. Selecteer het apparaat of dubbelklik erop om de details Apparaatpagina te openen. 
1. Selecteer de **Module identiteiten** in het menu en bevestigt u sprake is van de **ascforiotsecurity** module en een **verbindingsstatus** van **verbonden**in de lijst van module-id's die zijn gekoppeld aan het apparaat. 
    ![Modules die zijn gekoppeld aan een apparaat](./media/quickstart/verify-security-module-twin-2.png)


Zie voor meer informatie over het aanpassen van de eigenschappen van ASC voor IoT-moduledubbels [agentconfiguratie](concept-agent-configuration.md).

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het configureren van aangepaste waarschuwingen...

> [!div class="nextstepaction"]
> [Aangepaste waarschuwingen configureren](quickstart-create-custom-alerts.md)
