---
title: DNS-zones in Azure DNS - PowerShell beheren | Microsoft Docs
description: U kunt DNS-zones met Azure Powershell beheren. Dit artikel wordt beschreven hoe u bijwerken, verwijderen en DNS-zones maken op Azure DNS
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2016
ms.author: gwallace
ms.openlocfilehash: 3f28e70bb6ef46f53375d256a520db40fcb71ad0
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>DNS-Zones met behulp van PowerShell beheren

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

In dit artikel laat zien hoe uw DNS-zones beheren met behulp van Azure PowerShell. U kunt ook de DNS-zones met behulp van de platformoverschrijdende beheren [Azure CLI](dns-operations-dnszones-cli.md) of de Azure-portal.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de cmdlet `New-AzureRmDnsZone`.

Het volgende voorbeeld wordt een DNS-zone aangeroepen *contoso.com* aangeroepen in de resourcegroep *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

Het volgende voorbeeld laat zien hoe een DNS-zone maken met twee [Azure Resource Manager-tags](dns-zones-records.md#tags), *project = demo* en *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

Azure DNS ondersteunt nu ook persoonlijke DNS-zones (momenteel een preview-functie).  Zie voor een voorbeeld van het maken van een persoonlijke DNS-zone [aan de slag met persoonlijke Azure DNS-zones met behulp van PowerShell](./private-dns-getstarted-powershell.md).

## <a name="get-a-dns-zone"></a>Ophalen van een DNS-zone

Voor het ophalen van een DNS-zone gebruiken de `Get-AzureRmDnsZone` cmdlet. Deze bewerking wordt een DNS-zone-object overeenkomt met een bestaande zone in Azure DNS. Het object bevat gegevens over de zone (zoals het aantal recordsets), maar bevat niet de recordsets zelf (Zie `Get-AzureRmDnsRecordSet`).

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

## <a name="list-dns-zones"></a>Lijst met DNS-zones

Door de naam van de zone van weg te laten `Get-AzureRmDnsZone`, kunt u alle zones in een resourcegroep opsommen. Deze bewerking retourneert een matrix van objecten van de zone.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Door de naam van zone voor zowel de naam van de resourcegroep uit weg te laten `Get-AzureRmDnsZone`, kunt u alle zones in de Azure-abonnement opsommen.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>Bijwerken van een DNS-zone

Een DNS-zone-bron kan worden gewijzigd met behulp van `Set-AzureRmDnsZone`. Deze cmdlet werkt niet bij een van de DNS-recordsets binnen de zone (Zie [het beheren van DNS-records](dns-operations-recordsets.md)). Het wordt alleen gebruikt om de eigenschappen van de bron van de zone zelf te werken. De schrijfbaar zone-eigenschappen zijn momenteel beperkt tot de [Azure Resource Manager '-tags' voor de zoneresource](dns-zones-records.md#tags).

Gebruik een van de volgende twee manieren om bij te werken van een DNS-zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Geef de zone met behulp van de zone en de bron-groep

Deze aanpak vervangt de bestaande zone labels met de opgegeven waarden.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>Geef de zone met behulp van een object $zone

Deze aanpak opgehaald van het bestaande object in de zone, wijzigt de labels en vervolgens de wijzigingen worden doorgevoerd. Op deze manier kunnen u bestaande labels behouden.

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

Wanneer u `Set-AzureRmDnsZone` met een object $zone [Etag controleert](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet overschreven. U kunt de optionele `-Overwrite` overschakelen naar het onderdrukken van deze controles.

## <a name="delete-a-dns-zone"></a>Een DNS-Zone verwijderen

DNS-zones worden verwijderd met de `Remove-AzureRmDnsZone` cmdlet.

> [!NOTE]
> Een DNS-zone verwijderen, verwijdert tevens alle DNS-records in de zone. Deze bewerking kan niet ongedaan worden gemaakt. Als de DNS-zone gebruikt wordt, worden services met behulp van de zone mislukt wanneer de zone wordt verwijderd.
>
>Als u wilt beveiligen tegen het onbedoeld zone verwijderen, Zie [het beveiligen van DNS-zones en records](dns-protect-zones-recordsets.md).


Gebruik een van de volgende twee manieren een DNS-zone verwijderen:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geef de zone met de zonenaam en de naam van resourcegroep

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>Geef de zone met behulp van een object $zone

U kunt opgeven dat de zone moet worden verwijderd met behulp van een `$zone` object dat wordt geretourneerd door `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

De zone-object kan ook worden doorgesluisd in plaats van dat wordt doorgegeven als parameter:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

Net als bij `Set-AzureRmDnsZone`, geven de zone met behulp van een `$zone` object kunt Etag controles om ervoor te zorgen gelijktijdige wijzigingen worden niet verwijderd. Gebruik de `-Overwrite` overschakelen naar het onderdrukken van deze controles.

## <a name="confirmation-prompts"></a>Bevestiging vragen

De `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, en `Remove-AzureRmDnsZone` cmdlets alle bevestiging vragen ondersteunen.

Beide `New-AzureRmDnsZone` en `Set-AzureRmDnsZone` vragen om bevestiging als de `$ConfirmPreference` PowerShell voorkeursvariabele een waarde heeft van `Medium` of lager. Vanwege het potentieel grote invloed van het verwijderen van een DNS-zone, de `Remove-AzureRmDnsZone` cmdlet vraagt om bevestiging als de `$ConfirmPreference` PowerShell variabele heeft de waarde van een andere waarde dan `None`.

Sinds de standaardwaarde voor `$ConfirmPreference` is `High`, alleen `Remove-AzureRmDnsZone` vraagt om bevestiging standaard.

U kunt de huidige overschrijven `$ConfirmPreference` instellen met de `-Confirm` parameter. Als u opgeeft `-Confirm` of `-Confirm:$True` , vraagt de cmdlet u om bevestiging voordat deze wordt uitgevoerd. Als u opgeeft `-Confirm:$False` , de cmdlet wordt u niet gevraagd om bevestiging.

Voor meer informatie over `-Confirm` en `$ConfirmPreference`, Zie [over Voorkeursvariabelen](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [beheren recordsets en records](dns-operations-recordsets.md) in uw DNS-zone.
<br>
Meer informatie over hoe [uw domein delegeren naar Azure DNS](dns-domain-delegation.md).
<br>
Controleer de [Azure DNS PowerShell-naslagdocumentatie](/powershell/module/azurerm.dns).

