---
title: De validatie van uw Azure Stack als een service-account instellen | Microsoft Docs
description: Meer informatie over het instellen van de validatie uit als een service-account.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: f6c78a3e79ac88194e7e34ad8be7a941ee715d39
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235383"
---
# <a name="set-up-your-validation-as-a-service-account"></a>De validatie uit als een service-account instellen

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validatie als een service (VaaS) is een Azure-service die beschikbaar wordt gesteld aan Microsoft Azure Stack-partners die een collega engineering-overeenkomst met Microsoft voor het ontwerpen, ontwikkelen, valideren, verkopen, implementeren en ondersteunen van Azure Stack-oplossingen in de markt hebben.

Meer informatie over het verkrijgen van het systeem gereed voor validatie als service. Instellen van de Azure Active Directory-exemplaar en andere vereiste taken voor het voorbereiden voor het gebruik van VaaS uitvoeren. 

De taken zijn:

- Maken van een Azure Storage-blob voor het opslaan van Logboeken
- De lokale agent implementeren
- Downloaden van de installatiekopie van virtuele testmachines op de Azure Stack-instantie moet worden getest

## <a name="create-an-azure-active-directory-tenant-id"></a>Maak een Azure Active Directory-tenant-ID

1. Maken van een Azure Active Directory-tenant in de [Azure-portal](https://portal.azure.com) of gebruik een bestaande tenant.

    Het verdient aanbeveling een tenant maken specifiek voor gebruik met VaaS met een beschrijvende naam, zoals ContosoVaaS@onmicrosoft.com. De functies Role-based access control (RBAC) van de tenant wordt gebruikt door de partner om te beheren die in de partnerorganisatie VaaS kunt gebruiken.  
    
    Meer informatie, Zie [beheren van uw Azure AD-directory](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

    > [!Note]  
    > Zie voor meer informatie over het maken van nieuwe Azure Active Directory-tenants [aan de slag met Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad).

2. Toevoegen van leden van uw organisatie die verantwoordelijk is voor het gebruik van de service voor de tenant. Aan elke gebruiker in de tenant een van de volgende rollen voor het beheren van hun toegangsniveau tot VaaS toewijzen:

    | Naam van rol | Beschrijving |
    |---------------------|------------------------------------------|
    | Eigenaar | Heeft volledige toegang tot alle resources. |
    | Lezer | Alle resources bekijken maar niet bewerken. |
    | Test-Inzender | Test-resources kunt beheren. |
    | Catalogus-Inzender | Kan oplossing publiceren bronnen beheren. |

## <a name="set-up-your-tenant"></a>Instellen van uw tenant

Instellen van uw tenant in de **Azure Stack-validatieservice** toepassing. 

1. De volgende informatie over de tenant verzenden naar Microsoft op vaashelp@microsoft.com.

    | Gegevens | Beschrijving |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Naam van organisatie | De naam van de officiële organisatie. |
    | De naam van de map Azure AD-Tenant | Naam van Azure AD-Tenant-Directory worden geregistreerd. |
    | Azure AD-Tenant map-Id | Azure AD-Tenant-Directory GUID die is gekoppeld aan de directory.<br> Voor meer informatie over het vinden van uw Azure AD-Tenant-ID van de Directory kunnen vinden, Zie '[tenant-ID ophalen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). " |

    

2. De Azure Stack-team verzorgt bevestiging dat uw tenant de portal VaaS kunt gebruiken.

3. Gebruik de referenties van de globale beheerder voor de tenant aan te melden bij de [VaaS portal](https://azurestackvalidation.com/
). Selecteer **Mijn Account**.

    ![Meld u aan bij de portal VaaS](media/vaas_portalsignin.png)

3. De site wordt gevraagd of u om toegang te verlenen voor VaaS. Accepteer de voorwaarden om door te gaan.

## <a name="assign-user-roles"></a>Gebruikersrollen toewijzen

Een gebruikersrol toewijzen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services** > **Azure Active Directory** in de **identiteit** groep.
3. Selecteer **bedrijfstoepassingen** > **Azure Stack-validatieservice** toepassing.
4. Selecteer **Gebruikers en groepen**. De **Azure Stack-validatieservice - gebruikers en groepen** blade geeft een lijst van de gebruikers met een machtiging voor het gebruik van de toepassing.
5. Selecteer **+ gebruiker toevoegen** om toe te voegen een-toewijzing.

## <a name="create-an-azure-storage-blob-to-store-logs"></a>Maken van een Azure storage-blob voor het opslaan van Logboeken

VaaS maakt logboeken met diagnostische gegevens bij het uitvoeren van de validatietests. U moet een locatie een SAS-URL voor het opslaan van uw logboeken van de Azure blob-service. Het storage-account kan ook worden gebruikt om de pakketten store de OEM-aanpassing.

Deze stappen helpen bij het instellen en genereren van een storage-as-a-service (SAS)-URI voor een Azure storage-account, en waar u de storage-account in de portal VaaS opgeven bij het starten van tests in de portal.

### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

1. Voor het maken van een storage-account, volgt u de instructies [een opslagaccount maken](https://docs.microsoft.com/azure/storage/storage-create-storage-account#create-a-storage-account).

2. Bij het selecteren van het type opslagaccount, selecteer de **Blob storage** accounttype.

### <a name="generate-a-sas-url-for-the-storage-account"></a>Genereren van een SAS-URL voor de storage-account

1. Ga naar het storage-account dat eerder is gemaakt.

2. Op de blade onder **instellingen**, selecteer **handtekening voor gedeelde toegang**.

3. Alleen controleren **Blob** van **toegestane Services opties** (Schakel de resterende).

4. Controleer **Service**, ** Container en **Object** van **toegestane resourcetypen**.

5. Controleer **lezen**, **schrijven**, **lijst**, **toevoegen**, **maken** van **toegestane machtigingen**  (Schakel de resterende).

6. Stel **begintijd** op de huidige tijd en **eindtijd** tot drie maanden vanaf de huidige tijd.

7. Selecteer **SAS genereren en de verbindingsreeks** en sla de **Blob-service-SAS-URL** tekenreeks.

> [!Note]  
> De SAS-URL is verlopen op de eindtijd instellen wanneer de URL is gegenereerd. Zorg ervoor dat de URL voldoende geldig is voordat u met het productteam voor foutopsporing te delen, of dat de URL geldig is meer dan 30 dagen is bij het plannen van tests.

## <a name="next-steps"></a>Volgende stappen

- De lokale agent VaaS gebruiken om te controleren of uw hardware. Zie voor instructies, [implementeren van de lokale agent en test virtuele machine](azure-stack-vaas-test-vm.md).
- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).