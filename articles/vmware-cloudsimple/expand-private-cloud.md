---
title: Azure VMware-oplossing door CloudSimple Private Cloud uitbreiden
description: Beschrijft hoe u een bestaande CloudSimple Private Cloud capaciteit toe te voegen in een bestaande of nieuwe-cluster uitbreiden
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/06/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 293a09c57ca95e2774e44ff4bc9f9f2c31be2f49
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332991"
---
# <a name="expand-a-cloudsimple-private-cloud"></a>Een Privécloud CloudSimple uitvouwen

CloudSimple biedt de flexibiliteit om uit te breiden dynamisch een Private Cloud. U kunt beginnen met een kleinere configuratie had en vouw vervolgens als u meer capaciteit nodig hebt. Of u kunt maken van een Privécloud, op basis van huidige behoeften en vouw vervolgens als verbruik groeit.

Een Privécloud bestaat uit een of meer vSphere-clusters. Elk cluster kan 3 tot en met 16 knooppunten hebben.  Bij het uitbreiden van een Private Cloud, kunt u knooppunten toevoegen aan het bestaande cluster of een nieuw cluster maken. Als u wilt een bestaand cluster wilt uitbreiden, moet extra knooppunten hetzelfde type (SKU) als de bestaande knooppunten. Voor het maken van een nieuw cluster, kunnen de knooppunten van een ander type zijn. Voor meer informatie over de limieten van de Privécloud, Zie sectie in beperkt [CloudSimple privécloud overzicht](cloudsimple-private-cloud.md) artikel.

Een privécloud is gemaakt met een standaard **Datacenter** op vCenter.  Elk datacenter fungeert als een entiteit op het hoogste niveau management.  Voor een nieuw cluster biedt CloudSimple de keuze toe te voegen aan de bestaande datacenter of een nieuw datacenter te maken.

Als onderdeel van de configuratie van het nieuwe cluster, CloudSimple Hiermee configureert u de VMware-infrastructuur.  De instellingen omvatten Opslaginstellingen voor het vSAN schijfgroepen en hoge beschikbaarheid van VMware Distributed resources Scheduler (DRS).

Een Privécloud kan meerdere keren worden uitgebreid. Uitbreiding kan worden gedaan, alleen wanneer u binnen de grenzen van de algehele knooppunt blijven. Telkens wanneer u een Privécloud die u toevoegt aan het bestaande cluster of maak een nieuwe uitbreiden.

## <a name="before-you-begin"></a>Voordat u begint

Knooppunten moeten worden ingericht voordat u uw Private Cloud kunt uitbreiden.  Zie voor meer informatie over het inrichten van knooppunten [knooppunten in te richten voor VMware-oplossing door CloudSimple - Azure](create-nodes.md) artikel.  Voor het maken van een nieuw cluster, moet u ten minste drie beschikbare knooppunten van de dezelfde SKU hebben.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="expand-a-private-cloud"></a>Vouw een Private Cloud

1. [Toegang tot de portal CloudSimple](access-cloudsimple-portal.md).

2. Open de **Resources** pagina en selecteer de Privécloud waarvan u wilt uitbreiden.

3. Klik in de sectie Samenvatting, **uit te breiden**.

    ![Vouw privécloud](media/resources-expand-private-cloud.png)

4. Kies of u wilt het bestaande cluster uitbreiden of maak een nieuw vSphere-cluster. Als u wijzigingen aanbrengt, wordt de samenvattende informatie op de pagina bijgewerkt.

    * Als u wilt het bestaande cluster wilt uitbreiden, klikt u op **Vouw bestaand cluster**. Selecteer het cluster dat u wilt uitvouwen en voer het aantal knooppunten om toe te voegen. Elk cluster kan maximaal 16 knooppunten hebben.
    * Als u wilt een nieuw cluster hebt toegevoegd, klikt u op **nieuw cluster maken**. Voer een naam in voor het cluster. Selecteer een bestaand datacenter of voer een naam voor het maken van een nieuw datacenter. Kies het knooppunttype. U kunt een ander knooppunttype kiezen bij het maken van een nieuw vSphere-cluster, maar niet bij het uitbreiden van een bestaand vSphere-cluster. Selecteer het aantal knooppunten. Elk nieuw cluster moet ten minste drie knooppunten hebben.

    ![Vouw de privécloud - knooppunten toevoegen](media/resources-expand-private-cloud-add-nodes.png)

5. Klik op **indienen** om uit te breiden de privécloud.

## <a name="next-steps"></a>Volgende stappen

* [Virtuele VMware-machines op Azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* Meer informatie over [Privéclouds](cloudsimple-private-cloud.md)