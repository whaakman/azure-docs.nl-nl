---
title: SSL inschakelen op een Azure Machine Learning Compute (MLC)-cluster | Microsoft Docs
description: Instructies voor het instellen van SSL voor het aanroepen van beoordelingen op een Azure Machine Learning Compute (MLC)-cluster
services: machine-learning
author: SerinaKaye
ms.author: serinak
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 2a7733468ec082c8954f623f3ebe2cea1fbad561
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976218"
---
# <a name="enable-ssl-on-an-azure-machine-learning-compute-mlc-cluster"></a>SSL inschakelen op een Azure Machine Learning Compute (MLC)-cluster 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Deze instructies kunnen u het instellen van SSL voor het aanroepen van beoordelingen op een Machine Learning Compute (MLC)-cluster. 

## <a name="prerequisites"></a>Vereisten 

1. Beslissen over een CNAME (DNS-naam) te gebruiken bij het aanroepen van realtime scoring.

2. Maak een *wachtwoord gratis* certificaat door die CNAME.

3. Als het certificaat nog niet in de PEM-indeling, deze converteren naar PEM met behulp van hulpprogramma's zoals *openssl*.

Na het voltooien van de vereisten hebt u twee bestanden:

* Een bestand voor het certificaat, bijvoorbeeld `cert.pem`. Zorg ervoor dat het bestand heeft de volledige certificaatketen.
* Een bestand voor de sleutel, bijvoorbeeld: `key.pem`



## <a name="set-up-an-ssl-certificate-on-a-new-acs-cluster"></a>Een SSL-certificaat op een nieuwe ACS-cluster instellen

Met de Azure Machine Learning CLI, voer de volgende opdracht met de `-c` overschakelen naar een ACS-cluster maken met een SSL-certificaat dat is gekoppeld:

```
az ml env create -c -g <resource group name> -n <cluster name> --cert-cname <CNAME> --cert-pem <path to cert.pem file> --key-pem <path to key.pem file>
```


## <a name="set-up-an-ssl-certificate-on-an-existing-acs-cluster"></a>Een SSL-certificaat op een bestaande ACS-cluster instellen

Als u een cluster dat is gemaakt zonder SSL gebruiken, kunt u een certificaat met behulp van Azure PowerShell-cmdlets kunt toevoegen.

U moet de sleutel en het certificaat in een onbewerkte PEM-indeling opgeven. Deze kunnen worden gelezen in PowerShell-variabelen:

```
$keyValueInPemFormat = [IO.File]::ReadAllText('<path to key.pem file>')
$certValueInPemFormat = [IO.File]::ReadAllText('<path to cert.pem file>')
```
Het certificaat toevoegen aan het cluster: 

```
Set-AzureRmMlOpCluster -ResourceGroupName my-rg -Name my-cluster -SslStatus Enabled -SslCertificate $certValueInPemFormat -SslKey $keyValueInPemFormat -SslCName foo.mycompany.com
```

## <a name="map-the-cname-and-the-ip-address"></a>De CNAME- en het IP-adres toewijzen

Maak een toewijzing tussen de CNAME die u hebt geselecteerd in de vereisten en het IP-adres van de realtime front-end (FE). Voor het detecteren van het IP-adres van de FE, moet u de onderstaande opdracht uitvoeren. De uitvoer wordt weergegeven een veld met de naam 'openbare IP-adres', dat het IP-adres van de front-end voor realtime-cluster bevat. Raadpleeg de instructies van uw DNS-provider voor het instellen van een record van de FQDN-naam die wordt gebruikt in CNAME naar het openbare IP-adres.



## <a name="auto-detection-of-a-certificate"></a>Automatische detectie van een certificaat 

Wanneer u maakt een realtime web service met de '`az ml service create realtime`' opdracht, Azure Machine Learning de SSL instellen op het cluster automatisch gedetecteerd en de scoring-URL met het opgegeven certificaat worden automatisch ingesteld. 

Als u wilt zien van de status van het certificaat, moet u de volgende opdracht uitvoeren. U ziet het voorvoegsel 'https' van de scores URI en de CNAME-naam van de host. 

``` 
az ml service show realtime -n <service name>
{
                "id" : "<your service id>",
                "name" : "<your service name >",
                "state" : "Succeeded",
                "createdAt" : "<datetime>”,
                "updatedAt" : "< datetime>",
                "scoringUri" : "https://<your CNAME>/api/v1/service/<service name>/score"
}
```
