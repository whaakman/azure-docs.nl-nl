---
title: Downloaden en uitpakken van de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Beschrijft hoe downloaden en uitpakken van de Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0cf389c9443a9cff477b884c277d72de27769afc
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="download-and-extract-the-azure-stack-development-kit-asdk"></a>Downloaden en uitpakken van de Azure Stack Development Kit (ASDK)
Nadat u hebt gecontroleerd of uw development kit host-computer voldoet aan de basisvereisten voor het installeren van de ASDK, wordt de volgende stap is het downloaden en uitpakken van het implementatiepakket ASDK om op te halen van de Cloudbuilder.vhdx.

## <a name="download-the-asdk"></a>De ASDK downloaden
1. Voordat u het downloaden, zorg dat de computer voldoet aan de volgende vereisten:

  - De computer moet ten minste 60 GB aan schijfruimte beschikbaar op vier afzonderlijke, identieke logische vasteschijfstations verder naar de schijf van besturingssysteem hebben.
  - [.NET framework 4.6 (of een latere versie)](https://aka.ms/r6mkiy) moet worden geïnstalleerd.

2. [Ga naar de pagina aan de slag](https://azure.microsoft.com/overview/azure-stack/try/?v=try) waar u kunt download de Azure-Stack Development Kit, vindt u de informatie en klik vervolgens op **indienen**.
3. Downloaden en uitvoeren van de [implementatie Checker voor Azure Stack Development Kit](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) vereistencontrole script. Dit script zelfstandige gaat via de controles van vereisten uitgevoerd door de instellingen voor Azure Stack Development Kit. Het biedt een manier om te bevestigen dat u voldoet aan de hardware- en softwarevereisten voor het downloaden van het grotere pakket voor Azure Stack Development Kit.
4. Onder **Download de software**, klikt u op **Azure Stack Development Kit**.

  > [!NOTE]
  > Het downloaden van de ASDK (AzureStackDevelopmentKit.exe) is ongeveer 10GB.

## <a name="extract-the-asdk"></a>Pak de ASDK
1. Nadat het downloaden is voltooid, klikt u op **uitvoeren** starten van de ASDK zelfstandig uitpakken (AzureStackDevelopmentKit.exe).
2. Lees en accepteer de gebruiksrechtovereenkomst voor weergegeven van de **License Agreement** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **volgende**.
3. Lees de privacy-instructie informatie weergegeven op de **belangrijke mededeling** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **volgende**.
4. Selecteer de locatie voor de setup-bestanden worden uitgepakt naar op Azure-Stack de **doellocatie Selecteer** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **volgende**. De standaardlocatie is *huidige map*\Azure Stack Development Kit. 
5. Controleer de doellocatie samenvatting op de **gereed voor uitpakken** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **extraheren** uitpakken van de CloudBuilder.vhdx (ongeveer 25 GB) en ThirdPartyLicenses.rtf bestanden. Dit proces neemt enige tijd in beslag.
6. Kopieer of het bestand CloudBuilder.vhdx verplaatsen naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx) op de hostcomputer ASDK.

> [!NOTE]
> Nadat u de bestanden hebt uitgepakt, verwijdert u de. EXE en. Bestanden van de OPSLAGLOCATIE voor het herstellen van harde-schijfruimte. Of u kunt back-up van deze bestanden zodat u hoeft niet te downloaden de bestanden opnieuw als u wilt de ASDK implementeren.


## <a name="next-steps"></a>Volgende stappen
[De computer van de host ASDK voorbereiden](asdk-prepare-host.md)