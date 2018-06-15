---
title: Validatierapport voor Azure-Stack | Microsoft Docs
description: Het rapport Azure Stack gereedheid Checker gebruiken om te controleren validatieresultaten.
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
ms.openlocfilehash: a0ca0ae3ed615f6bc2774364f7a443023b911b5d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937816"
---
# <a name="azure-stack-validation-report"></a>Azure Stack validatierapport
Het hulpprogramma Azure Stack gereedheid Checker uitgevoerd validaties die ondersteuning bieden voor implementatie en het onderhoud van een Azure-Stack-omgeving. Het hulpprogramma schrijft validatieresultaten naar een .json-bestand van rapport. Het rapport bevat gedetailleerde en samengevatte gegevens op de status van de vereisten voor de implementatie van Azure-Stack en geheimen rotatie voor bestaande Azure-Stack-implementaties.  

 ## <a name="where-to-find-the-report"></a>Waar vind ik het rapport
Wanneer het hulpprogramma wordt uitgevoerd, geregistreerd in het logboek resultaten **AzsReadinessCheckerReport.json**. Het hulpprogramma maakt ook een logboek met de naam **AzsReadinessChecker.log**. De locatie van deze bestanden wordt weergegeven met de validatieresultaten in PowerShell.

![validatie van uitvoeren](./media/azure-stack-validation-report/validation.png)

Beide bestanden persistent maken van de resultaten van de volgende validatiecontroles wanneer uitvoert op dezelfde computer.  Bijvoorbeeld, kan het hulpprogramma worden uitgevoerd voor het valideren van certificaten, opnieuw uitvoeren om Azure identiteit te valideren en vervolgens een derde tijd voor de registratie valideert. De resultaten van alle drie validaties zijn beschikbaar in het resulterende .json-rapport.  

Standaard worden beide bestanden geschreven naar *C:\Users\<username > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Gebruik de **- OutputPath** ***&lt;pad&gt;*** parameter aan het einde van de uitgevoerde opdrachtregel op te geven van een andere locatie.   
- Gebruik de **- CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen van gegevens uit *AzsReadinessCheckerReport.json*. over vorige van het hulpprogramma kan worden uitgevoerd.

## <a name="view-the-report"></a>Het rapport weergeven
Geef voor het rapport weergeven in PowerShell, het pad naar het rapport als een waarde voor **- ReportPath**. Met deze opdracht wordt de inhoud van het rapport en identificeert ook validaties er nog geen resultaten.

Bijvoorbeeld: als u wilt weergeven in het rapport vanuit een PowerShell-prompt die is geopend op de locatie van het rapport, uitvoeren: 
   > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json` 

De uitvoer lijkt op de volgende afbeelding:

![rapport weergeven](./media/azure-stack-validation-report/view-report.png)

## <a name="view-the-report-summary"></a>Het rapport wordt een samenvatting weergeven
U kunt toevoegen om weer te geven een overzicht van het rapport, de **-samenvatting** overschakelen naar het einde van de PowerShell-opdrachtregel. Bijvoorbeeld: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -summary`  

De samenvatting toont validaties waarvoor geen resultaten en aangegeven gehaald of mislukken voor controles die voltooid zijn. De uitvoer lijkt op de volgende afbeelding:

![rapport-overzicht](./media/azure-stack-validation-report/report-summary.png)


## <a name="view-a-filtered-report"></a>Een gefilterde rapport weergeven
Als u een rapport gefilterd op één type validatie, gebruiken de **- ReportSections** parameter en geeft u een van de volgende waarden die overeenkomen met het validatietype die u wilt weergeven:
- Certificaat
- AzureRegistration
- AzureIdentity
- Taken   
- Alle  

Bijvoorbeeld, als het rapport wilt weergeven samenvatting voor certificaten, gebruik alleen de volgende PowerShell-opdrachtregel: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Zie ook
[Start AzsReadinessChecker cmdlet-verwijzing](azure-stack-azsreadiness-cmdlet.md)
