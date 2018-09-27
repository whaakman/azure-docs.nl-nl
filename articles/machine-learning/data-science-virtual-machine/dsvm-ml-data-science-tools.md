---
title: Machine learning en data science-hulpprogramma's - Azure | Microsoft Docs
description: Machine learning en data science-hulpprogramma 's
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 9a9dc868c4f22f95ca5027e3c95513d176c69eac
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392395"
---
# <a name="machine-learning-and-data-science-tools"></a>Machine learning en data science-hulpprogramma 's
De Data Science Virtual Machine (DSVM) is een uitgebreide set hulpprogramma's en bibliotheken voor machine learning is beschikbaar in populaire talen zoals Python, R, Julia. 

Hier volgen enkele van de machine learning-hulpprogramma's en bibliotheken op de DSVM. 

## <a name="azure-machine-learning-servicehttpsdocsmicrosoftcomazuremachine-learningserviceoverview-what-is-azure-ml-sdk"></a>[Azure Machine Learning-Service](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml) SDK
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Azure Machine Learning-service is een cloudservice die u gebruiken kunt om te ontwikkelen en implementeren van machine learning-modellen.  Bij het bouwen, trainen, schalen en beheren met behulp van Python-SDK, kunt u uw modellen bijhouden. Modellen als containers implementeren en voer ze uit op de cloud, on-premises of in de IoT Edge.   |
| Ondersteunde DSVM-edities     | Windows (Conda-omgeving: AzureML), Linux (Conda-omgeving: py36)    |
| Wordt doorgaans gebruikt      | Algemene ML-Platform      |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Geïnstalleerd met GPU-ondersteuning   |
| Hoe kan ik gebruik / uitvoeren?      | Als Python-SDK en hulpprogramma voor Azure vanaf de opdrachtregel (AZ CLI). Activeren naar conda-omgeving `AzureML` op Windows-editie of `py36` op Linux-editie.      |
| Koppelingen naar voorbeelden      | Voorbeelden van Jupyter notebooks zijn opgenomen in de `AzureML` map onder notitieblokken  |
| Meer hulpprogramma's op de DSVM      | Visual Studio Code, Jupyter   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Wat is het?   |    Snelle, draagbare en gedistribueerde Gradient Boosting (GBDT, GBRT of GBM)-bibliotheek voor Python, R, Java, Scala, C++ en meer. Wordt uitgevoerd op één computer, Hadoop, Spark    |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemene ML-bibliotheek      |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Geïnstalleerd met GPU-ondersteuning   |
| Hoe kan ik gebruik / uitvoeren?      | Als Python-bibliotheek (2.7 en 3.5), R-pakket en de pad-opdrachtregelprogramma (`C:\dsvm\tools\xgboost\bin\xgboost.exe` voor Windows, `/dsvm/tools/xgboost/xgboost` voor Linux)    |
| Koppelingen naar voorbeelden      | Voorbeelden zijn opgenomen op de virtuele machine, in `/dsvm/tools/xgboost/demo` op Linux en `C:\dsvm\tools\xgboost\demo` op Windows   |
| Meer hulpprogramma's op de DSVM      | LightGBM, MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Vowpal Wabbit (ook wel bekend als ' VW") is een open source snel out-van-core learning systeembibliotheek    |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemene ML-bibliotheek      |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      |  Windows - msi-installatieprogramma, Linux - apt-get |
| Hoe kan ik gebruik / uitvoeren?      | Als een op pad-opdrachtregelprogramma (`C:\Program Files\VowpalWabbit\vw.exe` op Windows, `/usr/bin/vw` op Linux)    |
| Koppelingen naar voorbeelden      | [VowPal Wabbit-voorbeelden](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| Meer hulpprogramma's op de DSVM      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Wat is het?   |  Weka is een verzameling van machine learning-algoritmen voor datamining taken. De algoritmen kunnen die rechtstreeks op een gegevensset toegepast of aangeroepen vanuit uw eigen Java-code. Weka bevat hulpprogramma's voor het vooraf verwerken van gegevens, classificatie, regressie, clustering, koppelingsregels voor en visualisatie. |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemene ML-hulpprogramma     |
| Hoe kan ik gebruik / uitvoeren?      | Op Windows, zoeken naar Weka in het Menu Start. Aanmelden op Linux met X2Go en navigeer naar toepassingen -> ontwikkeling Weka ->. |
| Koppelingen naar voorbeelden      | [Voorbeelden van weka](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Meer hulpprogramma's op de DSVM      |LightGBM, ratel, XGBooost   |

## <a name="rattle"></a>Rammelaar
|    |           |
| ------------- | ------------- |
| Wat is het?   |   Een grafische gebruikersinterface voor datamining met R   |
| Ondersteunde DSVM-edities     | Windows, Linux     |
| Wordt doorgaans gebruikt      | Algemene UI gegevensanalyse hulpprogramma voor R    |
| Hoe kan ik gebruik / uitvoeren?      | UI-hulpprogramma. Op Windows, start u een opdrachtprompt, uitvoeren van R, klikt u vervolgens in R uitvoeren `rattle()`. Op Linux, verbinding maken met X2Go, een terminal starten, uitvoeren van R, klikt u vervolgens in R uitvoeren `rattle()`. |
| Koppelingen naar voorbeelden      | [Rammelaar](https://togaware.com/onepager/) |
| Meer hulpprogramma's op de DSVM      |LightGBM, Weka, XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| Wat is het?   | Een snelle, gedistribueerde, hoogwaardige verloop versterking (GBDT, GBRT, GBM of DATAMART) framework op basis van algoritmen voor een beslissingsstructuur, die wordt gebruikt voor classificatie, classificatie en veel andere machine learning-taken.    |
| Ondersteunde DSVM-versies      | Windows, Linux    |
| Wordt doorgaans gebruikt      | Voor algemene doeleinden kleurovergang Gradient boosting-framework      |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      | LightGBM is geïnstalleerd op Windows, als een Python-pakket. Op Linux, uitvoerbare bestand van de opdrachtregel wordt `/opt/LightGBM/lightgbm`, het R-pakket is geïnstalleerd en Python-pakketten zijn geïnstalleerd.     |
| Koppelingen naar voorbeelden      | [Handleiding voor LightGBM](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| Meer hulpprogramma's op de DSVM      | MXNet, XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| Wat is het?   | Een open-source AI-platform ondersteunen in het geheugen, gedistribueerde, snelle en schaalbare machine learning  |
| Ondersteunde DSVM-versies      | Linux   |
| Wordt doorgaans gebruikt      | Algemeen gebruik gedistribueerde, schaalbare ML   |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?      | H2O is geïnstalleerd in `/dsvm/tools/h2o`.      |
| Hoe kan ik gebruik / uitvoeren?      | Verbinding maken met de virtuele machine met behulp van X2Go. Start een nieuwe terminal en voer `java -jar /dsvm/tools/h2o/current/h2o.jar`. Vervolgens start u een webbrowser en maak verbinding met `http://localhost:54321`.      |
| Koppelingen naar voorbeelden      | Voorbeelden zijn beschikbaar op de virtuele machine in Jupyter onder `h2o` directory.      |
| Meer hulpprogramma's op de DSVM      | Apache Spark, MXNet, XGBoost, mousserende Water, Deep Water    |

Er zijn verschillende andere ML-bibliotheken op de DSVM, zoals het populaire `scikit-learn` pakket dat als onderdeel van de Anaconda Python-distributie die is geïnstalleerd op de DSVM wordt geleverd. Zorg ervoor dat de lijst met pakketten die beschikbaar zijn in Python, R en Julia door het uitvoeren van de respectieve pakketmanagers Bekijk. 
