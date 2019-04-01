---
title: Over het implementeren van Azure IoT OPC UA device management-module aan een bestaand project | Microsoft Docs
description: Over het implementeren van OPC dubbele aan een bestaand project.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: da11d4977cc998c9fccb3628a44e45db95c7d977
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759439"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>OPC-Twin implementeren op een bestaand project

De OPC-Apparaatdubbel-module wordt uitgevoerd op IoT Edge en verschillende edge services levert aan de OPC-Apparaatdubbel en de Register-services. 

De OPC-Apparaatdubbel micro service vergemakkelijkt de communicatie tussen factory operators en OPC UA-serverapparaten op de werkvloer via een OPC dubbele IoT Edge-module. OPC UA-services (bladeren, lezen, schrijven en uitvoeren) via de REST-API wordt aangegeven dat de micro-service. 

De OPC UA-Apparaatregister micro-service biedt toegang tot geregistreerde OPC UA-toepassingen en de bijbehorende eindpunten. Operators en beheerders kunnen registreren en registratie van nieuwe OPC UA-toepassingen en bladeren van een bestaande, met inbegrip van de eindpunten. Naast de toepassing en het eindpunt management verzamelt de registry-service ook geregistreerde OPC Device Twin IoT Edge-modules. De service-API biedt u beheer van edge-module-functionaliteit, bijvoorbeeld starten of stoppen van detectie (scannen services) of nieuwe eindpunt dubbels die kunnen worden benaderd via de OPC-Twin micro-service activeren.

De kern van de module is de identiteit van de Supervisor. De supervisor beheert eindpunt dubbele, overeenkomt met de OPC UA-server-eindpunten die zijn geactiveerd met behulp van de bijbehorende API van de OPC UA-register. Dit eindpunt dubbels vertalen OPC UA-JSON heeft ontvangen van de OPC-Twin micro service wordt uitgevoerd in de cloud in de OPC UA binaire berichten, die worden verzonden via een stateful beveiligd kanaal naar het beheerde eindpunt. De supervisor biedt ook detectieservices apparaat detectie gebeurtenissen naar de service OPC UA-apparaat voorbereiden verzenden voor de verwerking, waar deze gebeurtenissen leiden tot updates aan de OPC UA-register.  Dit artikel ziet u hoe u de OPC-Twin-module implementeert in een bestaand project. 

> [!NOTE]
> Zie voor meer informatie over de details van de implementatie en instructies, de GitHub [opslagplaats](https://github.com/Azure/azure-iiot-opc-twin-module).

## <a name="prerequisites"></a>Vereisten

> [!NOTE]
> Opmerking: Vanwege de afhankelijkheid van de AzureRM-module, wordt implementatie momenteel alleen ondersteund voor Windows.

Zorg ervoor dat u hebt PowerShell en [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0) extensies geïnstalleerd.   Als u niet hebt gedaan, nog, kloon de GitHub-opslagplaats.  Open een opdrachtprompt of terminal en voer:

```bash
git clone --recursive https://github.com/Azure/azure-iiot-components 
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Industriële IoT-services implementeren in Azure

1. Op de open de opdrachtprompt of terminal uitvoeren:

   ```bash
   deploy
   ```

2. Volg de aanwijzingen voor het toewijzen van een naam op voor de resourcegroep van de implementatie en geef een naam naar de website.   Het script implementeert de microservices en hun Azure-platform-afhankelijkheden in de resourcegroep in uw Azure-abonnement.  Het script wordt een toepassing ook geregistreerd in uw tenant Azure Active Directory (AAD) om op basis van OAUTH-verificatie te ondersteunen.  Implementatie duurt enkele minuten.  Een voorbeeld van wat u zien wilt zodra de oplossing is geïmplementeerd:

   ![Industriële IoT OPC dubbele implementeren aan bestaand project](media/howto-opc-twin-deploy-existing/opc-twin-deploy-existing1.png)

   De uitvoer bevat de URL van het openbare eindpunt. 

3. Zodra het script voltooid is, selecteert u of u wilt de .env-bestand op te slaan.  U moet het omgevingsbestand .env als u wilt verbinding maken met het cloudeindpunt met behulp van hulpprogramma's, zoals de-Console of -modules voor ontwikkeling en foutopsporing te implementeren.

## <a name="troubleshooting-deployment-failures"></a>Fouten bij het oplossen van problemen

### <a name="resource-group-name"></a>Naam van de resourcegroep

Zorg ervoor dat u een kort en eenvoudig Resourcegroepnaam.  De naam wordt ook gebruikt voor naam resources als zodanig die het moet voldoen aan resource naamgevingsvereisten.  

### <a name="website-name-already-in-use"></a>De naam van website al in gebruik

Het is mogelijk dat de naam van de website al gebruikt wordt.  Als u deze fout, moet u de naam van een andere toepassing te gebruiken.

### <a name="azure-active-directory-aad-registration"></a>Registratie van Azure Active Directory (AAD)

Het implementatiescript probeert te registreren twee AAD-toepassingen in Azure Active Directory.  Afhankelijk van uw rechten voor de geselecteerde AAD-tenant, kan de implementatie mislukken. Er zijn twee opties:

1. Als u ervoor een AAD-tenant in een lijst met tenants kiest, het script nogmaals starten en kies een andere naam in de lijst.
2. U kunt ook een persoonlijke AAD-tenant in een ander abonnement implementeren, het script nogmaals starten en selecteren om deze te gebruiken.

> [!WARNING]
> NOOIT doorgaan zonder verificatie.  Als u ervoor kiest om dit te doen, kan iedereen toegang tot uw OPC-Apparaatbeheer-eindpunten van het Internet niet-geverifieerde.   U kunt altijd de ["local" Implementatieoptie](howto-opc-twin-deploy-dependencies.md) om te testen.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Een demo All-in-one industriële IoT-services implementeren

U kunt ook een alles-in-een demo in plaats van alleen de services en afhankelijkheden implementeren.  De alles-in-één demo bevat drie OPC UA-servers, de OPC-Twin-module, alle microservices en een voorbeeld van een Web-App.  Het is bedoeld voor demonstratiedoeleinden te gebruiken.

1. Zorg ervoor dat u hebt een kloon van de opslagplaats (Zie hierboven). Open een opdrachtprompt of terminal in de hoofdmap van de opslagplaats en uitvoeren:

   ```bash
   deploy -type demo
   ```

2. Volg de aanwijzingen voor het toewijzen van een nieuwe naam voor de resourcegroep en een naam op voor de website.  Eenmaal is geïmplementeerd, wordt het script de URL van het eindpunt op het web toepassing weergegeven.

## <a name="deployment-script-options"></a>Implementatieopties voor script

Het script neemt de volgende parameters:

```bash
-type
```

Het type implementatie (virtuele machine, lokale, demo)

```bash
-resourceGroupName
```

Mag u de naam van een bestaande of een nieuwe resourcegroep.

```bash
-subscriptionId
```

Optioneel; de abonnements-ID waarin resources worden geïmplementeerd.

```bash
-subscriptionName
```

Of de naam van het abonnement.

```bash
-resourceGroupLocation
```

Optioneel, de locatie van een resourcegroep. Als u opgeeft, wordt geprobeerd een nieuwe resourcegroep maken op deze locatie.

```bash
-aadApplicationName
```

Een naam voor de AAD-toepassing te registreren bij. 

```bash
-tenantId
```

AAD-tenant te gebruiken.

```bash
-credentials
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over het implementeren van OPC dubbele aan een bestaand project, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Beveiligde communicatie van OPC-Client en de OPC-PLC ](howto-opc-vault-deploy-existing-client-plc-communication.md)