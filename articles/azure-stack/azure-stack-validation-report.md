---
title: Validatierapport voor Azure Stack | Microsoft Docs
description: Het rapport Azure Stack-gereedheid Checker gebruiken om te bekijken van resultaten.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 10/23/2018
ms.openlocfilehash: b51f88aae20506e70c3a7bd1c199d032d98fd6fa
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57774758"
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
   > `Read-AzsReadinessReport -ReportPath .\AzsReadinessReport.json` 

De uitvoer lijkt op het volgende:

```PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

Certificate Validation results not available.

############### Registration Validation Summary ###############

Azure Registration Validation results not available.

############### Azure Identity Results ###############

Test                          : ServiceAdministrator
Result                        : OK
AAD Service Admin             : admin@contoso.onmicrosoft.com
Azure Environment             : AzureCloud
Azure Active Directory Tenant : contoso.onmicrosoft.com
Error Details                 : 

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.

############### AzsReadiness Job Summary ###############

Index             : 0
Operations        : 
StartTime         : 2018/10/22 14:24:16
EndTime           : 2018/10/22 14:24:19
Duration          : 3
PSBoundParameters : 
```

## <a name="view-the-report-summary"></a>Het rapport wordt een samenvatting weergeven
Als u een overzicht van het rapport, kunt u toevoegen de **-samenvatting** overschakelen naar het einde van de PowerShell-opdrachtregel. Bijvoorbeeld: 
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -summary`  

De samenvatting validaties waarvoor geen resultaten bevat en wordt aangegeven geslaagd of niet voor validaties die voltooid zijn. De uitvoer lijkt op het volgende:

```PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

    Certificate Validation found no errors or warnings.
    
############### Registration Validation Summary ###############

    Registration Validation found no errors or warnings.

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.
```


## <a name="view-a-filtered-report"></a>Een gefilterd rapport weergeven
Als u wilt een rapport dat wordt gefilterd op één type validatie weergeven, gebruikt u de **- ReportSections** parameter met een van de volgende waarden:
- Certificaat
- AzureRegistration
- AzureIdentity
- Graph
- ADFS
- Taken   
- Alle  

Bijvoorbeeld, om het rapport weer te geven samenvatting voor certificaten alleen, gebruik de volgende PowerShell-opdrachtregel: 
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Zie ook
