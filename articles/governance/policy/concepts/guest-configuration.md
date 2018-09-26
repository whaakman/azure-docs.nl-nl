---
title: Begrijpen hoe audits binnen een virtuele machine wordt uitgevoerd in Azure Policy
description: Meer informatie over hoe Azure Policy Gast-configuratie gebruikt voor controle-instellingen in een virtuele machine van Azure.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: ca96aea8f359f1df7da48f84a3317a2d8c7b52e4
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167900"
---
# <a name="understand-azure-policys-guest-configuration"></a>Informatie over Azure Policy Gast-configuratie

Naast het controleren en [herstellen](../how-to/remediate-resources.md) Azure-resources, Azure Policy is geschikt voor controle-instellingen in een virtuele machine. De validatie wordt uitgevoerd door de configuratie van de Gast-extensie en de client. De extensie, via de client, valideert instellingen zoals de configuratie van het besturingssysteem, Toepassingsconfiguratie of aanwezigheid en omgevingsinstellingen.

> [!IMPORTANT]
> Op dit moment alleen **ingebouwde** beleid met gast-configuratie wordt ondersteund.

## <a name="extension-and-client"></a>Extensie en client

Om te controleren van de instellingen in een virtuele machine, een [extensie van virtuele machine](../../../virtual-machines/extensions/overview.md) is ingeschakeld. De extensie wordt gedownload voor de toewijzing van configuratiebeleid van toepassing en de bijbehorende definitie van de configuratie.

### <a name="register-guest-configuration-resource-provider"></a>Configuratie van de Gast-resourceprovider registreren

Voordat u de configuratie van de Gast gebruiken kunt, moet u de resourceprovider registreren. U kunt dit doen via de portal of via PowerShell.

#### <a name="registration---portal"></a>Registratie - Portal

Volg deze stappen voor het registreren van de resourceprovider voor de configuratie van de Gast via Azure portal:

1. Starten van de Azure portal en klikt u op **alle services**. Zoek en selecteer **abonnementen**.

1. Zoek en klik op het abonnement dat u wilt de configuratie van de Gast voor inschakelen.

1. In het menu links van de **abonnement** pagina, klikt u op **resourceproviders**.

1. Filteren op of blader totdat u **Microsoft.GuestConfiguration**, klikt u vervolgens op **registreren** in dezelfde rij.

#### <a name="registration---powershell"></a>Registratie - PowerShell

Voor het registreren van de resourceprovider voor de configuratie van de Gast via PowerShell, voer de volgende opdracht:

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell
Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Hulpprogramma's voor validatie

In de virtuele machine gebruikt de client de configuratie van de Gast lokale hulpprogramma's om uit te voeren van de audit.

De volgende tabel bevat een overzicht van de lokale hulpprogramma's die op elk ondersteund besturingssysteem wordt gebruikt:

|Besturingssysteem|Hulpprogramma voor het valideren|Opmerkingen|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef inspectie mogelijk](https://www.chef.io/inspec/)| Ruby en Python worden geïnstalleerd door de configuratie van de Gast-extensie. |

### <a name="supported-client-types"></a>Ondersteunde client-typen

De volgende tabel ziet u een lijst met ondersteunde besturingssystemen op Azure-installatiekopieën:

|Uitgever|Naam|Versies|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter|
|OpenLogic|CentOS|7.3, 7.4, 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|SUSE|SLES|12 SP3|

> [!IMPORTANT]
> Gast-configuratie wordt momenteel niet ondersteund op aangepaste VM-installatiekopieën.

### <a name="unsupported-client-types"></a>Niet-ondersteunde client-typen

De volgende tabel geeft een overzicht van de besturingssystemen die niet worden ondersteund:

|Besturingssysteem|Opmerkingen|
|-|-|
|Windows-client | Client-besturingssystemen (zoals Windows 7 en Windows 10) worden niet ondersteund.
|Windows Server 2016 Nano Server | Wordt niet ondersteund.|

## <a name="guest-configuration-definition-requirements"></a>Configuratie van de Gast de definitie van vereisten

Elke controle uitgevoerd door de Gast-configuratie vereist twee beleidsdefinities, een **DeployIfNotExists** en **AuditIfNotExists**. **DeployIfNotExists** wordt gebruikt voor het voorbereiden van de virtuele machine met de configuratie van de Gast-agent en andere onderdelen voor de ondersteuning van de [-validatiehulpprogramma's](#validation-tools).

De **DeployIfNotExists** beleidsdefinitie valideert en corrigeert u het volgende:

- Zorg ervoor dat de virtuele machine een configuratie voor de evaluatie is toegewezen. Als er geen toewijzing momenteel aanwezig is, haalt u de toewijzing en voorbereiden van de virtuele machine door:
  - Verificatie bij de virtuele machine met een [beheerde identiteit](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Installeer de nieuwste versie van de **Microsoft.GuestConfiguration** extensie
  - Installeren van [-validatiehulpprogramma's](#validation-tools) en afhankelijkheden, indien nodig

Zodra de **DeployIfNotExists** compatibel, wordt de **AuditIfNotExists** beleidsdefinitie maakt gebruik van de lokale validatie van hulpprogramma's om te bepalen of de toewijzing van de toegewezen configuratie compatibele of Niet-compatibel. Het hulpprogramma voor het valideren biedt de resultaten naar de configuratie van de Gast-client, die deze naar de Gast-extensie doorstuurt zodat deze beschikbaar zijn via de configuratie van de Gast-resourceprovider.

Azure Policy maakt gebruik van de configuratie van de Gast-resourceproviders **complianceStatus** eigenschap voor rapport naleving in de **naleving** knooppunt. Zie voor meer informatie, [ophalen van Nalevingsgegevens](../how-to/getting-compliance-data.md).

> [!NOTE]
> Voor elke definitie van de Gast-configuratie, zowel de **DeployIfNotExists** en **AuditIfNotExists** beleidsdefinities moeten bestaan.

Alle ingebouwde beleidsregels voor de configuratie van de Gast zijn opgenomen in een initiatief aan groep de definities voor gebruik in toewijzingen. De ingebouwde initiatief met de naam *[Preview]: de instellingen voor controle wachtwoordbeveiliging in virtuele machines voor Linux en Windows* 18 beleid bevat. Er zijn zes **DeployIfNotExists** en **AuditIfNotExists** paren voor Windows en drie sets voor Linux. In elk geval wordt de logica in de definitie van de zorgt ervoor dat alleen het doelbesturingssysteem wordt geëvalueerd op basis van de [beleidsregel](definition-structure.md#policy-rule) definitie.

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Controleer de [structuur van beleidsdefinities](definition-structure.md)
- Beoordeling [effecten beleid begrijpen](effects.md)
- Begrijpen hoe u [programmatisch beleid maken](../how-to/programmatically-create.md)
- Meer informatie over het [Nalevingsgegevens ophalen](../how-to/getting-compliance-data.md)
- Ontdek hoe u [herstellen van niet-compatibele resources](../how-to/remediate-resources.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/index.md)