---
title: Hulp middelen voor het verkennen en visualisatie van gegevens-Azure | Microsoft Docs
description: Hulp middelen voor het verkennen en visualisatie van gegevens voor de Data Science Virtual Machine.
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: vijetaj
ms.openlocfilehash: 31b05ec4fa68c3d4804000caee94b62432bdaed9
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68557772"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Hulp middelen voor gegevens exploratie en visualisaties op het Data Science Virtual Machine

Een belang rijke stap in data Science is om inzicht te krijgen in de gegevens. Met hulpprogram ma's voor visualisatie en gegevens exploratie kunt u de gegevens beter leren gebruiken. Hier vindt u enkele hulp middelen die in de DSVM zijn opgenomen om deze sleutel stap te vereenvoudigen. 

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Wat is het?   | Open Source-SQL-query-engine op Big Data    |
| Ondersteunde DSVM-versies      | Windows, Linux  |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Alleen geïnstalleerd `/dsvm/tools/drill*` in de Inge sloten modus   |
| Wordt doorgaans gebruikt      |  In-situ-gegevens exploratie zonder ETL. Query's uitvoeren op verschillende gegevens bronnen en indelingen, waaronder CSV, JSON, relationele tabellen, Hadoop     |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling bureau blad  <br/> [Aan de slag met boren in 10 minuten](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| Meer hulpprogramma's op de DSVM      |   Rattle, Fridge, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Wat is het?   |  Fridge is een verzameling machine learning algoritmen voor gegevens analyse taken. De algoritmen kunnen rechtstreeks worden toegepast op een gegevensset of worden aangeroepen vanuit uw eigen Java-code. Weka bevat hulpprogramma's voor het vooraf verwerken van gegevens, classificatie, regressie, clustering, koppelingsregels voor en visualisatie. |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemeen ML-hulp programma     |
| Hoe kan ik gebruik / uitvoeren?      | Zoek in Windows naar Fridge in het menu Start. Meld u op Linux aan met X2Go en navigeer naar Applications-> Development-> Fridge. |
| Koppelingen naar voorbeelden      | [Voorbeelden van weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Meer hulpprogramma's op de DSVM      |LightGBM, Rattle, Xgboost   |

## <a name="rattle"></a>Rammelaar
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Een grafische gebruikers interface voor gegevens analyse met R   |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemeen hulp programma voor UI-gegevens analyse voor R    |
| Hoe kan ik gebruik / uitvoeren?      | UI-hulpprogramma. Start in Windows een opdracht prompt, voer R uit en klik vervolgens in R `rattle()`run. Op Linux maakt u verbinding met X2Go, start u een Terminal, voert u R uit en `rattle()`voert u vervolgens in r uit. |
| Koppelingen naar voorbeelden      | [Rammelaar](https://togaware.com/onepager/) |
| Meer hulpprogramma's op de DSVM      |LightGBM, Fridge, Xgboost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Wat is het?   | Interactieve gegevensvisualisatie en BI-hulpprogramma    |
| Ondersteunde DSVM-versies      | Windows  |
| Wordt doorgaans gebruikt      |  Gegevensvisualisatie en het bouwen van Dashboards   |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Meer hulpprogramma's op de DSVM      |   Visual Studio 2019, Visual Studio code, Juno      |

