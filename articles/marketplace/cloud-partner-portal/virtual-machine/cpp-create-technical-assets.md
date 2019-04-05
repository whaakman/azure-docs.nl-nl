---
title: Technische assets voor een virtuele machine-aanbieding maken voor de Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u de technische assets voor een virtuele machine-aanbieding maken in Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/20/2018
ms.author: pbutlerm
ms.openlocfilehash: 6f1a93c3d3059e612d8c309b263e263dbb84c67f
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2019
ms.locfileid: "59050098"
---
# <a name="create-technical-assets-for-a-virtual-machine-offer"></a>Technische assets voor een virtuele machine-aanbieding maken

In deze sectie helpt u bij het maken en configureren van de technische assets voor een virtuele machine (VM)-aanbieding voor Azure Marketplace.  Een virtuele machine bevat twee onderdelen: de oplossing voor virtuele harde schijf (VHD) en de optionele gekoppelde gegevensschijven.  

- *Virtuele harde schijven (VHD's)*, met het besturingssysteem en uw oplossing, die u in uw Azure Marketplace-aanbieding implementeren wilt. Het proces van het voorbereiden van de VHD is afhankelijk van of het is een op Linux gebaseerde, op basis van Windows of een VM op basis van een aangepast.
- *Gegevensschijven* vertegenwoordigen exclusieve, permanente opslag voor een virtuele machine. Voer *niet* gebruikt u de VHD-oplossing (bijvoorbeeld de `C:` station) permanente gegevens op te slaan.

Een VM-installatiekopie bevat een besturingssysteemschijf en nul of meer gegevensschijven. Één VHD is per schijf nodig. Even zelfs lege gegevensschijven vereisen dat u een VHD moet worden gemaakt.
U moet het besturingssysteem van de virtuele machine, de VM-grootte, de poorten te openen, configureren en tot maximaal 15 gekoppelde gegevensschijven.

> [!TIP] 
> Ongeacht het te gebruiken besturingssysteem voegt u alleen het minimum aantal gegevensschijven toe dat voor de SKU is vereist. Klanten schijven die deel van een installatiekopie op het moment van implementatie uitmaken niet verwijderen, maar ze kunnen altijd schijven toevoegen tijdens of na de implementatie. 

> [!IMPORTANT]
> *Het aantal schijven in een nieuwe Afbeeldingversie niet wijzigen.* Als u moet de configuratie van gegevensschijven in de afbeelding, definieert u een nieuwe SKU. Publiceren van een nieuwe installatiekopieversie met een andere schijf telt, heeft het potentieel van belangrijke nieuwe implementatie op basis van de versie van de nieuwe installatiekopie in geval van automatisch schalen, automatische implementaties van oplossingen via Azure Resource Manager-sjablonen en andere scenario's.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="fundamental-technical-knowledge"></a>Fundamentele technische kennis

Het ontwerpen, bouwen en testen van deze elementen tijd in beslag nemen en technische kennis van het Azure-platform en de technologieën die wordt gebruikt voor het bouwen van de aanbieding is vereist. Naast uw domein oplossing voor uw IT-team moet beschikken over kennis van de volgende Microsoft-technologieën: 
-   Basiskennis van [Azure Services](https://azure.microsoft.com/services/) 
-   Hoe u [ontwerpen en bouwen van Azure-toepassingen](https://azure.microsoft.com/solutions/architecture/)
-   Praktische kennis van [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) en [Azure Networking](https://azure.microsoft.com/services/?filter=networking)
-   Praktische kennis van [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
-   Praktische kennis van [JSON](https://www.json.org/)


## <a name="suggested-tools"></a>Voorgesteld hulpprogramma 's 

Kies een of beide van de volgende scripts omgevingen voor het beheer van VHD's en virtuele machines:
-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [Azure-CLI](https://docs.microsoft.com/cli/azure)

Bovendien is het raadzaam de volgende hulpprogramma's toe te voegen aan uw ontwikkelomgeving: 

-   [Azure Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   Toestel: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *   Toestel: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *   Toestel: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

We raden ook controleren van de beschikbare hulpprogramma's in de [Azure-ontwikkelhulpprogramma's](https://azure.microsoft.com/tools/) pagina en, als u Visual Studio, de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).


## <a name="next-steps"></a>Volgende stappen

De volgende artikelen in deze sectie helpt u bij de stappen voor het maken en registreren van de activa van deze virtuele machine:

1. [Maak een Azure-compatibele virtuele harde schijf](./cpp-create-vhd.md) wordt beschreven hoe u een van beide een Linux - of Windows-gebaseerde VHD die compatibel is met Azure.  Het bevat aanbevolen procedures, zoals de grootte, patchen en voorbereiden van de virtuele machine voor het uploaden.

2. [Verbinding maken met de virtuele machine](./cpp-connect-vm.md) wordt uitgelegd hoe u extern verbinding maken met uw nieuwe virtuele machine en meld u aan deze.  In dit artikel wordt ook uitgelegd hoe u de virtuele machine op te slaan op de kosten voor gebruik stoppen.

3. [Configureer de virtuele machine](./cpp-configure-vm.md) wordt uitgelegd hoe u de juiste VHD-grootte kiezen, generaliseer de installatiekopie, recente updates (patches) toe te passen en aangepaste configuraties plannen.

4. [Een virtuele machine implementeren vanuit een virtuele harde schijf](./cpp-deploy-vm-vhd.md) wordt uitgelegd hoe u voor het registreren van een virtuele machine van een VHD voor Azure zijn geïmplementeerd.  Een lijst met de hulpprogramma's die nodig zijn en hoe u kunt gebruiken voor het maken van een gebruiker VM-installatiekopie, en vervolgens implementeert naar Azure met behulp van de [Microsoft Azure portal](https://ms.portal.azure.com/) of PowerShell-scripts. 

5. [De installatiekopie van een virtuele machine certificeren](./cpp-certify-vm.md) wordt uitgelegd hoe u om te testen en verzenden van een VM-installatiekopie voor certificering voor Azure Marketplace. Hierin wordt uitgelegd waar u de *Test Certificeringshulpprogramma voor Azure Certified* hulpprogramma en hoe u dit hulpprogramma gebruiken om te certificeren van uw VM-installatiekopie. 

6. [SAS-URI ophalen](./cpp-get-sas-uri.md) wordt uitgelegd hoe u de shared access signature (SAS)-URI voor uw VM-installatiekopieën.
 
Als een ondersteunende artikel [gemeenschappelijke shared access signature-URL problemen](./cpp-common-sas-url-issues.md) geeft een lijst van enkele veelvoorkomende problemen die kunnen optreden met behulp van SAS URI's en de bijbehorende oplossingen.

Nadat u alle stappen hebt voltooid, kunt u zich kunt [uw VM-aanbieding publiceren](./cpp-publish-offer.md) in de Azure Marketplace.
