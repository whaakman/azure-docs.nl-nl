---
title: Een nieuwe Azure Stack-oplossing te valideren | Microsoft Docs
description: Leer hoe u een nieuwe Azure Stack-oplossing met validatie als een service te valideren.
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
ms.openlocfilehash: 90c18af0f846061c102f1daf1b84a332aaec1dc6
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235421"
---
# <a name="validate-a-new-azure-stack-solution"></a>Een nieuwe Azure Stack-oplossing te valideren

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Leer hoe u kunt de werkstroom van de validatie van oplossing voor externe certificerende nieuwe oplossingen voor Azure Stack.

Een oplossing voor Azure Stack is een hardware stuklijst (BoM) die gezamenlijk akkoord gaat met Microsoft en het logo Windows Server certificeringsvereisten is verstreken. U kunt ook de werkstroom van de oplossing validatie gebruiken wanneer er een wijziging in de hardware stuklijst die kan leiden dat een oplossing om te worden geclassificeerd is ertoe als *nieuwe*. Als er vragen over wat zouden activeren een **nieuwe** of **certificering** van een oplossing contact op met op [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Als u wilt certificeren van uw oplossing, moet u de werkstroom tweemaal uitgevoerd. Voer één keer voor de *minimaal* configuratie ondersteund. Uitvoeren in deze een tweede keer voor de *maximale* configuratie. Microsoft verklaart de oplossing als beide configuraties slagen voor alle tests.

In deze Quick Start kunt u het proces van het toevoegen van uw oplossing en -tests uitvoert aan de hand.

## <a name="add-a-new-solution"></a>Een nieuwe oplossing toevoegen

1. Aanmelden bij de [validatie portal](https://azurestackvalidation.com).
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
4. Selecteer **uploaden** en voeg uw configuratiebestand voor implementatie. Dit is een optionele stap. U kunt ook uw testparameters toevoegen met de volgende stappen in de volgende sectie.

    > [!note]  
    > U kunt uw configuratiebestand maken door de parameters in de omgeving parameters toe te voegen een algemene secties van de test-parameters in de interface. U kunt het bestand dat is gegenereerd door de service uit de Azure Stack-implementatie wordt gevalideerd ophalen. Zie voor instructies [werkstroom algemene parameters voor Azure Stack-validatie als een service](azure-stack-vaas-parameters.md).

5. Uw omgeving parameters toevoegen. Zie voor meer informatie, [uitwerking parameters toevoegen](#add-environmental-parameters).
6. Toevoegen van uw algemene testparameters. Zie voor meer informatie, [toevoegen van algemene testparameters](#add-common-test-parameters).

    De test-, afhankelijk van de definitie van de test mogelijk wanneer u een waarde onafhankelijk van de algemene parameters in te voeren, of kunt u de waarde van de algemene parameter overschrijven.

7. Klik op **indienen** voor het plannen van de test.

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

- [Opnieuw inplannen of annuleren van een test](azure-stack-vaas-monitor-test.md#reschedule-a-test)
- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).