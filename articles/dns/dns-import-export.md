---
title: Importeren en exporteren van een domein zonebestand naar Azure DNS met Azure CLI 2.0 | Microsoft Docs
description: Meer informatie over het importeren en exporteren van een DNS-zone-bestand naar Azure DNS met behulp van Azure CLI 2.0
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2018
ms.author: gwallace
ms.openlocfilehash: 6bb9b6de195eaea1f7c8591d2de47d360ccde488
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli-20"></a>Importeren en exporteren van een DNS-zone-bestand met de Azure CLI 2.0 

Dit artikel begeleidt u bij het importeren en exporteren van DNS-zone-bestanden voor Azure DNS met de Azure CLI 2.0.

## <a name="introduction-to-dns-zone-migration"></a>Inleiding tot DNS-zone-migratie

Een DNS-zonebestand is een tekstbestand met de details van elke record System DNS (Domain Name) in de zone. Volgt een standaardindeling, waardoor het geschikt is voor de DNS-records overbrengen tussen DNS-systemen. Met behulp van een zonebestand is een snelle, betrouwbare en handige manier om over te dragen van een DNS-zone van of naar Azure DNS.

Azure DNS ondersteunt importeren en exporteren van de zone-bestanden met behulp van de Azure-opdrachtregelinterface (CLI). Zone-bestand importeren is **niet** momenteel ondersteund via Azure PowerShell of Azure portal.

De Azure CLI 2.0 is een platformoverschrijdende-opdrachtregelprogramma voor het beheer van Azure-services. Is beschikbaar voor Windows, Mac en Linux-platforms van de [pagina Azure downloads](https://azure.microsoft.com/downloads/). Ondersteuning voor meerdere platforms is belangrijk voor importeren en exporteren van zonebestanden, aangezien de meest voorkomende naam serversoftware [BINDEN](https://www.isc.org/downloads/bind/), doorgaans op Linux wordt uitgevoerd.


## <a name="obtain-your-existing-dns-zone-file"></a>Verkrijgen van uw bestaande DNS-zonebestand

Voordat u een DNS-zone-bestand in Azure DNS importeren, moet u een exemplaar van de zonebestand. De bron van dit bestand is afhankelijk van waar de DNS-zone momenteel wordt gehost.

* Als uw DNS-zone wordt gehost door een partner-service (zoals een domeinregistrar, specifieke DNS-hostingprovider of alternatieve cloudprovider), leveren of de service de mogelijkheid voor het downloaden van het DNS-zonebestand.
* Als uw DNS-zone wordt gehost op Windows DNS, de standaardmap voor de zonebestanden is **%systemroot%\system32\dns**. Het volledige pad naar het bestand voor elke zone wordt ook weergegeven op de **algemene** tabblad van de DNS-console.
* Als uw DNS-zone wordt gehost met behulp van BIND, de locatie van de zonebestand voor elke zone die is opgegeven in het configuratiebestand BIND **named.conf**.

> [!NOTE]
> Zone-bestanden die zijn gedownload van GoDaddy hebben een iets andere indeling. U moet deze corrigeren voordat u deze zonebestanden in Azure DNS importeren.
>
> DNS-namen in het RDATA van elke DNS-record zijn opgegeven als FQDN-namen, maar ze niet beschikken over een afsluitende '. ' Dit betekent dat ze worden geïnterpreteerd door andere DNS-systemen als relatieve namen. U moet het zonebestand om toe te voegen de afsluitende bewerken '. ' voor hun namen voordat u ze in Azure DNS importeren.
>
> Bijvoorbeeld, moet de CNAME-record 'www 3600 IN CNAME contoso.com' worden gewijzigd in 'www 3600 IN CNAME contoso.com.'
> (met een afsluitende '. ').

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Een DNS-zone-bestand importeren in Azure DNS

Maakt een nieuwe zone een zonebestand te importeren in Azure DNS, als deze niet al bestaat. Als de zone al bestaat, moeten de recordsets in de zonebestand met de bestaande recordsets worden samengevoegd.

### <a name="merge-behavior"></a>Samenvoegen van gedrag

* Standaard worden bestaande en nieuwe recordsets samengevoegd. Identieke records binnen een samengevoegde Recordset zijn ongedaan gedupliceerde.
* Als recordsets worden samengevoegd, wordt de tijd van de bestaande recordsets live (TTL) gebruikt.
* Begin van de parameters Authority (SOA) (met uitzondering van `host`) altijd worden overgenomen uit het geïmporteerde zonebestand. Op deze manier voor de naamserverrecord ingesteld in het toppunt van de zone, is de TTL-waarde altijd overgenomen uit het geïmporteerde zonebestand.
* Een geïmporteerde CNAME-record is geen vervanging voor een bestaande CNAME-record met dezelfde naam.  
* Als er ontstaat een conflict tussen een CNAME-record en een andere record met dezelfde naam maar met een ander type (ongeacht die is bestaande of nieuwe), wordt de bestaande blijven behouden. 

### <a name="additional-information-about-importing"></a>Als u meer informatie over het importeren

De volgende opmerkingen bieden aanvullende technische gegevens over de zone importeren.

* De `$TTL` richtlijn is optioneel en wordt ondersteund. Als er geen `$TTL` richtlijn krijgt, records zonder een expliciete TTL geïmporteerde is ingesteld op een standaard-TTL van 3600 seconden zijn. Wanneer twee records in de dezelfde recordset verschillende TTLs opgeeft, wordt de lagere waarde gebruikt.
* De `$ORIGIN` richtlijn is optioneel en wordt ondersteund. Als er geen `$ORIGIN` is ingesteld, de standaardwaarde gebruikt, is de naam van de zone die is opgegeven op de opdrachtregel (plus de afsluitende '. ').
* De `$INCLUDE` en `$GENERATE` richtlijnen worden niet ondersteund.
* Deze typen worden ondersteund: A, AAAA, CNAME, MX, NS, SOA, SRV en TXT.
* De SOA-record wordt automatisch gemaakt door Azure DNS wanneer u een zone wordt gemaakt. Wanneer u een zonebestand importeert, alle SOA-parameters zijn overgenomen uit het zonebestand *behalve* de `host` parameter. De waarde die is verstrekt door Azure DNS maakt gebruik van deze parameter. Dit is omdat deze parameter naar de server de primaire naam van de Azure DNS verwijzen moet.
* De naamserverrecord ingesteld in het toppunt van de zone wordt ook automatisch gemaakt door Azure DNS wanneer de zone wordt gemaakt. Alleen de TTL van deze recordset is geïmporteerd. Deze records bevatten de servernamen verstrekt door Azure DNS. De gegevens van de record is niet overschreven door de waarden in de geïmporteerde zone-bestand.
* Azure DNS biedt ondersteuning voor slechts één tekenreeks TXT-records tijdens de openbare Preview. Multistring TXT-records zijn worden samengevoegd en afgekapt tot 255 tekens.

### <a name="cli-format-and-values"></a>CLI-indeling en waarden

De indeling van de Azure CLI-opdracht voor het importeren van een DNS-zone is:

```azurecli
az network dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Waarden:

* `<resource group>` is de naam van de resourcegroep voor de zone in Azure DNS.
* `<zone name>` is de naam van de zone.
* `<zone file name>` is de padnaam van het zonebestand moet worden geïmporteerd.

Als een zone met deze naam niet in de resourcegroep bestaat, wordt deze voor u gemaakt. Als de zone al bestaat, wordt de geïmporteerde recordsets worden samengevoegd met bestaande recordsets. 


### <a name="step-1-import-a-zone-file"></a>Step 1. Een zonebestand importeren

Voor het importeren van een zonebestand voor de zone **contoso.com**.

1. Als u nog geen een hebt, moet u een Resource Manager-resourcegroep maken.

    ```azurecli
    az group create --group myresourcegroup -l westeurope
    ```

2. Voor het importeren van de zone **contoso.com** uit het bestand **contoso.com.txt** in een nieuwe DNS-zone in de resourcegroep **myresourcegroup**, wordt u de opdracht uitgevoerd `az network dns zone import` .<BR>Deze opdracht wordt de zonebestand geladen en parseert het. De opdracht wordt een reeks opdrachten uitgevoerd op de Azure DNS-service voor het maken van de zone en alle recordsets in de zone. De opdracht wordt uitgevoerd in het consolevenster, samen met eventuele fouten of waarschuwingen gemeld. Omdat recordsets zijn gemaakt in de reeks, duurt een paar minuten een grote zone-bestand importeren.

    ```azurecli
    az network dns zone import -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a>Stap 2. Controleer of de zone

Om te controleren of de DNS-zone nadat u het bestand importeert, kunt u een van de volgende methoden gebruiken:

* U kunt de records weergeven met behulp van de volgende Azure CLI-opdracht:

    ```azurecli
    az network dns record-set list -g myresourcegroup -z contoso.com
    ```

* U kunt de records weergeven met behulp van de PowerShell-cmdlet `Get-AzureRmDnsRecordSet`.
* U kunt `nslookup` om te controleren of de naamomzetting voor de records. Omdat de zone nog niet is toegewezen, moet u de juiste Azure DNS-naamservers expliciet opgeven. Het volgende voorbeeld laat zien hoe de servernamen toegewezen aan de zone worden opgehaald. Dit voorbeeld toont ook de www-record met behulp van een query `nslookup`.
```
C:\>az network dns record-set ns list -g myresourcegroup -z  --output json 
  [
   .......
   "name": "@",
    "nsRecords": [
      {
        "additionalProperties": {},
        "nsdname": "ns1-03.azure-dns.com."
      },
      {
        "additionalProperties": {},
        "nsdname": "ns2-03.azure-dns.net."
      },
      {
        "additionalProperties": {},
        "nsdname": "ns3-03.azure-dns.org."
      },
      {
        "additionalProperties": {},
        "nsdname": "ns4-03.azure-dns.info."
      }
    ],
    "resourceGroup": "myresourcegroup",
    "ttl": 86400,
    "type": "Microsoft.Network/dnszones/NS"
  }
]

        C:\> nslookup www.contoso.com ns1-03.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221
```

### <a name="step-3-update-dns-delegation"></a>Stap 3. DNS-delegering bijwerken

Nadat u hebt gecontroleerd of de zone correct zijn geïmporteerd, moet u de DNS-delegering om te verwijzen naar de Azure DNS-naamservers bijwerken. Zie voor meer informatie het artikel [de DNS-delegering bijwerken](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Een DNS-zone-bestand exporteren uit Azure DNS

De indeling van de Azure CLI-opdracht voor het importeren van een DNS-zone is:

```azurecli
az network dns zone export -g <resource group> -z <zone name> -f <zone file name>
```

Waarden:

* `<resource group>` is de naam van de resourcegroep voor de zone in Azure DNS.
* `<zone name>` is de naam van de zone.
* `<zone file name>` is de padnaam van het zonebestand moet worden geëxporteerd.

Als met de import zone moet u eerst aanmelden, kiest u uw abonnement en configureren van de Azure CLI voor het gebruik van Resource Manager-modus.

### <a name="to-export-a-zone-file"></a>Een zonebestand exporteren

Exporteren van de bestaande Azure DNS-zone **contoso.com** in de resourcegroep **myresourcegroup** naar het bestand **contoso.com.txt** (in de huidige map), voert u `azure network dns zone export`. Met deze opdracht roept de service Azure DNS-recordsets in de zone inventariseren en de resultaten exporteren naar een zone BIND-compatibel bestand.

    ```
    az network dns zone export -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```
