---
title: Configureren van een toepassing uitgeversdomein | Azure
description: Informatie over het configureren van een toepassing uitgeversdomein zodat gebruikers weten waar hun gegevens worden verzonden.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/05/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, zachowd
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d47075f9e18b299341a98983ffb8a47389fd7063
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540244"
---
# <a name="how-to-configure-an-applications-publisher-domain-preview"></a>Procedure: Configureren van een toepassing uitgeversdomein (Preview)

Uitgeversdomein van een toepassing wordt weergegeven aan gebruikers op de [van de toepassing toestemmingsprompt](application-consent-experience.md) om gebruikers te informeren wanneer de gegevens worden verzonden. Toepassingen met meerdere tenants die zijn geregistreerd na 21 mei 2019 waarvoor geen een uitgeversdomein weergegeven als **niet-geverifieerde**. Toepassingen met meerdere tenants zijn toepassingen die ondersteuning bieden voor accounts buiten een organisatie één map; bijvoorbeeld, ondersteuning voor alle Azure AD-accounts of ondersteuning voor alle Azure AD-accounts en persoonlijke Microsoft-accounts.

## <a name="new-applications"></a>Nieuwe toepassingen

Als u een nieuwe app registreert, kan het uitgeversdomein van uw app op een standaardwaarde worden ingesteld. De waarde is afhankelijk van waar de app is geregistreerd, met name of de app in een tenant is geregistreerd en of de tenant heeft tenant domeinen geverifieerde.

Als er tenant-gecontroleerde domeinen, wordt de primaire geverifieerde domein voor de tenant van de app uitgeversdomein standaard. Als er zijn geen tenant geverifieerde domeinen (dit is het geval wanneer de toepassing is niet geregistreerd in een tenant), van de app-uitgeversdomein wordt ingesteld op null.

De volgende tabel geeft een overzicht van het standaardgedrag van de uitgever domein-waarde.  

| Tenant-gecontroleerde domeinen | De standaardwaarde van uitgeversdomein |
|-------------------------|----------------------------|
| null | null |
| *.onmicrosoft.com | *.onmicrosoft.com |
| - *.onmicrosoft.com<br/>-Domein1.com<br/>-Domein2.com (primair) | domain2.com |

Als een multitenant-toepassing uitgeversdomein is niet ingesteld of als deze ingesteld op een domein dat eindigt op. onmicrosoft.com, toestemmingsprompt van de app wordt weergegeven **niet-geverifieerde** in plaats van het uitgeversdomein.

## <a name="grandfathered-applications"></a>Grandfathered toepassingen

Als uw app is geregistreerd voordat 21 mei 2019 toestemmingsprompt van uw toepassing wordt niet weergegeven **niet-geverifieerde** als u een uitgeversdomein niet hebt ingesteld. U wordt aangeraden dat u de uitgever domeinwaarde zo instellen dat gebruikers deze informatie op toestemmingsprompt van uw app kunnen zien.

## <a name="configure-publisher-domain-using-the-azure-portal"></a>Uitgeversdomein met behulp van de Azure-portal configureren

Volg deze stappen om in te stellen van uw app uitgeversdomein.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.

1. Als uw account in meer dan één Azure AD-tenant aanwezig is:
   1. Selecteer uw profiel in het menu in de rechterbovenhoek van de pagina, en vervolgens **schakelen tussen mappen**.
   1. Uw sessie wijzigen in de Azure AD-tenant waar u om uw toepassing te maken.

1. Navigeer naar [Azure Active Directory > App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) om te zoeken en selecteer de app die u wilt configureren.

   Wanneer u de app hebt geselecteerd, ziet u de app **overzicht** pagina.

1. Uit van de app **overzicht** weergeeft, schakelt de **Branding** sectie.

1. Zoek de **uitgeversdomein** veld en selecteer een van de volgende opties:

   - Selecteer **configureren van een domein** als dit nog niet hebt u al een domein geconfigureerd.
   - Selecteer **updatedomein** als een domein is al geconfigureerd.

Als uw app in een tenant is geregistreerd, ziet u twee tabbladen software selecteren uit: **Selecteer een geverifieerd domein** en **controleren of een nieuw domein**.

Als uw app niet is geregistreerd bij een tenant, ziet u alleen de optie om te controleren of een nieuw domein voor uw toepassing.

### <a name="to-verify-a-new-domain-for-your-app"></a>Om te controleren of een nieuw domein voor uw app

1. Maak een bestand met de naam `microsoft-identity-association.json` en plak de volgende JSON-codefragment.

   ```json
   {
      "associatedApplications": [
        {
           "applicationId": "{YOUR-APP-ID-HERE}"
        }
      ]
    }
   ```

1. Vervang de tijdelijke aanduiding *{YOUR-APP-ID-HERE}* met de toepassing (client)-ID die overeenkomt met uw app.

1. Het bestand op te hosten: `https://{YOUR-DOMAIN-HERE}.com/.well-known/microsoft-identity-association.json`. Vervang de tijdelijke aanduiding *{uw domein hier}* zodat deze overeenkomt met het geverifieerde domein.

1. Klik op de **verifiëren en opslaan van domein** knop.

### <a name="to-select-a-verified-domain"></a>Een geverifieerd domein selecteren

- Als uw tenant heeft geverifieerd domeinen, selecteert u een van de domeinen van de **selecteert u een geverifieerd domein** vervolgkeuzelijst.

## <a name="implications-on-the-app-consent-prompt"></a>Gevolgen voor de app instemmingsprompt

Configureren van het uitgeversdomein heeft een invloed op wat gebruikers op de app toestemming gevraagd zien. Zie volledig inzicht in de onderdelen van de toestemmingsprompt [inzicht in de toepassing toestemming ervaringen](application-consent-experience.md).

De volgende tabel beschrijft het gedrag voor toepassingen die zijn gemaakt vóór 21 mei 2019.

![Toestemming gevraagd voor apps die zijn gemaakt vóór 21 mei 2019](./media/howto-configure-publisher-domain/old-app-behavior-table.png)

Het gedrag voor nieuwe toepassingen die zijn gemaakt na 21 mei 2019, is afhankelijk van het uitgeversdomein en het type toepassing. De volgende tabel beschrijft de wijzigingen die u kunt verwachten om te zien waarop de verschillende combinaties van configuraties.

![Toestemming gevraagd voor apps die zijn gemaakt na 21 mei 2019](./media/howto-configure-publisher-domain/new-app-behavior-table.png)

## <a name="implications-on-redirect-uris"></a>Gevolgen voor de omleidings-URI 's

Toepassingen die Meld u aan gebruikers met een werk- of schoolaccount of persoonlijk Microsoft-accounts ([multitenant](single-and-multi-tenant-apps.md)) zijn onderworpen aan enkele beperkingen bij het opgeven van omleidings-URI's.

### <a name="single-root-domain-restriction"></a>Beperking van één domein

Wanneer de waarde voor het domein van uitgever voor apps met meerdere tenants is ingesteld op null, apps zijn beperkt tot het delen van een domein met één hoofdmap voor de omleidings-URI's. Bijvoorbeeld, de volgende combinatie van waarden is niet toegestaan omdat het hoofddomein, contoso.com, fabrikam.com komt niet overeen.

```
"https://contoso.com",
"https://fabrikam.com",
```

### <a name="subdomain-restrictions"></a>Subdomein beperkingen

Subdomeinen zijn toegestaan, maar u moet het hoofddomein expliciet registreren. Bijvoorbeeld, terwijl de volgende URI's delen één hoofddomein, is niet de combinatie toegestaan.

```
"https://app1.contoso.com",
"https://app2.contoso.com",
```

Echter, als het hoofddomein expliciet door de ontwikkelaar worden toegevoegd, is de combinatie toegestaan.

```
"https://contoso.com",
"https://app1.contoso.com",
"https://app2.contoso.com",
```

### <a name="exceptions"></a>Uitzonderingen

De volgende gevallen zijn niet afhankelijk van de beperking van één domein:

- Apps met één tenant of apps die zijn gericht op accounts in één map
- Gebruik van ' localhost ' als omleidings-URI 's
- Omleidings-URI's met aangepaste schema's (niet-HTTP of HTTPS)

## <a name="configure-publisher-domain-programmatically"></a>Uitgeversdomein via een programma configureren

Er is op dit moment geen REST-API of PowerShell-ondersteuning aan uitgeversdomein via een programma configureren.
