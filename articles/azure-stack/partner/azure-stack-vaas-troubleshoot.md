---
title: Azure Stack-validatie als een Service oplossen | Microsoft Docs
description: Problemen met de validatie uit als een Service voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: a6fea6962c5e8864398cd80635d997407cf1d22e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250296"
---
# <a name="troubleshoot-validation-as-a-service"></a>Validatie as a Service oplossen

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Hieronder vindt u veelvoorkomende problemen die niet gerelateerd aan softwareversies en hun oplossingen.

## <a name="local-agent"></a>Lokale agent

### <a name="the-portal-shows-local-agent-in-debug-mode"></a>De portal ziet u lokale agent in de foutopsporingsmodus

Dit is waarschijnlijk omdat de agent, kan geen heartbeats naar de service verzenden vanwege een instabiele netwerkverbinding. Een heartbeat is verzonden om de vijf minuten. Als de service niet gedurende 15 minuten geen heartbeat ontvangt, de service rekening gehouden met de agent niet actief en tests wordt niet meer worden gepland op het. Controleer het foutbericht de *Agenthost.log* bestand zich in de map waarin de agent is gestart.

> [!Note]
> Alle tests die al wordt uitgevoerd op de agent worden uitgevoerd, maar als de heartbeat niet is hersteld voordat de test-ends en vervolgens de agent mislukt voor het bijwerken van de status van de test of Logboeken te uploaden. De test wordt altijd weergegeven als **met** en moet worden geannuleerd.

### <a name="agent-process-on-machine-was-shut-down-while-executing-test-what-to-expect"></a>Agent-proces op de machine is afgesloten tijdens het uitvoeren van test. Wat u kunt verwachten?

Als het agentproces wordt afgesloten ungracefully bijvoorbeeld, machine opnieuw opgestart, afgesloten proces (CTRL + C in het venster van de agent wordt beschouwd als correct afsluiten) en vervolgens de test die werd uitgevoerd op deze blijft om weer te geven als **met**. Als de agent opnieuw is opgestart, wordt de status van de test om te worden bijgewerkt door de agent **geannuleerd**. Als de agent niet opnieuw wordt opgestart, wordt de test wordt weergegeven als **met** en moet u handmatig de test te annuleren.

> [!Note]
> Testen in een werkstroom zijn gepland om te worden opeenvolgend uitgevoerd. **In behandeling** tests wordt niet uitgevoerd totdat de tests in de **met** staat in de dezelfde werkstroom is voltooid.

## <a name="vm-images"></a>VM-installatiekopieën

### <a name="handle-slow-network-connectivity"></a>Langzame netwerkverbinding verwerken

U kunt de installatiekopie van het PIR downloaden naar een share in uw lokale datacenter. En vervolgens kunt u de installatiekopie controleren.

<!-- This is from the appendix to the Deploy local agent topic. -->

#### <a name="download-pir-image-to-local-share-in-case-of-slow-network-traffic"></a>PIR installatiekopie downloaden naar lokale share in het geval van trage netwerkverkeer

1. Downloaden van AzCopy op: [vaasexternaldependencies(AzCopy)](https://vaasexternaldependencies.blob.core.windows.net/prereqcomponents/AzCopy.zip)

2. AzCopy.zip ophalen en wijzigen naar de map met AzCopy.exe

3. Open Windows PowerShell uit vanaf een opdrachtprompt. Voer de volgende opdrachten uit:

```PowerShell  
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterFullBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterCoreBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'WindowsServer2012R2DatacenterBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1404LTS.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1604-20170619.1.vhd' /NC:12 /V:azcopylog.log /Y
```

> [!Note]  
> LocalFileShare is het pad naar bestandsshare of een lokaal pad.

#### <a name="verifying-pir-image-file-hash-value"></a>Afbeelding van PIR bestand hash-waarde wordt geverifieerd

U kunt **Get-HashFile** cmdlet om op te halen van de hash-waarde voor de installatiekopie van de gedownloade openbare opslagplaats afbeeldingsbestanden om te controleren of de integriteit van de installatiekopieën.

| Bestandsnaam | SHA256 |
|---------------------------------------|------------------------------------------------------------------|
| Server2016DatacenterFullBYOL.vhd | 6ED58DCA666D530811A1EA563BA509BF9C29182B902D18FCA03C7E0868F733E9 |
| WindowsServer2012R2DatacenterBYOL.vhd | 9792CBF742870B1730B9B16EA814C683A8415EFD7601DDB6D5A76D0964767028 |
| Server2016DatacenterCoreBYOL.vhd | 5E80E1A6721A48A10655E6154C1B90E320DF5558487D6A0D7BFC7DCD32C4D9A5 |
| Ubuntu1404LTS.vhd | B24CDD12352AAEBC612A4558AB9E80F031A2190E46DCB459AF736072742E20E0 |
| Ubuntu1604-20170619.1.vhd | C481B88B60A01CBD5119A3F56632A2203EE5795678D3F3B9B764FFCA885E26CB |

### <a name="failure-occurs-when-uploading-vm-image-in-the-vaasprereq-script"></a>Fout treedt op tijdens het uploaden van VM-installatiekopie in de `VaaSPreReq` script

Controleer eerst of de omgeving in orde is:

1. Uit de DVM / jump-box, controleert u of u kunt aanmelden tot de beheerportal met behulp van de beheerdersreferenties.
1. Bevestig dat er geen waarschuwingen of waarschuwingen zijn.

Als de omgeving goed is, moet u handmatig de 5 VM-installatiekopieën vereist zijn voor testuitvoeringen VaaS uploaden:

1. Meld u aan als de servicebeheerder tot de beheerportal. U vindt de beheerportal URL in EEG store of op het stempel zijn gemaakt. Zie voor instructies [omgeving parameters](azure-stack-vaas-parameters.md#environment-parameters).
1. Selecteer **meer services** > **Resourceproviders** > **Compute** > **VM-installatiekopieën**.
1. Selecteer de **+ toevoegen** knop aan de bovenkant van de **VM-installatiekopieën** blade.
1. Wijzigen of Controleer de waarden van de volgende velden voor de eerste VM-installatiekopie:
    > [!IMPORTANT]
    > Niet alle standaardwaarden zijn voor de bestaande Marketplace-Item correct.

    | Veld  | Value  |
    |---------|---------|
    | Uitgever | MicrosoftWindowsServer |
    | Aanbieding | WindowsServer |
    | Het type besturingssysteem | Windows |
    | SKU | 2012-R2-Datacenter |
    | Versie | 1.0.0 |
    | Besturingssysteemschijf Blob-URI | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |

1. Selecteer de knop **Create** (Maken).
1. Herhaal dit voor de resterende VM-installatiekopieën.

De eigenschappen van alle 5 VM-installatiekopieën zijn als volgt:

| Uitgever  | Aanbieding  | Het type besturingssysteem | SKU | Versie | Besturingssysteemschijf Blob-URI |
|---------|---------|---------|---------|---------|---------|
| MicrosoftWindowsServer| WindowsServer | Windows | 2012-R2-Datacenter | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-Datacenter | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterFullBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-Datacenter-Server-Core | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterCoreBYOL.vhd |
| Canonical | UbuntuServer | Linux | 14.04.3-LTS | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1404LTS.vhd |
| Canonical | UbuntuServer | Linux | 16.04-LTS | 16.04.20170811 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1604-20170619.1.vhd |

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [opmerkingen bij de Release voor validatie als een Service](azure-stack-vaas-release-notes.md) voor wijzigingen in de meest recente versies.