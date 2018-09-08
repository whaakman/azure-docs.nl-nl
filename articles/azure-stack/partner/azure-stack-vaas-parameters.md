---
title: Werkstroom algemene parameters in de Azure Stack-validatie als een service | Microsoft Docs
description: Werkstroom algemene parameters voor Azure Stack-validatie als een service
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: c50e4b5c9eb81c9386e2cb0db96a88de70dcb9e9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157800"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Werkstroom algemene parameters voor Azure Stack-validatie als een service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Algemene parameters bevatten waarden zoals omgevingsvariabelen en de gebruiker referenties vereist voor alle tests in de validatie als een service (VaaS). U definiëren deze waarden op het werkstroomniveau van de. U kunt de waarden opslaan wanneer u maakt of wijzigt een werkstroom. Bij de geplande tijd worden de werkstroom de waarden voor de test. 

## <a name="environment-parameters"></a>Omgeving parameters

Omgeving parameters beschrijven de Azure Stack-omgeving onder belasting. Deze waarden moeten door te genereren en uploaden van het configuratiebestand van de stempel worden opgegeven `&lt;link&gt;. [How to get the stamp info link].`

| Parameternaam | Vereist | Type | Beschrijving |
|----------------------------------|----------|------|---------------------------------------------------------------------------------------------------------------------------------|
| Azure Stack-build | Vereist |  | Build-nummer van de Azure Stack-implementatie (bijvoorbeeld 1.0.170330.9) |
| OEM-versie | Ja |  | Versienummer van de OEM-pakket wordt gebruikt tijdens de implementatie van Azure Stack. |
| OEM-handtekening | Ja |  | De handtekening van de OEM-pakket wordt gebruikt tijdens de implementatie van Azure Stack. |
| AAD-tenant-ID | Vereist |  | Azure Active Directory-tenant GUID die is opgegeven tijdens de implementatie van Azure Stack.|
| Regio | Vereist |  | Azure Stack-implementatie-regio. |
| Tenant Resource Manager-eindpunt | Vereist |  | Eindpunt voor de tenant Azure resourcemanager-bewerkingen (bijvoorbeeld https://management.<ExternalFqdn>) |
| Beheerder Resource Manager-eindpunt | Ja |  | Eindpunt voor de Tenant Azure Resource Manager-bewerkingen (bijvoorbeeld https://adminmanagement.<ExternalFqdn>) |
| Externe FQDN-naam | Ja |  | Externe FQDN-naam gebruikt als het achtervoegsel voor eindpunten. (bijvoorbeeld local.azurestack.external of redmond.contoso.com). |
| Aantal knooppunten | Ja |  | Het aantal knooppunten op de implementatie. |

## <a name="test-parameters"></a>Testparameters

Algemene testparameters gevoelige informatie die niet opgeslagen in configuratiebestanden bevatten en moeten handmatig worden opgegeven.

| Parameternaam | Vereist | Type | Beschrijving |
|--------------------------------|------------------------------------------------------------------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gebruikersnaam van de tenant | Vereist |  | Azure Active Directory-Tenant-beheerder die ofwel al is ingericht of moet worden ingericht met de servicebeheerder in de AAD-Directory. Zie voor meer informatie over het inrichten van tenantaccount [aan de slag met Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). Deze waarde wordt gebruikt door de test tenant niveau bewerkingen zoals het implementeren van sjablonen voor het inrichten van resources uit te voeren (virtuele machines, opslagaccounts enzovoort) en workloads uit te voeren. Deze waarde wordt gebruikt door de test tenant niveau bewerkingen zoals het implementeren van sjablonen voor het inrichten van resources uit te voeren (virtuele machines, opslagaccounts enzovoort) en workloads uit te voeren. |
| Tenant-wachtwoord | Vereist |  | Wachtwoord voor de tenant-gebruiker. |
| Gebruikersnaam voor de Service-beheerder | Vereist: Oplossing validatie, validatie van het pakket<br>Niet vereist: Test geslaagd |  | Azure Active Directory-beheerder van de AAD-Directory-Tenant opgegeven tijdens de implementatie van Azure Stack. |
| Service Administrator-wachtwoord | Vereist: Oplossing validatie, validatie van het pakket<br>Niet vereist: Test geslaagd |  | Wachtwoord voor de Service-beheerder-gebruiker. |
| De gebruikersnaam van beheerder van de cloud | Vereist |  | Azure Stack-domein administrator-account (bijvoorbeeld contoso\cloudadmin). Zoeken naar gebruikersrol = "CloudAdmin' in het configuratiebestand en selecteert u de waarde in de code van de gebruikersnaam in het configuratiebestand. |
| Cloud-Administrator-wachtwoord | Vereist |  | Wachtwoord voor de beheerder van Cloud-gebruiker. |
| Verbindingsreeks voor diagnostische gegevens | Vereist |  | Een SAS-URI naar een Azure Storage-Account op welke diagnostische logboeken moeten worden gekopieerd tijdens het uitvoeren van de test. Instructies voor het genereren van de SAS-URI zich bevinden [instellen van een blob storage-account](azure-stack-vaas-set-up-account.md). |


## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
