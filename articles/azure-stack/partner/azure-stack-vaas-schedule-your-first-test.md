---
title: De validatie als een service voor Azure Stack-portal gebruiken om te plannen van uw eerste test | Microsoft Docs
description: De validatie gebruiken als een service voor Azure Stack-portal voor het plannen van uw eerste test.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 0f681070df4b4b3384171c05edb3851abec2ab5c
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235410"
---
# <a name="quickstart-use-the-validation-as-a-service-portal-to-schedule-your-first-test"></a>Snelstartgids: De validatie gebruiken als een serviceportal voor het plannen van uw eerste test

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Informatie over het plannen van uw eerste test met de validatie uit als een (VaaS)-serviceportal om te controleren of uw hardware. De lokale agent moet worden geïmplementeerd op de Azure Stack-oplossing vóór het uitvoeren van de validatietests worden gevalideerd.

In deze Quick Start u uw oplossing toevoegen en voer tests uit.

## <a name="prerequisites"></a>Vereisten

Voordat u deze Quick Start volgen, moet u zijn:
 - Een validatie uit als een service-account. Zie voor instructies [instellen dat uw gevalideerd als een serviceaccount](azure-stack-vaas-set-up-account.md).  
- De lokale agent is geïnstalleerd op uw systeem. Zie voor instructies [implementeren van de lokale agent en test virtuele machine](azure-stack-vaas-test-vm.md).

## <a name="add-a-new-solution"></a>Een nieuwe oplossing toevoegen

1. Aanmelden bij de [validatie portal](https://azurestackvalidation.com).

    ![Meld u aan bij de validatie-portal](media/vaas_portalsignin.png)  

2. Selecteer **nieuwe oplossing**.
3. Voer een naam voor de oplossing en selecteer **opslaan**.

## <a name="create-a-solution-validation-workflow"></a>Een oplossing voor validatie-werkstroom maken

1. Selecteer de naam van de oplossing.
2. Selecteer **beheren** op de **oplossing validaties** tegel.

    ![Oplossing voor validaties](media/image2.png)

## <a name="create-a-solution-workflow"></a>Een werkstroom van de oplossing maken

1. Selecteer **nieuwe oplossing validatie**.
2. Typ de naam van de validatie.
3. Selecteer **Minimum** of **maximale**.  
    - **Minimum**  
    De oplossing is geconfigureerd met de minimale ondersteunde aantal knooppunten.  
    - **Maximaal**  
    De oplossing is geconfigureerd met de maximale ondersteunde aantal knooppunten.
4. Uw omgeving parameters toevoegen. Zie voor meer informatie, [uitwerking parameters toevoegen](#add-environmental-parameters).
5. Toevoegen van uw algemene testparameters. Zie voor meer informatie, [toevoegen van algemene testparameters](#add-common-test-parameters).

    De test-, afhankelijk van de definitie van de test mogelijk wanneer u een waarde onafhankelijk van de algemene parameters in te voeren, of kunt u de waarde van de algemene parameter overschrijven.
6. Klik op **indienen** voor het plannen van de test.

## <a name="add-environmental-parameters"></a>Uitwerking parameters toevoegen

De volgende uitwerking parameters toevoegen:

| Test pass informatie | Vereist | Beschrijving |
| --- | --- | --- | --- |
| Azure Stack-build | Vereist | Azure Stack-build nummer (bijvoorbeeld 20170501.1) waarde een geldig Azure Stack build moet-nummer of versie, bijvoorbeeld 1.0.170330.9 |
| Tenant-id | Vereist | Active Directory-Tenant-ID. Dit moet een GUID (bijvoorbeeld ECA23256-6BA0-4F27-8E4D-AFB02F088363) |
| Regio | Vereist | Azure Stack-implementatieregio |
| Tenant Resource Manager-eindpunt | Vereist | Eindpunt voor de Tenant Azure Resource Manager-bewerkingen (bijvoorbeeld https://management.loc-ext.domain.com) |
| Beheerder Resource Manager-eindpunt | Niet vereist | Eindpunt voor de Tenant Azure Resource Manager-bewerkingen (bijvoorbeeld https://management.loc-ext.domain.com) |
| Externe FQDN-naam | Niet vereist | Externe FQDN-naam gebruikt als het achtervoegsel voor eindpunten. (bijvoorbeeld local.azurestack.external of redmond.contoso.com) |
| Aantal knooppunten | Vereist | Het aantal knooppunten in uw oplossing. |

## <a name="add-common-test-parameters"></a>Algemene testparameters toevoegen

De volgende algemene testparameters toevoegen:

| Test pass informatie | Vereist | Beschrijving |
| --- | --- | --- |
| Gebruikersnaam van de tenant | Vereist | Tenant-gebruikersnaam (bijvoorbeeld tenant@contoso.onmicrosoft.com) |
| Tenant-wachtwoord | Vereist | Het wachtwoord voor de tenant. |
| Gebruikersnaam voor de Service-beheerder | Niet vereist | Tenant-gebruikersnaam (bijvoorbeeld tenant@contoso.onmicrosoft.com) |
| Service Administrator-wachtwoord | Niet vereist | Gebruikersnaam voor de Service-beheerder (bijvoorbeeld serviceadmin@contoso.onmicrosoft.com) |
| De gebruikersnaam van beheerder van de cloud | Niet vereist | Azure Stack-domein administrator-account (bijvoorbeeld contoso\cloudadmin) |
| Cloud-Administrator-wachtwoord | Niet vereist | |
|  Verbindingsreeks voor diagnostische gegevens | Niet vereist | SAS-URI naar een Azure Storage-Account op welke diagnostische logboeken moeten worden gekopieerd tijdens het uitvoeren van de test. Zie [maken van een Azure storage-blob voor het opslaan van logboeken](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs). <br><br>De waarde van de **Diagnostics verbindingsreeks** algemene parameter worden opgeslagen door de service en opgegeven tijdens de planning voor alle tests in de werkstroom die gebruikmaken van deze parameter. Wanneer de SAS-URL binnen 30 dagen na verlopen wordt, wordt u gevraagd voor een nieuwe SAS-URL op de pagina met algemene parameters. |
| Tag - naam | Niet vereist |  Beschrijvende labels kunnen worden ingevoerd als u de naam van de werkstroom. Dit is de naam van de tag. |
| Tag - waarde | Niet vereist | Beschrijvende labels kunnen worden ingevoerd als u de naam van de werkstroom. Dit is de waarde van de tag. |

## <a name="next-steps"></a>Volgende stappen

- [Een nieuwe Azure Stack-oplossing te valideren](azure-stack-vaas-validate-solution-new.md)  
- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
