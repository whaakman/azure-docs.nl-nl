---
title: Testen van de parameters voor de validatie als een service van Azure Stack | Microsoft Docs
description: Parameters voor de validatie naslagonderwerp over de configuratie van bestands- en test doorgeven als een service van Azure Stack.
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
ms.openlocfilehash: 65cae4d10a4683207474008a18bac39751ce8e96
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234981"
---
# <a name="test-parameters-for-validation-as-a-service-azure-stack"></a>Parameters voor de validatie als een Azure Stack-service testen

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Voordat u een Test-Suite van validaties uitvoert als een service (VaaS), selecteert u een oplossing en maak een testronde.

- Meld u aan met uw geregistreerde VaaS tenant-referenties.
- Een nieuwe oplossing maken (hiervoor **oplossing toevoegen**) of Selecteer een bestaande.
- Selecteer de **Start** knop van de **Test passen** werkstroom tegel.

> [!Note]  
> Maak een nieuwe vermelding in de oplossing voor elke unieke machine set/hardware-configuratie die u wilt valideren, en een nieuwe test geeft voor elke implementatie van de build op deze computer is ingesteld.

U moet invoeren van de vereiste parameters voor het uitvoeren van de tests in de **doorgeven testgegevens** pagina. Deze parameters opgeven bij het starten van een nieuwe testronde of validatie worden uitgevoerd. De meeste van de parameters hebben dezelfde waarden die u hebt opgegeven tijdens de implementatie van Azure Stack.

Kunt u de waarden handmatig in de [Test parameters tabel](#test-parameters), of uploaden van het configuratiebestand voor implementatie, die de volledige Azure Stack stempel informatie bevat. Na het uploaden, worden de waarden in de portal geladen uit het bestand.

> [!Note]  
> U kunt zoeken naar en het typen van de test-parameterwaarden door te zoeken en het laden van het configuratiebestand in de portal.

## <a name="export-the-test-parameters-using-powershell"></a>Exporteren van de testparameters met behulp van PowerShell

1. Meld u aan DVM machine of elke computer die toegang tot de Azure Stack-omgeving heeft.
2. Open een PowerShell-venster met verhoogde bevoegdheid en voer:

    ````PowerShell  
        $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation'
        ConvertTo-Json $params > stampinfoproperties.json
    ````

3. Uploaden **stampinfoproperties.json** naar de portal VaaS.

## <a name="find-the-test-parameters-in-the-configuration-file"></a>De testparameters niet vinden in het configuratiebestand

U vindt hier ook de test parameterwaarden door het openen van de EEG **configuratiebestand**. U vindt het bestand op de volgende locatie op de computer DVM:  
`C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787`

## <a name="test-parameters"></a>Testparameters 

| Parameter    | Beschrijving |
|-------------|-----------------|
| Azure Stack-Build                      | Azure Stack bouwen of versie getal dat is geïmplementeerd bijvoorbeeld 1.0.170330.0 | 
| Tenant-id                              | Azure Active Directory-Tenant opgegeven tijdens de implementatie van Azure Stack. Zoeken naar **AADTenant** in het configuratiebestand en selecteer de **GUID** waarde in de `Id` tag. | 
| Regio                                 | Azure Stack-implementatie-regio. Zoeken naar **regio** in het configuratiebestand. | 
| Tenant Resource Manager                | Azure resourcemanager-eindpunt, bijvoorbeeld, tenant `https://management.<ExternalFqdn>` | 
| Beheerder Resource Manager                 | Eindpunt van Admin Azure Resource Manager. Bijvoorbeeld: `https://adminmanagement.<ExternalFqdn>` | 
| Externe FQDN-naam                          | Externe FQDN-naam van de omgeving. Zoeken naar **ExternalFqdn** in het configuratiebestand. | 
| Tenant-gebruiker                            | Azure Active Directory-Tenant-beheerder die ofwel al is ingericht of moet worden ingericht met de servicebeheerder in de Azure AD-Directory. Zie voor meer informatie over het inrichten van tenantaccount [toevoegen van een nieuw account voor Azure Stack-tenant in Azure Active Directory](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad). Deze waarde wordt gebruikt door de test tenant niveau bewerkingen zoals het implementeren van sjablonen voor het inrichten van resources uit te voeren (VM's, opslag-accounts enzovoort) en workloads uit te voeren. | 
| Service-beheerder             | Azure Active Directory-beheerder van de Azure AD-Directory-Tenant die is opgegeven tijdens de implementatie van Azure Stack. Zoeken naar **AADTenant** in de configuratie van bestands- en selecteert u de waarde in de `UniqueName` tag op in het configuratiebestand. | 

## <a name="checks-before-starting-the-tests"></a>Controles voordat u begint met de tests

De tests uitvoeren externe bewerkingen. De machine waarop de tests moet toegang hebben tot de Azure Stack-eindpunten. Anders wordt werkt de test niet. Als u de VaaS op lokale agent gebruikt, gebruikt u de computer waar de agent wordt uitgevoerd. U kunt controleren of uw computer toegang tot de Azure Stack-punten heeft door het uitvoeren van de volgende controles.

1. Controleer of de basis-URI kan worden bereikt. Open een opdrachtprompt of bash-shell en voer de volgende opdracht uit:

    ```bash  
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Open een webbrowser en navigeer naar `https://adminportal.<EXTERNALFQDN>` om te controleren dat de MAS-Portal kan worden bereikt.

3. Meld u aan met de Azure AD-service administrator-waarden en het wachtwoord opgegeven tijdens het maken van de testronde.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
