---
title: Beveiligen van DNS-Zones en Records | Microsoft Docs
description: Het beveiligen van DNS-zones en -recordsets in Microsoft Azure DNS.
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 190e69eb-e820-4fc8-8e9a-baaf0b3fb74a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2016
ms.author: jonatul
ms.openlocfilehash: 0b7040d6273b3a6b85cd55850d596807226b87fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-protect-dns-zones-and-records"></a>Het beveiligen van DNS-zones en records

DNS-zones en records zijn kritieke bronnen. Verwijderen van een DNS-zone of zelfs slechts één DNS-record kan leiden tot een onderbreking van deze totale service.  Het is daarom belangrijk dat kritieke DNS-zones en records zijn beschermd tegen onbevoegde of onbedoelde wijzigingen.

Dit artikel wordt uitgelegd hoe Azure DNS kunt u uw DNS-zones en records tegen dergelijke wijzigingen beveiligen.  Twee krachtige beveiligingsfuncties door Azure Resource Manager worden toegepast: [toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) en [resource vergrendelingen](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure op rollen gebaseerde toegangsbeheer (RBAC) kunt Geavanceerd toegangsbeheer voor Azure-gebruikers, groepen en bronnen. Met RBAC kunt verleent u nauwkeurig de mate van toegang dat gebruikers moeten uitvoeren van hun taken. Zie voor meer informatie over hoe RBAC helpt u bij het beheren van toegang [wat toegangsbeheer op basis van rollen is](../active-directory/role-based-access-control-what-is.md).

### <a name="the-dns-zone-contributor-role"></a>De rol 'DNS-Zone Inzender'

De rol 'DNS-Zone Inzender' is een ingebouwde rol die is geleverd door Azure voor het beheren van DNS-resources.  DNS-Zone Inzender-rechten toewijzen aan een gebruiker of groep, kunt die groep voor het beheren van DNS-resources, maar niet de resources van een ander type.

Stel bijvoorbeeld dat de resource-groep 'myzones' bevat vijf zones voor Contoso Corporation. De DNS-beheerder 'DNS-Zone Inzender' machtigingen verlenen aan die resourcegroep, kunt volledige controle over de DNS-zones. Dit voorkomt ook onnodige machtigingen verlenen bijvoorbeeld de DNS-beheerder niet maken of virtuele Machines stoppen.

De eenvoudigste manier om RBAC machtigingen toewijzen is [via de Azure-portal](../active-directory/role-based-access-control-configure.md).  Open de blade Access control (IAM) voor de resourcegroep, klik op 'Add', en vervolgens selecteert u de rol 'DNS-Zone Inzender' en selecteer de vereiste gebruikers of groepen om machtigingen te verlenen.

![Niveau van de resourcegroep RBAC via de Azure-portal](./media/dns-protect-zones-recordsets/rbac1.png)

Machtigingen kunnen ook worden [verleend met behulp van Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

De equivalente-opdracht is ook [beschikbaar via de Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Zone-niveau RBAC

Azure RBAC-regels kunnen worden toegepast op een abonnement, met een resourcegroep of op een afzonderlijke resource. In het geval van Azure DNS is die bron een afzonderlijke DNS-zone of een afzonderlijke Recordset.

Stel bijvoorbeeld dat de resource-groep 'myzones' bevat de zone contoso.com en een subzone 'customers.contoso.com' waarin de CNAME-records die zijn gemaakt voor elk klantaccount.  Het account dat wordt gebruikt voor het beheren van deze CNAME-records moet machtigingen voor het maken van records in de zone 'customers.contoso.com' alleen worden toegewezen, moet deze geen toegang hebben tot de andere zones.

Zoneniveau RBAC machtigingen kunnen worden verleend via de Azure-portal.  Open de blade Access control (IAM) voor de zone, klik op 'Add', en vervolgens selecteert u de rol 'DNS-Zone Inzender' en selecteer de vereiste gebruikers of groepen om machtigingen te verlenen.

![DNS-Zone niveau RBAC via de Azure-portal](./media/dns-protect-zones-recordsets/rbac2.png)

Machtigingen kunnen ook worden [verleend met behulp van Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

De equivalente-opdracht is ook [beschikbaar via de Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Record RBAC-niveau instellen

We kunt nog een stapje verder gaan. Houd rekening met het e-beheerder voor Contoso Corporation wie toegang tot de MX en TXT-record in de apex van de zone 'contoso.com heeft'.  Ze hoeft niet de toegang tot andere MX- of TXT records of aan een record van een ander type.  Azure DNS kunt u machtigingen op het niveau van de recordset toewijzen aan nauwkeurig de records die de e-beheerder toegang tot nodig.  De mailbeheerder krijgt nauwkeurig het besturingselement ze nodig heeft, en is geen andere wijzigingen aanbrengen.

Record-set RBAC machtigingen kunnen worden geconfigureerd via de Azure-portal met de knop 'Gebruikers' in de blade Recordset:

![De recordset niveau RBAC via de Azure-portal](./media/dns-protect-zones-recordsets/rbac3.png)

Record-set machtigingen op gebruikersniveau RBAC kunnen ook worden [verleend met behulp van Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant permissions to a specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

De equivalente-opdracht is ook [beschikbaar via de Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Aangepaste rollen

De ingebouwde 'DNS-Zone Inzender'-rol kunnen volledige controle over een DNS-bronrecords. Het is ook mogelijk te maken van uw eigen klant Azure rollen, zodat zelfs nauwkeurigere besturing.

Bekijk opnieuw de voorbeeld waarin een CNAME-record in de zone 'customers.contoso.com' is gemaakt voor elk account van de klant Contoso Corporation.  Het account dat wordt gebruikt voor het beheren van deze CNAME-records moet worden gemachtigd voor CNAME-records beheren.  Het is geen records van andere typen (zoals het wijzigen van een MX-record) wijzigen of zoneniveau bewerkingen zoals zone verwijderen.

Het volgende voorbeeld ziet u een aangepaste roldefinitie voor het beheren van CNAME-records:

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/ c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

De eigenschap Actions definieert de volgende DNS-specifieke machtigingen:

* `Microsoft.Network/dnsZones/CNAME/*`Hiermee wordt volledige controle over de CNAME-records
* `Microsoft.Network/dnsZones/read`Geeft het recht om te lezen van de DNS-zones, maar niet kunnen wijzigen, zodat u voor een overzicht van de zone waarin de CNAME wordt gemaakt.

De resterende acties worden gekopieerd van de [ingebouwde rol van inzender van DNS-Zone](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Met behulp van een aangepaste RBAC-rol om te voorkomen dat recordsets verwijderen terwijl u nog steeds zodat ze kunnen worden bijgewerkt, is niet een doeltreffende controle. Deze voorkomt dat recordsets worden verwijderd, maar dit voorkomt niet dat deze wordt gewijzigd.  Toegestane wijzigingen omvatten het toevoegen en verwijderen van records uit de recordset, met inbegrip van alle records te laten een 'empty' Recordset verwijderen. Dit heeft hetzelfde effect als het verwijderen van de record is ingesteld op basis van een DNS-omzetting oogpunt.

Aangepaste roldefinities kunnen niet op dit moment worden gedefinieerd via de Azure-portal. Een aangepaste rol die is gebaseerd op de roldefinitie van deze kan worden gemaakt met Azure PowerShell:

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

Het kan ook worden gemaakt via de Azure CLI:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

De rol kan vervolgens worden toegewezen op dezelfde manier als de ingebouwde rollen, zoals eerder in dit artikel wordt beschreven.

Zie voor meer informatie over het maken, beheren en aangepaste rollen toewijzen [aangepaste rollen in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).

## <a name="resource-locks"></a>Resource-vergrendelingen

Naast het RBAC, Azure Resource Manager biedt ondersteuning voor een ander type beveiligingscontrole, namelijk de mogelijkheid tot vergrendelingsbronnen ''. Waar RBAC regels kunnen u bepalen de acties van specifieke gebruikers en groepen resource vergrendelingen worden toegepast op de bron en gelden voor alle gebruikers en rollen. Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).

Er zijn twee soorten resource vergrendeling: **DoNotDelete** en **ReadOnly**. Deze kunnen worden toegepast op een DNS-zone of naar een afzonderlijke Recordset.  De volgende secties worden enkele algemene scenario's en hoe om ze te ondersteunen het gebruik van resource vergrendelingen.

### <a name="protecting-against-all-changes"></a>Bescherming tegen alle wijzigingen

Om te voorkomen dat eventuele wijzigingen, een alleen-lezen-vergrendeling van toepassing op de zone.  Dit voorkomt dat nieuwe recordsets wordt gemaakt en bestaande recordsets worden gewijzigd of verwijderd.

Zone niveau resource vergrendelingen kunnen worden gemaakt via de Azure-portal.  Klik op 'Vergrendelingen' van de DNS-zone-blade vervolgens 'Add':

![Zone niveau resource vergrendelingen via de Azure-portal](./media/dns-protect-zones-recordsets/locks1.png)

Zoneniveau resource vergrendelingen kunnen ook worden gemaakt via Azure PowerShell:

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Configureren van Azure-resource vergrendeld is momenteel niet ondersteund via de Azure CLI.

### <a name="protecting-individual-records"></a>Afzonderlijke records beveiligen

Om te voorkomen dat een bestaande DNS-record instellen tegen wijziging, een alleen-lezen-vergrendeling van toepassing op de recordset.

> [!NOTE]
> Een vergrendeling DoNotDelete toepassen op een recordset is niet een doeltreffende controle. Deze voorkomt dat de recordset worden verwijderd, maar dit voorkomt niet dat deze wordt gewijzigd.  Toegestane wijzigingen omvatten het toevoegen en verwijderen van records uit de recordset, met inbegrip van alle records te laten een 'empty' Recordset verwijderen. Dit heeft hetzelfde effect als het verwijderen van de record is ingesteld op basis van een DNS-omzetting oogpunt.

Recordset niveau resource vergrendelingen kunnen momenteel alleen worden geconfigureerd met behulp van Azure PowerShell.  Ze worden niet ondersteund in de Azure-portal of Azure CLI.

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Bescherming tegen zone verwijderen

Wanneer een zone in Azure DNS wordt verwijderd, worden ook alle recordsets in de zone verwijderd.  Deze bewerking kan niet ongedaan worden gemaakt.  Per ongeluk verwijderen van een kritieke zone heeft kunnen een aanzienlijke zakelijke invloed.  Het is daarom belangrijk om te beschermen tegen onbedoeld zone verwijderen.

Toepassen van een vergrendeling DoNotDelete aan een zone wordt voorkomen dat de zone wordt verwijderd.  Echter, omdat de vergrendelingen worden overgenomen door onderliggende resources, voorkomt u tevens dat alle recordsets in de zone worden verwijderd, die mogelijk ongewenste.  Bovendien zoals beschreven in de bovenstaande opmerking, is het ook ongeschikt Aangezien records kunnen nog steeds worden verwijderd uit de bestaande recordsets.

Een alternatief kunt u beter een vergrendeling DoNotDelete toepassen op een record in de zone, zoals de SOA-Recordset.  Dit biedt bescherming tegen verwijderen van een zone, terwijl u nog steeds recordsets binnen de zone vrijelijk worden gewijzigd omdat de zone kan niet worden verwijderd zonder de recordsets ook te verwijderen. Als wordt geprobeerd om de zone te verwijderen, detecteert de Azure Resource Manager dit zou de SOA-Recordset ook verwijderd en wordt de aanroep geblokkeerd omdat de SOA is vergrendeld.  Er is geen recordsets worden verwijderd.

De volgende PowerShell-opdracht maakt een vergrendeling DoNotDelete tegen de SOA-record van de opgegeven zone:

```powershell
# Protect against zone delete with DoNotDelete lock on the record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Een andere manier om te voorkomen dat per ongeluk zone verwijderen met behulp van een aangepaste beveiligingsrol om te controleren of de operator is en serviceaccounts gebruikt voor het beheren van uw zones hebben geen zone verwijderen van machtigingen. Wanneer u een zone verwijderen hoeft, kunt u in twee stappen verwijderen, eerste verlenen zone machtigingen voor het verwijderen (op het bereik zone om te voorkomen dat u de verkeerde zone verwijdert) en seconde de zone verwijderen afdwingen.

Deze tweede methode heeft het voordeel die geschikt is voor alle zones die toegankelijk is voor deze accounts zonder te hoeven onthouden alle vergrendelingen te maken. Het nadeel dat alle accounts met machtigingen op zone verwijderen, zoals de eigenaar van het abonnement nog steeds per ongeluk een kritieke zone kunnen verwijderen heeft.

Het is mogelijk gebruik van beide benaderingen - resource vergrendelingen en aangepaste rollen - tegelijkertijd wordt als een benadering defense-in-depth van DNS-zone-beveiliging.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het werken met RBAC [aan de slag met toegangsbeheer in Azure portal](../active-directory/role-based-access-control-what-is.md).
* Zie voor meer informatie over het werken met resource vergrendelingen [resources met Azure Resource Manager vergrendelen](../azure-resource-manager/resource-group-lock-resources.md).

