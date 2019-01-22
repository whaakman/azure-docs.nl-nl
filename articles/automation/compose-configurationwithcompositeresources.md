---
title: Samenstellen van DSC-configuraties in Azure Automation State Configuration (DSC) hebt met behulp van samengestelde Resources
description: Meer informatie over het opstellen van configuraties met behulp van samengestelde resources in Azure Automation State Configuration (DSC)
keywords: PowerShell dsc, configuratie van gewenste status, azure powershell dsc, samengestelde resources
services: automation
ms.service: automation
ms.subservice: dsc
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/21/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b82457d8a7ce70ecfde32ba625ef610a3772213c
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54432188"
---
# <a name="composing-dsc-configurations-in-azure-automation-state-configuration-dsc-using-composite-resources"></a>Samenstellen van DSC-configuraties in Azure Automation State Configuration (DSC) hebt met behulp van samengestelde Resources

Wanneer een resource worden beheerd met meer dan een enkele gewenste status configuration (DSC)-configuratie moet, het beste pad is het gebruik van [samengestelde resources](/powershell/dsc/authoringresourcecomposite). Een samengestelde resource is een geneste en geparameteriseerde configuratie wordt gebruikt als een DSC-resource in een andere configuratie. Hiermee wordt het maken van complexe configuraties terwijl de onderliggende samengestelde resources (configuraties met parameters) afzonderlijk worden beheerd en die zijn gebouwd.

Azure Automation kan de [importeren en de samenstelling van samengestelde resources](automation-dsc-compile.md#composite-resources). Als samengestelde resources zijn geïmporteerd in uw Automation-account, bent u kunt gebruiken de **opstellen configuratie** -ervaring in de **State Configuration (DSC)** pagina.

## <a name="composing-a-configuration-from-composite-resources"></a>Samenstellen van een configuratie van samengestelde resources

Voordat u een configuratie gemaakt van samengestelde resources in Azure portal toewijzen kunt, moet u deze samenstelt. Dit kan worden gedaan met **opstellen configuratie** op de **State Configuration (DSC)** terwijl op een pagina de **configuraties** of **Compiled configuraties** tabbladen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Aan de linkerkant, klikt u op **alle resources** en vervolgens de naam van uw Automation-account.
1. Op de **Automation-account** weergeeft, schakelt **State configuration (DSC)** onder **Configuratiebeheer**.
1. Op de **State configuration (DSC)** pagina, klik op de **configuraties** of **gecompileerd configuraties** tabblad en klik vervolgens op **configuratie opstellen**  in het menu aan de bovenkant van de pagina.
1. Op de **basisbeginselen** stap, geeft u de nieuwe configuratienaam (vereist) en klikt u op een willekeurige plaats in de rij van elke samengestelde resource die u wilt opnemen in uw nieuwe configuratie en klik vervolgens op **volgende** of klik op de **Broncode** stap. Voor de volgende stappen die we geselecteerd **PSExecutionPolicy** en **RenameAndDomainJoin** samengestelde resources.
   ![Schermafbeelding van de stap van de basisprincipes van de pagina van de configuratie opstellen](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. De **broncode** stap laat zien hoe de samengesteld configuratie van de geselecteerde samengestelde resources eruitziet. U ziet het samenvoegen van alle parameters en hoe ze worden doorgegeven aan de samengestelde resource. Wanneer u klaar bent controleren van de nieuwe broncode, klikt u op **volgende** of klik op de **Parameters** stap.
   ![Schermafbeelding van de stap van de bron-code van de pagina van de configuratie opstellen](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. Op de **Parameters** stap, de parameter die elke samengestelde resource wordt weergegeven, zodat ze kunnen worden opgegeven. Als een parameter een beschrijving heeft, wordt dit weergegeven naast het parameterveld. Als een veld is een **PSCredential** typeparameter, de vervolgkeuzelijst configureren geeft een lijst van **referentie** objecten in het huidige Automation-account. Een **+ een referentie toevoegen** optie is ook beschikbaar zijn. Wanneer alle vereiste parameters zijn opgegeven, klikt u op **opslaan en compileren**.
   ![Schermafbeelding van de parameters-stap van de pagina van de configuratie opstellen](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

Wanneer de nieuwe configuratie is opgeslagen, wordt deze verzonden voor de compilatie. Status van de Compilatietaak kan worden weergegeven, zoals een geïmporteerde configuratie. Zie voor meer informatie, [weergeven van een Compilatietaak](automation-dsc-getting-started.md#viewing-a-compilation-job).

Als de compilatie is voltooid, de nieuwe configuratie wordt weergegeven in de **gecompileerd configuraties** tabblad. Zodra deze zichtbaar zijn op dit tabblad is, kan deze worden toegewezen aan een beheerd knooppunt met behulp van de stappen in [opnieuw toewijzen van een knooppunt naar een ander knooppuntconfiguratie](automation-dsc-getting-started.md#reassigning-a-node-to-a-different-node-configuration).

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Voor meer informatie over hoe voor onboarding knooppunten, Zie [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)
