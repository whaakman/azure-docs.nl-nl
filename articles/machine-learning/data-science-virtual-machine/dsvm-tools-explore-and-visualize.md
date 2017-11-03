---
title: Gegevens te verkennen en visualisatie-hulpprogramma's - Azure | Microsoft Docs
description: Gegevens te verkennen en visualisatie-hulpprogramma's voor de gegevens wetenschappelijke virtuele Machine.
keywords: hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, hulpprogramma's voor gegevenswetenschap, gegevenswetenschap linux
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: d8f0616f17dbaa02082492cc1c68f1d989ea5aae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Gegevens te verkennen en visualisatie's op de gegevens wetenschappelijke virtuele Machine

Een belangrijke stap in de gegevenswetenschap is het begrijpen van de gegevens. Visualisatie en gegevens verkenning hulpmiddelen kunt inzicht in gegevens te versnellen. Hier volgen enkele extra opgegeven op de DSVM die zodat deze sleutel stap. 

## <a name="apache-drill"></a>Apache Inzoomen
|    |           |
| ------------- | ------------- |
| Wat is het?   | Open-source SQL query-engine van Big data    |
| Ondersteunde DSVM versies      | Windows, Linux  |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Geïnstalleerd in `/dsvm/tools/drill*` ingesloten alleen in modus   |
| Doorgaans worden gebruikt      |  Gegevensverkenning in situ zonder ETL. Querytabellen verschillende bronnen en indelingen includign CSV, JSON, relationele, Hadoop     |
| Hoe gebruik / uitvoeren?      | Snelkoppeling op het bureaublad  <br/> [Aan de slag met inzoomen in 10 minuten](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| Bijbehorende hulpprogramma's op de DSVM      |   Rammelaar, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Wat is het?   |  Weka is een verzameling van machine learning-algoritmen voor datamining taken. De algoritmen kunnen rechtstreeks toegepast op een dataset of aangeroepen vanuit uw eigen Java-code. Weka bevat hulpprogramma's voor gegevens vooraf verwerken, classificatie, regressie, clustering, association rules- en visualisatie. |
| Ondersteunde DSVM edities     | Windows, Linux     |
| Doorgaans worden gebruikt      | Algemene ML hulpprogramma     |
| Hoe gebruik / uitvoeren?      | Op Windows, zoeken naar Weka in het Menu Start. Op Linux, meld u aan met X2Go en vervolgens gaat u naar toepassingen-ontwikkeling > -> Weka. |
| Koppelingen naar voorbeelden      | [Voorbeelden van weka](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Bijbehorende hulpprogramma's op de DSVM      |LightGBM, ratel, Xgboost   |

## <a name="rattle"></a>Rammelaar
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Een grafische gebruikersinterface voor gegevensanalyse R gebruiken   |
| Ondersteunde DSVM edities     | Windows, Linux     |
| Doorgaans worden gebruikt      | Algemene UI gegevensanalyse hulpprogramma voor R    |
| Hoe gebruik / uitvoeren?      | UI-hulpprogramma. In Windows, start een opdrachtprompt, voert u R binnen R uitvoeren `rattle()`. Op Linux, verbinding maken met X2Go, een terminal starten, uitvoeren van R binnen R uitvoeren `rattle()`. |
| Koppelingen naar voorbeelden      | [Rammelaar](https://togaware.com/onepager/) |
| Bijbehorende hulpprogramma's op de DSVM      |LightGBM, Weka, Xgboost   |

## <a name="powerbi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Wat is het?   | Interactieve gegevensvisualisatie en BI-hulpprogramma    |
| Ondersteunde DSVM versies      | Windows  |
| Doorgaans worden gebruikt      |  Gegevensvisualisatie en het bouwen van Dashboards   |
| Hoe gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Bijbehorende hulpprogramma's op de DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |

