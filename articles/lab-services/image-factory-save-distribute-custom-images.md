---
title: Opslaan en distribueren van afbeeldingen in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie-factory in Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: feabd055833e5f0d850138af528cce1da82cae49
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439862"
---
# <a name="save-custom-images-and-distribute-to-multiple-labs"></a>Aangepaste installatiekopieën opslaan en distribueren naar meerdere labs
In dit artikel behandelt biedt u de stappen voor het opslaan van aangepaste installatiekopieën van de gemaakte virtuele machines (VM's). Ook wordt beschreven hoe u deze aangepaste installatiekopieën naar andere DevTest Labs in de organisatie distribueren.

## <a name="prerequisites"></a>Vereisten
De volgende items moeten al zijn voldaan:

- Een lab voor de Factory installatiekopie in Azure DevTest Labs.
- Een Azure DevOps-Project dat wordt gebruikt voor het automatiseren van de factory installatiekopie.
- Locatie van de code de scripts en configuratie (in ons voorbeeld, in dezelfde die devops-Project in de vorige stap is genoemd) met de gegevensbron.
- Build-definitie voor de organisatie van de Azure Powershell-taken.

Indien nodig, voert u de stappen de [een factory installatiekopie uitvoeren vanaf Azure DevOps](image-factory-set-up-devops-lab.md) maken of instellen van deze items. 

## <a name="save-vms-as-generalized-vhds"></a>Virtuele machines opslaan als een gegeneraliseerde VHD
Sla de bestaande VM's als gegeneraliseerde virtuele harde schijven.  Er is een PowerShell-voorbeeldscript om op te slaan van de bestaande VM's als gegeneraliseerde virtuele harde schijven. Als u wilt gebruiken, eerst, Voeg een ander **Azure Powershell** taak aan de build-definitie, zoals wordt weergegeven in de volgende afbeelding:

![Azure PowerShell-stap toevoegen](./media/save-distribute-custom-images/powershell-step.png)

Zodra u de nieuwe taak in de lijst hebt, selecteert u het item, zodat we alle gegevens invullen kunt zoals wordt weergegeven in de volgende afbeelding: 

![PowerShell-instellingen](./media/save-distribute-custom-images/powershell-settings.png)


## <a name="generalized-vs-specialized-custom-images"></a>Gegeneraliseerde en gespecialiseerde aangepaste installatiekopieën
In de [Azure-portal](https://portal.azure.com), wanneer het maken van een aangepaste installatiekopie van een virtuele machine, kunt u kiezen om een gegeneraliseerde of een gespecialiseerde aangepaste installatiekopie.

- **Gespecialiseerde aangepaste installatiekopie:** Sysprep/inrichting is niet uitgevoerd op de machine. Dit betekent dat de installatiekopie een exacte kopie van de schijf met het besturingssysteem op de bestaande virtuele machine (snapshot).  De dezelfde bestanden, toepassingen, gebruikersaccounts, computernaam, enzovoort, zijn alle aanwezig wanneer we een nieuwe virtuele machine van deze aangepaste installatiekopie maken.
- **Algemene aangepaste installatiekopie:** Sysprep/inrichting is op de machine uitgevoerd.  Wanneer dit proces wordt uitgevoerd, verwijdert u gebruikersaccounts, verwijdert u de naam van de computer, verwijdert de gebruiker registeronderdelen, enz., met het doel van de installatiekopie van het generaliseren, zodat deze kan worden aangepast bij het maken van een andere virtuele machine.  Wanneer u een virtuele machine generaliseren (door sysprep), de huidige virtuele machine door het proces wordt vernietigd – deze niet meer worden functionele.

Het script voor het uitlijnen van aangepaste installatiekopieën in de fabriek installatiekopie VHD's worden opgeslagen voor virtuele machines in de vorige stap hebt gemaakt (geïdentificeerd op basis van een label voor de resources in Azure).

## <a name="update-configuration-for-distributing-images"></a>Update-configuratie voor de distributie van afbeeldingen
De volgende stap in het proces is om de aangepaste installatiekopieën van de installatiekopie factory lab uit tot het andere labs die ze nodig hebt. Het belangrijkste onderdeel van dit proces is de **labs.json** configuratiebestand. U vindt dit bestand in de **configuratie** map in de fabriek installatiekopie.

Er zijn twee belangrijke dingen die u in het configuratiebestand labs.json vermeld:

- Voor een unieke id van een specifieke bestemming lab met behulp van de abonnements-ID en de naam van de testomgeving.
- De specifieke set installatiekopieën die naar het lab als relatieve paden naar de hoofdmap van de configuratie moet worden gepusht. U kunt hele map (voor alle installatiekopieën in die map) opgeven te.

Hier volgt een voorbeeld labs.json-bestand met twee labs vermeld. In dit geval, distribueert u afbeeldingen op twee verschillende labs.

```json
{
   "Labs": [
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2012R2",
               "Win2016/Datacenter.json"
         ]
      },
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2016/Datacenter.json"
         ]
      }
   ]
}
```

## <a name="create-a-build-task"></a>Een build-taak maken
Met behulp van de stappen die u eerder in dit artikel hebt gezien toevoegen van een extra **Azure Powershell** build-taak aan u build-definitie. Vul de gegevens zoals wordt weergegeven in de volgende afbeelding: 

![Taak voor het distribueren van afbeeldingen bouwen](./media/save-distribute-custom-images/second-build-task-powershell.png)

De parameters zijn: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(DevTestLabName) -maxConcurrentJobs 20`

Deze taak neemt aanwezig zijn in de fabriek de installatiekopie van aangepaste installatiekopieën en nieuwe versies tot een gedefinieerd in het bestand Labs.json labs.

## <a name="queue-the-build"></a>De build-wachtrij
Zodra de distributie-build-taak voltooid is, de wachtrij een nieuwe build om ervoor te zorgen dat alles werkt. Wanneer de build voltooid is, worden de nieuwe aangepaste installatiekopieën in de doel-testomgeving die is ingevoerd in het configuratiebestand Labs.json weergegeven.

## <a name="next-steps"></a>Volgende stappen
In het volgende artikel in de reeks, kunt u de factory installatiekopie bijwerken met een bewaarperiode beleid en opschonen stappen: [Een retentiebeleid instellen en uitvoeren van opschoningsscripts](image-factory-set-retention-policy-cleanup.md).
