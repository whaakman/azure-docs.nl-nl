---
title: Beheren van DNS-zones in Azure DNS - PowerShell | Microsoft Docs
description: U kunt DNS-zones met behulp van Azure Powershell beheren. In dit artikel wordt beschreven hoe u bijwerken, verwijderen en het maken van DNS-zones in Azure DNS
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: victorh
ms.openlocfilehash: 59f76d4c06a2a60cb513fc37a9ca3e27e13820b9
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173605"
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>Over het beheren van DNS-Zones met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Dit artikel ziet u hoe u uw DNS-zones beheren met behulp van Azure PowerShell. U kunt ook beheren met uw DNS-zones met behulp van de platformoverschrijdende [Azure CLI](dns-operations-dnszones-cli.md) of de Azure-portal.

Deze handleiding behandelt specifiek de openbare DNS-zones. Zie voor meer informatie over het gebruik van Azure PowerShell voor het beheren van Privézones in Azure DNS [aan de slag met Azure DNS Private Zones met behulp van Azure PowerShell](private-dns-getstarted-powershell.md).

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de cmdlet `New-AzureRmDnsZone`.

Het volgende voorbeeld wordt een DNS-zone met de naam *contoso.com* in de resourcegroep met de naam *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

Het volgende voorbeeld laat zien hoe een DNS-zone maken met twee [tags van Azure Resource Manager](dns-zones-records.md#tags), *project = demo* en *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

Azure DNS ondersteunt nu ook privé-DNS-zones (momenteel in openbare preview).  Voor meer informatie over privé-DNS-zones raadpleegt u [Using Azure DNS for private domains](private-dns-overview.md) (Azure DNS gebruiken voor privédomeinen). Zie voor een voorbeeld van het maken van een privé-DNS-zone [Aan de slag met privézones in Azure DNS met behulp van PowerShell](./private-dns-getstarted-powershell.md).

## <a name="get-a-dns-zone"></a>Een DNS-zone ophalen

Als u wilt ophalen van een DNS-zone, gebruikt u de `Get-AzureRmDnsZone` cmdlet. Met deze bewerking retourneert een DNS-zone-object hoort bij een bestaande zone in Azure DNS. Het object bevat gegevens over de zone (zoals het aantal recordsets), maar bevat niet de recordsets zelf (Zie `Get-AzureRmDnsRecordSet`).

```powershell
Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-8ec2-f4879750d201
Tags                  : {project, env}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org.,
                        ns4-01.azure-dns.info.}
NumberOfRecordSets    : 2
MaxNumberOfRecordSets : 5000
```

## <a name="list-dns-zones"></a>DNS-zones vermelden

Door de zonenaam weg te laten uit `Get-AzureRmDnsZone`, kunt u een opsomming maken van alle zones in een resourcegroep. Deze bewerking retourneert een matrix met zoneobjecten.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Door zowel de zonenaam als de resourcegroepnaam uit `Get-AzureRmDnsZone` weg te laten, kunt u een opsomming maken van alle zones in het Azure-abonnement.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>Een DNS-zone bijwerken

Wijzigingen aan een DNS-zoneresource kunnen worden aangebracht met behulp van `Set-AzureRmDnsZone`. Met deze cmdlet worden geen DNS-recordsets in de zone bijgewerkt (zie [DNS-records beheren](dns-operations-recordsets.md)). De cmdlet wordt alleen gebruikt voor het bijwerken van eigenschappen van de zoneresource zelf. De schrijfbaar zone-eigenschappen zijn momenteel beperkt tot de [Azure Resource Manager-tags' voor de zoneresource](dns-zones-records.md#tags).

Gebruik een van de volgende twee manieren om bij te werken van een DNS-zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Geef de zone met behulp van de zone-naam en resourcegroep-groep

Deze methode vervangt de bestaande tags voor de zone met de opgegeven waarden.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>Geef de zone op met behulp van een $zone-object

Deze methode wordt het bestaande zone-object opgehaald, Hiermee wijzigt u de tags en vervolgens de wijzigingen worden doorgevoerd. Op deze manier kunnen de bestaande tags worden bewaard.

```powershell
# Get the zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

Bij het gebruik van `Set-AzureRmDnsZone` met een object $zone [Etag controleert](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet overschreven. U kunt de optionele `-Overwrite` overschakelen naar deze controles onderdrukken.

## <a name="delete-a-dns-zone"></a>Een DNS-Zone verwijderen

DNS-zones kunnen worden verwijderd met de `Remove-AzureRmDnsZone` cmdlet.

> [!NOTE]
> Als u een DNS-zone verwijdert, worden ook alle DNS-records binnen de zone verwijderd. Deze bewerking kan niet ongedaan worden gemaakt. Als de DNS-zone in gebruik is en de zone wordt verwijderd, werken services die van de zone gebruik maken niet langer.
>
>Zie [DNS-zones en records beschermen](dns-protect-zones-recordsets.md) om beveiliging in te stellen tegen het per ongeluk verwijderen van zones.


Gebruik een van de volgende twee manieren om een DNS-zone te verwijderen:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geef de zone op met behulp van de zonenaam en de resourcegroepnaam

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>Geef de zone op met behulp van een $zone-object

U kunt de te verwijderen zone opgeven met behulp van een `$zone`-object dat wordt geretourneerd door `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Het zone-object kan ook worden doorgesluisd in plaats van te worden doorgegeven als een parameter:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

Net als bij `Set-AzureRmDnsZone`, op te geven de zone met behulp van een `$zone` object kunt Etag controles om ervoor te zorgen gelijktijdige wijzigingen zijn niet verwijderd. Gebruik de `-Overwrite` overschakelen naar deze controles onderdrukken.

## <a name="confirmation-prompts"></a>Bevestigingsprompts

De cmdlets `New-AzureRmDnsZone`, `Set-AzureRmDnsZone` en `Remove-AzureRmDnsZone` ondersteunen bevestigingspromts.

Zowel in `New-AzureRmDnsZone` als `Set-AzureRmDnsZone` wordt om bevestiging gevraagd als de PowerShell-voorkeursvariabele `$ConfirmPreference` de waarde `Medium` of lager heeft. Vanwege het potentieel grote effect van het verwijderen van een DNS-zone vraagt de cmdlet `Remove-AzureRmDnsZone` om bevestiging als de PowerShell-variabele `$ConfirmPreference` een andere waarde heeft dan `None`.

Omdat de standaardwaarde voor `$ConfirmPreference` `High` is, wordt standaard alleen in `Remove-AzureRmDnsZone` om bevestiging gevraagd.

U kunt de huidige instelling van `$ConfirmPreference` overschrijven met behulp van de parameter `-Confirm`. Als u `-Confirm` of `-Confirm:$True` opgeeft, vraagt de cmdlet u om bevestiging voordat deze wordt uitgevoerd. Als u `-Confirm:$False` opgeeft, wordt u niet om bevestiging gevraagd.

Zie [over voorkeursvariabelen](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables) voor meer informatie over `-Confirm` en `$ConfirmPreference`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren recordsets en records](dns-operations-recordsets.md) in uw DNS-zone.
<br>
Meer informatie over het [uw domein delegeren naar Azure DNS](dns-domain-delegation.md).
<br>
Controleer de [referentiedocumentatie voor PowerShell voor Azure DNS](/powershell/module/azurerm.dns).

