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
ms.openlocfilehash: b345283f87c446ff3b583b0c5dd8a4be222303ae
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
De volgende beperkingen gelden voor Azure Backup.

| Limiet-ID | Standaardlimiet |
| --- | --- |
| Aantal servers/machines dat kan worden geregistreerd voor elke kluis |50 voor Windows Client-Server/SCDPM <br/> 200 voor IaaS VM 's |
| Grootte van een gegevensbron voor gegevens die zijn opgeslagen in de kluis van Azure-opslag |54400 GB max<sup>1</sup> |
| Aantal back-upkluizen dat kan worden gemaakt in elk Azure-abonnement |25 recovery Services-kluizen per regio |
| Aantal keren dat back-up per dag kan worden gepland |3 per dag voor Windows Server /-Client <br/> 2 per dag voor SCDPM <br/> Eenmaal per dag voor IaaS VM 's |
| Gegevensschijven gekoppeld aan een virtuele machine van Azure voor back-up |16 |
| Grootte van afzonderlijke gegevensschijf gekoppeld aan een virtuele machine van Azure voor back-up| 4095 GB <sup>2</sup>|

* <sup>1</sup>de limiet van 54400 GB geldt niet voor IaaS VM back-up.
 

