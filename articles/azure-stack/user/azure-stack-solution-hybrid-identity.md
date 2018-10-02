---
title: Hybride cloud-identiteit met Azure en Azure Stack-toepassingen configureren | Microsoft Docs
description: Informatie over het configureren van hybride cloud-identiteit met Azure en Azure Stack-toepassingen.
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
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 7cea11f448f20ace2dd50454834e60ce3a3c3b70
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47584265"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Zelfstudie: Hybride cloud-identiteit voor Azure en Azure Stack-toepassingen configureren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Informatie over het configureren van een hybride cloud-identiteit voor uw toepassingen met Azure en Azure Stack.

U hebt twee opties voor het verlenen van toegang tot uw toepassingen in de globale Azure en Azure Stack.

 * Wanneer Azure Stack een continue verbinding met Internet heeft, kunt u Azure Active Directory (Azure AD) gebruiken.
 * Wanneer Azure Stack niet met Internet verbonden is, kunt u Azure Directory Federated Services (AD FS).

U kunt service-principals gebruiken om toegang te verlenen aan uw Azure Stack-toepassingen voor de implementatie of de configuratie met behulp van Azure Resource Manager in Azure Stack.

In deze zelfstudie bouwt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - Tot stand brengen van een hybride identiteit in de globale Azure en Azure Stack
> - Ophalen van een token voor toegang tot de Azure Stack-API.

U moet machtigingen voor Azure Stack-operator voor de stappen in deze zelfstudie hebben.

> [!Tip]  
> ![hybride-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack is een uitbreiding van Azure. Azure Stack brengt de flexibiliteit en innovatie van cloud computing naar uw on-premises omgeving en de enige hybride cloud waarmee u waar dan ook hybride apps bouwen en implementeren in te schakelen.  
> 
> Het technische document [ontwerpoverwegingen voor hybride toepassingen](https://aka.ms/hybrid-cloud-applications-pillars) beoordelingen van pijlers van softwarekwaliteit (plaatsing, schaalbaarheid, beschikbaarheid, tolerantie, beheerbaarheid en beveiliging) voor het ontwerpen, implementeren en hybride toepassingen. De overwegingen bij het ontwerpen helpen bij het optimaliseren van hybride toepassingsontwerp, uitdagingen in een productieomgeving te minimaliseren.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Maken van een service-principal voor Azure AD in de portal

Als u Azure Stack met Azure AD als de identiteitsarchief hebt geïmplementeerd, kunt u service-principals net zoals u zou voor Azure doen maken. De [service-principals maken](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) artikel leest u hoe u de stappen via de portal uitvoert. Controleer dat u hebt de [vereist Azure AD-machtigingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) voordat u begint.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Maken van een service-principal voor AD FS met behulp van PowerShell

Als u Azure Stack met AD FS hebt geïmplementeerd, kunt u PowerShell gebruiken om te maken van een service-principal en zich aanmelden vanaf PowerShell die identiteit met een rol voor toegang toewijzen. [Service-principal maken voor AD FS](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) ziet u hoe u de vereiste stappen met behulp van PowerShell uitvoeren.

## <a name="using-the-azure-stack-api"></a>Met behulp van de Azure Stack-API

De [API van Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-rest-api-use) zelfstudie leidt u door het proces van het ophalen van een token voor toegang tot de Azure Stack-API.

## <a name="connect-to-azure-stack-using-powershell"></a>Verbinding maken met Azure Stack met behulp van Powershell

De snelstartgids [aan de slag te kunnen met PowerShell in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart) leidt u door de stappen die nodig zijn voor Azure PowerShell installeren en verbinding maken met uw Azure Stack-installatie.

### <a name="prerequisites"></a>Vereisten

De installatie van een Azure Stack is verbonden met Azure Active Directory met een abonnement dat u toegang hebt tot. Als u een Azure Stack-installatie hebt, kunt u deze instructies voor het instellen van een [Azure Stack Development Kit](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy).

#### <a name="connect-to-azure-stack-using-code"></a>Verbinding maken met Azure Stack met behulp van code

Als u wilt verbinding maken met Azure Stack met behulp van code, gebruikmaken van de Azure Resource Manager-eindpunten API ophalen van de verificatie- en eindpunten van de grafiek voor uw Azure Stack-installatie en vervolgens verificatie uitvoeren met behulp van REST-aanvragen. U vindt een voorbeeld-clienttoepassing op [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>Als de Azure SDK voor uw taal naar keuze Azure API-profielen ondersteunt, werkt mogelijk niet de SDK met Azure Stack. Zie voor meer informatie over Azure API-profielen, de [API-versieprofielen beheren](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles) artikel.

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over hoe de identiteit in Azure Stack is verwerkt, [architectuur voor Cloudidentiteit voor Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).
 - Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).
