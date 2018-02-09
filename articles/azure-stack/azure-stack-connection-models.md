---
title: "Azure Stack geïntegreerd systemen verbinding modellen | Microsoft Docs"
description: Implementatie planningsbeslissingen voor meerdere knooppunten Azure Stack bepalen.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: e6c94ef1172ea6380a94d5907c24069ed8c48ff5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-stack-integrated-systems-connection-models"></a>Azure Stack geïntegreerd systemen verbinding modellen
Als u geïnteresseerd in een Azure-Stack geïntegreerd systeem bent, moet u begrijpen [verschillende datacenter Integratieoverwegingen](azure-stack-datacenter-integration.md) voor de implementatie van de Azure-Stack om te bepalen hoe u het systeem wordt inpassen in uw datacenter. Bovendien moet u beslissen precies hoe u wordt integreren met Azure-Stack uw hybride cloud-omgeving. Dit artikel bevat een overzicht van deze belangrijke beslissingen, waaronder Azure verbinding, identity-store en facturering model beslissingen te nemen.

Als u besluit om aan te schaffen een geïntegreerd systeem, helpt uw oorspronkelijke leveranciers (OEM) hardwareleverancier begeleiden u bij veel van de planning in meer detail. Ze worden ook de werkelijke implementatie uitvoeren.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Kies een Azure-Stack-implementatiemodel verbinding
U kunt voor het implementeren van Azure-Stack is verbonden met internet (en naar Azure) of de verbinding verbroken. Als u optimaal wilt profiteren van Azure-Stack, waaronder hybride scenario's tussen Azure-Stack en Azure, zou u wenst te implementeren met Azure bent verbonden. Deze keuze bepaalt welke opties beschikbaar zijn voor uw archief (Azure Active Directory of Active Directory Federation Services) en facturering model (betalen als u op basis van gebruik facturerings- of op basis van capaciteit facturering) zoals in het volgende diagram en de volgende tabel samengevat: 

![Azure Stack-implementatie en facturering scenario 's](media/azure-stack-connection-models/azure-stack-scenarios.png)  
  
> [!IMPORTANT]
> Dit is een belangrijke beslissingspunt! Het kiezen van Active Directory Federation Services (AD FS) of Azure Active Directory (Azure AD) is een eenmalige beslissing die u tijdens de implementatie aanbrengen moet. U kunt geen dit later wijzigen zonder het hele systeem opnieuw te implementeren.  


|Opties|Verbonden met Azure|Azure is verbroken|
|-----|-----|-----|
|Azure AD|![Ondersteund](media/azure-stack-connection-models/check.png)| |
|AD FS|![Ondersteund](media/azure-stack-connection-models/check.png)|![Ondersteund](media/azure-stack-connection-models/check.png)|
|Facturering op basis van verbruik|![Ondersteund](media/azure-stack-connection-models/check.png)| |
|Facturering op basis van capaciteit|![Ondersteund](media/azure-stack-connection-models/check.png)|![Ondersteund](media/azure-stack-connection-models/check.png)|
|Updatepakketten downloaden rechtstreeks naar de Azure-Stack|![Ondersteund](media/azure-stack-connection-models/check.png)|  |

Nadat u hebt besloten in het model van Azure verbinding moet worden gebruikt voor de implementatie van Azure-Stack, moeten aanvullende, verbinding-afhankelijke beslissingen voor de identiteit store en facturering methode worden aangebracht. 

## <a name="next-steps"></a>Volgende stappen

[Azure verbonden beslissingen voor de implementatie van Azure-Stack](azure-stack-connected-deployment.md)

[Azure beslissingen voor niet-verbonden Azure Stack-implementatie](azure-stack-disconnected-deployment.md)
