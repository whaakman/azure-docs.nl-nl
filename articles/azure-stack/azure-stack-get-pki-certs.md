---
title: Azure Stack Public Key Infrastructure-certificaten voor de implementatie van de geïntegreerde Azure Stack-systemen genereren | Microsoft Docs
description: Beschrijft het proces van de Azure Stack PKI-certificaat voor geïntegreerde Azure Stack-systemen.
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
ms.date: 09/05/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 698e044aea6bbd78847cb209160c1fa6b2edcdbf
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023416"
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Azure Stack-certificaten genereren van de aanvraag ondertekenen

Azure Stack gereedheid van de Registercontrole die worden beschreven in dit artikel vindt u [vanuit de PowerShell Gallery](https://aka.ms/AzsReadinessChecker). Het hulpprogramma maakt ondertekening certificaataanvragen (CSR) geschikt is voor een Azure Stack-implementatie. Certificaten moeten worden aangevraagd, gegenereerd en gevalideerd met voldoende tijd om te testen vóór de implementatie.

Het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) voert de volgende certificaataanvragen uit:

 - **Standaard-certificaataanvragen**  
    Aanvraag volgens [PKI-certificaten genereren voor implementatie van Azure Stack](azure-stack-get-pki-certs.md).
 - **Platform-as-a-Service**  
    (Optioneel) aanvragen platform-as-a-service (PaaS) namen voor certificaten die zijn opgegeven in [certificaatvereisten voor Azure Stack Public Key Infrastructure - optionele PaaS certificaten](azure-stack-pki-certs.md#optional-paas-certificates).



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

1.  AzsReadinessChecker installeren vanuit een PowerShell-prompt (5.1 of hoger), door de volgende cmdlet:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Declareer de **onderwerp** als een geordende woordenlijst. Bijvoorbeeld: 

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Als een algemene naam (CN) is opgegeven. deze wordt overschreven door de eerste DNS-naam van de certificaataanvraag.

3.  Declareer een map met de uitvoer die er al bestaat. Bijvoorbeeld:

    ````PowerShell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ````
4.  Declareer system identificeren

    Azure Active Directory

    ```PowerShell
    $IdentitySystem = "AAD"
    ````

    Active Directory Federation Services

    ```PowerShell
    $IdentitySystem = "ADFS"
    ````

5. Declareer **regionaam** en een **externe FQDN** die bestemd zijn voor de Azure Stack-implementatie.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` vormt de basis waarop alle externe DNS-namen in Azure Stack zijn gemaakt, in dit voorbeeld is, is de portal `portal.east.azurestack.contoso.com`.  

6. Aanvragen voor elke DNS-naam voor Certificaatondertekening genereren:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Als u wilt opnemen opgeven PaaS-Services de switch ```-IncludePaaS```

7. U kunt ook voor Dev/Test-omgevingen. Voor het genereren van een enkele aanvraag met meerdere onderwerp alternatieve namen toevoegen **- RequestType SingleCSR** parameter en de waarde (**niet** aanbevolen voor productie-omgevingen):

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Als u wilt opnemen opgeven PaaS-Services de switch ```-IncludePaaS```
    
8. Controleer de uitvoer:

    ````PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Request Generation

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com*dn2=*.adminhosting.east.azurestack.contoso.com@dns=*.hosting.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Finished Certificate Request Generation

    AzsReadinessChecker Log location: C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\1.1803.405.3\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

9.  Dien de **. Door de No** bestand gegenereerd op de Certificeringsinstantie (intern of openbaar).  De uitvoermap van **Start AzsReadinessChecker** bevat de CSR(s) die nodig zijn bij een certificeringsinstantie in te dienen.  Het bevat ook een onderliggende map met de INF-bestanden die tijdens de aanvraag genereren van het certificaat, als uitgangspunt gebruikt. Zorg dat uw CA genereert met behulp van uw verzoek gegenereerde certificaten die voldoen aan de [vereisten voor Azure Stack PKI](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Volgende stappen

[Bereid Azure Stack PKI-certificaten](azure-stack-prepare-pki-certs.md)
