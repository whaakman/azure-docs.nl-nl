---
title: Een aangepaste startpagina voor gepubliceerde apps instellen met behulp van Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD Application Proxy connectors
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51596e4db8999de5089748e40f9b24bd46c84e56
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807835"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Een aangepaste startpagina voor gepubliceerde apps instellen met behulp van Azure AD-toepassingsproxy

In dit artikel wordt beschreven hoe u een app configureren zodat een gebruiker naar de startpagina van een aangepaste. Wanneer u een app met Application Proxy publiceert, instellen van een interne URL, maar soms die niet de pagina die een gebruiker moet eerst te zien is. De startpagina van een aangepaste zo instellen dat een gebruiker beschikt over de juiste pagina wanneer ze toegang de app tot. Een gebruiker ziet de startpagina van aangepaste dat u instelt, ongeacht of ze de app vanuit het toegangsvenster Azure Active Directory of het startprogramma voor Office 365 openen.

Wanneer een gebruiker de app start, worden ze omgeleid standaard naar de hoofdmap domein-URL voor de gepubliceerde app. De startpagina is standaard ingesteld als de URL van startpagina. Gebruik de Azure AD PowerShell-module voor het definiëren van een aangepaste startpagina-URL als u wilt dat een app-gebruiker op een specifieke pagina in de app.

Hier volgt een scenario waarin wordt uitgelegd waarom uw bedrijf een aangepaste startpagina wordt ingesteld:

- Binnen uw bedrijfsnetwerk, gaat u een gebruiker naar `https://ExpenseApp/login/login.aspx` aanmelden en toegang tot uw app.
- Omdat er andere assets (zoals afbeeldingen) die Application Proxy moet toegang hebben tot op het hoogste niveau van de mapstructuur, publiceert u de app met `https://ExpenseApp` als de interne URL.
- Is de standaard-URL voor externe `https://ExpenseApp-contoso.msappproxy.net`, die een externe gebruiker naar de aanmeldingspagina niet uitvoeren.
- U wilt instellen `https://ExpenseApp-contoso.msappproxy.net/login/login.aspx` als de URL van de startpagina in plaats daarvan, dus een externe gebruiker ziet de aanmeldingspagina eerst.

> [!NOTE]
> Wanneer u gebruikers toegang tot gepubliceerde apps verleent, de apps worden weergegeven in de [Azure AD-Toegangsvenster](../user-help/my-apps-portal-end-user-access.md) en de [startprogramma voor Office 365](https://www.microsoft.com/microsoft-365/blog/2016/09/27/introducing-the-new-office-365-app-launcher/).

## <a name="before-you-start"></a>Voordat u begint

Voordat u de URL van startpagina, houd rekening met de volgende vereisten:

- Het pad dat u opgeeft moet een pad van het subdomein van de domein-URL voor hoofdmap zijn.

  Bijvoorbeeld, als de URL hoofddomein `https://apps.contoso.com/app1/`, de URL van startpagina die u configureert moet beginnen met `https://apps.contoso.com/app1/`.

- Als u een wijziging in de gepubliceerde app aanbrengt, kan de wijziging opnieuw instellen van de waarde van de URL van startpagina. Wanneer u de app in de toekomst bijwerkt, moet u controleren en, indien nodig werkt u de URL van startpagina.

U kunt de URL van startpagina via Azure portal of met behulp van PowerShell instellen.

## <a name="change-the-home-page-in-the-azure-portal"></a>Wijzigen van de startpagina in de Azure-portal

Als u wilt wijzigen van de URL van de startpagina van uw app via de Azure AD-portal, de volgende stappen uit:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer **Azure Active Directory**, en vervolgens **App-registraties**. De lijst met geregistreerde apps wordt weergegeven.
1. Kies uw app in de lijst. Een pagina met de details van de geregistreerde app wordt weergegeven.
1. Onder **beheren**, selecteer **Branding**.
1. Update de **URL van startpagina** met het nieuwe pad.

   ![Huisstijl van de pagina voor een geregistreerde app met de URL van startpagina-veld](media/application-proxy-configure-custom-home-page/app-proxy-app-branding.png)

1. Selecteer **Opslaan**.

## <a name="change-the-home-page-with-powershell"></a>Wijzigen van de startpagina met PowerShell

Als u wilt configureren op de startpagina van een app met behulp van PowerShell, moet u naar:

1. Installeer de Azure AD PowerShell-module.
1. De object-id-waarde van de app zoeken.
1. Werkt u de URL van de startpagina van de app met behulp van PowerShell-opdrachten.

### <a name="install-the-azure-ad-powershell-module"></a>De Azure AD PowerShell-module installeren

Voordat u de URL van een aangepaste startpagina definiëren met behulp van PowerShell, installeert u de Azure AD PowerShell-module. U kunt het downloaden van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.2.16), waarbij de Graph API-eindpunt wordt gebruikt.

Volg deze stappen voor het installeren van het pakket:

1. Open een standaard PowerShell-venster en voer de volgende opdracht:

   ```powershell
   Install-Module -Name AzureAD
   ```

    Als u de opdracht als een niet-beheerder uitvoert, gebruikt u de `-scope currentuser` optie.

1. Tijdens de installatie, selecteert u **Y** twee pakketten installeren via Nuget.org. Beide pakketten zijn vereist.

### <a name="find-the-objectid-of-the-app"></a>De object-id van de app zoeken

U krijgt de object-id van de app door te zoeken voor de app op de startpagina of weergavenaam.

1. In de dezelfde PowerShell-venster, de Azure AD-module te importeren.

   ```powershell
   Import-Module AzureAD
   ```

1. Meld u aan de module Azure AD als de tenantbeheerder.

   ```powershell
   Connect-AzureAD
   ```

1. De app te zoeken. In dit voorbeeld wordt PowerShell gebruikt voor de object-id vinden door te zoeken voor de app met een weergavenaam op van `SharePoint`.

   ```powershell
   Get-AzureADApplication | Where-Object { $_.DisplayName -eq "SharePoint" } | Format-List DisplayName, Homepage, ObjectId
   ```

   U moet een resultaat die vergelijkbaar is met hieronder. Kopieer de ObjectId GUID die u wilt gebruiken in de volgende sectie.

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

   U kunt ook kan u gewoon ophalen van de lijst met alle apps, zoeken in de lijst voor de app met een specifieke weergavenaam of de startpagina en kopiëren van de app-object-id als de app wordt gevonden.

   ```powershell
   Get-AzureADApplication | Format-List DisplayName, Homepage, ObjectId
   ```

### <a name="update-the-home-page-url"></a>Bijwerken van de URL van startpagina

De URL van startpagina maken en bijwerken van uw app met de waarde. Doorgaan met behulp van de dezelfde PowerShell-venster, of als u een nieuwe PowerShell-venster, aanmelden bij het opnieuw met behulp van Azure AD-module `Connect-AzureAD`. Volg deze stappen:

1. Maak een variabele voor de object-id-waarde die u in de vorige sectie hebt gekopieerd. (Vervang de waarde voor object-id in dit voorbeeld SharePoint met object-id-waarde van uw app.)

   ```powershell
   $objguid = "8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4"
   ```

1. Bevestig dat u de juiste app hebt met de volgende opdracht. De uitvoer moet identiek zijn aan de uitvoer die u hebt gezien in de vorige sectie ([de object-id van de app vinden](#find-the-objectid-of-the-app)).

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

1. Maak een lege toepassingsobject voor het opslaan van de wijzigingen die u wilt maken.

   ```powershell
   $appnew = New-Object "Microsoft.Open.AzureAD.Model.Application"
   ```

1. De URL van startpagina ingesteld op de waarde die u wilt. De waarde moet een pad van het subdomein van de gepubliceerde app. Bijvoorbeeld, als u de URL van de startpagina van `https://sharepoint-iddemo.msappproxy.net/` naar `https://sharepoint-iddemo.msappproxy.net/hybrid/`, gaat u rechtstreeks naar de startpagina van aangepaste app-gebruikers.

   ```powershell
   $homepage = "https://sharepoint-iddemo.msappproxy.net/hybrid/"
   ```

1. Controleer de update van de startpagina.

   ```powershell
   Set-AzureADApplication -ObjectId $objguid -Homepage $homepage
   ```

1. Als u wilt controleren of de wijziging voltooid is, voert u de volgende opdracht opnieuw uit uit stap 2.

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

   In ons voorbeeld wordt de uitvoer nu weergegeven als volgt:

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/hybrid/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

1. Start opnieuw op de app om te bevestigen dat de startpagina wordt weergegeven als het eerste scherm, zoals verwacht.

> [!NOTE]
> Alle wijzigingen die u in de app aanbrengt mogelijk opnieuw instellen van de URL van startpagina. Als de URL van uw startpagina wordt opnieuw ingesteld, herhaalt u de stappen in deze sectie om opnieuw ingesteld.

## <a name="next-steps"></a>Volgende stappen

- [Externe toegang tot SharePoint met Azure AD-toepassingsproxy inschakelen](application-proxy-integrate-with-sharepoint-server.md)
- [Zelfstudie: Toevoegen van een on-premises toepassing voor externe toegang via Application Proxy in Azure Active Directory](application-proxy-add-on-premises-application.md)