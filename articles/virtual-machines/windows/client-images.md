---
title: "Windows client-installatiekopieën gebruiken in Azure | Microsoft Docs"
description: Het gebruik van de voordelen als Visual Studio-abonnement voor het implementeren van Windows 7, Windows 8 of Windows 10 in Azure voor ontwikkel-/ Testscenario 's
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 207a6562965b4913416bd4dbf3eb132b42938dc9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Windows-client in Azure gebruiken voor scenario's ontwikkelen en testen
U kunt Windows 7, Windows 8 of Windows 10 in Azure voor ontwikkel-/ Testscenario's geleverde dat een juiste Visual Studio (voorheen MSDN)-abonnement hebt. In dit artikel bevat een overzicht van de vereisten in aanmerking komt voor actieve Windows-client in Azure en het gebruik van de installatiekopieën van het Azure-galerie.

## <a name="subscription-eligibility"></a>Abonnement welke in aanmerking komen
Actieve Visual Studio-abonnees (personen die een licentie Visual Studio-abonnement hebt aangeschaft) kunnen u Windows-client gebruiken voor ontwikkeling en voor testdoeleinden. Windows-client kan worden gebruikt op uw eigen hardware- en Azure virtuele machines die worden uitgevoerd in elk type Azure-abonnement. Windows-client kan niet worden geïmplementeerd op of gebruikt in Azure voor gebruik in productieomgevingen normale of gebruikt door mensen die geen actieve Visual Studio-abonnees.

Voor uw gemak we hebben aangebracht bepaalde Windows 10-installatiekopieën beschikbaar vanuit de Azure-galerie binnen [in aanmerking komende ontwikkelen en testen biedt](#eligible-offers). Visual Studio-abonnees binnen elk type aanbieding kunnen ook [voldoende voorbereiden en maken](prepare-for-upload-vhd-image.md) een 64-bits installatiekopie voor Windows 7, Windows 8 of Windows 10 en vervolgens [uploaden naar Azure](upload-generalized-managed.md). Het gebruik blijft beperkt tot ontwikkelen en testen door actieve Visual Studio-abonnees.

## <a name="eligible-offers"></a>In aanmerking komende aanbiedingen
De volgende tabel ziet de id's die in aanmerking komen voor het implementeren van Windows 10 tot en met de galerie van Azure-aanbieding. Installatiekopieën van het Windows 10 zijn alleen zichtbaar voor de volgende aanbiedingen. Visual Studio-abonnees die aan het Windows-client wordt uitgevoerd in een andere aanbiedingtype moeten moeten u [voldoende voorbereiden en maken](prepare-for-upload-vhd-image.md) een 64-bits installatiekopie voor Windows 7, Windows 8 of Windows 10 en [vervolgens uploaden naar Azure](upload-generalized-managed.md).

| Naam van aanbieding: | Nummer van de aanbieding | Beschikbare client-installatiekopieën |
|:--- |:---:|:---:|
| [Betalen naar gebruik ontwikkelen en testen](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Abonnees van Visual Studio Enterprise (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium met MSDN (voordeel)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Abonnees van Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Abonnees van Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Enterprise ontwikkelen en testen](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Controleer uw Azure-abonnement
Als u uw aanbieding-ID niet weet, kunt u deze kunt verkrijgen via de Azure portal op een van deze twee manieren:  

- Op de blade 'Abonnementen':

  ![Details van de ID van de aanbieding van de Azure-portal](./media/client-images/offer-id-azure-portal.png) 

- Of klik op **facturering** en klik vervolgens op uw abonnement-ID. De ID van de aanbieding wordt weergegeven in de blade facturering.

U kunt ook weergeven met de ID van de aanbieding van de [tabblad 'Abonnementen'](http://account.windowsazure.com/Subscriptions) van de portal voor Azure-Account:

![Details van de ID van de aanbieding vanuit de Azure-accountportal](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Volgende stappen
U kunt nu uw virtuele machines met implementeren [PowerShell](quick-create-powershell.md), [Resource Manager-sjablonen](ps-template.md), of [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

