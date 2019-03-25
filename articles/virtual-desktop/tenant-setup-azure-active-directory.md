---
title: Een tenant maken in Windows Virtual Desktop Preview - Azure
description: Beschrijft hoe u voor het instellen van Windows Virtual Desktop Preview tenants in Azure Active Directory.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: b8b5c2cef1db5018ce0d61e1950f49a3bd215ac2
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402895"
---
# <a name="tutorial-create-a-tenant-in-windows-virtual-desktop-preview"></a>Zelfstudie: Een tenant maken in Windows Virtual Desktop Preview

Het maken van een tenant in Windows Virtual Desktop Preview is de eerste stap voor het bouwen van uw bureaublad-virtualisatie-oplossing. Een tenant is een groep van een of meer groepen van de host. Elke host-pool bestaat uit meerdere sessie hosts, die wordt uitgevoerd als virtuele machines in Azure en geregistreerd bij de virtuele Windows-bureaublad-service. Elke host-pool bestaat ook uit een of meer app-groepen die worden gebruikt voor extern bureaublad en externe toepassingsresources publiceren voor gebruikers. Met een tenant, kunt u host-pools bouwen, app-groepen maken, gebruikers toe te wijzen en verbindingen via de service maken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Verlenen Azure Active Directory-machtigingen voor de virtuele Windows-bureaublad-service.
> * De rol van de toepassing TenantCreator toewijzen aan een gebruiker in uw Azure Active Directory-tenant.
> * Een virtuele Windows-bureaublad-tenant maken.

Dit is wat u nodig hebt voor het instellen van uw virtuele Windows-bureaublad-tenant:

* De [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) tenant-ID voor Windows virtueel bureaublad-gebruikers.
* Een globale beheerdersaccount in de Azure Active Directory-tenant.
   * Dit geldt ook voor Cloud Solution Provider (CSP) organisaties is het maken van een tenant virtueel bureaublad van Windows voor hun klanten. Als u een CSP-organisatie, moet u aanmelden als globale beheerder van de klant Azure Active Directory zijn.
* Een Azure-abonnement-ID

## <a name="grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-preview-service"></a>Machtigingen verlenen Azure Active Directory naar de service Windows Virtual Desktop Preview

Als u al machtigingen hebt verleend aan virtuele Windows-bureaublad voor deze Azure Active Directory, moet u deze sectie overslaan.

Machtigingen verlenen aan de virtuele Windows-bureaublad-service kunt deze query uitvoeren op uw Azure Active Directory voor administratieve en taken voor eindgebruikers.

De om servicemachtigingen te verlenen:

1. Open een browser en maak verbinding met de [virtuele Windows-bureaublad instemmingspagina](https://rdweb.wvd.microsoft.com).
2. Voor **toestemming geven optie** > **App Server**, voer de naam van de Azure Active Directory-tenant of de map-ID en vervolgens **indienen**.
        Voor Cloud Solution Provider-klanten is de ID van de klant Microsoft-ID van de Partner-Portal. Voor zakelijke klanten, de ID bevindt zich onder **Azure Active Directory** > **eigenschappen** > **map-ID**.
3. Meld u aan de pagina van de toestemming virtuele Windows-bureaublad met een globale beheerdersaccount. Bijvoorbeeld, als u met de Contoso-organisatie, uw account mogelijk admin@contoso.com of admin@contoso.onmicrosoft.com.  
4. Selecteer **Accepteren**.
5. Één minuut wacht.
6. Ga terug naar de [virtuele Windows-bureaublad instemmingspagina](https://rdweb.wvd.microsoft.com).
7. Ga naar **toestemming geven optie** > **Client-App**, voer de naam van dezelfde Azure AD-tenant of de map-ID en vervolgens **indienen**.
8. Meld u aan de virtuele Windows-bureaublad toestemming pagina als globale beheerder zoals u in stap 3 hebt gedaan.
9. Selecteer **Accepteren**.

## <a name="assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant"></a>De toepassingsrol TenantCreator toewijzen aan een gebruiker in uw Azure Active Directory-tenant

Toewijzen van een Azure Active Directory-gebruiker kan de toepassingsrol TenantCreator die gebruiker te maken van een virtuele Windows-bureaublad-tenant die is gekoppeld aan de Azure Active Directory. U moet uw account globale beheerder gebruiken voor de rol TenantCreator toe te wijzen.

De toepassingsrol TenantCreator met uw globale beheerdersaccount toewijzen:

1. Open een browser en maak verbinding met de [Azure Active Directory-portal](https://aad.portal.azure.com) met uw globale beheerdersaccount.
   - Als u met meerdere Azure AD-tenants werkt, is het aanbevolen procedure opent u een persoonlijke browsersessie en kopiëren en plakken van URL's in het adres.
2. Selecteer **bedrijfstoepassingen**, zoeken naar **virtuele Windows-bureaublad**. Hier ziet u de twee toepassingen die u hebt toestemming voor het in de vorige sectie hebt opgegeven. Selecteren van deze twee apps **virtuele Windows-bureaublad**.
3. Selecteer **gebruikers en groepen**en selecteer vervolgens **gebruiker toevoegen**.
4. Selecteer gebruikers en groepen in de **toevoegen toewijzing** blade
5. Zoeken naar een gebruikersaccount dat uw virtuele Windows-bureaublad-tenant maakt.
   - Dit kan het account globale beheerder zijn voor het gemak.
6. Selecteer het gebruikersaccount, klik op de **Selecteer** knop en selecteer vervolgens **toewijzen**.

## <a name="create-a-windows-virtual-desktop-preview-tenant"></a>Een Windows Virtual Desktop Preview-tenant maken

Nu dat u hebt de machtigingen van de virtuele Windows-bureaublad-service om op te vragen van de Azure Active Directory verleend en de TenantCreator-rol is toegewezen aan een gebruikersaccount, kunt u een virtuele Windows-bureaublad-tenant maken.

Eerste, [downloaden en importeren van de virtuele Windows-bureaublad-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Meld u aan virtuele Windows-bureaublad met behulp van het gebruikersaccount TenantCreator met deze cmdlet:

```powershell
Add-RdsAccount -DeploymentUrl “https://rdbroker.wvd.microsoft.com”
```

Maak daarna een nieuwe virtuele Windows-bureaublad-tenant die is gekoppeld aan de Azure Active Directory-tenant:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

De waarden tussen haakjes moeten worden vervangen door waarden die relevant zijn voor uw organisatie en de tenant. Stel dat u de Windows virtuele bureaublad TenantCreator voor de Contoso-organisatie. De cmdlet die u het volgende uit zou er als volgt:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

## <a name="next-steps"></a>Volgende stappen

Als u uw tenant hebt gemaakt, moet u een host-pool maken. Voor meer informatie over pools host, verder met de zelfstudie voor het maken van een host van toepassingen in virtuele Windows-bureaublad.

> [!div class="nextstepaction"]
> [Zelfstudie voor virtuele Windows-bureaublad host pool](./create-host-pools-azure-marketplace.md)
