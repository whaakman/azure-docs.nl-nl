---
title: Vereisten voor Azure Data Catalog
description: Meer informatie over de vereisten voor het aan de slag met Azure Data Catalog.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 788e7a453e9879d9c901448659ff1c8cfa7201ff
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57548304"
---
# <a name="azure-data-catalog-prerequisites"></a>Vereisten voor Azure Data Catalog

U moet zorgen voor een aantal dingen voordat u Azure Data Catalog kunt instellen. Maak u geen zorgen, dit proces duurt niet lang.

## <a name="azure-subscription"></a>Azure-abonnement
Als u Data Catalog instelt, moet u de eigenaar of mede-eigenaar van een Azure-abonnement zijn.

Azure-abonnementen kunnen u toegang tot cloud-service-resources zoals Data Catalog indelen. Abonnementen kunnen u ook bepalen hoe Resourcegebruik wordt gerapporteerd, gefactureerd en betaald. Elk abonnement kan een afzonderlijke facturering en betaling instellingen hebben, zodat u kunt abonnementen en plannen die per afdeling, project, regiokantoor, enzovoort variëren. Bij elke cloudservice behoort tot een abonnement en u moet beschikken over een abonnement voordat u het instellen van Data Catalog. Zie [Accounts, abonnementen en beheerdersrollen beheren](../active-directory/users-groups-roles/directory-assign-admin-roles.md) voor meer informatie.

## <a name="azure-active-directory"></a>Azure Active Directory
Als u Data Catalog instelt, moet u zijn aangemeld met een gebruikersaccount van Azure Active Directory (Azure AD).

Azure AD biedt uw bedrijf een eenvoudige manier om ID’s en toegang, zowel in de cloud als on-premises, te beheren. Gebruikers kunnen één werk- of schoolaccount gebruiken voor eenmalige aanmelding bij elke cloud en on-premises webtoepassing. Data Catalog maakt gebruik van Azure AD om te verifiëren aanmelden. Zie voor meer informatie, [wat is Azure Active Directory?](../active-directory/fundamentals/active-directory-whatis.md).

> [!NOTE]
> Met behulp van de [Azure-portal](https://portal.azure.com/), kunt u zich aanmelden met een persoonlijk Microsoft-account of een Azure Active Directory-werk- of schoolaccount. Data Catalog instellen met behulp van de Azure-portal of de [Data Catalog-portal](https://www.azuredatacatalog.com), u moet zich aanmelden met Azure Active Directory-account, niet een persoonlijk account.
>
>

## <a name="active-directory-policy-configuration"></a>Active Directory-beleid configureren
U kunt een situatie tegenkomen wanneer u zich kunt aanmelden bij de Data Catalog-portal, maar wanneer u probeert te melden bij het registratiehulpprogramma voor gegevensbronnen, er wordt een foutbericht weergegeven dat u niet aanmelden. Dit probleem probleem kan optreden wanneer u op het bedrijfsnetwerk, of deze zich voordoen kan alleen als u verbinding vanaf buiten het bedrijfsnetwerk bevindt maakt.

Het registratiehulpprogramma voor gegevensbronnen maakt gebruik van formulierverificatie voor het valideren van de referenties van de gebruiker op basis van Active Directory. Voor hulp bij het aanmelden is gelukt, moet een Active Directory-beheerder formulierverificatie in het globale verificatiebeleid inschakelen.

In het globale verificatiebeleid kunnen verificatiemethoden worden ingeschakeld afzonderlijk voor intranet en extranet-verbindingen, zoals wordt weergegeven in de volgende schermafbeelding. Aanmelden fouten optreden als formulierverificatie niet is ingeschakeld voor het netwerk van waaruit u verbinding maakt.

 ![Het algemene verificatiebeleid Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [verificatiebeleid configureren](https://technet.microsoft.com/library/dn486781.aspx).
