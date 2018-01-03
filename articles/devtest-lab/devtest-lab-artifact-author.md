---
title: Maken van aangepaste artefacten voor uw virtuele machine van DevTest Labs | Microsoft Docs
description: Informatie over het ontwerpen van uw eigen artefacten gebruiken met Azure DevTest Labs.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: v-craic
ms.openlocfilehash: 7766227d66df94eca72072f52ff02928f8ee277b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="create-custom-artifacts-for-your-devtest-labs-virtual-machine"></a>Maken van aangepaste artefacten voor uw virtuele machine van DevTest Labs

Bekijk de volgende video voor een overzicht van de stappen in dit artikel:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Overzicht
U kunt *artefacten* te implementeren en uw toepassing instellen nadat u een virtuele machine inrichten. Een artefact bestaat uit een definitiebestand van artefacten en andere scriptbestanden die zijn opgeslagen in een map in een Git-opslagplaats. Artefacten definitiebestanden bestaan uit JSON en uitdrukkingen die u gebruiken kunt om op te geven wat u wilt installeren op een virtuele machine. Bijvoorbeeld, kunt u de naam van een artefact, een opdracht om uit te voeren en parameters die beschikbaar zijn wanneer de opdracht wordt uitgevoerd. U kunt verwijzen naar andere scriptbestanden in de definitie van het artefact bestand met de naam.

## <a name="artifact-definition-file-format"></a>Bestandsindeling van pakketdefinities artefact
Het volgende voorbeeld ziet u de secties die gezamenlijk de basisstructuur van een definitiebestand van:

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Elementnaam | Vereist? | Beschrijving |
| --- | --- | --- |
| $schema |Nee |Locatie van het JSON-schema-bestand. Het JSON-schema-bestand kunt u de geldigheid van het definitiebestand voor testen. |
| titel |Ja |Naam van het artefact weergegeven in de testomgeving. |
| description |Ja |Beschrijving van het artefact weergegeven in de testomgeving. |
| iconUri |Nee |De URI van het pictogram weergegeven in de testomgeving. |
| targetOsType |Ja |Besturingssysteem van de virtuele machine waarop het artefact is geïnstalleerd. Ondersteunde opties zijn Windows- en Linux. |
| parameters |Nee |De waarden die beschikbaar zijn wanneer de installatieopdracht artefacten op een computer wordt uitgevoerd. Zo kunt u uw artefacten aanpassen. |
| OpdrachtUitvoeren |Ja |Artefacten installeren de opdracht die wordt uitgevoerd op een virtuele machine. |

### <a name="artifact-parameters"></a>Parameters van artefacten
Opgeven welke waarden van een gebruiker kan invoer wanneer ze een artefact installeert in het gedeelte parameters van de definitie van het bestand. U kunt verwijzen naar deze waarden in de opdracht voor het installeren van artefacten.

Om parameters te definiëren, gebruikt u de volgende structuur:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Elementnaam | Vereist? | Beschrijving |
| --- | --- | --- |
| type |Ja |Type van parameterwaarde. Zie de volgende lijst voor de toegestane typen. |
| Weergavenaam |Ja |De naam van de parameter die aan een gebruiker in de testomgeving wordt weergegeven. | |
| description |Ja |Beschrijving van de parameter die wordt weergegeven in de testomgeving. |

Toegestane typen zijn:

* tekenreeks (een willekeurige geldige JSON-tekenreeks)
* Int (een geldig JSON geheel getal)
* BOOL (geldige JSON Boolean)
* matrix (geldige JSON-matrix)

## <a name="artifact-expressions-and-functions"></a>Artefacten expressies en functies
U kunt expressies gebruiken en functies om te maken van het artefact opdracht installeren.
Expressies tussen haakjes ([en]), en worden geëvalueerd wanneer het artefact is geïnstalleerd. Expressies kunnen overal voorkomen in de waarde van een JSON-tekenreeks. Expressies wordt altijd een andere JSON-waarde retourneren. Als u een letterlijke tekenreeks die begint met een linkerhaak ([]) gebruiken moet, moet u twee vierkante haken ([[) gebruiken.
Normaal gesproken u expressies gebruiken met functies voor het maken van een waarde. Net als in JavaScript-functieaanroepen die zijn opgemaakt als **functionName(arg1,arg2,arg3)**.

De volgende lijst bevat algemene functies:

* **parameters(parameterName)**: retourneert een parameterwaarde die is opgegeven bij de artefacten-opdracht wordt uitgevoerd.
* **concat (arg1, arg2, arg3,...)** : Combineert meerdere tekenreekswaarden. Deze functie kan duren voordat een aantal argumenten.

Het volgende voorbeeld ziet u hoe om expressies en functies te maken van een waarde:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Een aangepaste artefacten maken

1. Installeer een JSON-editor. U moet een JSON-editor om te werken met artefacten definitiebestanden. Wordt u aangeraden [Visual Studio Code](https://code.visualstudio.com/), die beschikbaar is voor Windows, Linux en OS X.
2. Haal een voorbeeldbestand artifactfile.json-definitie. Bekijk de artefacten gemaakt door het team DevTest Labs in onze [GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab). Er is een uitgebreide bibliotheek met artefacten kunt u uw eigen artefacten gemaakt. Download een definitiebestand van artefacten en wijzigingen aanbrengen in het maken van uw eigen artefacten.
3. Maakt gebruik van IntelliSense. Gebruik IntelliSense om geldige elementen die u gebruiken kunt voor het maken van een artefact definitie-bestand. Ook ziet u de verschillende opties voor waarden van een element. Bijvoorbeeld, wanneer u bewerkt de **targetOsType** element IntelliSense ziet u twee opties voor Windows of Linux.
4. Opslaan van het artefact in een [Git-opslagplaats](devtest-lab-add-artifact-repo.md).
   
   1. Maak een afzonderlijke map voor elke artefacten. Naam van de map moet hetzelfde zijn als de naam van de artefacten.
   2. Sla het artefact-definitiebestand (artifactfile.json) in de map die u hebt gemaakt.
   3. De scripts die worden verwezen vanuit de installatieopdracht artefact opslaan.
      
      Hier volgt een voorbeeld van een map artefact kan als volgt uitzien:
      
      ![Voorbeeld van de map artefact](./media/devtest-lab-artifact-author/git-repo.png)
5. De artefactenopslagplaats toevoegen aan het lab. Zie [een Git-opslagplaats voor artefacten en sjablonen toevoegen](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Verwante artikelen:
* [Het artefact mislukte diagnoses in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Een virtuele machine toevoegen aan een bestaand Active Directory-domein met behulp van een Resource Manager-sjabloon in DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [een Git-opslagplaats met artefacten toevoegen aan een lab](devtest-lab-add-artifact-repo.md).

