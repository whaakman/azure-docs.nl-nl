---
title: Azure DevTest Labs gebruiken voor training | Microsoft Docs
description: Leer hoe u Azure DevTest Labs gebruiken voor training scenario's.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 85eddaaf101c3e85eca7514b04660163d23c1c80
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666791"
---
# <a name="use-azure-devtest-labs-for-training"></a>Azure DevTest Labs gebruiken voor training
Azure DevTest Labs kan worden gebruikt voor het implementeren van veel belangrijke scenario's naast ontwikkelen en testen. Een van deze scenario's is het instellen van een lab voor training. Azure DevTest Labs kunt u maken van een lab waarin u aangepaste sjablonen die elke leerling gebruiken kunt voor het maken van identieke en geïsoleerde omgevingen voor training kunt opgeven. U kunt beleid om ervoor te zorgen dat training omgevingen beschikbaar voor elke leerling zijn alleen wanneer ze deze nodig hebt en voldoende resources - zoals virtuele machines - vereiste voor de training bevatten toepassen. Ten slotte kunt u de testomgeving eenvoudig delen met stagiairs, die ze in één klik kunnen openen.

![DevTest Labs gebruiken voor training](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs voldoet aan de volgende vereisten die nodig zijn voor het trainen in een virtuele omgeving: 

* Stagiairs zien virtuele machines die zijn gemaakt door andere stagiairs niet
* Elke machine training moeten identiek zijn
* Stagiairs kunnen snel inrichten voor hun omgevingen voor training
* Kosten beheren door ervoor te zorgen dat stagiairs meer virtuele machines bevat dan ze nodig voor de trainings- en ook virtuele machines afsluiten hebt wanneer ze niet gebruiken kunnen niet ophalen
* In het lab training eenvoudig delen met elke leerling
* In het lab training steeds weer opnieuw gebruiken

In dit artikel leert u over verschillende Azure DevTest Labs-functies die kunnen worden gebruikt om te voldoen aan de eerder beschreven Trainingsvereisten en gedetailleerde stappen die u volgen kunt om het instellen van een testomgeving voor training.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Implementatie van training met Azure DevTest Labs
1. **Het lab maken** 
   
    Labs vormen het beginpunt in Azure DevTest Labs. Zodra u een lab hebt gemaakt, kunt u taken uitvoeren zoals als gebruikers (stagiairs) toevoegen aan het lab, beleid instellen voor kosten te beheren, het definiëren van VM-installatiekopieën die snel kunnen maken, en meer.   
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab maken in Azure DevTest Labs](devtest-lab-create-lab.md) |Leer hoe u een lab maken in Azure DevTest Labs in de Azure-portal. |
2. **Training virtuele machines in enkele minuten met kant-en-marketplace-installatiekopieën en aangepaste installatiekopieën maken** 
   
    U kunt kiezen en-klare afbeeldingen uit een groot aantal installatiekopieën in de Azure Marketplace en beschikbaar maken voor de stagiairs in het lab. Als de kant-en-afbeeldingen niet voldoen aan uw vereisten, kunt u een aangepaste installatiekopie maken met het maken van een lab-virtuele machine met een kant-en-installatiekopie uit Azure Marketplace, installeren van de software die u nodig hebt voor de training en opslaan van de virtuele machine als aangepaste installatiekopie in het lab. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Azure Marketplace-installatiekopieën configureren](devtest-lab-configure-marketplace-images.md) |Leer hoe u kunt Azure Marketplace-installatiekopieën whitelist; maken van alleen de installatiekopieën beschikbaar voor selectie wilt u gebruiken voor de training. |
   | [Een aangepaste installatiekopie maken](devtest-lab-create-template.md) |Een aangepaste installatiekopie maken door het vooraf installeren van de software die u nodig hebt voor de training zodat stagiairs kunnen snel een virtuele machine met behulp van de aangepaste installatiekopie maken. |
3. **Herbruikbare sjablonen maken voor training-machines** 
   
    Een formule in Azure DevTest Labs is een lijst van de eigenschap standaardwaarden gebruikt voor het maken van een virtuele machine. U kunt een formule in het lab maken door het verzamelen van een afbeelding, een VM-grootte (een combinatie van CPU en RAM-geheugen) en een virtueel netwerk. Elke leerling kan de formule in het lab zien en gebruiken om u te maken van een virtuele machine. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [DevTest Labs-formules voor het maken van virtuele machines beheren](devtest-lab-manage-formulas.md) |Meer informatie over hoe u een formule kunt maken met het ophalen van een installatiekopie van een VM-grootte (combinatie van CPU en RAM-geheugen) en een virtueel netwerk. |
4. **Beheersen van kosten**
   
    Azure DevTest Labs kunt u een beleid instellen in de testomgeving om op te geven van het maximum aantal virtuele machines dat kan worden gemaakt door een leerling in het lab. 
   
    Als u bent bezig met meerdere dagen training en wilt stoppen alle virtuele machines op een bepaald tijdstip van de dag en vervolgens automatisch opnieuw opgestart ze de volgende dag, kunt u eenvoudig uitvoeren die door in te stellen automatisch afsluiten en automatisch wordt gestart voor in het lab. 
   
    Ten slotte is het zo dat als training voltooid is u alle virtuele machines in één keer kunt verwijderen door het uitvoeren van een enkel PowerShell-script. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Beleid voor lab maken](devtest-lab-set-lab-policy.md) |Kosten beheren met het instellen van beleidsregels in het lab. |
   | [Alle in het lab-VM's met een PowerShell-script verwijderen](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Verwijder de labs in één bewerking als de training voltooid is. |
5. **De testomgeving met elke leerling delen**
   
    Labs kunnen rechtstreeks worden benaderd via een koppeling die u met uw stagiairs deelt. Uw stagiairs hoef niet eens een Azure-account hebben, zolang ze hebben een [Microsoft-account](devtest-lab-faq.md#what-is-a-microsoft-account). Stagiairs zien geen virtuele machines die zijn gemaakt door andere stagiairs.  
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een leerling toevoegen aan een lab in Azure DevTest Labs](devtest-lab-add-devtest-user.md) |Gebruik de Azure-portal stagiairs toevoegen aan uw testomgeving training. |
   | [Stagiairs toevoegen aan het lab met een PowerShell-script](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |PowerShell gebruiken voor het automatiseren van toe te voegen stagiairs aan uw lab training. |
   | [Haal een koppeling naar het lab](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Meer informatie over hoe een lab zijn rechtstreeks toegankelijk via een hyperlink. |
6. **In het lab opnieuw gebruiken** 
   
    U kunt lab maken, met inbegrip van aangepaste instellingen door Resource Manager-sjabloon maken en deze opnieuw maken van identieke labs automatiseren. 
   
    Meer informatie door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab met behulp van Resource Manager-sjabloon maken](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Labs maken in Azure DevTest Labs met behulp van Resource Manager-sjablonen. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

