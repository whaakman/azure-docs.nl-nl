---
title: Azure DevTest Labs gebruiken voor virtuele machine en PaaS-testomgevingen | Microsoft Docs
description: Leer hoe u Azure DevTest Labs gebruiken voor VM Testscenario's en PaaS-omgeving.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 58ab6d502ec5397604c562aedffddb9f48cbb699
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38697395"
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a>Gebruik Azure DevTest Labs voor virtuele machine en PaaS-testomgevingen

U kunt Azure DevTest Labs gebruiken voor het implementeren van allerlei scenario's. Eén van de belangrijkste scenario's is het inzetten van DevTest Labs voor het hosten van machines voor testers. 

In dit scenario biedt DevTest Labs deze voordelen:

- Testers kunnen de nieuwste versie van hun toepassing testen door Windows- en Linux-omgevingen snel in te richten met behulp van herbruikbare sjablonen en artefacten.
- Testers kunnen hun belastingstests omhoog schalen door meerdere test-agents in te richten.
- Beheerders kunnen kosten beheren door ervoor te zorgen dat:
  - Testers kunnen meer virtuele machines bevat dan ze nodig hebben niet ophalen.
  - VM's worden afgesloten wanneer deze niet in gebruik.

![DevTest Labs gebruiken voor training](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

In dit artikel leert u over verschillende Azure DevTest Labs-functies gebruikt om te voldoen aan vereisten tester en gedetailleerde stappen te volgen voor het instellen van een lab.

## <a name="implementing-test-environments-with-azure-devtest-labs"></a>Implementatie van testomgevingen met Azure DevTest Labs
1. **Het lab maken** 
   
    Labs vormen het beginpunt in Azure DevTest Labs. Zodra u een lab hebt gemaakt, kunt u uitvoeren zoals het toevoegen van gebruikers (testers) aan het lab instellen van beleidsregels voor het beheren van kosten, het definiëren van VM-installatiekopieën die snel kunnen maken en nog veel meer.  
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab maken in Azure DevTest Labs](devtest-lab-create-lab.md) |Leer hoe u een lab maken in Azure DevTest Labs in de Azure-portal. |
2. **Virtuele machines in enkele minuten met kant-en-marketplace-installatiekopieën en aangepaste installatiekopieën maken** 
   
    U kunt kiezen en-klare afbeeldingen uit een groot aantal installatiekopieën in de Azure Marketplace en zodat ze beschikbaar zijn in het lab. Als de kant-en-afbeeldingen niet voldoen aan uw vereisten, kunt u een aangepaste installatiekopie maken met het maken van een lab-virtuele machine een kant-en-installatiekopie met behulp van Azure Marketplace, de software die u nodig hebt, installeert en het opslaan van de virtuele machine als een aangepaste installatiekopie in het lab.

    Als u aangepaste installatiekopieën gebruikt, kunt u overwegen een factory installatiekopie te maken en distribueren van uw afbeeldingen. Een afbeelding factory is een configuratie als code-oplossing die regelmatig bouwt en distribueert het geconfigureerde installatiekopieën automatisch. Dit bespaart de tijd die nodig is om het systeem handmatig configureren nadat een virtuele machine is gemaakt met het Basisbesturingssysteem.
  
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Azure Marketplace-installatiekopieën configureren](devtest-lab-configure-marketplace-images.md) |Meer informatie over hoe u kunt Azure Marketplace-installatiekopieën van lijst met toegestane adressen, zodat beschikbaar voor selectie alleen de installatiekopieën die u wilt gebruiken voor de testers.|
   | [Een aangepaste installatiekopie maken](devtest-lab-create-template.md) |Een aangepaste installatiekopie maken door het vooraf installeren van de software die u nodig hebt, zodat de testers kunnen snel een virtuele machine met behulp van de aangepaste installatiekopie maken.|
   | [Meer informatie over de installatiekopie factory](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bekijk een video waarin wordt beschreven hoe u kunt instellen en gebruiken van een installatiekopie-factory.|

3. **Herbruikbare sjablonen voor testmachines maken** 
   
    Een formule in Azure DevTest Labs is een lijst van de eigenschap standaardwaarden gebruikt voor het maken van een virtuele machine. U kunt een formule in het lab maken door het verzamelen van een afbeelding, een VM-grootte (een combinatie van CPU en RAM-geheugen) en een virtueel netwerk. Elke tester kan de formule in het lab zien en gebruiken om u te maken van een virtuele machine. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [DevTest Labs-formules voor het maken van virtuele machines beheren](devtest-lab-manage-formulas.md) |Meer informatie over hoe u een formule kunt maken met het ophalen van een installatiekopie van een VM-grootte (combinatie van CPU en RAM-geheugen) en een virtueel netwerk.|

3. **Multi-VM-en testomgevingen maken** 
   
    U kunt Azure Resource Manager-sjablonen gebruiken en definieert de infrastructuur en configuratie van uw Azure-oplossing herhaaldelijk implementeren meerdere test-VM's in een consistente status.

    Azure PaaS-resources kunnen worden ingericht in een omgeving met een Resource Manager-sjabloon en weergegeven in het bijhouden van kosten. Virtuele machine automatisch afsluiten is echter niet van toepassing op PaaS-resources.

    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md) |Meer informatie over hoe u meerdere virtuele machines in een consistente status hebben voor uw testomgeving kunt implementeren.|

4. **Als u wilt inschakelen, flexibele aanpassing van de VM-artefacten maken**

   Artefacten worden gebruikt om te implementeren en configureren van uw toepassing nadat een virtuele machine is ingericht. Artefacten kunnen het volgende zijn:

   - Hulpprogramma's die u wilt installeren op de VM - zoals agents, Fiddler en Visual Studio.
   - Acties die u uitvoeren op de VM wilt-zoals een opslagplaats te klonen.
   - Programma's die u wilt testen.

   Veel artefacten zijn al beschikbaar out-of-the-box. Maar als u meer aanpassen voor uw specifieke behoeften wilt, kunt u uw eigen aangepaste artefacten maken.

   Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Aangepaste artefacten maken voor uw DevTest Labs-VM](devtest-lab-artifact-author.md) |Maak uw eigen aangepaste artefacten voor de virtuele machines in uw testomgeving.|
   | [Toevoegen van een Git-opslagplaats voor het opslaan van aangepaste artefacten en Azure Resource Manager-sjablonen voor gebruik in Azure DevTest Labs](devtest-lab-add-artifact-repo.md) |Leer hoe u voor het opslaan van uw aangepaste artefacten in uw eigen persoonlijke Git-opslagplaats.|

5. **Beheersen van kosten**
   
    Azure DevTest Labs kunt u een beleid instellen in de testomgeving om op te geven van het maximum aantal virtuele machines dat kan worden gemaakt door een testprogramma in het lab. 
   
    Als uw testteam heeft een set werken planning en u wilt stoppen alle virtuele machines op een bepaald tijdstip van de dag en automatisch start vervolgens de volgende dag, kunt u die eenvoudig uitvoeren door het instellen van beleidsregels voor automatisch afsluiten en automatisch wordt gestart in de testomgeving. 
   
    Ten slotte, als app-ontwikkeling voltooid is, kunt u verwijderen alle virtuele machines in één keer een eenmalige PowerShell-script uit te voeren. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Beleid voor lab maken](devtest-lab-set-lab-policy.md) |Kosten beheren met het instellen van beleidsregels in het lab. |
   | [Alle in het lab-VM's met een PowerShell-script verwijderen](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Verwijder de labs in één bewerking wanneer de test is voltooid.|

1. **Een virtueel netwerk toevoegen aan een Lab** 
   
    DevTest Labs maakt een nieuw virtueel netwerk (VNET) wanneer er een lab wordt gemaakt. Als u uw eigen VNET hebt geconfigureerd – bijvoorbeeld via ExpressRoute of site-naar-site VPN-kunt u dit VNET toevoegen aan de instellingen van de virtuele netwerken van uw testomgeving zodat deze beschikbaar is bij het maken van virtuele machines.

    Er is bovendien een Azure Active Directory-domein join artefact beschikbaar die aan een virtuele machine wordt toegevoegd aan een domein als de virtuele machine wordt gemaakt. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een virtueel netwerk configureren in Azure DevTest Labs](devtest-lab-configure-vnet.md) |Informatie over het configureren van een virtueel netwerk in Azure DevTest Labs met behulp van de Azure portal.|

6. **In het lab delen met elke tester**
   
    Labs kunnen rechtstreeks worden benaderd via een koppeling die u met uw testers delen. Ze hoeft te hebben van een Azure-account, zolang ze hebben een [Microsoft-account](devtest-lab-faq.md#what-is-a-microsoft-account). Testers zien geen virtuele machines die zijn gemaakt door andere testers.  
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een testprogramma toevoegen aan een lab in Azure DevTest Labs](devtest-lab-add-devtest-user.md) |Gebruik de Azure-portal testers toevoegen aan uw testomgeving.|
   | [Testers toevoegen aan het lab met een PowerShell-script](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |PowerShell gebruiken voor het automatiseren van toe te voegen testers aan uw testomgeving. |
   | [Haal een koppeling naar het lab](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Meer informatie over hoe testers hebben rechtstreeks toegang tot een lab via een hyperlink.|

7. **Lab maken voor meer teams automatiseren** 
   
    U kunt lab maken, met inbegrip van aangepaste instellingen door Resource Manager-sjabloon maken en deze opnieuw maken van identieke labs automatiseren. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab met behulp van Resource Manager-sjabloon maken](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Labs maken in Azure DevTest Labs met behulp van Resource Manager-sjablonen. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

