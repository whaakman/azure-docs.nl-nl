---
title: Bestanden van SharePoint - QnA Maker
titleSuffix: Azure Cognitive Services
description: Beveiligde Sharepoint-gegevensbronnen toevoegen aan uw knowledge base te verrijken van de knowledge base met vragen en antwoorden die kunnen worden beveiligd met Active Directory.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/26/2019
ms.author: tulasim
ms.openlocfilehash: 97da0608781c286e685fd70dd7be37192dd3c9fd
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58580410"
---
# <a name="add-a-secured-sharepoint-data-source-to-your-knowledge-base"></a>Een beveiligde Sharepoint-gegevensbron toevoegen aan uw knowledge base

Beveiligde Sharepoint-gegevensbronnen toevoegen aan uw knowledge base te verrijken van de knowledge base met vragen en antwoorden die kunnen worden beveiligd met Active Directory. 

Wanneer u een beveiligde Sharepoint-document aan uw knowledge base, als de beheerder van de QnA Maker toevoegt, moet u Active Directory-machtigingen voor QnA Maker vragen. Zodra deze machtiging is opgegeven in de Active Directory-beheer met QnA Maker voor toegang tot Sharepoint, moet deze niet opnieuw worden gegeven. Elke volgende document toevoeging aan de knowledge base moet autorisatie niet als het zich in dezelfde Sharepoint-resource. 

Als de QnA Maker knowledge base-manager niet gelijk is aan de Active Directory-manager, moet u om te communiceren met de Active Directory-beheer voor het voltooien van dit proces.

## <a name="add-supported-file-types-to-knowledge-base"></a>Ondersteunde bestandstypen in knowledge base toevoegen

U kunt toevoegen met alle QnA Maker-ondersteunde [bestandstypen](../Concepts/data-sources-supported.md) van een Sharepoint-server aan uw knowledge base. Mogelijk moet u verlenen [machtigingen](#permissions) als de bestandsbron is beveiligd.

1. Selecteer in de Sharepoint-server van het bestand beletseltekenmenu, `...`.
1. Kopieer de URL van het bestand.

    ![De URL van de Sharepoint-bestand ophalen door het selecteren van het bestand beletseltekenmenu vervolgens de URL te kopiëren.](../media/add-sharepoint-datasources/get-sharepoint-file-url.png)

1. In de QnA Maker-portal op de **instellingen** pagina [toevoegen de URL](edit-knowledge-base.md#add-datasource) in de knowledge base. 

## <a name="permissions"></a>Machtigingen

Machtigingen verlenen gebeurt wanneer een beveiligd bestand uit een Sharepoint-server is toegevoegd aan een knowledge base. Afhankelijk van hoe de Sharepoint is ingesteld en de machtigingen van de persoon die het bestand toevoegt die dit heeft mogelijk:

* Er zijn geen extra stappen uitvoeren: de persoon die het bestand toe te voegen heeft alle machtigingen die nodig zijn.
* stappen door beide [knowledge base-manager](#knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal) en [Active Directory-beheer](#active-directory-manager-grant-file-read-access-to-qna-maker).

Zie de onderstaande stappen uitvoeren. 

### <a name="knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal"></a>Knowledge base-manager: Sharepoint-gegevensbron in de QnA Maker portal toevoegen

Wanneer de **QnA Maker manager** een beveiligde Sharepoint-document wordt toegevoegd aan een knowledge base, de kennisdatabase manager start met een verzoek tot toestemming die de Active Directory-beheerder nodig heeft om te voltooien.

De aanvraag begint met een pop-upvenster om Active Directory-account te verifiëren. 

![Verifiëren van gebruikersaccount](../media/add-sharepoint-datasources/authenticate-user-account.png)

Zodra de QnA Maker-manager selecteert het account, is de Active Directory-beheerder ontvangt een melding dat hij moet de QnA Maker (niet de QnA Maker-manager) om apptoegang te verlenen tot de Sharepoint-resource. De Active Directory-beheerder moet dit doen voor elke Sharepoint-resource, maar niet elk document in die bron. 

### <a name="active-directory-manager-grant-file-read-access-to-qna-maker"></a>Active Directorybeheer: bestand lezen toegang verlenen tot QnA Maker

De Active Directory-beheer (niet de QnA Maker-manager) moet toegang verlenen tot QnA Maker toegang tot de Sharepoint-resource selecteren [deze koppeling](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=files.read%20openid%20profile&client_id=c2c11949-e9bb-4035-bda8-59542eb907a6&redirect_uri=https%3A%2F%2Fwww.qnamaker.ai%3A%2FCreate&state=68) om de QnA Maker Portal Sharepoint enterprise-app lezen-bestand te autoriseren machtigingen. 

![Azure Active Directory-beheer verleent machtiging interactief](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)

<!--
The Active Directory manager must grant QnA Maker access either by application name, `QnAMakerPortalSharepoint`, or by application ID, `c2c11949-e9bb-4035-bda8-59542eb907a6`. 
-->
<!--
### Grant access from the interactive pop-up window 

The Active Directory manager will get a pop-up window requesting permissions to the `QnAMakerPortalSharepoint` app. The pop-up window includes the QnA Maker Manager email address that initiated the request, an `App Info` link to learn more about **QnAMakerPortalSharepoint**, and a list of permissions requested. Select **Accept** to provide those permissions. 

![Azure Active Directory manager grants permission interactively](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)
-->
<!--

### Grant access from the App Registrations list

1. The Active Directory manager signs in to the Azure portal and opens **[App registrations list](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade)**. 

1. Search for and select the **QnAMakerPortalSharepoint** app. Change the second filter box from **My apps** to **All apps**. The app information will open on the right side.

    ![Select QnA Maker app in App registrations list](../media/add-sharepoint-datasources/select-qna-maker-app-in-app-registrations.png)

1. Select **Settings**.

    [![Select Settings in the right-side blade](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png)](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png#lightbox)

1. Under **API access**, select **Required permissions**. 

    ![Select 'Settings', then under 'API access', select 'Required permission'](../media/add-sharepoint-datasources/select-required-permissions-in-settings-blade.png)

1. Do not change any settings in the **Enable Access** window. Select **Grant Permission**. 

    [![Under 'Grant Permission', select 'Yes'](../media/add-sharepoint-datasources/grant-app-required-permissions.png)](../media/add-sharepoint-datasources/grant-app-required-permissions.png#lightbox)

1. Select **YES** in the pop-up confirmation windows. 

    ![Grant required permissions](../media/add-sharepoint-datasources/grant-required-permissions.png)

### Grant access from the Azure Active Directory admin center

1. The Active Directory manager signs in to the Azure portal and opens **[Enterprise applications](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps)**. 

1. Search for `QnAMakerPortalSharepoint` the select the QnA Maker app. 

    [![Search for QnAMakerPortalSharepoint in Enterprise apps list](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png)](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png#lightbox)

1. Under **Security**, go to **Permissions**. Select **Grant admin consent for Organization**. 

    [![Select authenticated user for Active Directory Admin](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png)](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png#lightbox)

1. Select a Sign-On account with permissions to grant permissions for the Active Directory. 

-->
  
<!--

## Add Sharepoint data source with APIs

You need to get the Sharepoint file's URI before adding it to QnA Maker. 

## Get Sharepoint File URI

Use the following steps to transform the Sharepoint URL into a sharing token.

1. Encode the URL using [base64](https://en.wikipedia.org/wiki/Base64). 

1. Convert the base64-encoded result to an unpadded base64url format with the following character changes. 

    * Remove the equal character, `=` from the end of the value. 
    * Replace `/` with `_`. 
    * Replace `+` with `-`. 
    * Append `u!` to be beginning of the string. 

1. Sign in to Graph explorer and run the following query, where `sharedURL` is ...:

    ```
    https://graph.microsoft.com/v1.0/shares/<sharedURL>/driveitem
    ```

    Get the **@microsoft.graph.downloadUrl** and use this as `fileuri` in the QnA Maker APIs.

### Add or update a Sharepoint File URI to your knowledge base

Use the **@microsoft.graph.downloadUrl** from the previous section as the `fileuri` in the QnA Maker API for [adding a knowledge base](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) or [updating a knowledge base](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600). The following fields are mandatory: name, fileuri, filename, source.

```
{
    "name": "Knowledge base name",
    "files": [
        {
            "fileUri": "<@microsoft.graph.downloadURL>",
            "fileName": "filename.xlsx",
            "source": "<sharepoint link>"
        }
    ],
    "urls": [],
    "users": [],
    "hostUrl": "",
    "qnaList": []
}
```



## Remove QnA Maker app from Sharepoint authorization

1. Use the steps in the previous section to find the Qna Maker app in the Active Directory admin center. 
1. When you select the **QnAMakerPortalSharepoint**, select **Overview**. 
1. Select **Delete** to remove permissions. 

-->

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Samenwerken aan uw knowledge base](collaborate-knowledge-base.md)