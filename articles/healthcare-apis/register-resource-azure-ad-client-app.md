---
title: Een resourcetoepassing registreren in Azure Active Directory - API van Azure voor FHIR
description: In dit artikel wordt uitgelegd hoe u een resourcetoepassing registreren in Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: c0602d44a5b01e31cd2082e2b05d17c4289adaa1
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824040"
---
# <a name="register-a-resource-application-in-azure-active-directory"></a>Een resource-App in Azure Active Directory registreren

In dit artikel leert u hoe u voor het registreren van een bron (of de API) toepassing in Azure Active Directory. Een resourcetoepassing is een Azure Active Directory-weergave van de server FHIR API zelf en clienttoepassingen toegang tot de resource kunnen aanvragen bij de verificatie. De resourcetoepassing wordt ook wel bekend als de *doelgroep* in OAuth jargon.

## <a name="app-registrations-in-azure-portal"></a>App-registraties in Azure portal

1. Klik in de [Azure-portal](https://portal.azure.com) in het navigatiepaneel aan de linkerkant op **Azure Active Directory**.

2. In de **Azure Active Directory** blade op **(Preview) van de App-registraties**:

    ![Azure-portal. Nieuwe App-registratie.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klik op de **registratie van nieuwe**.

## <a name="add-a-new-application-registration"></a>De registratie van een nieuwe toepassing toevoegen

Vul de details voor de nieuwe toepassing. Er zijn geen specifieke vereisten voor de weergavenaam, maar deze wordt ingesteld op de URI van de server FHIR maakt het gemakkelijk te vinden:

![Nieuwe toepassing registreren](media/how-to-aad/portal-aad-register-new-app-registration-NAME.png)

## <a name="set-identifier-uri-and-define-scopes"></a>Id-URI definiëren en bereiken

De resourcetoepassing van een heeft een URI (URI toepassings-ID), welke clients gebruiken kunnen bij het aanvragen van toegang tot de resource-id. Deze waarde wordt gevuld de `aud` claim van het toegangstoken. Het is raadzaam dat u deze URI naar de URI van uw FHIR-server worden ingesteld. Voor de SMARTCARD op FHIR-apps, wordt ervan uitgegaan dat de *doelgroep* is de URI van de FHIR-server.

1. Klik op **beschikbaar maken van een API**

2. Klik op **ingesteld** naast *URI toepassings-ID*.

3. Klik op **een bereik toevoegen** en alle scopes die u wilt definiëren voor u API toe te voegen. Azure AD is momenteel niet toegestaan voor slashes (`/`) in de scopenamen van de. Wordt u aangeraden `$` in plaats daarvan. Een bereik, zoals `patient/*.read` zou `patient$*.read`.

    ![Doelgroep en bereik](media/how-to-aad/portal-aad-register-new-app-registration-AUD-SCOPE.png)

## <a name="define-application-roles"></a>Toepassingsrollen definiëren

De API van Azure voor FHIR en de OSS FHIR-Server voor Azure-gebruik [rollen voor Azure Active Directory-toepassing](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) voor op rollen gebaseerd toegangsbeheer. Om te definiëren welke functies beschikbaar zijn voor uw API FHIR-Server moeten zijn, opent u de resourcetoepassing [manifest](https://docs.microsoft.com/azure/active-directory/active-directory-application-manifest/):

1. Klik op **Manifest**:

    ![Toepassingsrollen](media/how-to-aad/portal-aad-register-new-app-registration-APP-ROLES.png)

2. In de `appRoles` eigenschap, de rollen die u dat wilt gebruikers of toepassingen hebben toevoegen:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User",
          "Application"
        ],
        "description": "FHIR Server Administrators",
        "displayName": "admin",
        "id": "1b4f816e-5eaf-48b9-8613-7923830595ad",
        "isEnabled": true,
        "value": "admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "description": "Users who can read",
        "displayName": "reader",
        "id": "c20e145e-5459-4a6c-a074-b942bbd4cfe1",
        "isEnabled": true,
        "value": "reader"
      }
    ],
    ```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een resourcetoepassing registreren in Azure Active Directory. Vervolgens implementeert u een FHIR-API in Azure.
 
>[!div class="nextstepaction"]
>[Open Source-FHIR-server implementeren](fhir-oss-powershell-quickstart.md)