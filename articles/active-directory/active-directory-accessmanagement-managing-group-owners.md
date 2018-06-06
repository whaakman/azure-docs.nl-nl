---
title: Volgende stappen voor het toegangsbeheer van met behulp van groepen | Microsoft Docs
description: Hoe geavanceerde-aan de voor het beheren van beveiligingsgroepen en het gebruik van deze groepen voor het beheren van toegang tot een resource.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.custom: it-pro
ms.openlocfilehash: 0f379c22f1bad522a2d9bff399bbf7006f7c4920
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713276"
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
