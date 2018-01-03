---
title: Virtuele-Machineschaalset met Visual Studio implementeren | Microsoft Docs
description: Virtuele-Machineschaalsets met Visual Studio en het Resource Manager-sjabloon implementeren
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 73454abc11a832a1b7f4131bf13699bd0a94edea
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-visual-studio"></a>Het maken van een virtuele-Machineschaalset met Visual Studio
In dit artikel leest u hoe een Azure VM-Schaalset met behulp van een Visual Studio Resource Group Deployment implementeren.

[Azure virtuele-Machineschaalsets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) een Azure Compute-bron om te implementeren en beheren van een verzameling van soortgelijke virtuele machines met automatisch schalen en taakverdeling. U kunt inrichten en implementeren van virtuele-Machineschaalsets met [Azure Resource Manager-sjablonen](https://github.com/Azure/azure-quickstart-templates). Azure Resource Manager-sjablonen kunnen worden geïmplementeerd met behulp van de REST van de Azure CLI, PowerShell, en ook rechtstreeks vanuit Visual Studio. Visual Studio biedt een set van voorbeeld-sjablonen die kunnen worden geïmplementeerd als onderdeel van een implementatie van Azure Resource Group-project.

Azure Resource Group-implementaties zijn een manier om te groeperen en publiceren van een verzameling Azure gerelateerde resources in een met één implementatiebewerking. Voor meer informatie over deze hier: [maken en implementeren van Azure-resourcegroepen met Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Vereisten
Om te beginnen virtuele-Machineschaalsets in Visual Studio implementeren, moet u het volgende:

* Visual Studio 2013 of hoger
* Azure SDK 2.7, 2.8 of 2.9

>[!NOTE]
>Deze instructies wordt ervan uitgegaan dat u met behulp van Visual Studio met [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Een Project maken
1. Maak een nieuw project in Visual Studio door te kiezen **bestand | Nieuwe | Project**.
   
    ![Nieuw bestand][file_new]

2. Onder **Visual C# | Cloud**, kies **Azure Resource Manager** een project voor het implementeren van een Azure Resource Manager-sjabloon maken.
   
    ![Project maken][create_project]

3. Selecteer de Linux- of Windows virtuele Machine Scale ingesteld sjabloon uit de lijst met sjablonen.
   
   ![Sjabloon selecteren][select_Template]

4. Wanneer het project is gemaakt ziet u PowerShell-scripts voor implementatie, een Azure Resource Manager-sjabloon en een parameterbestand voor de virtuele-Machineschaalset.
   
    ![Solution Explorer][solution_explorer]

## <a name="customize-your-project"></a>Aanpassen van uw project
U kunt nu de sjabloon voor het aanpassen van het voor de behoeften van uw toepassing, zoals VM-extensie-eigenschappen toevoegen of bewerken van load-balancingregels bewerken. De virtuele Machine-sjablonen voor het instellen van Scale zijn standaard geconfigureerd voor het implementeren van de extensie AzureDiagnostics, waardoor het eenvoudig om toe te voegen regels voor automatisch schalen. Ook implementeert u een load balancer met een openbaar IP-adres geconfigureerd met binnenkomende NAT-regels. 

De load balancer kunt u verbinding maken met de VM-instanties met SSH (Linux) of RDP (Windows). Het bereik van de front-endpoort begint bij 50000. Voor linux dit dat betekent als u SSH op poort 50000 u doorgestuurd naar poort 22 van de eerste virtuele machine in de Schaalset. Verbinding maken met poort 50001 wordt doorgestuurd naar poort 22 van de tweede VM enzovoort.

 Een goede manier om uw sjablonen met Visual Studio bewerken is het gebruik van de JSON-overzicht om de parameters, variabelen en resources te organiseren. Visual Studio kan verwijzen fouten in de sjabloon met een goed begrip van het schema voordat u deze implementeert.

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a>Het project implementeren
1. De Azure Resource Manager-sjabloon voor het maken van de virtuele-Machineschaalset resource implementeren. Met de rechtermuisknop op het projectknooppunt en kies **implementeren | Nieuwe implementatie**.
   
    ![Sjabloon implementeren][5deploy_Template]
    
2. Uw abonnement te selecteren in het dialoogvenster 'Implementeren naar bronnengroep'.
   
    ![Sjabloon implementeren][6deploy_Template]

3. Hier kunt kunt u een Azure-resourcegroep voor het implementeren van uw sjabloon maken.
   
    ![Nieuwe resourcegroep][new_resource]

4. Klik vervolgens op **Parameters bewerken** parameters die worden doorgegeven aan de sjabloon opgeven. Geef de gebruikersnaam en het wachtwoord voor het besturingssysteem, en is vereist voor de implementatie te maken. Als er geen PowerShell-hulpprogramma's voor Visual Studio is geïnstalleerd, is het raadzaam om te controleren **wachtwoorden opslaan** om te voorkomen dat een verborgen PowerShell-opdrachtprompt of gebruik [keyvault ondersteuning](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Parameters bewerken][edit_parameters]

5. Klik nu op **implementeren**. De **uitvoer** venster wordt de voortgang van de implementatie. Denk eraan dat de actie wordt uitgevoerd de **implementeren AzureResourceGroup.ps1** script.
   
   ![Venster Output][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a>De virtuele-Machineschaalset verkennen
Zodra de implementatie is voltooid, vindt u de nieuwe virtuele-Machineschaalset in de Visual Studio **Cloud Explorer** (de lijst vernieuwen). Cloud Explorer kunt u Azure-resources in Visual Studio tijdens het ontwikkelen van toepassingen beheren. U kunt ook bekijken voor uw virtuele-Machineschaalset in de [Azure-portal](https://portal.azure.com) en [Azure Resource Explorer](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 De portal biedt de beste manier om uw Azure-infrastructuur met een webbrowser visueel beheren terwijl Azure Resource Explorer een eenvoudige manier om te verkennen en fouten opsporen in Azure-resources, biedt geeft een venster in de weergave' exemplaar' en ook weer met PowerShell-opdrachten voor de resources die u bekijkt.

## <a name="next-steps"></a>Volgende stappen
Als u virtuele-Machineschaalsets met Visual Studio hebt geïmplementeerd, kunt u uw project aanpassen aan uw toepassingsvereisten verder aanpassen. Bijvoorbeeld automatisch schalen configureren door het toevoegen van een **Insights** resource, infrastructuur toe te voegen aan uw sjabloon (zoals zelfstandige VM's) of de implementatie van toepassingen met behulp van de extensie voor aangepaste scripts. Goed voorbeeld sjablonen vindt u in de [Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates) GitHub-opslagplaats (zoek 'vmss').

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
