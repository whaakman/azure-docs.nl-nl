---
title: Een OPC-Twin-module implementeren op een bestaande Azure-project | Microsoft Docs
description: Over het implementeren van OPC dubbele aan een bestaand project.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 6eeca062bdc17ec207910b9ba4aa8cea4048f849
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080506"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>OPC-Twin implementeren op een bestaand project

De OPC-Twin-module wordt uitgevoerd op IoT Edge en verschillende edge services levert aan de OPC-Twin en de Register-services.

De OPC-Twin micro service vergemakkelijkt de communicatie tussen factory operators en OPC UA-serverapparaten op de werkvloer via een OPC dubbele IoT Edge-module. OPC UA-services (bladeren, lezen, schrijven en uitvoeren) via de REST-API wordt aangegeven dat de micro-service. 

De OPC UA-apparaat register microservice biedt toegang tot geregistreerde OPC UA-toepassingen en de bijbehorende eindpunten. Operators en beheerders kunnen registreren en registratie van nieuwe OPC UA-toepassingen en bladeren van een bestaande, met inbegrip van de eindpunten. Naast de toepassing en het eindpunt management verzamelt de registry-service ook geregistreerde OPC dubbele IoT Edge-modules. De service-API biedt u beheer van edge-module-functionaliteit, bijvoorbeeld starten of stoppen van detectie (scannen services) of nieuwe eindpunt dubbels die kunnen worden benaderd via de OPC-Twin micro-service activeren.

De kern van de module is de identiteit van de Supervisor. De supervisor beheert eindpunt dubbele, overeenkomt met de OPC UA-server-eindpunten die zijn geactiveerd met behulp van de bijbehorende API van de OPC UA-register. Dit eindpunt dubbels vertalen OPC UA-JSON heeft ontvangen van de OPC-Twin micro service wordt uitgevoerd in de cloud in de OPC UA binaire berichten, die worden verzonden via een stateful beveiligd kanaal naar het beheerde eindpunt. De supervisor biedt ook detectieservices apparaat detectie gebeurtenissen naar de OPC UA device onboarding-service verzenden voor de verwerking, waar deze gebeurtenissen leiden tot updates aan de OPC UA-register.  Dit artikel ziet u hoe u de OPC-Twin-module implementeert in een bestaand project.

> [!NOTE]
> Zie voor meer informatie over de details van de implementatie en instructies, de GitHub [opslagplaats](https://github.com/Azure/azure-iiot-opc-twin-module).

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt PowerShell en [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) extensies geïnstalleerd. Als u dit nog niet gedaan hebt, kloon de GitHub-opslagplaats. Voer de volgende opdrachten uit in PowerShell:

```powershell
git clone --recursive https://github.com/Azure/azure-iiot-components.git
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Industriële IoT-services implementeren in Azure

1. Voer in de PowerShell-sessie:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy.cmd
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
> NOOIT doorgaan zonder verificatie.  Als u ervoor kiest om dit te doen, kan iedereen toegang tot uw OPC-Twin-eindpunten van het Internet niet-geverifieerde.   U kunt altijd de ["local" Implementatieoptie](howto-opc-twin-deploy-dependencies.md) om te testen.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Een demo All-in-one industriële IoT-services implementeren

U kunt ook een alles-in-een demo in plaats van alleen de services en afhankelijkheden implementeren.  De alles-in-één demo bevat drie OPC UA-servers, de OPC-Twin-module, alle microservices en een voorbeeld van een Web-App.  Het is bedoeld voor demonstratiedoeleinden te gebruiken.

1. Zorg ervoor dat u hebt een kloon van de opslagplaats (Zie hierboven). Open een PowerShell-prompt in de hoofdmap van de opslagplaats en uitvoeren:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy -type demo
    ```

2. Volg de aanwijzingen voor het toewijzen van een nieuwe naam voor de resourcegroep en een naam op voor de website.  Eenmaal is geïmplementeerd, wordt het script de URL van het eindpunt op het web toepassing weergegeven.

## <a name="deployment-script-options"></a>Implementatieopties voor script

Het script neemt de volgende parameters:

```powershell
-type
```

Het type implementatie (virtuele machine, lokale, demo)

```powershell
-resourceGroupName
```

Mag u de naam van een bestaande of een nieuwe resourcegroep.

```powershell
-subscriptionId
```

Optioneel; de abonnements-ID waarin resources worden geïmplementeerd.

```powershell
-subscriptionName
```

Of de naam van het abonnement.

```powershell
-resourceGroupLocation
```

Optioneel, de locatie van een resourcegroep. Als u opgeeft, wordt geprobeerd een nieuwe resourcegroep maken op deze locatie.

```powershell
-aadApplicationName
```

Een naam voor de AAD-toepassing te registreren bij.

```powershell
-tenantId
```

AAD-tenant te gebruiken.

```powershell
-credentials
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het implementeren van OPC dubbele aan een bestaand project, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Beveiligde communicatie van OPC-Client en de OPC-PLC](howto-opc-vault-deploy-existing-client-plc-communication.md)
