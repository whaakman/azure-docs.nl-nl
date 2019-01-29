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
ms.date: 01/28/2019
ms.author: patricka
ms.reviewer: jerskine
ms.openlocfilehash: 7e6c54add856a69e1750b0b6ca0a058c2d80bfd8
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55101725"
---
# <a name="validate-ad-fs-integration-for-azure-stack"></a>Valideren van de AD FS-integratie voor Azure Stack

Gebruik het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) om te valideren dat uw omgeving gereed voor Active Directory Federation Services (AD FS)-integratie met Azure Stack is. Integratie van AD FS valideren voordat u begint met datacenter-integratie of vóór de implementatie van een Azure Stack.

De gereedheid van vereisten wordt gevalideerd:

* De *federatiemetagegevens* bevat de geldige XML-elementen voor Federatie.
* De *AD FS SSL-certificaat* kan worden opgehaald, en een keten van vertrouwen kan worden gebouwd. AD FS moeten de SSL-certificaat-keten vertrouwen op het stempel. Het certificaat moet worden ondertekend door dezelfde *certificeringsinstantie* gebruikt voor de Azure Stack-implementatie van certificaten of door een vertrouwde instantie partner. Zie voor de volledige lijst van vertrouwde basis-CA-partners, [TechNet](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).
* De *handtekeningcertificaat van de AD FS* vertrouwde en niet die bijna verlopen is.

Zie voor meer informatie over de integratie van Azure Stack-datacenter [integratie van Azure Stack datacenter - identiteit](azure-stack-integrate-identity.md).

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen

Download de nieuwste versie van het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) van de [PowerShell Gallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Vereisten

De volgende vereisten moeten worden voldaan.

**De computer waarop het hulpprogramma wordt uitgevoerd:**

* Windows 10 of Windows Server 2016 met de domeinconnectiviteit.
* PowerShell 5.1 of hoger. Voer de volgende PowerShell-opdracht om te controleren of uw versie, en bekijk de *belangrijke* versie en *kleine* versies:  
   > `$PSVersionTable.PSVersion`
* Meest recente versie van de [Microsoft Azure Stack-gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.

**Active Directory Federation Services-omgeving:**

U moet ten minste één van de volgende formulieren van de metagegevens van:

* De URL voor AD FS-federatiemetagegevens. Een voorbeeld is `https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`.
* De federatieve metagegevens van XML-bestand. Een voorbeeld is FederationMetadata.xml.

## <a name="validate-ad-fs-integration"></a>Valideren van de AD FS-integratie

1. Open een PowerShell-prompt met beheerdersrechten op een computer die voldoet aan de vereisten, en voer de volgende opdracht voor het installeren van AzsReadinessChecker:

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. Voer de volgende opdracht om te beginnen de validatie van de PowerShell-prompt. Geef de waarde voor **- CustomADFSFederationMetadataEndpointUri** als de URI voor de federatiemetagegevens.

     `Invoke-AzsADFSValidation -CustomADFSFederationMetadataEndpointUri https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

1. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer of de status OK voor AD FS-vereisten voor integratie. Een succesvolle validatie is vergelijkbaar met het volgende voorbeeld:

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

In een productieomgeving, certificaatketens vertrouwensniveau van van een operator werkstation testen is niet volledig indicatief voor de PKI-vertrouwensrelatie houding in de Azure Stack-infrastructuur. De Azure Stack-stempel openbare VIP-netwerk heeft de verbinding met de CRL voor de PKI-infrastructuur nodig.

## <a name="report-and-log-file"></a>Rapport en een logboekbestand

Elke validatie wordt uitgevoerd, resultaten van deze vereistencontrole **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden wordt weergegeven met de resultaten in PowerShell.

De validatie-bestanden kunt u delen van de status voordat u Azure Stack implementeren of validatieproblemen onderzoeken. Beide bestanden behouden de resultaten van elke volgende validatiecontrole. Het rapport geeft de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kan helpen bij uw implementatie of ondersteuning team validatieproblemen onderzoeken.

Standaard beide bestanden worden geschreven naar `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

U kunt gebruiken:

* **-OutputPath**: De *pad* parameter aan het einde van de opdracht uitvoeren om op te geven van een andere locatie.
* **-CleanReport**: De parameter die aan het einde van de opdracht uitvoeren om te wissen AzsReadinessCheckerReport.json van vorige rapportgegevens. Zie voor meer informatie, [Azure Stack-validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Validatiefouten

Als een validatiecontrole mislukt, wordt informatie over de fout in het PowerShell-venster weergegeven. Het hulpprogramma registreert ook informatie *AzsReadinessChecker.log*.

De volgende voorbeelden geven richtlijnen op algemene validatiefouten.

### <a name="command-not-found"></a>Kan de opdracht niet vinden

`Invoke-AzsADFSValidation : The term 'Invoke-AzsADFSValidation' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.`

**Oorzaak**: PowerShell Autoload kan niet correct laden van de gereedheid van de controle-module.

**Oplossing**: De module gereedheid Checker expliciet importeren. Kopieer en plak de volgende code in PowerShell en update \<versie\> met het nummer voor de huidige geïnstalleerde versie.

`Import-Module "c:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\<version>\Microsoft.AzureStack.ReadinessChecker.psd1" -Force`

## <a name="next-steps"></a>Volgende stappen

[Het rapport gereedheid weergeven](azure-stack-validation-report.md)  
[Aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)  
