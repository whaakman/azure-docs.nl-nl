---
title: bestand opnemen
description: bestand insluiten voor vertrouwelijke client scenario landings pagina's (daemon, Web-app, Web-API)
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
ms.openlocfilehash: 8f98808aa0f8a2c32e2117447824114747091a82
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912340"
---
## <a name="registration-of-secrets-or-certificates"></a>Registratie van geheimen of certificaten

Net als bij elke vertrouwelijke client toepassing moet u een geheim of certificaat registreren. U kunt uw toepassings geheimen registreren via de interactieve ervaring in de [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview), of opdracht regel Programma's gebruiken (zoals Power shell)

### <a name="registering-client-secrets-using-the-application-registration-portal"></a>Client geheimen registreren met de portal voor toepassings registratie

Het beheer van client referenties vindt plaats op de pagina **certificaten & geheimen** voor een toepassing:

![image](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets.png)

- het toepassings geheim (ook wel client geheim genoemd) wordt gegenereerd door Azure AD tijdens de registratie van de vertrouwelijke client toepassing. Deze generatie gebeurt wanneer u **Nieuw client geheim**selecteert. Op dat moment moet u de geheime teken reeks in het klem bord kopiëren voor gebruik in uw app voordat u **Opslaan**selecteert. Deze teken reeks wordt niet meer weer gegeven.
- het certificaat wordt geüpload in de registratie van de toepassing met behulp van de knop **certificaat uploaden** . Azure AD ondersteunt alleen certificaten die rechtstreeks zijn geregistreerd bij de toepassing en die geen certificaat ketens volgen.

Voor meer informatie raadpleegt [u Quick Start: Een client toepassing configureren voor toegang tot Web-Api's | Referenties toevoegen aan uw toepassing](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application)



### <a name="registering-client-secrets-using-powershell"></a>Client geheimen registreren met Power shell

U kunt uw toepassing ook registreren bij Azure AD met behulp van opdracht regel Programma's. In het voor beeld [Active-Directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) ziet u hoe u een toepassings geheim of een certificaat bij een Azure AD-toepassing registreert:

- Zie [AppCreationScripts/configure. ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190) voor meer informatie over het registreren van een toepassings geheim.
- Zie [AppCreationScripts-withCert/configure. ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178) voor meer informatie over het registreren van een certificaat bij de toepassing.
