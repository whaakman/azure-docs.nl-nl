<properties
    pageTitle="Uw aangepaste domeinnaam toevoegen aan Azure Active Directory | Microsoft Azure"
    description="Hier leest u hoe u de domeinnamen van uw bedrijf kunt toevoegen aan Azure Active Directory en hoe u de domeinnaam kunt verifiëren."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/18/2016"
    ms.author="curtand;jeffsta"/>

# Een aangepaste domeinnaam toevoegen aan Azure Active Directory

U hebt een of meer domeinnamen die door uw organisatie worden gebruikt voor zakelijke activiteiten en uw gebruikers melden zich aan bij uw bedrijfsnetwerk via uw bedrijfsdomeinnaam. Nu u Azure Active Directory (Azure AD) gebruikt, kunt u uw zakelijke domeinnaam ook toevoegen aan Azure AD. U kunt dan gebruikersnamen toewijzen in de directory die bekend is bij uw gebruikers, zoals 'gwenda@contoso.com'. Het proces is eenvoudig:

1. Voeg de aangepaste domeinnaam toe aan uw directory.
2. Voeg een DNS-vermelding voor de domeinnaam toe aan de domeinnaamregistrar.
3. Verifieer de aangepaste domeinnaam in Azure AD.

> [AZURE.NOTE] Als u van plan bent uw aangepaste domeinnaam te configureren voor gebruik met Active Directory Federation Services (AD FS) of een andere beveiligingstokenservice op uw bedrijfsnetwerk, volgt u de instructies in [Add and configure a domain for federation with Azure Active Directory](active-directory-add-domain-federated.md) (Een domein toevoegen en configureren voor federatie met Azure Active Directory). Dit is handig als u van plan bent gebruikers uit uw bedrijfsdirectory te synchroniseren met Azure AD en [wachtwoordhashsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md) niet aan uw vereisten voldoet.

## Een aangepaste domeinnaam toevoegen aan uw directory

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com/) met een gebruikersaccount met de rechten voor globale beheerder van uw Azure AD-directory.

2. Open in **Active Directory** uw directory en selecteer de tab **Domeinen**.

3. Selecteer op de opdrachtbalk **Toevoegen**. Voer de naam van uw aangepaste domein in, bijvoorbeeld 'contoso.com'. Zorg ervoor dat u .com, .net of een andere extensie op het hoogste niveau in de naam opneemt en laat het selectievakje voor 'eenmalige aanmelding' (federatie) uitgeschakeld.

4. Selecteer **Toevoegen**.

5. Op de tweede pagina van de wizard Domein toevoegen vindt u de DNS-vermelding die door Azure AD wordt gebruikt om te verifiëren of uw organisatie eigenaar is van de aangepaste domeinnaam.

Nu u een domeinnaam hebt toegevoegd, moet met Azure AD worden gecontroleerd of uw organisatie de eigenaar is van de domeinnaam. Voordat deze verificatie met Azure AD kan worden uitgevoerd, moet u een DNS-vermelding toevoegen in het DNS-zonebestand voor de domeinnaam. Deze taak wordt voor de domeinnaam uitgevoerd op de website voor domeinnaamregistrar.

## De DNS-vermelding op de domeinnaamregistrar voor het domein toevoegen

De volgende stap voor het gebruik van de aangepaste domeinnaam met Azure AD bestaat uit het bijwerken van het DNS-zonebestand voor het domein. Hierdoor kan Azure AD verifiëren of uw organisatie eigenaar is van de aangepaste domeinnaam.

1.  Meld u aan bij de domeinnaamregistrar voor het domein. Als u geen toegang hebt om de DNS-vermelding bij te werken, vraagt u de persoon die of het team dat wel over deze toegang beschikt om stap 2 uit te voeren en u te laten weten wanneer deze is voltooid.

2.  Werk het DNS-zonebestand voor het domein bij, door de DNS-vermelding toe te voegen die u van Azure AD hebt ontvangen. Met deze DNS-vermelding kan Azure AD controleren of u eigenaar bent van het domein. De DNS-vermelding leidt niet tot veranderingen in het gedrag van bijvoorbeeld mailroutering of webhosting.

Voor hulp bij het toevoegen van de DMS-vermelding leest u de [Instructies voor het toevoegen van een DNS-vermelding bij populaire DNS-registrars](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

## De domeinnaam verifiëren met Azure AD

Als u de DNS-vermelding hebt toegevoegd, kunt u de domeinnaam bij Azure AD verifiëren.

Als u de wizard **Domein toevoegen** nog geopend hebt, selecteert u **Verifiëren** in het derde venster van de wizard. Wanneer u **Verifiëren** selecteert, zoekt Azure AD de DNS-vermelding in het DNS-zonebestand voor het domein. Azure AD kan de domeinnaam alleen verifiëren nadat de DNS-records zijn doorgegeven. Deze doorgifte duurt vaak slechts enkele seconden, maar het kan ook wel eens een uur of langer duren. Als verificatie de eerste keer niet werkt, probeer het dan later nog eens.

Als de wizard **Domein toevoegen** niet meer is geopend, kunt u het domein verifiëren in de [klassieke Azure-portal](https://manage.windowsazure.com/):

1.  Meld u aan met een gebruikersaccount met de rechten voor globale beheerder van uw Azure AD-directory.

2.  Open uw directory en selecteer de tab **Domeinen**.

3.  Selecteer de domeinnaam die u wilt verifiëren en selecteer **Verifiëren** op de opdrachtbalk.

4. Selecteer **Verifiëren** in het dialoogvenster om de verificatie te voltooien.

Nu kunt u [gebruikersnamen toewijzen die uw aangepaste domeinnaam omvatten](active-directory-add-domain-add-users.md).

## Problemen oplossen

Als u een aangepaste domeinnaam niet kunt verifiëren, probeert u het volgende. We beginnen met de meest voorkomende en werken de lijst af naar de minst voorkomende.

1.  **Wacht een uur**. DNS-records moeten zijn doorgegeven voordat Azure AD het domein kan verifiëren. Dit kan een uur of langer duren.

2.  **Controleer of de DNS-record is opgegeven en of deze juist is**. Voer deze stap uit op de website voor de domeinnaamregistrar voor het domein. Azure AD kan de domeinnaam niet verifiëren als de DNS-vermelding niet aanwezig is in het DNS-zonebestand of als deze niet exact overeenkomt met de DNS-vermelding die u van Azure AD hebt gekregen. Als u geen toegang hebt om DNS-records bij te werken voor het domein op de domeinnaamregistrar, deel de DNS-vermelding dan met de persoon die of het team dat deze toegang in uw organisatie heeft en vraag om de DNS-vermelding toe te voegen.

3.  **Verwijder de domeinnaam vanuit een andere directory in Azure AD**. Een domeinnaam kan maar in één directory worden geverifieerd . Als een domeinnaam eerder is geverifieerd in een andere directory, moet deze daar eerst worden verwijderd voordat deze kan worden geverifieerd in een nieuwe directory. Zie [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md) voor meer informatie over het verwijderen van domeinnamen.


## Meer aangepaste domeinnamen toevoegen

Als uw organisatie meerdere aangepaste domeinnamen gebruikt, zoals 'contoso.com' en 'contosobank.com', kunt u tot maximaal 900 domeinnamen toevoegen. Gebruik dezelfde stappen in dit artikel om elke domeinnaam toe te voegen.

## Volgende stappen

-   [Gebruikersnamen toewijzen die uw aangepaste domeinnaam omvatten](active-directory-add-domain-add-users.md)
-   [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)
-   [Meer informatie over concepten wat betreft domeinbeheer in Azure AD](active-directory-add-domain-concepts.md)
-   [De huisstijl van uw bedrijf weergeven wanneer uw gebruikers zich aanmelden](active-directory-add-company-branding.md)
-   [PowerShell gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)



<!--HONumber=ago16_HO4-->


