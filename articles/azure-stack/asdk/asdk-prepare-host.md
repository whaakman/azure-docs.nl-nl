---
title: Voorbereiden van de hostcomputer Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Beschrijft hoe de hostcomputer Azure Stack Development Kit (ASDK) voorbereidt voor ASDK-installatie.
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
ms.openlocfilehash: 8b1a6298ab32dc364aa1543e4a8d5db47b02a098
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="prepare-the-asdk-host-computer"></a>De computer van de host ASDK voorbereiden
Voordat u de ASDK op de hostcomputer installeren kunt, moet u de omgeving ASDK voorbereid voor de installatie. Wanneer de computer development kit host is voorbereid, blijft deze opstarten van de virtuele machine CloudBuilder.vhdx vaste schijf om te beginnen met ASDK-implementatie.

## <a name="prepare-the-development-kit-host-computer"></a>Voorbereiden van de hostcomputer development kit
Voordat u de ASDK op de hostcomputer installeren kunt, moet de computer van ASDK hostomgeving worden voorbereid.
1. Meld u aan als lokale beheerder op uw computer development kit host.
2. Zorg ervoor dat het bestand CloudBuilder.vhdx is verplaatst naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx).
3. Voer het volgende script voor het downloaden van het installatiebestand van development kit (asdk installer.ps1) van de [Azure Stack GitHub-opslagplaats voor extra](https://github.com/Azure/AzureStack-Tools) naar de **C:\AzureStack_Installer** map op uw Development kit hostcomputer:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Op een PowerShell-console met verhoogde bevoegdheid, start de **C:\AzureStack_Installer\asdk-installer.ps1** script en klik vervolgens op **omgeving voorbereiden**.

    ![](media/asdk-prepare-host/1.PNG) 

5. Op de **Selecteer Cloudbuilder vhdx** pagina van het installatieprogramma, blader naar en selecteer de **cloudbuilder.vhdx** -bestand dat u hebt gedownload en uitgepakt [de vorige stappen](asdk-download.md). Op deze pagina kunt u ook, eventueel inschakelen de **toevoegen van stuurprogramma's** selectievakje in als u moet extra stuurprogramma's toevoegen aan de hostcomputer development kit. Klik op **Volgende**.  

    ![](media/asdk-prepare-host/2.PNG)

6. Op de **optionele instellingen** pagina, geeft u de lokale beheerder accountgegevens voor de development kit-hostcomputer en klik vervolgens op **volgende**. U kunt ook waarden opgeven voor de volgende optionele instellingen:
  - **Computernaam**: deze optie stelt u de naam van de host van development kit. De naam moet voldoen aan de vereisten van de FQDN-naam en moet zijn dan 15 tekens of minder. De standaardwaarde is de naam van een willekeurige computer door Windows wordt gegenereerd.
  - **Tijdzone**: Hiermee stelt u de tijdzone voor de host van development kit. De standaardwaarde is (UTC-8:00) Pacific Time (VS en Canada).
  - **Statische IP-configuratie**: Hiermee stelt u uw implementatie een statische IP-adres gebruiken. Wanneer het installatieprogramma opnieuw wordt opgestart in de cloudbuilder.vhx, worden de netwerkinterfaces anders geconfigureerd met DHCP.

    ![](media/asdk-prepare-host/3.PNG)

  > [!IMPORTANT]
  > Als u niet de lokale administrator-referenties in deze stap opgeeft, moet u direct of KVM-toegang tot de host nadat de computer opnieuw als onderdeel van het instellen van de development kit opgestart.

7. Als u een statisch IP-configuratie in de vorige stap hebt gekozen, moet u nu het volgende doen:
    - Selecteer een netwerkadapter. Zorg ervoor dat u kunt verbinding maken met de netwerkadapter voordat u op **volgende**.
    - Zorg ervoor dat de **IP-adres**, **Gateway**, en **DNS** waarden juist zijn en klik vervolgens op **volgende**.
13. Klik op **volgende** starten van het voorbereidingsproces.
14. Wanneer de voorbereiding blijkt **voltooid**, klikt u op **volgende**.
15. Klik op **nu opnieuw opstarten** development kit hostcomputer de cloudbuilder.vhdx opstarten en [gaan met het implementatieproces](asdk-install.md).

    ![](media/asdk-prepare-host/4.PNG)


## <a name="next-steps"></a>Volgende stappen
[De ASDK installeren](asdk-install.md)