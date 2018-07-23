---
title: Aangepaste DNS-records voor een web-app maken | Microsoft Docs
description: Het aangepaste domein-DNS-records voor web-app met behulp van Azure DNS maken.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: victorh
ms.openlocfilehash: f24c301cea5ef91d101206e71b69b7ceb03b0282
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172446"
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>DNS-records voor een web-app maken in een aangepast domein

U kunt Azure DNS gebruiken voor het hosten van een aangepast domein voor uw web-apps. Bijvoorbeeld, het maken van een Azure-web-app en u wilt dat uw gebruikers toegang tot het door een van beide contoso.com of www.contoso.com als een FQDN-naam gebruiken.

Om dit te doen, moet u twee records maken:

* Een 'A'-hoofdmaprecord die verwijst naar contoso.com
* Een 'CNAME'-record voor de www-naam die naar de A-record verwijst

Houd er rekening mee dat als u een A-record voor een web-app in Azure maken, de A-record handmatig moet worden bijgewerkt als het onderliggende IP-adres voor de web-app wordt gewijzigd.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint, moet u eerst een DNS-zone maken in Azure DNS, en de zone in uw registrar naar Azure DNS delegeren.

1. Volg de stappen in voor het maken van een DNS-zone, [maken van een DNS-zone](dns-getstarted-create-dnszone.md).
2. Volg de stappen in voor het overdragen van uw DNS naar Azure DNS, [domein-DNS-delegering](dns-domain-delegation.md).

Na het maken van een zone en deze naar Azure DNS delegeren, kunt u vervolgens records maken voor uw aangepaste domein.

## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Maken van een A-record voor uw aangepaste domein

Een A-record wordt gebruikt om een naam toewijzen aan het IP-adres. In het volgende voorbeeld toegewezen \@ als een A-record in een IPv4-adres:

### <a name="step-1"></a>Stap 1

Een A-record maken en toewijzen aan een variabele $rs

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a>Stap 2

De waarde IPv4 toevoegen aan de eerder gemaakte recordset '\@' met behulp van de variabele $rs is toegewezen. De toegewezen waarde voor IPv4 is het IP-adres voor uw web-app.

Het IP-adres voor een web-app vindt u de stappen in [een aangepaste domeinnaam configureren in Azure App Service](../app-service/app-service-web-tutorial-custom-domain.md).

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a>Stap 3

Voer de wijzigingen aan de recordset. Gebruik `Set-AzureRMDnsRecordSet` uploadt u de wijzigingen aan de recordset naar Azure DNS:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Maak een CNAME-record voor uw aangepaste domein

Als uw domein wordt al beheerd door Azure DNS (Zie [domein-DNS-delegering](dns-domain-delegation.md), kunt u het volgende voorbeeld maakt een CNAME-record voor contoso.azurewebsites.net.

### <a name="step-1"></a>Stap 1

Open PowerShell en een nieuwe CNAME-recordset maken en toewijzen aan een variabele $rs. In dit voorbeeld wordt een type recordset CNAME met een 'time to live"maken van een 600 seconden in DNS-zone met de naam 'contoso.com'.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

Het volgende voorbeeld is het antwoord.

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>Stap 2

Als de CNAME-recordset is gemaakt, moet u de aliaswaarde van een die naar de web-app verwijst maken.

Met behulp van de eerder toegewezen variabele '$rs' kunt u de onderstaande PowerShell-opdracht voor het maken van de alias voor de web-app contoso.azurewebsites.net.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

Het volgende voorbeeld is het antwoord.

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>Stap 3

De wijzigingen aan met behulp van de `Set-AzureRMDnsRecordSet` cmdlet:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

U kunt controleren of de record correct is gemaakt door het opvragen van de 'www.contoso.com' met behulp van nslookup, zoals hieronder wordt weergegeven:

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a>Maak een 'awverify'-record voor web-apps

Als u besluit een A-record voor uw web-app gebruiken, moet u een verificatieprocedure voor om te controleren of dat u eigenaar van het aangepaste domein doorlopen. Deze verificatiestap wordt uitgevoerd door het maken van een speciale CNAME-record met de naam 'awverify'. In deze sectie is van toepassing op een alleen-records.

### <a name="step-1"></a>Stap 1

De 'awverify'-record maken. In het onderstaande voorbeeld maken we de 'aweverify'-record voor contoso.com te verifiëren voor het aangepaste domein.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

Het volgende voorbeeld is het antwoord.

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>Stap 2

Zodra de recordset 'awverify' is gemaakt, wijzen de CNAME-Recordset alias. In het onderstaande voorbeeld zullen we de CNAMe-record ingesteld alias op awverify.contoso.azurewebsites.net toewijzen.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

Het volgende voorbeeld is het antwoord.

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>Stap 3

De wijzigingen aan met behulp van de `Set-AzureRMDnsRecordSet cmdlet`, zoals wordt weergegeven in de onderstaande opdracht.

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a>Volgende stappen

Volg de stappen in [configureren van een aangepaste domeinnaam voor App Service](../app-service/app-service-web-tutorial-custom-domain.md) aan uw web-app voor het gebruik van een aangepast domein configureren.
