---
title: Aan de slag met Azure AD Privileged Identity Management | Microsoft Docs
description: Informatie over het beheren van bevoegde identiteiten met de Azure Active Directory Privileged Identity Management-toepassing in de Azure Portal.
services: active-directory
documentationcenter: ''
author: kgremban
manager: femila
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/16/2016
ms.author: kgremban

---
# Aan de slag met Azure AD Privileged Identity Management
Met Azure Active Directory (AD) Privileged Identity Management kunt u toegang binnen de organisatie beheren, controleren en bewaken. Dit is inclusief toegang tot resources in Azure AD en andere Microsoft-onlineservices zoals Office 365 en Microsoft Intune.

In dit artikel leest u hoe u de PIM-app van Azure AD toevoegt aan uw Azure Portal-dashboard.

## De Privileged Identity Management-toepassing toevoegen
Voordat u Azure AD Privileged Identity Management gebruikt, moet u de toepassing toevoegen aan uw Azure Portal-dashboard.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als globale beheerder van uw directory.
2. Als uw organisatie meerdere directory's heeft, selecteert u uw gebruikersnaam rechtsboven in de Azure Portal. Selecteer de map waar u PIM gaat gebruiken.
3. Selecteer **Meer services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

Als u de eerste persoon bent die Azure AD Privileged Identity Management in uw directory gebruikt, wordt u via de [wizard Beveiliging](active-directory-privileged-identity-management-security-wizard.md) stapsgewijs begeleid bij de eerste toewijzing. Hierna wordt u automatisch de eerste **beveiligingsbeheerder** en **beheerder met bevoorrechte rol** van de directory. Alleen een beheerder met bevoorrechte rol heeft toegang tot deze toepassing om de toegang voor andere beheerders te beheren.  

## Navigeer naar uw taken
Nadat Azure AD Privileged Identity Management is ingesteld, ziet u de navigatieblade telkens wanneer u de toepassing opent. Gebruik deze blade om de taken voor identiteitsbeheer te voltooien.

![Taken op het hoogste niveau voor PIM - schermopname](./media/active-directory-privileged-identity-management-getting-started/pim_tasks.png)

* **Mijn rollen activeren**: hiermee gaat u naar de lijst met functies die aan u zijn toegewezen. Dit is waar u alle functies activeert waarvoor u in aanmerking komt.
* **Bevoorrechte rollen beheren**: dit is het dashboard waarmee beheerders met bevoorrechte rollen roltoewijzingen kunnen beheren, instellingen voor rolactivering kunnen wijzigen, toegangsbeoordelingen kunnen starten en meer. De opties in dit dashboard zijn uitgeschakeld voor iedereen die geen beheerder met een bevoorrechte rol is.
* **Bevoegde toegang beoordelen**: hiermee gaat u naar eventuele in behandeling zijnde toegangsbeoordelingen die u moet voltooien, of u nu de toegang beoordeelt voor uzelf of voor iemand anders. 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Volgende stappen
Het [overzicht van Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) bevat meer informatie over het beheren van beheerderstoegang in uw organisatie.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png



<!--HONumber=Sep16_HO3-->


