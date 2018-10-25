---
title: Azure Instance Metadata Service | Microsoft Docs
description: RESTful-interface voor informatie over Linux-VM berekening, netwerk en geplande onderhoudsgebeurtenissen.
services: virtual-machines-linux
documentationcenter: ''
author: harijayms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: harijayms
ms.openlocfilehash: 77b19b708b32003edc4555745a233a01d6f60b71
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026276"
---
# <a name="azure-instance-metadata-service"></a>Azure Instance Metadata service


De Azure Instance Metadata Service bevat informatie over het uitvoeren van de exemplaren van de virtuele machine die kunnen worden gebruikt om te beheren en configureren uw virtuele machines.
Dit omvat gegevens zoals SKU, netwerkconfiguratie en geplande onderhoudsgebeurtenissen. Zie voor meer informatie over welk type informatie beschikbaar is, [metagegevens categorieën](#instance-metadata-data-categories).

Van Azure Instance Metadata Service is een REST-eindpunt dat toegankelijk is voor IaaS-VM's die zijn gemaakt via de [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Het eindpunt is beschikbaar op een bekende niet-routeerbare IP-adres (`169.254.169.254`) die kunnen worden gebruikt alleen de virtuele machine.

> [!IMPORTANT]
> Deze service is **algemeen beschikbaar** in Azure-regio's.  Deze ontvangt regelmatig updates om nieuwe informatie over VM-exemplaren weer te geven. Deze pagina geeft de actuele [gegevenscategorieën](#instance-metadata-data-categories) beschikbaar.

## <a name="service-availability"></a>Beschikbaarheid van services
De service is beschikbaar in de algemeen beschikbare Azure-regio's. Niet alle API-versie is mogelijk beschikbaar in alle Azure-regio's.

Regio's                                        | Beschikbaarheid?                                 | Ondersteunde versies
-----------------------------------------------|-----------------------------------------------|-----------------
[Alle globale Azure regio's algemeen beschikbaar](https://azure.microsoft.com/regions/)     | Algemeen verkrijgbaar   | 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02-2017-04-02
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | Algemeen verkrijgbaar | 2017-04-02, 2017-08-01, 2017-12-01, 01-02-2018
[Azure China](https://www.azure.cn/)                                                           | Algemeen verkrijgbaar | 2017-04-02, 2017-08-01, 2017-12-01, 01-02-2018
[Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/)                    | Algemeen verkrijgbaar | 2017-04-02, 2017-08-01, 2017-12-01, 01-02-2018

Deze tabel wordt bijgewerkt wanneer er service-updates beschikbaar zijn en of nieuwe ondersteunde versies zijn beschikbaar

Als u wilt de Instance Metadata Service uitproberen, maakt u een VM op basis van [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) of de [Azure-portal](http://portal.azure.com) in de bovenstaande regio's en volg de onderstaande voorbeelden.

## <a name="usage"></a>Gebruik

### <a name="versioning"></a>Versiebeheer
De Instance Metadata Service is samengesteld. Versies zijn verplicht en de huidige versie op algemene Azure `2018-04-02`. Huidige ondersteunde versies zijn (2017-04-02, 2017-08-01, 2017-12-01, 2018-04-02-2018-02-01)

> [!NOTE] 
> Eerdere versies van de Preview-versie van geplande gebeurtenissen {nieuwste} wordt ondersteund als de api-versie. Deze indeling wordt niet meer ondersteund en wordt in de toekomst afgeschaft.

Als nieuwe versies worden toegevoegd, oudere versies nog steeds toegankelijk voor compatibiliteit als uw scripts afhankelijk van de opmaak van bepaalde gegevens zijn. Echter, de vorige preview-versie (2017-03-01) mogelijk niet beschikbaar zodra de service algemeen beschikbaar is.

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

Geef de vereiste indeling als een queryreeksparameter in de aanvraag voor toegang tot een niet-standaard antwoordindeling. Bijvoorbeeld:

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
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-12-01"
```

**Antwoord**

> [!NOTE] 
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

```json
{
  "compute": {
    "location": "westus",
    "name": "avset2",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "placementGroupId": "",
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "publisher": "Canonical",
    "resourceGroupName": "myrg",
    "sku": "16.04-LTS",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "16.04.201708030",
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

Metagegevens van het exemplaar in Windows kan worden opgehaald via de PowerShell-hulpprogramma `curl`: 

```bash
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-08-01 | select -ExpandProperty Content
```

Of via de `Invoke-RestMethod` cmdlet:
    
```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-08-01 -Method get 
```

**Antwoord**

> [!NOTE] 
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeldantwoord is pretty afgedrukt voor de leesbaarheid.

```json
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
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
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Exemplaar metagegevens gegevenscategorieën
De volgende gegevenscategorieën zijn beschikbaar via de Instance Metadata Service:

Gegevens | Beschrijving | Versie geïntroduceerd 
-----|-------------|-----------------------
location | Azure-regio de virtuele machine wordt uitgevoerd in | 2017-04-02 
naam | Naam van de virtuele machine | 2017-04-02
aanbieding | Biedt informatie over de VM-installatiekopie. Deze waarde is alleen aanwezig zijn voor installatiekopieën die zijn geïmplementeerd vanuit de galerie met installatiekopieën van Azure. | 2017-04-02
Uitgever | Uitgever van de VM-installatiekopie | 2017-04-02
SKU | Specifieke SKU voor de VM-installatiekopie | 2017-04-02
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
plan | [Plan](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) voor een virtuele machine in het is een Azure Marketplace-installatiekopie, bevat de naam, product en uitgever | 2017-04-02
publicKeys | Verzameling van openbare sleutels [https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#sshpublickey] toegewezen aan de virtuele machine en de paden | 2017-04-02
vmScaleSetName | [Naam van de virtuele Machine ScaleSet](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) instellen van uw virtuele-machineschaalset | 2017-12-01
zone | [Binnen een Beschikbaarheidszone](../../availability-zones/az-overview.md) van uw virtuele machine | 2017-12-01 
IPv4/privateIpAddress | Lokale IPv4-adres van de virtuele machine | 2017-04-02
IPv4/publicIpAddress | Openbare IPv4-adres van de virtuele machine | 2017-04-02
subnetadres / | Subnetadres van de virtuele machine | 2017-04-02 
/ subnetvoorvoegsel | Het subnetvoorvoegsel, voorbeeld 24 uur per dag | 2017-04-02 
ipv6/ipAddress | Lokale IPv6-adres van de virtuele machine | 2017-04-02 
MAC-adres | VM-mac-adres | 2017-04-02 
scheduledevents | Zie [geplande gebeurtenissen](scheduled-events.md) | 2017-08-01
identity | (Preview) Beheerde identiteiten voor Azure-resources. Zie [een toegangstoken verkrijgen](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01 

## <a name="example-scenarios-for-usage"></a>Voorbeeldscenario's voor gebruik  

### <a name="tracking-vm-running-on-azure"></a>Bijhouden van de VM wordt uitgevoerd op Azure

Als een serviceprovider, hebt u mogelijk nodig bij te houden van het aantal virtuele machines met uw software of agents die nodig hebt om bij te houden van uniekheid van de virtuele machine hebben. Als u een unieke ID voor een virtuele machine ophalen, gebruikt u de `vmId` veld Instance Metadata Service.

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Antwoord**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Plaatsing van containers, gegevens-partities op basis van fouten/updatedomein 

Voor bepaalde scenario's, plaatsing van replica's van andere gegevens is van belang is aangewezen. Bijvoorbeeld, [HDFS replica plaatsing](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) of de positie van de container via een [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) kan u nodig hebt om te weten de `platformFaultDomain` en `platformUpdateDomain` op de virtuele machine wordt uitgevoerd.
U kunt ook gebruikmaken van [Beschikbaarheidszones](../../availability-zones/az-overview.md) voor de exemplaren van deze beslissingen te nemen. U kunt deze gegevens rechtstreeks via de Instance Metadata Service op te vragen.

**Aanvraag**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text" 
```

**Antwoord**

```
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

Azure heeft verschillende onafhankelijke clouds, zoals [Azure Government](https://azure.microsoft.com/overview/clouds/government/). Soms moet u de Azure-omgeving om sommige runtime beslissingen te nemen. Het volgende voorbeeld ziet u hoe u dit kunt doen.

**Aanvraag**

> [!NOTE] 
> Vereist jq moet worden geïnstalleerd. 

```bash
  metadata=$(curl "http://169.254.169.254/metadata/instance/compute?api-version=2018-02-01" -H "Metadata:true")
  endpoints=$(curl "https://management.azure.com/metadata/endpoints?api-version=2017-12-01")
 
  location=$(echo $metadata | jq .location -r)
 
  is_ww=$(echo $endpoints | jq '.cloudEndpoint.public.locations[]' -r | grep -w $location)
  is_us=$(echo $endpoints | jq '.cloudEndpoint.usGovCloud.locations[]' -r | grep -w $location)
  is_cn=$(echo $endpoints | jq '.cloudEndpoint.chinaCloud.locations[]' -r | grep -w $location)
  is_de=$(echo $endpoints | jq '.cloudEndpoint.germanCloud.locations[]' -r | grep -w $location)
 
  environment="Unknown"
  if [ ! -z $is_ww ]; then environment="AzureCloud"; fi
  if [ ! -z $is_us ]; then environment="AzureUSGovernment"; fi
  if [ ! -z $is_cn ]; then environment="AzureChinaCloud"; fi
  if [ ! -z $is_de ]; then environment="AzureGermanCloud"; fi
 
  echo $environment
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
   * Stuur uw opmerkingen op http://feedback.azure.com.
7. Werkt dit voor instellen-instantie van virtuele-Machineschaalset?
   * Ja is Metadata-service beschikbaar voor Schalingsinstanties instellen. 
8. Hoe krijg ik ondersteuning voor de service?
   * Voor ondersteuning voor de service, maakt u een Ondersteuningsprobleem in Azure-portal voor de virtuele machine waar u zijn niet in staat om op te halen van de reactie met metagegevens na lange pogingen 
9. Kan ik er is een time-out opgetreden voor de aanroep naar de service krijgen?
   * Metagegevens-aanroepen moeten worden gemaakt van het primaire IP-adres toegewezen aan de netwerkkaart van de virtuele machine, ook als u hebt gewijzigd uw routes er moeten een route voor 169.254.0.0/16 adres buiten uw netwerkkaart.
10. Kan ik mijn labels in Virtual Machine Scale set bijgewerkt, maar ze niet weergegeven in de exemplaren in tegenstelling tot virtuele machines?
   * Op dit moment voor ScaleSets weergegeven labels alleen op de virtuele machine op een opnieuw opstarten/terugzetten van een installatiekopie/of een schijf met het exemplaar wijzigen. 

   ![Ondersteuning voor Instance-Metagegevens](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [geplande gebeurtenissen](scheduled-events.md)

