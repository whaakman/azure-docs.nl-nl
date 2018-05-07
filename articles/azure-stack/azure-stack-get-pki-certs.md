---
title: Genereren van certificaten voor de implementatie van Azure-Stack geïntegreerd systemen Azure Stack Public Key Infrastructure | Microsoft Docs
description: Beschrijving van het implementatieproces van Azure Stack PKI-certificaat voor Azure-Stack geïntegreerd systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: b65d0d88fd57dea59c79d2f72bab60967856e015
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Azure Stack certificaten ondertekening aanvraag genereren

Azure-Stack gereedheid van de Registercontrole beschreven in dit artikel vindt u [van de PowerShell Gallery](https://aka.ms/AzsReadinessChecker). Het hulpprogramma maakt ondertekening certificaataanvragen (CSR) geschikt is voor de implementatie van een Azure-Stack. Certificaten moeten worden aangevraagd, gegenereerd en gevalideerd met voldoende tijd voor het testen van vóór de implementatie.

Het hulpprogramma Azure Stack gereedheid Checker (AzsReadinessChecker) voert de volgende certificaataanvragen:

 - **Standaard-certificaataanvragen**  
    Aanvraag volgens [PKI-certificaten genereren voor implementatie van de Azure-Stack](azure-stack-get-pki-certs.md).
 - **Aanvraagtype**  
    Geeft aan of de aanvraag voor Certificaatondertekening één aanvraag of meerdere aanvragen.
 - **Platform as a Service**  
    Eventueel aanvragen platform as a service (PaaS) namen voor de certificaten die zijn opgegeven in [certificaatvereisten Azure Stack Public Key Infrastructure - optionele PaaS certificaten](azure-stack-pki-certs.md#optional-paas-certificates).

## <a name="prerequisites"></a>Vereisten

Uw systeem aan de volgende vereisten voldoen voordat u de CSR(s) voor PKI-certificaten voor de implementatie van een Azure-Stack genereert:

 - Microsoft Azure-Stack gereedheid Checker
 - Certificaat-kenmerken:
    - Regionaam
    - Externe volledig gekwalificeerde domeinnaam (FQDN)
    - Onderwerp
 - Windows 10 of Windows Server 2016
 
  > [!NOTE]
  > Wanneer u ontvangt uw certificaten opnieuw in via uw certificeringsinstantie de stappen in [voorbereiden Azure Stack PKI-certificaten](azure-stack-prepare-pki-certs.md) moet worden voltooid op hetzelfde systeem!

## <a name="generate-certificate-signing-requests"></a>Aanvragen voor Certificaatondertekening genereren

Volg deze stappen voor het voorbereiden en valideren van de Azure-Stack PKI-certificaten: 

1.  AzsReadinessChecker installeren vanaf een PowerShell-prompt (5.1 of hoger), door de volgende cmdlet:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Declareer de **onderwerp** als een geordende woordenlijst. Bijvoorbeeld: 

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Als een algemene naam (CN) worden opgegeven. Dit zal worden overschreven door de eerste DNS-naam van de certificaataanvraag.

3.  Declareer een uitvoermap die al bestaat:

    ````PowerShell  
    $outputDirectory = "$ENV:USERNAME\Documents\AzureStackCSR" 
    ````
4.  Declareren system identificeren

    Azure Active Directory

    ```PowerShell
    $IdentitySystem = "AAD"
    ````

    Active Directory Federation Services

    ```PowerShell
    $IdentitySystem = "ADFS"
    ````

5. Declareren **regionaam** en een **externe FQDN** bedoeld voor de implementatie van Azure-Stack.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` vormt de basis waarop alle externe DNS-namen in Azure-Stack zijn gemaakt, in dit voorbeeld, kunnen de portal zou `portal.east.azurestack.contoso.com`.

6. Voor het genereren van een aanvraag wordt één certificaat met meerdere alternatieve onderwerpnamen:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Als u wilt opnemen opgeven PaaS-Services de switch ```-IncludePaaS```

7. Voor het genereren van afzonderlijke Certificaatondertekening aanvragen voor elke DNS-naam:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType MultipleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Als u wilt opnemen opgeven PaaS-Services de switch ```-IncludePaaS```

8. Controleer de uitvoer:

    ````PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Request Generation

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Finished Certificate Request Generation

    AzsReadinessChecker Log location: C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\1.1803.405.3\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

9.  Verzenden van de **. Door de No** gegenereerde aan uw CA (intern of openbaar)-bestand.  De uitvoermap van **Start AzsReadinessChecker** bevat de CSR(s) in te dienen bij een certificeringsinstantie nodig.  Het bevat ook een onderliggende map met de INF-bestanden tijdens het genereren van certificaat aanvraag, als referentie gebruikt. Zorg dat uw CA-certificaten met behulp van de gegenereerde aanvraag die voldoen aan genereert de [PKI-vereisten voor Azure-Stack](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Volgende stappen

[Bereid Azure Stack PKI-certificaten](azure-stack-prepare-pki-certs.md)
