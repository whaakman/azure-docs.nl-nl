---
title: Gelicentieerde onderdelen voor de Azure-SSIS integratieruntime installeren | Microsoft Docs
description: Meer informatie over hoe een ISV kunt ontwikkelen en installeer een betaalde versie of aangepaste onderdelen voor de Azure-SSIS integratieruntime in licentie gegeven
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 80d4fff03422beacccd3aff3cdd8cb1047d5f5af
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022322"
---
# <a name="install-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Betaalde versie of gelicentieerde aangepaste onderdelen voor de Azure-SSIS integratieruntime installeren

Dit artikel wordt beschreven hoe een ISV kunt ontwikkelen en aangepaste onderdelen voor betaalde versie of een licentie voor SQL Server Integration Services (SSIS)-pakketten die worden uitgevoerd in Azure in de Azure-SSIS integratieruntime installeren.

## <a name="the-problem"></a>Het probleem

De aard van de Azure-SSIS integratieruntime biedt verschillende uitdagingen waarmee u de typische licentieverlening methoden gebruikt voor de on-premises installatie van de aangepaste onderdelen onvoldoende. Als gevolg hiervan moet de Azure-SSIS-IR een andere benadering.

-   De knooppunten van de Azure-SSIS-IR vluchtige zijn en kan worden toegewezen of die zijn uitgebracht op elk gewenst moment. U kunt bijvoorbeeld starten of stoppen van de knooppunten die moeten worden de kosten voor het beheren of omhoog en omlaag schalen via verschillende knooppuntgrootten. Een licentie van derden onderdeel binden aan een bepaald knooppunt met behulp van machine-specifieke informatie, zoals MAC-adres of CPU-ID is als gevolg hiervan niet langer beschikbaar.

-   Ook kunt u de Azure-SSIS-IR in of uit, zodat het aantal knooppunten kunt verkleinen of uitbreiden van op elk gewenst moment schalen.

## <a name="the-solution"></a>De oplossing

Als gevolg van de beperkingen van traditionele licentieverlening methoden die worden beschreven in de vorige sectie, biedt de Azure-SSIS-IR een nieuwe oplossing. Deze oplossing maakt gebruik van Windows-omgevingsvariabelen en systeemvariabelen SSIS voor de licentie-binding en de validatie van de onderdelen van derden. ISV's kunnen deze variabelen gebruiken om gegevens over de unieke en permanente verkrijgen voor een Azure-SSIS-IR, zoals Cluster-ID en het aantal clusterknooppunten. Met deze gegevens, ISV's kunnen vervolgens verbindt de licentie voor het onderdeel naar een Azure-SSIS IR *als een cluster*. Deze binding gebruikt een ID die niet worden gewijzigd wanneer klanten starten of stoppen, schaal omhoog of omlaag schalen in- of uitschalen of opnieuw configureren van de Azure-SSIS IR op geen enkele manier.

Het volgende diagram toont de typische installatie, activering en licentie-binding en validatie van stromen die voor de onderdelen van derden die gebruikmaken van deze nieuwe variabelen:

![Installatie van gelicentieerde onderdelen](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Instructies
1. ISV's kunnen hun gelicentieerde onderdelen in verschillende SKU's of lagen (bijvoorbeeld één knooppunt, maximaal 5 knooppunten, tot maximaal 10 knooppunten, enzovoort) bieden. De ISV biedt de bijbehorende productcode wanneer klanten een product kopen. De ISV biedt ook een Azure Storage blob-container met een ISV-Setup-script en de bijbehorende bestanden. Klanten kunnen deze bestanden kopiëren naar hun eigen container voor opslag en ze met hun eigen productcode wijzigen (bijvoorbeeld door het uitvoeren van `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Klanten kunnen vervolgens inrichten of opnieuw configureren van de Azure-SSIS IR met de SAS-URI van de container als parameter. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie.

2. Wanneer de Azure-SSIS IR is ingericht of opnieuw geconfigureerd, ISV Setup wordt uitgevoerd op elk knooppunt om op te vragen van de Windows-omgevingsvariabelen `SSIS_CLUSTERID` en `SSIS_CLUSTERNODECOUNT`. De Azure-SSIS-IR verzendt vervolgens de Cluster-ID en de productcode voor het gelicentieerde product op de activeringsserver ISV voor het genereren van een activeringscode.

3. Na de ontvangst van de activeringscode kan ISV Setup de sleutel lokaal opslaat op elk knooppunt (bijvoorbeeld in het register).

4. Wanneer klanten een pakket die gebruikmaakt van een licentie van de ISV-onderdeel op een knooppunt van de Azure-SSIS-IR uitvoert, het pakket lokaal opgeslagen activeringscode leest en wordt deze gevalideerd op basis van het knooppunt Cluster-ID. Het pakket kan eventueel ook het aantal clusterknooppunten rapporteren aan de ISV-activeringsserver.

    Hier volgt een voorbeeld van code die u valideert de activeringscode en rapporteert het aantal clusterknooppunten:

    ```csharp
    public override DTSExecResult Validate(Connections, VariableDispenser, IDTSComponentEvents componentEvents, IDTSLogging log) 
                                                                                                                               
    {                                                                                                                             
                                                                                                                               
    Variables vars = null;                                                                                                        
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterID");                                                                           
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterNodeCount");                                                                    
                                                                                                                               
    variableDispenser.GetVariables(ref vars);                                                                                     
                                                                                                                               
    // Validate Activation Key with ClusterID                                                                                     
                                                                                                                               
    // Report on ClusterNodeCount                                                                                                 
                                                                                                                               
    vars.Unlock();                                                                                                                
                                                                                                                               
    return base.Validate(connections, variableDispenser, componentEvents, log);                                                   
                                                                                                                               
    }
    ```

## <a name="isv-partners"></a>ISV-partners

U vindt een lijst van ISV-partners die hun onderdelen en -extensies voor de Azure-SSIS IR aan het einde van dit blogbericht - hebt aangepast [Enterprise Edition, aangepaste installatie en 3e partij uitbreidbaarheid voor SSIS in ADF](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/).

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste instellingen voor de Azure-SSIS integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Enterprise-editie van de Azure-SSIS Integratieruntime](how-to-configure-azure-ssis-ir-enterprise-edition.md)
