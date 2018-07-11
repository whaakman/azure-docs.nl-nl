---
title: Windows-clientinstallatiekopieën gebruiken in Azure | Microsoft Docs
description: Het gebruik van de voordelen als Visual Studio-abonnement voor het implementeren van Windows 7, Windows 8 of Windows 10 in Azure voor ontwikkelings-/ Testscenario 's
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: a2b9a06e8e3b1bda91050e1607c7265d6fe66bb1
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931786"
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Windows-client in Azure gebruiken voor scenario's voor ontwikkelen en testen
Kunt u Windows 7, Windows 8 of Windows 10 Enterprise (x64) in Azure voor dev/test-scenario's die dat u hebt een geschikt abonnement op Visual Studio (voorheen MSDN). In dit artikel bevat een overzicht van de vereisten voor geschiktheid voor het uitvoeren van Windows 7, Windows 8.1, Windows 10 Enterprise in Azure en het gebruik van de volgende installatiekopieën van Azure-galerie.

![Details van de installatiekopie van Azure portal](./media/client-images/windows-client-msdn-images.png) 

> [!NOTE]
> Raadpleeg voor Windows 10 Pro en Windows 10 Pro N installatiekopie in Azure-galerie, [over het implementeren van Windows 10 op Azure met Multitenant Hosting-rechten](windows-desktop-multitenant-hosting-deployment.md)
>![Pro Image-details van de Azure-portal](./media/client-images/windows-client-pro-images.png) 
>

## <a name="subscription-eligibility"></a>Abonnement in aanmerking komt
Actieve Visual Studio-abonnees (mensen die een licentie voor Visual Studio-abonnement hebben aangeschaft) kunnen Windows-client gebruiken voor ontwikkelings- en testdoeleinden. Windows-client kan worden gebruikt op uw eigen hardware- en virtuele Azure-machines die worden uitgevoerd in elk type Azure-abonnement. Windows-client kan niet worden geïmplementeerd op die op Azure gebruikt voor normale productiegebruik of wordt gebruikt door mensen die geen actieve Visual Studio-abonnees.

Voor uw gemak bepaalde Windows 10-installatiekopieën beschikbaar zijn vanuit de Azure-galerie binnen [in aanmerking komende dev/test-aanbiedingen](#eligible-offers). Visual Studio-abonnees binnen elk type aanbieding kunnen ook [voldoende voorbereiden en maak](prepare-for-upload-vhd-image.md) een 64-bits Windows 7, Windows 8 of Windows 10-installatiekopie en vervolgens [uploaden naar Azure](upload-generalized-managed.md). Het gebruik blijft beperkt tot ontwikkelen en testen door actieve Visual Studio-abonnees.

## <a name="eligible-offers"></a>In aanmerking komende aanbiedingen
De volgende tabel worden de aanbieding-id's die in aanmerking voor het implementeren van Windows 10 via Azure Gallery. De Windows 10-installatiekopieën zijn alleen zichtbaar voor de volgende aanbiedingen. Visual Studio-abonnees die Windows-client wordt uitgevoerd in een andere aanbieding wilt, moet u [voldoende voorbereiden en maak](prepare-for-upload-vhd-image.md) een 64-bits Windows 7, Windows 8 of Windows 10-installatiekopie en [vervolgens uploaden naar Azure](upload-generalized-managed.md).

| Naam van aanbieding: | Nummer van de aanbieding | Beschikbare client-installatiekopieën |
|:--- |:---:|:---:|
| [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise (MPN)-abonnees](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium met MSDN (voordeel)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise-abonnees](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Abonnees van Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Enterprise Dev/Test](https://azure.microsoft.com/en-us/offers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Controleer uw Azure-abonnement
Als u uw aanbiedings-ID niet weet, kunt u deze kunt verkrijgen via de Azure-portal in een van de volgende twee manieren:  

- Op de *abonnementen* venster:

  ![Details van de ID van de aanbieding van Azure portal](./media/client-images/offer-id-azure-portal.png) 

- Of klik op **facturering** en klik vervolgens op uw abonnements-ID. De aanbieding ID wordt weergegeven in de *facturering* venster.

U kunt ook weergeven met de aanbiedings-ID van de [tabblad 'Abonnementen'](http://account.windowsazure.com/Subscriptions) van de portal voor Azure-Account:

![Details van aanbieding-ID van de portal voor Azure-Account](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Volgende stappen
U kunt nu uw VM's implementeren met [PowerShell](quick-create-powershell.md), [Resource Manager-sjablonen](ps-template.md), of [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

