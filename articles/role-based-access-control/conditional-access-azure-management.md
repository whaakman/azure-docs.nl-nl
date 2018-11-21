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
ms.date: 11/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 4cfd3c38631778373e99872fd305d8316cdadb75
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52274735"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Toegang tot Azure-beheer met voorwaardelijke toegang beheren

Voorwaardelijke toegang in Azure Active Directory (Azure AD) beheert de toegang voor cloud-apps op basis van specifieke voorwaarden die u opgeeft. Voor toegang, u het beleid voor voorwaardelijke toegang die toegang op basis van of de vereisten in het beleid wordt voldaan toestaan of blokkeren. 

Meestal gebruikt u voorwaardelijke toegang om toegang te beheren voor uw cloud-apps. U kunt ook beleidsregels instellen voor het beheren van toegang tot Azure-beheer op basis van bepaalde voorwaarden (zoals aanmeldingsrisico, locatie of een apparaat) en om af te dwingen vereisten zoals multi-factor authentication.

Voor het maken van een beleid voor het beheer van Azure die u selecteert **Microsoft Azure Management** onder **Cloud-apps** bij het kiezen van de app in waarvoor het beleid wordt toegepast.

![Voorwaardelijke toegang voor Azure-beheer](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Het beleid dat u maakt geldt voor alle Azure-beheereindpunten, met inbegrip van de klassieke Azure-portal, Azure-portal, Azure Resource Manager-provider, klassieke Service Management API's en Azure PowerShell. Houd er rekening mee dat het beleid wordt toegepast op Azure PowerShell, waarmee de Azure Resource Manager-API aanroept. Dit geldt niet voor [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2), die Microsoft Graph aanroepen.

> [!CAUTION]
> Zorg ervoor dat u begrijpt hoe voorwaardelijke toegang werkt voordat u een beleid instelt voor het beheren van toegang tot Azure-beheer. Zorg ervoor dat u geen voorwaarden die kunnen geblokkeerd door uw eigen toegang tot de portal maakt.

Zie voor meer informatie over het instellen en gebruiken van voorwaardelijke toegang [voorwaardelijke toegang in Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).