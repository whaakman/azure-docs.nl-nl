---
title: Het configureren van Azure Machine Learning-Workbench om te werken met een IDE?  | Microsoft Docs
description: Een handleiding voor het configureren van Azure Machine Learning-Workbench om te werken met uw IDE.
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/05/2017
ms.openlocfilehash: 4e18a413a0559b1ddebecf1b29722d21ef35c337
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-azure-machine-learning-workbench-to-work-with-an-ide"></a>Het configureren van Azure Machine Learning-Workbench om te werken met een IDE 

Azure Machine Learning Workbench kan worden geconfigureerd om te werken met populaire Python IDE (Integrated Development Environment). Hiermee kunt een goede gegevens wetenschappelijke ontwikkeling biedt verplaatsen tussen het voorbereiden van gegevens, code schrijven, uitvoeren bijhouden en uitoefening. De ondersteunde IDE momenteel:
- Microsoft Visual Studio Code 
- JetBrain PyCharm 

## <a name="configure-workbench"></a>Workbench configureren
1. Klik op de **bestand** menu in het bovenste de linkerbovenhoek van de app. 
2. Selecteer de **configureren Project IDE** optie van het doel 
3. Typ in `VS Code` of `PyCharm` in de **naam** veld (de naam is willekeurige)
4. Geef de locatie aan het IDE uitvoerbare bestand (met de naam het uitvoerbare bestand en de extensie voltooid) in **uitvoeringspad**

### <a name="default-install-path-for-visual-studio-code"></a>Standaard installatiepad voor Visual Studio Code  

* Windows 32-bits-`C:\Program Files (x86)\Microsoft VS Code\Code.exe`
* Windows 64-bits-`C:\Program Files\Microsoft VS Code\Code.exe`
* Mac OS - selecteert u het pad .app bijvoorbeeld `/Applications/Visual Studio Code.app`, en de app wordt toegevoegd voor de rest van het pad voor u. Het volledige pad naar het uitvoerbare bestand standaard `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code`. Als u uitvoert, hebben de `Shell Command: Install 'code' command in PATH` opdracht in VS-Code, en vervolgens kunt u ook verwijzen naar de Code van de VS-script op`/usr/local/bin/code`

### <a name="default-install-path-for-pycharm"></a>Standaard installatiepad voor PyCharm 

* Windows 32-bits - `C:\Program Files (x86)\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm.exe`. 
* Windows 64-bits - `C:\Program Files\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm64.exe`.
* Mac OS - Selecteer het pad .app, bijvoorbeeld '/ Applications/PyCharm CE.app' en de app voegt de rest van het pad voor u. Het volledige pad naar het uitvoerbare bestand standaard `/Applications/PyCharm CE.app/Contents/MacOS/pycharm`. Wellicht vindt u ook PyCharm op de bin-map`/usr/local/bin/charm`

## <a name="open-project-in-ide"></a>Project openen in IDE 
Zodra de configuratie voltooid is, kunt u een Azure Machine Learning-project openen door het openen van de **bestand** menu in Azure Machine Learning Workbench, klikt u vervolgens op **Project openen (< IDE_Name >)**. Deze actie wordt het huidige actieve project in de geconfigureerde IDE geopend. _Opmerking: Als u niet in een project de **Project openen (< IDE_Name >)** wordt uitgeschakeld._

## <a name="configuring-the-integrated-terminal-in-visual-studio-code"></a>De geïntegreerde terminal configureren in Visual Studio Code

### <a name="windows"></a>Windows 
We hebben de standaardshell cmd in plaats van PowerShell worden overschreven. Op te klikken op **Project openen (< IDE_Name >)**, ziet u de opdrachtprompt: 

_Sta shell: `C:\windows\System32\cmd.exe` (gedefinieerd als een instelling van de werkruimte) moet worden gestart in de terminal in?_

Antwoord `yes` om toe te staan voor het configureren van de shell naadloos werken met Azure ML-Workbench opdrachtregel-interface.

### <a name="mac"></a>Mac
Om uit te voeren een `az` opdracht met behulp van Visual Studio-Code geïntegreerd terminal op Mac, moet u handmatig in te stellen de `PATH` moet dezelfde waarde als `PATH` in het project `.vscode/settings.json` bestand onder de sleutel `terminal.integrated.env.osx`. U kunt dit doen door met de volgende opdracht in de terminal:`PATH=<PATH in .vscode/settings>`
