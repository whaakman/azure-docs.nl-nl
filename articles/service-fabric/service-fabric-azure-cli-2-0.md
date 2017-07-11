---
title: Aan de slag met Service Fabric en Azure CLI 2.0
description: In dit artikel leest u hoe u de opdrachtenmodule van Service Fabric gebruikt in de Azure CLI, versie 2.0, inclusief het maken van verbinding met het cluster en het beheren van toepassingen.
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: c5cc6e54acf27456185eeb48858c4d981aa46b4b
ms.contentlocale: nl-nl
ms.lasthandoff: 07/01/2017

---
<a id="service-fabric-and-azure-cli-20" class="xliff"></a>

# Service Fabric en Azure CLI 2.0

De nieuwe Azure CLI 2.0 bevat nu opdrachten voor het beheren van Service Fabric-clusters. Deze documentatie bevat stappen om aan de slag te gaan met de Azure CLI.

<a id="install-azure-cli-20" class="xliff"></a>

## Azure CLI 2.0 installeren

De Azure CLI 2.0 bevat nu opdrachten voor het beheren en manipuleren van Service Fabric-clusters. Volg het [standaardinstallatieproces](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) om de nieuwste Azure CLI te downloaden.

Raadpleeg voor meer informatie de [Azure CLI 2.0-documentatie](https://docs.microsoft.com/en-us/cli/azure/overview).

<a id="cli-syntax" class="xliff"></a>

## De syntaxis van de CLI

Alle Azure Service Fabric-opdrachten worden voorafgegaan door `az sf` in de Azure CLI. Als u meer wilt weten over de beschikbare opdrachten, kunt u `az sf -h` uitvoeren voor algemene informatie. Gebruik `az sf <command> -h` voor gedetailleerde informatie over een bepaalde opdracht.

Azure Service Fabric-opdrachten in de CLI hebben een specifiek naamgevingspatroon:

```azurecli
az sf <object> <action>
```

Hier is `<object>` het doel voor `<action>`.

<a id="selecting-a-cluster" class="xliff"></a>

## Een cluster selecteren

U kunt pas bewerkingen uitvoeren nadat u een cluster hebt geselecteerd waarmee u verbinding wilt maken. Met het volgende codefragment kunt u bijvoorbeeld verbinding maken met een niet-beveiligd cluster.

> [!WARNING]
> Gebruik geen niet-beveiligde Service Fabric-clusters voor productieomgevingen.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Het clustereindpunt moet worden voorafgegaan door `http` of `https`, en moet de poort voor de HTTP-gateway bevatten. Deze poort en het adres komen overeen met de Service Fabric Explorer-URL.

Voor clusters die met een certificaat zijn beveiligd, worden niet-gecodeerde `pem`-bestanden ondersteund, of `crt`- en `key`-bestanden.

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Zie voor meer informatie [dit gedetailleerde document over het maken van verbinding met veilige clusters](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> De select-opdracht verwerkt geen aanvragen voordat de controle wordt teruggegeven. Om te controleren of een cluster juist is opgegeven, voert u een opdracht zoals `az sf cluster health` uit, en controleert u of de opdracht geen fouten retourneert.

<a id="performing-basic-operations" class="xliff"></a>

## Basisbewerkingen uitvoeren

De gegevens van een clusterverbinding blijven behouden tussen verschillende Azure CLI-sessies. Nadat een Service Fabric-cluster is geselecteerd, kunt u elke Service Fabric-opdracht uitvoeren.

Als u bijvoorbeeld de status van het Service Fabric-cluster wilt weten, voert u de volgende opdracht uit:

```azurecli
az sf cluster health
```

De opdracht levert de volgende uitvoer op, ervan uitgaande dat JSON-uitvoer is opgegeven in de configuratie van de Azure CLI:

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

<a id="tips-and-faq" class="xliff"></a>

## Tips en veelgestelde vragen

Hier volgt informatie die nuttig zijn kan als er problemen zijn met het uitvoeren van Service Fabric-opdrachten in de Azure CLI.

<a id="converting-a-certificate-from-pfx-to-pem" class="xliff"></a>

### Een certificaat converteren van PFX naar PEM

De Azure CLI ondersteunt clientcertificaten als PEM-bestanden (extensie `.pem`). Als u PFX-bestanden van Windows gebruikt, moeten deze certificaten worden geconverteerd naar de PEM-indeling. Gebruik de volgende opdracht om een PFX-bestand te converteren naar een PEM-bestand:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Raadpleeg de [OpenSSL-documentatie](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) voor meer informatie.

<a id="connection-issues" class="xliff"></a>

### Verbindingsproblemen

Bij het uitvoeren van bewerkingen kan de volgende fout optreden:

> `Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Controleer in dit geval nog een keer of het opgegeven clustereindpunt bereikbaar is en luistert. Controleer ook of de gebruikersinterface van Service Fabric Explorer bereikbaar is op die host en poort. Gebruik `az sf cluster select` om het eindpunt bij te werken.

<a id="getting-detailed-logs" class="xliff"></a>

### Gedetailleerde logboeken opvragen

Wanneer u fouten gaat opsporen of een probleem wilt melden, is het handig om over gedetailleerde logboeken te beschikken. De Azure CLI bevat een algemene `--debug`-vlag waarmee het detailniveau van logboeken wordt verhoogd.

<a id="command-help-and-syntax" class="xliff"></a>

### Syntaxis van Help-opdracht

De Service Fabric-opdrachten volgen dezelfde conventie als de Azure CLI. Geef de vlag `-h` op om informatie weer te geven voor een bepaalde opdracht of een groep opdrachten. Bijvoorbeeld:

```azurecli
az sf application -h
```

of

```azurecli
az sf application create -h
```

