---
title: Handmatig installeren of bijwerken van de bindinguitbreidingen Azure Functions
description: Informatie over het installeren of bijwerken van de bindinguitbreidingen Azure Functions voor geïmplementeerde functie-apps.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functies, binding-extensies, NuGet, updates
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/26/2018
ms.author: glenga
ms.openlocfilehash: 77b863bc32442261e220b5dd3f11c0bd33b4fa7a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809462"
---
# <a name="manually-install-or-update-azure-functions-binding-extensions-from-the-portal"></a>Handmatig installeren of bijwerken van Azure Functions-bindinguitbreidingen van de portal

De Azure Functions-runtime versie 2.x gebruikmaakt van extensies van de binding voor het implementeren van code voor triggers en bindingen. Bindinguitbreidingen vindt u in het NuGet-pakketten. Voor het registreren van een uitbreiding, moet u in feite een pakket installeren. Bij het ontwikkelen van functies, is de manier waarop dat u bindinguitbreidingen installeert afhankelijk van de ontwikkelomgeving. Zie voor meer informatie, [bindinguitbreidingen registreren](functions-triggers-bindings.md#register-binding-extensions) in de triggers en bindingen.

Soms moet u handmatig installeren of bijwerken van de bindinguitbreidingen in Azure portal. U wilt bijvoorbeeld een geregistreerde binding niet bijwerken naar een nieuwere versie. U moet mogelijk ook voor het registreren van een ondersteunde binding die kan worden geïnstalleerd in de **integreren** tabblad in de portal.

## <a name="install-a-binding-extension"></a>Een binding-extensie installeren

Gebruik de volgende stappen handmatig installeren of bijwerken van uitbreidingen van de portal.

1. In de [Azure-portal](https://portal.azure.com), zoek uw functie-app en selecteer deze. Kies de **overzicht** tabblad en selecteer **stoppen**.  Stoppen van de functie-app, ontgrendelt bestanden zodat de wijzigingen worden doorgevoerd.

1. Kies de **platformfuncties** tabblad en klik vervolgens onder **ontwikkelprogramma's** Selecteer **geavanceerde hulpmiddelen (Kudu)**. Het eindpunt van de Kudu (`https://<APP_NAME>.scm.azurewebsites.net/`) in een nieuw venster wordt geopend.

1. Selecteer in het venster Kudu **Foutopsporingsconsole** > **CMD**.  

1. Navigeer in het opdrachtvenster naar `D:\home\site\wwwroot` en kies het verwijderpictogram naast `bin` op de map te verwijderen. Selecteer **OK** om de verwijdering te bevestigen.

1. Kies het bewerkingspictogram naast de `extensions.csproj` -bestand, dat de bindinguitbreidingen voor de functie-app definieert. Het projectbestand wordt geopend in de online-editor.

1. Controleer de vereiste toevoegingen en updates van **PackageReference** -items in de **ItemGroup**en selecteer vervolgens **opslaan**. De huidige lijst van ondersteunde pakketversies vindt u de [welke pakketten heb ik nodig?](https://github.com/Azure/azure-functions-host/wiki/Updating-your-function-app-extensions#what-nuget-packages-do-i-need) wiki-artikel. Alle drie Azure Storage-bindingen moeten het pakket Microsoft.Azure.WebJobs.Extensions.Storage.

1. Uit de `wwwroot` map, voer de volgende opdracht om op te bouwen de waarnaar wordt verwezen, assembly's in de `bin` map.

    ```cmd
    dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget
    ```

1. Terug in de **overzicht** tabblad in de portal, kiest u **Start** de functie-app opnieuw te starten.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
