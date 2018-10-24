---
title: Valideren van de Azure Graph-integratie voor Azure Stack
description: Gebruik de Azure Stack-gereedheid van de vereisten voor het valideren van de graph-integratie voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: patricka
ms.reviewer: jerskine
ms.openlocfilehash: e1c1ba0a065a20874bf51d7464cbcfdfa13a571d
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947211"
---
# <a name="validate-graph-integration-for-azure-stack"></a>Valideren van de graph-integratie voor Azure Stack

Gebruik het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) om te valideren dat uw omgeving gereed voor graph-integratie met Azure Stack is. Voordat u data center-integratie of voor een Azure Stack-implementatie moet u de graph-integratie valideren.

De gereedheid van vereisten wordt gevalideerd:

* De referenties voor de serviceaccount voor graph-integratie gemaakt hebt de juiste rechten in Active Directory.
* De *globale catalogus* kunnen worden opgelost en beschikbaar is.
* Het KDC kan worden opgelost en beschikbaar is.
* Vereiste netwerkverbinding is voldaan.

Zie voor meer informatie over de integratie van Azure Stack data center [integratie van Azure Stack datacenter - identiteit](azure-stack-integrate-identity.md)

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen

Download de nieuwste versie van het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) van de [PSGallery](https://aka.ms/AzsReadinessChecker).

## <a name="prerequisites"></a>Vereisten

De volgende vereisten moeten worden voldaan.

**De computer waarop het hulpprogramma wordt uitgevoerd:**

* Windows 10 of Windows Server 2016 met de domeinconnectiviteit.
* PowerShell 5.1 of hoger. Voer de volgende PowerShell-opdracht om te controleren of uw versie, en bekijk vervolgens de *belangrijke* versie en *kleine* versies:  
   > `$PSVersionTable.PSVersion`
* Active Directory PowerShell-module
* De nieuwste versie van de [Microsoft Azure Stack-gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma

**Active Directory-omgeving:**

* Identificeren van de gebruikersnaam en het wachtwoord voor een account voor de graph-service in de bestaande Active Directory
* Identificeren van het foresthoofddomein Active Directory FQDN-naam

## <a name="validate-graph"></a>Valideren van grafiek

1. Op een computer die voldoet aan de vereisten, open een beheeropdrachtprompt PowerShell en voer de volgende opdracht voor het installeren van de AzsReadinessChecker.

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. Voer de volgende om in te stellen van de PowerShell-prompt *$graphCredential* variabele aan het account voor graph. Vervang `contoso\graphservice` met uw account met de `domain\username` indeling.

    `$graphCredential = Get-Credential contoso\graphservice -Message "Enter Credentials for the Graph Service Account"`

1. Voer de volgende voor het starten van de validatie voor graph vanuit de PowerShell-prompt. Geef de waarde voor **- ForestFQDN** als de FQDN-naam voor de hoofdforest:

     `Invoke-AzsGraphValidation -ForestFQDN contoso.com -Credential $graphCredential`

1. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer of de status OK voor vereisten voor integratie van grafiek. Een succesvolle validatie wordt de volgende strekking weergegeven:

    ```
    Testing Graph Integration (v1.0)
            Test Forest Root:            OK
            Test Graph Credential:       OK
            Test Global Catalog:         OK
            Test KDC:                    OK
            Test LDAP Search:            OK
            Test Network Connectivity:   OK

    Details:

    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.

    Additional help URL: https://aka.ms/AzsGraphIntegration

    AzsReadinessChecker Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log

    AzsReadinessChecker Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsGraphValidation Completed
    ```

In een productieomgeving, testen van verbinding met het netwerk van een werkstation met operators kan niet worden beschouwd als volledig indicatief voor de connectiviteit die beschikbaar zijn voor Azure Stack. De Azure Stack-stempel openbare VIP-netwerk moet de connectiviteit voor LDAP-verkeer om uit te voeren identiteitintegratie.

## <a name="report-and-log-file"></a>Rapport en een logboekbestand

Elke validatie wordt uitgevoerd, resultaten van deze vereistencontrole **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden wordt weergegeven met de validatieresultaten in PowerShell.

De validatie-bestanden kunt u delen van de status voordat u Azure Stack implementeren of validatieproblemen onderzoeken. Beide bestanden behouden de resultaten van elke volgende validatiecontrole. Het rapport bevat de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kan helpen bij uw implementatie of ondersteuning team validatieproblemen onderzoeken.

Standaard worden beide bestanden naar geschreven. `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`

U kunt gebruiken:

* **-OutputPath** *pad* parameter aan het einde van het uitvoeren vanaf de opdrachtregel om op te geven van een andere locatie.
* **-CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen *AzsReadinessCheckerReport.json* van vorige rapportgegevens. Zie voor meer informatie, [Azure Stack-validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Validatiefouten

Als een validatiecontrole mislukt, wordt informatie over de fout weergegeven in het PowerShell-venster. Het hulpprogramma registreert ook informatie naar de *AzsGraphIntegration.log*.

## <a name="next-steps"></a>Volgende stappen

[Het rapport gereedheid weergeven](azure-stack-validation-report.md)  
[Aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)  
