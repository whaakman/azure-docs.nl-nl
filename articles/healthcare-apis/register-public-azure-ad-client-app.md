---
title: Een openbare client-toepassing registreren in Azure Active Directory - API van Azure voor FHIR
description: In dit artikel wordt uitgelegd hoe u een openbare client-toepassing registreren in Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 69504bc9ba0420b47a26519a0b112ff102171254
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824081"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Een openbare client-App in Azure Active Directory registreren

In dit artikel leert u hoe u een openbare toepassing registreren in Azure Active Directory. Client-toepassingsregistraties zijn Azure Active Directory representaties van toepassingen die kunnen worden geverifieerd en vraag om de API-machtigingen namens een gebruiker. Openbare clients zijn toepassingen zoals mobiele toepassingen en één pagina javascript-toepassingen die niet kunnen geheimen vertrouwelijk houden. De procedure is vergelijkbaar met [registreren van een vertrouwelijke client](register-confidential-azure-ad-client-app.md), maar omdat openbare clients niet kunnen vertrouwd worden voor het opslaan van een toepassingsgeheim zijn, is het niet nodig een toe te voegen.

## <a name="app-registrations-in-azure-portal"></a>App-registraties in Azure portal

1. Klik in de [Azure-portal](https://portal.azure.com) in het navigatiepaneel aan de linkerkant op **Azure Active Directory**.

2. In de **Azure Active Directory** blade, klikt u op **(Preview) van de App-registraties**:

    ![Azure-portal. Nieuwe App-registratie.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klik op de **registratie van nieuwe**.

## <a name="application-registration-overview"></a>Overzicht van registratie van toepassingen

1. De toepassing met een naam geven.

2. Geef een antwoord-URL. De antwoord-URL is waar verificatie codes wordt geretourneerd naar de clienttoepassing. U kunt meer antwoord-URL's toevoegen en bestaande later bewerken.

    ![Azure-portal. Nieuwe openbare App-registratie.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)

## <a name="api-permissions"></a>API-machtigingen

Net als de [vertrouwelijke clienttoepassing](register-confidential-azure-ad-client-app.md), moet u selecteren welke API-machtigingen deze toepassing moet kunnen zijn om aan te vragen namens gebruikers:

1. Open de **API-machtigingen** en selecteer uw [FHIR API Resource Toepassingsregistratie](register-resource-azure-ad-client-app.md):

    ![Azure-portal. Nieuwe openbare API-machtigingen.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-API-PERMISSIONS.png)

2. Selecteer de bereiken die u de toepassing dat wilt kunnen zijn om aan te vragen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een openbare client-toepassing registreren in Azure Active Directory. Vervolgens implementeert u een FHIR-API in Azure.
 
>[!div class="nextstepaction"]
>[Open Source-FHIR-server implementeren](fhir-oss-powershell-quickstart.md)
