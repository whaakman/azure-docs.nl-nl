---
title: Validatierapport voor Azure Stack | Microsoft Docs
description: Het rapport Azure Stack-gereedheid Checker gebruiken om te bekijken van resultaten.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 06b5660a9428e98d2e99b5d447a05700968ec884
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2018
ms.locfileid: "43381910"
---
# <a name="azure-stack-validation-report"></a>Azure Stack-validatierapport
Het hulpprogramma Azure Stack-gereedheid van de Registercontrole voor validaties die ondersteuning bieden voor implementatie en het onderhoud van een Azure Stack-omgeving gebruiken. Het hulpprogramma schrijft de resultaten naar een .json-rapportbestand. Het rapport bevat gedetailleerde en samengevatte gegevens over de status van de vereisten voor de implementatie van Azure Stack. Het rapport bevat ook informatie over geheimen, rotatie voor bestaande Azure Stack-implementaties.  

 ## <a name="where-to-find-the-report"></a>Waar vind ik het rapport
Wanneer het hulpprogramma wordt uitgevoerd, registreert hij de resultaten naar **AzsReadinessCheckerReport.json**. Het hulpprogramma maakt ook een logboek met de naam **AzsReadinessChecker.log**. De locatie van deze bestanden wordt weergegeven met de validatieresultaten in PowerShell.

![Run-validatie](./media/azure-stack-validation-report/validation.png)

Beide bestanden behouden de resultaten van de volgende validatiecontroles wanneer op dezelfde computer worden uitgevoerd.  Het hulpprogramma kan bijvoorbeeld worden uitgevoerd voor het valideren van certificaten, opnieuw uitvoeren voor het valideren van Azure-identiteit en vervolgens een derde tijd voor de registratie valideert. De resultaten van alle drie validaties zijn beschikbaar in het resulterende .json-rapport.  

Standaard beide bestanden worden geschreven naar *C:\Users\<gebruikersnaam > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Gebruik de **- OutputPath** ***&lt;pad&gt;*** parameter aan het einde van het uitvoeren vanaf de opdrachtregel om op te geven van een andere locatie.   
- Gebruik de **- CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen van gegevens uit *AzsReadinessCheckerReport.json*. over eerdere uitvoeringen van het hulpprogramma.

## <a name="view-the-report"></a>Het rapport weergeven
Als u wilt het rapport weergeven in PowerShell, het pad naar het rapport als een waarde voor opgeven **- ReportPath**. Met deze opdracht wordt de inhoud van het rapport weergegeven en identificeert validaties waarvoor nog geen resultaten.

Bijvoorbeeld: als u wilt weergeven van het rapport vanuit een PowerShell-prompt die is geopend op de locatie van het rapport, uitvoeren: 
   > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json` 

De uitvoer lijkt op de volgende afbeelding:

![rapport weergeven](./media/azure-stack-validation-report/view-report.png)

## <a name="view-the-report-summary"></a>Het rapport wordt een samenvatting weergeven
Als u een overzicht van het rapport, kunt u toevoegen de **-samenvatting** overschakelen naar het einde van de PowerShell-opdrachtregel. Bijvoorbeeld: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -summary`  

De samenvatting validaties waarvoor geen resultaten bevat en wordt aangegeven geslaagd of niet voor validaties die voltooid zijn. De uitvoer lijkt op de volgende afbeelding:

![rapport-overzicht](./media/azure-stack-validation-report/report-summary.png)


## <a name="view-a-filtered-report"></a>Een gefilterd rapport weergeven
Als u wilt een rapport dat wordt gefilterd op één type validatie weergeven, gebruikt u de **- ReportSections** parameter met een van de volgende waarden:
- Certificaat
- AzureRegistration
- Azure-identiteit
- Taken   
- Alle  

Bijvoorbeeld, om het rapport weer te geven samenvatting voor certificaten alleen, gebruik de volgende PowerShell-opdrachtregel: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Zie ook
[Start-AzsReadinessChecker cmdlet-verwijzing](azure-stack-azsreadiness-cmdlet.md)
