---
title: Bestanden van SharePoint - QnA Maker
titleSuffix: Azure Cognitive Services
description: Beveiligde SharePoint-gegevensbronnen toevoegen aan uw knowledge base te verrijken van de knowledge base met vragen en antwoorden die kunnen worden beveiligd met Active Directory.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/24/2019
ms.author: diberry
ms.openlocfilehash: 99b20e36163ec8c91fba864706f883b7866de65c
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592903"
---
# <a name="add-a-secured-sharepoint-data-source-to-your-knowledge-base"></a>Een beveiligde SharePoint-gegevensbron toevoegen aan uw knowledge base

Beveiligde SharePoint-gegevensbronnen toevoegen aan uw knowledge base te verrijken van de knowledge base met vragen en antwoorden die kunnen worden beveiligd met Active Directory. 

Wanneer u een beveiligde SharePoint-document aan uw knowledge base, als de beheerder van de QnA Maker toevoegt, moet u Active Directory-machtigingen voor QnA Maker vragen. Zodra deze machtiging is opgegeven in de Active Directory-beheer met QnA Maker voor toegang tot SharePoint, moet deze niet opnieuw worden gegeven. Elke volgende document toevoeging aan de knowledge base moet autorisatie niet als het zich in dezelfde SharePoint-resource. 

Als de QnA Maker knowledge base-manager niet gelijk is aan de Active Directory-manager, moet u om te communiceren met de Active Directory-beheer voor het voltooien van dit proces.

## <a name="add-supported-file-types-to-knowledge-base"></a>Ondersteunde bestandstypen in knowledge base toevoegen

U kunt toevoegen met alle QnA Maker-ondersteunde [bestandstypen](../Concepts/data-sources-supported.md) uit een SharePoint-site aan uw knowledge base. Mogelijk moet u verlenen [machtigingen](#permissions) als de bestandsbron is beveiligd.

1. Selecteer in de bibliotheek met de SharePoint-site, beletseltekenmenu van het bestand, `...`.
1. Kopieer de URL van het bestand.

    ![De URL van de SharePoint-bestand ophalen door het selecteren van het bestand beletseltekenmenu vervolgens de URL te kopiëren.](../media/add-sharepoint-datasources/get-sharepoint-file-url.png)

1. In de QnA Maker-portal op de **instellingen** pagina [toevoegen de URL](edit-knowledge-base.md#add-datasource) in de knowledge base. 

### <a name="images-with-sharepoint-files"></a>Installatiekopieën met SharePoint-bestanden

Als bestanden afbeeldingen bevatten, worden deze niet opgehaald. U kunt de installatiekopie toevoegen vanuit de portal QnA Maker, nadat het bestand is uitgepakt naar paren met QnA.

De installatiekopie met de volgende markdown-syntaxis kunt toevoegen: 

```markdown
![Explanation or description of image](URL of public image)
```

De tekst in de tussen vierkante haken, `[]`, wordt de installatiekopie van het uitgelegd. De URL in de haakjes `()`, is de directe koppeling naar de installatiekopie. 

Wanneer u het paar QnA in het deelvenster interactieve test in de portal voor QnA Maker test, wordt de afbeelding weergegeven, in plaats van de markdown-tekst. Hiermee valideert u de installatiekopie van het openbaar kunnen worden opgehaald uit uw clienttoepassing.

## <a name="permissions"></a>Machtigingen

Machtigingen verlenen gebeurt wanneer een beveiligd bestand uit een SharePoint-site wordt toegevoegd aan een knowledge base. Afhankelijk van hoe de SharePoint is ingesteld en de machtigingen van de persoon die het bestand toevoegt die dit heeft mogelijk:

* Er zijn geen extra stappen uitvoeren: de persoon die het bestand toe te voegen heeft alle machtigingen die nodig zijn.
* stappen door beide [knowledge base-manager](#knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal) en [Active Directory-beheer](#active-directory-manager-grant-file-read-access-to-qna-maker).

Zie de onderstaande stappen uitvoeren. 

### <a name="knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal"></a>Knowledge base-manager: SharePoint-gegevensbron in de QnA Maker portal toevoegen

Wanneer de **QnA Maker manager** een beveiligde SharePoint-document wordt toegevoegd aan een knowledge base, de kennisdatabase manager start met een verzoek tot toestemming die de Active Directory-beheerder nodig heeft om te voltooien.

De aanvraag begint met een pop-upvenster om Active Directory-account te verifiëren. 

![Verifiëren van gebruikersaccount](../media/add-sharepoint-datasources/authenticate-user-account.png)

Zodra de QnA Maker-manager selecteert het account, is de Azure Active Directory-beheerder ontvangt een melding dat ze nodig hebben om toe te staan de QnA Maker (niet de QnA Maker-manager) apptoegang tot de SharePoint-resource. De Azure Active Directory-beheerder moet dit doen voor elke SharePoint-resource, maar niet elk document in die bron. 

### <a name="active-directory-manager-grant-file-read-access-to-qna-maker"></a>Active Directorybeheer: bestand lezen toegang verlenen tot QnA Maker

De Active Directory-beheer (niet de QnA Maker-manager) moet toegang verlenen tot QnA Maker toegang tot de SharePoint-resource selecteren [deze koppeling](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=Files.Read%20Files.Read.All%20Sites.Read.All%20User.Read%20User.ReadBasic.All%20profile%20openid%20email&client_id=c2c11949-e9bb-4035-bda8-59542eb907a6&redirect_uri=https%3A%2F%2Fwww.qnamaker.ai%3A%2FCreate&state=68) om de QnA Maker Portal SharePoint enterprise-app lezen-bestand te autoriseren machtigingen. 

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
-->
### <a name="grant-access-from-the-azure-active-directory-admin-center"></a>Toegang verlenen vanuit de Azure Active Directory-beheercentrum

1. De Active Directory-beheerder zich aanmeldt bij Azure portal en wordt geopend  **[bedrijfstoepassingen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps)** . 

1. Zoeken naar `QnAMakerPortalSharepoint` het selecteren van de QnA Maker-app. 

    [![QnAMakerPortalSharepoint zoeken in de lijst met Enterprise-apps](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png)](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png#lightbox)

1. Onder **Security**, gaat u naar **machtigingen**. Selecteer **beheerder toestemming voor organisatie**. 

    [![Selecteer de geverifieerde gebruiker voor Active Directory-beheerder](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png)](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png#lightbox)

1. Selecteer een Sign-On-account met machtigingen voor het verlenen van machtigingen voor de Active Directory. 


  
<!--

## Add SharePoint data source with APIs

You need to get the SharePoint file's URI before adding it to QnA Maker. 

## Get SharePoint File URI

Use the following steps to transform the SharePoint URL into a sharing token.

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

### Add or update a SharePoint File URI to your knowledge base

Use the **@microsoft.graph.downloadUrl** from the previous section as the `fileuri` in the QnA Maker API for [adding a knowledge base](https://go.microsoft.com/fwlink/?linkid=2092179) or [updating a knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). The following fields are mandatory: name, fileuri, filename, source.

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



## Remove QnA Maker app from SharePoint authorization

1. Use the steps in the previous section to find the Qna Maker app in the Active Directory admin center. 
1. When you select the **QnAMakerPortalSharePoint**, select **Overview**. 
1. Select **Delete** to remove permissions. 

-->

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Samenwerken aan uw knowledge base](collaborate-knowledge-base.md)
