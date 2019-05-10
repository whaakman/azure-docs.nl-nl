---
title: Gebruik een galerie met gedeelde installatiekopieën in Azure Lab-Services | Microsoft Docs
description: Informatie over het configureren van een lab-account voor het gebruik van een galerie met installatiekopieën van de gedeelde zodat een gebruiker een installatiekopie met andere delen kunt en een andere gebruiker met de installatiekopie kunt maken van een VM-sjabloon in het lab.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 8d8b6fffe197d4180b091518dcd1615d0e0b9d19
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412858"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Gebruik een galerie met gedeelde installatiekopieën in Azure Lab Services
Dit artikel wordt beschreven hoe docenten/lab-beheerder een sjabloon voor VM-installatiekopie voor het opnieuw worden gebruikt door anderen kan opslaan. Deze installatiekopieën worden opgeslagen in een Azure [gedeelde afbeeldingengalerie](../../virtual-machines/windows/shared-image-galleries.md). Als een eerste stap koppelt u de lab-beheerder een bestaande gedeelde afbeeldingengalerie aan het lab-account. Zodra de galerie met installatiekopieën van de gedeelde is gekoppeld, kunnen labs gemaakt in het lab-account afbeeldingen opslaan in de galerie met installatiekopieën van de gedeelde. Andere docenten kunnen deze installatiekopie in de galerie gedeelde installatiekopie te maken van een sjabloon voor de klassen selecteren. 

## <a name="prerequisites"></a>Vereisten
- Een gedeelde afbeeldingengalerie maken met behulp van [Azure PowerShell](../../virtual-machines/windows/shared-images.md) of [Azure CLI](../../virtual-machines/linux/shared-images.md).
- U hebt de galerie met installatiekopieën van de gedeelde gekoppeld aan het lab-account. Zie voor stapsgewijze instructies [koppelen of ontkoppelen gedeelde afbeeldingengalerie](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Een afbeelding opgeslagen in de galerie met installatiekopieën van de gedeelde
Nadat een galerie met installatiekopieën van de gedeelde is gekoppeld, kan een docent opslaan of upload een afbeelding naar de galerie met installatiekopieën van de gedeelde zodat deze kan worden gebruikt door andere docenten. Zie voor instructies voor het uploaden van een installatiekopie naar de galerie met installatiekopieën van de gedeelde [overzicht van de galerie met installatiekopieën van gedeelde](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> Momenteel, biedt geen de Leslokaallabs-gebruikersinterface (UI) ondersteuning voor opslaan van de installatiekopie van een lab in de galerie met installatiekopieën van de gedeelde. 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Een installatiekopie uit de galerie met gedeelde installatiekopie gebruiken
Een docent kan een aangepaste installatiekopie beschikbaar in de galerie met installatiekopieën van de gedeelde voor de sjabloon kiezen tijdens het maken van nieuwe lab.

![Installatiekopie voor virtuele machines uit de galerie gebruiken](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over gedeelde afbeeldingsgalerieën [gedeelde afbeeldingengalerie](../../virtual-machines/windows/shared-image-galleries.md).
