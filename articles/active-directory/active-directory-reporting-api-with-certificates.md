---
title: Gegevens opvragen met de rapportage-API van Azure AD met certificaten | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u de rapportage-API van Azure AD gebruikt met certificaatreferenties om zonder tussenkomst van de gebruiker gegevens op te halen uit directory's.
services: active-directory
documentationcenter: ''
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: ramical
ms.openlocfilehash: 5ceb3d1ef99189e5b0cfcc48b38906ed28c07730
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588954"
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Gegevens ophalen met de Azure Active Directory rapportage-API met certificaten

De [Azure Active Directory (Azure AD) rapportage-API's](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen.

Als u toegang wilt tot de Azure Active Directory Reporting API zonder tussenkomst van de gebruiker, kunt u uw toegang tot het gebruik van certificaten configureren

Dit artikel:

- Biedt u de vereiste stappen voor toegang tot de Azure Active Directory Reporting API met behulp van certificaten.
- Gaat ervan uit dat u de [vereisten voor toegang tot de Azure Active Directory-rapportage API](active-directory-reporting-api-prerequisites-azure-portal.md) hebt voltooid. 


Voor toegang tot de rapportage-API met certificaten, moet u:

1. De vereiste onderdelen installeren
2. Het certificaat instellen in uw app 
3. Machtigingen verlenen
4. Een toegangstoken opvragen




Zie [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils) (Rapportage-API-module gebruiken) voor meer informatie over broncode. 

## <a name="install-prerequisites"></a>Vereiste onderdelen installeren

Azure Active Directory PowerShell V2 en de module AzureADUtils moeten zijn geïnstalleerd.

1. Download en installeer Azure AD Powershell V2 met behulp van de instructies in [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).

2. Download de Azure AD Utils-module van [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1). 
  Deze module biedt verschillende cmdlets, waaronder:
    - De nieuwste versie van ADAL met Nuget
    - Toegangstokens van gebruiker, toepassingssleutels en certificaten met behulp van ADAL
    - Afhandeling van pagina's met zoekresultaten door Graph API

**De Azure AD Utils-module installeren:**

1. Maak een directory voor het opslaan van de module met hulpprogramma's (bijvoorbeeld c:\azureAD) en downloadt de module in GitHub.
2. Open een PowerShell-sessie en ga naar de directory die u net hebt gemaakt. 
3. Importeer de module en installeer deze met de cmdlet Install-AzureADUtilsModule in het pad van de PowerShell-module. 

De sessie moet er ongeveer uitzien zoals op dit scherm:

  ![Windows Powershell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

## <a name="set-the-certificate-in-your-app"></a>Het certificaat instellen in uw app

**Het certificaat in uw app instellen:**

1. [De object-id ophalen](active-directory-reporting-api-prerequisites-azure-portal.md#get-your-applications-client-id) van uw app vanuit Azure Portal. 

  ![Azure Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Open een PowerShell-sessie en maak verbinding met Azure AD via de cmdlet Connect-AzureAD.

  ![Azure Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. Gebruik de cmdlet New-AzureADApplicationCertificateCredential van AzureADUtils om er een certificaatreferentie aan toe te voegen. 

>[!Note]
>U moet de object-id van de toepassing opgeven die u eerder hebt opgevraagd, evenals het certificaatobject (vraag dit op met behulp van het Cert:-station).
>


  ![Azure Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
## <a name="get-an-access-token"></a>Een toegangstoken opvragen

U kunt een toegangstoken opvragen met de cmdlet **Get-AzureADGraphAPIAccessTokenFromCert** van AzureADUtils. 

>[!NOTE]
>Gebruik de toepassings-id en niet de object-id die u in de laatste sectie hebt gebruikt.
>

 ![Azure Portal](./media/active-directory-report-api-with-certificates/application-id.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Het toegangstoken gebruiken om de Graph API aan te roepen

U kunt nu het script maken. Hieronder ziet u een voorbeeld waarin de cmdlet **Invoke-AzureADGraphAPIQuery** wordt gebruikt. Met deze cmdlet worden meerdere pagina's met zoekresultaten verwerkt, waarna deze resultaten naar de PowerShell-pijplijn worden verstuurd. 

 ![Azure Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

U bent nu klaar om te gaan exporteren naar een CSV-bestand en dit bestand op te slaan in een SIEM-systeem. U kunt uw script ook verpakken in een geplande taak om periodiek gegevens van Azure AD op te halen uit uw tenant zonder dat u toepassingssleutels hoeft op te slaan in de broncode. 

## <a name="next-steps"></a>Volgende stappen

- [Een eerste indruk van de rapportage-API 's krijgen](active-directory-reporting-api-getting-started-azure-portal.md#explore)

- [Uw eigen oplossing maken](active-directory-reporting-api-getting-started-azure-portal.md#customize)




