---
title: Resourcelimieten voor Azure NetApp Files | Microsoft Docs
description: Beperkingen voor Azure NetApp Files bronnen, met inbegrip van limieten voor NetApp accounts, pools capaciteit, volumes, momentopnamen en het gedelegeerde subnet worden beschreven.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: b-juche
ms.openlocfilehash: b55467d77beb8f97b8e392b72682268ae0407e54
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65826381"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Resourcelimieten voor Azure NetApp Files

Inzicht krijgen in resourcelimieten voor Azure NetApp-bestanden, kunt u de volumes beheren.

## <a name="resource-limits"></a>Bronlimieten

De volgende tabel beschrijft de resourcelimieten voor Azure NetApp bestanden:

|  Resource  |  Standaardlimiet  |  Aanpasbare via een ondersteuningsaanvraag  |
|----------------|---------------------|--------------------------------------|
|  Aantal accounts NetApp per Azure-abonnement   |  10    |  Ja   |
|  Aantal capaciteit pools per NetApp-account   |    25     |   Ja   |
|  Aantal volumes per capaciteit van toepassingen     |    500   |    Ja     |
|  Het aantal momentopnamen per volume       |    255     |    Nee        |
|  Aantal subnetten gedelegeerd naar Azure NetApp bestanden (Microsoft.NetApp/volumes) per Virtueelnetwerk van Azure    |   1   |    Nee    |
|  Maximum aantal virtuele machines (met inbegrip van gekoppelde VNets) die verbinding kunnen maken met een volume     |    1000   |    Nee   |
|  Minimale grootte van een groep met één capaciteit   |  4 TiB     |    Nee  |
|  Maximale grootte van een groep met één capaciteit    |  500 TiB   |   Nee   |
|  Minimale grootte van een enkel volume    |    100 GiB    |    Nee    |
|  Maximum aantal toegewezen quotum van een enkel volume *   |   92 TiB   |    Nee   |
|  Maximale grootte van een enkel volume *     |    100 TiB    |    Nee       |

\* Een volume kan handmatig worden gemaakt of gewijzigd in bewaartemperatuur 92 TiB. Een volume kan echter maximaal 100 TiB in een overschrijding scenario groeien. Zie [Kostenmodel voor Azure NetApp Files](azure-netapp-files-cost-model.md) voor meer informatie over capaciteit overschrijding. 

## <a name="request-limit-increase"></a>Limiet verhoging aanvragen 

U kunt een Azure-ondersteuningsaanvraag te verhogen, de aanpasbare limieten van de bovenstaande tabel maken. 

In het vlak van de navigatie in de Azure portal: 

1. Klik op **Help en ondersteuning**.
2. Klik op **+ nieuw ondersteuningsverzoek**.
3. Geef op het tabblad grondbeginselen van de volgende informatie: 
    1. Type probleem: Selecteer **limieten voor Service en -abonnement (quota)** .
    2. Abonnementen: Selecteer het abonnement voor de resource die u het quotamaximum te verhogen moet.
    3. Quotumtype: Selecteer **opslag: Azure Files met NetApp beperkt**.
    4. Klik op **Next: Oplossingen**.
4. Op het tabblad Details:
    1. Geef de volgende informatie voor het bijbehorende brontype in het vak Beschrijving:

        |  Resource  |    Bovenliggende resources      |    Nieuwe grenzen aangevraagd     |    Reden voor verhoging       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Account |  *Subscription ID*   |  *Aangevraagde nieuwe maximum **account** getal*    |  *Welke scenario of gebruik het geval u hierom wordt gevraagd de aanvraag?*  |
        |  Pool    |  *Abonnements-ID, Account URI*  |  *Aangevraagde nieuwe maximum **pool** getal*   |  *Welke scenario of gebruik het geval u hierom wordt gevraagd de aanvraag?*  |
        |  Volume  |  *Abonnements-ID, Account URI, Pool URI*   |  *Aangevraagde nieuwe maximum **volume** getal*     |  *Welke scenario of gebruik het geval u hierom wordt gevraagd de aanvraag?*  |

    2. Geef de juiste methode ondersteund en geef uw contractinformatie.

    3. Klik op **Next: Beoordelen en maken** om de aanvraag te maken. 


## <a name="next-steps"></a>Volgende stappen  

- [Informatie over de opslaghiërarchie van Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Model met kosten van Azure NetApp bestanden](azure-netapp-files-cost-model.md)
