---
title: Uw aangepaste domeinnaam toevoegen en federatieve aanmelding bij Azure Active Directory instellen| Microsoft Docs
description: Lees hoe u de domeinnamen van uw bedrijf toevoegt aan Azure Active Directory om federatieve aanmelding in te stellen tussen Azure Active Directory en uw on-premises federatieoplossing.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 27126c7e-e6d6-4ef3-a4fb-f5f0706e749d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: curtand;jeffsta
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 7f257dff6fdd423b89d1c52a84b64951dcf5915f
ms.contentlocale: nl-nl
ms.lasthandoff: 05/17/2017


---
<a id="add-your-custom-domain-name-to-azure-active-directory" class="xliff"></a>

# Uw aangepaste domeinnaam toevoegen aan Azure Active Directory
U kunt een aangepaste domeinnaam, zoals contoso.com configureren, zodat gebruikers in contoso.com een federatieve ervaring voor eenmalige aanmelding bij het bedrijfsnetwerk kunnen hebben. Als Active Directory Federation Services (AD FS) of een andere federatieserver al op uw bedrijfsnetwerk wordt uitgevoerd, kunt u het hulpprogramma Azure AD Connect gebruiken om Azure AD te configureren voor gebruik van de aangepaste domeinnaam. U kunt Azure AD Connect ook gebruiken om een nieuwe AD FS-omgeving te implementeren en deze te configureren voor federatieve eenmalige aanmelding bij Azure AD.

Als u niet beschikt over AD FS of een andere federatieserver en deze ook niet van plan bent te implementeren, volgt u de instructies in [Een aangepaste domeinnaam toevoegen aan Azure Active Directory](active-directory-add-domain.md).

<a id="add-a-custom-domain-name-to-your-directory" class="xliff"></a>

## Een aangepaste domeinnaam toevoegen aan uw directory
1. Meld u aan bij de [klassieke Azure Portal](https://manage.windowsazure.com/) met een gebruikersaccount met de rechten voor globale beheerder van uw Azure AD-directory.
2. Open in **Active Directory** uw directory en selecteer de tab **Domeinen**.
3. Selecteer op de opdrachtbalk de optie **Toevoegen** en geef vervolgens de naam op van uw aangepaste domein, bijvoorbeeld 'contoso.com'. Vergeet niet de extensie .com, .net of een andere extensie van het hoogste niveau toe te voegen.
4. Schakel het selectievakje **Ik wil dit domein configureren voor eenmalige aanmelding met mijn lokale Active Directory** in.
5. Selecteer **Toevoegen**.

Voer het hulpprogramma Azure AD Connect uit om de DNS-vermelding te verkrijgen die door Azure AD wordt gebruikt om het domein te verifiëren. U ziet de DNS-vermelding in de stap **Azure AD-domein** van de wizard. [In deze instructies](connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) ziet u hoe die stap van de wizard eruitziet. Als u het hulpprogramma Azure AD Connect niet hebt, kunt u het [hier](http://go.microsoft.com/fwlink/?LinkId=615771) downloaden.

<a id="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain" class="xliff"></a>

## De DNS-vermelding op de domeinnaamregistrar voor het domein toevoegen
De volgende stap voor het gebruik van de aangepaste domeinnaam met Azure AD bestaat uit het bijwerken van het DNS-zonebestand voor het domein. Hierdoor kan Azure AD controleren of uw organisatie eigenaar is van de aangepaste domeinnaam.

1. Meld u aan bij de website van de domeinnaamregistrar voor uw domeinnaam. Als u niet gemachtigd bent om dit te doen, vraagt u de persoon die of het team dat in uw organisatie deze toegang heeft, om stap 2 uit te voeren en u te laten weten wanneer deze is voltooid.
2. Werk het DNS-zonebestand voor het domein bij, door de DNS-vermelding toe te voegen die u van Azure AD hebt ontvangen. Met deze DNS-vermelding kan Azure AD controleren of u eigenaar bent van het domein. De DNS-vermelding leidt niet tot veranderingen in het gedrag van bijvoorbeeld mailroutering of webhosting.

Voor hulp bij deze stap leest u de [Instructies voor het toevoegen van een DNS-vermelding bij populaire DNS-registrars](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

<a id="verify-the-domain-name-with-azure-ad" class="xliff"></a>

## De domeinnaam verifiëren met Azure AD
Nadat u de DNS-vermelding hebt toegevoegd, kunt u de domeinnaam bij Azure AD verifiëren.

Als u het domein wilt verifiëren, selecteert u **Volgende** in de stap **Azure AD-domein** van de Azure AD Connect-wizard. Azure AD zoekt de DNS-vermelding in het DNS-zonebestand voor het domein. Azure AD controleert de domeinnaam alleen wanneer de DNS-records zijn doorgegeven. Doorgifte duurt vaak slechts enkele seconden, maar het kan ook een uur of langer duren. Als de verificatie de eerste keer niet werkt, probeer het dan later nog eens.

Vervolgens gaat u verder met de overige stappen van de Azure AD Connect-wizard. Hiermee worden gebruikers vanuit de Windows Server AD gesynchroniseerd met Azure AD. Gesynchroniseerde gebruikers in het domein dat u voor federatie hebt geconfigureerd, kunnen een federatieve ervaring voor eenmalige aanmelding bij Azure AD vanuit het bedrijfsnetwerk verkrijgen.

<a id="troubleshooting" class="xliff"></a>

## Problemen oplossen
Als u een aangepaste domeinnaam niet kunt verifiëren, probeert u het volgende. We beginnen met de meest voorkomende en werken de lijst af naar de minst voorkomende.

1. **Wacht een uur**. DNS-records moeten zijn doorgegeven voordat Azure AD het domein kan verifiëren. Dit kan een uur of langer duren.
2. **Controleer of de DNS-record is opgegeven en of deze juist is**. Voer deze stap uit op de website van de domeinnaamregistrar voor het domein. Azure AD kan de domeinnaam niet verifiëren als de DNS-vermelding niet aanwezig is in het DNS-zonebestand of als deze niet exact overeenkomt met de DNS-vermelding die u van Azure AD hebt gekregen. Als u geen toegang hebt tot de site van de domeinnaamregistrar om de DNS-records voor het domein bij te werken, deel de DNS-vermelding dan met de persoon die of het team dat in uw organisatie deze toegang heeft en vraag om de DNS-vermelding toe te voegen.
3. **Verwijder de domeinnaam uit andere mappen in Azure AD**. Een domeinnaam kan maar in één map worden geverifieerd. Als een domeinnaam eerder is geverifieerd in een andere map, moet de domeinnaam daar eerst uit worden verwijderd voordat deze kan worden geverifieerd in een nieuwe map. Zie [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md) voor meer informatie over het verwijderen van domeinnamen.

<a id="add-more-custom-domain-names" class="xliff"></a>

## Meer aangepaste domeinnamen toevoegen
Als uw organisatie meerdere aangepaste domeinnamen gebruikt, zoals contoso.com en contosobank.com, kunt u tot maximaal 900 domeinnamen toevoegen. Gebruik dezelfde stappen in dit artikel om elke domeinnaam toe te voegen.

<a id="next-steps" class="xliff"></a>

## Volgende stappen
* [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)
* [Meer informatie over concepten met betrekking tot domeinbeheer in Azure AD](active-directory-add-domain-concepts.md)
* [De huisstijl van uw bedrijf weergeven wanneer uw gebruikers zich aanmelden](active-directory-add-company-branding.md)
* [PowerShell gebruiken voor het beheer van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)


