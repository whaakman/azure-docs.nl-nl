---
title: Een Azure AD-tenant verkrijgen | Microsoft Docs
description: Een Azure Active Directory-tenant verkrijgen voor het registreren en maken van toepassingen.
services: active-directory
documentationcenter: ''
author: mtillman
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/23/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: ab7db49fa07f260de6ebbe4b2cee943b64cab7fe
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Een Azure Active Directory-tenant verkrijgen
In Azure Active Directory (Azure AD) is een [tenant](https://msdn.microsoft.com/library/azure/jj573650.aspx#Anchor_0) representatief voor een organisatie.  Het is een toegewezen exemplaar van de Azure AD-service die een organisatie ontvangt en waarvan de organisatie de eigenaar is wanneer deze een relatie aangaat met Microsoft, bijvoorbeeld door zich te registreren voor een Microsoft-cloudservice zoals Azure, Microsoft Intune of Office 365.  Elke Azure AD-tenant is uniek en werkt afzonderlijk van andere Azure AD-tenants.  

Een tenant bevat alle gebruikers in een bedrijf en de bijbehorende informatie: hun wachtwoorden, gebruikersprofielgegevens, machtigingen enzovoort.  Het bevat ook groepen, toepassingen en andere informatie over een organisatie en de beveiliging.

Als u wilt dat Azure AD-gebruikers zich kunnen aanmelden bij uw toepassing, moet u uw toepassing registreren in een tenant van uzelf.  Het maken van een Azure AD-tenant en het publiceren van een toepassing in deze tenant is **helemaal gratis** (u kunt er wel voor kiezen om te betalen voor bepaalde premium-functies in uw tenant).  Veel ontwikkelaars maken verschillende tenants en toepassingen om te experimenten, te ontwikkelen, te faseren en te testen.

## <a name="use-an-existing-azure-ad-tenant"></a>Een bestaande Azure AD-tenant gebruiken

Veel ontwikkelaars hebben al tenants via services of abonnementen die zijn gekoppeld aan Azure AD-tenants (bijvoorbeeld via Office 365- of Azure-abonnementen).  Om te zien of u al een tenant hebt, moet u zich aanmelden bij [Azure Portal](https://portal.azure.com) met het account dat u wilt gebruiken voor het beheren van uw toepassing en kijken wat er in de rechterbovenhoek wordt weergegeven over uw account.  Als u een tenant hebt, wordt u automatisch aangemeld bij deze tenant en ziet u de naam van de tenant direct onder de accountnaam.  Als uw account is gekoppeld aan meerdere tenants, kunt u op de accountnaam klikken om een menu te openen waarmee u kunt schakelen tussen de tenants.

Als er geen bestaande tenant is gekoppeld aan uw account, ziet u een GUID onder de accountnaam. U moet dan eerst [een nieuwe tenant maken](#create-a-new-azure-ad-tenant) voordat u bewerkingen kunt uitvoeren zoals het registreren van apps.

## <a name="create-a-new-azure-ad-tenant"></a>Een nieuwe Azure AD-tenant maken

Als u nog geen Azure AD-tenant hebt of een nieuwe wilt maken, kan dat met behulp van de [interface voor het maken van een map](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) in [Azure Portal](https://portal.azure.com).  Dit duurt ongeveer een minuut en u kunt dan meteen naar uw nieuwe tenant navigeren.