---
title: Uw domein delegeren naar Azure DNS | Microsoft Docs
description: Lees hoe u de domeindelegering wijzigt en DNS-naamservers kunt gebruiken om domeinen te hosten.
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: 33b3ec24432ff1268860b9a2e9d5098600a8dedc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="delegate-a-domain-to-azure-dns"></a>Een domein delegeren naar Azure DNS

Met Azure DNS kunt u een DNS-zone hosten en de DNS-records voor een domein in Azure beheren. Het domein moet vanaf het bovenliggende domein worden gedelegeerd naar Azure DNS om ervoor te zorgen dat DNS-query's voor een domein Azure DNS bereiken. Houd er rekening mee Azure DNS is niet de domeinregistrar. In dit artikel wordt uitgelegd hoe u uw domein delegeert naar Azure DNS.

Voor domeinen die bij een registrar worden gekocht, zal de registrar u de optie bieden om deze NS-records in te stellen. U hoeft niet de eigenaar van een domeinnaam te zijn om een DNS-zone met die domeinnaam in Azure DNS te maken. U hoeft echter geen eigenaar van het domein te zijn om de delegering naar Azure DNS in te stellen voor de registrar.

Stel dat u het domein contoso.net koopt en een zone met de naam contoso.net in Azure DNS maakt. Als eigenaar van het domein zal uw registrar u de optie bieden om de adressen van de naamserver (oftewel de NS-records) te configureren voor uw domein. De registrar slaat deze NS-records op in het bovenliggende domein, in dit geval .net. Clients over de hele wereld kunnen dan naar uw domein in de Azure DNS-zone worden omgeleid wanneer ze DNS-records omzetten in contoso.net.

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

1. Aanmelden bij Azure Portal
1. Klik in het menu Hub en klik op **Nieuw > Netwerken >** en klik vervolgens op **DNS-zone** om de blade DNS-zone maken te openen.

    ![DNS-zone](./media/dns-domain-delegation/dns.png)

1. Voer op de blade **DNS-zone maken** de volgende waarden in en klik op **Maken**:

   | **Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|contoso.net|De naam van de DNS-zone|
   |**Abonnement**|[Uw abonnement]|Selecteer het abonnement waarin u de toepassingsgateway wilt maken.|
   |**Resourcegroep**|**Nieuwe maken:** contosoRG|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
   |**Locatie**|VS - west||

> [!NOTE]
> De resourcegroep verwijst naar de locatie van de resourcegroep en heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

## <a name="retrieve-name-servers"></a>Naamservers ophalen

Voordat u uw DNS-zone naar Azure DNS kunt delegeren, moet u eerst de servernamen voor uw zone weten. Telkens wanneer er een zone wordt gemaakt, wijst Azure DNS naamservers uit een groep toe.

1. Nu de DNS-zone is gemaakt, klikt u in het deelvenster **Favorieten** van Azure Portal op **Alle resources**. Klik op de blade **Alle resources** op de DNS-zone **contoso.net**. Als het abonnement dat u hebt geselecteerd, al verschillende resources heeft, kunt u **contoso.net** invoeren in het vak Filteren op naam… voor eenvoudige toegang tot de toepassingsgateway. 

1. U vindt de naamservers op de blade DNS-zone. In dit voorbeeld zijn de naamservers ns1-01.azure-dns.com, ns2 01.azure dns.net, ns3-01.azure-dns.org en ns4-01.azure-dns.info aan de zone contoso.net toegewezen:

 ![DNS-naamserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS maakt automatisch gezaghebbende NS-records in uw zone die de toegewezen naamservers bevatten.  Als u de namen van de naamservers wilt weergeven via Azure PowerShell of Azure CLI, hoeft u deze records alleen maar op te halen.

De volgende voorbeelden geven ook de stappen voor het ophalen van naamservers voor een zone in Azure DNS met PowerShell en Azure CLI.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Het volgende voorbeeld is het antwoord.

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

Het volgende voorbeeld is het antwoord.

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-the-domain"></a>Het domein delegeren

Nu de DNS-zone is gemaakt en u de naamservers hebt, moet het bovenliggende domein worden bijgewerkt met de Azure DNS-naamservers. Elke registrar heeft zijn eigen hulpprogramma's voor DNS-beheer om de naamserverrecords voor een domein te wijzigen. Ga naar de DNS-beheerpagina van de registrar, bewerk de NS-records en vervang de NS-records door de records die door Azure DNS zijn gemaakt.

Wanneer u een domein naar Azure DNS delegeert, moet u de naamservernamen gebruiken die zijn verstrekt door Azure DNS. Het is raadzaam om altijd alle vier de naamservernamen te gebruiken, ongeacht de naam van uw domein. Domeindelegatie vereist niet dat de naamservernaam hetzelfde domein op het hoogste niveau gebruikt als uw domein.

U moet niet glue records gebruiken om naar het IP-adres van de Azure DNS-naamserver te wijzen, aangezien deze IP-adressen in de toekomst kunnen worden gewijzigd. Delegeringen die gebruikmaken van de naamservernamen in uw eigen zone, ook wel vanity naamservers genoemd, worden momenteel niet ondersteund in Azure DNS.

## <a name="verify-name-resolution-is-working"></a>Controleren of de naamomzetting werkt

Nadat het delegeren is voltooid, kunt u controleren of de naamomzetting werkt door een hulpprogramma zoals nslookup te gebruiken om een query op de SOA-record voor uw zone uit te voeren (die ook automatisch wordt gemaakt wanneer de zone wordt gemaakt).

Als de delegering goed is ingesteld, hoeft u de Azure DNS-naamservers niet op te geven. Het gangbare DNS-omzettingsproces zorgt er dan voor dat de naamservers automatisch worden gevonden.

```
nslookup -type=SOA contoso.com
```

Hier volgt een voorbeeld van de reactie op de voorgaande opdracht:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a>Subdomeinen delegeren in Azure DNS

Als u een afzonderlijke onderliggende zone wilt instellen, kunt u een subdomein delegeren in Azure DNS. Stel dat u contoso.net hebt ingesteld en gedelegeerd in Azure DNS en vervolgens een afzonderlijke onderliggende zone, partners.contoso.net, wilt instellen.

1. Maak de onderliggende zone partners.contoso.net in Azure DNS.
2. Zoek de gezaghebbende NS-records in de onderliggende zone om de naamservers op te halen die de onderliggende zone in Azure DNS hosten.
3. Delegeer de onderliggende zone door NS-records in de bovenliggende te configureren die wijzen naar de onderliggende zone.

### <a name="create-a-dns-zone"></a>Een DNS-zone maken

1. Aanmelden bij Azure Portal
1. Klik in het menu Hub en klik op **Nieuw > Netwerken >** en klik vervolgens op **DNS-zone** om de blade DNS-zone maken te openen.

    ![DNS-zone](./media/dns-domain-delegation/dns.png)

1. Voer op de blade **DNS-zone maken** de volgende waarden in en klik op **Maken**:

   | **Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|partners.contoso.net|De naam van de DNS-zone|
   |**Abonnement**|[Uw abonnement]|Selecteer het abonnement waarin u de toepassingsgateway wilt maken.|
   |**Resourcegroep**|**Bestaande gebruiken:** contosoRG|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
   |**Locatie**|VS - west||

> [!NOTE]
> De resourcegroep verwijst naar de locatie van de resourcegroep en heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

### <a name="retrieve-name-servers"></a>Naamservers ophalen

1. Nu de DNS-zone is gemaakt, klikt u in het deelvenster **Favorieten** van Azure Portal op **Alle resources**. Klik op de blade **Alle resources** op de DNS-zone **partners.contoso.net**. Als het abonnement dat u hebt geselecteerd, al verschillende resources heeft, kunt u **partners.contoso.net** invoeren in het vak Filteren op naam… voor eenvoudige toegang tot de DNS-zone.

1. U vindt de naamservers op de blade DNS-zone. In dit voorbeeld zijn de naamservers ns1-01.azure-dns.com, ns2 01.azure dns.net, ns3-01.azure-dns.org en ns4-01.azure-dns.info aan de zone contoso.net toegewezen:

 ![DNS-naamserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS maakt automatisch gezaghebbende NS-records in uw zone die de toegewezen naamservers bevatten.  Als u de namen van de naamservers wilt weergeven via Azure PowerShell of Azure CLI, hoeft u deze records alleen maar op te halen.

### <a name="create-name-server-record-in-parent-zone"></a>Een naamserverrecord maken in de bovenliggende zone

1. Navigeer in Azure Portal naar de DNS-zone **contoso.net**.
1. Klik op **Recordset toevoegen**
1. Voer op de blade **Recordset toevoegen** de volgende waarden in en klik op **OK**:

   | **Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|partners|De naam van de onderliggende DNS-zone|
   |**Type**|NS|Gebruik NS voor naamserverrecords.|
   |**TTL**|1|Time to live.|
   |**TTL-eenheid**|Uren|hiermee stelt u de Time to Live in op uren|
   |**NAAMSERVER**|{naamservers van zone partners.contoso.net}|Voer de vier naamservers van de zone partners.contoso.net in. |

   ![DNS-naamserver](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Subdomeinen in Azure DNS delegeren met andere hulpprogramma’s

De volgende voorbeelden geven de stappen voor het delegeren van subdomeinen in Azure DNS met PowerShell en CLI:

#### <a name="powershell"></a>PowerShell

In het volgende PowerShell-voorbeeld kunt u zien hoe dit werkt. Dezelfde stappen kunnen ook worden uitgevoerd via Azure Portal of via de platformoverschrijdende CLI van Azure.

```powershell
# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Gebruik `nslookup` om te controleren of alles juist is ingesteld, door de SOA-record van de onderliggende zone te zoeken.

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Haal de naamservers voor de zone `partners.contoso.net` op uit de uitvoer.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Maak de recordset en NS-records voor elke naamserver.

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

1. Klik in het deelvenster **Favorieten** van Azure Portal op **Alle resources**. Klik op de blade Alle resources op de resourcegroep **contosorg**. Als het abonnement dat u hebt geselecteerd, al verschillende resources heeft, kunt u **contosorg** invoeren in het vak **Filteren op naam...** voor eenvoudige toegang tot de resourcegroep.
1. Klik op de blade **contosorg** op de knop **Verwijderen**.
1. De portal vereist dat u de naam van de resourcegroep typt om te bevestigen dat u deze wilt verwijderen. Typ *contosorg* als naam van de resourcegroep en klik op **Verwijderen**. Als u een resourcegroep verwijdert, worden alle resources binnen de resourcegroep verwijderd. Zorg er dus altijd voor dat u de inhoud van een resourcegroep bevestigt, voordat u deze verwijdert. Alle resources die zich in de resourcegroep bevinden en de resourcegroep zelf worden verwijderd. Dit proces duurt enkele minuten.

## <a name="next-steps"></a>Volgende stappen

[DNS-zones beheren](dns-operations-dnszones.md)

[DNS-records beheren](dns-operations-recordsets.md)
