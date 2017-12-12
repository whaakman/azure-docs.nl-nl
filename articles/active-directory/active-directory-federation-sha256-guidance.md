---
title: Wijziging handtekening hash-algoritme voor Office 365 relying-partyvertrouwensrelatie | Microsoft Docs
description: Deze pagina bevat richtlijnen voor het wijzigen van SHA-algoritme voor federatieve vertrouwensrelatie met Office 365
keywords: SHA1, SHA256, O365, Federatie, aadconnect, adfs, ad fs, wijziging sha, federatieve vertrouwensrelatie, vertrouwensrelatie van relying party
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: mtillman
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: ec7eee36888d825d65335db590731103aabbf5c2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Handtekening hash-algoritme voor Office 365 vertrouwensrelatie van relying party wijzigen
## <a name="overview"></a>Overzicht
Active Directory Federation Services (AD FS), ondertekent de tokens in Microsoft Azure Active Directory om ervoor te zorgen dat ze met kunnen worden geknoeid. Deze handtekening kan zijn gebaseerd op SHA1 of SHA256. Azure Active Directory biedt nu ondersteuning voor tokens die zijn ondertekend met een algoritme SHA256 en het is raadzaam als u het algoritme voor token-ondertekening op SHA256 voor het hoogste niveau van beveiliging. In dit artikel beschrijft de stappen die nodig zijn voor de algoritme voor token-ondertekening ingesteld op veiliger SHA256 niveau.

>[!NOTE]
>Microsoft raadt informatie over het gebruik van SHA256 als voor het ondertekenen van tokens als het is veiliger dan SHA1 maar nog steeds een ondersteunde optie SHA1-algoritme.

## <a name="change-the-token-signing-algorithm"></a>Wijzigen van het algoritme voor token-ondertekening
Nadat u de algoritme voor handtekening met een van de twee onderstaande processen hebt ingesteld, wordt in AD FS de tokens voor Office 365 relying party trust met SHA256 ondertekent. U hoeft niet te worden eventuele extra configuratiewijzigingen aanbrengen en deze wijziging heeft geen invloed op uw mogelijkheid voor toegang tot Office 365 of andere Azure AD-toepassingen.

### <a name="ad-fs-management-console"></a>AD FS-beheerconsole
1. Open de AD FS-beheerconsole op de primaire AD FS-server.
2. Vouw het knooppunt van de AD FS en klik op **Relying Party-vertrouwensrelaties**.
3. Met de rechtermuisknop op uw Office 365/Azure-vertrouwensrelatie voor relying party en selecteer **eigenschappen**.
4. Selecteer de **Geavanceerd** tabblad en selecteer het veilige hash-algoritme SHA256.
5. Klik op **OK**.

![SHA256 ondertekeningsalgoritme--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS-PowerShell-cmdlets
1. Open PowerShell onder administrator-bevoegdheden op de AD FS-server.
2. Het veilige hash-algoritme ingesteld met behulp van de **Set-AdfsRelyingPartyTrust** cmdlet.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Lees ook
* [Herstel van Office 365-vertrouwensrelatie met Azure AD Connect](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

