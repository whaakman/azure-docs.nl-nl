---
title: Betaalde of gelicentieerde onderdelen voor de integratie van Azure SSIS runtime ontwikkelen | Microsoft Docs
description: Dit artikel wordt beschreven hoe een ISV kunt ontwikkelen en installatie betaald of aangepaste onderdelen voor de integratie van Azure SSIS runtime licentie
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: 42f1bb247533fafbc6947e77cc1a1f07a482fd45
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700499"
---
# <a name="develop-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Betaalde of gelicentieerde aangepaste onderdelen voor de integratie van Azure SSIS runtime ontwikkelen

## <a name="problem---the-azure-ssis-ir-requires-a-different-approach"></a>Probleem - de Azure-SSIS-IR vereist een andere benadering

De aard van de Azure-SSIS-integratie runtime geeft enkele uitdagingen, waardoor de typische licentieverlening methoden voor de lokale installatie van aangepaste onderdelen niet voldoende.

-   De knooppunten van de Azure-SSIS-IR vluchtige zijn en kan worden toegewezen of op elk gewenst moment worden vrijgegeven. U kunt bijvoorbeeld starten of stoppen van de knooppunten de kosten beheren of omhoog en omlaag geschaald via verschillende knooppuntgrootten. Een component van derden licentie binden aan een bepaald knooppunt met behulp van de machine-specifieke informatie, zoals MAC-adres of CPU-ID is daardoor niet langer beschikbaar.

-   U kunt ook de Azure-SSIS-IR in of uit schalen zodat het aantal knooppunten kunt verkleinen of Vouw op elk gewenst moment.

## <a name="solution---windows-environment-variables-and-ssis-system-variables-for-license-binding-and-validation"></a>Oplossing - Windows-omgevingsvariabelen en SSIS systeemvariabelen voor licentie-binding en validatie

Als gevolg van de beperkingen van traditionele licentieverlening methoden die worden beschreven in de vorige sectie, biedt de Azure-SSIS-IR Windows-omgevingsvariabelen en SSIS systeemvariabelen voor de licentie-binding en de validatie van de onderdelen van derden. ISV's kunnen u deze variabelen gebruiken uniek en permanente info ophalen voor een Azure-SSIS-IR, zoals Cluster-ID en het aantal clusters-knooppunt. Met deze gegevens kunnen onafhankelijke softwareleveranciers de licentie voor hun onderdeel binden aan een Azure-SSIS-IR *als een cluster*, met een ID die verandert niet wanneer klanten starten of stoppen, schaal omhoog of omlaag, in of uit te schalen of opnieuw configureren van de Azure-SSIS-IR op elke manier.

Het volgende diagram toont de standaardinstallatie, activering en licentie-binding en validatie loopt voor onderdelen van derden die deze nieuwe variabelen gebruiken:

![Installatie van een licentie-onderdelen](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Instructies
1. ISV's te kunnen bieden hun gelicentieerde onderdelen in verschillende SKU's of lagen (bijvoorbeeld één knooppunt, maximaal 5 knooppunten, tot maximaal 10 knooppunten, enzovoort). De ISV biedt de bijbehorende productcode wanneer klanten een product aanschaffen. De ISV biedt ook een Azure Storage-blob-container die een ISV-installatiescript en bijbehorende bestanden bevat. Klanten kunnen deze bestanden kopiëren naar hun eigen storage-container en ze met hun eigen productcode wijzigen (bijvoorbeeld door het uitvoeren van `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Klanten kunnen vervolgens inrichten of opnieuw configureren van de Azure-SSIS-IR met SAS-URI van de container als parameter. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie.

2. Wanneer de Azure-SSIS-IR is ingericht of opnieuw geconfigureerd, ISV-Setup wordt uitgevoerd op elk knooppunt om een query uitvoeren op de Windows-omgevingsvariabelen `SSIS_CLUSTERID` en `SSIS_CLUSTERNODECOUNT`. De Azure-SSIS-IR verzendt vervolgens de Cluster-ID en de productcode voor het gelicentieerde product met de ISV-activeringsserver om een activeringscode genereren.

3. Na de ontvangst van de activeringscode kan ISV Setup de sleutel lokaal opslaan op elk knooppunt (bijvoorbeeld in het register).

4. Wanneer klanten een pakket dat gebruikmaakt van gelicentieerde de ISV-onderdeel op een knooppunt van de Azure-SSIS-IR uitvoert, het pakket lokaal opgeslagen activeringscode leest en wordt deze gevalideerd op basis van het knooppunt Cluster-ID. Het pakket kan eventueel ook het aantal clusters knooppunt aan de ISV-activeringsserver rapporteren.

    Hier volgt een voorbeeld van code die valideert de activeringscode en rapporteert het aantal clusters knooppunt:

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

U vindt een lijst met de ISV-partners die de onderdelen en -extensies voor de Azure-SSIS-IR aan het einde van dit blogbericht - hebt aangepast [Enterprise Edition en aangepaste installatie 3e partij uitbreidbaarheid van SSIS in ADF](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/).

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste instellingen voor de integratie van Azure SSIS runtime](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Enterprise-editie van de Runtime Azure SSIS-integratie](how-to-configure-azure-ssis-ir-enterprise-edition.md)
