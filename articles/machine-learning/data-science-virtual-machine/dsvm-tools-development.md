---
title: Data Science Virtual Machine ontwikkelprogramma's - Azure | Microsoft Docs
description: Meer informatie over de hulpprogramma's en geïntegreerde ontwikkelingsomgevingen die vooraf geïnstalleerd op de Data Science Virtual Machine zijn.
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 7983169c2b1123c57a48471e3f4d9ad6f19c84dc
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314719"
---
# <a name="development-tools-on-the-data-science-virtual-machine"></a>Ontwikkelhulpprogramma's op de Data Science Virtual Machine

De Data Science Virtual Machine (DSVM) biedt een productieve omgeving voor het ontwikkelen van apps door bundeling van verschillende populaire hulpprogramma's en IDE. Hier zijn enkele hulpprogramma's die beschikbaar zijn op de DSVM. 

## <a name="visual-studio-2017"></a>Visual Studio 2017  

|    |           |
| ------------- | ------------- |
| Wat is het?   | Algemeen gebruik IDE      |
| Ondersteunde DSVM-versies      | Windows      |
| Wordt doorgaans gebruikt      | Ontwikkeling van software    |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      | Data Science-werkbelasting (Python / R tools), Azure-workload (Hadoop, Data Lake), Node.js, SQL Server-hulpprogramma's, [Azure Machine Learning voor Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai)    |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`)    |
| Meer hulpprogramma's op de DSVM      |     Visual Studio Code, RStudio, Juno  |

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Wat is het?   | Algemeen gebruik IDE      |
| Ondersteunde DSVM-versies      | Windows, Linux     |
| Wordt doorgaans gebruikt      | Code-editor en Git-integratie   |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) in Windows, de snelkoppeling op het bureaublad of terminal (`code`) in Linux    |
| Meer hulpprogramma's op de DSVM      |     Visual Studio 2017, RStudio, Juno  |

## <a name="rstudio--desktop"></a>RStudio Desktop 

|    |           |
| ------------- | ------------- |
| Wat is het?   | Client IDE voor R    |
| Ondersteunde DSVM-versies      | Windows, Linux      |
| Wordt doorgaans gebruikt      |  R-ontwikkeling     |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\Program Files\RStudio\bin\rstudio.exe`) op Windows, snelkoppeling op het bureaublad (`/usr/bin/rstudio`) op Linux      |
| Meer hulpprogramma's op de DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |

## <a name="rstudio--server"></a>RStudio Server 

|    |           |
| ------------- | ------------- |
| Wat is het?   | Webgebaseerde IDE voor R    |
| Ondersteunde DSVM-versies      | Linux      |
| Wordt doorgaans gebruikt      |  R-ontwikkeling     |
| Hoe kan ik gebruik / uitvoeren?      | Schakel de service met _systemctl inschakelen rstudio server_, start u de service met _systemctl start rstudio server_. U kunt vervolgens aanmelden bij RStudio Server http:\// uw-vm-ip:8787.       |
| Meer hulpprogramma's op de DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Wat is het?   | IDE-client voor Julia-taal   |
| Ondersteunde DSVM-versies      | Windows, Linux      |
| Wordt doorgaans gebruikt      |  Julia-ontwikkeling     |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\JuliaPro-0.5.1.1\Juno.bat`) op Windows, snelkoppeling op het bureaublad (`/opt/JuliaPro-VERSION/Juno`) op Linux      |
| Meer hulpprogramma's op de DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| Wat is het?   | IDE-client voor Python-taal    |
| Ondersteunde DSVM-versies      | Linux      |
| Wordt doorgaans gebruikt      |  Python-ontwikkeling     |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`/usr/bin/pycharm`) op Linux      |
| Meer hulpprogramma's op de DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio      |



## <a name="powerbi-desktop"></a>PowerBI Desktop 

|    |           |
| ------------- | ------------- |
| Wat is het?   | Interactieve gegevensvisualisatie en BI-hulpprogramma    |
| Ondersteunde DSVM-versies      | Windows  |
| Wordt doorgaans gebruikt      |  Gegevensvisualisatie en het bouwen van Dashboards   |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Meer hulpprogramma's op de DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |

