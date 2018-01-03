---
title: Beheerde Service-identiteit (MSI) voor Azure Active Directory
description: Een overzicht van beheerde Service-identiteit voor Azure-resources.
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ms.reviewer: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 53577c8da5f82235284d1cb9e48f2d47254aa6bd
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
#  <a name="managed-service-identity-msi-for-azure-resources"></a>Managed Service-identiteit (MSI) voor Azure-resources

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Een algemene uitdaging bij het bouwen van cloud-toepassingen is het beheren van de referenties die moeten worden in uw code voor verificatie bij de cloud-services. Deze referenties veilig te houden, is een belangrijke taak. In het ideale geval ze nooit worden weergegeven op ontwikkelaars werkstations of ophalen ingecheckt in broncodebeheer. Azure Sleutelkluis biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Sleutelkluis om op te halen ze. Beheerde Service-identiteit (MSI) maakt het oplossen van dit probleem eenvoudiger door middel van een identiteit automatisch beheerde Azure-services in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken om alle services die ondersteuning biedt voor Azure AD-verificatie, met inbegrip van de Sleutelkluis, zonder dat u geen referenties hoeft in uw code te verifiëren.

## <a name="how-does-it-work"></a>Hoe werkt het?

Wanneer u een beheerde Service-identiteit op een Azure-service-exemplaar, wordt een identiteit in Azure gemaakt in de Azure AD-tenant die wordt gebruikt door uw Azure-abonnement. Daarnaast levert Azure de referenties voor de identiteit, naar het service-exemplaar. Als gevolg hiervan kan een lokale aanvraag toegangstokens ophalen voor services die ondersteuning bieden voor Azure AD-verificatie vervolgens maken door uw code. Terwijl Azure zorgt voor het implementeren van de referenties die worden gebruikt door het service-exemplaar.

## <a name="how-do-i-enable-my-resources-to-use-a-managed-service-identity"></a>Hoe kan ik mijn bronnen voor het gebruik van een beheerde Service-identiteit inschakelen?

Er zijn twee soorten beheerde Service-identiteiten beschikbaar: *door het systeem toegewezen* en *gebruiker toegewezen*.

- Een **door het systeem toegewezen** MSI rechtstreeks op een Azure-service-exemplaar is ingeschakeld. Door het proces inschakelen Azure wordt een identiteit voor het service-exemplaar gemaakt in de Azure AD-tenant en de bepalingen van de referenties voor de identiteit op het service-exemplaar. De levenscyclus van een systeem toegewezen MSI rechtstreeks is gekoppeld aan de Azure service-exemplaar ingeschakeld op. Als het service-exemplaar wordt verwijderd, ruimt Azure automatisch de referenties en de identiteit in Azure AD.

- Een **gebruiker toegewezen** MSI *(afgeschermd voorbeeld)* als zelfstandige Azure-resource is gemaakt. Bij het maken maakt Azure een identiteit in de Azure AD-tenant. Nadat de identiteit is gemaakt, kan deze worden toegewezen aan een of meer exemplaren van Azure-service. Omdat een gebruiker toegewezen MSI kan worden gebruikt door meerdere exemplaren van de Azure-service, wordt de levenscyclus afzonderlijk beheerd.

Hier volgt een voorbeeld van hoe een MSI systeem toegewezen met Azure Virtual Machines werkt.

![Voorbeeld van de MSI van de virtuele Machine](~/articles/active-directory/media/msi-vm-example.png)

1. Azure Resource Manager ontvangt een bericht van het systeem toegewezen MSI op een virtuele machine inschakelen.
2. Azure Resource Manager maakt een Service-Principal in Azure AD om weer te geven van de identiteit van de virtuele machine. De Service-Principal gemaakt in de Azure AD-tenant die wordt vertrouwd door dit abonnement.
3. Azure Resource Manager configureert de details van de Service-Principal in de VM-extensie van de MSI van de virtuele machine. Deze stap omvat het configureren van client-ID en certificaat dat wordt gebruikt door de uitbreiding toegangstokens ophalen uit Azure AD.
4. Nu dat de identiteit van de virtuele machine van de Service-Principal bekend is, is het kan dat deze toegang tot Azure-resources worden verleend. Bijvoorbeeld, als uw code aan te roepen Azure Resource Manager, wilt u toewijzen van de VM-Service-Principal de juiste rol met behulp van op rollen gebaseerde toegangsbeheer (RBAC) in Azure AD.  Als uw code aan te roepen Sleutelkluis, kunt u uw code toegang tot het specifiek geheim of de sleutel in de Sleutelkluis wilt verlenen.
5. Uw code die wordt uitgevoerd op de virtuele machine vraagt een token van een lokaal eindpunt dat wordt gehost door de MSI-VM-extensie: oauth2-http://localhost:50342-token. De resourceparameter geeft u de service die het token wordt verzonden. Bijvoorbeeld, als u wilt dat uw code te verifiëren voor Azure Resource Manager, gebruikt u resource = https://management.azure.com/.
6. De VM-extensie MSI maakt gebruik van de geconfigureerde client-ID en certificaat voor het aanvragen van een toegangstoken van Azure AD.  Azure AD retourneert een toegangstoken JSON Web Token (JWT).
7. Uw code verzendt het toegangstoken op een aanroep van een service die Azure AD-verificatie ondersteunt.

Met behulp van dezelfde van het diagram, hier een voorbeeld van hoe een gebruiker toegewezen MSI met Azure Virtual Machines werkt.

![Voorbeeld van de MSI van de virtuele Machine](~/articles/active-directory/media/msi-vm-example.png)

1. Azure Resource Manager ontvangt een bericht voor het maken van een MSI gebruiker toegewezen.
2. Azure Resource Manager maakt een Service-Principal in Azure AD om weer te geven van de identiteit van het MSI-bestand. De Service-Principal gemaakt in de Azure AD-tenant die wordt vertrouwd door dit abonnement.
3. Azure Resource Manager ontvangt een bericht voor het configureren van de details van de Service-Principal in de VM-extensie van de MSI van een virtuele machine. Deze stap omvat het configureren van client-ID en certificaat dat wordt gebruikt door de uitbreiding toegangstokens ophalen uit Azure AD.
4. Nu dat de identiteit van de MSI van de Service-Principal bekend is, is het kan dat deze toegang tot Azure-resources worden verleend. Bijvoorbeeld, als uw code aan te roepen Azure Resource Manager, wilt u toewijzen het MSI Service-Principal de juiste rol met behulp van op rollen gebaseerde toegangsbeheer (RBAC) in Azure AD. Als uw code aan te roepen Sleutelkluis, kunt u uw code toegang tot het specifiek geheim of de sleutel in de Sleutelkluis wilt verlenen. Opmerking: De stap 3 is niet vereist als u stap 4. Zodra een MSI-bestand bestaat, kan worden deze toegang tot netwerkbronnen, ongeacht het op een virtuele machine wordt geconfigureerd of niet verleend.
5. Uw code die wordt uitgevoerd op de virtuele machine vraagt een token van een lokaal eindpunt dat wordt gehost door de MSI-VM-extensie: oauth2-http://localhost:50342-token. De client-ID-parameter bevat de naam van de MSI-identiteit moet worden gebruikt. De resourceparameter bevat bovendien de service die het token wordt verzonden. Bijvoorbeeld, als u wilt dat uw code te verifiëren voor Azure Resource Manager, gebruikt u resource = https://management.azure.com/.
6. De VM-extensie MSI controleert het certificaat voor de aangevraagde client-ID is geconfigureerd als een toegangstoken van Azure AD-aanvragen. Azure AD retourneert een toegangstoken JSON Web Token (JWT).
7. Uw code verzendt het toegangstoken op een aanroep van een service die Azure AD-verificatie ondersteunt.

Elke Azure-service die ondersteuning biedt voor Service-identiteit beheerd heeft een eigen methode voor uw code verkrijgen van een toegangstoken. Bekijk de zelfstudies voor elke service om erachter te komen de specifieke methode voor het ophalen van een token.

## <a name="try-managed-service-identity"></a>Probeer beheerde Service-identiteit

Probeer een Service-identiteit beheerd-zelfstudie voor meer informatie over de end-to-end-scenario's voor toegang tot verschillende Azure-resources:
<br><br>
| MSI-functionaliteit-bron | Procedures voor |
| ------- | -------- |
| Azure virtuele machine (Linux)   | [Toegang tot Azure Resource Manager met een virtuele Linux-machine beheerde Service-identiteit](msi-tutorial-linux-vm-access-arm.md) |
|                    | [Toegang tot Azure Storage via toegangssleutel met een Linux VM beheerde Service-identiteit](msi-tutorial-linux-vm-access-storage.md) |

## <a name="which-azure-services-support-managed-service-identity"></a>Welke Azure-services ondersteuning bieden voor Service-identiteiten beheerd?

Azure-services die ondersteuning bieden voor Service-identiteit beheerd kunnen MSI te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie gebruiken.  We zijn bezig het MSI- en Azure AD authentication integreren in Azure.  Controleer regelmatig of er updates.

### <a name="azure-services-that-support-managed-service-identity"></a>Azure-services die ondersteuning bieden voor Service-identiteit beheerd

De volgende Azure-services ondersteuning bieden voor Service-identiteiten beheerd.

| Service | Status | Date | Configureren | Een token ophalen |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Virtual Machines | Preview | September 2017 | [Azure-CLI](msi-qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager-sjablonen](msi-qs-configure-template-windows-vm.md) | [Bash/Curl](msi-how-to-use-vm-msi-token.md#get-a-token-using-curl)<br>[HTTP/REST](msi-how-to-use-vm-msi-token.md#get-a-token-using-http) |

### <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-services die ondersteuning voor Azure AD-verificatie

De volgende services ondersteuning bieden voor Azure AD-verificatie en getest met clientservices die gebruikmaken van beheerde Service-identiteit.

| Service | Resource-id | Status | Date | Toegang toewijzen |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://Management.Azure.com/ | Beschikbaar | September 2017 | [Azure-CLI](msi-howto-assign-access-CLI.md) |
| Azure Key Vault | https://Vault.Azure.NET/ | Beschikbaar | September 2017 | |
| Azure Data Lake | https://datalake.Azure.NET/ | Beschikbaar | September 2017 | |
| Azure SQL | https://database.Windows.NET/ | Beschikbaar | Oktober 2017 | |

## <a name="how-much-does-managed-service-identity-cost"></a>Wat kost Service-identiteit beheerd?

Beheerde Service-identiteit die wordt geleverd met Azure Active Directory vrij, dit is de standaardoptie voor Azure-abonnementen.  Er is geen extra kosten voor Service-identiteit beheerd.

## <a name="support-and-feedback"></a>Ondersteuning en feedback

We graag horen van u!

* Hoe kan ik vragen stellen op de Stack Overflow met het label [azure msi](http://stackoverflow.com/questions/tagged/azure-msi).
* Functie aanvragen of feedback geven over de [forum met feedback van Azure AD voor ontwikkelaars](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences).






