---
title: Een aangepaste startpagina voor gepubliceerde apps instellen met behulp van Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD Application Proxy connectors
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: e6f4e67d09eacadbbf9d74f417357a87ece0a951
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238502"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Een aangepaste startpagina voor gepubliceerde apps instellen met behulp van Azure AD-toepassingsproxy

In dit artikel wordt beschreven hoe u apps voor gebruikers naar de startpagina van een aangepaste configureren. Wanneer u een toepassing met Application Proxy publiceert, moet u een interne URL, maar soms is niet de pagina die moeten eerst te zien krijgen uw gebruikers instellen. De startpagina van een aangepaste zo instellen dat uw gebruikers gaat u naar de juiste pagina wanneer ze toegang krijgen de apps tot. Uw gebruikers zien de aangepaste startpagina die u hebt ingesteld, of ze de app vanuit het toegangsvenster Azure Active Directory of het startprogramma voor Office 365 openen.

Wanneer gebruikers de app starten, worden ze omgeleid standaard naar de hoofdmap domein-URL voor de gepubliceerde app. De startpagina is standaard ingesteld als de URL van startpagina. Gebruik de Azure AD PowerShell-module voor het definiëren van aangepaste startpagina-URL's als u wilt dat appgebruikers op een specifieke pagina in de app. 

Hier volgt een voorbeeld van waarom een bedrijf een aangepaste startpagina wordt ingesteld:
- Binnen uw bedrijfsnetwerk gebruikers gaan naar *https://ExpenseApp/login/login.aspx* aanmelden en toegang tot uw app.
- Omdat er andere activa, zoals afbeeldingen die Application Proxy moet toegang hebben tot op het hoogste niveau van de mapstructuur, publiceert u de app met *https://ExpenseApp* als de interne URL.
- Is de standaard-URL voor externe *https://ExpenseApp-contoso.msappproxy.net*, die uw gebruikers naar de aanmeldingspagina niet uitvoeren.  
- Stel *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* als de URL van startpagina. 

>[!NOTE]
>Wanneer u gebruikers toegang tot gepubliceerde apps verleent, de apps worden weergegeven in de [Azure AD-Toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md) en de [startprogramma voor Office 365](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

## <a name="before-you-start"></a>Voordat u begint

Voordat u de URL van startpagina, houd rekening met de volgende vereisten:

* Zorg ervoor dat het opgegeven pad een subdomein-pad van de basis-URL-domein is.

  Als de URL hoofddomein, bijvoorbeeld https://apps.contoso.com/app1/, de URL van startpagina die u configureert moet beginnen met https://apps.contoso.com/app1/.

* Als u een wijziging in de gepubliceerde app aanbrengt, kan de wijziging opnieuw instellen van de waarde van de URL van startpagina. Wanneer u de app in de toekomst bijwerkt, moet u controleren en, indien nodig werkt u de URL van startpagina.

## <a name="change-the-home-page-in-the-azure-portal"></a>Wijzigen van de startpagina in de Azure-portal

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **App-registraties** en kiest u uw toepassing in de lijst. 
3. Selecteer **eigenschappen** uit de instellingen.
4. Update de **URL van startpagina** veld met het nieuwe pad. 

   ![Nieuwe startpagina-URL opgeven](./media/application-proxy-configure-custom-home-page/homepage.png)

5. Selecteer **opslaan**

## <a name="change-the-home-page-with-powershell"></a>Wijzigen van de startpagina met PowerShell

### <a name="install-the-azure-ad-powershell-module"></a>De Azure AD PowerShell-module installeren

Voordat u de URL van een aangepaste startpagina definiëren met behulp van PowerShell, installeert u de Azure AD PowerShell-module. U kunt het downloaden van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), waarbij de Graph API-eindpunt wordt gebruikt. 

Volg deze stappen voor het installeren van het pakket:

1. Open een standaard PowerShell-venster en voer de volgende opdracht:

    ```
     Install-Module -Name AzureAD
    ```
    Als u de opdracht als een niet-beheerder uitvoert, gebruikt u de `-scope currentuser` optie.
2. Tijdens de installatie, selecteert u **Y** twee pakketten installeren via Nuget.org. Beide pakketten zijn vereist. 

### <a name="find-the-objectid-of-the-app"></a>De object-id van de app zoeken

De object-id van de app verkrijgen en zoek vervolgens de app door de startpagina.

1. In de dezelfde PowerShell-venster, de Azure AD-module te importeren.

    ```
    Import-Module AzureAD
    ```

2. Meld u aan de module Azure AD als de tenantbeheerder.

    ```
    Connect-AzureAD
    ```
3. De app op basis van de URL van de startpagina te zoeken. U vindt de URL in de beheerportal door te gaan naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen**. In dit voorbeeld wordt *sharepoint-iddemo*.

    ```
    Get-AzureADApplication | where { $_.Homepage -like "sharepoint-iddemo" } | fl DisplayName, Homepage, ObjectID
    ```
4. U moet een resultaat die vergelijkbaar is met hieronder. Kopieer de ObjectID GUID die u wilt gebruiken in de volgende sectie.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a>Bijwerken van de URL van startpagina

De URL van startpagina maken en bijwerken van uw toepassing met de waarde. Ga door met de dezelfde PowerShell-venster voor het uitvoeren van deze opdrachten. Of, als u een nieuwe PowerShell-venster, aanmelden bij het opnieuw met behulp van Azure AD-module `Connect-AzureAD`. 

1. Bevestig dat u de juiste App en vervang *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* met de object-id die u in de voorgaande sectie hebt gekopieerd.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Nu dat u hebt vastgesteld dat de app, u kunt als volgt bijwerken van de startpagina.

2. Maak een lege toepassingsobject voor het opslaan van de wijzigingen die u wilt maken. Deze variabele bevat de waarden die u wilt bijwerken. Niets is in deze stap gemaakt.

    ```
    $appnew = New-Object "Microsoft.Open.AzureAD.Model.Application"
    ```

3. De URL van startpagina ingesteld op de waarde die u wilt. De waarde moet een pad van het subdomein van de gepubliceerde app. Bijvoorbeeld, als u de URL van de startpagina van *https://sharepoint-iddemo.msappproxy.net/* naar *https://sharepoint-iddemo.msappproxy.net/hybrid/*, gaat u rechtstreeks naar de startpagina van aangepaste app-gebruikers.

    ```
    $homepage = "https://sharepoint-iddemo.msappproxy.net/hybrid/"
    ```
4. Moet u de update met behulp van de GUID (object-id) die u hebt genoteerd in ' stap 1: de object-id van de app vinden. "

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. Als u wilt controleren of de wijziging voltooid is, start u de app opnieuw.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Alle wijzigingen die u in de app aanbrengt mogelijk opnieuw instellen van de URL van startpagina. Als de URL van uw startpagina wordt opnieuw ingesteld, herhaalt u de stappen in deze sectie om opnieuw ingesteld.

## <a name="next-steps"></a>Volgende stappen

- [Externe toegang tot SharePoint met Azure AD-toepassingsproxy inschakelen](application-proxy-integrate-with-sharepoint-server.md)
- [Toepassingsproxy inschakelen in Azure portal](application-proxy-enable.md)
