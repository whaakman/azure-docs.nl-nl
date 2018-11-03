---
title: Opties voor het opsporen van fouten in Azure cloud services | Microsoft Docs
description: Foutopsporing in Azure cloudservices
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: mikejo
ms.openlocfilehash: e4ca0f710257530a05acdec2b240ae2870d4fa80
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969305"
---
# <a name="learn-the-various-ways-to-debug-an-azure-cloud-service"></a>Informatie over de verschillende manieren om fouten opsporen in een Azure-cloud-service
In dit artikel bevat koppelingen naar de verschillende manieren om fouten opsporen in een Azure-cloud-service. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Foutopsporing in een Azure-cloud-service in Visual Studio
U bespaart tijd en geld met behulp van de Azure-rekenemulator fouten opsporen in uw cloudservice op een lokale computer. U kunt door een service-lokaal foutopsporing voordat u deze implementeert, betrouwbaarheid en prestaties verbeteren zonder te hoeven betalen voor de rekentijd. Echter enkele fouten optreden wanneer u een cloudservice in Azure uitvoert. Fouten die optreden wanneer u een cloudservice in Azure uitvoert, kunnen worden opgespoord door in te schakelen voor foutopsporing op afstand bij het publiceren van uw service en vervolgens het foutopsporingsprogramma koppelen aan een rolinstantie. Zie voor meer informatie, [fouten opsporen in uw cloudservice op de lokale computer](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-intellitrace"></a>Met behulp van IntelliTrace 
Als u van Visual Studio Enterprise gebruikmaakt schrijven rollen gericht .NET Framework 4.5, kunt u IntelliTrace inschakelen op het moment dat u een Azure-cloudservice implementeren vanuit Visual Studio. IntelliTrace biedt een logboek dat u met Visual Studio fouten opsporen in uw toepassing kunt alsof deze worden uitgevoerd in Azure. Zie voor meer informatie, [opsporen van fouten in een gepubliceerde cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Foutopsporing op afstand 
U kunt foutopsporing op afstand inschakelen op uw cloud-services op het moment waarop u de service in de cloud vanuit Visual Studio implementeren. Als u ervoor kiest om in te schakelen foutopsporing op afstand voor een implementatie, is externe foutopsporing services zijn geïnstalleerd op de virtuele machines waarop elke rolinstantie wordt uitgevoerd. Deze services,-zoals `msvsmon.exe` - geen invloed hebben op prestaties of leiden tot extra kosten. Zie voor meer informatie, [fouten opsporen in een cloudservice in Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Volgende stappen
- [Foutopsporing in een Azure-cloudservice of virtuele machine in Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -informatie over de details van fouten opsporen in Azure-cloudservices.