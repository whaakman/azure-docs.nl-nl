---
title: Machine learning en wetenschappelijke-hulpprogramma's - Azure | Microsoft Docs
description: Machine learning en wetenschappelijke hulpprogramma 's
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
ms.openlocfilehash: c772fa16b94d09e0fc0450ce86503553c26f8a24
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="machine-learning-and-data-science-tools"></a>Machine learning en wetenschappelijke hulpprogramma 's
De gegevens wetenschappelijke virtuele Machine (DSVM) heeft een uitgebreide set hulpprogramma's en bibliotheken voor machine learning in populaire talen zoals Python, R, Julia beschikbaar. 

Hier zijn enkele van de machine learning-hulpprogramma's en bibliotheken op de DSVM. 

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Wat is het?   |    Snelle, Portable en gedistribueerd kleurovergang versterking (GBDT, GBRT of GBM)-bibliotheek, voor Python, R-, Java, Scala, C++ en meer. Wordt uitgevoerd op één computer, Hadoop, Spark    |
| Ondersteunde DSVM edities     | Windows, Linux     |
| Doorgaans worden gebruikt      | Algemene ML-bibliotheek      |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Geïnstalleerd met GPU-ondersteuning   |
| Hoe gebruik / uitvoeren?      | Als de Python-bibliotheek (2.7 en 3.5), R-pakket en de pad-opdrachtregelprogramma (`C:\dsvm\tools\xgboost\bin\xgboost.exe` voor Windows, `/dsvm/tools/xgboost/xgboost` voor Linux)    |
| Koppelingen naar voorbeelden      | Voorbeelden zijn opgenomen op de VM in `/dsvm/tools/xgboost/demo` op Linux en `C:\dsvm\tools\xgboost\demo` in Windows   |
| Bijbehorende hulpprogramma's op de DSVM      | LightGBM, MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Vowpal Wabbit (ook wel bekend als een ' code') is een open-source snel out-van-core learning systeembibliotheek    |
| Ondersteunde DSVM edities     | Windows, Linux     |
| Doorgaans worden gebruikt      | Algemene ML-bibliotheek      |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Windows - installatieprogramma msi van Linux - apt get |
| Hoe gebruik / uitvoeren?      | Als op pad-opdrachtregelprogramma (`C:\Program Files\VowpalWabbit\vw.exe` op Windows, `/usr/bin/vw` op Linux)    |
| Koppelingen naar voorbeelden      | [Voorbeelden van VowPal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| Bijbehorende hulpprogramma's op de DSVM      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Wat is het?   |  Weka is een verzameling van machine learning-algoritmen voor datamining taken. De algoritmen kunnen rechtstreeks toegepast op een dataset of aangeroepen vanuit uw eigen Java-code. Weka bevat hulpprogramma's voor gegevens vooraf verwerken, classificatie, regressie, clustering, association rules- en visualisatie. |
| Ondersteunde DSVM edities     | Windows, Linux     |
| Doorgaans worden gebruikt      | Algemene ML hulpprogramma     |
| Hoe gebruik / uitvoeren?      | Op Windows, zoeken naar Weka in het Menu Start. Op Linux, meld u aan met X2Go en vervolgens gaat u naar toepassingen-ontwikkeling > -> Weka. |
| Koppelingen naar voorbeelden      | [Voorbeelden van weka](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Bijbehorende hulpprogramma's op de DSVM      |LightGBM, ratel, XGBooost   |

## <a name="rattle"></a>Rammelaar
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Een grafische gebruikersinterface voor gegevensanalyse R gebruiken   |
| Ondersteunde DSVM edities     | Windows, Linux     |
| Doorgaans worden gebruikt      | Algemene UI gegevensanalyse hulpprogramma voor R    |
| Hoe gebruik / uitvoeren?      | UI-hulpprogramma. In Windows, start een opdrachtprompt, voert u R binnen R uitvoeren `rattle()`. Op Linux, verbinding maken met X2Go, een terminal starten, uitvoeren van R binnen R uitvoeren `rattle()`. |
| Koppelingen naar voorbeelden      | [Rammelaar](https://togaware.com/onepager/) |
| Bijbehorende hulpprogramma's op de DSVM      |LightGBM, Weka, XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| Wat is het?   | Een snelle, gedistribueerde, hoge prestaties verloop versterking (GBRT GBDT GBM of DATAMART) framework op basis van decision tree algoritmen, voor ranking, classificatie en veel andere machine learning-taken.    |
| Ondersteunde DSVM versies      | Windows, Linux    |
| Doorgaans worden gebruikt      | Voor algemene doeleinden kleurovergang prestatieverbetering framework      |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      | Op Windows, LightGBM geïnstalleerd als een Python-pakket. Op Linux, het opdrachtregelprogramma uitvoerbare bestand zich in `/opt/LightGBM/lightgbm`, het R-pakket is geïnstalleerd en Python-pakketten zijn geïnstalleerd.     |
| Koppelingen naar voorbeelden      | [LightGBM handleiding](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| Bijbehorende hulpprogramma's op de DSVM      | MXNet, XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| Wat is het?   | Een open source AI-platform die in het geheugen, gedistribueerde, snelle en schaalbare machine learning ondersteunen  |
| Ondersteunde DSVM versies      | Linux   |
| Doorgaans worden gebruikt      | Voor algemene doeleinden gedistribueerd, schaalbaar ML   |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      | H2O is geïnstalleerd in `/dsvm/tools/h2o`.      |
| Hoe gebruik / uitvoeren?      | Verbinding maken met de virtuele machine met behulp van X2Go. Start een nieuwe terminal en voer `java -jar /dsvm/tools/h2o/current/h2o.jar`. Vervolgens start u een webbrowser en maak verbinding met `http://localhost:54321`.      |
| Koppelingen naar voorbeelden      | Voorbeelden zijn beschikbaar op de virtuele machine in Jupyter onder `h2o` directory.      |
| Bijbehorende hulpprogramma's op de DSVM      | Apache Spark, MXNet, XGBoost, mousserende Water, grondige Water    |

Er zijn verschillende andere ML-bibliotheken op de DSVM zoals de populaire `scikit-learn` pakket dat wordt geleverd als onderdeel van de Anaconda Python-distributie die is geïnstalleerd op de DSVM. Zorg ervoor dat de lijst met pakketten beschikbaar in Python, R en Julia uitchecken door het uitvoeren van de respectieve pakket managers. 
