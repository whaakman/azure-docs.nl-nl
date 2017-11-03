---
title: Opties voor het opsporen van Azure-cloudservices | Microsoft Docs
description: Foutopsporing van Azure-cloudservices
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: ghogen
editor: 
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: mikejo
ms.openlocfilehash: 196a39a75f8a3151c277f0b2a7944e6886192171
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="learn-the-various-ways-to-debug-an-azure-cloud-service"></a>Meer informatie over de verschillende manieren fouten opsporen in een Azure cloudservice
In dit artikel bevat koppelingen naar de verschillende manieren fouten opsporen in een Azure-cloudservice. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Foutopsporing van een Azure-cloud-service in Visual Studio
Bespaart u tijd en geld met behulp van de Azure-rekenemulator fouten opsporen in uw cloudservice op een lokale machine. U kunt door foutopsporing lokaal een service in voordat u deze implementeert, betrouwbaarheid en prestaties verbeteren zonder te betalen voor compute-tijd. Echter een aantal fouten optreden alleen wanneer u een cloudservice in Azure uitvoert. Fouten die optreden alleen wanneer u een cloudservice in Azure uitvoeren kunnen fouten worden opgespoord door in te schakelen foutopsporing op afstand wanneer u uw service publiceert en vervolgens het foutopsporingsprogramma koppelen aan een rolexemplaar. Zie voor meer informatie [fouten opsporen in uw cloudservice op de lokale computer](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-azure-diagnostics"></a>Met behulp van Azure Diagnostics 
Azure Diagnostics kunt u gedetailleerde informatie van de code die wordt uitgevoerd binnen rollen, meld u of de functies worden uitgevoerd in de ontwikkelomgeving of in Azure. Zie voor meer informatie [Azure Diagnostics inschakelen in Azure Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=400450).

## <a name="using-intellitrace"></a>Met behulp van IntelliTrace 
Als u van Visual Studio Enterprise gebruikmaakt schrijven rollen gericht .NET Framework 4.5, kunt u IntelliTrace inschakelen op het moment dat u een Azure-cloudservice implementeren vanuit Visual Studio. IntelliTrace voorziet in een logboek die u met Visual Studio gebruiken kunt voor foutopsporing van uw toepassing als in Azure werd uitgevoerd. Zie voor meer informatie [foutopsporing van een gepubliceerde cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Foutopsporing op afstand 
Foutopsporing op afstand op uw cloudservices op het moment wanneer u de cloudservice vanuit Visual Studio implementeert, kunt u inschakelen. Als u foutopsporing op afstand voor een implementatie inschakelt, worden externe foutopsporing services zijn geïnstalleerd op de virtuele machines met elk rolexemplaar. Deze services - zoals `msvsmon.exe` - geen invloed hebben op prestaties of leiden tot extra kosten. Zie voor meer informatie [fouten opsporen in een cloudservice in Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Volgende stappen
- [Een Azure-cloudservice of virtuele machine in Visual Studio-foutopsporing](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -informatie over de details van fouten opsporen in Azure-cloudservices.