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
ms.date: 03/22/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: 1deabcf64b3fbf3cbc89232c340a8882cd2591e8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Snelstartgids: De Azure-Stack Development Kit evalueren
De [Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) is een test- en -omgeving die u implementeren kunt om te evalueren en de Azure-Stack-functies en services demonstreren. Om te beginnen met de ASDK, moet u de host voorbereiden computerhardware en voer vervolgens een aantal scripts (installatie duurt enkele uren). Daarna kunt kunt u aanmelden bij de portals beheerder en gebruiker aan de slag met Azure-Stack.

## <a name="prerequisites"></a>Vereisten 
Voordat u de ASDK installeert, moet u de computer die als host voor de development kit (de host development kit fungeert) voorbereiden. De hostcomputer development kit moet voldoen aan de minimale eisen aan hardware, software en netwerk. 

Ook moet u kiezen tussen het gebruik van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) als oplossing voor de identiteit voor uw implementatie. 

Zorg ervoor dat de development kit host voldoet aan de minimale hardwarevereisten en dat u hebt aangebracht aan uw identiteit oplossing beslissing gemaakt voordat u de implementatie start, zodat het installatieproces soepel wordt uitgevoerd. 

**[Bekijk de planningsoverwegingen ASDK-implementatie](.\asdk\asdk-deploy-considerations.md)**

> [!TIP]
> U kunt de [Azure Stack implementatievereisten controleren hulpprogramma](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) na de installatie van het besturingssysteem op de hostcomputer van development kit om te bevestigen dat de hardware voldoet aan alle vereisten.

## <a name="download-and-extract-the-deployment-package"></a>Downloaden en uitpakken van het implementatiepakket
Nadat u hebt gecontroleerd of uw development kit host-computer voldoet aan de basisvereisten voor het installeren van de ASDK, worden de volgende stap is het downloaden en uitpakken van het implementatiepakket ASDK. Het implementatiepakket bevat de Cloudbuilder.vhdx-bestand, dat een virtuele harde schijf met een besturingssysteem en de installatiebestanden van de Azure-Stack.

U kunt het implementatiepakket downloaden naar de development kit host of een andere computer. De implementatie van de uitgepakte bestanden duren 60 GB aan vrije schijfruimte, zodat een andere computer kan helpen verminderen de hardwarevereisten voor de host van development kit.

**[Downloaden en uitpakken van de Azure Stack Development Kit (ASDK)](.\asdk\asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Voorbereiden van de hostcomputer development kit
Voordat u de ASDK op de hostcomputer installeren kunt, de omgeving moet worden voorbereid en het systeem geconfigureerd om op te starten vanaf VHD. Nadat de computer development kit host is voorbereid, deze wordt opgestart vanaf de vaste schijf van CloudBuilder.vhdx virtuele machine zodat u met ASDK-implementatie beginnen kunt.

**[De computer van de host ASDK voorbereiden](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Installeer de ASDK op de hostcomputer
Na het voorbereiden van de hostcomputer development kit, kan de ASDK worden geïmplementeerd in de afbeelding CloudBuilder.vhdx. De ASDK kan worden geïmplementeerd met een grafische gebruikersinterface (GUI) dat is opgegeven door te downloaden en uitvoeren van het PowerShell-script asdk installer.ps1 of volledig van [de opdrachtregel](.\asdk\asdk-deploy-powershell.md). 

> [!NOTE]
> Eventueel nadat de computer is opgestart de CloudBuilder.vhdx, kunt u configureren [telemetrie-instellingen van Azure-Stack](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *voordat* de ASDK installeren.


**[Installeer de Azure-Stack Development Kit (ASDK)](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Configuraties na de implementatie uitvoeren
Na de installatie van de ASDK, zijn er enkele aanbevolen na de installatie controleert en configuratiewijzigingen die moeten worden aangebracht. U kunt controleren of de installatie is geïnstalleerd met behulp van de cmdlet test-AzureStack en hulpprogramma's Azure Stack PowerShell en GitHub installeren. 

Na de implementaties die gebruikmaken van Azure AD, moet u zowel de Azure-Stack-beheerder- en tenantverkeer de portals activeren. Deze activering instemt geven de Stack van Azure portal en Azure Resource Manager de juiste machtigingen (die wordt weergegeven op de pagina toestemming) voor alle gebruikers van de map.

U moet ook opnieuw instellen voor het vervalbeleid voor wachtwoorden om ervoor te zorgen dat het wachtwoord voor de development kit host niet voordat uw evaluatieversie is afgelopen periode verloopt.

> [!NOTE]
> Desgewenst kunt u ook configureren [telemetrie-instellingen van Azure-Stack](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *nadat* de ASDK installeren.

**[Post-ASDK implementatietaken](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registreren bij Azure
U moet Azure Stack registreren met Azure zodat u kunt [Azure marketplace-items downloaden](.\asdk\asdk-marketplace-item.md) naar Azure-Stack.

**[Azure Stack registreren bij Azure](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd! Nadat u deze stappen uitvoert, hebt u een development kit omgeving met zowel [beheerder](https://adminportal.local.azurestack.external) en [gebruiker](https://portal.local.azurestack.external) portals. 
