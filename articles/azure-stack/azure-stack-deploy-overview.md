---
title: Evalueren van de Azure Stack Development Kit | Microsoft Docs
description: Informatie over het implementeren van de Azure Stack Development Kit voor evaluatiedoeleinden.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/05/2018
ms.author: jeffgilb
ms.custom: mvc
ms.reviewer: misainat
ms.openlocfilehash: 44fed3311234e1a64cb46c3403f39a9e269d189b
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956922"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Snelstartgids: Azure Stack Development Kit evalueren

De [Azure Stack Development Kit (ASDK)](./asdk/asdk-what-is.md) is een omgeving voor testen en ontwikkeling die u implementeren kunt om te evalueren en Azure Stack-functies en -services demonstreren. Als u wilt beginnen met de ASDK, moet u voor het voorbereiden van de host computerhardware en voert u enkele scripts (installatie duurt enkele uren). Hierna kunt u zich de portals beheerder of gebruiker aan de slag met Azure Stack.

## <a name="prerequisites"></a>Vereisten

### <a name="asdk-host-computer-requirements"></a>ASDK host computervereisten

Voordat u de ASDK installeert, moet u de computer die als voor de development kit host voorbereiden. De hostcomputer development kit moet voldoen aan de hardware, software en netwerkvereisten beschreven in  **[bekijken van de implementatie van de ASDK planningsoverwegingen](./asdk/asdk-deploy-considerations.md)**.

> [!TIP]
> U kunt de [Azure Stack-implementatievereisten controleren hulpprogramma](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) na de installatie van het besturingssysteem op de hostcomputer van development kit om te bevestigen dat de hardware voldoet aan alle vereisten.

### <a name="account-requirements"></a>Accountvereisten

Ook moet u kiezen tussen het gebruik van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) als de identiteitsoplossing voor uw implementatie. Bekijk de accountvereisten voor het in  **[implementatie planningsoverwegingen](./asdk/asdk-deploy-considerations.md#account-requirements)**

## <a name="download-and-extract-the-deployment-package"></a>Downloaden en uitpakken van het implementatiepakket

Na het voorbereiden van de hostcomputer van uw development kit downloaden en uitpakken van het implementatiepakket ASDK. Het implementatiepakket bevat het bestand Cloudbuilder.vhdx, dit is een virtuele harde schijf (VHD) met een opstartbare besturingssysteem, en de installatiebestanden van de Azure Stack.

U kunt het implementatiepakket downloaden naar de development kit-host of een andere computer. De van de uitgepakte implementatiebestanden duren 60 GB aan vrije schijfruimte beschikbaar, zodat met behulp van een andere computer kan helpen verminderen de opslagvereisten op de host van development kit.

**[Downloaden en uitpakken van de Azure Stack Development Kit (ASDK)](./asdk/asdk-download.md)**

## <a name="prepare-the-host-computer"></a>De computer voorbereiden

Voordat u de ASDK installeren kunt, de hostomgeving moet worden voorbereid en het systeem geconfigureerd om op te starten vanuit de development kit VHD. Wanneer u de host opnieuw start, deze wordt opgestart vanaf CloudBuilder.vhdx en start u de ASDK implementeren.

**[De hostcomputer ASDK voorbereiden](./asdk/asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>De ASDK installeren op de hostcomputer

Nadat de computer wordt opgestart vanaf de VHD, kunt u de development kit implementeren aan de virtuele omgeving Cloudbuilder. U kunt de ASDK met behulp van de grafische gebruikersinterface (GUI), dat is verstrekt door het asdk installer.ps1 PowerShell-script is uitgevoerd, of van implementeren [de PowerShell-opdrachtregel](./asdk/asdk-deploy-powershell.md)

> [!NOTE]
> Als de host wordt opgestart in de afbeelding Cloudbuilder.vhdx, u hebt de mogelijkheid van het configureren van [telemetrie-instellingen voor Azure Stack](./asdk/asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *voordat* de ASDK installeren.

**[De Azure Stack Development Kit (ASDK) installeren](./asdk/asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Uitvoeren na de implementatie-configuraties

Na de installatie van de ASDK, zijn er enkele aanbevolen controles voor na de installatie en configuratie wijzigingen beschreven die moeten worden aangebracht.

**Hulpprogramma's**

Hulpprogramma's voor PowerShell voor Azure Stack- en GitHub installeren en controleren van het succes van uw installatie met behulp van de cmdlet test-AzureStack.

**Oplossing voor identiteit**

Voor een implementatie die gebruikmaakt van Azure AD, moet u zowel de Azure Stack-beheerder en tenant de portals activeren. Deze activering toestemming heeft om op te geven van de Azure Stack-portal en Azure Resource Manager de juiste machtigingen (die worden vermeld op de pagina toestemming) voor alle gebruikers van de map.

**Wachtwoord verloopt**

U moet het vervalbeleid voor wachtwoorden om ervoor te zorgen dat het wachtwoord voor de host van development kit niet voordat u een einde van de evaluatie verloopt opnieuw instellen.

> [!NOTE]
> U hebt ook de mogelijkheid van het configureren van [telemetrie-instellingen voor Azure Stack](./asdk/asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *nadat* de ASDK installeren.

**[Post-ASDK implementatietaken](./asdk/asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registreren bij Azure

U moet Azure Stack registreren met Azure zodat u kunt [Azure marketplace-items downloaden](./asdk/asdk-marketplace-item.md) naar Azure Stack.

**[Azure Stack registreren bij Azure](./asdk/asdk-register.md)**

## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd! Door de stappen in deze Quick Start hebt u een omgeving ASDK met een [beheerder](https://adminportal.local.azurestack.external) portal en een [gebruiker](https://portal.local.azurestack.external) portal.
