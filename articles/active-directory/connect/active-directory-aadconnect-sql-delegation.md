---
title: Installeren Azure AD Connect met SQL gedelegeerde beheerdersmachtigingen | Microsoft Docs
description: Dit onderwerp beschrijft een update naar Azure AD Connect waarmee de installatie met een account dat alleen SQL dbo-machtigingen heeft.
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.reviewer: jparsons
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 198ecdbf81c2b8efeec23da2c5d5d087128b20e9
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233560"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>Installeren Azure AD Connect met SQL delegated administrator-machtigingen
Vóór de laatste build van de Azure AD Connect beheerdersrechten delegering bij het implementeren van configuraties die SQL vereist, niet wordt ondersteund.  Gebruikers die wilden Azure AD Connect installeert die nodig zijn om machtigingen voor server-beheerder (SA) op de SQL server.

Met de meest recente versie van Azure AD Connect worden inrichten van de database, kunnen nu uitgevoerd buiten de band door de SQL-beheerder en geïnstalleerd door de beheerder van Azure AD Connect met database-eigendomsrechten.

## <a name="before-you-begin"></a>Voordat u begint
U moet deze functie wilt gebruiken, houd er rekening mee dat er verschillende bewegende delen en elk een andere beheerder in uw organisatie mogelijk.  De volgende tabel geeft een overzicht van de afzonderlijke rollen en hun respectieve taken bij het implementeren van Azure AD Connect met deze functie.

|Rol|Beschrijving|
|-----|-----|
|Domein of Forest AD-beheerder|Het serviceaccount dat wordt gebruikt door Azure AD Connect voor het uitvoeren van de synchronisatieservice niveau van domein maakt.  Zie voor meer informatie over serviceaccounts [Accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md).
|SQL-beheerder|De database van ADSync maakt en aanmelding + dbo verleent toegang tot de Azure AD Connect-beheerder en het serviceaccount gemaakt door de beheerder van de domein-forest.|
Azure AD Connect-beheerder|Azure AD Connect installeert en geeft van het serviceaccount tijdens een aangepaste installatie.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>Stappen voor het installeren van Azure AD Connect met SQL overgedragen machtigingen
Gebruik de volgende stappen voor het inrichten van de buiten-band-database en Azure AD Connect installeert met machtigingen voor database-eigenaar.

>[!NOTE]
>Hoewel dit niet vereist is, is het **ten zeerste aangeraden** dat de sortering Latin1_General_CI_AS is geselecteerd bij het maken van de database.


1.  De SQL-beheerder de database ADSync maakt met een reeks hoofdlettergevoelige sortering **(Latin1_General_CI_AS)**.  De database moet de naam **ADSync**.  Het herstelmodel, compatibiliteitsniveau en containment-type zijn bijgewerkt naar de juiste waarden wanneer Azure AD Connect is geïnstalleerd.  Maar de volgorde moet correct zijn ingesteld door de SQL-beheerder anders Azure AD Connect zal de installatie geblokkeerd.  Als u wilt herstellen van de SA moeten verwijderen en opnieuw maken van de database.</br>
![Sortering](media/active-directory-aadconnect-sql-delegation/sql4.png)
2.  Verleen de beheerder van de Azure AD Connect en het domeinaccount van de service de volgende machtigingen:
    - SQL-aanmelding 
    - **database-owner(dbo)** rechten.  </br>
![Machtigingen](media/active-directory-aadconnect-sql-delegation/sql3a.png)
3.  Een e-mail sturen naar de Azure AD Connect-beheerder die aangeeft dat de SQL server en het exemplaar dat moet worden gebruikt bij het installeren van Azure AD Connect.

## <a name="additional-information"></a>Aanvullende informatie
Zodra de database is geconfigureerd, wordt dit door de beheerder van de Azure AD Connect kunt installeren en configureren van lokale synchronisatie op hun gemak.  

De **/UseExistingDatabase** vlag is vereist als u een vooraf gemaakte database.  Het is niet alleen gebruikt in recovery situaties.

Naast de ondersteuning van nieuwe installaties van Azure AD Connect met deze functie ook kunt delegatie voor elk scenario betrekking hebben op de **/UseExistingDatabase** vlag.  Zie voor meer informatie over het installeren van Azure AD Connect met een bestaande database [Azure AD Connect installeren met een bestaande database van ADSync](active-directory-aadconnect-existing-database.md)


## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met Azure AD Connect met Express-instellingen](active-directory-aadconnect-get-started-express.md)
- [Aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
- [Azure AD Connect met een bestaande database van ADSync installeren](active-directory-aadconnect-existing-database.md)  
