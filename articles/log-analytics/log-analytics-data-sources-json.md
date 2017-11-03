---
title: Verzamelen van aangepaste JSON-gegevens in OMS Log Analytics | Microsoft Docs
description: Aangepaste JSON-gegevensbronnen kunnen worden verzameld in de OMS-Agent voor Linux met logboekanalyse.  Deze aangepaste gegevensbronnen mag eenvoudige scripts JSON zoals curl of een van de FluentD 300 + plugins retourneren. In dit artikel beschrijft de vereiste configuratie voor deze gegevensverzameling.
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 800ee1269556e7c2d56fbbf2b497c10509b5c78c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="collecting-custom-json-data-sources-with-the-oms-agent-for-linux-in-log-analytics"></a>Verzamelen van aangepaste JSON-gegevensbronnen met de OMS-Agent voor Linux in Log Analytics
Aangepaste JSON-gegevensbronnen kunnen worden verzameld in de OMS-Agent voor Linux met logboekanalyse.  Deze aangepaste gegevensbronnen kunnen worden eenvoudige scripts JSON zoals retourneren [curl](https://curl.haxx.se/) of een van [FluentD van 300 + plugins](http://www.fluentd.org/plugins/all). In dit artikel beschrijft de vereiste configuratie voor deze gegevensverzameling.

> [!NOTE]
> OMS-Agent voor Linux v1.1.0-217 + is vereist voor de aangepaste JSON-gegevens

## <a name="configuration"></a>Configuratie

### <a name="configure-input-plugin"></a>Invoer-invoegtoepassing configureren

Voor het verzamelen van JSON-gegevens in logboekanalyse toevoegen `oms.api.` toe aan het begin van een tag FluentD in een invoegtoepassing voor invoer.

Volgende is bijvoorbeeld een afzonderlijk configuratiebestand `exec-json.conf` in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Dit maakt gebruik van de invoegtoepassing FluentD `exec` elke 30 seconden een curl-opdracht uitvoeren.  De uitvoer van deze opdracht worden verzameld door de JSON-uitvoer-invoegtoepassing.

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
Het configuratiebestand wordt toegevoegd onder `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` is vereist om het eigendom ervan gewijzigd met de volgende opdracht.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Output-invoegtoepassing configureren 
De configuratie van de volgende uitvoer-invoegtoepassing toevoegen aan de configuratie van de belangrijkste in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` of als een afzonderlijk configuratiebestand in geplaatst`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-oms-agent-for-linux"></a>Opnieuw opstarten OMS-Agent voor Linux
Start de OMS-Agent voor Linux-service met de volgende opdracht opnieuw.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Uitvoer
De gegevens worden verzameld in logboekanalyse met een recordtype van `<FLUENTD_TAG>_CL`.

Bijvoorbeeld, het aangepaste label `tag oms.api.tomcat` in logboekanalyse met een recordtype van `tomcat_CL`.  U kunt alle records van dit type met de volgende logboek search kan ophalen.

    Type=tomcat_CL

Geneste JSON-gegevens bronnen worden ondersteund, maar zijn geïndexeerd gebaseerd op de bovenliggende veld. Bijvoorbeeld de volgende JSON-gegevens geretourneerd van een zoekopdracht logboekanalyse als `tag_s : "[{ "a":"1", "b":"2" }]`.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren. 
 