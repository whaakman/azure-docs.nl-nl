---
title: Werkstroom algemene parameters in de Azure Stack-validatie als een Service | Microsoft Docs
description: Werkstroom algemene parameters voor Azure Stack-validatie als een Service
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/19/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 9802184ebef764e9e493b8477fcca72e308987d0
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337223"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Werkstroom algemene parameters voor Azure Stack-validatie als een Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Algemene parameters bevatten waarden zoals omgevingsvariabelen en de gebruiker referenties vereist voor alle tests in de validatie als een Service (VaaS). Deze waarden worden gedefinieerd op het werkstroomniveau van de bij het maken of wijzigen van een werkstroom. Bij het plannen van tests, worden deze waarden als parameters doorgegeven aan elke test in de werkstroom.

> [!NOTE]
> Elke test een eigen set parameters gedefinieerd. Op tijd plannen, een test mogelijk wanneer u een waarde onafhankelijk van de algemene parameters in te voeren, of kunt u de waarde van de algemene parameter overschrijven.

## <a name="environment-parameters"></a>Omgeving parameters

Omgeving parameters beschrijven de Azure Stack-omgeving onder belasting. Deze waarden moeten worden opgegeven door te genereren en uploaden van een Azure Stack stempel bestand voor het specifieke exemplaar dat u wilt testen.

> [!NOTE]
> In de officiële validatie werkstromen, worden niet omgeving parameters gewijzigd nadat de werkstroom is gemaakt.

### <a name="generate-the-stamp-information-file"></a>Het stempel bestand genereren

1. Meld u aan bij de DVM of computers die toegang tot de Azure Stack-omgeving heeft.
2. Voer de volgende opdrachten in een PowerShell-venster met verhoogde bevoegdheid:
    ```PowerShell  
    $CloudAdminUser = "<cloud admin username>"
    $CloudAdminPassword = ConvertTo-SecureString "<cloud admin password>" -AsPlainText -Force
    $stampInfoCreds = New-Object System.Management.Automation.PSCredential($CloudAdminUser, $CloudAdminPassword)
    $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation' -Credential $stampInfoCreds
    ConvertTo-Json $params > stampinfoproperties.json
    ```

### <a name="locate-values-in-the-ece-configuration-file"></a>Waarden vinden in het configuratiebestand EEG

Omgeving parameterwaarden kunnen ook handmatig aanwezig zijn in de **EEG configuratiebestand** dat zich bevindt in `C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787` op de DVM.

## <a name="test-parameters"></a>Testparameters

Algemene testparameters gevoelige gegevens bevatten die in configuratiebestanden kan niet worden opgeslagen. Dit moeten handmatig opgegeven.

Parameter    | Description
-------------|-----------------
Tenant-beheerder                            | Azure Active Directory Tenant-beheerder die is ingericht met de servicebeheerder in de AAD-directory. Deze gebruiker wordt uitgevoerd op tenantniveau acties, zoals sjablonen voor het instellen van resources (virtuele machines, opslagaccounts, enz.) implementeren en uitvoeren van workloads. Zie voor meer informatie over het inrichten van de tenantaccount [toevoegen van een nieuwe Azure Stack-tenant](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad).
Service-beheerder             | Azure Active Directory Administrator van de AAD-Directory-Tenant opgegeven tijdens de implementatie van Azure Stack. Zoeken naar `AADTenant` in de configuratie van EEG-bestand en selecteer de waarde in de `UniqueName` element.
De beheerder van cloud-gebruiker               | Azure Stack-domein administrator-account (bijvoorbeeld `contoso\cloudadmin`). Zoeken naar `User Role="CloudAdmin"` in de configuratie van EEG-bestand en selecteer de waarde in de `UserName` element.
Verbindingsreeks voor diagnostische gegevens          | Een SAS-URL naar een Azure Storage-Account op welke diagnostische logboeken moeten worden gekopieerd tijdens het uitvoeren van de test. Zie voor instructies over het genereren van de SAS-URL [genereren van de verbindingsreeks van diagnostische gegevens over](#generate-the-diagnostics-connection-string). |

> [!IMPORTANT]
> De **diagnostics verbindingsreeks** moet geldig zijn voordat u doorgaat.

### <a name="generate-the-diagnostics-connection-string"></a>De verbindingsreeks van diagnostische gegevens genereren

De verbindingsreeks van diagnostische gegevens is vereist voor het opslaan van logboeken met diagnostische gegevens tijdens het uitvoeren van de test. Gebruik de Azure Storage-Account gemaakt tijdens de installatie (Zie [instellen dat uw gevalideerd als een serviceresources](azure-stack-vaas-set-up-resources.md)) te maken van een shared access signature (SAS)-URL waarmee u VaaS toegang om Logboeken te uploaden naar uw opslagaccount.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Selecteer **Blob** van **toegestane Services opties**. Schakel alle overige opties uit.

1. Selecteer **Service**, **Container**, en **Object** van **toegestane resourcetypen**.

1. Selecteer **lezen**, **schrijven**, **lijst**, **toevoegen**, **maken** van **toegestaan machtigingen**. Schakel alle overige opties uit.

1. Stel **begintijd** op de huidige tijd en **eindtijd** tot drie maanden vanaf de huidige tijd.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]

> [!NOTE]  
> De SAS-URL is verlopen op de eindtijd opgegeven wanneer de URL is gegenereerd.  
Bij het plannen van tests, zorg ervoor dat de URL geldig voor ten minste 30 dagen is plus de tijd die nodig is voor de testuitvoering van de (de drie maanden wordt aanbevolen).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [validatie uit als een Service-belangrijkste concepten](azure-stack-vaas-key-concepts.md)