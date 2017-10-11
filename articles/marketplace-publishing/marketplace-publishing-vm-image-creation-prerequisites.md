---
title: Technische vereisten voor het maken van de installatiekopie van een virtuele machine voor Azure Marketplace | Microsoft Docs
description: Begrip van de vereisten voor het maken en implementeren van de installatiekopie van een virtuele machine naar de Azure Marketplace voor anderen om aan te schaffen.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: af3e2ad623d8d7bfafe676411f9ae3fbee78aab8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technische vereisten voor het maken van de installatiekopie van een virtuele machine voor Azure Marketplace
Het proces zorgvuldig door voordat u begint lees en begrijp waar en waarom elke stap wordt uitgevoerd. Zo veel mogelijk, u moet voorbereiden van uw bedrijfsgegevens en andere gegevens, vereiste hulpprogramma's downloaden en/of technische onderdelen maken voordat u begint met een proces voor het maken van de aanbieding. Deze items moeten worden van de hand van dit artikel.  

## <a name="download-needed-tools--applications"></a>Download de benodigde hulpprogramma's en toepassingen
U hebt de volgende items gereed is voordat u begint met het proces:

* Afhankelijk van het besturingssysteem die u ontwikkelt, installeert u de [Azure PowerShell-cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) of [Linux opdrachtregelinterface hulpprogramma](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) van de [Azure downloadt](https://azure.microsoft.com/downloads/) pagina.
* Azure Opslagverkenner van CodePlex installeren.
* Download en installeer het hulpprogramma certificeringsinstantie Test voor Azure gecertificeerd:
  * [http://go.Microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913). U moet een Windows-computer het certificeringsinstantie-hulpprogramma uitvoert. Als u een computer op basis van Windows niet hebt, kunt u het hulpprogramma voor het gebruik van een VM op basis van Windows in Azure uitvoeren.

## <a name="platforms-supported"></a>Ondersteunde platforms
U kunt ontwikkelen op basis van een Azure-machines op Windows of Linux. Bepaalde elementen van het publicatieproces--zoals het maken van een Azure-compatibele virtuele harde schijf (VHD)--Gebruik verschillende hulpprogramma's en stappen, afhankelijk van welk besturingssysteem u gebruikt:  

* Als u van Linux gebruikmaakt, raadpleegt u de sectie 'Een Azure-compatibele-VHD maken (op basis van Linux)' van de [virtuele machine installatiekopie publishing handleiding](marketplace-publishing-vm-image-creation.md).
* Als u van Windows gebruikmaakt, raadpleegt u de sectie 'Een Azure-compatibele-VHD maken (op Windows gebaseerd)' van de [virtuele machine installatiekopie publishing handleiding](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> U moet toegang tot een Windows-machine op:
> 
> * Voer de validatie certificeringsinstantie.
> * Maken van de VHD gedeeld access signature-URL voor de verzending van het VHD-certificering.
> 
> 

## <a name="develop-your-vhd"></a>Uw VHD ontwikkelen
U kunt Azure VHD's in de cloud of on-premises ontwikkelen:

* Cloud-gebaseerde ontwikkeling betekent dat alle stappen van de ontwikkeling op afstand worden uitgevoerd op een VHD residente op Azure.
* Lokale ontwikkeling vereist downloaden van een VHD en ontwikkelen met behulp van on-premises infrastructuur. Hoewel dit mogelijk is raadzaam niet deze. Houd er rekening mee dat ontwikkelen voor Windows of SQL lokale, moet u de relevante lokale licentiesleutels hebben. U kan opnemen of installatie van SQL Server na het maken van een virtuele machine. Ook moet u uw aanbieding baseren op een goedgekeurde SQL-installatiekopie van de Azure-portal. Als u lokale ontwikkelen besluit, moet u anders dan als u in de cloud ontwikkelt een aantal stappen uitvoeren. U kunt relevante informatie vinden in [maken van een installatiekopie van een lokale virtuele machine](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
