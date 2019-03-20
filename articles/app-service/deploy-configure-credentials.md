---
title: Referenties voor implementatie - Azure App Service configureren | Microsoft Docs
description: Informatie over het gebruik van de referenties voor Azure App Service-implementatie.
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnoc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/10/2019
ms.author: cephalin;byvinyal
ms.custom: seodec18
ms.openlocfilehash: df874ab77c88f05b048b1f9d10873943b7bebf36
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57884384"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Referenties voor implementatie voor Azure App Service configureren
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) ondersteunt twee soorten referenties voor [lokale Git-implementatie](deploy-local-git.md) en [FTP/S implementatie](deploy-ftp.md). Deze referenties zijn niet gelijk zijn aan uw Azure Active Directory-referenties.

* **De referenties op gebruikersniveau**: één set referenties voor de gehele Azure-account. Het kan worden gebruikt voor het implementeren in App Service voor apps, in elk abonnement dat het Azure-account gemachtigd is voor toegang tot. Het is een standaardreeks die in de portal GUI wordt opgehaald (zoals de **overzicht** en **eigenschappen** van van de app [resourcepagina](../azure-resource-manager/manage-resources-portal.md#manage-resources)). Als een gebruiker is toegang tot Apps via Role-Based Access Control (RBAC) of coadmin machtigingen worden verleend, kan die gebruiker zijn of haar eigen gebruikersniveau-referenties gebruiken totdat de toegang is ingetrokken. U mag deze referenties niet delen met andere Azure-gebruikers.

* **De referenties op App-niveau**: één set referenties voor elke app. Het kan worden gebruikt om te implementeren voor die app alleen. De referenties voor elke app worden automatisch gegenereerd tijdens het maken van apps. Ze kunnen niet handmatig worden geconfigureerd, maar u kunnen op elk gewenst moment opnieuw instellen. Voor een gebruiker toegang heeft tot de referenties op app-niveau via (RBAC) worden verleend, moet die gebruiker Inzender of hoger op de app. Lezers zijn niet toegestaan om te publiceren, en geen toegang tot deze referenties.

## <a name="userscope"></a>Instellen en op gebruikersniveau referenties opnieuw instellen

U kunt uw referenties op gebruikersniveau configureren van een App [resourcepagina](../azure-resource-manager/manage-resources-portal.md#manage-resources). Ongeacht in welke app u configureert deze referenties, geldt dit voor alle apps en voor alle abonnementen in uw Azure-account. 

Uw referenties op gebruikersniveau configureren:

1. In de [Azure-portal](https://portal.azure.com), in het menu links, klikt u op **App Services** > **&lt;any_app >** > **implementatie Center** > **implementatiereferenties**.

    In de portal, moet u ten minste één app hebben voordat u krijgt de implementatiepagina-referenties tot toegang. Met de [Azure CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set), kunt u de referenties op gebruikersniveau zonder een bestaande app configureren.

2. Klik op **gebruikersreferenties**, configureren van de gebruikersnaam en wachtwoord en klik vervolgens op **referenties opslaan**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Als u de referenties voor implementatie hebt ingesteld, kunt u vinden de *Git* gebruikersnaam van de implementatie van uw App **overzicht**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

en *FTP* gebruikersnaam van de implementatie van uw App **eigenschappen**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Uw implementatiewachtwoord op gebruikersniveau niet wordt weergegeven in Azure. Als u het wachtwoord vergeet, kunt u uw referenties opnieuw instellen door de stappen in dit gedeelte te volgen.
>
>  

## <a name="use-user-level-credentials-with-ftpftps"></a>Gebruik op gebruikersniveau referenties met FTP/FTPS

Verificatie bij een FTP-/ FTPS-eindpunt met behulp van de referenties op gebruikersniveau requirers een gebruikersnaam in de volgende indeling: `<app-name>\<user-name>`

Omdat op gebruikersniveau referenties zijn gekoppeld aan de gebruiker en niet op een specifieke bron, wordt de gebruikersnaam moet in deze indeling om te leiden van de actie aanmelden op de juiste app-eindpunt.

## <a name="appscope"></a>Ophalen en op app-niveau referenties opnieuw instellen
De referenties op app-niveau ophalen:

1. In de [Azure-portal](https://portal.azure.com), in het menu links, klikt u op **App Services** > **&lt;any_app >** > **implementatie Center** > **implementatiereferenties**.

2. Klik op **App referenties**, en klik op de **kopiëren** koppeling om te kopiëren van de gebruikersnaam of wachtwoord.

    ![](./media/app-service-deployment-credentials/deployment_credentials_app_level.png)

Als u de referenties op app-niveau herstellen, klikt u op **opnieuw instellen van referenties** in het dialoogvenster voor dezelfde.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u gebruikt deze referenties voor uw app implementeren vanuit [lokale Git](deploy-local-git.md) of met behulp van [FTP/S](deploy-ftp.md).
