---
title: Het configureren van Azure Machine Learning Workbench om te werken met een IDE?  | Microsoft Docs
description: Een handleiding voor het configureren van Azure Machine Learning Workbench om te werken met uw IDE.
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 18692fe631a7e1349ead6bc68a87934e6d030913
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644386"
---
# <a name="how-to-configure-azure-machine-learning-workbench-to-work-with-an-ide"></a>Het configureren van Azure Machine Learning Workbench om te werken met een IDE 

Azure Machine Learning Workbench kan worden geconfigureerd om te werken met populaire Python IDE (Integrated Development Environment). Hiermee kunt een goede data science-ontwikkelervaring verplaatsen tussen de gegevens voor te bereiden, code schrijven, uitvoeren bijhouden en uitoefening. De ondersteunde IDE's zijn momenteel:
- Microsoft Visual Studio Code 
- JetBrain PyCharm 

## <a name="configure-workbench"></a>Workbench configureren
1. Klik op de **bestand** menu in de linkerbovenhoek van de app. 
2. Selecteer de **Project IDE configureren** optie in de flyout 
3. Typ in `VS Code` of `PyCharm` in de **naam** veld (de naam is willekeurig)
4. Geef de locatie op de IDE uitvoerbaar bestand (voltooid met de naam van uitvoerbaar bestand en de extensie) in **uitvoeringspad**

### <a name="default-install-path-for-visual-studio-code"></a>Standaardinstallatiepad voor Visual Studio Code  

* Windows 32-bits- `C:\Program Files (x86)\Microsoft VS Code\Code.exe`
* Windows 64-bits- `C:\Program Files\Microsoft VS Code\Code.exe`
* macOS - selecteert u het pad .app bijvoorbeeld `/Applications/Visual Studio Code.app`, en de app wordt toegevoegd voor de rest van het pad voor u. Het volledige pad naar het uitvoerbare bestand standaard `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code`. Als u hebt uitgevoerd de `Shell Command: Install 'code' command in PATH` opdracht in VS Code, en u kunt ook verwijzen naar de VS Code-script aan `/usr/local/bin/code`

### <a name="default-install-path-for-pycharm"></a>Standaardinstallatiepad voor PyCharm 

* Windows 32-bits - `C:\Program Files (x86)\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm.exe`. 
* Windows 64-bits - `C:\Program Files\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm64.exe`.
* macOS - Selecteer de .app-pad, bijvoorbeeld "/ Applications/PyCharm CE.app" en de app wordt toegevoegd voor de rest van het pad voor u. Het volledige pad naar het uitvoerbare bestand standaard `/Applications/PyCharm CE.app/Contents/MacOS/pycharm`. U vindt ook PyCharm op de bin-map `/usr/local/bin/charm`

## <a name="open-project-in-ide"></a>Het project openen in de IDE 
Nadat de configuratie voltooid is, kunt u een Azure Machine Learning-project openen door het openen van de **bestand** in het menu in Azure Machine Learning Workbench, klikt u vervolgens op **Project openen (< IDE_Name >)**. Deze actie wordt het huidige actieve project geopend in de geconfigureerde IDE. _Opmerking: Als u zich niet in een project, de **Project openen (< IDE_Name >)** wordt uitgeschakeld._

## <a name="configuring-the-integrated-terminal-in-visual-studio-code"></a>Configureren van de geïntegreerde terminal in Visual Studio Code

### <a name="windows"></a>Windows 
We hebben de standaardshell cmd in plaats van PowerShell worden overschreven. Op te klikken op **Project openen (< IDE_Name >)**, ziet u een opdrachtprompt: 

_Laat u de shell: `C:\windows\System32\cmd.exe` (gedefinieerd als een instelling van de werkruimte) moet worden gestart in de terminal?_

Antwoord `yes` om toe te staan voor het configureren van de shell voor een naadloze samenwerking met Azure ML Workbench-opdrachtregelinterface.

### <a name="mac"></a>Mac
Om uit te voeren een `az` opdracht met behulp van Visual Studio-Code geïntegreerde terminal op Mac, moet u handmatig instellen de `PATH` moet dezelfde waarde als `PATH` in van het project `.vscode/settings.json` bestand onder de sleutel `terminal.integrated.env.osx`. U kunt dit doen door het uitvoeren van de volgende opdracht uit in de terminal: `PATH=<PATH in .vscode/settings>`
