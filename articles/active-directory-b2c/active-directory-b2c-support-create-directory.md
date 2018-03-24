---
title: 'Azure Active Directory B2C: Oplossen maken tenants | Microsoft Docs'
description: Problemen en oplossingen voor het maken van een Azure Active Directory of Azure Active Directory B2C-tenant.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: 3daf232d7fb1f95c390c1e6b8c168ec585484c65
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Problemen met een Azure Active Directory of Azure Active Directory B2C-tenant maken 

## <a name="create-an-azure-ad-tenant"></a>Een Azure AD-tenant maken
Als u een tenant van Azure Active Directory (Azure AD) op de eerste poging maken kunt, probeer het opnieuw. Als het probleem zich blijft voordoen, neem dan contact op met ondersteuning van Azure.

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken
Als u problemen ondervindt wanneer u [maken van een Azure Active Directory B2C-tenant (Azure AD B2C)](active-directory-b2c-get-started.md), probeert u de volgende opties:

* Als de Azure AD B2C-tenant niet in de lijst met tenants weergegeven, probeer het opnieuw maken van de tenant.
* Als de Azure AD B2C-tenant wordt weergegeven in de lijst met tenants en u het volgende foutbericht weergegeven ziet, de tenant verwijderen en opnieuw maken:

    "Kan het maken van de B2C-tenant 'contosob2c' niet voltooien. Ga naar dit [koppeling](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) voor meer informatie. "
* Er zijn bekende problemen wanneer u een bestaande Azure AD B2C-tenant verwijderen en opnieuw maken met behulp van dezelfde domeinnaam. Wanneer u een nieuwe Azure AD B2C-tenant maakt, moet u een andere domeinnaam.
* Als deze oplossingen niet werken, moet u contact op met ondersteuning van Azure. Zie voor meer informatie [bestand ondersteuningsaanvragen voor Azure AD B2C](active-directory-b2c-support.md).

