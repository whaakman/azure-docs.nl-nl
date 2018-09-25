---
title: Beheren van DNS-records in Azure DNS met behulp van Azure PowerShell | Microsoft Docs
description: DNS-recordsets en records in Azure DNS beheren bij het hosten van uw domein in Azure DNS. Alle PowerShell-opdrachten voor bewerkingen voor recordsets en records.
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: victorh
ms.openlocfilehash: b89b7885989a5e93d3d292e5cdcff733fed657af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990175"
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>DNS-records en recordsets in Azure DNS met behulp van Azure PowerShell beheren

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [CLI van Azure classic](dns-operations-recordsets-cli-nodejs.md)
> * [Azure-CLI](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

In dit artikel wordt beschreven hoe u voor het beheren van DNS-records voor uw DNS-zone met behulp van Azure PowerShell. DNS-records kunnen ook worden beheerd met behulp van de platformoverschrijdende [Azure CLI](dns-operations-recordsets-cli.md) of de [Azure-portal](dns-operations-recordsets-portal.md).

De voorbeelden in dit artikel wordt ervan uitgegaan dat u al hebt [Azure PowerShell, aangemeld, geïnstalleerd en wordt gemaakt van een DNS-zone](dns-operations-dnszones.md).

## <a name="introduction"></a>Inleiding

Voordat u DNS-records in DNS Azure maakt, leest u eerst hoe Azure DNS DNS-records organiseert in DNS-recordsets.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Zie [DNS-zones en -records](dns-zones-records.md) voor meer informatie over DNS-records in Azure DNS.


## <a name="create-a-new-dns-record"></a>Een nieuwe DNS-record maken

Als de nieuwe record dezelfde naam en hetzelfde type als een bestaand record heeft, moet u [toe te voegen aan de bestaande recordset](#add-a-record-to-an-existing-record-set). Als de nieuwe record een andere naam en type alle bestaande records heeft, moet u een nieuwe recordset maken. 

### <a name="create-a-records-in-a-new-record-set"></a>'A'-records in een nieuwe recordset maken

U kunt recordsets maken met behulp van de cmdlet `New-AzureRmDnsRecordSet`. Wanneer u een Recordset maakt, moet u opgeven de record de naam, de zone, de tijd ingesteld op live (TTL), het recordtype en de records die moeten worden gemaakt.

De parameters voor het toevoegen van records aan een recordset variëren afhankelijk van het type recordset. Bijvoorbeeld, wanneer u een recordset van het type 'A', moet u om op te geven van het IP-adres met de parameter `-IPv4Address`. Andere parameters worden gebruikt voor andere recordtypen. Zie [voorbeelden van aanvullende recordtypen](#additional-record-type-examples) voor meer informatie.

Het volgende voorbeeld wordt een record met de relatieve naam 'www' in de DNS-Zone 'contoso.com'. De volledig gekwalificeerde naam van de recordset is 'www.contoso.com'. Het recordtype is 'A', en de TTL is 3600 seconden. De recordset bevat één record met IP-adres '1.2.3.4'.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Een record in de apex van een zone wilt maken (in dit geval 'contoso.com'), gebruikt u de recordnaam '\@' (zonder aanhalingstekens):

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Als u een recordset met meer dan één record maken wilt, eerst een lokale matrix maken en toevoegen van de records, wordt doorgeven van de matrix, zodat `New-AzureRmDnsRecordSet` als volgt:

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[Metagegevens van de recordset](dns-zones-records.md#tags-and-metadata) toepassingsspecifieke gegevens koppelen aan elke recordset als sleutel-waardeparen kunnen worden gebruikt. Het volgende voorbeeld ziet u hoe u een recordset met twee metagegevens vermeldingen maken ' schuld = Financiën ' en ' omgeving productie ='.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Azure DNS ondersteunt ook 'empty' recordsets als een tijdelijke aanduiding voor een DNS-naam reserveren fungeren kunnen voordat u DNS-records maakt. Lege recordsets zijn zichtbaar in de Azure DNS-besturingselement vlak, maar worden weergegeven op de DNS-naamservers. Het volgende voorbeeld wordt een lege recordset gemaakt:

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Records van andere typen maken

Hebben gezien in detail 'A'-records maken, de volgende voorbeelden laten zien hoe andere opnemen die worden ondersteund door Azure DNS-records maken.

In elk geval laten we zien hoe u een recordset met één record maken. De eerdere voorbeelden voor 'A'-records kunnen worden aangepast om te maken van recordsets van andere typen met meerdere records, met metagegevens, of lege recordsets maken.

We bieden geen een voorbeeld van een set van de SOA-record maken omdat SOA worden gemaakt en verwijderd met de DNS-zone en kan niet worden gemaakt of afzonderlijk worden verwijderd. Echter, [de SOA kan worden gewijzigd, zoals wordt weergegeven in het voorbeeld van een hoger](#to-modify-an-SOA-record).

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
> De DNS-standaarden staan niet toe dat CNAME-records in de apex van een zone (`-Name '@'`), noch staan ze recordsets met meer dan één record toe.
> 
> Zie voor meer informatie, [CNAME-records](dns-zones-records.md#cname-records).


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Een MX-recordset met één record maken

In dit voorbeeld gebruiken we de naam van de recordset '\@' een MX-record maken in de apex van de zone (in dit geval 'contoso.com').


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Een NS-recordset met één record maken

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Een PTR-recordset met één record maken

In dit geval ' Mijn-arpa-zone.com ' de ARPA-reverse lookup-zone voor uw IP-adresbereik vertegenwoordigt. Elke PTR-recordset die is ingesteld in deze zone komt overeen met een IP-adres in dit IP-bereik. De recordnaam "10" is het laatste achttal werd van het IP-adres binnen deze IP-adresbereik dat wordt vertegenwoordigd door deze record.

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Een SRV-recordset met één record maken

Bij het maken van een [SRV-Recordset](dns-zones-records.md#srv-records), geef de  *\_service* en  *\_protocol* in de recordnaam. Er is niet nodig om op te nemen '\@' in de naam van de recordset wanneer het maken van een SRV-record ingesteld in het toppunt van de zone.

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Een TXT-recordset met één record maken

Het volgende voorbeeld ziet hoe u een TXT-record te maken. Zie voor meer informatie over de maximale tekenreekslengte ondersteund in een TXT-records, [TXT-records](dns-zones-records.md#txt-records).

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Een recordset ophalen

Gebruiken om op te halen van een bestaande recordset, `Get-AzureRmDnsRecordSet`. Deze cmdlet retourneert een lokale object dat staat voor de recordset in Azure DNS.

Net als bij `New-AzureRmDnsRecordSet`, de naam van de recordset gegeven moet een *relatieve* naam, wat betekent dat de naam van de zone moet worden uitgesloten. U moet ook het recordtype opgeven en instellen van de zone die de record bevat.

Het volgende voorbeeld ziet hoe u een recordset ophaalt. In dit voorbeeld wordt de zone is opgegeven met behulp van de `-ZoneName` en `-ResourceGroupName` parameters.

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

U kunt ook u kunt ook opgeven de zone met behulp van een zone-object doorgegeven met behulp van de `-Zone` parameter.

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Lijst met recordsets

U kunt ook `Get-AzureRmDnsZone` aan de lijst met recordsets in een zone, zonder de `-Name` en/of `-RecordType` parameters.

Het volgende voorbeeld worden alle recordsets in de zone:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Het volgende voorbeeld laat zien hoe alle recordsets bent van een bepaald type kunnen worden opgehaald door het recordtype op te geven terwijl de naam als de record ingesteld:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Om op te halen alle recordsets met een naam gegeven over recordtypen, moet u alle recordsets ophalen en vervolgens de resultaten te filteren:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

In de bovenstaande voorbeelden de zone kan worden opgegeven door de `-ZoneName` en `-ResourceGroupName`parameters (zoals weergegeven), of door een zone-object op te geven:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a>Een record toevoegt aan een bestaande record-set

Als u wilt een record toevoegt aan een bestaande record-set, volgt u de volgende drie stappen:

1. De bestaande recordset ophalen

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. De nieuwe record toevoegen aan de lokale Recordset. Dit is een offline-bewerking.

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Voer de wijziging terug naar de Azure DNS-service. 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

Met behulp van `Set-AzureRmDnsRecordSet` *vervangt* de bestaande recordset in Azure DNS (en alle records die deze bevat) met de opgegeven recordset. [ETag controles](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet overschreven. U kunt de optionele `-Overwrite` overschakelen naar deze controles onderdrukken.

Deze reeks bewerkingen kan ook worden *doorgesluisd*, wat betekent dat u de Recordset-object met behulp van de pipe in plaats van deze door te geven als een parameter doorgeven:

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

De bovenstaande voorbeelden laten zien hoe u een A-record toevoegt aan een bestaande recordset van het type 'A'. Een vergelijkbare reeks bewerkingen wordt gebruikt voor het toevoegen van records aan de recordsets van andere typen, waarbij de `-Ipv4Address` parameter van `Add-AzureRmDnsRecordConfig` met andere parameters die specifiek zijn voor elk recordtype. De parameters van elk recordtype zijn hetzelfde als voor de `New-AzureRmDnsRecordConfig` cmdlet, zoals wordt weergegeven in [voorbeelden van aanvullende recordtypen](#additional-record-type-examples) hierboven.

Recordsets van het type 'CNAME' of 'SOA' mag niet meer dan één record bevatten. Deze beperking voortvloeit uit de DNS-standaarden. Het is niet een beperking van Azure DNS.

## <a name="remove-a-record-from-an-existing-record-set"></a>Een record verwijderen uit een bestaande recordset

Het proces voor het verwijderen van een record uit een recordset is vergelijkbaar met het proces voor het toevoegen van een record aan een bestaande recordset:

1. De bestaande recordset ophalen

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Verwijder de record van de lokale Recordset-object. Dit is een offline-bewerking. De record die wordt verwijderd, moet exact overeenkomt met een bestaande record zijn voor alle parameters.

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Voer de wijziging terug naar de Azure DNS-service. Gebruik het optionele `-Overwrite` switch moet worden onderdrukt [Etag controleert](dns-zones-records.md#etags) voor gelijktijdige wijzigingen.

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

De laatste record verwijderen uit een recordset met behulp van de bovenstaande volgorde de recordset niet verwijderen, in plaats daarvan het verlaten van een lege recordset. Als u wilt verwijderen van een recordset volledig, Zie [verwijderen van een recordset](#delete-a-record-set).

Aan de records toe te voegen aan een recordset, kan de volgorde van bewerkingen te verwijderen van een recordset op dezelfde manier ook worden doorgesluisd:

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Verschillende recordtypen worden ondersteund door de juiste typespecifieke parameters doorgeven `Remove-AzureRmDnsRecordSet`. De parameters van elk recordtype zijn hetzelfde als voor de `New-AzureRmDnsRecordConfig` cmdlet, zoals wordt weergegeven in [voorbeelden van aanvullende recordtypen](#additional-record-type-examples) hierboven.


## <a name="modify-an-existing-record-set"></a>Een bestaande recordset wijzigen

De stappen voor het wijzigen van een bestaande recordset zijn vergelijkbaar met de stappen waarmee u bij het toevoegen of verwijderen van records uit een Recordset:

1. Ophalen van de bestaande recordset met behulp van `Get-AzureRmDnsRecordSet`.
2. Het lokale Recordset-object door te wijzigen:
    * Toevoegen of verwijderen van records
    * De parameters van bestaande records wijzigen
    * Het wijzigen van de record metagegevens en de tijd ingesteld op live (TTL)
3. Leg uw wijzigingen vast met behulp van de `Set-AzureRmDnsRecordSet` cmdlet. Dit *vervangt* de bestaande recordset in Azure DNS met de opgegeven recordset.

Bij het gebruik van `Set-AzureRmDnsRecordSet`, [Etag controleert](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet overschreven. U kunt de optionele `-Overwrite` overschakelen naar deze controles onderdrukken.

### <a name="to-update-a-record-in-an-existing-record-set"></a>Een record in een bestaande recordset bij te werken

In dit voorbeeld wijzigen we het IP-adres van een bestaande A-record:

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a>Een SOA-record wijzigen

U kan toevoegen of verwijderen van records uit de automatisch gemaakte SOA-record ingesteld in het toppunt van de zone (`-Name "@"`, inclusief de aanhalingstekens). Echter, u kunt wijzigen van de parameters in de SOA-record (met uitzondering van 'Host') en de TTL-waarde van de recordset.

Het volgende voorbeeld laat zien hoe u de *e* eigenschap van de SOA-record:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS-records in de apex van de zone wijzigen

De NS-recordset in de apex van de zone wordt automatisch gemaakt met elke DNS-zone. Deze bevat de namen van de Azure DNS-naamservers toegewezen aan de zone.

U kunt de naam van aanvullende servers aan deze NS-record ingesteld voor de ondersteuning van collega hosting-domeinen met meer dan één DNS-provider toevoegen. U kunt ook de TTL-waarde en de metagegevens voor deze recordset wijzigen. U kan echter verwijderen of wijzigen van de vooraf ingestelde Azure DNS-naamservers.

Houd er rekening mee dat dit alleen voor de NS-recordset in de apex van de zone geldt. Andere NS-recordsets in de zone (zoals die wordt gebruikt om te delegeren onderliggende zones) kunnen worden gewijzigd zonder beperking.

Het volgende voorbeeld laat zien hoe een extra UPN-server toevoegen aan de NS-recordset in de apex van de zone:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a>De metagegevens van de recordset wijzigen

[Metagegevens van de recordset](dns-zones-records.md#tags-and-metadata) toepassingsspecifieke gegevens koppelen aan elke recordset als sleutel-waardeparen kunnen worden gebruikt.

Het volgende voorbeeld laat zien hoe de metagegevens van een bestaande recordset te wijzigen:

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

Recordsets kunnen worden verwijderd met behulp van de `Remove-AzureRmDnsRecordSet` cmdlet. Als u een recordset verwijdert, verwijdert u ook alle records in de recordset.

> [!NOTE]
> De SOA kan niet worden verwijderd en NS-record wordt ingesteld in het toppunt van de zone (`-Name '@'`).  Deze wordt automatisch door Azure DNS gemaakt wanneer de zone is gemaakt en worden automatisch verwijderd wanneer de zone wordt verwijderd.

Het volgende voorbeeld ziet hoe u een recordset verwijderen. In dit voorbeeld wordt zijn de RecordsetNaam, de Recordset type, de zonenaam en de resourcegroep elk expliciet opgegeven.

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

De recordset kan ook worden opgegeven door de naam en type en de zone worden opgegeven met behulp van een object:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Een derde mogelijkheid is, kan de recordset zelf worden opgegeven met behulp van een Recordset-object:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

Wanneer u de recordset moet worden verwijderd met behulp van een object Recordset opgeeft [Etag controleert](dns-zones-records.md#etags) worden gebruikt om ervoor te zorgen gelijktijdige wijzigingen zijn niet verwijderd. U kunt de optionele `-Overwrite` overschakelen naar deze controles onderdrukken.

De Recordset-object kan ook worden doorgesluisd in plaats van dat wordt doorgegeven als parameter:

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a>Bevestigingsprompts

De cmdlets `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet` en `Remove-AzureRmDnsRecordSet` ondersteunen bevestigingspromts.

Elke cmdlet vraagt om bevestiging als de `$ConfirmPreference` PowerShell-voorkeursvariabele heeft een waarde van `Medium` of lager. Omdat de standaardwaarde voor `$ConfirmPreference` is `High`, krijgen deze vraag niet bij het gebruik van de standaardinstellingen voor PowerShell.

U kunt de huidige instelling van `$ConfirmPreference` overschrijven met behulp van de parameter `-Confirm`. Als u `-Confirm` of `-Confirm:$True` opgeeft, vraagt de cmdlet u om bevestiging voordat deze wordt uitgevoerd. Als u `-Confirm:$False` opgeeft, wordt u niet om bevestiging gevraagd. 

Zie [over voorkeursvariabelen](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables) voor meer informatie over `-Confirm` en `$ConfirmPreference`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [zones en -records in Azure DNS](dns-zones-records.md).
<br>
Meer informatie over het [uw zones en records beschermen](dns-protect-zones-recordsets.md) bij het gebruik van Azure DNS.
<br>
Controleer de [referentiedocumentatie voor PowerShell voor Azure DNS](/powershell/module/azurerm.dns).
