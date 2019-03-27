---
title: Application Proxy cookie-instellingen - Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) toegangs- en sessiebeleid cookies heeft voor toegang tot on-premises toepassingen via de toepassingsproxy. In dit artikel vindt u informatie over het gebruik en de cookie-instellingen configureren.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: celested
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06fd83ee815e9e207c1fa5a1c6767280122c4d0c
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482688"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>Cookie-instellingen voor toegang tot on-premises toepassingen in Azure Active Directory

Azure Active Directory (Azure AD) toegangs- en sessiebeleid cookies heeft voor toegang tot on-premises toepassingen via de toepassingsproxy. Meer informatie over het gebruik van de toepassingsproxy cookie-instellingen. 

## <a name="what-are-the-cookie-settings"></a>Wat zijn de cookie-instellingen?

[Toepassingsproxy](application-proxy.md) maakt gebruik van de volgende toegangs- en sessiebeleid cookie-instellingen.

| Cookie-instelling | Standaard | Description | Aanbevelingen |
| -------------- | ------- | ----------- | --------------- |
| Alleen-HTTP-cookies gebruiken | **Nee** | **Ja** kunt toepassingsproxy om op te nemen van de vlag HTTPOnly in HTTP-antwoordheaders. Met deze markering biedt extra voordelen met beveiliging, bijvoorbeeld het uitvoeren van scripts aan de clientzijde (CSS) kopiëren of aanpassen van de cookies die voorkomt dat.<br></br><br></br>Voordat we de alleen-HTTP-instelling wordt ondersteund, worden de Application Proxy versleuteld en cookies verzonden via een beveiligde SSL-kanaal om te beveiligen tegen wijziging. | Gebruik **Ja** vanwege de aanvullende beveiligingsvoordelen.<br></br><br></br>Gebruik **geen** voor clients of Gebruikeragents die toegang tot de sessiecookie nodig hebben. Gebruik bijvoorbeeld **geen** voor een RDP- of MTSC client die verbinding met een extern bureaublad-Gateway-server via de toepassingsproxy maakt.|
| Beveiligde cookies gebruiken | **Nee** | **Ja** Application Proxy om op te nemen van de Secure kunt vlag in HTTP-antwoordheaders. Beveiligde Cookies verbetert de beveiliging door het verzenden van cookies via een veilig TLS-kanaal, zoals HTTPS. Dit voorkomt dat cookies waargenomen door onbevoegden vanwege de overdracht van de cookie in niet-versleutelde tekst. | Gebruik **Ja** vanwege de aanvullende beveiligingsvoordelen.|
| Permanente cookies gebruiken | **Nee** | **Ja** kunt toepassingsproxy om in te stellen de toegang-cookies niet verlopen wanneer de webbrowser wordt gesloten. De persistentie duurt totdat het toegangstoken is verlopen, of de gebruiker de permanente cookies voor het handmatig verwijdert. | Gebruik **Nee** vanwege de beveiligingsrisico's die zijn gekoppeld aan om geverifieerde gebruikers te houden.<br></br><br></br>Het is raadzaam alleen met behulp van **Ja** voor oudere toepassingen die cookies tussen processen niet delen. Het is beter om uw toepassing bijwerken voor het verwerken van delen cookies tussen processen in plaats van het gebruik van permanente cookies. Bijvoorbeeld, moet u mogelijk permanente cookies toestaan dat een gebruiker naar het Office-documenten openen in Verkenner-weergave van een SharePoint-site. Zonder permanente cookies worden met deze bewerking kan mislukken als de cookies die toegang niet worden gedeeld tussen de browser, het explorer-proces en de Office-proces. |

## <a name="set-the-cookie-settings---azure-portal"></a>Stel de cookie-instellingen - Azure portal
Om in te stellen de cookie-instellingen met behulp van de Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Navigeer naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen**.
3. Selecteer de toepassing waarvoor u wilt een cookie is ingeschakeld.
4. Klik op **toepassingsproxy**.
5. Onder **extra instellingen**, de cookie instelt op **Ja** of **Nee**.
6. Klik op **opslaan** de wijzigingen worden toegepast. 

## <a name="view-current-cookie-settings---powershell"></a>Huidige cookie-instellingen - PowerShell weergeven

Als de huidige cookie-instellingen voor de toepassing weergeven, gebruikt u deze PowerShell-opdracht:  

```powershell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## <a name="set-cookie-settings---powershell"></a>Cookie-instellingen - PowerShell instellen

In de volgende PowerShell-opdrachten, ```<ObjectId>``` de object-id van de toepassing is. 

**Alleen HTTP-Cookie** 

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Secure Cookie**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Permanente Cookies**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```
