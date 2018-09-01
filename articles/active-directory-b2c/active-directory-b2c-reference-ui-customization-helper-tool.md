---
title: Hulpprogramma voor aanpassing Page UI in Azure Active Directory B2C | Microsoft Docs
description: Een hulpprogramma gebruikt om te demonstreren van de pagina UI-aanpassing-functie in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1a37a37dbed3b5ef9733f1105444529b4d255bcf
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336778"
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Een hulpprogramma dat wordt gebruikt ter illustratie van de functie pagina gebruiker gebruikersinterface (UI) aanpassen
In dit artikel is een aanvulling op de [belangrijkste UI-aanpassing artikel](active-directory-b2c-reference-ui-customization.md) in Azure Active Directory (Azure AD) B2C. De volgende stappen wordt beschreven hoe u de pagina UI-aanpassing functie oefenen met behulp van de voorbeeld-HTML en CSS-inhoud die beschikbaar is gesteld.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant verkrijgen
Voordat u alles aanpassen kunt, moet u [een Azure AD B2C-tenant verkrijgen](active-directory-b2c-get-started.md) als u er nog geen hebt.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Registratie- of aanmeldingsbeleid maken
De voorbeeldinhoud zijn er beschikbaar gesteld kan worden gebruikt om customze twee pagina's in een [beleid voor registreren of aanmelden](active-directory-b2c-reference-policies.md): de [geïntegreerde aanmeldingspagina](active-directory-b2c-reference-ui-customization.md) en de [pagina zelf-gecontroleerde kenmerken](active-directory-b2c-reference-ui-customization.md). Wanneer [maken van uw beleid voor registreren of aanmelden](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), lokaal Account (e-mailadres), Facebook, Google en Microsoft als toevoegen **id-providers**. Dit zijn de enige id-providers die onze HTML-voorbeeldinhoud accepteert.  U kunt ook een subset van deze id-providers toevoegen als u wenst.

## <a name="register-an-application"></a>Een toepassing registreren
U moet [registreren van een toepassing](active-directory-b2c-app-registration.md) in uw B2C-tenant die kan worden gebruikt voor het uitvoeren van uw beleid. U hebt een aantal opties die u kunt het registratiebeleid daadwerkelijk moet uitvoeren na de registratie van uw toepassing:

* Bouw een van de Azure AD B2C-snel starten-toepassingen die worden vermeld in de sectie 'Aan de slag' van [ondertekenen en consumenten in uw toepassingen aanmelden](active-directory-b2c-overview.md).
* Gebruik vooraf gemaakte [Azure AD B2C-Speelplaats](https://aadb2cplayground.azurewebsites.net) toepassing. Als u ervoor de playground gebruiken kiest, moet u een toepassing registreren in uw B2C-tenant met behulp van de **omleidings-URI** `https://aadb2cplayground.azurewebsites.net/`.
* Gebruik de **nu uitvoeren** knop op het beleid in de [Azure-portal](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Het beleid aanpassen
Voor het aanpassen van het uiterlijk van uw beleid, moet u eerst maken HTML en CSS-bestanden met behulp van de specifieke regels van het Azure AD B2C. Vervolgens kunt u de statische inhoud uploaden naar een openbaar beschikbare locatie zodat deze toegankelijk in de Azure AD B2C. Dit wordt mogelijk uw eigen specifieke webserver, Azure Blob Storage, Azure Content Delivery Network of iedere andere statische resource-hosting provider. De enige vereisten zijn dat uw inhoud beschikbaar via HTTPS is en kan worden geopend met behulp van CORS. Nadat u uw statische inhoud op Internet beschikbaar gemaakt hebt, kunt u uw beleid om te verwijzen naar deze locatie en die inhoud aan uw klanten kunt bewerken. De [belangrijkste UI-aanpassing artikel](active-directory-b2c-reference-ui-customization.md) een gedetailleerde beschrijving van de werking van de functie voor het aanpassen van Azure AD B2C.

We hebben al enkele voorbeelden voor inhoud die zijn gemaakt en deze die worden gehost op Azure Blob-opslag voor de doeleinden van deze zelfstudie. De voorbeeldinhoud is een zeer eenvoudige aanpassingen in het thema van onze fictieve bedrijf, 'Wingtip Toys'. Probeer het uit in uw eigen beleid, de volgende stappen uit:

1. Aanmelden bij uw tenant op de [Azure-portal](https://portal.azure.com/) en navigeer naar de blade B2C-functies.
2. Klikt u op **beleid voor registreren of aanmelden**, klikt u op uw beleid en klik op bewerken (bijvoorbeeld ' b2c\_1\_aanmelding\_van\_aanmelding\_in ').
3. Klik op **Page UI-aanpassing** en vervolgens **Unified registreren of aanmelden pagina**.
4. Schakelen tussen de **gebruik aangepaste pagina** overschakelen naar **Ja**. In de **aangepaste pagina URI** veld `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klik op **OK**.
5. Klik op **pagina voor het registreren van lokale account**. Schakelen tussen de **aangepaste sjabloon gebruiken** overschakelen naar **Ja**. In de **aangepaste pagina URI** veld `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. Herhaal de dezelfde stap voor de **aanmeldpagina voor sociaal account**.
   Klik op **OK** tweemaal in sluit de blades UI-aanpassing.
7. Klik op **Opslaan**.

U kunt nu uw aangepaste beleid uitproberen. U kunt uw eigen toepassing of de Azure AD B2C-Speelplaats gebruiken als u wilt, maar u kunt ook gewoon klikken op de **nu uitvoeren** opdracht in de beleidsblade. Selecteer uw toepassing in de vervolgkeuzelijst en kies de juiste omleidings-URI. Klik op de **nu uitvoeren** knop. Er wordt een nieuw browsertabblad geopend en u kunt uitvoeren via de ervaring van de gebruiker zich registreert voor uw toepassing met de nieuwe inhoud in plaats.

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>De voorbeeldinhoud uploaden naar Azure Blob Storage
Als u gebruiken van Azure Blob Storage wilt voor het hosten van uw pagina-inhoud, kunt u uw eigen storage-account maken en onze hulpprogramma B2C gebruiken om uw bestanden te uploaden.

### <a name="create-a-storage-account"></a>Create a storage account
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **+ nieuw** > **gegevens en opslag** > **opslagaccount**. U moet een Azure-abonnement te maken van een Azure Blob Storage-account. U kunt zich aanmelden met een gratis proefversie op de [Azure-website](https://azure.microsoft.com/pricing/free-trial/).
3. Geef een **naam** voor de storage-account (bijvoorbeeld 'contoso') en kies de gewenste selecties voor **prijscategorie**, **resourcegroep** en  **Abonnement**. Zorg ervoor dat u hebt de **vastmaken aan Startboard** optie is ingeschakeld. Klik op **Create**.
4. Ga terug naar het Startboard en klik op het opslagaccount dat u zojuist hebt gemaakt.
5. In de **samenvatting** sectie, klikt u op **Containers**, en klik vervolgens op **+ toevoegen**.
6. Geef een **naam** voor de container (bijvoorbeeld ' b2c') en selecteer **Blob** als de **toegangstype**. Klik op **OK**.
7. De container die u hebt gemaakt in de lijst wordt weergegeven op de **Blobs** blade. Noteer de URL van de container. bijvoorbeeld, er ongeveer als volgt uitzien `https://contoso.blob.core.windows.net/b2c`. Sluit de **Blobs** blade.
8. Klik op de blade opslagaccount **sleutels** en noteer de waarden van de **Opslagaccountnaam** en **primaire toegangssleutel** velden.

> [!NOTE]
> **Primaire toegangssleutel** is een belangrijke beveiligingsreferentie.
> 
> 

### <a name="download-the-helper-tool-and-sample-files"></a>Download de helper hulpprogramma en de voorbeeld-bestanden
U kunt downloaden de [Azure Blob Storage helper hulpprogramma en de voorbeeld-bestanden als een ZIP-bestand](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) of deze klonen vanuit GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Deze opslagplaats bevat een `sample_templates\wingtip` directory waarin de voorbeeld-HTML, CSS en afbeeldingen. Voor deze sjablonen gebruiken om te verwijzen naar uw eigen Azure Blob Storage-account, moet u de HTML-bestanden bewerken. Open `unified.html` en `selfasserted.html` en Vervang alle exemplaren van `https://localhost` met de URL van uw eigen container die u hebt genoteerd in de vorige stappen. U kunt het absolute pad van de HTML-bestanden moet gebruiken, omdat in dit geval wordt de HTML-code worden geleverd door Azure AD, onder het domein `tenantname.b2clogin.com`.

### <a name="upload-the-sample-files"></a>De voorbeeldbestanden uploaden
In dezelfde opslagplaats, pak deze uit `B2CAzureStorageClient.zip` en voer de `B2CAzureStorageClient.exe` binnen het bestand. Dit programma wordt gewoon alle bestanden in de map die u naar uw opslagaccount opgeeft uploaden en inschakelen van CORS-toegang voor deze bestanden. Als u de bovenstaande stappen hebt gevolgd, is de HTML en CSS-bestanden wordt nu verwezen naar uw storage-account. Houd er rekening mee dat de naam van uw storage-account is het gedeelte die voorafgaat aan `blob.core.windows.net`, bijvoorbeeld `contoso`. U kunt controleren of dat de inhoud correct is geüpload door het openen van `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` in een browser. Ook gebruiken [ http://test-cors.org/ ](http://test-cors.org/) om ervoor te zorgen dat de inhoud nu CORS is ingeschakeld is. (Zoek naar ' XHR status: 200 ' in het resultaat.)

### <a name="customize-your-policy-again"></a>Het beleid opnieuw aanpassen
Nu dat u hebt de voorbeeldinhoud geüpload naar uw eigen opslagaccount, moet u het registratiebeleid om te verwijzen naar deze bewerken. Herhaal de stappen van de ['Uw beleid aanpassen'](#customize-your-policy) sectie hierboven, met behulp van URL's van uw eigen opslagaccount. Bijvoorbeeld, de locatie van uw `unified.html` bestand `<url-of-your-container>/wingtip/unified.html`.

Nu kunt u de **nu uitvoeren** knop of uw eigen toepassing om uw beleid voor opnieuw uitvoeren. Het resultaat ziet er bijna exact dezelfde--u gebruikt hetzelfde voorbeeld HTML en CSS in beide gevallen. Uw beleid nu verwijzen naar uw eigen exemplaar van Azure Blob Storage, en u zijn echter gratis om te bewerken en de bestanden opnieuw als u kunt uploaden. Raadpleeg voor meer informatie over het aanpassen van de HTML en CSS, de [belangrijkste UI-aanpassing artikel](active-directory-b2c-reference-ui-customization.md).

