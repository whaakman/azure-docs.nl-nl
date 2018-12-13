---
title: Azure DNS-Zones en Records beschermen
description: Het beveiligen van DNS-zones en -recordsets in Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: 137d8e1c1477d5b9c88cecc39316d62a79a4cab8
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873916"
---
# <a name="how-to-protect-dns-zones-and-records"></a>DNS-zones en records beschermen

DNS-zones en records zijn kritieke bronnen. Verwijderen van een DNS-zone of zelfs één DNS-record kan resulteren in een totaal van de serviceonderbreking.  Het is daarom belangrijk dat kritieke DNS-zones en records zijn beschermd tegen onbevoegde of onopzettelijke wijzigingen.

In dit artikel wordt uitgelegd hoe Azure DNS kunt u uw DNS-zones en records tegen dergelijke wijzigingen beveiligen.  We twee krachtige beveiligingsfuncties die worden geleverd door Azure Resource Manager toepassen: [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) en [resourcevergrendelingen](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure Role-Based Access Control (RBAC) kunt over Geavanceerd toegangsbeheer voor Azure-gebruikers, groepen en resources. Met RBAC, verleent u nauwkeurig de mate van toegang dat gebruikers moeten hun werk. Zie voor meer informatie over hoe met RBAC kunt u bij het beheren van toegang, [wat is toegangsbeheer op basis van rollen](../role-based-access-control/overview.md).

### <a name="the-dns-zone-contributor-role"></a>De rol Inzender voor DNS-Zone

De rol Inzender voor DNS-Zone is een ingebouwde rol die is geleverd door Azure voor het beheren van DNS-resources.  DNS-Zone Inzender-rechten toewijzen aan een gebruiker of groep, kunt deze groep voor het beheren van DNS-resources, maar niet de resources van een ander type.

Stel bijvoorbeeld dat de resourcegroep *myzones* bevat vijf zones voor Contoso Corporation. De DNS-beheerder DNS-Zone Inzender machtigingen verlenen aan deze resourcegroep, kunt volledige controle over de DNS-zones. Dit voorkomt ook het verlenen van onnodige machtigingen, bijvoorbeeld de DNS-beheerder kan niet maken of stoppen van virtuele Machines.

De eenvoudigste manier om toe te wijzen RBAC-machtigingen is [via Azure portal](../role-based-access-control/role-assignments-portal.md).  Open **toegangsbeheer (IAM)** voor de resourcegroep, selecteer **toevoegen**en selecteer vervolgens de **DNS-Zone Inzender** rol en selecteer de vereiste gebruikers of groepen te verlenen machtigingen.

![Niveau van de resourcegroep RBAC via Azure portal](./media/dns-protect-zones-recordsets/rbac1.png)

Machtigingen kunnen ook worden [verleend met behulp van Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

De equivalente-opdracht is ook [beschikbaar via de Azure CLI](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Zone-niveau RBAC

Azure RBAC-regels kunnen worden toegepast op een abonnement, een resourcegroep of op een afzonderlijke resource. In het geval van Azure DNS, kan deze resource een afzonderlijke DNS-zone of een afzonderlijke Recordset zijn.

Stel bijvoorbeeld dat de resourcegroep *myzones* bevat de zone *contoso.com* en een subzone *customers.contoso.com* in welke CNAME-records zijn gemaakt voor elk account van de klant.  Het account dat wordt gebruikt voor het beheren van deze CNAME-records moet worden toegewezen machtigingen voor het maken van records in de *customers.contoso.com* alleen de zones, moet deze geen toegang hebben tot de andere zones.

Zone-niveau RBAC-machtigingen kunnen worden verleend via de Azure-portal.  Open **toegangsbeheer (IAM)** voor de zone, selecteert u vervolgens **toevoegen**en selecteer vervolgens de **DNS-Zone Inzender** rol en selecteer de vereiste gebruikers of groepen om machtigingen te verlenen.

![DNS-Zone niveau RBAC via Azure portal](./media/dns-protect-zones-recordsets/rbac2.png)

Machtigingen kunnen ook worden [verleend met behulp van Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

De equivalente-opdracht is ook [beschikbaar via de Azure CLI](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Het niveau van RBAC van de recordset

We kunt een stap verder gaan. Houd rekening met de e mailbeheerder voor Contoso Corporation, die toegang nodig tot de MX- en TXT-records in de apex van de zone contoso.com heeft.  Ze hoeft niet de toegang tot alle andere MX of TXT-records, of alle records van een ander type.  Azure DNS kunt u machtigingen op het niveau van de recordset, toewijzen aan nauwkeurig de records die de beheerder van de e-mail toegang hebben tot moet.  De e mailbeheerder krijgt precies het besturingselement ze nodig heeft en is geen andere wijzigingen aanbrengen.

Record-set niveau RBAC-machtigingen kunnen worden geconfigureerd via de Azure-portal met behulp van de **gebruikers** knop op de pagina Recordset:

![Het niveau van RBAC via de Azure-portal van de recordset](./media/dns-protect-zones-recordsets/rbac3.png)

Record-set niveau RBAC-machtigingen kunnen ook worden [verleend met behulp van Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell
# Grant permissions to a specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

De equivalente-opdracht is ook [beschikbaar via de Azure CLI](../role-based-access-control/role-assignments-cli.md):

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Aangepaste rollen

De ingebouwde rol van de DNS-Zone Inzender kunt volledige controle over een DNS-bronrecords. Het is ook uw eigen klanten Azure-rollen, voor nog nauwkeurigere besturing mogelijk maken.

Bekijk nogmaals het voorbeeld waarin een CNAME opnemen in de zone *customers.contoso.com* voor elk account van de klant Contoso Corporation is gemaakt.  Het account dat wordt gebruikt voor het beheren van deze CNAME-records moet worden verleend als machtiging voor het beheren van CNAME-records.  Is deze niet wijzigt u records van andere typen (zoals het wijzigen van een MX-record) of zone-niveau bewerkingen uitvoeren zoals zone verwijderen.

Het volgende voorbeeld ziet de definitie van een aangepaste rol voor het beheren van CNAME-records:

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

* `Microsoft.Network/dnsZones/CNAME/*` Hiermee wordt volledige controle over CNAME-records
* `Microsoft.Network/dnsZones/read` verleent toestemming om te lezen van de DNS-zones, maar niet kunnen wijzigen, zodat u kunt om te zien van de zone waarin de CNAME wordt gemaakt.

De resterende acties worden gekopieerd van de [ingebouwde rol Inzender voor DNS-Zone](../role-based-access-control/built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Met behulp van een aangepaste RBAC-rol om te voorkomen dat recordsets verwijderen terwijl nog steeds zodat ze kunnen worden bijgewerkt niet een doeltreffende controle is. Dit voorkomt dat recordsets worden verwijderd, maar voorkomt niet dat deze wordt gewijzigd.  Toegestane wijzigingen bevatten toevoegen en verwijderen van records van de recordset, met inbegrip van alle records als u wilt een lege recordset laten verwijderen. Dit heeft hetzelfde effect als de recordset van een DNS-omzetting viewpoint verwijderen.

Aangepaste roldefinities kunnen niet op dit moment worden gedefinieerd via de Azure-portal. Een aangepaste rol op basis van de roldefinitie van deze kan worden gemaakt met behulp van Azure PowerShell:

```azurepowershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

Het kan ook worden gemaakt via de Azure CLI:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

De rol kan vervolgens worden toegewezen op dezelfde manier als ingebouwde rollen, zoals eerder in dit artikel beschreven.

Zie voor meer informatie over het maken, beheren en aangepaste rollen toewijzen [aangepaste rollen in Azure RBAC](../role-based-access-control/custom-roles.md).

## <a name="resource-locks"></a>Resourcevergrendelingen

Naast het RBAC, Azure Resource Manager biedt ondersteuning voor een ander type beveiligingscontrole, namelijk de mogelijkheid om resources vergrendelen. Waar RBAC regels kunnen u om te bepalen de acties van specifieke gebruikers en groepen, resourcevergrendelingen worden toegepast op de resource en gelden voor alle gebruikers en rollen. Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).

Er zijn twee soorten resourcevergrendeling: **CanNotDelete** en **ReadOnly**. Deze kunnen worden toegepast op een DNS-zone of aan een afzonderlijke record-set.  De volgende secties worden enkele algemene scenario's, en hoe u om ze te ondersteunen met behulp van de resource wordt vergrendeld.

### <a name="protecting-against-all-changes"></a>Bescherming tegen alle wijzigingen

Om te voorkomen dat alle wijzigingen, een alleen-lezenvergrendeling van toepassing op de zone.  Dit voorkomt dat nieuwe recordsets worden gemaakt en worden bestaande recordsets worden gewijzigd of verwijderd.

Zone-niveau resourcevergrendelingen kunnen worden gemaakt via Azure portal.  Selecteer in de pagina DNS-zone **Hiermee vergrendelt u**en selecteer vervolgens **+ toevoegen**:

![Zone niveau resourcevergrendelingen via Azure portal](./media/dns-protect-zones-recordsets/locks1.png)

Zone-niveau resource wordt vergrendeld kunnen ook worden gemaakt via Azure PowerShell:

```azurepowershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Configureren van Azure-resourcevergrendelingen wordt momenteel niet ondersteund via de Azure CLI.

### <a name="protecting-individual-records"></a>Afzonderlijke records beschermen

Om te voorkomen dat een bestaande DNS-record ingesteld op basis van de wijziging, een alleen-lezenvergrendeling van toepassing op de recordset.

> [!NOTE]
> Een CanNotDelete-vergrendeling toepassen op een recordset is niet een doeltreffende controle. Hiermee voorkomt u dat de recordset worden verwijderd, maar voorkomt niet dat deze wordt gewijzigd.  Toegestane wijzigingen bevatten toevoegen en verwijderen van records van de recordset, met inbegrip van alle records als u wilt een lege recordset laten verwijderen. Dit heeft hetzelfde effect als de recordset van een DNS-omzetting viewpoint verwijderen.

Recordset niveau resourcevergrendelingen op dit moment kunnen alleen worden geconfigureerd met behulp van Azure PowerShell.  Ze worden niet ondersteund in de Azure portal of Azure CLI.

```azurepowershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Bescherming tegen zone verwijderen

Wanneer een zone in Azure DNS wordt verwijderd, worden alle recordsets in de zone worden ook verwijderd.  Deze bewerking kan niet ongedaan worden gemaakt.  Per ongeluk verwijderen van een zone voor kritieke in potentie een grote bedrijfsimpact hebben.  Daarom is het belangrijk om te beveiligen tegen onopzettelijk zone verwijderen.

Een CanNotDelete-vergrendeling toepassen op een zone wordt voorkomen dat de zone wordt verwijderd.  Echter omdat vergrendelingen worden overgenomen door onderliggende resources, voorkomt u tevens dat alle recordsets in de zone worden verwijderd, wat mogelijk niet gewenst.  Bovendien zoals beschreven in de bovenstaande opmerking, is het ook niet effectief Aangezien records kunnen nog steeds worden verwijderd uit de bestaande recordsets.

Als alternatief kunt u een CanNotDelete-vergrendeling toepassen op een record in de zone, zoals de SOA-record-set.  Dit biedt bescherming tegen verwijderen van zones, terwijl u nog steeds recordsets in de zone vrijelijk kan worden gewijzigd omdat de zone kan niet worden verwijderd zonder de recordsets ook verwijderen. Als is geprobeerd om de zone te verwijderen, wordt Azure Resource Manager detecteert dit ook de SOA-recordset wilt verwijderen, en wordt de aanroep geblokkeerd omdat de SOA is vergrendeld.  Er is geen recordsets zijn verwijderd.

De volgende PowerShell-opdracht maakt een CanNotDelete-vergrendeling op basis van de SOA-record van de opgegeven zone:

```azurepowershell
# Protect against zone delete with CanNotDelete lock on the record set
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Een andere manier om te voorkomen dat onbedoelde zone verwijderen met behulp van een aangepaste rol om te controleren of de operator is en serviceaccounts die worden gebruikt voor het beheren van de zones nog geen zone machtigingen verwijderen. Als u een zone verwijderen wilt, kunt u het verwijderen van een verificatie in twee stappen, eerste verlenen zone verwijdermachtigingen ({bij de scope om te voorkomen dat u de verkeerde zone verwijdert) en seconde als u wilt verwijderen van de zone afdwingen.

Deze tweede methode heeft het voordeel dat de Tool voor alle zones die toegankelijk is voor die accounts zonder te hoeven werkt te onthouden om te maken van een wordt vergrendeld. Heeft het nadeel is dat alle accounts met machtigingen voor zone verwijderen, zoals de eigenaar van het abonnement nog steeds per ongeluk een belangrijke zone kunnen verwijderen.

Het is mogelijk met gebruik van beide benaderingen - resourcevergrendelingen en aangepaste rollen - op hetzelfde moment als een ingrijpende aanpak van DNS-zone gegevensbeveiliging.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het werken met RBAC [aan de slag met toegangsbeheer in Azure portal](../role-based-access-control/overview.md).
* Zie voor meer informatie over het werken met resourcevergrendelingen [resources met Azure Resource Manager vergrendelen](../azure-resource-manager/resource-group-lock-resources.md).