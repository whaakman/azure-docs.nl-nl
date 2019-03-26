---
title: Azure Data Box Gateway General Availability opmerkingen bij de release | Microsoft Docs
description: Beschrijving van essentiële openstaande problemen en oplossingen voor de Azure Data Box Gateway met versie met algemene beschikbaarheid.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 3f3e07d745d83041afc9dafd64678a3ac4a65012
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418021"
---
# <a name="azure-data-box-edgeazure-data-box-gateway-general-availability-release-notes"></a>Azure Data Box Edge/Azure Data Box Gateway General Availability opmerkingen bij de release

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen de kritieke open problemen identificeren en de opgeloste problemen voor algemene beschikbaarheid (GA) de release van Azure Data Box Edge en Azure Data Box-Gateway.

Opmerkingen bij de release worden voortdurend bijgewerkt en als kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw gegevens in het Edge/Data Box-Gateway implementeert, zorgvuldig door de informatie in de release-opmerkingen.

De GA-versie komt overeen met de versies van software:

- **Data Box Gateway 1903 (1.5.810.441)**
- **Data Box Edge 1903 (1.5.810.441)**


## <a name="whats-new"></a>Nieuwe functies

- **Nieuwe installatiekopieën van virtuele schijven** -nieuwe VHDX en VMDK zijn nu beschikbaar in de Azure-portal. Download deze installatiekopieën als u wilt inrichten, configureren en implementeren van nieuwe apparaten van de gegevensgateway in het algemeen beschikbaar. De Data Box-Gateway-apparaten die zijn gemaakt in het eerdere voorbeeld releases kunnen niet worden bijgewerkt naar deze versie. Ga voor meer informatie naar [voorbereidingen voor het implementeren van Azure Data Box Gateway](data-box-gateway-deploy-prep.md).
- **Ondersteuning voor NFS** -ondersteuning voor NFS is momenteel in preview en beschikbaar voor v3.0 en v4.1 clients die toegang hebben tot de gegevens in het Edge- en Data Box-Gateway-apparaten.
- **Opslag tolerantie** -uw gegevens in het Edge-apparaat bestand zijn tegen het uitvallen van één schijf met de functie voor opslag tolerantie. Deze functie is momenteel beschikbaar als preview-product. U kunt opslag tolerantie inschakelen door te selecteren de **Resilient** optie in de **Opslaginstellingen** in de lokale webgebruikersinterface.


## <a name="known-issues-in-ga-release"></a>Bekende problemen in GA-versie

De volgende tabel bevat een overzicht van de bekende problemen voor de Data Box-Gateway met versie uitgevoerd.

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Bestandstypen | De volgende bestandstypen worden niet ondersteund: teken bestanden, bestanden blokkeren, sockets, pipes, symbolische koppelingen.  |Deze bestanden zijn gekopieerd resultaten in 0 heeft een lengte van bestanden die worden gemaakt op de NFS delen. Deze bestanden blijven in een foutstatus en er wordt ook gerapporteerd *error.xml*. <br> Symbolische koppelingen naar de directory's leiden tot mappen ophalen nooit offline gemarkeerd. U mag de grijze cross als gevolg hiervan niet weergegeven in de mappen die aangeeft dat de mappen zijn offline en alle gekoppelde inhoud is volledig geüpload naar Azure. |
| **2.** |Verwijderen | Vanwege een fout in deze release, als een NFS-share is verwijderd, kan klikt u vervolgens de share niet worden verwijderd. De status van de share wordt weergegeven *verwijderen*.  |Dit gebeurt alleen als de share met behulp van de naam van een niet-ondersteund bestandstype. |
| **3.** |Exemplaar | Het kopiëren van gegevens is mislukt met fout:  De aangevraagde bewerking kan niet worden voltooid vanwege een beperking van bestand system.  |De alternatieve gegevens Stream (AD) die zijn gekoppeld aan groter zijn dan 128 KB wordt niet ondersteund.   |


## <a name="next-steps"></a>Volgende stappen

- [Voorbereidingen voor het implementeren van Azure Data Box Gateway](data-box-gateway-deploy-prep.md).
- [Voorbereidingen voor het implementeren van Azure Data Box Edge](data-box-edge-deploy-prep.md).
