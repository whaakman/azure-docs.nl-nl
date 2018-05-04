---
title: SSL in te schakelen op een cluster met Azure Machine Learning berekenen (MLC) | Microsoft Docs
description: Instructies voor het instellen van SSL voor score berekenen voor aanroepen in een Azure Machine Learning berekenen (MLC)-cluster
services: machine-learning
author: SerinaKaye
ms.author: serinak
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ms.openlocfilehash: 444803eeb77e79a8bfe3271ddf27bd428042c875
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="enable-ssl-on-an-azure-machine-learning-compute-mlc-cluster"></a>SSL inschakelen op een Azure Machine Learning berekenen (MLC)-cluster 

Deze instructies kunnen u voor het instellen van SSL voor score berekenen voor aanroepen op een Machine Learning berekenen (MLC)-cluster. 

## <a name="prerequisites"></a>Vereisten 

1. Bepaal op een CNAME (DNS-naam) te gebruiken bij het aanroepen van realtime score.

2. Maak een *wachtwoord gratis* certificaat met deze CNAME.

3. Als het certificaat nog niet in PEM-indeling, converteren naar PEM met hulpprogramma's zoals *openssl*.

U hebt twee bestanden na het voltooien van de vereisten:

* Een bestand voor het certificaat, bijvoorbeeld `cert.pem`. Zorg ervoor dat het bestand heeft de volledige certificaatketen.
* Een bestand voor de sleutel, bijvoorbeeld: `key.pem`



## <a name="set-up-an-ssl-certificate-on-a-new-acs-cluster"></a>Een SSL-certificaat op een nieuwe ACS-cluster instellen

Met de Azure Machine Learning CLI, voer de volgende opdracht met de `-c` overschakelen naar een ACS-cluster maken met een SSL-certificaat dat is gekoppeld:

```
az ml env create -c -g <resource group name> -n <cluster name> --cert-cname <CNAME> --cert-pem <path to cert.pem file> --key-pem <path to key.pem file>
```


## <a name="set-up-an-ssl-certificate-on-an-existing-acs-cluster"></a>Een SSL-certificaat op een bestaand ACS-cluster instellen

Als u voor een cluster dat is gemaakt zonder SSL ontwikkelt, kunt u een certificaat met behulp van Azure PowerShell-cmdlets kunt toevoegen: 

```
Set-AzureRmMlOpCluster -ResourceGroupName my-rg -Name my-cluster -SslStatus Enabled -SslCertificate $certValueInPemFormat -SslKey $keyValueInPemFormat -SslCName foo.mycompany.com
```

## <a name="map-the-cname-and-the-ip-address"></a>De CNAME- en het IP-adres toewijzen

Maak een toewijzing tussen de CNAME-u hebt geselecteerd in de vereisten en het IP-adres van de realtime front-end (FE). Voer de onderstaande opdracht voor het detecteren van de IP-adres van de FE. De uitvoer geeft een veld met de naam 'publicIpAddress' waarin het IP-adres van de realtime-front-cluster. Raadpleeg de documentatie van uw DNS-provider voor het instellen van een record van de FQDN-naam in CNAME gebruikt voor het openbare IP-adres.



## <a name="auto-detection-of-a-certificate"></a>Automatische detectie van een certificaat 

Wanneer u maakt een realtime web service met de '`az ml service create realtime`' opdracht Azure Machine Learning automatisch detecteert de SSL instellen op het cluster en de URL van de score met het opgegeven certificaat worden automatisch ingesteld. 

Voer de volgende opdracht overzicht van de certificaatstatus. Let op het voorvoegsel 'https' van de score berekenen voor URI en de CNAME van de hostnaam. 

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
