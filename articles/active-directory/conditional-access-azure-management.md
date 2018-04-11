---
title: Toegang tot Azure management met voorwaardelijke toegang in Azure Active Directory beheren
description: Meer informatie over het gebruik van voorwaardelijke toegang in Azure AD voor het beheren van toegang tot beheer van Azure.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: bryanla
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: skwan
ms.openlocfilehash: 22d0e53c201853e2c316089479ffbd4d9e5d92be
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Toegang tot beheer van Azure met voorwaardelijke toegang beheren

Voorwaardelijke toegang in Azure Active Directory (Azure AD) beheert de toegang voor cloud-apps op basis van bepaalde voorwaarden die u opgeeft. Om toegang te verlenen, maakt u beleid voor voorwaardelijke toegang toestaan of blokkeren van toegang op basis van of de vereisten in het beleid wordt voldaan. 

Doorgaans kunt u voorwaardelijke toegang om toegang te controleren uw cloud-apps. U kunt ook beleidsregels instellen om toegang aan Azure-beheer op basis van bepaalde voorwaarden (zoals risico aanmelden, locatie of apparaat) te beheren en om af te dwingen vereisten zoals multi-factor authentication-server.

Voor het maken van een beleid voor het beheer van Azure die u selecteert **Microsoft Azure Management** onder **Cloud-apps** bij het kiezen van de app kan het beleid wordt toegepast.

![Voorwaardelijke toegang voor Azure-beheer](./media/conditional-access-azure-mgmt.png)

Het beleid dat u maakt is van toepassing op alle Azure management eindpunten, met inbegrip van de klassieke Azure-portal, Azure-portal, Azure Resource Manager-provider, klassieke Service Management-API's en Azure PowerShell.

> [!CAUTION]
> Zorg dat u begrijpt hoe voorwaardelijke toegang werkt voordat u een beleid voor het beheren van toegang tot Azure management instelt. Zorg ervoor dat u de voorwaarden die uw eigen toegang tot de portal kunnen blokkeren niet maken.

Zie voor meer informatie over het instellen en gebruiken van voorwaardelijke toegang [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md).