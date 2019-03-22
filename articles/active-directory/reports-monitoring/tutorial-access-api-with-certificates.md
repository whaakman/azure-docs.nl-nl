---
title: Zelfstudie Get-gegevens met behulp van de Reporting API van Azure AD met certificaten | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u de Azure AD rapportage-API gebruikt met certificaatreferenties om gegevens uit directory's zonder tussenkomst van de gebruiker.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8706414b27969efd6aa26085370f8a0b1f891d69
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58095191"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Zelfstudie: Gegevens ophalen met de Azure Active Directory rapportage-API met certificaten

De [Azure Active Directory (Azure AD) rapportage-API's](concept-reporting-api.md) bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze API's vanuit een groot aantal computertalen en hulpprogramma's aanroepen. Als u toegang wilt tot de Azure AD rapportage-API zonder tussenkomst van de gebruiker, moet u de toegang voor het gebruik van certificaten configureren.

In deze zelfstudie leert u hoe u een testcertificaat gebruikt voor toegang tot de MS Graph-API voor rapportage. U kunt beter geen testcertificaten in een productieomgeving. 

## <a name="prerequisites"></a>Vereisten

1. Voor toegang tot aanmeldingsgegevens, zorg ervoor dat u hebt een Azure Active Directory-tenant met een licentie premium (P1/P2). Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden. Als er vóór de upgrade nog geen activiteitgegevens waren, duurt het na het upgraden naar een premium-licentie enkele dagen voordat er gegevens worden weergegeven in de rapporten. 

2. Maken of schakel over naar een gebruikersaccount in de **hoofdbeheerder**, **beveiligingsbeheerder**, **beveiligingslezer** of **rapporteren lezer** rol voor de tenant. 

3. Voltooi de [vereisten voor toegang tot de Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md). 

4. Download en installeer [Azure AD PowerShell V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md).

5. Install [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Deze module biedt verschillende cmdlets, waaronder:
    - De ADAL-bibliotheken die nodig zijn voor verificatie
    - Toegangstokens van gebruiker, toepassingssleutels en certificaten met behulp van ADAL
    - Afhandeling van pagina's met zoekresultaten door Graph API

6. Als dit de eerste keer is met de module voeren **installeren MSCloudIdUtilsModule**, anders importeren met behulp van de **Import-Module** Powershell-opdracht. Uw sessie moet lijken op dit scherm: ![Windows Powershell](./media/tutorial-access-api-with-certificates/module-install.png)
  
7. Gebruik de **New-SelfSignedCertificate** Powershell-commandlet een testcertificaat maken.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

8. Gebruik de **Export-certificaat** commandlet exporteren naar een certificaatbestand.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Gegevens ophalen met de Azure Active Directory rapportage-API met certificaten

1. Navigeer naar de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory**en selecteer vervolgens **App-registraties** en kiest u uw toepassing in de lijst. 

2. Selecteer **instellingen** > **sleutels** en selecteer **openbare sleutel uploaden**.

3. Selecteer het certificaatbestand in de vorige stap en selecteer **opslaan**. 

4. Noteer de toepassings-ID en de vingerafdruk van het certificaat dat u zojuist hebt geregistreerd met uw toepassing. Als u wilt zoeken van de vingerafdruk, op de toepassingspagina van uw in de portal, gaat u naar **instellingen** en klikt u op **sleutels**. De vingerafdruk van het zich onder de **openbare sleutels** lijst.

5. Open het toepassingsmanifest in het manifest inline-editor en vervang de *keyCredentials* eigenschap met de nieuwe gegevens van een certificaat met het volgende schema. 

   ```
   "keyCredentials": [
        {
            "customKeyIdentifier": "$base64Thumbprint", //base64 encoding of the certificate hash
            "keyId": "$keyid", //GUID to identify the key in the manifest
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value":  "$base64Value" //base64 encoding of the certificate raw data
        }
    ]
   ```

6. Sla het manifest. 
  
7. Nu kunt u een toegangstoken ophalen voor de MS Graph API met behulp van dit certificaat. Gebruik de **Get-MSCloudIdMSGraphAccessTokenFromCert** cmdlet uit de MSCloudIdUtils PowerShell-module, doorgeven in de toepassings-ID en de vingerafdruk die u hebt verkregen in de vorige stap. 

   ![Azure Portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

8. Het toegangstoken in uw Powershell-script gebruiken om te vragen van de Graph API. Gebruik de **Invoke-MSCloudIdMSGraphQuery** cmdlet uit de MSCloudIDUtils het eindpunt signins en directoryAudits inventariseren. Deze cmdlet met meerdere pagina's met zoekresultaten verwerkt, en stuurt deze resultaten naar de PowerShell-pijplijn.

9. Query uitvoeren op het eindpunt directoryAudits om op te halen van de auditlogboeken. 
   ![Azure-portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

10. Query uitvoeren op het eindpunt signins om op te halen van de logboeken van de aanmelding.
    ![Azure-portal](./media/tutorial-access-api-with-certificates/query-signins.png)

11. Nu kunt u deze gegevens exporteren naar een CSV en opslaan in een SIEM-systeem. U kunt uw script ook verpakken in een geplande taak om periodiek gegevens van Azure AD op te halen uit uw tenant zonder dat u toepassingssleutels hoeft op te slaan in de broncode. 

## <a name="next-steps"></a>Volgende stappen

* [Een eerste indruk van de rapportage-API 's krijgen](concept-reporting-api.md)
* [Controle van de API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Rapport van aanmeldingsactiviteiten API-verwijzing](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
