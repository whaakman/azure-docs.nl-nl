---
title: Implementatiebronnen configureren voor App-Services in Azure Stack | Microsoft Docs
description: Hoe een servicebeheerder implementatiebronnen (Git, GitHub, BitBucket, DropBox en OneDrive) voor App-Services in Azure Stack kunt configureren
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: sethm
ms.reviewer: anwestg
ms.openlocfilehash: b81b9b34843974ceacf41b08a40a52418ea46d22
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53547546"
---
# <a name="configure-deployment-sources"></a>Implementatiebronnen configureren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

App Service in Azure Stack biedt ondersteuning voor implementatie van meerdere resourcebeheerproviders op aanvraag. Deze functie kunt toepassingsontwikkelaars rechtstreeks implementeren via hun broncodebeheeropslagplaatsen. Als gebruikers configureren, App Service wilt verbinden met hun opslagplaatsen, moet eerst de integratie tussen App Service in Azure Stack en de gewenste provider configureren in een cloud-operator.  

Naast de lokale Git, worden de volgende resourcebeheerproviders ondersteund:

* GitHub
* BitBucket
* OneDrive
* DropBox

## <a name="view-deployment-sources-in-app-service-administration"></a>Implementatie van bronnen weergeven in App Service-beheer

1. Aanmelden bij de Azure Stack-beheerportal (https://adminportal.local.azurestack.external) als de service-beheerder.
2. Blader naar **alle Services** en selecteer de **App Service**.
    ![App Service Resource Provider Admin][1]
3. Klik op **bronbeheerconfiguratie**. Hier ziet u de lijst met alle geconfigureerde implementatiebronnen.
    ![App Service Resource Provider Admin Bronbeheerconfiguratie][2]

## <a name="configure-github"></a>Configureren van GitHub

Hebt u een GitHub-account om deze taak te voltooien. Mogelijk wilt u een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

1. Zich aanmelden met GitHub, blader naar https://www.github.com/settings/developers, en klik vervolgens op **een nieuwe toepassing registreren**.
    ![GitHub - een nieuwe toepassing registreren][3]
2. Voer een **toepassingsnaam**, bijvoorbeeld **App Service in Azure Stack**.
3. Voer de **URL van de startpagina**. URL van de startpagina moet het adres van Azure Stack-portal. Bijvoorbeeld https://portal.local.azurestack.external.
4. Voer een **Toepassingsbeschrijving**.
5. Voer de **URL voor terugbellen voor autorisatie**. In een standaard Azure Stack-implementatie, de URL in het formulier is https://portal.local.azurestack.external/TokenAuthorize. Als u worden uitgevoerd onder een ander domein, vervangt u de naam van het domein voor local.azurestack.external.
6. Klik op **toepassing registreren**. Een pagina wordt weergegeven met de **Client-ID** en **Clientgeheim** voor de toepassing.
    ![GitHub - voltooide toepassing registreren][5]
7.  In een nieuw browsertabblad of -venster moet u zich aanmelden bij de Azure Stack-beheerportal (https://adminportal.local.azurestack.external) als de service-beheerder.
8.  Blader naar **Resourceproviders**, en selecteer de **beheer van App Service Resource Provider**.
9. Klik op **bronbeheerconfiguratie**.
10. Kopieer en plak de **Client-ID** en **Clientgeheim** dialoogvensters in de bijbehorende invoer voor GitHub.
11. Klik op **Opslaan**.

## <a name="configure-bitbucket"></a>BitBucket configureren

U moet een BitBucket-account om deze taak te voltooien. Mogelijk wilt u een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

1. Aanmelden bij BitBucket en blader naar **integraties** onder uw account.
    ![Dashboard van de BitBucket - integraties][7]
2. Klik op **OAuth** onder beheer van toegang en **toevoegen consument**.
    ![BitBucket OAuth-consument toevoegen][8]
3. Voer een **naam** voor de consument; bijvoorbeeld, **App Service in Azure Stack**.
4. Voer een **beschrijving** voor de toepassing.
5. Voer de **URL voor terugbellen**. In een standaard Azure Stack-implementatie, de callback-URL is in het formulier https://portal.local.azurestack.external/TokenAuthorize. Als u worden uitgevoerd onder een ander domein, vervangt u de naam van het domein voor azurestack.local. Voor de integratie van de BitBucket te voltooien, moet de URL volgen het hoofdlettergebruik die hier worden vermeld.
6. Voer de **URL**. Deze URL moet de Azure Stack-portal URL; bijvoorbeeld, https://portal.local.azurestack.external.
7. Selecteer de **machtigingen** vereist:
    - **Opslagplaatsen**: *Lezen*
    - **Webhooks**: *Lezen en schrijven*
8. Klik op **Opslaan**. Nu ziet u deze nieuwe toepassing, samen met de **sleutel** en **geheim**onder **OAuth consumenten**.
    ![Aanbieding voor BitBucket-toepassing][9]
9.  In een nieuw browsertabblad of -venster moet u zich aanmelden bij de Azure Stack-beheerportal (https://adminportal.local.azurestack.external) als de service-beheerder.
10.  Blader naar **Resourceproviders** en selecteer de **beheer van App Service Resource Provider**.
11. Klik op **bronbeheerconfiguratie**.
12. Kopieer en plak de **sleutel** in de **Client-ID** invoervak en **geheim** in de **Clientgeheim** invoervak voor BitBucket.
13. Klik op **Opslaan**.

## <a name="configure-onedrive"></a>Configureren van OneDrive

U moet Microsoft-Account hebt gekoppeld aan een OneDrive-account om deze taak te voltooien.  Mogelijk wilt u een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

> [!NOTE]
> OneDrive voor bedrijven-accounts worden momenteel niet ondersteund.

1. Blader naar https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm en meld u aan met uw Microsoft-Account.
2. Onder **mijn toepassingen**, klikt u op **een app toevoegen**.
![OneDrive-toepassingen][10]
3. Voer een **naam** voor de registratie van nieuwe toepassing: Voer **App Service in Azure Stack**, en klik vervolgens op **toepassing maken**
4. Het volgende scherm worden de eigenschappen van uw nieuwe toepassing. Sla de **toepassings-ID** naar een tijdelijke locatie.
![Eigenschappen van de toepassing OneDrive][11]
5. Onder **Toepassingsgeheimen**, klikt u op **nieuw wachtwoord genereren**. Noteer de **nieuw wachtwoord gegenereerd**. Dit is het toepassingsgeheim en kan niet worden opgehaald nadat u op **OK**.
6. Onder **Platforms**, klikt u op **Platform toevoegen**, en selecteer vervolgens **Web**.
7. Voer de **omleidings-URI**. In een standaard Azure Stack-implementatie, de omleidings-URI is in het formulier https://portal.local.azurestack.external/TokenAuthorize. Als u worden uitgevoerd onder een ander domein, vervangt u de naam van het domein voor azurestack.local.
![OneDrive-toepassing - webplatform toevoegen][12]
8. Voeg de **machtigingen voor Microsoft Graph** - **gedelegeerde machtigingen**
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![OneDrive-toepassing - machtigingen voor Graph][13]
9. Klik op **Opslaan**.
10.  In een nieuw browsertabblad of -venster moet u zich aanmelden bij de Azure Stack-beheerportal (https://adminportal.local.azurestack.external) als de service-beheerder.
11.  Blader naar **Resourceproviders** en selecteer de **beheer van App Service Resource Provider**.
12. Klik op **bronbeheerconfiguratie**.
13. Kopieer en plak de **toepassings-ID** in de **Client-ID** invoervak en **wachtwoord** in de **Clientgeheim** invoervak voor OneDrive.
14. Klik op **Opslaan**.

## <a name="configure-dropbox"></a>DropBox configureren

> [!NOTE]
> U moet een DropBox-account om deze taak te voltooien. Mogelijk wilt u een account gebruiken voor uw organisatie in plaats van een persoonlijk account.

1. Blader naar https://www.dropbox.com/developers/apps en meld u aan met behulp van de referenties van uw DropBox-account.
2. Klik op **-app maken**.

    ![Dropbox-toepassingen][14]

3. Selecteer **DropBox API**.
4. Stel het op **App map**.
5. Voer een **naam** voor uw toepassing.
![Dropbox-toepassing registreren][15]
6. Klik op **-App maken**. Krijgt u een pagina weergegeven van de instellingen voor de app, met inbegrip van **App-sleutel** en **appgeheim**.
7. Zorg ervoor dat de **App mapnaam** is ingesteld op **App Service in Azure Stack**.
8. Stel de **OAuth 2 omleidings-URI** en klik vervolgens op **toevoegen**. In een standaard Azure Stack-implementatie, de omleidings-URI is in het formulier https://portal.local.azurestack.external/TokenAuthorize. Als u worden uitgevoerd onder een ander domein, vervangt u uw domein voor azurestack.local.
![De configuratie van de dropbox-toepassing][16]
9.  In een nieuw browsertabblad of -venster moet u zich aanmelden bij de Azure Stack-beheerportal (https://adminportal.local.azurestack.external) als de service-beheerder.
10.  Blader naar **Resourceproviders** en selecteer de **beheer van App Service Resource Provider**.
11. Klik op **bronbeheerconfiguratie**.
12. Kopieer en plak de **Toepassingssleutel** in de **Client-ID** invoervak en **appgeheim** in de **Clientgeheim** invoervak voor DropBox.
13. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

Gebruikers kunnen nu de van implementatiebronnen gebruiken voor zaken zoals [continue implementatie](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment), [lokale Git-implementatie](https://docs.microsoft.com/azure/app-service/deploy-local-git), en [cloud mapsynchronisatie](https://docs.microsoft.com/azure/app-service-web/deploy-content-sync).

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
