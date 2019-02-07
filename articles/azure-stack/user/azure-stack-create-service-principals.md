---
title: Maken van een Service-Principal voor Azure Stack | Microsoft Docs
description: Beschrijft hoe u een service-principal die kan worden gebruikt met de op rollen gebaseerd toegangsbeheer in Azure Resource Manager voor het beheren van toegang tot bronnen te maken.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 12/12/2018
ms.openlocfilehash: da75d829ccb74f7be675501b601ad3e6375bfb70
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813157"
---
# <a name="give-applications-access-to-azure-stack-resources-by-creating-service-principals"></a>Toepassingen toegang geven tot Azure Stack-bronnen met het maken van service-principals

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt een toepassing toegang geven tot Azure Stack bronnen door een service-principal die gebruikmaakt van Azure Resource Manager maken. Een service-principal kunt u de specifieke machtigingen delegeren met behulp van [op rollen gebaseerd toegangsbeheer](azure-stack-manage-permissions.md).

Als een best practice, moet u service-principals voor uw toepassingen. Service-principals zijn het geschiktst is voor het uitvoeren van een app met uw eigen referenties voor de volgende redenen:

* U kunt machtigingen toewijzen aan de service-principal die anders is dan de accountmachtigingen van uw eigen zijn. Een service-principal machtigingen zijn meestal beperkt tot precies wat de app moet doen.
* U hoeft niet te wijzigen van de referenties van de app als uw rol of verantwoordelijkheden wijzigt.
* U kunt een certificaat gebruiken voor het automatiseren van verificatie bij het uitvoeren van een script zonder toezicht.

## <a name="example-scenario"></a>Voorbeeldscenario

U hebt een app voor configuratie waarmee u moet het inventariseren van Azure-resources met Azure Resource Manager. U kunt een service-principal maken en deze toewijzen aan de rol van lezer. Deze rol biedt de app alleen-lezen toegang tot Azure-resources.

## <a name="getting-started"></a>Aan de slag

Gebruik de stappen in dit artikel als richtlijn voor:

* Maak een service-principal voor uw app.
* Uw app registreren en een verificatiesleutel te maken.
* Uw app toewijzen aan een rol.

De manier waarop u Active Directory hebt geconfigureerd voor Azure Stack wordt bepaald hoe u een service-principal maken. Kies een van de volgende opties:

* Een service-principal voor maken [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad).
* Een service-principal voor maken [Active Directory Federation Services (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

De stappen voor het toewijzen van een service-principal aan een rol hetzelfde voor Azure AD en AD FS. Nadat u de service-principal gemaakt, kunt u [overdragen van machtigingen](azure-stack-create-service-principals.md#assign-role-to-service-principal) aan een rol toe te wijzen.

## <a name="create-service-principal-for-azure-ad"></a>Service-principal maken voor Azure AD

Als uw Azure Stack Azure AD als het identiteitsarchief gebruikt, kunt u een service-principal met behulp van dezelfde stappen als voor Azure met behulp van de Azure-portal maken.

>[!NOTE]
Controleer dat u hebt de [vereist Azure AD-machtigingen](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) voordat u begint met het maken van een service-principal.

### <a name="create-service-principal"></a>Een service-principal maken

Maken van een service-principal voor uw toepassing:

1. Meld u aan met uw Azure-Account via de [Azure-portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **App-registraties** > **toevoegen**.
3. Geef een naam en URL op voor de toepassing. Selecteer een **Web-app / API** of **systeemeigen** voor het type van de toepassing die u wilt maken. Na het instellen van de waarden, selecteer **maken**.

### <a name="get-credentials"></a>Referenties ophalen

Wanneer u zich programmatisch aanmeldt, gebruikt u de ID voor uw toepassing en een verificatiesleutel nodig. Deze waarden ophalen:

1. Van **App-registraties** in Active Directory, selecteer uw toepassing.

2. Kopieer de **Toepassings-id** en sla deze op in uw toepassingscode. De toepassingen in de voorbeeldtoepassingen **client-id** met betrekking tot de **toepassings-ID**.

     ![Toepassings-ID voor de toepassing](./media/azure-stack-create-service-principal/image12.png)
3. Selecteer **Sleutels** om een verificatiesleutel te genereren.

4. Geef een beschrijving op van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

>[!IMPORTANT]
Nadat u de sleutel opgeslagen **waarde** wordt weergegeven. Noteer deze waarde omdat u de sleutel later niet ophalen. Bewaar de sleutelwaarde op een locatie waar de toepassing deze kan ophalen.

![Waarschuwing: sleutelwaarde voor opgeslagen sleutel.](./media/azure-stack-create-service-principal/image15.png)

De laatste stap is [toewijzen van uw toepassing een rol](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Service-principal maken voor AD FS

Als u Azure Stack met AD FS als de identiteitsarchief hebt geïmplementeerd, kunt u PowerShell gebruiken voor de volgende taken:

* Een service-principal maken.
* Service-principal toewijzen aan een rol.
* Meld u met behulp van de service-principal-id.

Zie voor meer informatie over het maken van de service-principal [service-principal maken voor AD FS](../azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

## <a name="assign-the-service-principal-to-a-role"></a>De service-principal toewijzen aan een rol

Voor toegang tot resources in uw abonnement, moet u de toepassing aan een rol toewijzen. Bepaal welke rol staat voor de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](../../role-based-access-control/built-in-roles.md).

>[!NOTE]
U kunt van een rol bereik instellen op het niveau van een abonnement, een resourcegroep of een resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Een app met de rol van lezer voor een resourcegroep betekent bijvoorbeeld dat de app een van de resources in de resourcegroep kan lezen.

Gebruik de volgende stappen uit als richtlijn voor een rol toewijzen aan een service-principal.

1. Ga naar het niveau van het bereik dat u wilt toewijzen van de toepassing in de Azure Stack-portal. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **abonnementen**.

2. Selecteer het abonnement de toepassing toewijzen aan. In dit voorbeeld is het abonnement op Visual Studio Enterprise.

     ![Visual Studio Enterprise-abonnement selecteren voor toewijzing](./media/azure-stack-create-service-principal/image16.png)

3. Selecteer **Access Control (IAM)** voor het abonnement.

4. Selecteer **roltoewijzing toevoegen**.

5. Selecteer de rol die u wilt toewijzen aan de toepassing.

6. Uw toepassing zoekt en selecteert u deze.

7. Selecteer **OK** voltooien van de rol toe te wijzen. Hier ziet u uw toepassing in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

Nu dat u hebt gemaakt van een service-principal en een rol is toegewezen, kan uw toepassing toegang krijgen tot bronnen van de Azure Stack.

## <a name="next-steps"></a>Volgende stappen

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
