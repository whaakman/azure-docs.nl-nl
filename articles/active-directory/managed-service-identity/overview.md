---
title: Managed Service Identity voor Azure-resources
description: Een overzicht van de functie Managed Service Identity voor Azure-resources.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 03/28/2018
ms.author: daveba
ms.openlocfilehash: 1edd6f846d539b1f263877a5e0af107148513c6e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398382"
---
# <a name="what-is-managed-service-identity-for-azure-resources"></a>Wat is Managed Service Identity voor Azure-resources?

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Een veelvoorkomende uitdaging bij het bouwen van cloud-apps is het beheren van de referenties in uw code voor verificatie bij cloudservices. Het is belangrijk dat de referenties veilig worden bewaard. In het ideale geval worden de referenties nooit weergegeven op werkstations van ontwikkelaars en niet ingecheckt in broncodebeheer. Azure Key Vault biedt een manier voor het veilig opslaan van referenties, geheimen en andere sleutels, maar uw code moet worden geverifieerd bij Key Vault om ze op te halen. 

De functie Managed Service Identity in Azure Active Directory (Azure AD) lost dit probleem op. De functie biedt Azure-services met een automatisch beheerde identiteit in Azure AD. U kunt de identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder referenties in de code.

De functie Managed Service Identity is voor Azure-abonnees gratis bij Azure AD. Er zijn geen extra kosten.

## Hoe werkt de functie?<a name="how-does-it-work"></a>

Er zijn twee typen beheerde identiteit:

- Een **door het systeem toegewezen identiteit** wordt rechtstreeks op een Azure-service-exemplaar ingeschakeld. Wanneer de identiteit is ingeschakeld, wordt een identiteit voor het service-exemplaar in de Azure AD-tenant gemaakt, dat wordt vertrouwd door het abonnement van het service-exemplaar. Nadat de identiteit is gemaakt, worden de referenties op het exemplaar ingericht. De levenscyclus van een door het systeem toegewezen identiteit is rechtstreeks gekoppeld aan het Azure-service-exemplaar waarop de identiteit is ingeschakeld. Als het exemplaar wordt verwijderd, ruimt Azure automatisch de referenties en de identiteit in Azure AD op.
- Een **door de gebruiker toegewezen identiteit** wordt als een zelfstandige Azure-resource gemaakt. Via een productieproces maakt Azure een identiteit in de Azure AD-tenant, die wordt vertrouwd door het gebruikte abonnement. Nadat de identiteit is gemaakt, kan deze worden toegewezen aan een of meer Azure-service-exemplaren. De levenscyclus van een door de gebruiker toegewezen identiteit wordt afzonderlijk beheerd van de levenscyclus van de Azure Service-exemplaren waaraan de identiteit is toegewezen.

De code kan gebruikmaken van een beheerde service-identiteit om toegangstokens aan te vragen voor services die ondersteuning bieden voor Azure AD-verificatie. Azure zorgt voor het implementeren van de referenties die worden gebruikt door het service-exemplaar.

In het volgende diagram ziet u hoe beheerde service-identiteiten samenwerken met virtuele machines (VM's) van Azure:

![Beheerde service-identiteiten en Azure-VM's](media/overview/msi-vm-vmextension-imds-example.png)

### <a name="how-a-system-assigned-identity-works-with-an-azure-vm"></a>Hoe een door het systeem toegewezen identiteit samenwerkt met een Azure-VM

1. Azure Resource Manager ontvangt een aanvraag voor het inschakelen van de door het systeem toegewezen identiteit op een VM.
2. Azure Resource Manager maakt een service-principal in Azure AD voor de identiteit van de VM. De service-principal wordt gemaakt in de Azure AD-tenant die wordt vertrouwd door het abonnement.
3. Azure Resource Manager configureert de identiteit op de VM:
    1. Het Azure Instance Metadata Service-eindpunt voor identiteit wordt bijgewerkt met de client-id en het certificaat van de service-principal.
    1. De VM-extensie wordt ingericht en de client-id en het certificaat van de service-principal wordt toegevoegd. (Deze stap wordt later afgeschaft.)
4. Als de VM een identiteit heeft, gebruikt u de informatie van de service-principal om de VM toegang te verlenen tot Azure-resources. Voor het aanroepen van Azure Resource Manager gebruikt u op rollen gebaseerd toegangsbeheer (RBAC) in Azure AD om de juiste rol aan de VM-service-principal toe te wijzen. Als u Key Vault wilt aanroepen, geeft u de code toegang tot het specifieke geheim of de specifieke sleutel in Key Vault.
5. De code die wordt uitgevoerd op de VM, kan een token aanvragen vanaf twee eindpunten die alleen toegankelijk zijn vanuit de VM:

    - Azure Instance Metadata Service-eindpunt voor identiteit (aanbevolen): `http://169.254.169.254/metadata/identity/oauth2/token`
        - De resourceparameter geeft de service op waarnaar het token wordt verzonden. Gebruik `resource=https://management.azure.com/` om bij Azure Resource Manager te verificatie uit te voeren.
        - De API-versieparameter specificeert de IMDS-versie, gebruik api-version=2018-02-01 of hoger.
    - VM-extensie-eindpunt (wordt later afgeschaft): `http://localhost:50342/oauth2/token` 
        - De resourceparameter geeft de service op waarnaar het token wordt verzonden. Gebruik `resource=https://management.azure.com/` om bij Azure Resource Manager te verificatie uit te voeren.

6. Er wordt een aanroep uitgevoerd naar Azure AD om een toegangstoken aan te vragen (zoals beschreven in stap 5) met behulp van de client-id en het certificaat dat in stap 3 is geconfigureerd. Azure AD retourneert een JWT-toegangstoken (JSON Web Token).
7. Uw code verzendt het toegangstoken bij een aanroep naar een service die Azure AD-verificatie ondersteunt.

### <a name="how-a-user-assigned-identity-works-with-an-azure-vm"></a>Hoe een door een gebruiker toegewezen identiteit samenwerkt met een Azure-VM

1. Azure Resource Manager ontvangt een aanvraag voor het maken van een door de gebruiker toegewezen identiteit.
2. Azure Resource Manager maakt een service-principal in Azure AD voor de door de gebruiker toegewezen identiteit. De service-principal wordt gemaakt in de Azure AD-tenant die wordt vertrouwd door het abonnement.
3. Azure Resource Manager ontvangt een aanvraag voor het configureren van de door de gebruiker toegewezen identiteit op een VM.
    1. Het Azure Instance Metadata Service-eindpunt voor identiteit wordt bijgewerkt met de client-id en het certificaat van de service-principal voor de door de gebruiker toegewezen identiteit.
    1. De VM-extensie wordt ingericht en de client-id en het certificaat van de service-principal voor de door de gebruiker toegewezen identiteit worden toegevoegd. (Deze stap wordt later afgeschaft.)
4. Nadat de door de gebruiker toegewezen identiteit is gemaakt, gebruikt u de informatie van de service-principal om de identiteit toegang te verlenen tot Azure-resources. Voor het aanroepen van Azure Resource Manager gebruikt u RBAC in Azure AD om de juiste rol aan de service-principal van de door de gebruiker toegewezen identiteit toe te wijzen. Als u Key Vault wilt aanroepen, geeft u de code toegang tot het specifieke geheim of de specifieke sleutel in Key Vault.

   > [!Note]
   > U kunt deze stap ook vóór stap 3 uitvoeren.

5. De code die wordt uitgevoerd op de VM, kan een token aanvragen vanaf twee eindpunten die alleen toegankelijk zijn vanuit de VM:

    - Azure Instance Metadata Service-eindpunt voor identiteit (aanbevolen): `http://169.254.169.254/metadata/identity/oauth2/token`
        - De resourceparameter geeft de service op waarnaar het token wordt verzonden. Gebruik `resource=https://management.azure.com/` om bij Azure Resource Manager te verificatie uit te voeren.
        - De client-id-parameter bevat de identiteit waarvoor het token wordt aangevraagd. Deze waarde is vereist om ambiguïteit op te heffen wanneer meer dan een door de gebruiker toegewezen identiteit aanwezig is op één VM.
        - De parameter voor de API-versie geeft de versie van Azure Instance Metadata Service op. Gebruik `api-version=2018-02-01` of hoger.

    - VM-extensie-eindpunt (wordt later afgeschaft): `http://localhost:50342/oauth2/token`
        - De resourceparameter geeft de service op waarnaar het token wordt verzonden. Gebruik `resource=https://management.azure.com/` om bij Azure Resource Manager te verificatie uit te voeren.
        - De client-id-parameter bevat de identiteit waarvoor het token wordt aangevraagd. Deze waarde is vereist om ambiguïteit op te heffen wanneer meer dan een door de gebruiker toegewezen identiteit aanwezig is op één VM.
6. Er wordt een aanroep uitgevoerd naar Azure AD om een toegangstoken aan te vragen (zoals beschreven in stap 5) met behulp van de client-id en het certificaat dat in stap 3 is geconfigureerd. Azure AD retourneert een JWT-toegangstoken (JSON Web Token).
7. Uw code verzendt het toegangstoken bij een aanroep naar een service die Azure AD-verificatie ondersteunt.

## <a name="how-can-i-use-managed-service-identities"></a>Hoe kan ik gebruikmaken van beheerde service-identiteiten?

In deze zelfstudies leert u hoe u gebruik kunt maken van beheerde service-identiteiten om toegang te krijgen tot verschillende Azure-resources.

Informatie over het gebruik van een beheerde service-identiteit met een Windows-VM:

* [Toegang tot Azure Data Lake Store](tutorial-windows-vm-access-datalake.md)
* [Toegang tot Azure Resource Manager](tutorial-windows-vm-access-arm.md)
* [Toegang tot Azure SQL](tutorial-windows-vm-access-sql.md)
* [Toegang tot Azure Storage met behulp van een toegangssleutel](tutorial-windows-vm-access-storage.md)
* [Toegang tot Azure Storage met behulp van handtekeningen voor gedeelde toegang](tutorial-windows-vm-access-storage-sas.md)
* [Toegang tot een niet-Azure-resource met Azure Key Vault](tutorial-windows-vm-access-nonaad.md)

Informatie over het gebruik van een beheerde service-identiteit met een Linux-VM:

* [Toegang tot Azure Data Lake Store](tutorial-linux-vm-access-datalake.md)
* [Toegang tot Azure Resource Manager](tutorial-linux-vm-access-arm.md)
* [Toegang tot Azure Storage met behulp van een toegangssleutel](tutorial-linux-vm-access-storage.md)
* [Toegang tot Azure Storage met behulp van handtekeningen voor gedeelde toegang](tutorial-linux-vm-access-storage-sas.md)
* [Toegang tot een niet-Azure-resource met Azure Key Vault](tutorial-linux-vm-access-nonaad.md)

Informatie over het gebruik van een beheerde service-identiteit met andere Azure-services:

* [Azure App Service](/azure/app-service/app-service-managed-service-identity)
* [Azure Functions](/azure/app-service/app-service-managed-service-identity)
* [Azure Service Bus](../../service-bus-messaging/service-bus-managed-service-identity.md)
* [Azure Event Hubs](../../event-hubs/event-hubs-managed-service-identity.md)
* [Azure API Management](../../api-management/api-management-howto-use-managed-service-identity.md)

## Welke Azure-services bieden ondersteuning voor de functie?<a name="which-azure-services-support-managed-service-identity"></a>

Beheerde service-identiteiten kunnen worden gebruikt voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie. Zie [Services die ondersteuning bieden voor Managed Service Identity](services-support-msi.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met de functie Managed Service Identity aan de hand van de volgende snelstartgidsen:

* [Toegang krijgen tot Resource Manager met een Managed Service Identity voor Windows-VM](tutorial-windows-vm-access-arm.md)
* [Toegang krijgen tot Azure Resource Manager met Managed Service Identity voor een Linux-VM](tutorial-linux-vm-access-arm.md)
