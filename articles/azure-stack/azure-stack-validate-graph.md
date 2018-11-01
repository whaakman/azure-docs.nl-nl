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
ms.openlocfilehash: 43f30989fa09e711fc71941e7722dcd195212472
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416228"
---
# <a name="validate-graph-integration-for-azure-stack"></a>Valideren van de graph-integratie voor Azure Stack

Gebruik het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) om te valideren dat uw omgeving gereed voor graph-integratie met Azure Stack is. Graph-integratie valideren voordat u begint met datacenter-integratie of vóór de implementatie van een Azure Stack.

De gereedheid van vereisten wordt gevalideerd:

* De referenties voor de serviceaccount voor graph-integratie gemaakt hebt de juiste rechten in Active Directory.
* De *globale catalogus* kunnen worden opgelost en beschikbaar is.
* Het KDC kan worden opgelost en beschikbaar is.
* Vereiste netwerkverbinding is voldaan.

Zie voor meer informatie over de integratie van Azure Stack-datacenter [integratie van Azure Stack datacenter - identiteit](azure-stack-integrate-identity.md).

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen

Download de nieuwste versie van het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) van de [PowerShell Gallery](https://aka.ms/AzsReadinessChecker).

## <a name="prerequisites"></a>Vereisten

De volgende vereisten moeten worden voldaan.

**De computer waarop het hulpprogramma wordt uitgevoerd:**

* Windows 10 of Windows Server 2016 met de domeinconnectiviteit.
* PowerShell 5.1 of hoger. Voer de volgende PowerShell-opdracht om te controleren of uw versie, en bekijk de *belangrijke* versie en *kleine* versies:  
   > `$PSVersionTable.PSVersion`
* Active Directory PowerShell-module.
* Meest recente versie van de [Microsoft Azure Stack-gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.

**Active Directory-omgeving:**

* Identificeer de gebruikersnaam en het wachtwoord voor een account voor de graph-service in de bestaande Active Directory-instantie.
* Identificeer de hoofdforest Active Directory FQDN-naam.

## <a name="validate-the-graph-service"></a>Valideren van de graph-service

1. Open een PowerShell-prompt met beheerdersrechten op een computer die voldoet aan de vereisten, en voer de volgende opdracht voor het installeren van de AzsReadinessChecker:

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. Uitvoeren van de PowerShell-prompt de volgende opdracht uit om de *$graphCredential* variabele aan het account voor graph. Vervang `contoso\graphservice` met uw account met behulp van de `domain\username` indeling.

    `$graphCredential = Get-Credential contoso\graphservice -Message "Enter Credentials for the Graph Service Account"`

1. Voer de volgende opdracht om te valideren voor de graph-service starten vanuit de PowerShell-prompt. Geef de waarde voor **- ForestFQDN** als de FQDN-naam voor het foresthoofddomein.

     `Invoke-AzsGraphValidation -ForestFQDN contoso.com -Credential $graphCredential`

1. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer of de status OK voor vereisten voor integratie van grafiek. Een succesvolle validatie is vergelijkbaar met het volgende voorbeeld:

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

In een productieomgeving, testen van verbinding met het netwerk via de operator is niet volledig indicatief voor de connectiviteit die beschikbaar zijn voor Azure Stack. De Azure Stack-stempel openbare VIP-netwerk moet de connectiviteit voor LDAP-verkeer om uit te voeren identiteitintegratie.

## <a name="report-and-log-file"></a>Rapport en een logboekbestand

Elke validatie wordt uitgevoerd, resultaten van deze vereistencontrole **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden wordt weergegeven met de resultaten in PowerShell.

De validatie-bestanden kunt u delen van de status voordat u Azure Stack implementeren of validatieproblemen onderzoeken. Beide bestanden behouden de resultaten van elke volgende validatiecontrole. Het rapport geeft de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kan helpen bij uw implementatie of ondersteuning team validatieproblemen onderzoeken.

Standaard beide bestanden worden geschreven naar `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

U kunt gebruiken:

* **-OutputPath**: de *pad* parameter aan het einde van de opdracht uitvoeren om op te geven van een andere locatie.
* **-CleanReport**: de parameter aan het einde van de opdracht uitvoeren om te wissen *AzsReadinessCheckerReport.json* van vorige rapportgegevens. Zie voor meer informatie, [Azure Stack-validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Validatiefouten

Als een validatiecontrole mislukt, wordt informatie over de fout in het PowerShell-venster weergegeven. Het hulpprogramma registreert ook informatie *AzsGraphIntegration.log*.

## <a name="next-steps"></a>Volgende stappen

[Het rapport gereedheid weergeven](azure-stack-validation-report.md)  
[Aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)  
