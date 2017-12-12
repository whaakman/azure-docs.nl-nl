---
title: 'Azure Active Directory B2C: Hulpprogramma Page UI aanpassing helper | Microsoft Docs'
description: Een helper hulpprogramma dat wordt gebruikt voor het demonstreren van de functie pagina UI aanpassen in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: ae935d52-3520-4a94-b66e-b35bb40e7514
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: swkrish
ms.openlocfilehash: a9ccdea64213d564b271699afe28f5ae6db0a71a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Een helper hulpprogramma dat wordt gebruikt voor het demonstreren van de pagina gebruiker gebruikersinterface (UI)-functie met aanpassen
Dit artikel is een aanvulling op de [belangrijkste UI aanpassing artikel](active-directory-b2c-reference-ui-customization.md) in Azure Active Directory (Azure AD) B2C. De volgende stappen wordt beschreven hoe de functie UI aanpassing oefenen met behulp van HTML en CSS voorbeeldinhoud die we hebt opgegeven.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant verkrijgen
Voordat u alles aanpassen kunt, moet u [een Azure AD B2C-tenant verkrijgen](active-directory-b2c-get-started.md) als u er nog geen hebt.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Maak een beleid registreren of aanmelden
De voorbeeldinhoud bevat, kan worden gebruikt voor customze twee pagina's in een [registreren of aanmelden beleid](active-directory-b2c-reference-policies.md): de [unified aanmeldingspagina](active-directory-b2c-reference-ui-customization.md) en de [pagina zelf aangenomen kenmerken](active-directory-b2c-reference-ui-customization.md). Wanneer [maken van uw beleid registreren of aanmelden](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), Voeg het lokale Account (e-mailadres), Facebook, Google en Microsoft als **identiteitsproviders**. Dit zijn de enige IDPs die onze voorbeeld HTML-inhoud worden geaccepteerd.  U kunt ook een subset van deze IDPs toevoegen als u wenst.

## <a name="register-an-application"></a>Een toepassing registreren
U moet [een toepassing registreren](active-directory-b2c-app-registration.md) in uw B2C-tenant die kan worden gebruikt voor het uitvoeren van uw beleid. U hebt een aantal opties die u kunt het registratiebeleid daadwerkelijk worden uitgevoerd na de registratie van uw toepassing:

* Een van de Azure AD B2C snel starten-toepassingen die worden vermeld in de sectie 'Aan de slag' van bouwen [aanmelden en aanmelden van gebruikers in uw toepassingen](active-directory-b2c-overview.md#get-started).
* Gebruik de vooraf gemaakte [Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) toepassing. Als u de playground gebruiken wilt, moet u een toepassing registreren in uw B2C-tenant met behulp van de **omleidings-URI** `https://aadb2cplayground.azurewebsites.net/`.
* Gebruik de **nu uitvoeren** knop op uw beleid in de [Azure-portal](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Het beleid aanpassen
Voor het aanpassen van het uiterlijk van uw beleid, moet u eerst maken HTML en CSS-bestanden met behulp van de specifieke regels van het Azure AD B2C. Vervolgens kunt u uw statische inhoud uploaden naar een openbaar toegankelijke locatie zodat Azure AD B2C toegang kunnen hebben. Dit kan zijn eigen speciale webserver, Azure Blob Storage, Azure Content Delivery Network of elke andere statische resource-hosting provider. De enige vereisten zijn dat uw inhoud beschikbaar via HTTPS is en kan worden geopend met behulp van CORS. Zodra u hebt uw statische inhoud op het web blootgesteld, kunt u uw beleid om te verwijzen naar deze locatie is en die inhoud voor uw klanten kunt bewerken. De [belangrijkste UI aanpassing artikel](active-directory-b2c-reference-ui-customization.md) een gedetailleerde beschrijving van de werking van de functie voor het aanpassen van Azure AD B2C.

Voor de doeleinden van deze zelfstudie hebt we al enkele voorbeeldinhoud gemaakt en wordt gehost op Azure Blob Storage. De voorbeeldinhoud is een zeer eenvoudige aanpassing van het thema van onze fictieve bedrijf, 'Wingtip Toys'. Als u wilt uitproberen in uw eigen beleid, de volgende stappen uit:

1. Aanmelden bij uw tenant op de [Azure-portal](https://portal.azure.com/) en navigeer naar de blade B2C-functies.
2. Klik op **registreren of aanmelden beleid** en klik vervolgens op het beleid (bijvoorbeeld ' b2c\_1\_aanmelding\_up\_aanmelding\_in ').
3. Klik op **Page UI-aanpassing** en vervolgens **Unified registreren of aanmelden pagina**.
4. Schakelen tussen de **gebruik aangepaste pagina** overschakelen naar **Ja**. In de **aangepaste pagina URI** veld `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klik op **OK**.
5. Klik op **aanmeldpagina voor lokaal account**. Schakelen tussen de **aangepaste sjabloon gebruiken** overschakelen naar **Ja**. In de **aangepaste pagina URI** veld `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. Herhaal de dezelfde stap voor de **sociale account aanmeldingspagina**.
   Klik op **OK** tweemaal in de gebruikersinterface aanpassing blades te sluiten.
7. Klik op **Opslaan**.

U kunt nu uw aangepaste beleid uitproberen. U kunt uw eigen toepassing of de Azure AD B2C-playground gebruiken als u wilt, maar u kunt ook gewoon op de **nu uitvoeren** opdracht in de blade beleid. Selecteer uw toepassing in de vervolgkeuzelijst en kies de juiste omleidings-URI. Klik op de **nu uitvoeren** knop. Een nieuw browsertabblad geopend en u kunt uitvoeren via de ervaring van de gebruiker zich registreert voor uw toepassing met de nieuwe inhoud aanwezig!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>De voorbeeldinhoud uploaden naar Azure Blob Storage
Als u gebruiken van Azure Blob Storage wilt voor het hosten van uw pagina-inhoud, kunt u uw eigen opslagaccount maken en onze B2C helper-hulpprogramma gebruiken om uw bestanden te uploaden.

### <a name="create-a-storage-account"></a>Een opslagaccount maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **+ nieuw** > **gegevens en opslag** > **opslagaccount**. U moet een Azure-abonnement om een Azure Blob Storage-account te maken. U kunt aanmelden met een gratis proefversie op de [Azure-website](https://azure.microsoft.com/pricing/free-trial/).
3. Geef een **naam** voor de opslag account (bijvoorbeeld 'contoso') en kies de gewenste selecties voor **prijscategorie**, **resourcegroep** en  **Abonnement**. Zorg ervoor dat u hebt de **vastmaken aan Startboard** optie ingeschakeld. Klik op **Create**.
4. Ga terug naar het Startboard en klik op het opslagaccount dat u zojuist hebt gemaakt.
5. In de **samenvatting** sectie, klikt u op **Containers**, en klik vervolgens op **+ toevoegen**.
6. Geef een **naam** voor de container (bijvoorbeeld ' b2c') en selecteer **Blob** als de **toegangstype**. Klik op **OK**.
7. De container die u hebt gemaakt in de lijst worden weergegeven op de **Blobs** blade. Noteer de URL van de container. bijvoorbeeld, moet er ongeveer als `https://contoso.blob.core.windows.net/b2c`. Sluit de **Blobs** blade.
8. Klik op de blade opslagaccount **sleutels** en noteer de waarden van de **Opslagaccountnaam** en **primaire toegangssleutel** velden.

> [!NOTE]
> **Primaire toegangssleutel** is een belangrijke beveiligingsreferentie.
> 
> 

### <a name="download-the-helper-tool-and-sample-files"></a>De helper-hulpprogramma's en voorbeeld-bestanden downloaden
U kunt downloaden via de [Azure Blob Storage helper hulpprogramma's en voorbeeld-bestanden als een ZIP-bestand](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) of het klonen van GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Deze bibliotheek bevat een `sample_templates\wingtip` directory waarin voorbeeld HTML, CSS en afbeeldingen. Voor deze sjablonen om te verwijzen naar uw eigen Azure Blob Storage-account, moet u de HTML-bestanden bewerken. Open `unified.html` en `selfasserted.html` en Vervang alle exemplaren van `https://localhost` met de URL van uw eigen container die u hebt genoteerd in de vorige stappen. U kunt het absolute pad van de HTML-bestanden moet gebruiken, omdat in dit geval de HTML-code kan worden geleverd door Azure AD, onder het domein `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>De voorbeeldbestanden uploaden
Pak in dezelfde opslagplaats `B2CAzureStorageClient.zip` en voer de `B2CAzureStorageClient.exe` binnen het bestand. Dit programma wordt gewoon alle bestanden in de map die u voor uw opslagaccount opgeeft te uploaden en CORS-toegang voor deze bestanden. Als u de bovenstaande stappen hebt gevolgd, is de HTML- en CSS-bestanden wordt nu verwijst naar uw storage-account. Houd er rekening mee dat de naam van uw storage-account is het gedeelte die voorafgaat aan `blob.core.windows.net`, bijvoorbeeld `contoso`. U kunt controleren dat de inhoud is geüpload correct door probeert te krijgen tot `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` in een browser. Ook gebruiken [http://test-cors.org/](http://test-cors.org/) om ervoor te zorgen dat de inhoud nu CORS is ingeschakeld is. (Zoek naar ' XHR status: 200 ' in het resultaat.)

### <a name="customize-your-policy-again"></a>Het beleid opnieuw aanpassen
Nu dat u hebt de voorbeeldinhoud geüpload naar uw eigen opslagaccount, moet u het registratiebeleid om te verwijzen naar deze bewerken. Herhaal de stappen van de ['Aanpassen van uw beleid'](#customize-your-policy) sectie hierboven, met behulp van URL's van uw eigen opslagaccount. Bijvoorbeeld, de locatie van uw `unified.html` bestand `<url-of-your-container>/wingtip/unified.html`.

Nu kunt u de **nu uitvoeren** knop of uw eigen toepassing naar uw beleid voor opnieuw uitvoeren. Het resultaat moet bijna hetzelfde er hetzelfde uitzien--u gebruikt hetzelfde HTML en CSS steekproef in beide gevallen. Echter uw beleid nu verwijzen naar uw eigen exemplaar van Azure Blob Storage en vrij om te bewerken en Neem de bestanden als u opnieuw uploaden. Raadpleeg voor meer informatie over het aanpassen van de HTML en CSS de [belangrijkste UI aanpassing artikel](active-directory-b2c-reference-ui-customization.md).

