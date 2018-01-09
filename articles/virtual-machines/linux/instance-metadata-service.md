---
title: Service voor Azure-instantie metagegevens | Microsoft Docs
description: RESTful-interface voor informatie over Linux-VM compute, netwerk en toekomstig onderhoud gebeurtenissen.
services: virtual-machines-linux
documentationcenter: 
author: harijayms
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: harijayms
ms.openlocfilehash: 40b684fe5681123f3c32d3984b2725f97b427f1b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="azure-instance-metadata-service"></a>Azure service van de metagegevens van het exemplaar


De Azure-Service-exemplaar metagegevens bevat informatie over het uitvoeren van de virtuele machine-exemplaren die kunnen worden gebruikt voor het beheren en configureren van uw virtuele machines.
Dit omvat gegevens zoals SKU netwerkconfiguratie en toekomstig onderhoud gebeurtenissen. Zie voor meer informatie over wat voor soort informatie beschikbaar is, [metagegevens categorieën](#instance-metadata-data-categories).

Service voor de metagegevens van Azure exemplaar is een REST-eindpunt toegankelijk voor alle IaaS VM's die worden gemaakt via de [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Het eindpunt is beschikbaar op een bekende niet-routeerbare IP-adres (`169.254.169.254`) die kan alleen worden benaderd binnen de virtuele machine.

> [!IMPORTANT]
> Deze service is **algemeen beschikbaar** in alle Azure-regio's.  Het ontvangt regelmatig updates om nieuwe informatie over de virtuele machine-exemplaren weer te geven. Deze pagina geeft de actuele [gegevenscategorieën](#instance-metadata-data-categories) beschikbaar.

## <a name="service-availability"></a>Beschikbaarheid van services
De service is beschikbaar in alle algemeen beschikbaar alle Azure-regio's. Niet alle API-versie is mogelijk beschikbaar in alle Azure-regio's.

Regio's                                        | Beschikbaarheid?                                 | Ondersteunde versies
-----------------------------------------------|-----------------------------------------------|-----------------
[Alle in het algemeen beschikbare globale Azure-regio 's](https://azure.microsoft.com/regions/)     | Algemeen beschikbaar   | 2017-04-02, 2017-08-01
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | Algemeen beschikbaar | 2017-04-02
[Azure China](https://www.azure.cn/)                                                           | Algemeen beschikbaar | 2017-04-02
[Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/)                    | Algemeen beschikbaar | 2017-04-02

Deze tabel wordt bijgewerkt wanneer er updates van de service en of nieuwe ondersteunde versies zijn beschikbaar

Maak een VM uit om de Service-exemplaar voor metagegevens uitproberen, [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) of de [Azure-portal](http://portal.azure.com) in de bovenstaande regio's en volg de onderstaande voorbeelden.

## <a name="usage"></a>Gebruik

### <a name="versioning"></a>Versiebeheer
De Service-exemplaar voor metagegevens is samengestelde. Versies zijn verplicht en de huidige versie op globale Azure is `2017-08-01`. Huidige ondersteunde versies zijn (2017-04-02, 2017-08-01)

> [!NOTE] 
> Eerdere versies van de preview van geplande gebeurtenissen {laatste} wordt ondersteund als de api-versie. Deze indeling wordt niet meer ondersteund en in de toekomst wordt afgeschaft.

Als er nieuwere versies toevoegt, oudere versies is nog steeds toegankelijk voor compatibiliteit als uw scripts afhankelijk van specifieke gegevensindelingen zijn. Houd er echter rekening mee dat het vorige voorbeeld version(2017-03-01) mogelijk niet beschikbaar zodra de service in het algemeen beschikbaar is.

### <a name="using-headers"></a>Met behulp van headers
Wanneer u de Service-exemplaar voor metagegevens query uitvoert, moet u de header geven `Metadata: true` om te controleren of de aanvraag is niet per ongeluk wordt omgeleid.

### <a name="retrieving-metadata"></a>Ophalen van metagegevens

Metagegevens van het exemplaar is beschikbaar voor het uitvoeren van virtuele machines die zijn gemaakt/beheerd met behulp van [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Toegang tot alle gegevenscategorieën voor een exemplaar van de virtuele machine met de volgende aanvraag:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> Alle query's voor exemplaar-metagegevens zijn hoofdlettergevoelig.

### <a name="data-output"></a>Data-uitvoer
Standaard stuurt de metagegevens-Service-exemplaar gegevens in JSON-indeling (`Content-Type: application/json`). Verschillende API's retourneren echter gegevens in verschillende indelingen als aangevraagd.
De volgende tabel bevat een verwijzing naar andere indelingen met de die API 's kunnen ondersteunen.

API | Standaardindeling voor gegevens | Andere indelingen
--------|---------------------|--------------
/Instance | JSON | Tekst
/scheduledevents | JSON | geen

Geef de vereiste indeling als een parameter van de querytekenreeks worden opgegeven in de aanvraag voor toegang tot een niet-standaard antwoordindeling moet. Bijvoorbeeld:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a>Beveiliging
Het exemplaar metagegevens Service-eindpunt is alleen toegankelijk vanuit de actieve sessie van de virtuele machine op een niet-routeerbare IP-adres. Bovendien verzoeken met een `X-Forwarded-For` header is geweigerd door de service.
Moet ook aanvragen bevatten een `Metadata: true` header om ervoor te zorgen dat de werkelijke aanvraag rechtstreeks bestemd en geen deel uit van onbedoelde omleiding is. 

### <a name="error"></a>Fout
Als er een gegevenselement niet gevonden of een onjuist gevormde aanvraag, retourneert de Service-exemplaar voor metagegevens standaard HTTP-fouten. Bijvoorbeeld:

HTTP-statuscode | Reden
----------------|-------
200 OK |
400 onjuiste aanvraag | Ontbrekende `Metadata: true` koptekst
404 – Niet gevonden | Het gevraagde element bestaat niet 
405 methode is niet toegestaan | Alleen `GET` en `POST` aanvragen worden ondersteund
429 te veel aanvragen | De API ondersteunt momenteel maximaal 5 query's per seconde
Fout 500-Service     | Probeer na enige tijd

### <a name="examples"></a>Voorbeelden

> [!NOTE] 
> Alle antwoorden op de API zijn JSON-tekenreeksen. Alle antwoorden op de volgende voorbeeld zijn voor de leesbaarheid pretty afgedrukt.

#### <a name="retrieving-network-information"></a>Bij het ophalen van informatie over het netwerk

**Aanvraag**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Antwoord**

> [!NOTE] 
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeld-antwoord is voor de leesbaarheid pretty afgedrukt.

```
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

#### <a name="retrieving-public-ip-address"></a>Openbaar IP-adres ophalen

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Ophalen van metagegevens voor een exemplaar van alle

**Aanvraag**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

**Antwoord**

> [!NOTE] 
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeld-antwoord is voor de leesbaarheid pretty afgedrukt.

```
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
    "vmSize": "Standard_D1"
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

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Ophalen van metagegevens in Windows virtuele Machine

**Aanvraag**

Metagegevens van het exemplaar kan worden opgehaald in Windows via het hulpprogramma PowerShell `curl`: 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

Of via de `Invoke-RestMethod` cmdlet:
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

**Antwoord**

> [!NOTE] 
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeld-antwoord is voor de leesbaarheid pretty afgedrukt.

```
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
De volgende gegevenscategorieën zijn beschikbaar via de Service-exemplaar voor metagegevens:

Gegevens | Beschrijving | Versie geïntroduceerd 
-----|-------------|-----------------------
location | Azure-regio de virtuele machine wordt uitgevoerd in de | 2017-04-02 
naam | Naam van de virtuele machine | 2017-04-02
aanbieding | Bieden informatie over de VM-afbeelding. Deze waarde is alleen aanwezig voor afbeeldingen van afbeelding voor Azure-galerie geïmplementeerd. | 2017-04-02
Uitgever | Uitgever van de VM-installatiekopie | 2017-04-02
SKU | Specifieke SKU voor de VM-installatiekopie | 2017-04-02
versie | Versie van de VM-afbeelding | 2017-04-02
besturingssysteemtype | Linux- of Windows | 2017-04-02
platformUpdateDomain |  [Updatedomein](manage-availability.md) in de virtuele machine wordt uitgevoerd | 2017-04-02
platformFaultDomain | [Foutdomein](manage-availability.md) in de virtuele machine wordt uitgevoerd | 2017-04-02
vmId | [De unieke id](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) voor de virtuele machine | 2017-04-02
vmSize | [VM-grootte](sizes.md) | 2017-04-02
subscriptionId | Azure-abonnement voor de virtuele Machine | 2017-08-01
tags | [Labels](../../azure-resource-manager/resource-group-using-tags.md) voor uw virtuele Machine  | 2017-08-01
resourceGroupName | [Resourcegroep](../../azure-resource-manager/resource-group-overview.md) voor uw virtuele Machine | 2017-08-01
placementGroupId | [Plaatsing groep](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) van de schaal van uw virtuele Machine instellen | 2017-08-01
IPv4/privateIpAddress | Lokale IPv4-adres van de virtuele machine | 2017-04-02
IPv4/publicIpAddress | Openbaar IPv4-adres van de virtuele machine | 2017-04-02
subnetadres / | Subnetadres van de virtuele machine | 2017-04-02 
subnetvoorvoegsel / | Subnetvoorvoegsel, voorbeeld 24 | 2017-04-02 
IPv6/IP-adres | Lokale IPv6-adres van de virtuele machine | 2017-04-02 
MAC-adres | Mac-adres van VM | 2017-04-02 
scheduledevents | Op dit moment in de openbare Preview Zie [scheduledevents](scheduled-events.md) | 2017-03-01

## <a name="example-scenarios-for-usage"></a>Voorbeeldscenario's voor gebruik  

### <a name="tracking-vm-running-on-azure"></a>Uitgevoerd op Azure VM bijhouden

Als een serviceprovider mogelijk nodig bij te houden van het aantal virtuele machines met uw software of agents die u wilt volgen Uniekheid van de virtuele machine. Als u een unieke ID voor een virtuele machine ophalen, gebruikt u de `vmId` veld van metagegevens-Service-exemplaar.

**Aanvraag**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

**Antwoord**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Plaatsing van containers domein veroorzaakt of bij te werken op basis van gegevens partities 

Voor bepaalde scenario's, de plaatsing van replica's van verschillende gegevens is van primair belang. Bijvoorbeeld: [HDFS replica plaatsing](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) of plaatsing van de container via een [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) mogelijk dat u wilt weten de `platformFaultDomain` en `platformUpdateDomain` op de virtuele machine wordt uitgevoerd.
U kunt deze gegevens rechtstreeks via de Service-exemplaar voor metagegevens opvragen.

**Aanvraag**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

**Antwoord**

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Meer informatie over de virtuele machine tijdens ondersteuningsaanvraag ophalen 

Als een serviceprovider krijgt u mogelijk een telefoongesprek ondersteuning waarin u wilt weten van meer informatie over de virtuele machine. Vragen van de klant voor het delen van de compute-metagegevens kunt basisinformatie voor de medewerker weten over de aard van de virtuele machine in Azure. 

**Aanvraag**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

**Antwoord**

> [!NOTE] 
> Het antwoord is een JSON-tekenreeks. Het volgende voorbeeld-antwoord is voor de leesbaarheid pretty afgedrukt.

```
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

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>Voorbeelden van het aanroepen van metagegevensservice met behulp van verschillende talen in de virtuele machine 

Taal | Voorbeeld 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.RB
Aan de slag  | https://github.com/Microsoft/azureimds/BLOB/master/imdssample.go            
Python   | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.PY
C++      | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample-Windows.cpp
C#       | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.cs
Javascript | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/BLOB/master/imdssample.Java
Visual Basic | https://github.com/Microsoft/azureimds/BLOB/master/IMDSSample.vb
    

## <a name="faq"></a>Veelgestelde vragen
1. Ik krijg de fout `400 Bad Request, Required metadata header not specified`. Wat betekent dit?
   * De Service-exemplaar voor metagegevens is vereist voor de header `Metadata: true` in de aanvraag moet worden doorgegeven. Deze header wordt doorgegeven in de REST-aanroep staat toegang tot de Service-exemplaar voor metagegevens. 
2. Waarom niet krijg ik compute-informatie voor mijn VM?
   * De Service-exemplaar voor metagegevens ondersteunt momenteel alleen exemplaren gemaakt met Azure Resource Manager. In de toekomst kunnen we ondersteuning voor virtuele machines van Cloud Service toevoegen.
3. Ik heb mijn virtuele Machine via Azure Resource Manager een tijd terug gemaakt. Waarom kan ik niet zien compute-metagegevens?
   * Voor alle virtuele machines na Sep 2016 is gemaakt, voegt u een [Tag](../../azure-resource-manager/resource-group-using-tags.md) om te beginnen te zien compute metagegevens. Voor oudere virtuele machines (gemaakt vóór Sep-2016), software-extensies of gegevens schijven aan de virtuele machine vernieuwen-metagegevens.
4. Ik zie niet alle gegevens voor de nieuwe versie van 2017-08-01 gevuld
   * Voor alle virtuele machines na Sep 2016 is gemaakt, voegt u een [Tag](../../azure-resource-manager/resource-group-using-tags.md) om te beginnen te zien compute metagegevens. Voor oudere virtuele machines (gemaakt vóór Sep-2016), software-extensies of gegevens schijven aan de virtuele machine vernieuwen-metagegevens.
5. Waarom krijg ik de fout `500 Internal Server Error`?
   * Probeer uw aanvraag op basis van de exponentiële back uit het systeem. Neem contact op met de ondersteuning van Azure als het probleem zich blijft voordoen.
6. Waar kan ik aanvullende vragen/opmerkingen delen
   * Uw opmerkingen over http://feedback.azure.com verzenden.
7. Dit werkt voor virtuele Machine Scale ingesteld exemplaar, zou?
   * Ja is metagegevens-service beschikbaar voor Scale-exemplaren instellen. 
8. Hoe krijg ik ondersteuning voor de service
   * Maak voor ondersteuning voor de service een ondersteuningsaanvraag in Azure-portal voor de virtuele machine waar u ze niet ophalen van metagegevens antwoord na lang pogingen 

   ![Ondersteuning voor Instance Metagegevens](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [gepland gebeurtenissen](scheduled-events.md) API **openbare preview** geleverd door de service-exemplaar metagegevens.
