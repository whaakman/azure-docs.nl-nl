---
title: Azure App Service-implementatie referenties | Microsoft Docs
description: Informatie over het gebruik van de referenties voor de Azure App Service-implementatie.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: d66b5aa4eb2ad90596dfe9e26bbc18996c967295
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Referenties voor implementatie configureren voor Azure App Service
[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) ondersteunt twee soorten referenties voor [lokale Git-implementatie](app-service-deploy-local-git.md) en [FTP-/ S implementatie](app-service-deploy-ftp.md). Deze zijn niet hetzelfde zijn als uw Azure Active Directory-referenties.

* **Beveiliging op gebruikersniveau referenties**: één set met referenties voor de volledige Azure-account. Het kan worden gebruikt om te implementeren in App Service voor een app, in een abonnement dat het Azure-account toegang heeft tot. Dit zijn de standaardset van de referenties die u configureert in **App Services** > **&lt;app_naam >** > **implementatiereferenties**. Dit is de standaardinstelling set die wordt opgehaald in de GUI-portal (zoals de **overzicht** en **eigenschappen** van uw app [bronpagina](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > Wanneer u toegang tot Azure-resources via op rollen gebaseerd toegangsbeheer (RBAC) of co-beheerder machtigingen delegeren, kunt elke Azure-gebruiker die toegang tot een app ontvangt stelt zijn/haar persoonlijke op gebruikersniveau referenties gebruiken totdat de toegang is ingetrokken. Deze referenties voor implementatie mag niet worden gedeeld met andere Azure-gebruikers.
    >
    >

* **App-niveau referenties**: één set met referenties voor elke app. Het kan worden gebruikt om deze app alleen te implementeren. De referenties publicatieprofiel voor elke app bij het maken van de app automatisch wordt gegenereerd en is gevonden in de app. U kunt handmatig de referenties niet configureren, maar u kunt ze op elk gewenst moment voor een app herstellen.

    > [!NOTE]
    > Om aan te geven die iemand toegang tot deze referenties via rollen gebaseerd toegangsbeheer (RBAC), moet u ze Inzender of hoger op de Web-App. Lezers zijn niet toegestaan voor het publiceren van, en daarom geen toegang tot deze referenties.
    >
    >

## <a name="userscope"></a>Instellen en de referenties op gebruikersniveau resetten

U kunt uw referenties op gebruikersniveau configureren in elke app [bronpagina](../azure-resource-manager/resource-group-portal.md#manage-resources). Ongeacht in welke app configureert u deze referenties, geldt dit voor alle apps en voor alle abonnementen in uw Azure-account. 

Uw referenties op gebruikersniveau configureren:

1. In de [Azure-portal](https://portal.azure.com), klik op App Service >  **&lt;any_app >** > **implementatiereferenties**.

    > [!NOTE]
    > U moet ten minste één app in de portal hebben voordat u toegang hebt tot de pagina implementatie-referenties. Bij de [Azure CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az_webapp_deployment_user_set), kunt u de referenties op gebruikersniveau zonder een bestaande app configureren.

2. Configureer de gebruikersnaam en wachtwoord en klik vervolgens op **opslaan**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Als u de referenties voor uw implementatie hebt ingesteld, kunt u vinden de *Git* gebruikersnaam van de implementatie in uw app **overzicht**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

en *FTP* gebruikersnaam van de implementatie in uw app **eigenschappen**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Uw wachtwoord op gebruikersniveau implementatie wordt niet door Azure worden weergegeven. Als u het wachtwoord vergeet, kunt u het niet ophalen. U kunt echter uw referenties herstellen door de stappen in deze sectie.
>
>  

## <a name="appscope"></a>Ophalen en app-niveau referenties opnieuw instellen
Voor elke app in App Service, de referenties van de app-niveau worden opgeslagen in het XML-profiel publiceren.

De referenties van de app-niveau ophalen:

1. In de [Azure-portal](https://portal.azure.com), klik op App Service >  **&lt;any_app >** > **overzicht**.

2. Klik op **... Meer** > **Get publicatieprofiel**, en begint met het downloaden voor een. Bestand met publicatie-instellingen.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Open de. Met publicatie-instellingen bestand en zoek de `<publishProfile>` tag met het kenmerk `publishMethod="FTP"`. Haal vervolgens, de `userName` en `password` kenmerken.
Dit zijn de referenties van de app-niveau.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Vergelijkbaar met de referenties op gebruikersniveau, de gebruikersnaam van FTP-implementatie is in de indeling van `<app_name>\<username>`, en de gebruikersnaam Git-implementatie is zojuist `<username>` zonder de voorafgaande `<app_name>\`.

Om in te stellen de referenties van de app-niveau:

1. In de [Azure-portal](https://portal.azure.com), klik op App Service >  **&lt;any_app >** > **overzicht**.

2. Klik op **... Meer** > **Reset publicatieprofiel**. Klik op **Ja** om te bevestigen dat het opnieuw instellen.

    Reset-actie wordt een eerder gedownloade ongeldig. Bestanden met publicatie-instellingen.

## <a name="next-steps"></a>Volgende stappen

Ontdek hoe u kunt deze referenties gebruiken voor het implementeren van uw app uit [lokale Git](app-service-deploy-local-git.md) of met behulp van [FTP/S](app-service-deploy-ftp.md).
