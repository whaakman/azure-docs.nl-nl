---
title: Implementatie-bronnen configureren voor App-Services op Azure Stack | Microsoft Docs
description: Hoe een servicebeheerder implementatie bronnen (Git, GitHub, BitBucket, DropBox en OneDrive) kunt configureren voor App-Service op Azure-Stack
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: anwestg
ms.openlocfilehash: dc341d872a3b8943a934217ace21537f45bafd10
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="configure-deployment-sources"></a>Implementatiebronnen configureren

App-Service op Azure-Stack ondersteunt implementatie van meerdere Resourcebeheerproviders op aanvraag. Deze functie kunt direct implementeren vanaf hun source control-opslagplaatsen voor ontwikkelaars van toepassingen. Als gebruikers verbinding maken met hun opslagplaatsen-App Service configureren, moet eerst de integratie tussen op Azure-Stack-App Service en de broncodebeheerprovider configureren in een cloud-operator.  

De Resourcebeheerproviders wordt ondersteund, naast de lokale Git zijn:

* GitHub
* BitBucket
* OneDrive
* DropBox

## <a name="view-deployment-sources-in-app-service-administration"></a>Weergave implementatie bronnen in App Service-beheer

1. Meld u aan bij de beheerportal van de Azure-Stack (https://adminportal.local.azurestack.external) als de servicebeheerder.
2. Blader naar **Resourceproviders** en selecteer de **beheer-App Service Resource Provider**.  ![App Service Resource Provider Admin][1]
3. Klik op **bron configuratie**.  Hier ziet u de lijst met alle implementatie bronnen die zijn geconfigureerd.
    ![App Service Resource Provider Admin bron configuratie][2]

## <a name="configure-github"></a>GitHub configureren

Er moet een GitHub-account om deze taak te voltooien. Mogelijk wilt een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

1. Aanmelden bij GitHub, blader naar https://www.github.com/settings/developers en klikt u op **registreren van een nieuwe toepassing**.
    ![GitHub - een nieuwe toepassing registreren][3]
2. Voer een **toepassingsnaam** voorbeeld - App Service op Azure-Stack.
3. Voer de **startpagina URL**. De startpagina-URL moet een adres van de Stack-Azure-Portal. Bijvoorbeeld: https://portal.local.azurestack.external.
4. Voer een **Toepassingsbeschrijving**.
5. Voer de **retouraanroep-autorisatie-URL**.  De Url is in een Azure-Stack standaardimplementatie in de https://portal.local.azurestack.external/tokenauthorize formulier als u werkt met om uw domein voor azurestack.local onder een ander domein substitute.
    ![GitHub - Register een nieuwe toepassing met waarden ingevuld][4]
6. Klik op **toepassing registreren**.  Nu worden weergegeven met een aanbieding pagina de **Client-ID** en **Clientgeheim** voor de toepassing.
    ![GitHub - registratie van de voltooide toepassing][5]
7.  In een nieuw browsertabblad of venster aanmelden bij de Azure-Stack-beheerportal (https://adminportal.local.azurestack.external) als de servicebeheerder.
8.  Blader naar **Resourceproviders** en selecteer de **beheer-App Service Resource Provider**.
9. Klik op **bron configuratie**.
10. Kopieer en plak de **Client-Id** en **Clientgeheim** dialoogvensters in de bijbehorende invoer voor GitHub.
11. Klik op **Opslaan**.

## <a name="configure-bitbucket"></a>BitBucket configureren

Er moet een BitBucket-account om deze taak te voltooien. Mogelijk wilt een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

1. Aanmelden bij BitBucket en blader naar **integraties** onder uw account.
    ![Dashboard BitBucket - integraties][7]
2. Klik op **OAuth** onder beheer van toegang en **toevoegen consumer**.
    ![BitBucket OAuth Consumer toevoegen][8]
3. Voer een **naam** de consument moet de voorbeeld-App Service op Azure-Stack.
4. Voer een **beschrijving** voor de toepassing.
5. Voer de **retouraanroep URL**.  De Callback-Url is in een Azure-Stack standaardimplementatie in de https://portal.local.azurestack.external/TokenAuthorize formulier als u werkt met om uw domein voor azurestack.local onder een ander domein substitute.  De Url moet het hoofdlettergebruik volgen, zoals hier vermeld voor de BitBucket-integratie te kunnen uitvoeren.
6. Voer de **URL** -deze Url moet de Azure-Stack-URL, bijvoorbeeld https://portal.local.azurestack.external.
7. Selecteer de **machtigingen** vereist:
    - **Opslagplaatsen**: *lezen*
    - **Webhooks**: *lezen en schrijven*
8. Klik op **Opslaan**.  U ziet nu deze nieuwe toepassing, samen met de **sleutel** en **geheim** onder **OAuth consumenten**.
    ![BitBucket toepassing aanbieding][9]
9.  In een nieuw browsertabblad of venster aanmelden bij de Azure-Stack-beheerportal (https://adminportal.local.azurestack.external) als de servicebeheerder.
10.  Blader naar **Resourceproviders** en selecteer de **beheer-App Service Resource Provider**.
11. Klik op **bron configuratie**.
12. Kopieer en plak de **sleutel** in de **Client-Id** invoervak en **geheim** in de **Clientgeheim** invoervak voor BitBucket.
13. Klik op **Opslaan**.


## <a name="configure-onedrive"></a>OneDrive configureren

U moet Microsoft-Account hebt gekoppeld aan een OneDrive-account om deze taak te voltooien.  Mogelijk wilt een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

> [!NOTE]
> OneDrive voor bedrijven-Accounts worden momenteel niet ondersteund.

1. Blader naar https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm en meld aan met uw Microsoft-Account.
2. Onder **mijn toepassingen**, klikt u op **een app toevoegen**.
![OneDrive-toepassingen][10]
3. Voer een **naam** voor de nieuwe registratie van de toepassing invoeren **App-Service op Azure-Stack**, en klik op **toepassing maken**
4. Het volgende scherm worden de eigenschappen van uw nieuwe toepassing. Record de **toepassings-Id**. ![Toepassingseigenschappen OneDrive][11]
5. Onder **toepassing geheimen**, klikt u op **nieuw wachtwoord genereren**. Maak een notitie van **nieuw wachtwoord gegenereerd**. Dit is uw toepassingsgeheim en kan niet worden opgehaald nadat u op **OK** in deze fase.
6. Onder **Platforms** klikt u op **toevoegen Platform** en selecteer **Web**.
7. Voer de **omleidings-URI**.  In een implementatie van de Azure-Stack standaard de omleidings-URI is in de https://portal.local.azurestack.external/tokenauthorize formulier als u werkt met onder een ander domein substitute uw domein voor azurestack.local ![OneDrive-toepassing - toevoegen Webplatform][12]
8. Voeg de **machtigingen voor Microsoft Graph** - **overgedragen machtigingen**
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![OneDrive-toepassing - machtigingen van de grafiek][13]
9. Klik op **Opslaan**.
10.  In een nieuw browsertabblad of venster aanmelden bij de Azure-Stack-beheerportal (https://adminportal.local.azurestack.external) als de servicebeheerder.
11.  Blader naar **Resourceproviders** en selecteer de **beheer-App Service Resource Provider**.
12. Klik op **bron configuratie**.
13. Kopieer en plak de **toepassings-Id** in de **Client-Id** invoervak en **wachtwoord** in de **Clientgeheim** invoervak voor OneDrive.
14. Klik op **Opslaan**.

## <a name="configure-dropbox"></a>DropBox configureren

> [!NOTE]
> U moet een DropBox-account om deze taak te voltooien.  U kunt desgewenst een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

1. Blader naar https://www.dropbox.com/developers/apps en meld aan met uw DropBox-Account.
2. Klik op **-app maken**.

    ![Dropbox-toepassingen][14]

3. Selecteer **DropBox API**.
4. Stel de toegang op **App map**.
5. Voer een **naam** voor uw toepassing.
![Dropbox-toepassing registreren][15]
6. Klik op **-App maken**.  Nu worden weergegeven met een pagina met de instellingen voor de App, waaronder **App-sleutel** en **App geheim**.
7. Controleer de **App mapnaam** is ingesteld op **App-Service op Azure-Stack**.
8. Stel de **OAuth 2 omleidings-URI** en klik op **toevoegen**.  De omleidings-URI is in een Azure-Stack standaardimplementatie in de https://portal.local.azurestack.external/tokenauthorize formulier als u werkt met om uw domein voor azurestack.local onder een ander domein substitute.
![De configuratie van de toepassing dropbox][16]
9.  In een nieuw browsertabblad of venster aanmelden bij de Azure-Stack-beheerportal (https://adminportal.local.azurestack.external) als de servicebeheerder.
10.  Blader naar **Resourceproviders** en selecteer de **beheer-App Service Resource Provider**.
11. Klik op **bron configuratie**.
12. Kopieer en plak de **Toepassingssleutel** in de **Client-Id** invoervak en **App geheim** in de **Clientgeheim** invoervak voor DropBox.
13. Klik op **Opslaan**.


<!--Image references-->
[1]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin.png
[2]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-source-control-configuration.png
[3]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-developer-applications.png
[4]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-populated.png
[5]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-complete.png
[6]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-roles-management-server-repair-all.png
[7]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-dashboard.png
[8]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer.png
[9]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer-complete.png
[10]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-applications.png
[11]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-registration.png
[12]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-platform.png
[13]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-graph-permissions.png
[14]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-applications.png
[15]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-registration.png
[16]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-configuration.png

## <a name="next-steps"></a>Volgende stappen

Gebruikers kunnen nu de implementatie-bronnen gebruiken voor zaken als [continue implementatie](https://docs.microsoft.com/azure/app-service-web/app-service-continuous-deployment), [lokale Git-implementatie](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-local-git), en [cloud mapsynchronisatie](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-content-sync).
