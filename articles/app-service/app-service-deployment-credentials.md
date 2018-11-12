---
title: Azure App Service-implementatie referenties | Microsoft Docs
description: Informatie over het gebruik van de referenties voor Azure App Service-implementatie.
services: app-service
documentationcenter: ''
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
ms.openlocfilehash: 79841887ce8413970cb7dff0ca0099d4e0745a68
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259306"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Referenties voor implementatie voor Azure App Service configureren
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) ondersteunt twee soorten referenties voor [lokale Git-implementatie](app-service-deploy-local-git.md) en [FTP/S implementatie](app-service-deploy-ftp.md). Dit zijn niet gelijk zijn aan uw Azure Active Directory-referenties.

* **De referenties op gebruikersniveau**: één set referenties voor de gehele Azure-account. Het kan worden gebruikt voor het implementeren in App Service voor apps, in elk abonnement dat het Azure-account gemachtigd is voor toegang tot. Dit zijn de standaardset van de referenties die u configureert in **App Services** > **&lt;app_name >** > **implementatiereferenties**. Dit is ook de standaardwaarde instellen die in de portal GUI wordt opgehaald (zoals de **overzicht** en **eigenschappen** van uw app [resourcepagina](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > Wanneer u toegang tot Azure-resources via de rollen gebaseerd toegangsbeheer (RBAC) of co-beheerder machtigingen delegeren, kunt elke Azure-gebruiker die toegang tot een app ontvangt zijn of haar persoonlijke op gebruikersniveau-referenties gebruiken totdat de toegang is ingetrokken. Deze referenties voor implementatie mag niet worden gedeeld met andere Azure-gebruikers.
    >
    >

* **De referenties op App-niveau**: één set referenties voor elke app. Het kan worden gebruikt om te implementeren voor die app alleen. De referenties publiceren voor elke app automatisch wordt gegenereerd tijdens het maken van apps, en is gevonden in van de app profiel. U kunt handmatig de referenties niet configureren, maar u kunt ze op elk gewenst moment voor een app herstellen.

    > [!NOTE]
    > Om aan te geven die iemand toegang tot deze referenties via rollen gebaseerd toegangsbeheer (RBAC), moet u bijdrager maken of hoger op de Web-App. Lezers zijn niet toegestaan om te publiceren, en kan daarom geen toegang tot deze referenties.
    >
    >

## <a name="userscope"></a>Instellen en op gebruikersniveau referenties opnieuw instellen

U kunt uw referenties op gebruikersniveau configureren van een App [resourcepagina](../azure-resource-manager/resource-group-portal.md#manage-resources). Ongeacht in welke app u configureert deze referenties, geldt dit voor alle apps en voor alle abonnementen in uw Azure-account. 

Uw referenties op gebruikersniveau configureren:

1. In de [Azure-portal](https://portal.azure.com), klikt u op App Service >  **&lt;any_app >** > **implementatiereferenties**.

    > [!NOTE]
    > In de portal, moet u ten minste één app hebben voordat u krijgt de implementatiepagina-referenties tot toegang. Met de [Azure CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set), kunt u de referenties op gebruikersniveau zonder een bestaande app configureren.

2. Configureer de gebruikersnaam en het wachtwoord en klik vervolgens op **opslaan**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Als u de referenties voor implementatie hebt ingesteld, kunt u vinden de *Git* gebruikersnaam van de implementatie van uw App **overzicht**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

en *FTP* gebruikersnaam van de implementatie van uw App **eigenschappen**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Uw implementatiewachtwoord op gebruikersniveau niet wordt weergegeven in Azure. Als u het wachtwoord vergeet, kunt u het niet ophalen. U kunt echter uw referenties opnieuw met de volgende stappen in deze sectie.
>
>  

## <a name="appscope"></a>Ophalen en op app-niveau referenties opnieuw instellen
Voor elke app in App Service, de referenties op app-niveau worden opgeslagen in het XML-bestand publicatieprofiel.

De referenties op app-niveau ophalen:

1. In de [Azure-portal](https://portal.azure.com), klikt u op App Service >  **&lt;any_app >** > **overzicht**.

2. Klik op **... Meer** > **publicatieprofiel ophalen**, en begint met het downloaden van een. Het bestand met publicatie-instellingen.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Open het. Met publicatie-instellingen-bestand en zoek de `<publishProfile>` tag met het kenmerk `publishMethod="FTP"`. Vervolgens kunt u de `userName` en `password` kenmerken.
Dit zijn de referenties op app-niveau.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Net als bij de referenties op gebruikersniveau, de gebruikersnaam van de FTP-implementatie is in de indeling van `<app_name>\<username>`, en de gebruikersnaam van de Git-implementatie is zojuist `<username>` zonder de voorafgaande `<app_name>\`.

De referenties op app-niveau opnieuw instellen:

1. In de [Azure-portal](https://portal.azure.com), klikt u op App Service >  **&lt;any_app >** > **overzicht**.

2. Klik op **... Meer** > **publicatieprofiel opnieuw instellen**. Klik op **Ja** om te bevestigen van het opnieuw instellen.

    De actie opnieuw instellen wordt een eerder gedownloade ongeldig. Bestanden met publicatie-instellingen.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u gebruikt deze referenties voor uw app implementeren vanuit [lokale Git](app-service-deploy-local-git.md) of met behulp van [FTP/S](app-service-deploy-ftp.md).
