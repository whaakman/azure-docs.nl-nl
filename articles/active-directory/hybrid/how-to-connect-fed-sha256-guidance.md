---
title: Wijziging handtekening hash-algoritme voor Office 365 relying partyvertrouwensrelatie | Microsoft Docs
description: Deze pagina bevat richtlijnen voor het wijzigen van SHA-algoritme voor federatieve vertrouwensrelatie met Office 365
keywords: SHA1, SHA256, O365, Federatie, aadconnect, adfs, ad fs, wijziging sha, federatieve vertrouwensrelatie, vertrouwensrelatie van relying party
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2018
ms.author: billmath
ms.openlocfilehash: ca200c76596dc1a4f31bf36f00bb1661d12d19f7
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426351"
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Handtekening hash-algoritme voor Office 365 vertrouwensrelatie van relying party wijzigen
## <a name="overview"></a>Overzicht
Active Directory Federation Services (AD FS) ondertekent de tokens in Microsoft Azure Active Directory om ervoor te zorgen dat ze kunnen worden geknoeid. Deze handtekening kan worden gebaseerd op SHA1 of SHA256. Azure Active Directory biedt nu ondersteuning voor tokens die zijn ondertekend met een algoritme SHA256 en het beste het algoritme voor token-ondertekening voor SHA256 instellen voor het hoogste niveau van beveiliging. Dit artikel beschrijft de stappen die nodig zijn om het algoritme voor token-ondertekening in te stellen de veiliger SHA256 niveau.

>[!NOTE]
>Microsoft raadt het gebruik van SHA256 als voor het ondertekenen van tokens, omdat het is veiliger dan SHA1 maar nog steeds een ondersteunde optie SHA1-algoritme.

## <a name="change-the-token-signing-algorithm"></a>Wijzigen van het algoritme voor token-ondertekening
Nadat u de handtekeningalgoritme met een van de twee onderstaande processen hebt ingesteld, wordt in AD FS de tokens voor Office 365 relying party trust met SHA256 ondertekent. U hoeft niet elke extra configuratiewijzigingen aanbrengen en deze wijziging heeft geen invloed op uw mogelijkheid voor toegang tot Office 365 of andere Azure AD-toepassingen.

### <a name="ad-fs-management-console"></a>AD FS-beheerconsole
1. Open de AD FS-beheerconsole op de primaire AD FS-server.
2. Vouw het knooppunt van AD FS en klikt u op **Relying Party-vertrouwensrelaties**.
3. Met de rechtermuisknop op uw Office 365/Azure-vertrouwensrelatie voor relying party en selecteer **eigenschappen**.
4. Selecteer de **Geavanceerd** tabblad en selecteer het veilige hash-algoritme SHA256.
5. Klik op **OK**.

![SHA256 ondertekeningsalgoritme--MMC](./media/how-to-connect-fed-sha256-guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS-PowerShell-cmdlets
1. Open PowerShell onder administrator-bevoegdheden op elke AD FS-server.
2. De veilige hash-algoritme instellen met behulp van de **Set AdfsRelyingPartyTrust** cmdlet.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'https://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Lees ook
* [Herstellen van Office 365-vertrouwensrelatie met Azure AD Connect](how-to-connect-fed-management.md#repairthetrust)

