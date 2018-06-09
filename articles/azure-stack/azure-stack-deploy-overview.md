---
title: Evalueren van de Azure-Stack Development Kit | Microsoft Docs
description: Informatie over het implementeren van de Azure-Stack Development Kit voor evaluatiedoeleinden.
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
ms.date: 06/04/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: a0e742ab3ac43cc7977761dd94c9689e3a7c2e0b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235182"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Snelstartgids: de Azure-Stack Development Kit evalueren

De [Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) is een test- en -omgeving die u implementeren kunt om te evalueren en de Azure-Stack-functies en services demonstreren. Om te beginnen met de ASDK, moet u de host voorbereiden computerhardware en voer vervolgens een aantal scripts (installatie duurt enkele uren). Daarna kunt kunt u aanmelden bij de portals beheerder of gebruiker aan de slag met Azure-Stack.

## <a name="prerequisites"></a>Vereisten

### <a name="asdk-host-computer-requirements"></a>ASDK host computervereisten

Voordat u de ASDK installeert, moet u de computer die als host voor de development kit fungeert voorbereiden. De hostcomputer development kit moet voldoen aan de hardware, software en netwerkvereisten beschreven in  **[bekijken van de implementatie van de ASDK planningsoverwegingen](.\asdk\asdk-deploy-considerations.md)**.

> [!TIP]
> U kunt de [Azure Stack implementatievereisten controleren hulpprogramma](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) na de installatie van het besturingssysteem op de hostcomputer van development kit om te bevestigen dat de hardware voldoet aan alle vereisten.

### <a name="account-requirements"></a>Vereisten voor account

Ook moet u kiezen tussen het gebruik van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) als oplossing voor de identiteit voor uw implementatie. Overzicht van de accountvereisten in  **[implementatie planningsoverwegingen](.\asdk\asdk-deploy-considerations.md#account-requirements)**

## <a name="download-and-extract-the-deployment-package"></a>Downloaden en uitpakken van het implementatiepakket

Na het voorbereiden van de hostcomputer van development kit downloaden en uitpakken van het implementatiepakket ASDK. Het implementatiepakket bevat de Cloudbuilder.vhdx-bestand, dat een virtuele harde schijf (VHD) met een besturingssysteem, en de installatiebestanden van de Azure-Stack.

U kunt het implementatiepakket downloaden naar de development kit host of een andere computer. De implementatie van de uitgepakte bestanden duren 60 GB aan vrije schijfruimte, zodat een andere computer kan helpen verminderen de opslagvereisten op de host van development kit.

**[Downloaden en uitpakken van de Azure Stack Development Kit (ASDK)](.\asdk\asdk-download.md)**

## <a name="prepare-the-host-computer"></a>Voorbereiden van de hostcomputer

Voordat u de ASDK installeren kunt, de hostomgeving moet worden voorbereid en het systeem geconfigureerd om op te starten vanuit de development kit VHD. Wanneer u de host opnieuw start, wordt opgestart CloudBuilder.vhdx en start u de ASDK implementeren.

**[De computer van de host ASDK voorbereiden](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Installeer de ASDK op de hostcomputer

Nadat de computer vanaf de VHD opgestart, kunt u de development kit aan de virtuele omgeving Cloudbuilder implementeren. U kunt de ASDK met behulp van de grafische gebruikersinterface (GUI) dat is opgegeven door het uitvoeren van het PowerShell-script asdk installer.ps1 of vanuit implementeren [de PowerShell-opdrachtregel](.\asdk\asdk-deploy-powershell.md)

> [!NOTE]
> Nadat de host opnieuw wordt opgestart vanaf de installatiekopie Cloudbuilder.vhdx, hebt u de optie van de configuratie van [telemetrie-instellingen van Azure-Stack](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *voordat* de ASDK installeren.

**[Installeer de Azure-Stack Development Kit (ASDK)](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Configuraties na de implementatie uitvoeren

Na de installatie van de ASDK, zijn er enkele aanbevolen na de installatie controleert en configuratiewijzigingen die moeten worden aangebracht.

**Hulpprogramma's**

Installeren van hulpprogramma's voor Azure Stack PowerShell en GitHub, en het succes van uw installatie met de cmdlet test-AzureStack controleren.

**Identiteitsoplossing**

Voor een implementatie die gebruikmaakt van Azure AD, moet u zowel de Azure-Stack-beheerder- en tenantverkeer de portals activeren. Deze activering instemt geven de Stack van Azure portal en Azure Resource Manager de juiste machtigingen (die wordt weergegeven op de pagina toestemming) voor alle gebruikers van de map.

**Verlopen van wachtwoorden**

U moet het vervalbeleid voor wachtwoorden om ervoor te zorgen dat het wachtwoord voor de development kit host niet voordat uw evaluatieversie is afgelopen periode verloopt opnieuw instellen.

> [!NOTE]
> U hebt ook de mogelijkheid van de configuratie van [telemetrie-instellingen van Azure-Stack](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *nadat* de ASDK installeren.

**[Post-ASDK implementatietaken](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registreren bij Azure

U moet Azure Stack registreren met Azure zodat u kunt [Azure marketplace-items downloaden](.\asdk\asdk-marketplace-item.md) naar Azure-Stack.

**[Azure Stack registreren bij Azure](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd! Via de stappen in deze snelstartgids hebt u een omgeving ASDK met een [beheerder](https://adminportal.local.azurestack.external) portal en een [gebruiker](https://portal.local.azurestack.external) portal.
