---
title: Emulator Express gebruiken voor het uitvoeren en fouten opsporen in een Azure-cloud-service op een lokale machine | Microsoft Docs
description: Emulator Express gebruiken voor het uitvoeren en fouten opsporen in een cloudservice op een lokale computer
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: mikejo
ms.openlocfilehash: 3432e8778b2c5b5fd4db53a82ca23b55d75bcac5
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969536"
---
# <a name="using-emulator-express-to-run-and-debug-an-azure-cloud-service-on-a-local-machine"></a>Emulator Express gebruiken voor het uitvoeren en fouten opsporen in een Azure-cloud-service op een lokale computer
U kunt met behulp van Emulator Express, testen en fouten opsporen in een cloudservice zonder Visual Studio als beheerder uit te voeren. De instellingen van uw project Emulator Express of de volledige emulator, afhankelijk van de vereisten van uw cloudservice te gebruiken, kunt u instellen. Zie voor meer informatie over de volledige emulator, [een Azure-toepassing uitvoert in de Rekenemulator](storage/common/storage-use-emulator.md).

## <a name="using-emulator-express-in-visual-studio"></a>Emulator Express gebruiken in Visual Studio
Wanneer u een Azure-project in de Azure SDK 2.3 of hoger maakt, wordt Emulator Express automatisch gebruikt. Voor bestaande projecten die zijn gemaakt met een eerdere versie van de Azure SDK, gebruiken de volgende stappen uit om te selecteren Emulator Express:

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **eigenschappen**.

1. Selecteer in de projecten eigenschappenpagina's, de **Web** tabblad.

    ![Eigenschappen voor een Azure-cloud service-project](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. Onder **lokale Ontwikkelaarsserver**, selecteer **optie Gebruik IIS Express**.

1. Onder **Emulator**, selecteer **gebruik Emulator Express**.
   
1. Als u wilt starten op de Emulator Express, voer de volgende opdracht achter de opdrachtprompt: 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a>Emulator Express beperkingen
De volgende problemen zijn bekende beperkingen van de Emulator Express: 

- Emulator Express is niet compatibel met IIS-webserver.
- Uw cloudservice kan meerdere rollen bevatten, maar elke rol is beperkt tot één exemplaar.
- U geen toegang tot de poortnummers minder dan 1000. Als u een verificatieprovider die normaal gesproken een poort minder dan 1000 gebruikt gebruikt, moet u mogelijk deze waarde wijzigen in een poortnummer dat hoger is dan 1000.
- De beperkingen die van toepassing op de Azure-Rekenemulator gelden ook voor de Emulator Express. Bijvoorbeeld geen instanties van de meer dan 50 per implementatie. Zie voor meer informatie over de Azure-Rekenemulator [een Azure-toepassing uitvoert in de Rekenemulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).

## <a name="next-steps"></a>Volgende stappen
[Foutopsporing in Azure cloudservices](https://msdn.microsoft.com/library/azure/ee405479.aspx)
