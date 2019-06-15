---
title: Toegang tot Azure-beheer met voorwaardelijke toegang in Azure Active Directory beheren
description: Meer informatie over het gebruik van voorwaardelijke toegang in Azure AD om toegang tot Azure-beheer te beheren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2019
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 4ee58835a5055f5382cefa3a049424ffe97a01c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122947"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Toegang tot Azure-beheer met voorwaardelijke toegang beheren

Voorwaardelijke toegang in Azure Active Directory (Azure AD) beheert de toegang voor cloud-apps op basis van specifieke voorwaarden die u opgeeft. Voor toegang, u het beleid voor voorwaardelijke toegang die toegang op basis van of de vereisten in het beleid wordt voldaan toestaan of blokkeren. 

Normaal gesproken gebruikt u voorwaardelijke toegang voor toegang tot uw cloud-apps beheren. U kunt ook beleidsregels instellen voor het beheren van toegang tot Azure-beheer op basis van bepaalde voorwaarden (zoals aanmeldingsrisico, locatie of een apparaat) en om af te dwingen vereisten zoals multi-factor authentication.

Voor het maken van een beleid voor het beheer van Azure die u selecteert **Microsoft Azure Management** onder **Cloud-apps** bij het kiezen van de app in waarvoor het beleid wordt toegepast.

![Voorwaardelijke toegang voor Azure-beheer](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Het beleid dat u maakt, is van toepassing op alle Azure-beheereindpunten, met inbegrip van Azure portal, Azure Resource Manager-provider, klassieke Service Management API's, Azure PowerShell en Visual Studio-abonnementen beheerdersportal. Houd er rekening mee dat het beleid wordt toegepast op Azure PowerShell, waarmee de Azure Resource Manager-API aanroept. Dit geldt niet voor [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2), die Microsoft Graph aanroepen.

> [!CAUTION]
> Zorg ervoor dat u begrijpt hoe voorwaardelijke toegang werkt voordat u een beleid instelt voor het beheren van toegang tot Azure-beheer. Zorg ervoor dat u geen voorwaarden die kunnen geblokkeerd door uw eigen toegang tot de portal maakt.

Zie voor meer informatie over het instellen en gebruiken van voorwaardelijke toegang [voorwaardelijke toegang in Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).