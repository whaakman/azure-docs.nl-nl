---
title: Endpoint protection oplossingen detectie en status evaluatie in Azure Security Center | Microsoft Docs
description: Hoe worden de oplossingen voor eindpuntbeveiliging gedetecteerd en geïdentificeerd als in orde.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2019
ms.author: v-mohabe
ms.openlocfilehash: b17e5f16b988bfa562b00bc6f5b9dfd34be4ca43
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247962"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Endpoint protection evaluatie en aanbevelingen in Azure Security Center

Endpoint protection evaluatie en aanbevelingen in Azure Security Center detecteert en beoordeling van de replicatiestatus van [ondersteund](https://docs.microsoft.com/azure/security-center/security-center-os-coverage#supported-platforms-for-windows-computers-and-vms) versies van oplossingen voor Eindpuntbeveiliging. Dit onderwerp worden de scenario's voor het genereren van de volgende twee aanbevelingen voor oplossingen voor Eindpuntbeveiliging door Azure Security Center.

* **Oplossingen voor eindpuntbeveiliging installeren op uw virtuele machine**
* **Oplossen van problemen met de status van endpoint protection op uw virtuele machines**

## <a name="windows-defender"></a>Windows Defender

* De **"Oplossingen voor eindpuntbeveiliging installeren op de virtuele machine"** aanbeveling wordt gegenereerd wanneer [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) wordt uitgevoerd en het resultaat is **AMServiceEnabled: False**

* De **'Oplossen van problemen met de status van endpoint protection op uw virtuele machines'** aanbeveling wordt gegenereerd wanneer [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) wordt uitgevoerd en een of beide van de volgende optreedt:

  * Ten minste één van de volgende eigenschappen is ingesteld op false:

     **AMServiceEnabled**

     **AntispywareEnabled**

     **RealTimeProtectionEnabled**

     **BehaviorMonitorEnabled**

     **IoavProtectionEnabled**

     **OnAccessProtectionEnabled**

  * Als een of beide van de volgende eigenschappen zijn groter of gelijk aan 7.

     **AntispywareSignatureAge**

     **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Microsoft System Center endpoint protection

* De **"Oplossingen voor eindpuntbeveiliging installeren op de virtuele machine"** aanbeveling wordt gegenereerd bij het importeren van **SCEPMpModule ("$env: Program Security Client\MpProvider\MpProvider.psd1")** en uitvoeren van **Get-MProtComputerStatus** resultaten met **AMServiceEnabled = false**

* De **'Oplossen van problemen met de status van endpoint protection op uw virtuele machines'** aanbeveling wordt gegenereerd wanneer **Get-MprotComputerStatus** wordt uitgevoerd en een of beide van de volgende optreedt:

    * Ten minste één van de volgende eigenschappen is ingesteld op false:

       **AMServiceEnabled**
    
       **AntispywareEnabled**
    
       **RealTimeProtectionEnabled**
    
       **BehaviorMonitorEnabled**
    
       **IoavProtectionEnabled**
    
       **OnAccessProtectionEnabled**
          
    * Als een of beide van de volgende Updates van de handtekening is groter of gelijk aan 7. 

       **AntispywareSignatureAge**
    
       **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* De **"Oplossingen voor eindpuntbeveiliging installeren op de virtuele machine"** aanbeveling wordt gegenereerd als er een of meer van de volgende controles worden niet voldaan:
    * **De beveiligingsagent HKLM:\SOFTWARE\TrendMicro\Deep** bestaat
    * **HKLM:\SOFTWARE\TrendMicro\Deep Security Agent\InstallationFolder** bestaat
    * De **dsq_query.cmd** bestand is gevonden in de map voor installatie
    * Met **dsa_query.cmd** resultaten met **Component.AM.mode: op - Trend Micro Deep Security Agent gedetecteerd**

## <a name="symantec-endpoint-protection"></a>Symantec endpoint protection
De **"Oplossingen voor eindpuntbeveiliging installeren op de virtuele machine"** aanbeveling wordt gegenereerd als een van de volgende controles worden niet voldaan:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

of

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

De **'Oplossen van problemen met de status van endpoint protection op uw virtuele machines'** aanbeveling wordt gegenereerd als een van de volgende controles worden niet voldaan:  

* Controleer de versie van de Symantec > = 12:  Registerlocatie: **HKLM:\Software\Symantec\Symantec eindpunt Protection\CurrentVersion'-waarde "PRODUCTVERSIE"**

* Controleer de status van realtime-beveiliging: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* Status van handtekening controleren: **HKLM\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LatestVirusDefsDate <= 7 days**

* Controleer de status van volledige Scan: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTime <= 7 days**

* Zoek handtekening versienummer dat pad is naar handtekeningversie voor Symantec 12: **Register paden + 'CurrentVersion\SharedDefs'-waarde "SRTSP"** 

* Pad naar de versie van de handtekening voor Symantec 14: **Registry Paths+ "CurrentVersion\SharedDefs\SDSDefs" -Value "SRTSP"**

Registerpaden:

**"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path;** 
 **"HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>McAfee endpoint protection voor Windows

De **"Oplossingen voor eindpuntbeveiliging installeren op de virtuele machine"** aanbeveling wordt gegenereerd als de volgende controles worden niet voldaan:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** bestaat

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

De **'Oplossen van problemen met de status van endpoint protection op uw virtuele machines'** aanbeveling wordt gegenereerd als de volgende controles worden niet voldaan:

* McAfee versie: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* Handtekeningversie vinden: **HKLM:\Software\McAfee\AVSolution\DS\DS-waarde "dwContentMajorVersion"**

* Handtekening datum zoeken: **HKLM:\Software\McAfee\AVSolution\DS\DS-waarde "szContentCreationDate" > = 7 dagen**

* Scandatum zoeken: **HKLM:\Software\McAfee\Endpoint\AV\ODS-waarde "LastFullScanOdsRunTime" > = 7 dagen**

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Microsoft Antimalware-uitbreiding logboeken zijn beschikbaar op:  
**%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware(Or PaaSAntimalware)\1.5.5.x(version#)\CommandExecution.log**

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). Of u kunt een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
