---
title: Hybride cloud-identiteit configureren met Azure en Azure Stack toepassingen | Microsoft Docs
description: Informatie over het configureren van hybride cloud-identiteit in Azure en Azure Stack-toepassingen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/22/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: a57afb4a90da5877879afddc35545e0bfef622a7
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808159"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Zelfstudie: hybride cloud-identiteit voor Azure en Azure Stack-toepassingen configureren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Informatie over het configureren van een hybride cloud-identiteit voor uw toepassingen Azure en Azure-Stack.

Hebt u twee opties voor het verlenen van toegang tot uw toepassingen in de globale Azure- en Azure-Stack.

 * Wanneer Azure Stack een continue verbinding met Internet heeft, kunt u Azure Active Directory (Azure AD).
 * Wanneer u Azure-Stack is niet verbonden met Internet, kunt u Azure Directory Federated Services (AD FS) gebruiken.

U service-principals gebruiken om toegang te verlenen aan uw Azure-Stack toepassingen omwille van de implementatie of een configuratie met behulp van de Azure Resource Manager in Azure-Stack.

In deze zelfstudie maakt u een Voorbeeldomgeving:

> [!div class="checklist"]
> * Tot stand brengen van een hybride identiteit in globale Azure en Azure-Stack
> * Het ophalen van een token voor toegang tot de Azure-Stack-API.

U moet Azure Stack operatormachtigingen hebben voor de stappen in deze zelfstudie.

## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Een service-principal maken voor Azure AD in de portal

Als u Azure-Stack gebruikmaken van Azure AD als identiteit archief hebt geïmplementeerd, kunt u de service-principals, net zoals u zou voor Azure doen maken. De [maken van de service-principals](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) artikel laat zien hoe u de stappen uitvoert via de portal. Controleer dat u hebt de [Azure AD-machtigingen vereist](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) voordat u begint.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Maken van een service-principal voor AD FS met behulp van PowerShell

Als u Azure-Stack met AD FS hebt geïmplementeerd, kunt u PowerShell naar een service-principal maken, een rol voor toegang toe te wijzen en aan te melden met die identiteit PowerShell. [Service-principal maken voor AD FS](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) ziet u hoe u de vereiste stappen uitvoert met behulp van PowerShell.

## <a name="using-the-azure-stack-api"></a>Met behulp van de Azure API-Stack

De [Stack-API van Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-rest-api-use) zelfstudie wordt u begeleid bij het proces van het ophalen van een token voor toegang tot de Azure-Stack-API.

## <a name="connect-to-azure-stack-using-powershell"></a>Verbinding maken met Azure-Stack met behulp van Powershell

De Quick Start [om aan de slag te kunnen met PowerShell in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart) leidt u door de stappen die nodig zijn voor Azure PowerShell installeren en verbinding maken met uw Azure-Stack-installatie.

### <a name="prerequisites"></a>Vereisten

De installatie van een Azure-Stack is verbonden met Azure Active Directory met een abonnement dat u toegang hebt tot. Als u een Azure-Stack-installatie hebt, kunt u deze instructies voor het instellen van een [Azure Stack Development Kit](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy).

#### <a name="connect-to-azure-stack-using-code"></a>Verbinding maken met Azure-Stack met code

Gebruiken voor verbinding met Azure-Stack met code, de Azure Resource Manager-eindpunten API voor ophalen van de verificatie- en eindpunten van de grafiek voor uw Azure-Stack-installatie en vervolgens te verifiëren met behulp van REST-aanvragen. U kunt een voorbeeld van een client-toepassing vinden op [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>Tenzij de Azure SDK voor uw taal van keuze Azure API-profielen ondersteunt, werken mogelijk niet de SDK met de Azure-Stack. Zie voor meer informatie over Azure API-profielen, de [API-versie Profielbeheer](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles) artikel.

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over hoe de identiteit in Azure-Stack wordt verwerkt, [Identity-architectuur voor Azure-Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).
 - Zie voor meer informatie over Azure-Cloud-patronen, [ontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).
