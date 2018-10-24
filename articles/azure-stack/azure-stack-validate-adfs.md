---
title: Valideren van de AD FS-integratie voor Azure Stack
description: Gebruik de Azure Stack-gereedheid van de vereisten voor het valideren van de AD FS-integratie voor Azure Stack.
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
ms.openlocfilehash: 0aa13f2fced9122163d5278b5bb50cc1e11a0379
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947214"
---
# <a name="validate-adfs-integration-for-azure-stack"></a>Valideren van de AD FS-integratie voor Azure Stack

Gebruik het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) om te valideren dat uw omgeving gereed voor AD FS-integratie met Azure Stack is. Voordat u data center-integratie of vóór de implementatie van een Azure Stack, moet u AD FS-integratie valideren.

De gereedheid van vereisten wordt gevalideerd:

* De *federatiemetagegevens* bevat de geldige XML-elementen voor Federatie.
* De *ADFS-SSL-certificaat* kunnen worden opgehaald en vertrouwensketen kan worden gebouwd. AD FS moeten de SSL-certificaten-keten vertrouwen op het stempel. Het certificaat moet worden ondertekend door dezelfde *certificeringsinstantie* als de Azure Stack-implementatie van certificaten of door een vertrouwde instantie partner. Zie voor de volledige lijst van vertrouwde basis-CA-partners: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca
* *AD FS-handtekeningcertificaat* vertrouwde en niet die bijna verlopen is.

Zie voor meer informatie over de integratie van Azure Stack data center [integratie van Azure Stack datacenter - identiteit](azure-stack-integrate-identity.md)

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen

Download de nieuwste versie van het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) van de [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Vereisten

De volgende vereisten moeten worden voldaan.

**De computer waarop het hulpprogramma wordt uitgevoerd:**

* Windows 10 of Windows Server 2016 met de domeinconnectiviteit.
* PowerShell 5.1 of hoger. Voer de volgende PowerShell-opdracht om te controleren of uw versie, en bekijk vervolgens de *belangrijke* versie en *kleine* versies:  
   > `$PSVersionTable.PSVersion`
* De nieuwste versie van de [Microsoft Azure Stack-gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.

**Active Directory Federation Services-omgeving:**

U moet ten minste één van de volgende formulieren van de metagegevens van:

* De URL voor AD FS-federatiemetagegevens. Bijvoorbeeld: `https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
* De federatieve metagegevens van XML-bestand. Bijvoorbeeld, FederationMetadata.xml

## <a name="validate-adfs-integration"></a>Valideren van de AD FS-integratie

1. Op een computer die voldoet aan de vereisten, open een beheeropdrachtprompt PowerShell en voer de volgende opdracht voor het installeren van de AzsReadinessChecker.

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. Voer de volgende voor het starten van validatie van de PowerShell-prompt. Geef de waarde voor **- CustomADFSFederationMetadataEndpointUri** als de URI voor de federatiemetagegevens:

     `Invoke-AzsADFSValidation -CustomADFSFederationMetadataEndpointUri https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

1. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer of de status OK voor AD FS-vereisten voor integratie. Een succesvolle validatie wordt de volgende strekking weergegeven.

    ```
    Invoke-AzsADFSValidation v1.1809.1001.1 started.

    Testing ADFS Endpoint https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

            Read Metadata:                         OK
            Test Metadata Elements:                OK
            Test SSL ADFS Certificate:             OK
            Test Certificate Chain:                OK
            Test Certificate Expiry:               OK

    Details:
    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.
    Additional help URL: https://aka.ms/AzsADFSIntegration

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsADFSValidation Completed
    ```

In een productieomgeving, certificaatketens van de vertrouwensrelatie van een werkstation met operators testen kan niet worden beschouwd als volledig indicatief voor de PKI-vertrouwensrelatie houding in de Azure Stack-infrastructuur. De Azure Stack-stempel openbare VIP-netwerk heeft de verbinding met de CRL voor de PKI-infrastructuur nodig.

## <a name="report-and-log-file"></a>Rapport en een logboekbestand

Elke validatie wordt uitgevoerd, resultaten van deze vereistencontrole **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden wordt weergegeven met de validatieresultaten in PowerShell.

De validatie-bestanden kunt u delen van de status voordat u Azure Stack implementeren of validatieproblemen onderzoeken. Beide bestanden behouden de resultaten van elke volgende validatiecontrole. Het rapport bevat de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kan helpen bij uw implementatie of ondersteuning team validatieproblemen onderzoeken.

Standaard beide bestanden worden geschreven naar `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

U kunt gebruiken:

* **-OutputPath** *pad* parameter aan het einde van het uitvoeren vanaf de opdrachtregel om op te geven van een andere locatie.
* **-CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen *AzsReadinessCheckerReport.json* van vorige rapportgegevens. Zie voor meer informatie, [Azure Stack-validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Validatiefouten

Als een validatiecontrole mislukt, wordt informatie over de fout weergegeven in het PowerShell-venster. Het hulpprogramma registreert ook informatie naar de *AzsReadinessChecker.log*.

De volgende voorbeelden geven richtlijnen op algemene validatiefouten.

### <a name="command-not-found"></a>Kan de opdracht niet vinden

`Invoke-AzsADFSValidation : The term 'Invoke-AzsADFSValidation' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.`

**Oorzaak** -PowerShell Autoload Checker van de gereedheid van de module correct laden is mislukt.

**Resolutie** -module importeren gereedheid Checker expliciet. Kopieer en plak de code hieronder in PowerShell en update de \<versie\> met het versienummer voor de momenteel geïnstalleerde versie.

`Import-Module "c:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\<version>\Microsoft.AzureStack.ReadinessChecker.psd1" -Force`

## <a name="next-steps"></a>Volgende stappen

[Het rapport gereedheid weergeven](azure-stack-validation-report.md)  
[Aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)  
