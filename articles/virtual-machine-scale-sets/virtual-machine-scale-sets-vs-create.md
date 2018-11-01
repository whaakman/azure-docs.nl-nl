---
title: Virtuele-Machineschaalset met behulp van Visual Studio implementeren | Microsoft Docs
description: Virtual Machine Scale Sets met Visual Studio en Resource Manager-sjabloon implementeren
services: virtual-machine-scale-sets
ms.custom: H1Hack27Feb2017
ms.workload: na
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: manayar
ms.openlocfilehash: 3d472aeaae7e7f02eba58aadea1df042d6c0f27b
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741419"
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-visual-studio"></a>Een virtuele-Machineschaalset maken met Visual Studio
Dit artikel laat u over het implementeren van een Azure VM-Schaalset met Visual Studio implementatie van een resourcegroep.

[Azure Virtual Machine Scale Sets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) is een Azure Compute-resource om te implementeren en beheren van een verzameling van dergelijke virtuele machines met automatisch schalen en taakverdeling. U kunt inrichten en implementeren van Virtual Machine Scale Sets met behulp van [Azure Resource Manager-sjablonen](https://github.com/Azure/azure-quickstart-templates). Azure Resource Manager-sjablonen kunnen worden geïmplementeerd met behulp van Azure CLI, PowerShell, REST en ook rechtstreeks vanuit Visual Studio. Visual Studio biedt een aantal voorbeeldsjablonen, die kan worden geïmplementeerd als onderdeel van een implementatie van Azure Resource Group-project.

Azure-resourcegroep implementaties zijn een manier om te groeperen en publiceren van een set van gerelateerde Azure-resources in een van één implementatiebewerking. Meer informatie over deze hier: [maken en implementeren van Azure-resourcegroepen met Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Vereisten
Om te beginnen Virtual Machine Scale Sets in Visual Studio implementeren, moet u het volgende:

* Visual Studio 2013 of hoger
* Azure SDK 2.7, 2.8 of 2.9

>[!NOTE]
>Deze instructies wordt ervan uitgegaan dat u met behulp van Visual Studio met [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Een Project maken
1. Een nieuw project maken in Visual Studio door te kiezen **bestand | Nieuwe | Project**.
   
    ![Nieuw bestand][file_new]

2. Onder **Visual C# | Cloud**, kiest u **Azure Resource Manager** om een project voor het implementeren van een Azure Resource Manager-sjabloon te maken.
   
    ![Project maken][create_project]

3. Selecteer de Linux- of Windows Virtual Machine Scale ingesteld sjabloon uit de lijst met sjablonen.
   
   ![Sjabloon selecteren][select_Template]

4. Wanneer het project is gemaakt ziet u PowerShell-scripts voor implementatie, een Azure Resource Manager-sjabloon en een parameterbestand voor de virtuele-Machineschaalset.
   
    ![Solution Explorer][solution_explorer]

## <a name="customize-your-project"></a>Aanpassen van uw project
U kunt nu de sjabloon aan te passen aan de behoeften van uw toepassing, zoals VM-extensie-eigenschappen toe te voegen of het bewerken van de load balancer-regels bewerken. De virtuele Machine-sjablonen voor het instellen van schaal zijn standaard geconfigureerd voor het implementeren van de extensie AzureDiagnostics, waardoor het eenvoudig om toe te voegen regels voor automatisch schalen. Deze implementeert ook een load balancer met een openbaar IP-adres, geconfigureerd met binnenkomende NAT-regels. 

De load balancer kunt u verbinding maken met de VM-exemplaren met SSH (Linux) of RDP (Windows). De front-end-poortbereik begint bij 50000. Voor linux dat dat betekent als u SSH op poort 50000 u worden doorgestuurd naar poort 22 van de eerste virtuele machine in de Schaalset. Verbinding maken met poort 50001 wordt doorgestuurd naar poort 22 van de tweede VM enzovoort.

 Een goede manier om het bewerken van sjablonen met Visual Studio is het gebruik van de JSON-overzicht om de parameters, variabelen en resources te organiseren. Met een goed begrip van het schema kan Visual Studio fouten in uw sjabloon verwijzen, voordat u deze implementeert.

![JSON-Explorer][json_explorer]

## <a name="deploy-the-project"></a>Het project implementeren
1. De Azure Resource Manager-sjabloon voor het maken van de virtuele-Machineschaalset resource implementeren. Met de rechtermuisknop op het projectknooppunt en kies **implementeren | Nieuwe implementatie**.
   
    ![Sjabloon implementeren][5deploy_Template]
    
2. Selecteer uw abonnement in het dialoogvenster 'Implementeren naar resourcegroep'.
   
    ![Sjabloon implementeren][6deploy_Template]

3. Hier kunt u een Azure-resourcegroep voor het implementeren van uw sjabloon te maken.
   
    ![Nieuwe resourcegroep][new_resource]

4. Klik vervolgens op **Parameters bewerken** parameters die worden doorgegeven aan uw sjabloon opgeven. Geef de gebruikersnaam en wachtwoord voor het besturingssysteem, die vereist is om de implementatie te maken. Als u geen PowerShell-hulpprogramma's voor Visual Studio is geïnstalleerd, is het raadzaam om te controleren op **wachtwoorden opslaan** om te voorkomen dat een verborgen PowerShell-opdrachtprompt, of gebruik [keyvault ondersteuning](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Parameters bewerken][edit_parameters]

5. Klik nu op **implementeren**. De **uitvoer** venster toont de voortgang van de implementatie. Houd er rekening mee dat de actie wordt uitgevoerd de **implementeren AzureResourceGroup.ps1** script.
   
   ![Uitvoervenster][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a>Uw virtuele-Machineschaalset verkennen
Nadat de implementatie is voltooid, vindt u de nieuwe virtuele-Machineschaalset in de Visual Studio **Cloud Explorer** (Vernieuw de lijst met). Cloud Explorer kunt u in Visual Studio Azure-resources beheren tijdens het ontwikkelen van toepassingen. U kunt ook weergeven voor uw virtuele-Machineschaalset opgehaald in de [Azure-portal](https://portal.azure.com) en [Azure Resource Explorer](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 De portal biedt de beste manier om uw Azure-infrastructuur met een webbrowser, visueel beheren terwijl Azure Resource Explorer een eenvoudige manier om te verkennen en fouten opsporen in Azure-resources, biedt waardoor een venster in de instantieweergave' ' en ook PowerShell-opdrachten worden weergegeven voor de resources die wilt u.

## <a name="next-steps"></a>Volgende stappen
Zodra u Virtual Machine Scale Sets met Visual Studio hebt geïmplementeerd, kunt u uw project aan de behoeften van uw toepassingsvereisten verder aanpassen. Bijvoorbeeld automatisch schalen configureren door toe te voegen een **Insights** resource, infrastructuur toe te voegen aan uw sjabloon (zoals zelfstandige VM's) of de implementatie van toepassingen die gebruikmaken van de extensie voor aangepaste scripts. Goed voorbeeldsjablonen kunnen u vinden in de [Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates) GitHub-opslagplaats (zoek 'vmss').

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
