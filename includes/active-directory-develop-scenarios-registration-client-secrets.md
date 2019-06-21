---
title: bestand opnemen
description: include-bestand voor vertrouwelijke client scenario landingspagina's (daemon, Web-app, Web-API)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 9ee7422b372993d60c629524eb036b9678e5776c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176217"
---
## <a name="registration-of-secrets-or-certificates"></a>Registratie van geheimen of certificaten

Zoals voor elke toepassing vertrouwelijke client moet u een geheim of het certificaat niet registreren. U kunt de toepassingsgeheimen van uw registreren via de interactieve ervaring in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview), of met opdrachtregelprogramma's (zoals PowerShell)

### <a name="registering-client-secrets-using-the-application-registration-portal"></a>Client-geheimen met behulp van de portal voor appregistratie registreren

Het beheer van clientreferenties gebeurt in de **certificaten en geheimen** pagina voor een toepassing:

![image](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png)

- het toepassingsgeheim (ook met de naam clientgeheim) is gegenereerd door Azure AD, tijdens de registratie van de toepassing vertrouwelijke client. Deze generatie gebeurt er wanneer u selecteert **nieuwe clientgeheim**. Op dat moment moet u de geheime tekenreeks in het Klembord voor gebruik in uw app voordat u selecteert **opslaan**. Deze tekenreeks wordt niet meer worden weergegeven.
- het certificaat is geüpload in de toepassing registreren met de **certificaat uploaden** knop

Zie voor meer informatie, [Quick Start: Configureren van een clienttoepassing voor toegang tot web-API's | Referenties toevoegen aan uw toepassing](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application)

### <a name="registering-client-secrets-using-powershell"></a>Registreren van client-geheimen met behulp van PowerShell

U kunt ook uw toepassing registreren met Azure AD met behulp van opdrachtregelprogramma's. De [active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) voorbeeld laat zien hoe u een toepassing het clientgeheim of een certificaat met een Azure AD-toepassing registreren:

- Zie voor meer informatie over het registreren van een toepassingsgeheim [AppCreationScripts/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190)
- Zie voor meer informatie over het registreren van een certificaat met de toepassing [AppCreationScripts-withCert/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178)