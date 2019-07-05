---
title: Azure VMware-oplossing door CloudSimple Private Cloud verkleinen
description: Beschrijft hoe u voor het verkleinen van een CloudSimple Private Cloud.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6e639feb603f1654b4dcd40f16d8e3094307839a
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544515"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>Een Privécloud CloudSimple verkleinen

CloudSimple biedt de flexibiliteit om te verkleinen dynamisch een Private Cloud.  Een Privécloud bestaat uit een of meer vSphere-clusters. Elk cluster kan 3 tot en met 16 knooppunten hebben. Tijdens het verkleinen van een Private Cloud, kunt u een knooppunt verwijderen uit het bestaande cluster of een geheel cluster verwijderen. 

## <a name="before-you-begin"></a>Voordat u begint

Volgende voorwaarden wordt voldaan voor het verkleinen van een Privécloud.  Beheer (eerste cluster)-cluster gemaakt wanneer een Privécloud is gemaakt kan niet worden verwijderd.

* Een vSphere-cluster moet drie knooppunten hebben.  Een cluster met slechts drie knooppunten niet verkleinen.
* Totale hoeveelheid opslagruimte mag niet groter zijn dan de totale capaciteit na verkleinen van het cluster. 

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="shrink-a-private-cloud"></a>Verkleinen van een Private Cloud 

1. [Toegang tot de portal CloudSimple](access-cloudsimple-portal.md).

2. Open de **Resources** pagina.

3. Klik op de Privécloud die u wilt verkleinen

4. Klik op de pagina overzicht **verkleinen**.

    ![Privécloud verkleinen](media/shrink-private-cloud.png)

5. Selecteer het cluster die u wilt verkleinen of verwijderen. 

    ![Privécloud - Selecteer cluster verkleinen](media/shrink-private-cloud-select-cluster.png)

6. Selecteer **verwijderen van één knooppunt** of **verwijderen van het hele cluster**. 

7. Controleer of de capaciteit van het cluster

8. Klik op **indienen** voor het verkleinen van de Privécloud.

Verkleinen van de Privécloud wordt gestart.  U kunt de voortgang van taken.  De verkleinen kan enkele uren duren, afhankelijk van de gegevens die moet worden resynced op virtueel SAN.

## <a name="next-steps"></a>Volgende stappen

* [Virtuele VMware-machines op Azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* Meer informatie over [Privéclouds](cloudsimple-private-cloud.md)