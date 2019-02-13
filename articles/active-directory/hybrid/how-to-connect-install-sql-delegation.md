---
title: Installeer Azure AD Connect met SQL delegated administrator-machtigingen | Microsoft Docs
description: Dit onderwerp beschrijft een update naar Azure AD Connect voor installatie met behulp van een account met alleen SQL dbo-machtigingen.
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: jparsons
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38ad75d22d21a141d48e9664ae580dfb5577a389
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184921"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>Installeer Azure AD Connect met behulp van SQL delegated administrator-machtigingen
Voordat u de nieuwste build van Azure AD Connect, met beheerdersrechten de delegatie, bij het implementeren van configuraties die SQL vereist, niet wordt ondersteund.  Gebruikers die wil Azure AD Connect installeert die nodig zijn om machtigingen voor serverbeheerders (SA) op de SQL server.

Met de meest recente versie van Azure AD Connect, worden inrichten van de database kan nu uitgevoerd buiten-band door de SQL-beheerder en vervolgens worden geïnstalleerd door de Azure AD Connect-beheerder met eigendomsrechten van de database.

## <a name="before-you-begin"></a>Voordat u begint
U moet deze functie wilt gebruiken, houd er rekening mee dat er meerdere bewegende onderdelen en elk ervan kan betrekking hebben op een andere beheerder in uw organisatie.  De volgende tabel geeft een overzicht van de afzonderlijke functies en hun respectieve taken bij het implementeren van Azure AD Connect met deze functie.

|Rol|Description|
|-----|-----|
|Domein of Forest AD-beheerder|Hiermee maakt u het serviceaccount dat wordt gebruikt door Azure AD Connect om uit te voeren van de synchronisatieservice niveau van domein.  Zie voor meer informatie over service-accounts, [Accounts en machtigingen](reference-connect-accounts-permissions.md).
|SQL-beheerder|De ADSync-database wordt gemaakt en aanmelding + dbo verleent toegang tot de Azure AD Connect-beheerder en het serviceaccount dat is gemaakt door de beheerder domein/forest.|
Azure AD Connect-beheerder|Azure AD Connect installeert en Hiermee geeft u de serviceaccount tijdens een aangepaste installatie.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>Stappen voor het installeren van Azure AD Connect met behulp van SQL-gedelegeerde machtigingen
Gebruik de volgende stappen voor het inrichten van de buiten-band-database en Azure AD Connect installeren met machtigingen voor database-eigenaar.

>[!NOTE]
>Hoewel het niet vereist is, is het **ten zeerste aangeraden** dat de sortering Latin1_General_CI_AS is geselecteerd bij het maken van de database.


1.  De SQL-beheerder de ADSync-database maken met een reeks hoofdlettergevoelige sortering **(Latin1_General_CI_AS)**.  De database moet de naam **ADSync**.  Het herstelmodel, compatibiliteitsniveau en containment-type zijn bijgewerkt naar de juiste waarden als Azure AD Connect is geïnstalleerd.  Maar de volgorde moet correct zijn ingesteld door de SQL-beheerder anders blokkeren Azure AD Connect de installatie.  Als u wilt herstellen van de SA moeten verwijderen en opnieuw maken van de database.</br>
![Sortering](./media/how-to-connect-install-sql-delegation/sql4.png)
2.  De Azure AD Connect-beheerder en het domeinaccount van de service de volgende machtigingen verlenen:
    - SQL-aanmelding 
    - **database-owner(dbo)** rechten.  </br>
![Machtigingen](./media/how-to-connect-install-sql-delegation/sql3a.png)
3.  Een e-mailbericht verzenden naar de Azure AD Connect-beheerder die wijzen op de naam van SQL server en het exemplaar dat moet worden gebruikt bij het installeren van Azure AD Connect.

## <a name="additional-information"></a>Aanvullende informatie
Zodra de database is ingericht, wordt dit door de beheerder van de Azure AD Connect kunt installeren en configureren van on-premises synchronisatie op het gebruiksgemak.  

Zie voor meer informatie over het installeren van Azure AD Connect met een bestaande database [Installeer Azure AD Connect met behulp van een bestaande ADSync-database](how-to-connect-install-existing-database.md)

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met Azure AD Connect met Express-instellingen](how-to-connect-install-express.md)
- [Aangepaste installatie van Azure AD Connect](how-to-connect-install-custom.md)
- [Azure AD Connect installeren met behulp van een bestaande ADSync-database](how-to-connect-install-existing-database.md)  
