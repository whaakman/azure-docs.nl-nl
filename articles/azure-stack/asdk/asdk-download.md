---
title: Downloaden en uitpakken van de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Hierin wordt beschreven hoe u kunt downloaden en uitpakken van de Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 08/10/2018
ms.openlocfilehash: 830a7ef1f25ea52959baf992274b5f7711b646b2
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165400"
---
# <a name="download-and-extract-the-azure-stack-development-kit-asdk"></a>Downloaden en uitpakken van de Azure Stack Development Kit (ASDK)
Nadat u hebt gecontroleerd of uw development kit host-computer voldoet aan de basisvereisten voor het installeren van de ASDK, wordt de volgende stap is het downloaden en uitpakken van het implementatiepakket ASDK om op te halen van de Cloudbuilder.vhdx.

## <a name="download-the-asdk"></a>De ASDK downloaden
1. Voordat u het downloaden begint, zorg dat uw computer voldoet aan de volgende vereisten:

  - De computer moet ten minste 60 GB aan schijfruimte beschikbaar op vier afzonderlijke, identieke logische vasteschijfstations verder naar de schijf met besturingssysteem hebben.
  - [.NET framework 4.6 (of een nieuwere versie)](https://dotnet.microsoft.com/download/dotnet-framework-runtime/net46) moet worden geïnstalleerd.

2. [Ga naar de pagina aan de slag](https://azure.microsoft.com/overview/azure-stack/try/?v=try) waar u kunt downloaden van de Azure Stack Development Kit, vindt u de informatie en klik vervolgens op **indienen**.
3. Downloaden en uitvoeren van de [Implementatiechecker voor Azure Stack Development Kit](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) vereistencontrole script. Met dit script zelfstandige verloopt via de vereiste controles uitgevoerd door het installatieprogramma voor Azure Stack Development Kit. Het biedt een om te bevestigen dat u voldoet aan de hardware- en softwarevereisten voor het downloaden van het pakket met grotere voor Azure Stack Development Kit.
4. Onder **downloaden van de software**, klikt u op **Azure Stack Development Kit**.

  > [!NOTE]
  > Het downloaden van het ASDK (AzureStackDevelopmentKit.exe) is ongeveer 10GB.

## <a name="extract-the-asdk"></a>De ASDK extraheren
1. Nadat het downloaden is voltooid, klikt u op **uitvoeren** om te starten van de ASDK zelf extractor (AzureStackDevelopmentKit.exe).
2. Lees en accepteer de gebruiksrechtovereenkomst voor weergegeven van de **License Agreement** pagina van de Wizard zelf Extractor en klik vervolgens op **volgende**.
3. Lees de privacy statement-informatie weergegeven op de **belangrijke kennisgeving** pagina van de Wizard zelf Extractor en klik vervolgens op **volgende**.
4. Selecteer de locatie voor Azure Stack-setup-bestanden moeten worden uitgepakt in op aan de **doellocatie selecteren** pagina van de Wizard zelf Extractor en klik vervolgens op **volgende**. De standaardlocatie is *huidige map*\Azure Stack Development Kit. 
5. Controleer de doellocatie samenvatting op de **gereed voor uitpakken** pagina van de Wizard zelf Extractor, en klik vervolgens op **extraheren** om op te halen van de CloudBuilder.vhdx (ongeveer 28 GB) en ThirdPartyLicenses.rtf bestanden. Dit proces duurt enige tijd om te voltooien.
6. Kopieer of het bestand CloudBuilder.vhdx verplaatsen naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx) op de hostcomputer ASDK.

> [!NOTE]
> Nadat u de bestanden hebt uitgepakt, kunt u de. EXE en. BIN-bestanden te herstellen van de ruimte op harde schijf. Of u kunt back-up van deze bestanden zodat u niet hoeft te downloaden de bestanden opnieuw als u nodig hebt om de ASDK opnieuw te implementeren.


## <a name="next-steps"></a>Volgende stappen
[De hostcomputer ASDK voorbereiden](asdk-prepare-host.md)