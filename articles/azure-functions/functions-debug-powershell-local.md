---
title: Lokaal fouten opsporen in PowerShell Azure Functions
description: Lees hoe u voor het ontwikkelen van functies met behulp van PowerShell.
services: functions
documentationcenter: na
author: tylerleonhardt
manager: jeconnoc
ms.service: azure-functions
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha
ms.reviewer: glenga
ms.openlocfilehash: fc30a2efb21d5b7f3168d9229ec5baf9a7f05eb1
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706414"
---
# <a name="debug-powershell-azure-functions-locally"></a>Lokaal fouten opsporen in PowerShell Azure Functions

Azure Functions kunt u uw functies als PowerShell-scripts ontwikkelen.

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

U kunt fouten opsporen in uw PowerShell-functies lokaal net als elke PowerShell-scripts met behulp van de volgende standaard ontwikkelhulpprogramma's:

* [Visual Studio Code](https://code.visualstudio.com/): Van Microsoft gratis, lichtgewicht en open-source editor met de PowerShell-extensie die een volledige ervaring van de PowerShell-ontwikkeling biedt.
* Een PowerShell-console: Fouten opsporen met behulp van de dezelfde opdrachten die u gebruikt voor fouten opsporen in een andere PowerShell-proces.

[Azure Functions Core Tools](functions-run-local.md) biedt ondersteuning voor lokale foutopsporing van Azure Functions, zoals PowerShell-functies.

## <a name="example-function-app"></a>Voorbeeld van de functie-app

De functie-app die wordt gebruikt in dit artikel is van één HTTP-geactiveerde functie en heeft de volgende bestanden:

```
PSFunctionApp
 | - HttpTriggerFunction
 | | - run.ps1
 | | - function.json
 | - local.settings.json
 | - host.json
 | - profile.ps1
```

Deze functie-app is die vergelijkbaar is met de krijgt u na voltooiing van de [PowerShell-snelstartgids](functions-create-first-function-powershell.md).

De functiecode in `run.ps1` ziet eruit als in het volgende script:

```powershell
param($Request)

$name = $Request.Query.Name

if($name) {
    $status = 200
    $body = "Hello $name"
}
else {
    $status = 400
    $body = "Please pass a name on the query string or in the request body."
}

Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```

## <a name="set-the-attach-point"></a>De punt toevoegen

Om op te sporen een PowerShell-functie, wordt de functie moet worden gestopt voor het foutopsporingsprogramma moet worden gekoppeld. De `Wait-Debugger` cmdlet stopt u de uitvoering en wacht tot het foutopsporingsprogramma.

U hoeft alleen is, Voeg een aanroep naar de `Wait-Debugger` cmdlet net boven de `if` instructie als volgt te werk:

```powershell
param($Request)

$name = $Request.Query.Name

# This is where we will wait for the debugger to attach
Wait-Debugger

if($name) {
    $status = 200
    $body = "Hello $name"
}
# ...
```

Foutopsporing begint bij de `if` instructie. 

Met `Wait-Debugger` aanwezig is, kunt u nu de functies die met behulp van Visual Studio Code of een PowerShell-console foutopsporing.

## <a name="debug-in-visual-studio-code"></a>Problemen oplossen in Visual Studio Code

Als u wilt fouten opsporen in uw PowerShell-functies in Visual Studio Code, hebt u het volgende zijn geïnstalleerd:

* [PowerShell-extensie voor Visual Studio Code](/powershell/scripting/components/vscode/using-vscode)
* [Azure Functions extension for Visual Studio Code](functions-create-first-function-vs-code.md) (Azure Functions-extensie voor Visual Studio Code)
* [PowerShell Core 6.2 of hoger](/powershell/scripting/install/installing-powershell-core-on-windows)

Na de installatie van deze afhankelijkheden laden van een bestaande PowerShell-Functions-project, of [uw eerste PowerShell-Functions-project maakt](functions-create-first-function-powershell.md).

>[!NOTE]
> Moet uw project niet de benodigde configuratiebestanden hebt, wordt u gevraagd te toe te voegen.

### <a name="set-the-powershell-version"></a>De PowerShell-versie instellen

PowerShell Core installeert naast Windows PowerShell. PowerShell Core instellen als de PowerShell-versie moet worden gebruikt met de PowerShell-extensie voor Visual Studio Code.

1. Druk op F1 om weer te geven van de opdracht palet en te zoeken naar `Session`.

1. Kies **PowerShell: Sessie-Menu weergeven**.

1. Als uw **huidige sessie** is niet **PowerShell Core 6**, kiest u **overschakelen naar: PowerShell Core 6**.

Wanneer u een PowerShell-bestand opent, ziet u de versie die in het groen aan de onderkant rechts van het venster weergegeven. De sessie-menu selecteren van deze tekst ook worden weergegeven. Zie voor meer informatie, de [kiezen van een versie van PowerShell gebruiken met de extensie](/powershell/scripting/components/vscode/using-vscode#choosing-a-version-of-powershell-to-use-with-the-extension).

### <a name="start-the-function-app"></a>De functie-app te starten

Controleer `Wait-Debugger` is ingesteld in de functie waar u het foutopsporingsprogramma koppelen.  Met `Wait-Debugger` toegevoegd, kunt u uw functie-app met behulp van Visual Studio Code fouten opsporen.

Kies de **Debug** deelvenster en vervolgens **koppelen aan PowerShell-functie**.

![debugger](https://user-images.githubusercontent.com/2644648/56166073-8a7b3780-5f89-11e9-85ce-36ed38e221a2.png)

U kunt ook druk op F5 foutopsporing te starten.

Het begin foutopsporing bewerking heeft de volgende taken:

* Uitvoeringen `func extensions install` in de terminal te installeren van een Azure Functions-uitbreidingen die vereist zijn voor uw functie-app.
* Uitvoeringen `func host start` in de terminal om te starten van de functie-app in de Functions-host.
* Het PowerShell-foutopsporingsprogramma koppelen aan de PowerShell-runspace in de Functions-runtime.

Met uw functie-app die wordt uitgevoerd, moet u een aparte PowerShell-console de HTTP-geactiveerde functie aanroept.

De PowerShell-console is in dit geval de client. De `Invoke-RestMethod` wordt gebruikt om de functie te activeren.

Voer de volgende opdracht in een PowerShell-console:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

U zult zien dat er onmiddellijk een antwoord is niet geretourneerd. Dat komt doordat `Wait-Debugger` heeft die zijn gekoppeld aan de foutopsporing en PowerShell uitvoering is opgenomen in de modus einde zodra deze kan. Dit is vanwege de [BreakAll concept](#breakall-might-cause-your-debugger-to-break-in-an-unexpected-place), die later wordt toegelicht. Nadat u op de `continue` knop, het foutopsporingsprogramma nu onderbreekt op de regel na de juiste `Wait-Debugger`.

Op dit moment het foutopsporingsprogramma is gekoppeld en u het normale foutopsporingsprogramma bewerkingen kan uitvoeren. Zie voor meer informatie over het gebruik van de foutopsporing in Visual Studio Code [de officiële documentatie](https://code.visualstudio.com/Docs/editor/debugging#_debug-actions).

Nadat u doorgaan en volledig uw script te starten, ziet u dat:

* De PowerShell-console die u hebt de `Invoke-RestMethod` heeft een resultaat geretourneerd
* De geïntegreerde PowerShell-Console in Visual Studio Code wordt gewacht tot een script dat moet worden uitgevoerd

Later wanneer u dezelfde functie, de foutopsporing in PowerShell-uitbreiding aanroepen regeleinden juiste na de `Wait-Debugger`.

## <a name="debugging-in-a-powershell-console"></a>Foutopsporing in een PowerShell-Console

>[!NOTE]
> In deze sectie wordt ervan uitgegaan dat u hebt gelezen de [Azure Functions Core Tools docs](functions-run-local.md) en weet hoe u gebruik van de `func host start` opdracht om uw functie-app te starten.

Open een console `cd` in de map van uw functie-app, en voer de volgende opdracht uit:

```sh
func host start
```

Met de functie-app die wordt uitgevoerd en de `Wait-Debugger` aanwezig is, kunt u koppelen aan het proces. U moet twee meer PowerShell-consoles.

Een van de consoles fungeert als de client. Dit u aanroepen `Invoke-RestMethod` om de functie te activeren. U kunt bijvoorbeeld de volgende opdracht uitvoeren:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

U zult zien dat er een antwoord dat een resultaat is wordt niet geretourneerd van de `Wait-Debugger`. De PowerShell-runspace is nu wachten op een foutopsporingsprogramma moet worden gekoppeld. Laten we nu ophalen die gekoppeld.

Voer de volgende opdracht uit in de andere PowerShell-console:

```powershell
Get-PSHostProcessInfo
```

Deze cmdlet retourneert een tabel die lijkt op de volgende uitvoer:

```output
ProcessName ProcessId AppDomainName
----------- --------- -------------
dotnet          49988 None
pwsh            43796 None
pwsh            49970 None
pwsh             3533 None
pwsh            79544 None
pwsh            34881 None
pwsh            32071 None
pwsh            88785 None
```

Noteer de `ProcessId` voor het item in de tabel met de `ProcessName` als `dotnet`. Dit proces is de functie-app.

Voer vervolgens het volgende codefragment:

```powershell
# This enters into the Azure Functions PowerShell process.
# Put your value of `ProcessId` here.
Enter-PSHostProcess -Id $ProcessId

# This triggers the debugger.
Debug-Runspace 1
```

Eenmaal is gestart, wordt het foutopsporingsprogramma verbroken en ziet er ongeveer als de volgende uitvoer:

```
Debugging Runspace: Runspace1

To end the debugging session type the 'Detach' command at the debugger prompt, or type 'Ctrl+C' otherwise.

At /Path/To/PSFunctionApp/HttpTriggerFunction/run.ps1:13 char:1
+ if($name) { ...
+ ~~~~~~~~~~~
[DBG]: [Process:49988]: [Runspace1]: PS /Path/To/PSFunctionApp>>
```

Op dit punt u bent gestopt op een onderbrekingspunt in de [foutopsporing voor PowerShell](/powershell/module/microsoft.powershell.core/about/about_debuggers). Hier kunt u alle van de gebruikelijke debug-bewerkingen uitvoeren, stap over, binnengaat, blijven, afsluiten, enzovoort. Als u wilt zien van de volledige reeks foutopsporing opdrachten die beschikbaar zijn in de console, de `h` of `?` opdrachten.

U kunt ook onderbrekingspunten instellen op dit niveau met de `Set-PSBreakpoint` cmdlet.

Wanneer u doorgaan en volledig uw script te starten, ziet u dat:

* De PowerShell-console waarbij u uitgevoerd `Invoke-RestMethod` heeft nu een resultaat geretourneerd.
* De PowerShell-console waarbij u uitgevoerd `Debug-Runspace` wordt gewacht tot een script dat moet worden uitgevoerd.

U kunt dezelfde functie opnieuw activeren (met behulp van `Invoke-RestMethod` bijvoorbeeld) en het foutopsporingsprogramma onderbreekt in meteen na de `Wait-Debugger` opdracht.

## <a name="considerations-for-debugging"></a>Overwegingen voor het opsporen van fouten

Houd rekening met de volgende problemen bij het opsporen van fouten in uw Functions-code.

### <a name="breakall-might-cause-your-debugger-to-break-in-an-unexpected-place"></a>`BreakAll` kan ertoe leiden dat uw foutopsporingsprogramma om in een onverwachte interne breken

De PowerShell-extensie gebruikt `Debug-Runspace`, die op zijn beurt is gebaseerd op PowerShell van `BreakAll` functie. Deze functie vertelt PowerShell om te stoppen op de eerste opdracht die wordt uitgevoerd. Dit gedrag biedt u de mogelijkheid om in te stellen onderbrekingspunten in de softwareontwikkelingsproces runspace.

De Azure Functions-runtime wordt een aantal opdrachten uitgevoerd voordat er daadwerkelijk aanroepen van uw `run.ps1` script, zodat het mogelijk dat het foutopsporingsprogramma belangrijke in het geval is de `Microsoft.Azure.Functions.PowerShellWorker.psm1` of `Microsoft.Azure.Functions.PowerShellWorker.psd1`.

Moet deze onderbreking gebeuren, voer de `continue` of `c` opdracht voor het overslaan van deze onderbrekingspunt. U klikt op de verwachte onderbrekingspunt te stoppen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het ontwikkelen van functies met behulp van PowerShell, [ontwikkelaarsgids van Azure Functions PowerShell](functions-reference-powershell.md).
