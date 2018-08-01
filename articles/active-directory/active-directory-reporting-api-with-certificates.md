---
title: Gegevens opvragen met de rapportage-API van Azure AD met certificaten | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u de rapportage-API van Azure AD gebruikt met certificaatreferenties om zonder tussenkomst van de gebruiker gegevens op te halen uit directory's.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 44ec19721ba59898915f6547231fb586cb44eb30
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389749"
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Gegevens ophalen met de Azure Active Directory rapportage-API met certificaten

De [Azure Active Directory (Azure AD) rapportage-API's](active-directory-reporting-api-getting-started-azure-portal.md) bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen. Als u toegang wilt tot de Azure AD rapportage-API zonder tussenkomst van de gebruiker, kunt u de toegang voor het gebruik van certificaten configureren.

Dit omvat de volgende stappen:

1. [De vereiste onderdelen installeren](#install-prerequisites)
2. [Het certificaat in uw app registreren](#register-the-certificate-in-your-app)
3. [Een toegangstoken ophalen voor MS Graph API](#get-an-access-token-for-ms-graph-api)
4. [Query uitvoeren op de MS Graph API-eindpunten](#query-the-ms-graph-api-endpoints)


## <a name="install-prerequisites"></a>Vereiste onderdelen installeren

1. Eerst, zorg ervoor dat u hebt voltooid de [vereisten voor toegang tot de Azure Active Directory reporting API](active-directory-reporting-api-prerequisites-azure-portal.md). 

2. Download en installeer Azure AD Powershell V2 met behulp van de instructies op de [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md)

3. Installeer de MSCloudIDUtils van de [PowerShellGallery - MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Deze module biedt verschillende cmdlets, waaronder:
    - De ADAL-bibliotheken die nodig zijn voor verificatie
    - Toegangstokens van gebruiker, toepassingssleutels en certificaten met behulp van ADAL
    - Afhandeling van pagina's met zoekresultaten door Graph API

4. Als dit de eerste keer is met de module voeren **installeren MSCloudIdUtilsModule** om installatie te voltooien, anders kunt u eenvoudig importeren met behulp van de **Import-Module** Powershell-opdracht.

Uw sessie moet lijken op dit scherm:

  ![Windows Powershell](./media/active-directory-reporting-api-with-certificates/module-install.png)

## <a name="register-the-certificate-in-your-app"></a>Het certificaat in uw app registreren

1. Ga eerst naar de registratiepagina van uw toepassing. U kunt dit doen door te navigeren naar de [Azure-portal](https://portal.azure.com), te klikken op **Azure Active Directory**, vervolgens te klikken op **App-registraties** en het kiezen van uw toepassing in de lijst. 

2. Volg daarna de stappen voor het [uw certificaat registreren bij Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-certificate-credentials#register-your-certificate-with-azure-ad) voor de toepassing. 

3. Noteer de toepassings-ID en de vingerafdruk van het certificaat dat u zojuist hebt geregistreerd met uw toepassing. Als u wilt zoeken van de vingerafdruk, op de toepassingspagina van uw in de portal, gaat u naar **instellingen** en klikt u op **sleutels**. De vingerafdruk van het zich onder de **openbare sleutels** lijst.

  
## <a name="get-an-access-token-for-ms-graph-api"></a>Een toegangstoken ophalen voor MS Graph API

Voor een toegangstoken voor de MS Graph-API, gebruikt u de **Get-MSCloudIdMSGraphAccessTokenFromCert** cmdlet uit de MSCloudIdUtils PowerShell-module. 

>[!NOTE]
>U moet de toepassings-ID (ook wel bekend als ClientID) en de vingerafdruk van het certificaat met de persoonlijke sleutel geïnstalleerd in het certificaatarchief van de computer (CurrentUser of LocalMachine-certificaatarchief) gebruiken.
>

 ![Azure Portal](./media/active-directory-reporting-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Het toegangstoken gebruiken om de Graph API aan te roepen

U kunt nu het toegangstoken in uw Powershell-script gebruiken om op te vragen van de Graph API. Hieronder wordt een voorbeeld gebruikt de **Invoke-MSCloudIdMSGraphQuery** cmdlet uit de MSCloudIDUtils het eindpunt signins of diectoryAudits inventariseren. Met deze cmdlet worden meerdere pagina's met zoekresultaten verwerkt, waarna deze resultaten naar de PowerShell-pijplijn worden verstuurd.

### <a name="query-the-directoryaudits-endpoint"></a>Query uitvoeren op het eindpunt DirectoryAudits
 ![Azure Portal](./media/active-directory-reporting-api-with-certificates/query-directoryAudits.png)

 ### <a name="query-the-signins-endpoint"></a>Query uitvoeren op het eindpunt SignIns
 ![Azure Portal](./media/active-directory-reporting-api-with-certificates/query-signins.png)

Nu kunt u deze gegevens exporteren naar een CSV en opslaan in een SIEM-systeem. U kunt uw script ook verpakken in een geplande taak om periodiek gegevens van Azure AD op te halen uit uw tenant zonder dat u toepassingssleutels hoeft op te slaan in de broncode. 


## <a name="next-steps"></a>Volgende stappen

* [Een eerste indruk van de rapportage-API 's krijgen](active-directory-reporting-api-getting-started-azure-portal.md)
* [Controle van de API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Rapport van aanmeldingsactiviteiten API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



