---
title: Instellen van Azure Stream Analytics-hulpprogramma's voor Visual Studio
description: Dit artikel beschrijft de vereisten voor de installatie en het instellen van de Azure Stream Analytics-hulpprogramma's voor Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 47bdbc2297fb1e1ef24f13710391b4e9d154d37c
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49983819"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Azure Stream Analytics-hulpprogramma's voor Visual Studio installeren
Azure Stream Analytics-hulpprogramma's ondersteunen Visual Studio 2017, 2015 en 2013. In dit artikel wordt beschreven hoe u het installeren en verwijderen van de hulpprogramma's.

Zie voor meer informatie over het gebruik van de hulpprogramma's, [Stream Analytics-hulpprogramma's voor Visual Studio](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Installeren
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Download [Visual Studio 2017 (15.3 of hoger)](https://www.visualstudio.com/). Enterprise- (Ultimate/Premium), Professional- en Community-edities worden ondersteund. De Express-editie wordt niet ondersteund. 
* Stream Analytics-hulpprogramma's maken deel uit van de **Azure-ontwikkeling** en **gegevensopslag en verwerking** werkbelastingen in Visual Studio 2017. Schakel een van deze twee workloads in als onderdeel van uw installatie van Visual Studio.

Schakel de **gegevensopslag en verwerking** workload zoals wordt weergegeven:

![Gegevens gegevensopslag en verwerkingsworkload is geselecteerd](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Schakel de **Azure-ontwikkeling** workload zoals wordt weergegeven:

![Azure-ontwikkelworkload is geselecteerd](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)

* Kies in het menu Extra **extensies en Updates**. Zoeken naar Azure Data Lake en Stream Analytics-hulpprogramma's in de geïnstalleerde extensies en klik op **Update** naar de nieuwste extensie installeren. 

![Visual Studio-extensies en updates](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-extensions-updates.png)

### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
* Visual Studio 2015 of Visual Studio 2013 Update 4 installeren. Enterprise- (Ultimate/Premium), Professional- en Community-edities worden ondersteund. De Express-editie wordt niet ondersteund. 
* Installeer de Microsoft Azure SDK voor .NET versie 2.7.1 of hoger met behulp van de [Web platform installer](https://www.microsoft.com/web/downloads/platform.aspx).
* Installeer [Azure Stream Analytics-hulpprogramma's voor Visual Studio](https://www.microsoft.com/en-us/download/details.aspx?id=49504).

## <a name="update"></a>Update

### <a name="visual-studio-2017"></a>Visual Studio 2017
De nieuwe versie herinnering wordt weergegeven in de Visual Studio-melding.

![Visual Studio de nieuwe versie herinnering](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-new-version-reminder-vs-tools.png)

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 en Visual Studio 2015
De geïnstalleerde Stream Analytics-hulpprogramma's voor Visual Studio controleren op nieuwe versies automatisch. Volg de instructies in het pop-upvenster voor het installeren van de meest recente versie. 


## <a name="uninstall"></a>Verwijderen

### <a name="visual-studio-2017"></a>Visual Studio 2017
Dubbelklik op het installatieprogramma voor Visual Studio en selecteer **wijzigen**. Wissen de **Azure Data Lake en Stream Analytics-hulpprogramma's** selectievakje vanuit de **gegevensopslag en verwerking** workload of de **Azure-ontwikkeling** werkbelasting.

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 en Visual Studio 2015
Ga naar het Configuratiescherm en verwijderen **Microsoft Azure Data Lake en Stream Analytics-hulpprogramma's voor Visual Studio**.





