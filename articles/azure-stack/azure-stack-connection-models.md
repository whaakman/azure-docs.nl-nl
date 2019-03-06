---
title: Geïntegreerde Azure Stack-systemen verbinding modellen | Microsoft Docs
description: Bepaal implementatie planningsbeslissingen voor meerdere knooppunten Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: wfayed
ms.lastreviewed: 02/21/2019
ms.openlocfilehash: f58d4e4f8b0e095fe64489fb42f78ef82c5af359
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57407813"
---
# <a name="azure-stack-integrated-systems-connection-models"></a>Geïntegreerde Azure Stack-systemen verbinding modellen
Als u geïnteresseerd bent in een geïntegreerde Azure Stack-systeem aanschaffen, moet u om te begrijpen [overwegingen met betrekking tot verschillende datacenter-integratie](azure-stack-datacenter-integration.md) voor Azure Stack-implementatie om te bepalen hoe het systeem past in uw datacenter. Bovendien moet u bepalen hoe u Azure Stack wordt integreren in uw hybride cloudomgeving. Dit artikel bevat een overzicht van deze belangrijke beslissingen, waaronder Azure-verbinding, opslag, identiteit, en het facturering modelbeslissingen.

Als u besluit om aan te schaffen een geïntegreerd systeem, wordt uw hardwareleverancier oorspronkelijke leveranciers (OEM) helpt u te helpen bij veel van het planningsproces in meer detail. Ze worden ook de werkelijke implementatie uitvoeren.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Kies een Azure Stack-implementatiemodel verbinding
U kunt implementeren Azure Stack is verbonden met internet (en naar Azure) of de verbinding verbroken. Voor het ophalen van het meeste voordeel uit Azure Stack, hybride scenario's tussen Azure Stack en Azure, zoals wilt u implementeren met Azure bent verbonden. Deze keuze bepaalt welke opties zijn beschikbaar voor uw identiteitsarchief (Azure Active Directory of Active Directory Federation Services) en de facturering van maandabonnementen (betalen als u op basis van gebruik facturering of op basis van capaciteit facturering) zoals in het volgende diagram en de volgende tabel wordt samengevat: 

![Azure Stack-implementatie en scenario's voor facturering](media/azure-stack-connection-models/azure-stack-scenarios.png)  
  
> [!IMPORTANT]
> Dit is een belangrijke beslissing. Kiezen voor Active Directory Federation Services (AD FS) of Azure Active Directory (Azure AD) is een eenmalige beslissing die u tijdens de implementatie moet aanbrengen. U kunt geen dit later wijzigen zonder het hele systeem opnieuw te implementeren.  


|Opties|Met Azure bent verbonden|Niet verbonden met Azure|
|-----|:-----:|:-----:|
|Azure AD|![Ondersteund](media/azure-stack-connection-models/check.png)| |
|AD FS|![Ondersteund](media/azure-stack-connection-models/check.png)|![Ondersteund](media/azure-stack-connection-models/check.png)|
|Facturering op basis van gebruik|![Ondersteund](media/azure-stack-connection-models/check.png)| |
|Facturering op basis van capaciteit|![Ondersteund](media/azure-stack-connection-models/check.png)|![Ondersteund](media/azure-stack-connection-models/check.png)|
|Licentieverlening| Enterprise Agreement of een Cloud Solution Provider | Enterprise Agreement |
|Voor patches en updates|Updatepakket kan rechtstreeks vanuit het Internet worden gedownload met Azure Stack |  Vereist<br><br>Vereist ook verwisselbare media<br> en een afzonderlijke verbonden apparaat |
| Registratie | Geautomatiseerd | Vereist<br><br>Vereist ook verwisselbare media<br> en een afzonderlijke verbonden apparaat |

Nadat u hebt besloten op de verbinding met het Azure-model moet worden gebruikt voor Azure Stack-implementatie, moeten aanvullende, afhankelijk van het verbinding beslissingen voor het identiteitsarchief en factureringsmethode worden gemaakt. 

## <a name="next-steps"></a>Volgende stappen

[Azure verbonden beslissingen voor de implementatie van Azure Stack](azure-stack-connected-deployment.md)

[Azure beslissingen voor niet-verbonden Azure Stack-implementatie](azure-stack-disconnected-deployment.md)
