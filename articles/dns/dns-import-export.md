---
title: Importeren en exporteren van een domein zone-bestand naar Azure DNS met behulp van Azure CLI | Microsoft Docs
description: Meer informatie over het importeren en exporteren van een DNS-zonebestand naar Azure DNS met behulp van Azure CLI
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2018
ms.author: victorh
ms.openlocfilehash: 5afb607f0410b428d8e67fdff043a4e376dd60a5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956350"
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importeren en exporteren van een DNS-zone-bestand met de Azure CLI 

In dit artikel leert u hoe u om te importeren en exporteren van DNS-zonebestanden voor Azure DNS met behulp van de Azure CLI.

## <a name="introduction-to-dns-zone-migration"></a>Inleiding tot de DNS-zone migreren

Een DNS-zonebestand is een tekstbestand met details van elke record van de Domain Name System (DNS) in de zone. Er volgt een standaardindeling, waardoor het geschikt is voor de overdracht van DNS-records van DNS-systemen. Met behulp van een zonebestand is een snelle, betrouwbare en handige manier om over te dragen van een DNS-zone of naar Azure DNS.

Azure DNS ondersteunt importeren en exporteren van zone-bestanden met behulp van de Azure-opdrachtregelinterface (CLI). Importeren is **niet** die momenteel worden ondersteund via Azure PowerShell of Azure portal.

De Azure-CLI is een platformoverschrijdende opdrachtregelprogramma dat wordt gebruikt voor het beheren van Azure-services. Het is beschikbaar voor Windows, Mac en Linux-platforms van de [pagina Azure downloads](https://azure.microsoft.com/downloads/). Ondersteuning voor meerdere platforms is belangrijk voor het importeren en exporteren van zonebestanden, omdat de meest voorkomende naam server-software, [BINDEN](https://www.isc.org/downloads/bind/), meestal op Linux wordt uitgevoerd.


## <a name="obtain-your-existing-dns-zone-file"></a>Uw bestaande DNS-zonebestand verkrijgen

Voordat u een DNS-zonebestand in Azure DNS importeren, moet u een exemplaar van de zonebestand. De bron van dit bestand is afhankelijk van waar de DNS-zone dat momenteel wordt gehost.

* Als uw DNS-zone wordt gehost door een partnerservice (zoals een domeinregistrar, toegewezen DNS-hostingprovider of andere cloudprovider), moet deze service de mogelijkheid voor het downloaden van de DNS-zonebestand bieden.
* Als uw DNS-zone wordt gehost op Windows-DNS, de standaardmap voor de zonebestanden is **%systemroot%\system32\dns**. Het volledige pad naar elke zonebestand wordt ook weergegeven op de **algemene** tabblad van de DNS-console.
* Als uw DNS-zone wordt gehost met behulp van BIND, de locatie van de zonebestand voor elke zone die is opgegeven in het configuratiebestand van de binding **named.conf**.

> [!NOTE]
> Zone-bestanden die zijn gedownload via godaddy is gekocht hebben een iets andere indeling. U moet deze corrigeren voordat u deze zonebestanden in Azure DNS importeren.
>
> DNS-namen in de RDATA van elke DNS-record als FQDN-namen zijn opgegeven, maar ze niet beschikken over een afsluitende "." Dit betekent dat ze worden geïnterpreteerd door andere DNS-systemen als relatieve namen. U moet de zonebestand om toe te voegen de afsluitende bewerken '. ' in hun naam voordat u ze in Azure DNS importeren.
>
> Bijvoorbeeld, moet de CNAME-record 'www 3600 IN CNAME contoso.com' worden gewijzigd in "www 3600 IN contoso.com CNAME."
> (met een afsluitende '. ').

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Een DNS-zonebestand importeren in Azure DNS

Importeren van een zonebestand maakt u een nieuwe zone in Azure DNS als deze niet al bestaat. Als de zone al bestaat, moeten de recordsets in de zonebestand worden samengevoegd met de bestaande recordsets.

### <a name="merge-behavior"></a>Gedrag samenvoegen

* Standaard, worden bestaande en nieuwe recordsets samengevoegd. Identieke records in een samengevoegde Recordset zijn ongedaan maken gedupliceerd.
* Als recordsets worden samengevoegd, wordt de time to live (TTL) van de bestaande recordsets gebruikt.
* Start Authority (SOA)-parameters (met uitzondering van `host`) altijd zijn afkomstig uit het geïmporteerde zonebestand. Op dezelfde manier voor de naamserverrecord instellen in de apex van de zone, is de TTL-waarde altijd afkomstig uit het geïmporteerde zonebestand.
* Een geïmporteerde CNAME-record niet vervangen door een bestaande CNAME-record met dezelfde naam.  
* Als een conflict optreedt tussen een CNAME-record en een andere record met dezelfde naam maar met een ander type (ongeacht welke is bestaande of nieuwe), wordt de bestaande record bewaard. 

### <a name="additional-information-about-importing"></a>Als u meer informatie over het importeren

De volgende opmerkingen bieden aanvullende technische gegevens over de zone importeren.

* De `$TTL` richtlijn is optioneel en wordt ondersteund. Als er geen `$TTL` richtlijn wordt vermeld, zonder een expliciete TTL-records zijn geïmporteerd is ingesteld op een standaard-TTL van 3600 seconden. Wanneer twee records in de dezelfde recordset verschillende TTL opgeeft, wordt de lagere waarde gebruikt.
* De `$ORIGIN` richtlijn is optioneel en wordt ondersteund. Als er geen `$ORIGIN` is ingesteld, de standaardwaarde gebruikt, is de naam van de zone die is opgegeven op de opdrachtregel (plus de wordt beëindigd '. ').
* De `$INCLUDE` en `$GENERATE` richtlijnen worden niet ondersteund.
* Deze typen worden ondersteund: A, AAAA, CNAME, MX, NS, SOA, SRV en TXT.
* De SOA-record wordt automatisch gemaakt door Azure DNS wanneer een zone wordt gemaakt. Wanneer u een zonebestand importeert, alle SOA-parameters zijn afkomstig uit het zonebestand *behalve* de `host` parameter. Deze parameter wordt de waarde die is geleverd door Azure DNS. Dit is omdat deze parameter naar de primaire naamserver verstrekt door Azure DNS verwijzen moet.
* De naam van server-recordset in de apex van de zone wordt ook automatisch gemaakt door Azure DNS wanneer de zone wordt gemaakt. Alleen de TTL van deze recordset is geïmporteerd. Deze records bevatten de namen van de naam verstrekt door Azure DNS. De gegevens van de record niet wordt overschreven door de waarden die zijn opgenomen in de geïmporteerde zonebestand.
* Azure DNS biedt ondersteuning voor slechts één tekenreeks TXT-records tijdens de openbare Preview. Multistring TXT-records worden samengevoegd en afgekapt tot 255 tekens.

### <a name="cli-format-and-values"></a>CLI-indeling en -waarden

De indeling van de Azure CLI-opdracht voor het importeren van een DNS-zone is:

```azurecli
az network dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Waarden:

* `<resource group>` is de naam van de resourcegroep voor de zone in Azure DNS.
* `<zone name>` is de naam van de zone.
* `<zone file name>` is de padnaam van de zone dat u wilt importeren.

Als een zone met deze naam niet in de resourcegroep bestaat, wordt deze voor u gemaakt. Als de zone al bestaat, worden de geïmporteerde recordsets worden samengevoegd met bestaande recordsets. 


### <a name="step-1-import-a-zone-file"></a>Step 1. Een zonebestand importeren

Voor het importeren van een zonebestand voor de zone **contoso.com**.

1. Als u er nog geen hebt, moet u een Resource Manager-resourcegroep maken.

    ```azurecli
    az group create --group myresourcegroup -l westeurope
    ```

2. Voor het importeren van de zone **contoso.com** uit het bestand **contoso.com.txt** in een nieuwe DNS-zone in de resourcegroep **myresourcegroup**, u de opdracht wordt uitgevoerd `az network dns zone import` .<BR>Met deze opdracht wordt de zonebestand geladen en parseert deze. De opdracht wordt een reeks opdrachten uitgevoerd op de Azure DNS-service voor het maken van de zone en alle recordsets in de zone. De opdracht wordt uitgevoerd in het consolevenster, samen met eventuele fouten of waarschuwingen gemeld. Omdat recordsets in serie worden gemaakt, duurt het enkele minuten om een grote zone-bestand te importeren.

    ```azurecli
    az network dns zone import -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a>Stap 2. Controleer of de zone

Als u wilt controleren of de DNS-zone nadat u het bestand importeert, kunt u een van de volgende methoden:

* U kunt de records weergeven met behulp van de volgende Azure CLI-opdracht:

    ```azurecli
    az network dns record-set list -g myresourcegroup -z contoso.com
    ```

* U kunt de records weergeven met behulp van de PowerShell-cmdlet `Get-AzureRmDnsRecordSet`.
* U kunt `nslookup` om te controleren of de naamomzetting voor de records. Omdat de zone nog niet is toegewezen, moet u de juiste DNS-naamservers expliciet opgeven. Het volgende voorbeeld laat zien hoe de namen van de naam toegewezen aan de zone worden opgehaald. Dit ook wordt uitgelegd hoe u de record 'www' query's uitvoeren met behulp van `nslookup`.

    ```azurecli
    az network dns record-set ns list -g myresourcegroup -z  --output json 
    ```

    ```json
    [
      {
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
    ```

    ```cmd
    nslookup www.contoso.com ns1-03.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221
    ```

### <a name="step-3-update-dns-delegation"></a>Stap 3. DNS-delegering bijwerken

Nadat u hebt gecontroleerd of de zone correct zijn geïmporteerd, moet u de DNS-delegering om te verwijzen naar de DNS-naamservers bijwerken. Zie voor meer informatie het artikel [bijwerken van de DNS-delegering](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Een DNS-zonebestand exporteren uit Azure DNS

De indeling van de Azure CLI-opdracht voor het importeren van een DNS-zone is:

```azurecli
az network dns zone export -g <resource group> -n <zone name> -f <zone file name>
```

Waarden:

* `<resource group>` is de naam van de resourcegroep voor de zone in Azure DNS.
* `<zone name>` is de naam van de zone.
* `<zone file name>` is de padnaam van het zonebestand moet worden geëxporteerd.

Als met het importeren van de zone, moet u eerst aanmelden, kies uw abonnement, en configureert de Azure CLI voor het gebruik van Resource Manager-modus.

### <a name="to-export-a-zone-file"></a>Een zonebestand exporteren

Voor het exporteren van de bestaande Azure DNS-zone **contoso.com** in resourcegroep **myresourcegroup** naar het bestand **contoso.com.txt** uitvoeren (in de huidige map), `azure network dns zone export`. Met deze opdracht wordt de service Azure DNS-recordsets in de zone inventariseren en de resultaten exporteren naar een bestand van de zone BIND-compatibel.

    ```
    az network dns zone export -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```
