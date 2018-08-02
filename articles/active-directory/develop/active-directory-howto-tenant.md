---
title: Een Azure AD-tenant verkrijgen | Microsoft Docs
description: Een Azure Active Directory-tenant verkrijgen voor het registreren en maken van toepassingen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 1f866d3ee56b0c9a1e7a986d3ac951764b6a1cae
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399511"
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Een Azure Active Directory-tenant verkrijgen

In Azure Active Directory (Azure AD) is een [tenant](https://msdn.microsoft.com/library/azure/jj573650.aspx#Anchor_0) representatief voor een organisatie. Het is een toegewezen exemplaar van de Azure AD-service die een organisatie ontvangt en waarvan de organisatie de eigenaar is wanneer deze een relatie aangaat met Microsoft, bijvoorbeeld door zich te registreren voor een Microsoft-cloudservice zoals Azure, Microsoft Intune of Office 365. Elke Azure AD-tenant is uniek en werkt afzonderlijk van andere Azure AD-tenants. 

Een tenant bevat alle gebruikers in een bedrijf en de bijbehorende informatie: hun wachtwoorden, gebruikersprofielgegevens, machtigingen enzovoort. Het bevat ook groepen, toepassingen en andere informatie over een organisatie en de beveiliging.

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij uw toepassing, moet u uw toepassing registreren in een tenant van uzelf. Het maken van een Azure AD-tenant en het publiceren van een toepassing in deze tenant is **helemaal gratis**. U kunt er echter wel voor kiezen om te betalen voor bepaalde premium-functies in uw tenant. Veel ontwikkelaars maken verschillende tenants en toepassingen om te experimenten, te ontwikkelen, te faseren en te testen.

## <a name="use-an-existing-azure-ad-tenant"></a>Een bestaande Azure AD-tenant gebruiken

Veel ontwikkelaars hebben al tenants via services of abonnementen die zijn gekoppeld aan Azure AD-tenants, zoals via Office 365- of Azure-abonnementen. Om te zien of u al een tenant hebt, moet u zich aanmelden bij [Azure Portal](https://portal.azure.com) met het account dat u wilt gebruiken voor het beheren van uw toepassing en kijken wat er in de rechterbovenhoek wordt weergegeven over uw account. Als u een tenant hebt, wordt u automatisch aangemeld bij deze tenant en ziet u de naam van de tenant direct onder de accountnaam. Als u de muisaanwijzer op uw accountnaam houdt rechtsboven op de Azure Portal, ziet u uw naam, uw e-mailadres, directory en tenant-id (een GUID), en uw domein. Als uw account is gekoppeld aan meerdere tenants, kunt u de accountnaam selecteren om een menu te openen waarmee u kunt schakelen tussen de tenants. Elke tenant heeft zijn eigen tenant-id.

> [!TIP]
> Als u de tenant-id wilt, kunt u deze informatie op meerdere manieren vinden. U kunt de muisaanwijzer over de accountnaam bewegen om de tenant-id te zien of u kunt **Azure Active Directory > Eigenschappen > Map-id** in Azure Portal selecteren.

Als er geen bestaande tenant is gekoppeld aan uw account, ziet u een GUID onder de accountnaam. U moet dan eerst [een nieuwe tenant maken](#create-a-new-azure-ad-tenant) voordat u bewerkingen kunt uitvoeren zoals het registreren van apps.

## <a name="create-a-new-azure-ad-tenant"></a>Een nieuwe Azure AD-tenant maken

Als u nog geen Azure AD-tenant hebt of een nieuwe wilt maken, kan dat met behulp van de [interface voor het maken van een map](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) in [Azure Portal](https://portal.azure.com). Dit duurt ongeveer een minuut en u kunt dan meteen naar uw nieuwe tenant navigeren.
