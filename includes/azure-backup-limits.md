---
title: bestand opnemen
description: bestand opnemen
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 2/7/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 1f41567f7d5dcc9b6536b47ef639ba53abff8ec4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755580"
---
De volgende limieten gelden voor Azure Backup.

| Limiet-ID | Standaardlimiet |
| --- | --- |
| Aantal servers/machines dat kan worden geregistreerd voor elke kluis |50 voor Windows Server/Client-/ SCDPM <br/> 1000 voor IaaS-VM 's |
| Grootte van een gegevensbron voor gegevens die zijn opgeslagen in de opslag van Azure sleutelkluis |Maximum aantal 54400 GB<sup>1</sup> |
| Aantal back-upkluizen dat kan worden gemaakt in elk Azure-abonnement |500 recovery Services-kluizen per regio |
| Aantal keren dat het back-up per dag kan worden gepland |3 per dag voor Windows Server/Client <br/> 2 per dag voor SCDPM <br/> Eenmaal per dag voor IaaS-VM 's |
| Gegevensschijven die zijn gekoppeld aan een virtuele machine van Azure voor back-up |16 |
| Grootte van afzonderlijke schijf die is gekoppeld aan een virtuele machine van Azure voor back-up| 4095 GB <sup>2</sup>|

* <sup>1</sup>de 54400 GB-limiet geldt niet voor IaaS VM backup.
 

