---
title: Gegevens verkennen en visualiseren hulpprogramma's - Azure | Microsoft Docs
description: Gegevens verkennen en visualiseren hulpprogramma's voor de Data Science Virtual Machine.
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 165df03ec06081fe9b2e1ab84ffe7579ac457758
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855483"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Gegevens verkennen en visualiseren hulpprogramma's op de Data Science Virtual Machine

Er is een belangrijke stap in de data science om te begrijpen van de gegevens. Visualisatie en hulpprogramma's kunnen inzicht in gegevens te versnellen. Hier volgen enkele hulpprogramma's op de DSVM die het eenvoudiger maken van deze belangrijke stap. 

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Wat is het?   | Open-source SQL query-engine van Big data    |
| Ondersteunde DSVM-versies      | Windows, Linux  |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Geïnstalleerd `/dsvm/tools/drill*` in ingesloten alleen in de modus   |
| Wordt doorgaans gebruikt      |  Gegevensverkenning in situ zonder ETL. Query uitvoeren op verschillende gegevensbronnen en vormen, inclusief CSV, JSON, relationele tabellen, Hadoop     |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad  <br/> [Aan de slag met inzoomen in 10 minuten](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| Meer hulpprogramma's op de DSVM      |   Rattle, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Wat is het?   |  Weka is een verzameling van machine learning-algoritmen voor datamining taken. De algoritmen kunnen die rechtstreeks op een gegevensset toegepast of aangeroepen vanuit uw eigen Java-code. Weka bevat hulpprogramma's voor het vooraf verwerken van gegevens, classificatie, regressie, clustering, koppelingsregels voor en visualisatie. |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemene ML-hulpprogramma     |
| Hoe kan ik gebruik / uitvoeren?      | Op Windows, zoeken naar Weka in het Menu Start. Aanmelden op Linux met X2Go en navigeer naar toepassingen -> ontwikkeling Weka ->. |
| Koppelingen naar voorbeelden      | [Voorbeelden van weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Meer hulpprogramma's op de DSVM      |LightGBM, ratel, Xgboost   |

## <a name="rattle"></a>Rammelaar
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Een grafische gebruikersinterface voor datamining met R   |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemene UI gegevensanalyse hulpprogramma voor R    |
| Hoe kan ik gebruik / uitvoeren?      | UI-hulpprogramma. Op Windows, start u een opdrachtprompt, uitvoeren van R, klikt u vervolgens in R uitvoeren `rattle()`. Op Linux, verbinding maken met X2Go, een terminal starten, uitvoeren van R, klikt u vervolgens in R uitvoeren `rattle()`. |
| Koppelingen naar voorbeelden      | [Rammelaar](https://togaware.com/onepager/) |
| Meer hulpprogramma's op de DSVM      |LightGBM, Weka, Xgboost   |

## <a name="powerbi-desktop"></a>PowerBI Desktop 
|    |           |
| ------------- | ------------- |
| Wat is het?   | Interactieve gegevensvisualisatie en BI-hulpprogramma    |
| Ondersteunde DSVM-versies      | Windows  |
| Wordt doorgaans gebruikt      |  Gegevensvisualisatie en het bouwen van Dashboards   |
| Hoe kan ik gebruik / uitvoeren?      | Snelkoppeling op het bureaublad (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Meer hulpprogramma's op de DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |

