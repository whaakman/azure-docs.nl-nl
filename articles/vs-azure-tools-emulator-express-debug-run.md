---
title: Emulator Express gebruiken voor het uitvoeren en fouten opsporen in een Azure-cloud-service op een lokale machine | Microsoft Docs
description: Emulator Express gebruiken voor het uitvoeren en fouten opsporen in een cloudservice op een lokale computer
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d7d87045569fdc473333deae05f95d1df343b68c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="using-emulator-express-to-run-and-debug-an-azure-cloud-service-on-a-local-machine"></a>Emulator Express gebruiken voor het uitvoeren en fouten opsporen in een Azure-cloud-service op een lokale computer
U kunt testen en fouten opsporen in een cloudservice zonder Visual Studio als administrator uitvoeren met behulp van de Express-Emulator. U kunt instellen dat uw projectinstellingen Emulator Express of de volledige emulator, afhankelijk van de vereisten van uw cloudservice te gebruiken. Zie voor meer informatie over de volledige emulator, [een Azure-toepassing uitvoert in de Emulator Compute](storage/common/storage-use-emulator.md).

## <a name="using-emulator-express-in-visual-studio"></a>Met een Emulator snelle in Visual Studio
Wanneer u een Azure-project in de Azure SDK 2.3 of hoger maakt, wordt Emulator Express automatisch gebruikt. Gebruik de volgende stappen om te selecteren Emulator Express voor bestaande projecten die zijn gemaakt met een eerdere versie van de Azure SDK:

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **eigenschappen**.

1. Selecteer in de projecten eigenschappenpagina's, de **Web** tabblad.

    ![Eigenschappen voor een Azure-cloud service-project](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. Onder **lokale Ontwikkelaarsserver**, selecteer **optie Gebruik IIS Express**.

1. Onder **Emulator**, selecteer **gebruik Emulator Express**.
   
1. Start de Emulator Express, de volgende opdracht uit te voeren achter de opdrachtprompt: 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a>Emulator Express beperkingen
De volgende problemen zijn bekende beperkingen van de Emulator Express: 

- Emulator Express is niet compatibel met IIS-webserver.
- Uw cloudservice meerdere rollen kan bevatten, maar elke rol is beperkt tot één exemplaar.
- U kunt poortnummers minder dan 1000 niet openen. Als u een verificatieprovider die normaal gesproken gebruikmaakt van een poort minder dan 1000 gebruikt, moet u mogelijk deze waarde wijzigen in een poortnummer dat is meer dan 1000.
- De beperkingen die van toepassing op de Azure Compute-Emulator zijn ook van toepassing op de Express-Emulator. Er geen bijvoorbeeld meer dan 50 rolinstanties per implementatie. Zie voor meer informatie over de Azure Compute-Emulator [een Azure-toepassing uitvoert in de Emulator Compute](http://go.microsoft.com/fwlink/p/?LinkId=623050).

## <a name="next-steps"></a>Volgende stappen
[Foutopsporing van Azure-cloudservices](https://msdn.microsoft.com/library/azure/ee405479.aspx)
