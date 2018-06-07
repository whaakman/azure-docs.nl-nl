---
title: Instellen van Azure Stream Analytics-hulpprogramma's voor Visual Studio
description: In dit artikel beschrijft de vereisten voor de installatie en het instellen van de Azure Stream Analytics-hulpprogramma's voor Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 94ed603990859d12f709e4a6121e3736221cf10a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651175"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Azure Stream Analytics-hulpprogramma's voor Visual Studio installeren
Azure Stream Analytics-hulpprogramma's ondersteuning voor Visual Studio 2017 2015 en 2013. In dit artikel wordt beschreven hoe installeren en verwijderen van de hulpprogramma's.

Zie voor meer informatie over het gebruik van de hulpprogramma's [Stream Analytics-tools voor Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="install"></a>Installeren
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Download [Visual Studio 2017 (15.3 of hoger)](https://www.visualstudio.com/). Enterprise (Ultimate/Premium), Professional en Community-edities worden ondersteund. Express edition wordt niet ondersteund. 
* Stream Analytics-hulpprogramma's maken deel uit van de **ontwikkelen van Azure** en **gegevensopslag en verwerking** werkbelastingen in Visual Studio 2017. Schakel een van deze twee workloads in als onderdeel van uw installatie van Visual Studio.

Schakel de **gegevensopslag en verwerking** werkbelasting zoals wordt weergegeven:

![Gegevens opgeslagen en verwerkt werkbelasting is geselecteerd](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Schakel de **ontwikkelen van Azure** werkbelasting zoals wordt weergegeven:

![Ontwikkelen van Azure werkbelasting is geselecteerd](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)


### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
* Visual Studio 2015 of Visual Studio 2013 Update 4 installeren. Enterprise (Ultimate/Premium), Professional en Community-edities worden ondersteund. Express edition wordt niet ondersteund. 
* Installeer de Microsoft Azure SDK voor .NET versie 2.7.1 of hoger met behulp van de [webplatforminstallatieprogramma](http://www.microsoft.com/web/downloads/platform.aspx).
* Installeer [Azure Stream Analytics-tools voor Visual Studio](http://aka.ms/asatoolsvs).

## <a name="update"></a>Update

### <a name="visual-studio-2017"></a>Visual Studio 2017
De nieuwe versie herinnering weergegeven in de Visual Studio-melding. 

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 en Visual Studio 2015
De geïnstalleerde Stream Analytics-tools voor Visual Studio controleren op nieuwe versies automatisch. Volg de instructies in het pop-upvenster voor het installeren van de meest recente versie. 


## <a name="uninstall"></a>Verwijderen

### <a name="visual-studio-2017"></a>Visual Studio 2017
Dubbelklik op het installatieprogramma van de Visual Studio en selecteer **wijzigen**. Schakel de **Stream Analytics-hulpprogramma's en Azure Data Lake** selectievakje vanuit de **gegevensopslag en verwerking** werkbelasting of de **ontwikkelen van Azure** werkbelasting.

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 en Visual Studio 2015
Ga naar Configuratiescherm en verwijder **Microsoft Azure Data Lake en Stream Analytics-tools voor Visual Studio**.





