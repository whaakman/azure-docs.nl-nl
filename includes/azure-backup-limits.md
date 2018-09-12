---
title: bestand opnemen
description: bestand opnemen
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 9/10/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 64101ea5a3bbaac4a6b2e349a04d06ea84a87081
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381088"
---
De volgende limieten gelden voor Azure Backup.

| Limiet-ID | Standaardlimiet |
| --- | --- |
| Aantal servers/machines dat kan worden geregistreerd voor elke kluis |50 voor Windows Server/Client-/ SCDPM <br/> 1000 voor IaaS-VM 's |
| Grootte van een gegevensbron voor gegevens die zijn opgeslagen in de opslag van Azure sleutelkluis |Maximum aantal 54400 GB<sup>1</sup> |
| Aantal back-upkluizen dat kan worden gemaakt in elk Azure-abonnement |500 recovery Services-kluizen per regio |
| Aantal keren dat het back-up per dag kan worden gepland |3 per dag voor Windows Server/Client <br/> 2 per dag voor SCDPM <br/> Eenmaal per dag voor IaaS-VM 's |
| Gegevensschijven die zijn gekoppeld aan een virtuele machine van Azure voor back-up |16 |
| Grootte van afzonderlijke schijf die is gekoppeld aan een virtuele machine van Azure voor back-up| 4095 GB|

* <sup>1</sup>de 54400 GB-limiet geldt niet voor IaaS VM backup.
 

