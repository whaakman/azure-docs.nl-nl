
<properties
    pageTitle="Azure AD Connect Health gebruiken met AD DS | Microsoft Azure"
    description="Dit is de Azure AD Connect Health-pagina waarop wordt besproken hoe AD DS kunt controleren."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="arluca"/>

# Azure AD Connect Health gebruiken met AD DS
De volgende documentatie is specifiek voor het controleren van Active Directory Domain Services met Azure AD Connect Health. Dit geldt ook voor AD DS geïnstalleerd op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.

Zie [Using Azure AD Connect Health with AD FS](active-directory-aadconnect-health-adfs.md) (Engelstalig) voor informatie over het controleren van AD FS met Azure AD Connect Health. Zie [Using Azure AD Connect Health for Sync](active-directory-aadconnect-health-sync.md) voor informatie over het controleren van AD Connect (Sync) met Azure AD Connect Health.

![Azure AD Connect Health voor AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## Waarschuwingen voor Azure AD Connect Health voor AD DS
De sectie waarschuwingen in Azure AD Connect Health voor AD DS biedt u een lijst met actieve en opgeloste waarschuwingen die betrekking hebben op uw domeincontrollers. Bij het selecteren van een actieve of opgeloste waarschuwing wordt een nieuwe blade geopend met aanvullende informatie, samen met stappen om het probleem op te lossen en koppelingen naar ondersteunende documentatie. Elk type waarschuwing kan een of meer exemplaren hebben die overeenstemmen met elk van de domeincontrollers die betrekking hebben op die waarschuwing. Aan de onderkant van de waarschuwingsblade, kunt u betrokken domeincontrollers selecteren en een nieuwe blad wordt geopend met aanvullende informatie over dat specifieke waarschuwingsexemplaar.

Inschakelen van e-mailmeldingen voor waarschuwingen is beschikbaar in deze blade, evenals het wijzigen van het tijdsbereik in deze weergave. Door het tijdsbereik uit te breiden kunt u eerder opgeloste waarschuwingen weergegeven.

![Azure AD Connect-synchronisatiefout](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## Domeincontrollers
Dit dashboard biedt een topologische weergave van uw omgeving, samen met belangrijke operationele statistieken en de status van elk van de bewaakte domeincontrollers. De aangeboden metrische gegevens helpen om snel alle domeincontrollers die mogelijk verder onderzocht moeten worden te identificeren. Alleen een subset van kolommen worden standaard weergegeven. Door te klikken op de opdrachtkolommen ziet u echter de gehele set kolommen die er zijn. Door de voor u meest interessante kolommen te selecteren wordt dit dashboard een handige plek om de status van uw AD DS-omgeving te bekijken. 

![Domeincontrollers](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Domeincontrollers kunnen worden gegroepeerd op hun respectievelijke domein of de site, wat nuttig is voor het begrijpen van de topologie van de omgeving. Als u ten slotte dubbelklikt u op de bladekop, wordt het dashboard gemaximaliseerd om de beschikbare schermruimte te gebruiken. Dit kan vooral nuttig zijn wanneer meerdere kolommen worden weergegeven. 

## Replicatiestatus
Dit dashboard geeft een overzicht van de replicatiestatus en -topologie van uw bewaakte domeincontrollers. De status van de meest recente replicatiepoging wordt weergegeven, samen met nuttige documentatie voor elke fout die is gevonden. Bij het selecteren van domeincontroller met een fout wordt een nieuwe blade geopend met aanvullende informatie, samen met stappen om het probleem op te lossen en koppelingen naar ondersteunende documentatie. 

![Replicatiestatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## Bewaking
Deze functie biedt grafische trends van verschillende prestatiemeters, die voortdurend worden verzameld van elke bewaakte domeincontroller. Prestaties van een domeincontroller kunnen eenvoudig worden vergeleken met andere bewaakte domeincontrollers in uw forest. Bovendien ziet u verschillende prestatiemeters naast elkaar, dit is nuttig bij probleemoplossing in uw omgeving. 

![Bewaking](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Standaard hebben we vier prestatiemeters vooraf geselecteerd; u kunt echter andere meters opnemen door op de filteropdracht te klikken en de gewenste meters te (de)selecteren. Als u bovendien klikt op de grafiek van een prestatiemeter, wordt een nieuwe blade geopend. Deze bevat de respectievelijke datapunten voor elke bewaakte domeincontroller.

## Verwante koppelingen

* [Azure AD Connect Health (Engelstalig)](active-directory-aadconnect-health.md)
* [De Azure AD Connect Health-agent installeren](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-bewerkingen](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md)
* [Veelgestelde vragen over Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=ago16_HO4-->


