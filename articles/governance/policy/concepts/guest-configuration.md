---
title: Meer informatie over het controleren van de inhoud van een virtuele machine
description: Meer informatie over hoe Azure Policy Gast-configuratie gebruikt voor controle-instellingen in een virtuele machine van Azure.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/18/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 045339ec0a725359593797bda172a2e93f7c2890
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339020"
---
# <a name="understand-azure-policys-guest-configuration"></a>Informatie over Azure Policy Gast-configuratie

Naast het controleren en [herstellen](../how-to/remediate-resources.md) Azure-resources, Azure Policy kunnen controle-instellingen in een virtuele machine. De validatie wordt uitgevoerd door de configuratie van de Gast-extensie en de client. De extensie, via de client, valideert instellingen zoals de configuratie van het besturingssysteem, Toepassingsconfiguratie of aanwezigheid en omgevingsinstellingen.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="extension-and-client"></a>Extensie en client

Om te controleren van de instellingen in een virtuele machine, een [extensie van virtuele machine](../../../virtual-machines/extensions/overview.md) is ingeschakeld. De extensie wordt gedownload voor de toewijzing van configuratiebeleid van toepassing en de bijbehorende definitie van de configuratie.

### <a name="register-guest-configuration-resource-provider"></a>Configuratie van de Gast-resourceprovider registreren

Voordat u de configuratie van de Gast gebruiken kunt, moet u de resourceprovider registreren. U kunt registreren via de portal of via PowerShell. De resourceprovider wordt automatisch geregistreerd als de toewijzing van een gast-configuratiebeleid wordt gedaan via de portal.

#### <a name="registration---portal"></a>Registratie - Portal

Volg deze stappen voor het registreren van de resourceprovider voor de configuratie van de Gast via Azure portal:

1. Starten van de Azure portal en klikt u op **alle services**. Zoek en selecteer **abonnementen**.

1. Zoek en klik op het abonnement dat u wilt de configuratie van de Gast voor inschakelen.

1. In het menu links van de **abonnement** pagina, klikt u op **resourceproviders**.

1. Filteren op of blader totdat u **Microsoft.GuestConfiguration**, klikt u vervolgens op **registreren** in dezelfde rij.

#### <a name="registration---powershell"></a>Registratie - PowerShell

Voor het registreren van de resourceprovider voor de configuratie van de Gast via PowerShell, voer de volgende opdracht:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Hulpprogramma's voor validatie

In de virtuele machine gebruikt de client de configuratie van de Gast lokale hulpprogramma's om uit te voeren van de audit.

De volgende tabel bevat een overzicht van de lokale hulpprogramma's die op elk ondersteund besturingssysteem wordt gebruikt:

|Besturingssysteem|Hulpprogramma voor het valideren|Opmerkingen|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef inspectie mogelijk](https://www.chef.io/inspec/)| Ruby en Python worden geïnstalleerd door de configuratie van de Gast-extensie. |

### <a name="validation-frequency"></a>Validatie frequentie

De configuratie van de Gast-client controleert nieuwe inhoud om de 5 minuten. Nadat de toewijzing van een Gast is ontvangen, worden de instellingen in een interval van 15 minuten gecontroleerd. Resultaten worden verzonden naar de configuratie van de Gast-resourceprovider als de controle is voltooid. Wanneer een beleid [evaluatie trigger](../how-to/get-compliance-data.md#evaluation-triggers) optreedt, de status van de machine wordt geschreven bij de configuratie van de Gast-resourceprovider. Dit zorgt ervoor dat Azure-beleid om te evalueren van de Azure Resource Manager-eigenschappen. Een evaluatie van het beleid op aanvraag haalt de laatste waarde van de configuratie van de Gast-resourceprovider. Echter activeren niet het van een nieuwe controle van de configuratie van de virtuele machine.

### <a name="supported-client-types"></a>Ondersteunde client-typen

De volgende tabel ziet u een lijst met ondersteunde besturingssystemen op Azure-installatiekopieën:

|Uitgever|Name|Versies|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter, 2019 Datacenter|
|Microsoft|Windows-client|Windows 10|
|OpenLogic|CentOS|7.3, 7.4, 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|SUSE|SLES|12 SP3|

> [!IMPORTANT]
> Configuratie van de Gast kunt controleren knooppunten waarop een ondersteund besturingssysteem wordt uitgevoerd.  Als u controleren van virtuele machines die gebruikmaken van een aangepaste installatiekopie wilt, moet u voor het dupliceren van de **DeployIfNotExists** definitie en wijzig de **als** sectie om op te nemen van uw installatiekopie-eigenschappen.

### <a name="unsupported-client-types"></a>Niet-ondersteunde client-typen

Windows Server Nano Server wordt niet ondersteund in elke versie.

### <a name="guest-configuration-extension-network-requirements"></a>Vereisten voor uitbreiding van Gast-configuratie

Om te communiceren met de configuratie van de Gast-resourceprovider in Azure, virtuele machines uitgaande toegang tot Azure-datacenters op poort vereisen **443**. Als u een virtueel particulier netwerk in Azure en niet is toegestaan uitgaand verkeer, uitzonderingen moeten worden geconfigureerd met behulp van [Network Security Group](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) regels. Op dit moment een servicetag die voor Azure-beleidsconfiguratie Gast bestaat niet.

Voor een lijst met IP-adres, kunt u downloaden [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Dit bestand wordt wekelijks bijgewerkt, en heeft de bereiken momenteel zijn geïmplementeerd en eventuele toekomstige wijzigingen in de IP-adresbereiken. U hoeft alleen uitgaande toegang tot de IP-adressen in de regio's waar uw virtuele machines worden geïmplementeerd.

> [!NOTE]
> Het Azure Datacenter IP-adres XML-bestand bevat de IP-adresbereiken die worden gebruikt in de Microsoft Azure-datacenters. Het bestand bevat compute, SQL en storage.
> Wekelijks wordt een bijgewerkt bestand geplaatst. Het bestand weerspiegelt bereiken momenteel zijn geïmplementeerd en eventuele toekomstige wijzigingen in de IP-adresbereiken. Nieuwe bereiken die worden weergegeven in het bestand worden niet gebruikt in de datacenters voor ten minste één week.
> Er is een goed idee om te downloaden van het nieuwe XML-bestand elke week. Werk vervolgens uw site services die worden uitgevoerd in Azure correct kan worden geïdentificeerd. Gebruikers van Azure ExpressRoute Houd er rekening mee dat dit bestand wordt gebruikt voor het bijwerken van de aankondiging Border Gateway Protocol (BGP) van Azure-ruimte in de eerste week van elke maand.

## <a name="guest-configuration-definition-requirements"></a>Configuratie van de Gast de definitie van vereisten

Elke controle uitvoeren door Gast configuratie vereist twee beleidsdefinities, een **DeployIfNotExists** definitie en een **controleren** definitie. De **DeployIfNotExists** definitie wordt gebruikt voor het voorbereiden van de virtuele machine met de configuratie van de Gast-agent en andere onderdelen voor de ondersteuning van de [-validatiehulpprogramma's](#validation-tools).

De **DeployIfNotExists** beleidsdefinitie valideert en corrigeert de volgende items:

- Valideren van de virtuele machine een configuratie voor de evaluatie is toegewezen. Als er geen toewijzing momenteel aanwezig is, haalt u de toewijzing en voorbereiden van de virtuele machine door:
  - Verificatie bij de virtuele machine met een [beheerde identiteit](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Installeer de nieuwste versie van de **Microsoft.GuestConfiguration** extensie
  - Installeren van [-validatiehulpprogramma's](#validation-tools) en afhankelijkheden, indien nodig

Als de **DeployIfNotExists** toewijzing is niet-compatibel, een [herstel taak](../how-to/remediate-resources.md#create-a-remediation-task) kan worden gebruikt.

Zodra de **DeployIfNotExists** toewijzing is compatibel, de **Audit** beleidstoewijzing maakt gebruik van de lokale validatie van hulpprogramma's om te bepalen of de toewijzing van de configuratie van compatibele of niet-compatibel.
Het hulpprogramma voor het valideren biedt de resultaten naar de configuratie van de Gast-client. De client verzendt de resultaten naar de Gast-extensie, waardoor ze beschikbaar zijn via de configuratie van de Gast-resourceprovider.

Azure Policy maakt gebruik van de configuratie van de Gast-resourceproviders **complianceStatus** eigenschap voor rapport naleving in de **naleving** knooppunt. Zie voor meer informatie, [ophalen van Nalevingsgegevens](../how-to/getting-compliance-data.md).

> [!NOTE]
> Voor elke definitie van de Gast-configuratie, zowel de **DeployIfNotExists** en **Audit** beleidsdefinities moeten bestaan.

Alle ingebouwde beleidsregels voor de configuratie van de Gast zijn opgenomen in een initiatief aan groep de definities voor gebruik in toewijzingen. De ingebouwde initiatief met de naam *[Preview]: Controle-instellingen van wachtwoord-beveiliging in virtuele machines voor Linux en Windows* 18 beleid bevat. Er zijn zes **DeployIfNotExists** en **Audit** paren voor Windows en drie sets voor Linux. In elk geval wordt de logica in de definitie van de alleen het doel valideert besturingssysteem wordt geëvalueerd op basis van de [beleidsregel](definition-structure.md#policy-rule) definitie.

## <a name="client-log-files"></a>Logboekbestanden van client

De configuratie van de Gast-extensie schrijft logboekbestanden naar de volgende locaties:

Windows: `C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\<version>\dsc\logs\dsc.log`

Linux: `/var/lib/waagent/Microsoft.GuestConfiguration.ConfigurationforLinux-<version>/GCAgent/logs/dsc.log`

Waar `<version>` verwijst naar het huidige versienummer.

## <a name="guest-configuration-samples"></a>Voorbeelden van configuraties van de Gast

Voorbeelden voor configuratie van beleid voor gast zijn beschikbaar in de volgende locaties:

- [Voorbeelden van index - Gast-configuratie](../samples/index.md#guest-configuration)
- [Azure Policy-voorbeelden voor GitHub-opslagplaats](https://github.com/Azure/azure-policy/tree/master/samples/GuestConfiguration).

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [voorbeelden voor Azure Policy](../samples/index.md).
- Bekijk de [structuur van Azure Policy-definities](definition-structure.md).
- Lees [Informatie over de effecten van het beleid](effects.md).
- Begrijpen hoe u [programmatisch beleid maken](../how-to/programmatically-create.md).
- Meer informatie over het [ophalen compatibiliteitsgegevens](../how-to/getting-compliance-data.md).
- Meer informatie over het [herstellen van niet-compatibele resources](../how-to/remediate-resources.md).
- Lees wat een beheergroep met is [organiseren van uw resources met Azure-beheergroepen](../../management-groups/index.md).
