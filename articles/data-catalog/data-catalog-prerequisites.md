---
title: Vereisten voor Azure Data Catalog | Microsoft Docs
description: Meer informatie over de vereisten die u moet aan de slag met Azure Data Catalog.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: a7effaaaeb23661b9be96dcddc2c140ab8c8b92b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-data-catalog-prerequisites"></a>Vereisten voor Azure Data Catalog

U moet zorgen voor een aantal dingen voordat u Azure Data Catalog kunt instellen. Maak je geen zorgen, dit proces is niet lang duren.

## <a name="azure-subscription"></a>Azure-abonnement
Als u Data Catalog instelt, moet u de eigenaar of mede-eigenaar van een Azure-abonnement.

Azure-abonnementen kunnen u toegang tot de service in de cloud bronnen zoals Data Catalog indelen. Abonnementen kunnen u ook bepalen hoe Resourcegebruik is gerapporteerd, kosten in rekening gebracht en betaald. Elk abonnement kan een afzonderlijke facturerings- en -instellingen hebben, zodat u kunt abonnementen en schema's die per afdeling, project en regionaal kantoor verschillen. Elke cloudservice behoort tot een abonnement en moet u beschikken over een abonnement voordat u Data Catalog instelt. Zie [Accounts, abonnementen en beheerdersrollen beheren](../active-directory/active-directory-assign-admin-roles-azure-portal.md) voor meer informatie.

## <a name="azure-active-directory"></a>Azure Active Directory
Als u Data Catalog instelt, moet u bent aangemeld met een gebruikersaccount van Azure Active Directory (Azure AD).

Azure AD biedt uw bedrijf een eenvoudige manier om ID’s en toegang, zowel in de cloud als on-premises, te beheren. Gebruikers kunnen één werk- of schoolaccount gebruiken voor één aanmelding toe aan een cloud en lokale webtoepassing. Data Catalog gebruikt Azure AD om te verifiëren aanmelden. Zie voor meer informatie, [wat is Azure Active Directory?](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Met behulp van de [Azure-portal](http://portal.azure.com/), kunt u zich aanmelden met een persoonlijk Microsoft-account of een Azure Active Directory werk- of schoolaccount. Voor het instellen van de catalogus met gegevens met behulp van de Azure-portal of de [Data Catalog-portal](http://www.azuredatacatalog.com), moet u zich aanmelden met een Azure Active Directory-account, niet een persoonlijk account.
>
>

## <a name="active-directory-policy-configuration"></a>Configuratie van Active Directory-beleid
U kunt een situatie tegenkomen wanneer u zich kunt aanmelden bij de Data Catalog-portal, maar wanneer u probeert aan te melden bij het registratiehulpprogramma van gegevensbronnen, u ontvangt een foutmelding dat u niet aanmelden. Dit probleem kan gebeuren wanneer je op het bedrijfsnetwerk, of deze zich voordoen kan alleen wanneer u verbinding vanaf buiten het bedrijfsnetwerk maakt.

Het registratiehulpprogramma van gegevensbronnen formulierverificatie gebruikt voor het valideren van uw gebruikersreferenties op basis van Active Directory. Als u met succes aanmelden, moet een Active Directory-beheerder formulierverificatie in het globale verificatiebeleid inschakelen.

In het globale verificatiebeleid kunnen verificatiemethoden worden ingeschakeld afzonderlijk voor intranet en extranet-verbindingen, zoals wordt weergegeven in de volgende schermafbeelding. Aanmelden fouten optreden als formulierverificatie niet is ingeschakeld voor het netwerk van waaruit u verbinding maakt.

 ![Het globale verificatiebeleid Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [Authenticatiebeleid configureren](https://technet.microsoft.com/library/dn486781.aspx).
