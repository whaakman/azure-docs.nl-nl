---
title: Voorbereiden van de hostcomputer Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Beschrijft hoe u de Azure Stack Development Kit (ASDK)-hostcomputer voorbereiden voor ASDK-installatie.
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
ms.date: 10/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: ec7b56a7324f3c8c3e3459639e4fd00e92d93e8f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249745"
---
# <a name="prepare-the-asdk-host-computer"></a>De hostcomputer ASDK voorbereiden
Voordat u de ASDK op de computer installeren kunt, moet de omgeving ASDK worden voorbereid voor de installatie. Wanneer de computer development kit is voorbereid, zal het opstarten van de virtuele machine CloudBuilder.vhdx vaste schijf om te beginnen met ASDK implementatie.

## <a name="prepare-the-development-kit-host-computer"></a>Voorbereiden van de hostcomputer development kit
Voordat u de ASDK op de host installeren kunt, kan de omgeving ASDK host computer moet worden voorbereid.
1. Meld u aan als lokale beheerder is op uw computer kit host.
2. Zorg ervoor dat het bestand CloudBuilder.vhdx is verplaatst naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx).
3. Voer het volgende script om te downloaden van het installatiebestand van development kit (asdk-installer.ps1) van de [Azure Stack-GitHub-opslagplaats voor extra](https://github.com/Azure/AzureStack-Tools) naar de **C:\AzureStack_Installer** map op uw Development kit-hostcomputer:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Enforce usage of TLSv1.2 to download from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Op een PowerShell-console met verhoogde bevoegdheid, start de **C:\AzureStack_Installer\asdk-installer.ps1** script en klik vervolgens op **omgeving voorbereiden**.

    ![](media/asdk-prepare-host/1.PNG) 

5. Op de **Selecteer Cloudbuilder vhdx** pagina van het installatieprogramma, blader naar en selecteer de **cloudbuilder.vhdx** -bestand dat u hebt gedownload en uitgepakt [de vorige stappen](asdk-download.md). Op deze pagina ook, indien gewenst, kunt u de **toevoegen van stuurprogramma's** selectievakje in als u wilt toevoegen van extra stuurprogramma's op de computer development kit. Klik op **volgende**.  

    ![](media/asdk-prepare-host/2.PNG)

6. Op de **optionele instellingen** pagina, geef de lokale beheerder accountgegevens voor de hostcomputer development kit en klik vervolgens op **volgende**. U kunt ook waarden opgeven voor de volgende optionele instellingen:
  - **ComputerName**: Deze optie stelt u de naam van de host van development kit. De naam moet voldoen aan de vereisten van de FQDN-naam en 15 tekens of minder moet zijn. De standaardwaarde is de naam van een willekeurige computer die door Windows gegenereerd.
  - **Statische IP-configuratie**: Hiermee stelt u uw implementatie een statisch IP-adres gebruiken. Wanneer het installatieprogramma opnieuw wordt opgestart in de cloudbuilder.vhdx, worden de netwerkinterfaces anders geconfigureerd met DHCP.

    ![](media/asdk-prepare-host/3.PNG)

  > [!IMPORTANT]
  > Als u het lokale administrator-referenties in deze stap niet opgeeft, moet u direct of KVM-toegang tot de host nadat de computer opnieuw wordt opgestart als onderdeel van het instellen van de development kit.

7. Als u een statisch IP-configuratie in de vorige stap hebt gekozen, moet u nu:
    - Selecteer een netwerkadapter. Zorg ervoor dat u kunt verbinding maken met de adapter voordat u klikt op **volgende**.
    - Zorg ervoor dat de **IP-adres**, **Gateway**, en **DNS** waarden correct zijn en klik vervolgens op **volgende**.
13. Klik op **volgende** starten van het voorbereidingsproces.
14. Wanneer de voorbereiding van het geeft **voltooid**, klikt u op **volgende**.
15. Klik op **nu opnieuw opstarten** opstarten van de hostcomputer development kit naar de cloudbuilder.vhdx en [doorgaan met het implementatieproces](asdk-install.md).

    ![](media/asdk-prepare-host/4.PNG)


## <a name="next-steps"></a>Volgende stappen
[De ASDK installeren](asdk-install.md)
