---
title: Technische vereisten voor het maken van een VM-installatiekopie voor de Azure Marketplace | Microsoft Docs
description: Informatie over de vereisten voor het maken en implementeren van een VM-installatiekopie naar de Azure Marketplace voor anderen om aan te schaffen.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ROBOTS: NOINDEX
ms.openlocfilehash: 1fdf945645b2d5c89d7f3750ec836595adb90b06
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077076"
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technische vereisten voor het maken van een VM-installatiekopie voor de Azure Marketplace
Het proces aandachtig door voordat u begint gelezen en u begrijpt waar en waarom elke stap wordt uitgevoerd. Zo veel mogelijk, u moet voorbereiden van gegevens van uw bedrijf en andere gegevens, die nodig zijn hulpprogramma's downloaden en/of technische componenten maken voordat u begint met het proces voor het maken van aanbieding. Deze items moet wissen uit het controleren van dit artikel.  

## <a name="download-needed-tools--applications"></a>Download de benodigde hulpprogramma's en toepassingen
U hebt de volgende items gereed is voordat u begint met het proces:

* Afhankelijk van welk besturingssysteem u ontwikkelt, installeert u de [Azure PowerShell-cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) of [Linux opdrachtregelinterface hulpprogramma](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) uit de [Azure Downloads](https://azure.microsoft.com/downloads/) pagina.
* Installeer Azure Storage Explorer vanaf CodePlex.
* Download en installeer het hulpprogramma van de Test-certificering voor Azure Certified:
  * [http://go.microsoft.com/fwlink/?LinkID=526913](https://go.microsoft.com/fwlink/?LinkID=526913). U moet een Windows-computer om uit te voeren van het certificeringshulpprogramma. Als u een Windows-computer beschikbaar hebt, kunt u het hulpprogramma met behulp van een Windows-VM in Azure uitvoeren.

## <a name="platforms-supported"></a>Platforms die worden ondersteund
U kunt ontwikkelen met Azure-VM's in Windows of Linux. Bepaalde elementen van het publicatieproces, zoals het maken van een Azure-compatibele virtuele harde schijf (VHD)--Gebruik verschillende hulpprogramma's en stappen, afhankelijk van welk besturingssysteem u gebruikt:  

* Als u van Linux gebruikmaakt, raadpleegt u de sectie 'Een Azure-compatibele VHD maken (Linux-indeling)' van de [virtuele machine-installatiekopie publicatiehandleiding voor](marketplace-publishing-vm-image-creation.md).
* Als u Windows gebruikt, raadpleegt u de sectie 'Een Azure-compatibele VHD maken (Windows-indeling)' van de [virtuele machine-installatiekopie publicatiehandleiding voor](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> U hebt toegang tot een Windows-machine naar:
> 
> * Het certificeringshulpprogramma validatie worden uitgevoerd.
> * Maak de VHD shared access signature-URL voor het verzenden van de VHD-certificering.
> 
> 

## <a name="develop-your-vhd"></a>Om uw VHD ontwikkelen
U kunt Azure-VHD's in de cloud of on-premises ontwikkelen:

* Ontwikkeling van cloud-gebaseerde betekent dat alle stappen van de ontwikkeling van op afstand worden uitgevoerd op een VHD residente op Azure.
* On-premises ontwikkeling is vereist voor het downloaden van een VHD en het ontwikkelen met behulp van on-premises infrastructuur. Hoewel deze benadering mogelijk is, niet aangeraden deze. Ontwikkelen voor Windows of SQL on-premises, moet u de relevante on-premises licentiesleutels hebben. U kan opnemen of installatie van SQL Server na het maken van een virtuele machine. Ook moet u uw aanbieding baseren op een goedgekeurde SQL-installatiekopie vanuit Azure portal. Als u besluit op locatie ontwikkelen, moet u enkele stappen anders dan als u in de cloud ontwikkelt uitvoeren. U kunt relevante informatie vinden in [maken van een on-premises VM-installatiekopie](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
