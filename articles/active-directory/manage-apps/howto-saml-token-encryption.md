---
title: SAML-tokenversleuteling in Azure Active Directory
description: Informatie over het configureren van Azure Active Directory-SAML-tokenversleuteling.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: celested
ms.reviewer: paulgarn
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3a2c79fd46b9c14f1bbb2794581746f6ff45cd6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56179617"
---
# <a name="how-to-configure-azure-ad-saml-token-encryption-preview"></a>Procedure: Configureren van Azure AD-SAML-tokenversleuteling (Preview)

> [!NOTE]
> Versleuteling van beveiligingstoken is een Azure Active Directory (Azure AD) premium-functie. Zie voor meer informatie over Azure AD-edities, functies en prijzen, [prijzen voor Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

SAML-tokenversleuteling maakt het gebruik van versleutelde SAML-asserties ondertekend met een toepassing die wordt ondersteund. Wanneer geconfigureerd voor een toepassing, wordt de SAML-asserties ondertekend die het verzendt voor de toepassing met behulp van de openbare sleutel die is verkregen van een certificaat dat is opgeslagen in Azure AD versleutelen in Azure AD. De toepassing moet de overeenkomende persoonlijke sleutel gebruiken voor het ontsleutelen van het token voordat deze kan worden gebruikt als bewijs van verificatie voor de aangemelde gebruiker.

Versleutelen van het SAML-asserties ondertekend tussen Azure AD en de toepassing biedt extra zekerheid dat de inhoud van het token kan niet worden onderschept, en de persoonlijke of zakelijke gegevens aangetast.

Azure AD-SAML-tokens worden ook zonder versleuteling van beveiligingstoken, nooit op het netwerk in de doorgegeven. Azure AD moet token verzoek/reactie-uitwisseling moet plaatsvinden via versleutelde HTTPS/TLS-kanalen, zodat de communicatie tussen de id-provider, de browser en de toepassing verspreid over een versleutelde koppelingen. Houd rekening met de waarde van de versleuteling van beveiligingstoken voor uw situatie vergeleken met de overhead van het beheren van extra certificaten.   

Voor het configureren van versleuteling van beveiligingstoken, moet u een x.509-certificaat-bestand met de openbare sleutel naar het Azure AD-toepassing-object dat staat voor de toepassing uploaden. Als u wilt het X.509-certificaat hebt verkregen, kunt u downloaden van de toepassing zelf, of ophalen van de leverancier van de toepassing in gevallen waar de leverancier van de toepassing biedt versleutelingssleutels of in gevallen waar de toepassing wordt verwacht u dat voor een persoonlijke sleutel kan zijn gemaakt met behulp van hulpprogramma's voor cryptografie, de persoonlijke sleutel gedeelte geüpload naar het sleutelarchief van de toepassing en de bijbehorende openbare-sleutelcertificaat dat is geüpload naar Azure AD.

Azure AD maakt gebruik van AES-256 voor het versleutelen van de SAML-verklaring-gegevens.

## <a name="configure-saml-token-encryption"></a>SAML-tokenversleuteling configureren

Volg deze stappen voor het configureren van SAML-tokenversleuteling:

1. Verkrijgen van een openbare-sleutelcertificaat dat die overeenkomt met een persoonlijke sleutel die geconfigureerd in de toepassing.

    Maak een asymmetrisch sleutelpaar moet worden gebruikt voor versleuteling. Of, als de toepassing een openbare sleutel levert voor versleuteling, volgt u de instructies van de toepassing te downloaden van het X.509-certificaat.

    De openbare sleutel moet worden opgeslagen in een bestand van de x.509-certificaat in cer-indeling.

    Als de toepassing gebruikmaakt van een sleutel die u voor uw exemplaar maakt, volg de instructies van uw toepassing voor het installeren van de persoonlijke sleutel die de toepassing wordt gebruikt voor het decoderen van tokens van uw Azure AD-tenant.

1. Het certificaat toevoegen aan de configuratie van de toepassing in Azure AD.

### <a name="to-configure-token-encryption-in-the-azure-portal"></a>Versleuteling van beveiligingstoken configureren in Azure portal

U kunt het openbare certificaat toevoegen aan de configuratie van uw toepassing in Azure portal.

1. Ga naar de [Azure Portal](https://portal.azure.com).

1. Ga naar de **Azure Active Directory > bedrijfstoepassingen** blade en selecteer vervolgens de toepassing die u wilt configureren voor de versleuteling van beveiligingstoken voor.

1. Selecteer op de pagina van de toepassing **Token versleuteling**.

    ![Versleuteling van beveiligingstoken optie in Azure portal](./media/howto-saml-token-encryption/token-encryption-option-small.png)

    > [!NOTE]
    > De **Token versleuteling** optie is alleen beschikbaar voor SAML-toepassingen die zijn ingesteld vanuit de **bedrijfstoepassingen** blade in Azure portal, hetzij uit de galerie of een Buiten de galerie-app. Voor andere toepassingen, is deze optie uitgeschakeld. Voor toepassingen die zijn geregistreerd via de **App-registraties** ervaring in Azure portal, kunt u versleuteling configureren voor SAML-tokens met behulp van de toepassing het manifest, via Microsoft Graph of via PowerShell.

1. Op de **Token versleuteling** weergeeft, schakelt **certificaat importeren** het cer-bestand met uw openbare X.509-certificaat importeren.

    ![Het cer-bestand met het X.509-certificaat importeren](./media/howto-saml-token-encryption/import-certificate-small.png)

1. Zodra het certificaat is geïmporteerd en de persoonlijke sleutel is geconfigureerd voor gebruik aan van de toepassing, versleuteling activeren door het selecteren van de **...**  volgende voor de status van de vingerafdruk en selecteer vervolgens **tokenversleuteling activeren** van de opties in de vervolgkeuzelijst.

1. Selecteer **Ja** om te bevestigen van de activering van het token versleutelingscertificaat.

1. Bevestig dat de SAML-asserties ondertekend verzonden voor de toepassing worden versleuteld.

### <a name="to-deactivate-token-encryption-in-the-azure-portal"></a>Om te deactiveren versleuteling van beveiligingstoken in Azure portal

1. In de Azure-portal, gaat u naar **Azure Active Directory > bedrijfstoepassingen**, en selecteer vervolgens de toepassing die SAML-token versleuteling ingeschakeld is.

1. Selecteer op de pagina van de toepassing **Token versleuteling**, zoek het certificaat, en selecteer vervolgens de **...**  optie om het vervolgkeuzemenu weer te geven.

1. Selecteer **deactiveren versleuteling van beveiligingstoken**.

## <a name="configure-saml-token-encryption-using-graph-api-powershell-or-app-manifest"></a>Configureren met behulp van de Graph API, PowerShell of app-manifest SAML-tokenversleuteling

Certificaten voor bestandsversleuteling voor het toepassingsobject worden opgeslagen in Azure AD met een `encrypt` gebruik tag. U kunt meerdere certificaten voor bestandsversleuteling configureren en die actief zijn voor het versleutelen van tokens wordt geïdentificeerd door de `tokenEncryptionKeyID` kenmerk.

U moet de object-ID van de toepassing te configureren met behulp van Microsoft Graph API of PowerShell versleuteling van beveiligingstoken. U kunt deze waarde vinden via een programma, of door te gaan naar de toepassingsmap **eigenschappen** pagina in Azure portal en dat de **Object-ID** waarde.

Bij het configureren van een keyCredential met Graph, PowerShell, of in het toepassingsmanifest, moet u een GUID moet worden gebruikt voor de keyId genereren.

### <a name="to-configure-token-encryption-using-microsoft-graph"></a>Het configureren van versleuteling van beveiligingstoken met behulp van Microsoft Graph

1. Bijwerken van de toepassing `keyCredentials` met een X.509-certificaat voor versleuteling. Het volgende voorbeeld ziet hoe u dit doet.

    ```
    Patch https://graph.microsoft.com/beta/applications/<application objectid>

    { 
       "keyCredentials":[ 
          { 
             "type":"AsymmetricX509Cert","usage":"Encrypt",
             "keyId":"fdf8c5d8-f727-43fd-beaf-0f1521cf3d35",    (Use a GUID generator to obtain a value for the keyId)
             "key": "MIICADCCAW2gAwIBAgIQ5j9/b+n2Q4pDvQUCcy3…"  (Base64Encoded .cer file)
          }
        ]
    }
    ```

1. Identificeer het versleutelingscertificaat dat actief is voor het versleutelen van tokens. Het volgende voorbeeld ziet hoe u dit doet.

    ```
    Patch https://graph.microsoft.com/beta/applications/<application objectid> 

    { 
       "tokenEncryptionKeyId":"fdf8c5d8-f727-43fd-beaf-0f1521cf3d35" (The keyId of the keyCredentials entry to use)
    }
    ```

### <a name="to-configure-token-encryption-using-powershell"></a>Het configureren van versleuteling van beveiligingstoken met behulp van PowerShell

Deze functionaliteit is binnenkort beschikbaar. 

<!--
1. Use the latest Azure AD PowerShell module to connect to your tenant.

1. Set the token encryption settings using the **[Set-AzureApplication](https://docs.microsoft.com/powershell/module/azuread/set-azureadapplication?view=azureadps-2.0-preview)** command.

    ```
    Set-AzureADApplication -ObjectId <ApplicationObjectId> -KeyCredentials “<KeyCredentialsObject>”  -TokenEncryptionKeyId <keyID>
    ```

1. Read the token encryption settings using the following commands.

    ```
    $app=Get-AzureADApplication -ObjectId <ApplicationObjectId>
    $app.KeyCredentials
    $app.TokenEncryptionKeyId
    ```
-->

### <a name="to-configure-token-encryption-using-the-application-manifest"></a>Versleuteling van beveiligingstoken met behulp van het toepassingsmanifest configureren

1. Vanuit de Azure-portal, gaat u naar **Azure Active Directory > App-registraties**.

1. Selecteer **alle apps** in de vervolgkeuzelijst voor het weergeven van alle apps, en selecteer vervolgens de enterprise-toepassing die u wilt configureren.

1. Selecteer in de toepassingspagina **Manifest** bewerken de [toepassingsmanifest](../develop/reference-app-manifest.md).

1. Stel de waarde voor de `tokenEncryptionKeyId` kenmerk.

    Het volgende voorbeeld wordt een manifest van de toepassing geconfigureerd met twee versleutelingscertificaten en met de tweede geselecteerd als de actieve sessie met behulp van de tokenEnryptionKeyId.

    ```
    { 
      "id": "3cca40e2-367e-45a5-8440-ed94edd6cc35",
      "accessTokenAcceptedVersion": null,
      "allowPublicClient": false,
      "appId": "cb2df8fb-63c4-4c35-bba5-3d659dd81bf1",
      "appRoles": [],
      "oauth2AllowUrlPathMatching": false,
      "createdDateTime": "2017-12-15T02:10:56Z",
      "groupMembershipClaims": "SecurityGroup",
      "informationalUrls": { 
         "termsOfService": null, 
         "support": null, 
         "privacy": null, 
         "marketing": null 
      },
      "identifierUris": [ 
        "https://testapp"
      ],
      "keyCredentials": [ 
        { 
          "customKeyIdentifier": "Tog/O1Hv1LtdsbPU5nPphbMduD=", 
          "endDate": "2039-12-31T23:59:59Z", 
          "keyId": "8be4cb65-59d9-404a-a6f5-3d3fb4030351", 
          "startDate": "2018-10-25T21:42:18Z", 
          "type": "AsymmetricX509Cert", 
          "usage": "Encrypt", 
          "value": <Base64EncodedKeyFile> 
          "displayName": "CN=SAMLEncryptTest" 
        }, 
        {
          "customKeyIdentifier": "U5nPphbMduDmr3c9Q3p0msqp6eEI=",
          "endDate": "2039-12-31T23:59:59Z", 
          "keyId": "6b9c6e80-d251-43f3-9910-9f1f0be2e851",
          "startDate": "2018-10-25T21:42:18Z", 
          "type": "AsymmetricX509Cert", 
          "usage": "Encrypt", 
          "value": <Base64EncodedKeyFile> 
          "displayName": "CN=SAMLEncryptTest2" 
        } 
      ], 
      "knownClientApplications": [], 
      "logoUrl": null, 
      "logoutUrl": null, 
      "name": "Test SAML Application", 
      "oauth2AllowIdTokenImplicitFlow": true, 
      "oauth2AllowImplicitFlow": false, 
      "oauth2Permissions": [], 
      "oauth2RequirePostResponse": false, 
      "orgRestrictions": [], 
      "parentalControlSettings": { 
         "countriesBlockedForMinors": [], 
         "legalAgeGroupRule": "Allow" 
        }, 
      "passwordCredentials": [], 
      "preAuthorizedApplications": [], 
      "publisherDomain": null, 
      "replyUrlsWithType": [], 
      "requiredResourceAccess": [], 
      "samlMetadataUrl": null, 
      "signInUrl": "https://127.0.0.1:444/applications/default.aspx?metadata=customappsso|ISV9.1|primary|z" 
      "signInAudience": "AzureADMyOrg",
      "tags": [], 
      "tokenEncryptionKeyId": "6b9c6e80-d251-43f3-9910-9f1f0be2e851" 
    }  
    ```

## <a name="next-steps"></a>Volgende stappen

* Ontdek [hoe Azure AD het SAML-protocol gebruikt](../develop/active-directory-saml-protocol-reference.md)
* Informatie over de indeling, de beveiligingskenmerken en de inhoud van [SAML-tokens in Azure AD](../develop/reference-saml-tokens.md)
