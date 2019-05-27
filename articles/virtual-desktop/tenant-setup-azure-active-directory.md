---
title: Een tenant maken in Windows Virtual Desktop Preview - Azure
description: Beschrijft hoe u voor het instellen van Windows Virtual Desktop Preview tenants in Azure Active Directory.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 2c9682746201306f1b99a04462819618225caa11
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66164250"
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
   * Het administrator-account moet afkomstig zijn van de Azure Active Directory-tenant in die u probeert te maken van de tenant virtuele Windows-bureaublad. Dit proces biedt geen ondersteuning voor Azure Active Directory B2B-accounts (Gast).
   * Het administrator-account moet een account voor werk of school.
* Een Azure-abonnement

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
7. Ga naar **toestemming geven optie** > **Client-App**, voert u dezelfde naam van de Azure Active Directory-tenant of map-ID en selecteert **indienen**.
8. Meld u aan de virtuele Windows-bureaublad toestemming pagina als globale beheerder zoals u in stap 3 hebt gedaan.
9. Selecteer **Accepteren**.

## <a name="assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant"></a>De toepassingsrol TenantCreator toewijzen aan een gebruiker in uw Azure Active Directory-tenant

Toewijzen van een Azure Active Directory-gebruiker kan de toepassingsrol TenantCreator die gebruiker te maken van een virtuele Windows-bureaublad-tenant die is gekoppeld aan de Azure Active Directory. U moet uw account globale beheerder gebruiken voor de rol TenantCreator toe te wijzen.

De toepassingsrol TenantCreator met uw globale beheerdersaccount toewijzen:

1. Open een browser en maak verbinding met de [Azure-portal](https://portal.azure.com) met uw globale beheerdersaccount.
   - Als u met meerdere Azure Active Directory-tenants werkt, is het aanbevolen om te openen van een persoonlijke browsersessie en kopieer en plak de URL's in de adresbalk.
2. Zoek in de zoekbalk in Azure portal, **bedrijfstoepassingen** en selecteer het item dat wordt weergegeven onder de **Services** categorie.
3. Binnen **bedrijfstoepassingen**, zoeken naar **virtuele Windows-bureaublad**. Hier ziet u de twee toepassingen die u hebt toestemming voor het in de vorige sectie hebt opgegeven. Selecteren van deze twee apps **virtuele Windows-bureaublad**.
        ![Een schermafbeelding van de lijst met zoekresultaten bij het zoeken naar 'Windows Virtual Desktop' in 'Enterprise Application'. De app met de naam 'Virtuele Windows-Desktop' is gemarkeerd.](media/tenant-enterprise-app.png)
4. Selecteer **Gebruikers en groepen**. Mogelijk ziet u dat de beheerder die de toepassing toestemming verleend al wordt vermeld met de **standaardtoegang** rol die is toegewezen. Dit is niet voldoende is om een virtuele Windows-bureaublad-tenant te maken. Deze instructies om toe te voegen Doorloop de **TenantCreator** rol aan een gebruiker.
        ![Een schermafbeelding van de gebruikers en groepen die zijn toegewezen voor het beheren van de "Virtuele Windows-bureaublad" enterprise-toepassing. De schermafbeelding ziet u slechts één toewijzing voor 'standaardtoegang'.](media/tenant-default-access.png)
5. Selecteer **gebruiker toevoegen**, klikt u vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** blade.
6. Zoeken naar een gebruikersaccount dat uw virtuele Windows-bureaublad-tenant maakt. Dit kan het account globale beheerder zijn voor het gemak.

    ![Een schermafbeelding van het selecteren van een gebruiker toe te voegen als het 'TenantCreator'.](media/tenant-assign-user.png)

   > [!NOTE]
   > U moet een gebruiker (of een groep met een gebruiker) selecteren die afkomstig is van deze Azure Active Directory. U kunt een gastgebruiker (B2B) of een service-principal niet kiezen.

7. Account van de gebruiker selecteert, selecteert u de **Selecteer** knop en selecteer vervolgens **toewijzen**.
8. Op de **Windows virtueel bureaublad - gebruikers en groepen** pagina, controleert u of er een nieuwe vermelding met de **TenantCreator** rol die is toegewezen aan de gebruiker die de virtuele Windows-bureaublad-tenant maken.
        ![Een schermafbeelding van de gebruikers en groepen die zijn toegewezen voor het beheren van de "Virtuele Windows-bureaublad" enterprise-toepassing. De schermafbeelding bevat nu een tweede vermelding van een gebruiker die is toegewezen aan de rol 'TenantCreator'.](media/tenant-tenant-creator-added.png)

Voordat u doorgaat op om uw virtuele Windows-bureaublad-tenant te maken, moet u twee soorten informatie:
- Uw Azure Active Directory-Tenant-ID (of **map-ID**)
- Uw Azure-abonnement-ID

Uw Azure Active Directory-Tenant-ID vinden (of **map-ID**):
1. Zoek in dezelfde Azure portal-sessie, **Azure Active Directory** in de zoekbalk en selecteer het item dat wordt weergegeven onder de **Services** categorie.
        ![Een schermafbeelding van de lijst met zoekresultaten voor 'Azure Active Directory' in de Azure-portal. Het zoekresultaat onder 'Services' is gemarkeerd.](media/tenant-search-azure-active-directory.png)
2. Schuif omlaag totdat u **eigenschappen**, selecteert u deze.
3. Zoek naar **map-ID**, selecteer vervolgens het pictogram van het Klembord. Plak deze in een een handige locatie zodat u deze kunt later als de **AadTenantId**.
        ![Een schermafbeelding van de Azure Active Directory-eigenschappen. U beweegt de muis boven het pictogram voor de map-ID' kopiëren en plakken van Klembord.](media/tenant-directory-id.png)

Zoek uw Azure-abonnement-ID:
1. Zoek in dezelfde Azure portal-sessie, **abonnementen** in de zoekbalk en selecteer het item dat wordt weergegeven onder de **Services** categorie.
        ![Een schermafbeelding van de lijst met zoekresultaten voor 'Azure Active Directory' in de Azure-portal. Het zoekresultaat onder 'Services' is gemarkeerd.](media/tenant-search-subscription.png)
2. Selecteer het Azure-abonnement dat u wilt gebruiken om virtuele Windows-bureaublad-servicemeldingen te ontvangen.
3. Zoek naar **abonnements-ID**, klikt u vervolgens Beweeg de muisaanwijzer over de waarde tot een pictogram van het Klembord wordt weergegeven. Selecteer het pictogram van het Klembord en plak deze in een handige locatie zodat u deze kunt later als de **AzureSubscriptionId**.
        ![Een schermafbeelding van de eigenschappen van de Azure-abonnement. U beweegt de muis boven het pictogram voor de 'abonnements-ID' kopiëren en plakken van Klembord.](media/tenant-subscription-id.png)

## <a name="create-a-windows-virtual-desktop-preview-tenant"></a>Een Windows Virtual Desktop Preview-tenant maken

Nu dat u hebt de machtigingen van de virtuele Windows-bureaublad-service om op te vragen van de Azure Active Directory verleend en de TenantCreator-rol is toegewezen aan een gebruikersaccount, kunt u een virtuele Windows-bureaublad-tenant maken.

Eerste, [downloaden en importeren van de virtuele Windows-bureaublad-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Meld u aan virtuele Windows-bureaublad met behulp van het gebruikersaccount TenantCreator met deze cmdlet:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Maak daarna een nieuwe virtuele Windows-bureaublad-tenant die is gekoppeld aan de Azure Active Directory-tenant:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

De waarden tussen haakjes moeten worden vervangen door waarden die relevant zijn voor uw organisatie en de tenant. De naam die u voor uw nieuwe virtuele Windows-bureaublad-tenant kiest moet globaal uniek zijn. Stel dat u de Windows virtuele bureaublad TenantCreator voor de Contoso-organisatie. De cmdlet die u het volgende uit zou er als volgt:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

## <a name="next-steps"></a>Volgende stappen

Als u uw tenant hebt gemaakt, moet u een service-principal maken in Azure Active Directory en hieraan een rol binnen de virtuele Windows-bureaublad. De service-principal kunt u voor een succesvolle implementatie Windows virtuele bureaublad Azure Marketplace biedt om een host-pool te maken. Voor meer informatie over pools host, verder met de zelfstudie voor het maken van een host van toepassingen in virtuele Windows-bureaublad.

> [!div class="nextstepaction"]
> [Service-principals en roltoewijzingen maken met PowerShell](./create-service-principal-role-powershell.md)
