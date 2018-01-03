---
title: DNS-records in Azure DNS met Azure PowerShell beheren | Microsoft Docs
description: Het beheren van DNS-recordsets en records op Azure DNS bij het hosten van uw Azure DNS-domein. Alle PowerShell-opdrachten voor bewerkingen op recordsets en records.
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: fee96a77436f09e5cf2841b36b244e2d03f57f74
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>DNS-records en recordsets in Azure DNS met Azure PowerShell beheren

> [!div class="op_single_selector"]
> * [Azure-portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

In dit artikel laat zien hoe DNS-records voor de DNS-zone beheren met behulp van Azure PowerShell. DNS-records kunnen ook worden beheerd met behulp van de platformoverschrijdende [Azure CLI](dns-operations-recordsets-cli.md) of de [Azure-portal](dns-operations-recordsets-portal.md).

De voorbeelden in dit artikel wordt ervan uitgegaan dat u al hebt [Azure PowerShell aangemeld, geïnstalleerd en wordt gemaakt van een DNS-zone](dns-operations-dnszones.md).

## <a name="introduction"></a>Inleiding

Voordat u DNS-records in DNS Azure maakt, leest u eerst hoe Azure DNS DNS-records organiseert in DNS-recordsets.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Zie [DNS-zones en -records](dns-zones-records.md) voor meer informatie over DNS-records in Azure DNS.


## <a name="create-a-new-dns-record"></a>Maak een nieuwe DNS-record

Als uw nieuwe record dezelfde naam en hetzelfde type als een bestaande record heeft, moet u [toe te voegen aan de bestaande recordset](#add-a-record-to-an-existing-record-set). Als uw nieuwe record een andere naam en type alle bestaande records heeft, moet u een nieuwe recordset maken. 

### <a name="create-a-records-in-a-new-record-set"></a>"A" records in een nieuwe recordset maken

U kunt recordsets maken met behulp van de cmdlet `New-AzureRmDnsRecordSet`. Wanneer u een recordset, moet u aangeven dat de record naam, de zone, de tijd ingesteld op live (TTL), het recordtype en de records die moeten worden gemaakt.

De parameters voor het toevoegen van records aan een recordset variëren afhankelijk van het type recordset. Bijvoorbeeld, wanneer u een recordset van het type "A", moet u het IP-adres met de parameter opgeven `-IPv4Address`. Andere parameters worden gebruikt voor andere typen records. Zie [voorbeelden van recordtypen aanvullende](#additional-record-type-examples) voor meer informatie.

Het volgende voorbeeld maakt een recordset met de relatieve naam 'www' in de DNS-Zone 'contoso.com'. De volledig gekwalificeerde naam van de recordset is 'www.contoso.com'. Het recordtype is "A" en de TTL 3600 seconden. De recordset bevat één record, met IP-adres '1.2.3.4'.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Maken van een recordset in de apex van een zone (in dit geval 'contoso.com'), gebruikt u de naam van de recordset ' @' (zonder aanhalingstekens):

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Als u een recordset met meer dan één record maken wilt, maakt eerst een lokale matrix en de records toevoegen en de matrix te geven `New-AzureRmDnsRecordSet` als volgt:

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[Metagegevens van de recordset](dns-zones-records.md#tags-and-metadata) kan worden gebruikt om toepassingsspecifieke gegevens koppelen aan elke recordset als sleutel-waardeparen. Het volgende voorbeeld ziet u hoe u een recordset met twee metagegevensvermeldingen maken ' afdeling Financiën =' en ' omgeving productie ='.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Azure DNS ondersteunt ook 'empty' recordsets als een tijdelijke aanduiding voor een DNS-naam reserveren fungeren kunnen voordat u DNS-records maakt. Lege recordsets zijn zichtbaar in het vlak van Azure DNS-beheer, maar worden weergegeven op de Azure DNS-naamservers. Het volgende voorbeeld wordt een lege recordset gemaakt:

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Records van andere typen maken

Hebben gezien in detail "A" records maken, de volgende voorbeelden laten zien hoe andere recordtypen ondersteund door Azure DNS-records maken.

In elk geval laten we zien hoe u een recordset met één record maken. De eerdere voorbeelden voor "A" records kunnen worden aangepast voor het maken van recordsets van andere typen met meerdere records met metagegevens, of leeg recordsets maken.

We geven geen bevoegdheden als een voorbeeld voor het maken van een recordset SOA sinds SOA's zijn gemaakt en verwijderd met elke DNS-zone en kan niet worden gemaakt of afzonderlijk worden verwijderd. Echter, [de SOA kan worden gewijzigd, zoals wordt weergegeven in het voorbeeld van een hoger](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Een AAAA-recordset met één record maken

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-caa-record-set-with-a-single-record"></a>Een CAA-recordset met één record maken

```powershell
New-AzureRmDnsRecordSet -Name "test-caa" -RecordType CAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Caaflags 0 -CaaTag "issue" -CaaValue "ca1.contoso.com") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a>Een CNAME-recordset met één record maken

> [!NOTE]
> De DNS-standaarden staan niet toe dat CNAME-records in de apex van een zone (`-Name '@'`), noch staan ze recordsets met meer dan één record.
> 
> Zie voor meer informatie [CNAME-records](dns-zones-records.md#cname-records).


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Een MX-recordset met één record maken

In dit voorbeeld gebruiken we de naam van de recordset ' @' een MX-record maken in het toppunt van de zone (in dit geval 'contoso.com').


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Een NS-recordset met één record maken

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Een PTR-recordset met één record maken

In dit geval ' Mijn-arpa-zone.com' zone voor reverse lookup ARPA waarmee uw IP-bereik vertegenwoordigt. Elke PTR-recordset die is ingesteld in deze zone komt overeen met een IP-adres in dit IP-bereik. De recordnaam "10" is het laatste octet van het IP-adres binnen deze IP-bereik dat wordt vertegenwoordigd door deze record.

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Een SRV-recordset met één record maken

Bij het maken van een [SRV-Recordset](dns-zones-records.md#srv-records), geef de  *\_service* en  *\_protocol* in de naam van de recordset. Er is niet nodig om ' @' in de recordnaam bij het maken van een SRV-record in het toppunt van de zone.

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Een TXT-recordset met één record maken

Het volgende voorbeeld laat zien hoe een TXT-record te maken. Zie voor meer informatie over de maximale tekenreekslengte ondersteund in de TXT-records [TXT-records](dns-zones-records.md#txt-records).

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Een recordset ophalen

Gebruik voor het ophalen van een bestaande recordset `Get-AzureRmDnsRecordSet`. Deze cmdlet retourneert een lokaal object met de recordset in Azure DNS.

Net als bij `New-AzureRmDnsRecordSet`, de naam van de recordset gegeven moet een *relatieve* naam, wat betekent dat de naam van de zone moet worden uitgesloten. U moet ook opgeven het recordtype en de zone met de record ingesteld.

Het volgende voorbeeld laat zien hoe een recordset ophalen. In dit voorbeeld wordt de zone wordt opgegeven met de `-ZoneName` en `-ResourceGroupName` parameters.

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

U kunt eventueel ook opgeven de zone met behulp van een zone-object dat is doorgegeven met behulp van de `-Zone` parameter.

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Lijst met recordsets

U kunt ook `Get-AzureRmDnsZone` aan de lijst met recordsets in een zone, zonder de `-Name` en/of `-RecordType` parameters.

Het volgende voorbeeld retourneert alle recordsets in de zone:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Het volgende voorbeeld ziet u hoe-alle recordsets van een bepaald type kan worden opgehaald door het recordtype geven terwijl naam als de record wordt weggelaten instellen:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Voor het ophalen van alle recordsets met een opgegeven naam over recordtypen, moet u alle recordsets ophalen en vervolgens de resultaten te filteren:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

In de bovenstaande voorbeelden de zone kan worden opgegeven via de `-ZoneName` en `-ResourceGroupName`parameters (zoals weergegeven), of door te geven van een zone-object:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a>Een record aan een bestaande recordset toevoegen

Als een record toevoegen aan een bestaande recordset, volgt u de volgende drie stappen:

1. De bestaande recordset ophalen

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. De nieuwe record toevoegen aan de lokale Recordset. Dit is een offline-bewerking.

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. De wijziging doorvoeren terug naar de Azure DNS-service. 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

Met behulp van `Set-AzureRmDnsRecordSet` *vervangt* de bestaande recordset in Azure DNS (en alle records die deze bevat) met de opgegeven recordset. [ETag controles](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet overschreven. U kunt de optionele `-Overwrite` overschakelen naar het onderdrukken van deze controles.

Deze reeks bewerkingen kan ook worden *doorgesluisd*, wat betekent dat u het object Recordset met behulp van de pipe in plaats van deze doorgegeven als parameter doorgeven:

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

De bovenstaande voorbeelden laten zien hoe een 'A'-record toevoegen aan een bestaande recordset van het type "A". Een vergelijkbare reeks bewerkingen wordt gebruikt voor het toevoegen van records aan de recordsets van andere typen, vervangen door de `-Ipv4Address` parameter van `Add-AzureRmDnsRecordConfig` met andere parameters die specifiek zijn voor elk recordtype. De parameters voor elk recordtype zijn hetzelfde als voor de `New-AzureRmDnsRecordConfig` cmdlet, zoals wordt weergegeven in [voorbeelden van recordtypen aanvullende](#additional-record-type-examples) hierboven.

Recordsets van het type 'CNAME-' of 'SOA' mag niet meer dan een record bevatten. Deze beperking voortvloeit uit de DNS-standaarden. Het is niet een beperking van Azure DNS.

## <a name="remove-a-record-from-an-existing-record-set"></a>Een record verwijderen uit een bestaande recordset

Een record verwijderen uit een recordset van het proces is vergelijkbaar met het proces voor het toevoegen van een record aan een bestaande recordset:

1. De bestaande recordset ophalen

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. De record verwijderen uit het lokale Recordset-object. Dit is een offline-bewerking. De record die wordt verwijderd moet een exacte overeenkomst aan een bestaande record binnen alle parameters.

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. De wijziging doorvoeren terug naar de Azure DNS-service. Gebruik het optionele `-Overwrite` switch moet worden onderdrukt [Etag controleert](dns-zones-records.md#etags) voor gelijktijdige wijzigingen.

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

De laatste record verwijderen uit een recordset met behulp van de bovenstaande reeks de recordset niet verwijderen, in plaats daarvan het verlaten van een lege recordset. Verwijdert u een recordset volledig [verwijderen van een recordset](#delete-a-record-set).

Naar de records aan een recordset toe te voegen, kan de volgorde van bewerkingen voor het verwijderen van een recordset op dezelfde manier ook worden doorgesluisd:

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Verschillende recordtypen worden ondersteund door de juiste parameters typespecifieke voor `Remove-AzureRmDnsRecordSet`. De parameters voor elk recordtype zijn hetzelfde als voor de `New-AzureRmDnsRecordConfig` cmdlet, zoals wordt weergegeven in [voorbeelden van recordtypen aanvullende](#additional-record-type-examples) hierboven.


## <a name="modify-an-existing-record-set"></a>Een bestaande recordset wijzigen

De stappen voor het wijzigen van een bestaande recordset zijn vergelijkbaar met de stappen waarmee u bij het toevoegen of verwijderen van records uit een Recordset:

1. Ophalen van de bestaande record ingesteld met behulp van `Get-AzureRmDnsRecordSet`.
2. Wijzig het lokale Recordset-object door:
    * Het toevoegen of verwijderen van records
    * De parameters van bestaande records wijzigen
    * Het wijzigen van de record metagegevens en de tijd ingesteld op live (TTL)
3. Uw wijzigingen met behulp van de `Set-AzureRmDnsRecordSet` cmdlet. Dit *vervangt* de bestaande recordset in Azure DNS met de opgegeven recordset.

Wanneer u `Set-AzureRmDnsRecordSet`, [Etag controleert](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet overschreven. U kunt de optionele `-Overwrite` overschakelen naar het onderdrukken van deze controles.

### <a name="to-update-a-record-in-an-existing-record-set"></a>Een record in een bestaande recordset bij te werken

In dit voorbeeld wijzigen we het IP-adres van een bestaande "A" record:

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a>Een SOA-record wijzigen

U kunt toevoegen of verwijderen van records uit de automatisch gemaakte SOA-record is ingesteld in het toppunt van de zone (`-Name "@"`, inclusief de aanhalingstekens). Echter, kunt u een van de parameters binnen de SOA-record (met uitzondering van de "Host") en de TTL-waarde van de recordset.

Het volgende voorbeeld ziet u het wijzigen van de *e* eigenschap van de SOA-record:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS-records in het toppunt van de zone wijzigen

De NS-recordset in het toppunt van de zone wordt automatisch gemaakt met elke DNS-zone. Het bevat de namen van de Azure DNS-naamservers toegewezen aan de zone.

U kunt extra naam ingesteld van servers aan deze NS-record, ter ondersteuning van collega hosting domeinen met meer dan één DNS-provider toevoegen. U kunt ook de TTL-waarde en de metagegevens voor deze recordset wijzigen. U kan echter verwijderen of wijzigen van de vooraf ingestelde Azure DNS-naamservers.

Houd er rekening mee dat dit alleen voor de NS-recordset in het toppunt van de zone geldt. Andere NS-recordsets in de zone (zoals gebruikt voor het delegeren van onderliggende zones) kunnen worden gewijzigd zonder beperking.

Het volgende voorbeeld ziet u hoe een extra naamserver aan de NS-recordset in het toppunt van de zone toevoegen:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a>Recordset metagegevens wijzigen

[Metagegevens van de recordset](dns-zones-records.md#tags-and-metadata) kan worden gebruikt om toepassingsspecifieke gegevens koppelen aan elke recordset als sleutel-waardeparen.

Het volgende voorbeeld ziet u hoe de metagegevens van een bestaande recordset wijzigen:

```powershell
# Get the record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a>Een recordset verwijderen

Recordsets kunnen worden verwijderd met behulp van de `Remove-AzureRmDnsRecordSet` cmdlet. Een recordset te verwijderen, verwijdert tevens alle records in de recordset.

> [!NOTE]
> U kunt de SOA niet verwijderen en NS-record wordt ingesteld in het toppunt van de zone (`-Name '@'`).  Deze wordt automatisch door Azure DNS gemaakt wanneer de zone is gemaakt en worden automatisch verwijderd wanneer de zone wordt verwijderd.

Het volgende voorbeeld laat zien hoe een recordset te verwijderen. In dit voorbeeld wordt worden de naam van de recordset, type recordset, zonenaam en resourcegroep elk expliciet opgegeven.

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

De recordset kan ook worden opgegeven door de naam en type en de zone die is opgegeven met behulp van een object:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Een derde optie, kan de recordset zelf worden opgegeven met behulp van een Recordset-object:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

Wanneer u de recordset moet worden verwijderd met behulp van een object Recordset opgeeft [Etag controleert](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet verwijderd. U kunt de optionele `-Overwrite` overschakelen naar het onderdrukken van deze controles.

De Recordset-object kan ook worden doorgesluisd in plaats van dat wordt doorgegeven als parameter:

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a>Bevestiging vragen

De `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, en `Remove-AzureRmDnsRecordSet` cmdlets alle bevestiging vragen ondersteunen.

Elke cmdlet vraagt om bevestiging als de `$ConfirmPreference` PowerShell voorkeursvariabele een waarde heeft van `Medium` of lager. Sinds de standaardwaarde voor `$ConfirmPreference` is `High`, deze vragen zijn niet opgegeven voor het met de standaardinstellingen voor PowerShell.

U kunt de huidige overschrijven `$ConfirmPreference` instellen met de `-Confirm` parameter. Als u opgeeft `-Confirm` of `-Confirm:$True` , vraagt de cmdlet u om bevestiging voordat deze wordt uitgevoerd. Als u opgeeft `-Confirm:$False` , de cmdlet wordt u niet gevraagd om bevestiging. 

Voor meer informatie over `-Confirm` en `$ConfirmPreference`, Zie [over Voorkeursvariabelen](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [zones en -records in Azure DNS](dns-zones-records.md).
<br>
Meer informatie over hoe [beveiligen van uw zones en records](dns-protect-zones-recordsets.md) bij gebruik van Azure DNS.
<br>
Controleer de [Azure DNS PowerShell-naslagdocumentatie](/powershell/module/azurerm.dns).
