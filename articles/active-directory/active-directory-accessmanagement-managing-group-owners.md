---
title: Volgende stappen voor het toegangsbeheer van met behulp van groepen | Microsoft Docs
description: Hoe geavanceerde-aan de voor het beheren van beveiligingsgroepen en het gebruik van deze groepen voor het beheren van toegang tot een resource.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.custom: it-pro
ms.openlocfilehash: 177e31b8b8aff90cd91135d02eb3a2d8c85e2c33
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="managing-owners-for-a-group"></a>Eigenaars voor een groep beheren
Wanneer een resource-eigenaar heeft toegang tot een resource aan een Azure AD-groep is toegewezen, wordt het lidmaatschap van de groep wordt beheerd door de eigenaar van de groep. De eigenaar van de resource delegeert effectief de machtiging gebruikers toewijzen aan de resource toe aan de eigenaar van de groep.

## <a name="add-an-owner-to-a-group"></a>Een eigenaar toevoegen aan een groep

1. In de [Azure AD-beheercentrum](https://aad.portal.azure.com), selecteer **gebruikers en groepen**.
2. Selecteer **alle groepen**, en open vervolgens de groep die u wilt toevoegen, eigenaren.
3. Selecteer **eigenaars toevoegen**.
4. Op de **eigenaars toevoegen** pagina, selecteert u de gebruiker die u wilt toevoegen als de eigenaar van deze groep en zorg ervoor dat deze naam wordt toegevoegd aan de **geselecteerde** deelvenster.

## <a name="remove-an-owner-from-a-group"></a>Een eigenaar verwijderen uit een groep

1. In de [Azure AD-beheercentrum](https://aad.portal.azure.com), selecteer **gebruikers en groepen**.
2. Selecteer **alle groepen**, en open vervolgens de groep waaruit u wilt verwijderen van eigenaren.
3. Selecteer de **eigenaars** tabblad.
4. Selecteer de eigenaar die u wilt verwijderen uit deze groep en selecteer vervolgens **verwijderen**.

## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Azure Active Directory cmdlets for configuring group settings](active-directory-accessmanagement-groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [What is Azure Active Directory?](active-directory-whatis.md) (Wat is Azure Active Directory?)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
