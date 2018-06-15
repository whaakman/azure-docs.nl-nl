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
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31613633"
---
De volgende beperkingen gelden voor Azure Backup.

| Limiet-ID | Standaardlimiet |
| --- | --- |
| Aantal servers/machines dat kan worden geregistreerd voor elke kluis |50 voor Windows Client-Server/SCDPM <br/> 1000 voor IaaS VM 's |
| Grootte van een gegevensbron voor gegevens die zijn opgeslagen in de kluis van Azure-opslag |54400 GB max<sup>1</sup> |
| Aantal back-upkluizen dat kan worden gemaakt in elk Azure-abonnement |500 recovery Services-kluizen per regio |
| Aantal keren dat back-up per dag kan worden gepland |3 per dag voor Windows Server /-Client <br/> 2 per dag voor SCDPM <br/> Eenmaal per dag voor IaaS VM 's |
| Gegevensschijven gekoppeld aan een virtuele machine van Azure voor back-up |16 |
| Grootte van afzonderlijke gegevensschijf gekoppeld aan een virtuele machine van Azure voor back-up| 4095 GB <sup>2</sup>|

* <sup>1</sup>de limiet van 54400 GB geldt niet voor IaaS VM back-up.
 

