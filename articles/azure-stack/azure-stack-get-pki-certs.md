---
title: Azure Stack Public Key Infrastructure-certificaten voor de implementatie van de geïntegreerde Azure Stack-systemen genereren | Microsoft Docs
description: Beschrijft het proces van de Azure Stack PKI-certificaat voor geïntegreerde Azure Stack-systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: 9d358c021f795172e7ced0ba2a2f309a0a0dab6e
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56649724"
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Azure Stack-certificaten genereren van de aanvraag ondertekenen

Het hulpprogramma Azure Stack-gereedheid van de Registercontrole kunt u ondertekening certificaataanvragen (CSR) maken die geschikt zijn voor de implementatie van een Azure Stack. Certificaten moeten worden aangevraagd, gegenereerd en gevalideerd met voldoende tijd om te testen vóór de implementatie. U kunt het hulpprogramma [vanuit de PowerShell Gallery](https://aka.ms/AzsReadinessChecker).

U kunt het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) gebruiken om aan te vragen van de volgende certificaten:

- **Standard certificaataanvragen** volgens [PKI-certificaten genereren voor implementatie van Azure Stack](azure-stack-get-pki-certs.md).
- **Platform-as-a-Service**: U kunt de namen van de platform-as-a-service (PaaS) voor certificaten die zijn opgegeven in aanvragen [certificaatvereisten voor Azure Stack Public Key Infrastructure - optionele PaaS certificaten](azure-stack-pki-certs.md#optional-paas-certificates).

## <a name="prerequisites"></a>Vereisten

Uw systeem moet voldoen aan de volgende vereisten vóór het genereren van de CSR(s) voor PKI-certificaten voor de implementatie van een Azure Stack:

- Microsoft Azure Stack-gereedheid Checker
- Certificaatkenmerken van:
  - Regionaam
  - Externe volledig gekwalificeerde domeinnaam (FQDN)
  - Onderwerp
- Windows 10 of Windows Server 2016

  > [!NOTE]  
  > Wanneer u ontvangt uw certificaten back-ups maken van uw certificeringsinstantie de stappen in [voorbereiden Azure Stack PKI-certificaten](azure-stack-prepare-pki-certs.md) moet worden uitgevoerd op hetzelfde systeem!

## <a name="generate-certificate-signing-requests"></a>Aanvragen voor Certificaatondertekening genereren

Volg deze stappen voor het voorbereiden en valideren van de Azure Stack PKI-certificaten:

1. AzsReadinessChecker installeren vanuit een PowerShell-prompt (5.1 of hoger), door de volgende cmdlet:

    ```PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ```

2. Declareer de **onderwerp** als een geordende woordenlijst. Bijvoorbeeld:

    ```PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
    ```

    > [!note]  
    > Als een algemene naam (CN) is opgegeven. deze wordt overschreven door de eerste DNS-naam van de certificaataanvraag.

3. Declareer een map met de uitvoer die er al bestaat. Bijvoorbeeld:

    ```PowerShell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ```

4. Identiteitssysteem declareren

    Azure Active Directory

    ```PowerShell
    $IdentitySystem = "AAD"
    ```

    Active Directory Federation Services

    ```PowerShell
    $IdentitySystem = "ADFS"
    ```

5. Declareer **regionaam** en een **externe FQDN** die bestemd zijn voor de Azure Stack-implementatie.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ```

    > [!note]  
    > `<regionName>.<externalFQDN>` vormt de basis waarop alle externe DNS-namen in Azure Stack zijn gemaakt, in dit voorbeeld is, is de portal `portal.east.azurestack.contoso.com`.  

6. Aanvragen voor elke DNS-naam voor Certificaatondertekening genereren:

    ```PowerShell  
    New-AzsCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    Als u wilt opnemen PaaS-Services, geef de switch ```-IncludePaaS```

7. U kunt ook voor Dev/Test-omgevingen voor het genereren van een enkele aanvraag met meerdere alternatieve namen voor onderwerpen toevoegen **- RequestType SingleCSR** parameter en de waarde (**niet** aanbevolen voor productie-omgevingen):

    ```PowerShell  
    New-AzsCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    Als u wilt opnemen PaaS-Services, geef de switch ```-IncludePaaS```

8. Controleer de uitvoer:

    ```PowerShell  
    New-AzsCertificateSigningRequest v1.1809.1005.1 started.

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com*dn2=*.adminhosting.east.azurestack.contoso.com@dns=*.hosting.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    New-AzsCertificateSigningRequest Completed
    ```

9. Dien de **. Door de No** bestand gegenereerd op de Certificeringsinstantie (intern of openbaar).  De uitvoermap van **New-AzsCertificateSigningRequest** bevat de CSR(s) die nodig zijn bij een certificeringsinstantie in te dienen.  De map bevat ook, ter referentie, een onderliggende map met de INF-bestanden die tijdens het genereren van de aanvraag certificaat wordt gebruikt. Zorg dat uw CA genereert met behulp van uw verzoek gegenereerde certificaten die voldoen aan de [vereisten voor Azure Stack PKI](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Volgende stappen

[Bereid Azure Stack PKI-certificaten](azure-stack-prepare-pki-certs.md)
