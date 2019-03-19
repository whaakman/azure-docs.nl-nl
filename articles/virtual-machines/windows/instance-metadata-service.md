---
title: Azure Instance Metadata Service | Microsoft Docs
description: RESTful-interface voor informatie over Windows-VM's berekening, netwerk en geplande onderhoudsgebeurtenissen.
services: virtual-machines-windows
documentationcenter: ''
author: KumariSupriya
manager: harijayms
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/15/2019
ms.author: sukumari
ms.reviewer: azmetadata
ms.openlocfilehash: 8cdf8022f87c8fa3e81e2544a6678751726b2b3b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57889825"
---
# <a name="azure-instance-metadata-service"></a>Azure Instance Metadata service

De Azure Instance Metadata Service bevat informatie over het uitvoeren van de exemplaren van de virtuele machine die kunnen worden gebruikt om te beheren en configureren uw virtuele machines.
Dit omvat gegevens zoals SKU, netwerkconfiguratie en geplande onderhoudsgebeurtenissen. Zie voor meer informatie over welk type informatie beschikbaar is, [metagegevens categorieën](#instance-metadata-data-categories).

Van Azure Instance Metadata Service is een REST-eindpunt dat toegankelijk is voor alle IaaS-VM's die zijn gemaakt via de [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).
Het eindpunt is beschikbaar op een bekende niet-routeerbare IP-adres (`169.254.169.254`) die kunnen worden gebruikt alleen de virtuele machine.

> [!IMPORTANT]
> Deze service is **algemeen beschikbaar** in alle Azure-regio's.  Deze ontvangt regelmatig updates om nieuwe informatie over VM-exemplaren weer te geven. Deze pagina geeft de actuele [gegevenscategorieën](#instance-metadata-data-categories) beschikbaar.

## <a name="service-availability"></a>Beschikbaarheid van services

De service is beschikbaar in de algemeen beschikbare Azure-regio's. Niet alle API-versie is mogelijk beschikbaar in alle Azure-regio's.

Regio's                                        | Beschikbaarheid?                                 | Ondersteunde versies
-----------------------------------------------|-----------------------------------------------|-----------------
[Alle globale Azure regio's algemeen beschikbaar](https://azure.microsoft.com/regions/)     | Algemeen verkrijgbaar   | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | Algemeen verkrijgbaar | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure China](https://www.azure.cn/)                                                           | Algemeen verkrijgbaar | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/)                    | Algemeen verkrijgbaar | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01

Deze tabel wordt bijgewerkt wanneer er service-updates beschikbaar zijn en of nieuwe ondersteunde versies zijn beschikbaar.

Als u wilt de Instance Metadata Service uitproberen, maakt u een VM op basis van [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) of de [Azure-portal](https://portal.azure.com) in de bovenstaande regio's en volg de onderstaande voorbeelden.

## <a name="usage"></a>Gebruik

### <a name="versioning"></a>Versiebeheer

De Instance Metadata Service is samengesteld. Versies zijn verplicht en de huidige versie op algemene Azure `2018-10-01`. Huidige ondersteunde versies zijn (2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 10-01-2018)

Als nieuwe versies worden toegevoegd, oudere versies nog steeds toegankelijk voor compatibiliteit als uw scripts afhankelijk van de opmaak van bepaalde gegevens zijn.

Als er geen versie wordt opgegeven, wordt een fout geretourneerd met een lijst met de nieuwste ondersteunde versies.

> [!NOTE]
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance"
```

**Antwoord**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2018-10-01",
        "2018-04-02",
        "2018-02-01"
    ]
}
```

### <a name="using-headers"></a>Met behulp van headers

Wanneer u een query de Instance Metadata Service, moet u de header `Metadata: true` om te controleren of de aanvraag is niet per ongeluk wordt omgeleid.

### <a name="retrieving-metadata"></a>Bij het ophalen van metagegevens

Metagegevens van het exemplaar is beschikbaar voor het uitvoeren van virtuele machines die zijn gemaakt of worden beheerd met behulp van [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Toegang tot alle gegevenscategorieën voor een VM-exemplaar met behulp van de volgende aanvraag:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

> [!NOTE]
> Alle exemplaar metagegevensquery's zijn hoofdlettergevoelig.

### <a name="data-output"></a>De gegevensuitvoer
Standaard de Instance Metadata Service gegevens geretourneerd in JSON-indeling (`Content-Type: application/json`). Verschillende API's retourneren echter gegevens in verschillende indelingen als aangevraagd.
De volgende tabel bevat een verwijzing van de opmaak van andere gegevens die API 's kunnen ondersteunen.

API | Standaardindeling voor gegevens | Andere indelingen
--------|---------------------|--------------
/ Instance | json | tekst
/scheduledevents | json | geen
/ blijkt | json | geen

Geef de vereiste indeling als een queryreeks-parameter in de aanvraag voor toegang tot een niet-standaard antwoordindeling. Bijvoorbeeld:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

### <a name="security"></a>Beveiliging

Het Instance Metadata Service-eindpunt is alleen toegankelijk vanuit het actieve exemplaar van de virtuele machine op een niet-routeerbare IP-adres. Bovendien een aanvraag met een `X-Forwarded-For` header is geweigerd door de service.
Aanvragen moeten ook bevatten een `Metadata: true` header om ervoor te zorgen dat de werkelijke aanvraag rechtstreeks bestemd en niet een deel van onbedoelde omleiding is.

### <a name="error"></a>Fout

Als er een gegevenselement niet gevonden of een onjuist gevormde aanvraag, retourneert de Instance Metadata Service standaard HTTP-fouten. Bijvoorbeeld:

HTTP-statuscode | Reden
----------------|-------
200 OK |
400-Ongeldige aanvraag | Ontbrekende `Metadata: true` koptekst
404 – Niet gevonden | Het gevraagde element bestaat niet 
405 methode is niet toegestaan | Alleen `GET` en `POST` aanvragen worden ondersteund
429 te veel aanvragen | De API ondersteunt momenteel een maximum van 5 query's per seconde
500 servicefout     | Voer na enige tijd opnieuw uit

### <a name="examples"></a>Voorbeelden

> [!NOTE]
> Alle API-antwoorden zijn JSON-tekenreeksen. Alle volgende voorbeeldantwoorden zijn behoorlijk afgedrukt voor de leesbaarheid.

#### <a name="retrieving-network-information"></a>Bij het ophalen van informatie over het netwerk

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Antwoord**

> [!NOTE]
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Bij het ophalen van openbare IP-adres

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Het ophalen van alle metagegevens voor een exemplaar

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"
```

**Antwoord**

> [!NOTE]
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

```json
{
  "compute": {
    "azEnvironment": "AZUREPUBLICCLOUD",
    "location": "westus",
    "name": "jubilee",
    "offer": "Windows-10",
    "osType": "Windows",
    "placementGroupId": "",
    "plan": {
        "name": "",
        "product": "",
        "publisher": ""
    },
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "MicrosoftWindowsDesktop",
    "resourceGroupName": "myrg",
    "sku": "rs4-pro",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "17134.345.59",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_D1",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Bij het ophalen van metagegevens in Windows virtuele Machine

**Aanvraag**

Metagegevens van het exemplaar kan worden opgehaald in Windows via het `curl` programma:

```bash
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2018-10-01 | select -ExpandProperty Content
```

Of via de `Invoke-RestMethod` PowerShell-cmdlet:

```powershell

Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2018-10-01 -Method get
```

**Antwoord**

> [!NOTE]
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

```json
{
  "compute": {
    "azEnvironment": "AZUREPUBLICCLOUD",
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "placementGroupId": "",
    "plan": {
        "name": "",
        "product": "",
        "publisher": ""
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "MicrosoftSQLServer",
    "resourceGroupName": "myrg",
    "sku": "Enterprise",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmScaleSetName": "",
    "vmSize": "Standard_DS2",
    "zone": ""
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Exemplaar metagegevens gegevenscategorieën

De volgende gegevenscategorieën zijn beschikbaar via de Instance Metadata Service:

Gegevens | Description | Versie geïntroduceerd
-----|-------------|-----------------------
azEnvironment | Azure-omgeving waarop de virtuele machine wordt uitgevoerd in | 10-01-2018
location | Azure-regio de virtuele machine wordt uitgevoerd in | 2017-04-02
naam | Naam van de virtuele machine | 2017-04-02
aanbieding | Biedt informatie over de VM-installatiekopie. Deze waarde is alleen aanwezig zijn voor installatiekopieën die zijn geïmplementeerd vanuit de galerie met installatiekopieën van Azure. | 2017-04-02
Uitgever | Uitgever van de VM-installatiekopie | 2017-04-02
sku | Specifieke SKU voor de VM-installatiekopie | 2017-04-02
versie | Versie van de VM-installatiekopie | 2017-04-02
besturingssysteemtype | Linux of Windows | 2017-04-02
platformUpdateDomain |  [Updatedomein](manage-availability.md) in de virtuele machine wordt uitgevoerd | 2017-04-02
platformFaultDomain | [Foutdomein](manage-availability.md) in de virtuele machine wordt uitgevoerd | 2017-04-02
vmId | [De unieke id](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) voor de virtuele machine | 2017-04-02
vmSize | [VM-grootte](sizes.md) | 2017-04-02
subscriptionId | Azure-abonnement voor de virtuele Machine | 2017-08-01
tags | [Tags](../../azure-resource-manager/resource-group-using-tags.md) voor uw virtuele Machine  | 2017-08-01
resourceGroupName | [Resourcegroep](../../azure-resource-manager/resource-group-overview.md) voor uw virtuele Machine | 2017-08-01
placementGroupId | [Plaatsingsgroep](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) instellen van uw virtuele-machineschaalset | 2017-08-01
plan | [Plan](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) voor een virtuele machine in de een Azure Marketplace-installatiekopie, bevat de naam, product en uitgever | 2018-04-02
provider | Provider van de virtuele machine | 10-01-2018
publicKeys | Verzameling van openbare sleutels [<https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#sshpublickey>] toegewezen aan de virtuele machine en de paden | 2018-04-02
vmScaleSetName | [Naam van de virtuele Machine ScaleSet](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) instellen van uw virtuele-machineschaalset | 2017-12-01
zone | [Binnen een Beschikbaarheidszone](../../availability-zones/az-overview.md) van uw virtuele machine | 2017-12-01
ipv4/privateIpAddress | Lokale IPv4-adres van de virtuele machine | 2017-04-02
ipv4/publicIpAddress | Openbare IPv4-adres van de virtuele machine | 2017-04-02
subnet/address | Subnetadres van de virtuele machine | 2017-04-02
subnet/prefix | Het subnetvoorvoegsel, voorbeeld 24 uur per dag | 2017-04-02
ipv6/ipAddress | Lokale IPv6-adres van de virtuele machine | 2017-04-02
MAC-adres | VM-mac-adres | 2017-04-02
scheduledevents | Zie [geplande gebeurtenissen](scheduled-events.md) | 2017-08-01
identity | Beheerde identiteiten voor Azure-resources. Zie [een toegangstoken verkrijgen](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01
Attestation | Zie [Attestation-gegevens](#attested-data) | 10-01-2018

## <a name="attested-data"></a>Blijkt gegevens

Metagegevens van het exemplaar reageert op http-eindpunt op 169.254.169.254. Onderdeel van het scenario dat wordt bediend door Instance Metadata Service is het bieden van garanties dat de gegevens heeft gereageerd afkomstig is van Azure. We een deel van deze informatie ondertekenen zodat marketplace-installatiekopieën ervoor dat deze de installatiekopie die wordt uitgevoerd op Azure kunnen worden.

### <a name="example-attested-data"></a>Voorbeeld van de Attestation-gegevens

 > [!NOTE]
> Alle API-antwoorden zijn JSON-tekenreeksen. Het volgende voorbeeldantwoorden zijn pretty afgedrukt voor de leesbaarheid.

 **Aanvraag**

 ```bash
curl -H Metadata:true "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"

```

API-versie is een verplicht veld zijn en de versie die wordt ondersteund voor blijkt gegevens 2018-01-10 is.
Nonce is een optionele 10 cijfers tekenreeks opgegeven. Nonce kan worden gebruikt voor het bijhouden van de aanvraag en indien niet opgegeven, in antwoord gecodeerde tekenreeks, de huidige UTC tijdstempel wordt geretourneerd.

 **Antwoord**

> [!NOTE]
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

> De handtekening-blob is een [pkcs7](https://aka.ms/pkcs7) versie van het document is ondertekend. Het bevat het certificaat voor ondertekening, samen met de details van de virtuele machine, zoals vmId, nonce, timeStamp voor het maken en de vervaldatum van het document en de schema-informatie over de installatiekopie van het. De schema-informatie wordt alleen ingevuld voor installatiekopieën van Azure-markt plaats. Het certificaat kan worden opgehaald uit het antwoord en gebruikt om te valideren dat de reactie geldig is en afkomstig is van Azure.

#### <a name="retrieving-attested-metadata-in-windows-virtual-machine"></a>Bij het ophalen van blijkt metagegevens in Windows virtuele Machine

 **Aanvraag**

Metagegevens van het exemplaar in Windows kan worden opgehaald via de PowerShell-hulpprogramma `curl`:

 ```bash
curl -H @{'Metadata'='true'} "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" | select -ExpandProperty Content
```

 Of via de `Invoke-RestMethod` cmdlet:

 ```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" -Method get
```

API-versie is een verplicht veld zijn en de versie die wordt ondersteund voor blijkt gegevens 2018-01-10 is.
Nonce is een optionele 10 cijfers tekenreeks opgegeven. Nonce kan worden gebruikt voor het bijhouden van de aanvraag en indien niet opgegeven, in antwoord gecodeerde tekenreeks, de huidige UTC tijdstempel wordt geretourneerd.

 **Antwoord**

> [!NOTE]
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

> De handtekening-blob is een [pkcs7](https://aka.ms/pkcs7) versie van het document is ondertekend. Het bevat het certificaat voor ondertekening, samen met de details van de virtuele machine, zoals vmId, nonce, timeStamp voor het maken en de vervaldatum van het document en de schema-informatie over de installatiekopie van het. De schema-informatie wordt alleen ingevuld voor installatiekopieën van Azure-markt plaats. Het certificaat kan worden opgehaald uit het antwoord en gebruikt om te valideren dat de reactie geldig is en afkomstig is van Azure.

## <a name="example-scenarios-for-usage"></a>Voorbeeldscenario's voor gebruik  

### <a name="tracking-vm-running-on-azure"></a>Bijhouden van de VM wordt uitgevoerd op Azure

Als een serviceprovider, hebt u mogelijk nodig bij te houden van het aantal virtuele machines met uw software of agents die nodig hebt om bij te houden van uniekheid van de virtuele machine hebben. Als u een unieke ID voor een virtuele machine ophalen, gebruikt u de `vmId` veld Instance Metadata Service.

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Antwoord**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Plaatsing van containers, gegevens-partities op basis van fouten/updatedomein 

Voor bepaalde scenario's, plaatsing van replica's van andere gegevens is van belang is aangewezen. Bijvoorbeeld, [HDFS replica plaatsing](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) of de positie van de container via een [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) kan u nodig hebt om te weten de `platformFaultDomain` en `platformUpdateDomain` op de virtuele machine wordt uitgevoerd.
U kunt ook [Beschikbaarheidszones](../../availability-zones/az-overview.md) voor de exemplaren van deze beslissingen te nemen.
U kunt deze gegevens rechtstreeks via de Instance Metadata Service op te vragen.

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Antwoord**

```text
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Meer informatie over de virtuele machine tijdens de ondersteuningsaanvraag ophalen

Als een serviceprovider krijgt u mogelijk een telefoongesprek ondersteuning waar u wilt meer informatie over de virtuele machine. Waarin wordt gevraagd de klant voor het delen van de metagegevens van de compute krijgt u algemene informatie voor de ondersteuning van professional op de hoogte van het type van de virtuele machine op Azure. 

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-08-01"
```

**Antwoord**

> [!NOTE]
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

```json
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="getting-azure-environment-where-the-vm-is-running"></a>Ophalen van Azure-omgeving waarop de virtuele machine wordt uitgevoerd

Azure heeft verschillende onafhankelijke clouds, zoals [Azure Government](https://azure.microsoft.com/overview/clouds/government/). Soms moet u de Azure-omgeving om sommige runtime beslissingen te nemen. Het volgende voorbeeld laat zien hoe u dit gedrag kunt bereiken.

**Aanvraag**
``` bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Antwoord**
```
AZUREPUBLICCLOUD
```

### <a name="validating-that-the-vm-is-running-in-azure"></a>Valideren dat de virtuele machine wordt uitgevoerd in Azure

 Marketplace-leveranciers ervoor wilt zorgen dat de software is gelicentieerd voor het alleen worden uitgevoerd in Azure. Als iemand de VHD uit naar kopieert on-premises, klikt u vervolgens ze moeten de mogelijkheid hebben om te detecteren die. Aanroepen in Instance Metadata Service Marketplace krijgt leveranciers ondertekende gegevens die alleen Azure-antwoord wordt gegarandeerd.
**Aanvraag**
> [!NOTE]
> Vereist jq moet worden geïnstalleerd.

 ```bash
  # Get the signature
   curl  --silent -H Metadata:True http://169.254.169.254/metadata/attested/document?api-version=2018-10-01 | jq -r '.["signature"]' > signature
  # Decode the signature
  base64 -d signature > decodedsignature
  #Get PKCS7 format
  openssl pkcs7 -in decodedsignature -inform DER -out sign.pk7
  # Get Public key out of pkc7
  openssl pkcs7 -in decodedsignature -inform DER  -print_certs -out signer.pem
  #Get the intermediate certificate
  wget -q -O intermediate.cer "$(openssl x509 -in signer.pem -text -noout | grep " CA Issuers -" | awk -FURI: '{print $2}')"
  openssl x509 -inform der -in intermediate.cer -out intermediate.pem
  #Verify the contents
  openssl smime -verify -in sign.pk7 -inform pem -noverify
 ```

 **Antwoord**

```json
Verification successful
{"nonce":"20181128-001617",
  "plan":
    {
     "name":"",
     "product":"",
     "publisher":""
    },
"timeStamp":
  {
    "createdOn":"11/28/18 00:16:17 -0000",
    "expiresOn":"11/28/18 06:16:17 -0000"
  },
"vmId":"d3e0e374-fda6-4649-bbc9-7f20dc379f34"
}
```

Gegevens | Description
-----|------------
nonce | Gebruiker opgegeven optionele tekenreeks met de aanvraag. Als er geen nonce is opgegeven in de aanvraag, wordt de huidige UTC-timestamp geretourneerd
plan | [Plan](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) voor een virtuele machine in het is een Azure Marketplace-installatiekopie, bevat de naam, product en uitgever
timestamp/createdOn | De tijdstempel waarmee het eerste ondertekende document is gemaakt
timestamp/expiresOn | De tijdstempel die het ondertekende document is verlopen
vmId |  [De unieke id](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) voor de virtuele machine

#### <a name="verifying-the-signature"></a>Verifiëren van de handtekening

Wanneer u de bovenstaande handtekening, kunt u controleren of de handtekening van Microsoft is. U kunt ook controleren of het tussenliggende certificaat en de certificaatketen.

> [!NOTE]
> Het certificaat voor de openbare cloud en onafhankelijke Clouds zijn verschillend.

 Regio's | Certificaat
---------|-----------------
[Alle globale Azure regio's algemeen beschikbaar](https://azure.microsoft.com/regions/)     | metadata.azure.com
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | metadata.azure.us
[Azure China](https://www.azure.cn/)                                                           | metadata.azure.cn
[Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/)                    | metadata.microsoftazure.de

```bash

# Verify the subject name for the main certificate
openssl x509 -noout -subject -in signer.pem
# Verify the issuer for the main certificate
openssl x509 -noout -issuer -in signer.pem
#Validate the subject name for intermediate certificate
openssl x509 -noout -subject -in intermediate.pem
# Verify the issuer for the intermediate certificate
openssl x509 -noout -issuer -in intermediate.pem
# Verify the certificate chain
openssl verify -verbose -CAfile /etc/ssl/certs/Baltimore_CyberTrust_Root.pem -untrusted intermediate.pem signer.pem
```

### <a name="failover-clustering-in-windows-server"></a>Failover Clustering in WindowsServer

Voor bepaalde scenario's, bij het opvragen van Instance Metadata Service met Failover Clustering, dit is nodig om een route toevoegen aan de routeringstabel.

1. Open de opdrachtprompt met beheerdersbevoegdheden.

2. Voer de volgende opdracht uit en noteer het adres van de Interface voor het netwerkadres (`0.0.0.0`) in de routetabel voor IPv4.

```bat
route print
```

> [!NOTE] 
> De volgende voorbeelduitvoer van een Windows Server-VM met Failover-Cluster ingeschakeld bevat alleen de IPv4-Route-Table voor het gemak.

```bat
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0         10.0.1.1        10.0.1.10    266
         10.0.1.0  255.255.255.192         On-link         10.0.1.10    266
        10.0.1.10  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.15  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.63  255.255.255.255         On-link         10.0.1.10    266
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link     169.254.1.156    271
    169.254.1.156  255.255.255.255         On-link     169.254.1.156    271
  169.254.255.255  255.255.255.255         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         10.0.1.10    266
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         10.0.1.10    266
```

1. Voer de volgende opdracht uit en gebruik het adres van de Interface voor doel-netwerk (`0.0.0.0`) die is (`10.0.1.10`) in dit voorbeeld.

```bat
route add 169.254.169.254/32 10.0.1.10 metric 1 -p
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>Voorbeelden van het aanroepen van metadata-service met behulp van verschillende talen in de virtuele machine 

Taal | Voorbeeld
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Aan de slag  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
Javascript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
Visual Basic | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb
Puppet | https://github.com/keirans/azuremetadata

## <a name="faq"></a>Veelgestelde vragen

1. Ik krijg de fout `400 Bad Request, Required metadata header not specified`. Wat betekent dit?
   * De Instance Metadata Service is vereist voor de header `Metadata: true` moeten worden doorgegeven in de aanvraag. Deze header doorgeven in de REST-aanroep biedt toegang tot de Instance Metadata Service.
2. Waarom krijg ik geen compute-informatie voor mijn VM?
   * De Instance Metadata Service ondersteunt momenteel alleen exemplaren met Azure Resource Manager gemaakt. In de toekomst ondersteuning voor virtuele machines van Cloud Service kan worden toegevoegd.
3. Ik heb mijn virtuele Machine via Azure Resource Manager een tijd weer gemaakt. Waarom kan ik niet Zie compute-metagegevens?
   * Voor virtuele machines die na Sep 2016 zijn gemaakt, voegt u toe een [Tag](../../azure-resource-manager/resource-group-using-tags.md) om te zien starten compute-metagegevens. Voor oudere VM's (gemaakt voordat Sep 2016), toevoegen/verwijderen-extensies of gegevens schijven aan de virtuele machine metagegevens vernieuwen.
4. Ik zie niet alle gegevens voor de nieuwe versie is ingevuld
   * Voor virtuele machines die na Sep 2016 zijn gemaakt, voegt u toe een [Tag](../../azure-resource-manager/resource-group-using-tags.md) om te zien starten compute-metagegevens. Voor oudere VM's (gemaakt voordat Sep 2016), toevoegen/verwijderen-extensies of gegevens schijven aan de virtuele machine metagegevens vernieuwen.
5. Waarom krijg ik de fout `500 Internal Server Error`?
   * Probeer uw aanvraag op basis van exponentieel uitstel system. Neem contact op met ondersteuning van Azure als het probleem zich blijft voordoen.
6. Waar kan ik delen als u meer vragen/opmerkingen?
   * Stuur uw opmerkingen op https://feedback.azure.com.
7. Werkt dit voor instellen-instantie van virtuele-Machineschaalset?
   * Ja is Metadata-service beschikbaar voor Schalingsinstanties instellen.
8. Hoe krijg ik ondersteuning voor de service?
   * Voor ondersteuning voor de service, moet u een Ondersteuningsprobleem maken in Azure-portal voor de virtuele machine waar u zijn niet in staat om op te halen van de reactie met metagegevens na lange pogingen.
9. Kan ik er is een time-out opgetreden voor de aanroep naar de service krijgen?
   * Metagegevens-aanroepen moeten worden gemaakt van het primaire IP-adres toegewezen aan de netwerkkaart van de virtuele machine, ook als u hebt gewijzigd uw routes er moeten een route voor 169.254.0.0/16 adres buiten uw netwerkkaart.
10. Kan ik mijn labels in virtuele-machineschaalset bijgewerkt, maar ze niet weergegeven in de exemplaren in tegenstelling tot virtuele machines?
    * Op dit moment voor ScaleSets weergegeven labels alleen op de virtuele machine op een opnieuw opstarten/terugzetten van een installatiekopie/of een schijf met het exemplaar wijzigen.

    ![Ondersteuning voor Instance-Metagegevens](./media/instance-metadata-service/InstanceMetadata-support.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [geplande gebeurtenissen](scheduled-events.md)
