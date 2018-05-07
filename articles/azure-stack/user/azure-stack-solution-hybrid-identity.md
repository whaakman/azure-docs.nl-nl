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
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 9a025716c2bb6266c1c1c552a2d0791b39429cac
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-configure-hybrid-cloud-identity-with-azure-and-azure-stack-applications"></a>Zelfstudie: Hybride cloud-identiteit in Azure en Azure Stack-toepassingen configureren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Hebt u twee opties toegang geven tot uw toepassingen in zowel globale Azure en Azure-Stack. Wanneer Azure Stack een continue verbinding met Internet heeft, kunt u Azure Active Directory (Azure AD). Wanneer u Azure-Stack is niet verbonden met Internet, kunt u Azure Directory Federated Services (AD FS) gebruiken. U service-principals gebruiken om toegang te verlenen aan uw toepassingen in Azure-Stack om het te implementeren of configuratie via de Azure Resource Manager in Azure-Stack. 

In deze zelfstudie maakt u een Voorbeeldomgeving:

> [!div class="checklist"]
> * Tot stand brengen van een hybride identiteit in globale Azure en Azure-Stack
> * Het ophalen van een token voor toegang tot de Azure-Stack-API.

Deze stappen vereisen dat u Azure-Stack operatormachtigingen.

## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Een service-principal maken voor Azure AD in de portal

Als u Azure-Stack gebruikmaken van Azure AD als identiteit archief hebt geïmplementeerd, kunt u de service-principals, net zoals u zou voor Azure doen maken. [Dit document](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) ziet u hoe u de stappen uitvoert via de portal. Controleer of u hebt de [Azure AD-machtigingen vereist](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) voordat u begint.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Maken van een service-principal voor AD FS met behulp van PowerShell

Als u Azure-Stack met AD FS hebt geïmplementeerd, kunt u PowerShell naar een service-principal maken, een rol voor toegang toe te wijzen en aan te melden met die identiteit PowerShell. [Dit document](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) ziet u hoe u de stappen uitvoert met behulp van PowerShell.

## <a name="using-the-azure-stack-api"></a>Met behulp van de Azure API-Stack

[Deze zelfstudie](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-rest-api-use) begeleidt u stapsgewijs door het proces van het ophalen van een token voor toegang tot de Azure-Stack-API.

## <a name="connect-to-azure-stack-using-powershell"></a>Verbinding maken met Azure-Stack met behulp van Powershell

Hier volgt een voorbeeld van een script met behulp van de concepten geleerd in dit document voor het verbinden met uw Azure-Stack.

### <a name="prerequisites"></a>Vereisten

De installatie van een Azure-Stack is verbonden met Azure Active Directory met een abonnement dat u toegang hebt tot. Als u een Azure-Stack-installatie hebt, kunt u deze instructies voor het instellen van een [Azure Stack Development Kit](https://docs.microsoft.com/en-us/azure/azure-stack/asdk/asdk-deploy).

[Deze zelfstudie](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-configure-quickstart) begeleidt u stapsgewijs door de stappen die nodig zijn voor het Azure PowerShell installeren en verbinding maken met uw Azure-Stack-installatie.

#### <a name="connect-to-azure-stack-using-code"></a>Verbinding maken met Azure-Stack met code

Gebruiken voor verbinding met Azure-Stack met code, de Azure Resource Manager-eindpunten API voor ophalen van de verificatie- en eindpunten van de grafiek voor uw Azure-Stack-installatie en vervolgens te verifiëren met behulp van REST-aanvragen. U vindt een voorbeeldtoepassing [hier](https://github.com/shriramnat/HybridARMApplication).

> [!note]  
Tenzij de Azure SDK voor uw taal van keuze Azure API-profielen ondersteunt, werken mogelijk niet de SDK met de Azure-Stack. Ga voor meer informatie over Azure API-profielen, [hier](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles).

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over hoe de identiteit in Azure-Stack wordt verwerkt, [Identity-architectuur voor Azure-Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).  
 - Zie voor meer informatie over Azure-Cloud-patronen, [ontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).
