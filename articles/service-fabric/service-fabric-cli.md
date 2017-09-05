---
title: Aan de slag met Azure Service Fabric-CLI (sfctl)
description: Lees hier alles over het gebruik van de CLI van Azure Service Fabric. Informatie over verbinding maken met een cluster en het beheren van toepassingen.
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 2faca2887f25b45d833dea7b2259277466290670
ms.contentlocale: nl-nl
ms.lasthandoff: 08/29/2017

---
# <a name="azure-service-fabric-command-line"></a>Azure Service Fabric-opdrachtregel

De CLI van Azure Service Fabric (sfctl) is een opdrachtregelprogramma voor interactie met en het beheer van Azure Service Fabric-entiteiten. Sfctl kan worden gebruikt met Windows- of Linux-clusters. Sfctl kan worden uitgevoerd op elk platform dat ondersteuning biedt voor python.

## <a name="prerequisites"></a>Vereisten

Voorafgaand aan installatie controleert u of zowel python als pip is geïnstalleerd in uw omgeving. Raadpleeg voor meer informatie de [Quick Start-documentatie voor pip](https://pip.pypa.io/en/latest/quickstart/) en de officiële [installatiedocumentatie voor python](https://wiki.python.org/moin/BeginnersGuide/Download).

Hoewel python 2.7 en 3.6 allebei worden ondersteund, is het raadzaam om python 3.6 te gebruiken. De volgende sectie gaat over het installeren van alle vereiste onderdelen en de CLI.

## <a name="install-pip-python-and-sfctl"></a>Pip, python en sfctl installeren

Hoewel er veel manieren zijn om zowel pip als python te installeren op uw platform, zijn dit enkele stappen om snel aan de slag te gaan met python 3.6 en pip voor belangrijke besturingssystemen:

### <a name="windows"></a>Windows

Voor Windows 10, Server 2016 en Server 2012R2 kunt u de officiële standaardinstructies voor de installatie gebruiken. Tijdens de installatie van python wordt pip ook standaard geïnstalleerd.

- Navigeer naar de officiële [python-pagina met downloads](https://www.python.org/downloads/) en download de nieuwste release van python 3.6
- Start het installatieprogramma
- Selecteer de optie `Add Python 3.6 to PATH` onderaan de prompt
- Selecteer `Install Now`
- Voltooi de installatie

U moet nu een nieuw opdrachtvenster kunnen openen en de versie van python en pip kunnen opvragen:

```bat
python --version
pip --version
```

Voer vervolgens deze opdracht uit om de Service Fabric-CLI te installeren:

```
pip install sfctl
sfctl -h
```

### <a name="ubuntu"></a>Ubuntu

Voor Ubuntu 16.04 Desktop kunt u python 3.6 installeren met behulp van een PPA van een andere leverancier:

Voer in een terminalvenster de volgende opdrachten uit:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Voer de volgende opdracht uit als u sfctl alleen wilt installeren voor de installatie van python 3.6:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

Deze stappen hebben geen invloed op de door het systeem geïnstalleerde versies python 3.5 en 2.7. Laat deze installaties ongewijzigd, tenzij u bekend bent met Ubuntu.

### <a name="macos"></a>MacOS

Voor Mac OS is het raadzaam om [het pakketbeheerprogramma HomeBrew](https://brew.sh) te gebruiken. Installeer HomeBrew als dat nog niet is gebeurd door de volgende opdracht uit te voeren:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Installeer vervolgens vanuit de terminal python 3.6, pip en sfctl

```bash
brew install python3
pip3 install sfctl
sfctl -h
```

Met deze stappen wordt de installatie door het systeem van python 2.7 niet gewijzigd.

## <a name="cli-syntax"></a>De syntaxis van de CLI

Opdrachten worden altijd voorafgegaan door `sfctl`. Voor algemene informatie over alle opdrachten die u kunt gebruiken, gebruikt u `sfctl -h`. Gebruik `sfctl <command> -h` voor hulp bij één opdracht.

Opdrachten volgen een herhaalbare structuur, met het doel van de opdracht vóór de bewerking of actie:

```azurecli
sfctl <object> <action>
```

In dit voorbeeld is `<object>` het doel voor `<action>`.

## <a name="select-a-cluster"></a>Een cluster selecteren

U kunt pas bewerkingen uitvoeren nadat u een cluster hebt geselecteerd waarmee u verbinding wilt maken. Voer bijvoorbeeld de volgende opdracht uit om het cluster met de naam `testcluster.com` te selecteren en er verbinding mee te maken.

> [!WARNING]
> Gebruik geen niet-beveiligde Service Fabric-clusters in een productieomgeving.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Het clustereindpunt moet worden voorafgegaan door `http` of `https`. Het moet de poort voor de HTTP-gateway bevatten. De poort en het adres komen overeen met de Service Fabric Explorer-URL.

Voor clusters die zijn beveiligd met een certificaat, kunt u een met PEM gecodeerd certificaat opgeven. Het certificaat kan worden opgegeven als een enkel bestand of als een certificaat-sleutelpaar.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Zie [Verbinding maken met een beveiligd Azure Service Fabric-cluster](service-fabric-connect-to-secure-cluster.md) voor meer informatie.

## <a name="basic-operations"></a>Basisbewerkingen

De gegevens van een clusterverbinding blijven behouden tussen meerdere sfctl-sessies. Nadat u een Service Fabric-cluster hebt geselecteerd, kunt u elke Service Fabric-opdracht in het cluster uitvoeren.

Als u bijvoorbeeld de status van het Service Fabric-cluster wilt weten, voert u de volgende opdracht uit:

```azurecli
sfctl cluster health
```

De opdracht resulteert in de volgende uitvoer:

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a>Tips en probleemoplossing

Enkele suggesties en tips voor het oplossen van veelvoorkomende problemen.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Een certificaat converteren van PFX- naar PEM-indeling

De CLI van Service Fabric ondersteunt clientcertificaten als PEM-bestanden (extensie .pem). Als u PFX-bestanden van Windows gebruikt, moet u deze certificaten converteren naar PEM-indeling. Gebruik de volgende opdracht om een PFX-bestand te converteren naar een PEM-bestand:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Voor meer informatie raadpleegt u de [OpenSSL-documentatie](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Verbindingsproblemen

Bepaalde bewerkingen genereren mogelijk het volgende bericht:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Controleer of het opgegeven clustereindpunt beschikbaar is en luistert. Controleer ook of de gebruikersinterface van Service Fabric Explorer beschikbaar is op die host en poort. Gebruik `sfctl cluster select` om het eindpunt bij te werken.

### <a name="detailed-logs"></a>Gedetailleerde logboeken

Gedetailleerde logboeken zijn vaak nuttig zijn wanneer u fouten opspoort of een probleem meldt. Via een algemene `--debug`-vlag kan het detailniveau van logboeken worden verhoogd.

### <a name="command-help-and-syntax"></a>Syntaxis van Help-opdracht

Als u hulp nodig hebt met een specifieke opdracht of een groep opdrachten, gebruikt u de `-h`-vlag:

```azurecli
sfctl application -h
```

Nog een voorbeeld:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Volgende stappen

* [Een toepassing implementeren met de Azure Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md)
* [Aan de slag met Service Fabric in Linux](service-fabric-get-started-linux.md)

