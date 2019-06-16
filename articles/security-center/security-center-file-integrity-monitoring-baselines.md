---
title: Basislijnen met bestandsintegriteit controleren in Azure Security Center vergelijken | Microsoft Docs
description: Informatie over het vergelijken van basislijnen met bestandsintegriteit controleren in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c8a2a589-b737-46c1-b508-7ea52e301e8f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: monhaber
ms.openlocfilehash: e403a9bd4d3f8668544dab1d81e9052b37839bef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358436"
---
# <a name="compare-baselines-using-file-integrity-monitoring-fim"></a>Vergelijken met bestand integriteit controleren (FIM) basislijnen

Bestand-integriteit controleren (FIM) informeert u wanneer er wijzigingen optreden tot gevoelige gebieden in uw resources, zodat u kunt onderzoeken en oplossen van niet-geautoriseerde activiteit. FIM controleert Windows-bestanden, registers van Windows en Linux-bestanden.

In dit onderwerp wordt uitgelegd hoe u FIM voor de bestanden en registers inschakelen. Zie voor meer informatie over FIM [bestandsintegriteit controleren in Azure Security Center](security-center-file-integrity-monitoring.md).

## <a name="why-use-fim"></a>Waarom FIM gebruiken?

Besturingssysteem, toepassingen en bijbehorende configuraties beheren van de status van het gedrag en de beveiliging van uw resources. Aanvallers richten daarom de bestanden die regelen van uw resources, om het besturingssysteem van een resource gehinderd en/of activiteiten uitvoeren zonder te worden gedetecteerd.

In feite vereisen veel standaarden voor naleving van regelgeving zoals PCI-DSS en ISO 17799 FIM besturingselementen implementeren.  

## <a name="enable-built-in-recursive-registry-checks"></a>Ingebouwde recursieve registercontroles inschakelen

De standaardinstellingen van FIM-register hive bieden een handige manier om recursieve wijzigingen in algemene aspecten van de beveiliging te bewaken.  Een kwaadwillende persoon kan bijvoorbeeld een script om uit te voeren in de context van LOCAL_SYSTEM door het configureren van een uitvoering bij het opstarten of afsluiten configureren.  Schakel de ingebouwde controle voor het controleren van wijzigingen van dit type.  

![Register](./media/security-center-file-integrity-monitoring-baselines/baselines-registry.png)

>[!NOTE]
> Recursieve gelden alleen voor aanbevolen security componenten en niet voor aangepaste registerpaden.  

## <a name="adding-a-custom-registry-check"></a>Toevoegen van een aangepaste Registercontrole

FIM-basislijnen starten door het identificeren van de kenmerken van een bekende goede status voor het besturingssysteem en ondersteunen van toepassing.  In dit voorbeeld gaan we in op de beleidsconfiguraties wachtwoord voor Windows Server 2008 en hoger.


|Beleidsnaam                 | Registerinstelling|
|---------------------------------------|-------------|
|Domeincontroller: Wijzigen van wachtwoorden van computeraccounts weigeren| MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RefusePasswordChange|
|Lid van domein: Digitaal versleutelen of ondertekenen (altijd) gegevens in beveiligd kanaal|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireSignOrSeal|
|Lid van domein: Digitaal versleutelen van gegevens in beveiligd kanaal (indien mogelijk)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\SealSecureChannel|
|Lid van domein: Digitaal ondertekenen gegevens in beveiligd kanaal (indien mogelijk)|MACHINE\System\CurrentControlSet\Services   \Netlogon\Parameters\SignSecureChannel|
|Lid van domein: Wachtwoord van computeraccount uitschakelen|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DisablePasswordChange|
|Lid van domein: Wachtwoord voor maximale machine computeraccount|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\MaximumPasswordAge|
|Lid van domein: Gebruik van sterke sessiesleutel voor (Windows 2000 of hoger)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireStrongKey|
|Netwerkbeveiliging: NTLM beperken:  NTLM-authenticatie in dit domein|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RestrictNTLMInDomain|
|Netwerkbeveiliging: NTLM beperken: Uitzonderingen voor servers in dit domein toevoegen|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DCAllowedNTLMServers|
|Netwerkbeveiliging: NTLM beperken: NTLM-authenticatie in dit domein controleren|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\AuditNTLMInDomain|

> [!NOTE]
> Voor meer informatie over registerinstellingen die door verschillende versies van besturingssystemen worden ondersteund, verwijzen naar de [instellingen voor Groepsbeleid verwijzen naar werkblad](https://www.microsoft.com/en-us/download/confirmation.aspx?id=25250).

*Het configureren van FIM voor het controleren van register-basislijnen:*

1. In de **Windows-register voor wijzigingen bijhouden toevoegen** venster in de **Windows-registersleutel** tekst voert u de registersleutel.

    <code>

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters
    </code>

      ![FIM op een register inschakelen](./media/security-center-file-integrity-monitoring-baselines/baselines-add-registry.png)

## <a name="tracking-changes-to-windows-files"></a>Wijzigingen bijhouden in de Windows-bestanden

1. In de **Windows-bestand voor wijzigingen bijhouden toevoegen** venster in de **Enter pad** tekst voert u de map waarin de bestanden die u wilt bijhouden. In het voorbeeld in de volgende afbeelding, **Web-App van Contoso** bevindt zich in de D:\ station in de **ContosWebApp** mapstructuur.  
1. Maak een aangepaste Windows-bestandsvermelding door het opgeven van de naam van de instellingsklasse, inschakelen recursie en de bovenste map met een jokerteken (*)-achtervoegsel op te geven.

    ![Inschakelen van FIM voor een bestand](./media/security-center-file-integrity-monitoring-baselines/baselines-add-file.png)

## <a name="retrieving-change-data"></a>Het ophalen van wijzigingsgegevens

Bestandsintegriteit controleren gegevens zich in de Azure Log Analytics bevinden / ConfigurationChange tabel ingesteld.  

 1. Stel een tijdsbereik voor het ophalen van een overzicht van wijzigingen met de resource.
In het volgende voorbeeld, we zijn bezig met ophalen van alle wijzigingen in de afgelopen veertien dagen in de categorieën van het register en bestanden:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry', 'Files')

    > | summarize count() by Computer, ConfigChangeType

    </code>

1. Voor meer informatie van de wijzigingen in het register:

    1. Verwijder **bestanden** uit de **waar** -component 
    1. De samenvatting regel verwijderen en vervang deze door een bestellen component:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry')

    > | order by Computer, RegistryKey

    </code>

Rapporten kunnen worden geëxporteerd naar CSV voor archivering en/of channeled aan een Power BI-rapport.  

![FIM-gegevens](./media/security-center-file-integrity-monitoring-baselines/baselines-data.png)