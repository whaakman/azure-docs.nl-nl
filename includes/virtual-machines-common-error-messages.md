---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: genlin
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/25/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: db241c1a3c8bfd15e13ae0bd9f1cdf4c92c7081d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
>[!NOTE]
> U kunt opmerkingen laten op deze pagina voor feedback of via [Azure feedback](https://feedback.azure.com/forums/216843-virtual-machines) met #azerrormessage-tag.

## <a name="error-response-format"></a>Fout antwoord-indeling 
Virtuele machines in Azure gebruiken de volgende JSON-indeling voor foutbericht:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Reactie op een fout bevat altijd een statuscode en een error-object. Elk foutobject bevat altijd een foutcode en een bericht. Als de virtuele machine wordt gemaakt met een sjabloon, bevat het foutobject ook een sectie voor informatie die een binnenste niveau van foutcodes en het bericht bevat. Het meest binnenste niveau van het foutbericht is normaal gesproken de basis-fout.


## <a name="common-virtual-machine-management-errors"></a>Algemene fouten voor het beheer van virtuele machine

Deze sectie vindt u de algemene foutberichten die optreden kunnen bij het beheren van virtuele machines:

|  Foutcode  |  Foutbericht  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Lease verkrijgen bij het maken van schijf kan niet '{0}' met blob met URI {1}. BLOB wordt al gebruikt.  |  
|  AllocationFailed  |  Toewijzing is mislukt. Verminder de VM-grootte of het aantal virtuele machines, probeer het later opnieuw of probeer te implementeren naar een andere Beschikbaarheidsset of andere Azure-locatie.  |  
|  AllocationFailed  |  De VM-toewijzing is mislukt vanwege een interne fout. Probeer het later opnieuw of probeer te implementeren naar een andere locatie.  |
|  ArtifactNotFound  |  De VM-extensie met uitgever '{0}en type{1}'kan niet worden gevonden op locatie'{2}'.  |
|  ArtifactNotFound  |  Extensie met uitgever '{0}', type '{1}', en type handlerversie '{2}' is niet gevonden in de extensieopslagplaats.  |
|  ArtifactVersionNotFound  |  Geen versie gevonden in de artefactopslagplaats die overeenkomt met de gevraagde versie '{0}'.  |
|  ArtifactVersionNotFound  |  Geen versie gevonden in de artefactopslagplaats die overeenkomt met de gevraagde versie '{0}'voor VM-extensie met uitgever'{1}en type{2}'.  |
|  AttachDiskWhileBeingDetached  |  Kan geen gegevensschijf koppelen '{0}'naar de virtuele machine'{1}' omdat de schijf is op dit moment wordt losgekoppeld. Wacht totdat de schijf volledig losgekoppeld is en probeer het opnieuw.  |
|  BadRequest  |  Uitgelijnd ' Beschikbaarheidssets worden nog niet ondersteund in deze regio.  |
|  BadRequest  |  Toevoeging van een virtuele machine met beheerde-schijven voor niet-beheerde Beschikbaarheidsset of toevoeging van een virtuele machine met blobs gebaseerde schijven aan beheerde Beschikbaarheidsset wordt niet ondersteund. Maak een Beschikbaarheidsset met 'beheerde'-eigenschap is ingesteld om een virtuele machine met beheerde schijven aan toe te voegen.  |
|  BadRequest  |  Beheerde schijven worden niet ondersteund voor deze regio.  |
|  BadRequest  |  Meerdere VM-extensies per handler worden niet ondersteund voor OS-type '{0}'. VMExtension '{1}'met de handler'{2}is al toegevoegd of opgegeven in de invoer.  |
|  BadRequest  |  Bewerking '{0}'wordt niet ondersteund voor Resource'{1}' met beheerde-schijven.  |
|  CertificateImproperlyFormatted  |  De JSON-weergave van het geheim die is opgehaald uit {0} bevat een gegevensveld dat niet een onjuist geformatteerd PFX-bestand of het opgegeven wachtwoord het PFX-bestand niet correct decoderen.  |
|  CertificateImproperlyFormatted  |  De gegevens uit de {0} is niet opnieuw worden geserialiseerd naar JSON.  |
|  Conflict  |  Het wijzigen van de schijfgrootte is alleen toegestaan tijdens het maken van een VM of wanneer de toewijzing van de VM ongedaan wordt gemaakt.  |
|  ConflictingUserInput  |  Schijf '{0}'kan niet worden gekoppeld omdat de schijf is al het eigendom van de virtuele machine'{1}'.  |
|  ConflictingUserInput  |  De bron- en doelresourcegroep zijn dezelfde.  |
|  ConflictingUserInput  |  Bron- en doelserver opslagaccounts voor schijf {0} zijn verschillend.  |
|  ContainerAlreadyOnLease  |  Er is al een lease op de opslagcontainer die de blob met URI {0}.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  De aanvraag voor het verplaatsen van resources bevat KeyVault-resources waarnaar wordt verwezen door een of meer {0}s in de aanvraag. Dit wordt niet wordt momenteel ondersteund kruislingse verplaatsen. Controleer de foutdetails voor de KeyVault-resource-id.  |
|  DiagnosticsOperationInternalError  |  Een interne fout opgetreden tijdens het verwerken van diagnostische profiel van VM {0}.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  BLOB {0} wordt al gebruikt door een andere schijf die behoort tot VM '{1}'. U kunt de blobmetagegevens voor de schijfverwijzingsgegevens bekijken.  |
|  DiskBlobNotFound  |  Kan niet worden gevonden, VHD-blob met URI {0} voor schijf '{1}'.  |
|  DiskBlobNotFound  |  Kan niet worden gevonden, VHD-blob met URI {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} geheim beschikt niet over de {1} labels. Werk de geheime versie, de vereiste labels toevoegen en probeer het opnieuw.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Uitpakken van de geheime {0} waarde met sleutel {1} is mislukt.  |
|  DiskImageNotReady  |  Schijfimage {0} bevindt zich in {1} staat. Probeer het opnieuw wanneer de installatiekopie is nu klaar.  |
|  DiskPreparationError  |  Een of meer fouten opgetreden tijdens het voorbereiden van VM-schijven. Zie schijf exemplaar weergeven voor meer informatie.  |
|  DiskProcessingError  |  Het verwerken van de schijf is gestopt omdat de virtuele machine andere schijven heeft in niet-werkende schijven heeft.  |
|  ImageBlobNotFound  |  Kan niet worden gevonden, VHD-blob met URI {0} voor schijf '{1}'.  |
|  ImageBlobNotFound  |  Kan niet worden gevonden, VHD-blob met URI {0}.  |
|  IncorrectDiskBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} voor schijf '{1}' is van het type blok-blob.  |
|  IncorrectDiskBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} is van het type '{1}'.  |
|  IncorrectImageBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} voor schijf '{1}' is van het type blok-blob.  |
|  IncorrectImageBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} is van het type '{1}'.  |
|  InternalOperationError  |  Storage-account kan niet worden omgezet {0}. Zorg ervoor dat deze is gemaakt via de Storage Resource Provider op dezelfde locatie als de rekenbron.  |
|  InternalOperationError  |  {0} taken voor Doelzoeken zijn mislukt.  |
|  InternalOperationError  |  Er is een fout opgetreden bij het valideren van het netwerkprofiel van VM '{0}'.  |
|  InvalidAccountType  |  Het AccountType {0} is ongeldig.  |
|  InvalidParameter  |  De waarde van parameter {0} is ongeldig.  |
|  InvalidParameter  |  Het opgegeven beheerderswachtwoord is niet toegestaan.  |
|  InvalidParameter  |  ' Het opgegeven wachtwoord moet tussen {0}-{1} tekens lang en moet ten minste voldoen aan {2} van vereisten voor wachtwoordcomplexiteit uit de volgende opties: <ol><li> Bevat een hoofdletter</li><li>Bevat een kleine letter</li><li>Bevat een numerieke getal</li><li>Bevat een speciaal teken.</li></ol>  |
|  InvalidParameter  |  De opgegeven gebruikersnaam voor een beheerder is niet toegestaan.  |
|  InvalidParameter  |  Kan een bestaande OS-schijf niet koppelen als de VM is gemaakt op basis van een installatiekopie van een platform of gebruiker.  |
|  InvalidParameter  |  Containernaam {0} is ongeldig. Containernamen moeten 3-63 tekens lang zijn en mag alleen kleine alfanumerieke tekens en afbreekstreepjes bevatten. Afbreekstreepje moet worden voorafgegaan en gevolgd door een alfanumeriek teken.  |
|  InvalidParameter  |  Containernaam {0} in URL {1} is ongeldig. Containernamen moeten 3-63 tekens lang zijn en mag alleen kleine alfanumerieke tekens en afbreekstreepjes bevatten. Afbreekstreepje moet worden voorafgegaan en gevolgd door een alfanumeriek teken.  |
|  InvalidParameter  |  De blobnaam in URL {0} bevat een slash. Dit wordt momenteel niet ondersteund voor schijven.  |
|  InvalidParameter  |  De URI {0} lijkt niet de juiste BLOB-URI.  |
|  InvalidParameter  |  Een schijf met de naam '{0}' wordt al gebruikt hetzelfde LUN: {1}.  |
|  InvalidParameter  |  Een schijf met de naam '{0}' bestaat al.  |
|  InvalidParameter  |  Er kan niet worden opgegeven dat de gebruikersinstallatiekopie moet worden overschreven voor een schijf die al is gedefinieerd in de opgegeven installatiekopieverwijzing.  |
|  InvalidParameter  |  Een schijf met de naam '{0}' wordt al gebruikt dezelfde VHD URL {1}.  |
|  InvalidParameter  |  Het opgegeven aantal foutdomeinen {0} in het bereik moeten vallen {1} naar {2}.  |
|  InvalidParameter  |  Het licentietype {0} is ongeldig. Geldige licentietypen zijn: Windows_Client of Windows_Server, hoofdlettergevoelig.  |
|  InvalidParameter  |  Linux-hostnaam mag niet meer dan {0} tekens of de volgende tekens bevatten: {1}.  |
|  InvalidParameter  |  Doelpad voor openbare Ssh-sleutels is momenteel beperkt tot de standaardwaarde {0} vanwege een bekend probleem in Linux-inrichtingsagent.  |
|  InvalidParameter  |  Een schijf voor LUN {0} bestaat al.  |
|  InvalidParameter  |  Abonnement {0} van de aanvraag moet overeenkomen met het {1} opgenomen in de beheerde schijf-id.  |
|  InvalidParameter  |  Aangepaste gegevens in OSProfile moeten in Base64-codering en met een maximale lengte van {0} tekens.  |
|  InvalidParameter  |  BLOB-naam in URL {0} moet eindigen met '{1}' extensie.  |
|  InvalidParameter  |  {0}' is niet een geldig voorvoegsel van het vastgelegde VHD-blob. Een geldig voorvoegsel komt overeen met reguliere expressie '{1}'.  |
|  InvalidParameter  |  Certificaten kunnen niet worden toegevoegd aan uw virtuele machine als de VM-agent niet is ingericht.  |
|  InvalidParameter  |  Een schijf voor LUN {0} bestaat al.  |
|  InvalidParameter  |  Kan de virtuele machine maken omdat de aangevraagde grootte {0} is niet beschikbaar in het cluster waar de beschikbaarheidsset momenteel is toegewezen. De beschikbare grootten zijn: {1}. Lees meer over VM vergroten of verkleinen strategy op https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  De aangevraagde VM-grootte {0} is niet beschikbaar in de huidige regio. De grootten die beschikbaar zijn in het huidige gebied zijn: {1}. Meer informatie over de beschikbare grootten voor virtuele machine in elke regio op https://aka.ms/azure-regions.  |
|  InvalidParameter  |  De aangevraagde VM-grootte {0} is niet beschikbaar in de huidige regio. Meer informatie over de beschikbare grootten voor virtuele machine in elke regio op https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Windows-beheerdersgebruikersnaam mag niet meer dan {0} tekens lang, eindigen met een period(.) of de volgende tekens bevatten: {1}.  |
|  InvalidParameter  |  Windows-computernaam mag niet meer dan {0} tekens lang, uitsluitend uit cijfers bestaan, of de volgende tekens bevatten: {1}.  |
|  MissingMoveDependentResources  |  De aanvraag voor het verplaatsen van resources bevat niet alle afhankelijke resources. Controleer de foutdetails op ontbrekende resource-id.  |
|  MoveResourcesHaveInvalidState  |  De aanvraag Resources verplaatsen bevat virtuele machines die gekoppeld met ongeldige storage-accounts zijn. Controleer de details voor deze resource-id en de namen van opslagaccounts waarnaar wordt verwezen.  |
|  MoveResourcesHavePendingOperations  |  De aanvraag voor het verplaatsen van resources bevat resources waarvoor een bewerking in behandeling is. Controleer de details voor deze resource-id. Probeer de bewerking opnieuw nadat de bewerkingen in behandeling is voltooid.  |
|  MoveResourcesNotFound  |  De aanvraag voor het verplaatsen van resources bevat resources die niet is gevonden. Controleer de details voor deze resource-id.  |
|  NetworkingInternalOperationError  |  Onbekende netwerktoewijzingsfout.  |
|  NetworkingInternalOperationError  |  Onbekende netwerktoewijzingsfout  |
|  NetworkingInternalOperationError  |  Er is een interne fout opgetreden tijdens het verwerken van het netwerkprofiel van de VM.  |
|  Niet gevonden  |  De Beschikbaarheidsset {0} kan niet worden gevonden.  |
|  Niet gevonden  |  Bron van de virtuele Machine '{0}' opgegeven in de aanvraag bestaat niet in deze Azure-locatie.  |
|  Niet gevonden  |  Tenant met id {0} niet gevonden.  |
|  Niet gevonden  |  De installatiekopie van het {0} kan niet worden gevonden.  |
|  NotSupported  |  Het licentietype is {0}, maar de blob voor installatiekopie {1} is niet afkomstig van on-premises.  |
|  OperationNotAllowed  |  Beschikbaarheidsset {0} kan niet worden verwijderd. Voordat u verwijdert een Beschikbaarheidsset Zorg ervoor dat het heeft geen VM bevat.  |
|  OperationNotAllowed  |  Het wijzigen van de beschikbaarheidsset is SKU van 'Uitgelijnd' naar 'Classic' niet toegestaan.  |
|  OperationNotAllowed  |  Kan geen extensies wijzigen in de VM wanneer de VM niet wordt uitgevoerd.  |
|  OperationNotAllowed  |  De actie vastleggen wordt alleen ondersteund op een virtuele Machine met blobs gebaseerde schijven. Gebruik de resource 'Image' API's om een installatiekopie maken van een beheerde virtuele Machine.  |
|  OperationNotAllowed  |  De resource {0} kan niet worden gemaakt van de installatiekopie {1} totdat de installatiekopie is gemaakt.  |
|  OperationNotAllowed  |  Bijwerken van encryptionsettings is niet toegestaan wanneer de VM is toegewezen, probeer het opnieuw nadat de toewijzing van VM ongedaan is gemaakt  |
|  OperationNotAllowed  |  Het toevoegen van een beheerde schijf aan een VM met blobschijven wordt niet ondersteund.  |
|  OperationNotAllowed  |  Het maximum aantal gegevensschijven dat mag worden gekoppeld aan een VM van deze omvang is {0}.  |
|  OperationNotAllowed  |  Het toevoegen van een blobschijf aan een VM met beheerde schijven wordt niet ondersteund  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op de installatiekopie'{1}' omdat de installatiekopie is gemarkeerd voor verwijdering. U kunt alleen de Delete-bewerking opnieuw proberen (of wacht u totdat er een om te voltooien).  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de VM is gegeneraliseerd.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan als terugzetten punt verzameling'{1}' is gemarkeerd voor verwijdering.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM-extensie'{1}' omdat deze is gemarkeerd voor verwijdering. U kunt alleen de Delete-bewerking opnieuw proberen (of wacht u totdat er een om te voltooien).  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan omdat de virtuele Machines'{1}'zijn ingericht met behulp van de installatiekopie van het'{2}'.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan omdat de virtuele Machine ScaleSet'{1}'momenteel wordt gebruikt door de installatiekopie van het'{2}'.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de virtuele machine is gemarkeerd voor verwijdering. U kunt alleen de Delete-bewerking opnieuw proberen (of wacht u totdat er een om te voltooien).  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de VM ongedaan is gemaakt of gemarkeerd voor de toewijzing ongedaan worden gemaakt.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de VM wordt uitgevoerd. Geef power uitschakelen expliciet als u de virtuele machine uit binnen het gastbesturingssysteem afsluiten.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de virtuele machine wordt niet vrijgegeven.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}'Omdat de VM heeft een extensie'{2}' in de status is mislukt.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat een andere bewerking uitgevoerd wordt.  |
|  OperationNotAllowed  |  De bewerking '{0}'vereist dat de virtuele Machine'{1}' om te worden gegeneraliseerd.  |
|  OperationNotAllowed  |  Voor de bewerking is het vereist dat de VM wordt uitgevoerd (of is ingesteld om te worden uitgevoerd).  |
|  OperationNotAllowed  |  De schijf met een grootte {0}GB die kleiner is dan de grootte {1}GB van de bijbehorende schijf in de installatiekopie, is niet toegestaan.  |
|  OperationNotAllowed  |  VM-Schaalset extensies van handler '{0}' kan alleen op het moment van het maken van VM-Schaalset worden toegevoegd.  |
|  OperationNotAllowed  |  VM-Schaalset extensies van handler '{0}' kan alleen op het moment van VM-Schaalset verwijderen worden verwijderd.  |
|  OperationNotAllowed  |  Virtuele machine '{0}' wordt al gebruikt door beheerde schijven.  |
|  OperationNotAllowed  |  Virtuele machine '{0}'behoort tot de beschikbaarheidsset 'Classic' '{1}'. Werk de beschikbaarheidsset 'Uitgelijnd' SKU gebruiken en probeer het vervolgens opnieuw de conversie.  |
|  OperationNotAllowed  |  VM gemaakt op basis van de installatiekopie kan geen BLOBs gebaseerde schijven hebben. Alle schijven moeten beheerde schijven.  |
|  OperationNotAllowed  |  Opnamebewerking kan niet worden voltooid omdat de VM niet is gegeneraliseerd.  |
|  OperationNotAllowed  |  Beheerbewerkingen op virtuele machine '{0}' zijn niet toegestaan omdat het VM-schijven worden geconverteerd naar beheerde schijven.  |
|  OperationNotAllowed  |  Er een bewerking voedingsstatus van de virtuele Machine wordt gewijzigd {0} naar {1}. Voer de bewerking {2} na enige tijd.  |
|  OperationNotAllowed  |  Kan niet toevoegen of bijwerken van de virtuele machine. De aangevraagde VM-grootte {0} mogelijk niet beschikbaar in het bestaande cluster. Lees meer over VM vergroten of verkleinen strategy op https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Kan het formaat van de virtuele machine omdat de aangevraagde grootte {0} is niet beschikbaar in het cluster waar de beschikbaarheidsset momenteel is toegewezen. De beschikbare grootten zijn: {1}. Lees meer over VM vergroten of verkleinen strategy op https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Kan het formaat van de virtuele machine omdat de aangevraagde grootte {0} is niet beschikbaar in het cluster waarop de virtuele machine momenteel is toegewezen. Het formaat van uw virtuele machine naar {1} Neem ongedaan gemaakt (dit is Stop-bewerking in de Azure portal) en probeer het formaat van de bewerking opnieuw. Lees meer over VM vergroten of verkleinen strategy op https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  Besturingssysteem inrichten is mislukt voor de virtuele machine '{0}' omdat het gastbesturingssysteem momenteel wordt ingericht.  |
|  OSProvisioningClientError  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is mislukt. Details van fout: {1} Zorg ervoor dat de installatiekopie juist is voorbereid (gegeneraliseerd). <ul><li>Instructies voor Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Sleutel genereren voor SSH-host is mislukt. Details van fout: {0}. U lost dit probleem controleren als Linux-agent correct is ingesteld. <ul><li>U kunt de instructies op controleren: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Gebruikersnaam is opgegeven voor de virtuele machine is ongeldig voor deze Linux-distributie. Details van fout: {0}.  |
|  OSProvisioningInternalError  |  Besturingssysteem inrichten is mislukt voor de virtuele machine '{0}' vanwege een interne fout.  |
|  OSProvisioningTimedOut  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is niet voltooid binnen de toegewezen tijd. Inrichting mogelijk alsnog voltooien door de virtuele machine. Controleer de Inrichtingsstatus later.  |
|  OSProvisioningTimedOut  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is niet voltooid binnen de toegewezen tijd. Inrichting mogelijk alsnog voltooien door de virtuele machine. Controleer de Inrichtingsstatus later. Controleer ook of de installatiekopie juist is voorbereid (gegeneraliseerd).   <ul><li>Instructies voor Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instructies voor Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is niet voltooid binnen de toegewezen tijd. Echter, de VM-gastagent is uitgevoerd. Dit kan erop wijzen het gastbesturingssysteem is niet correct voorbereid om te worden gebruikt als een VM-installatiekopie (CreateOption = FromImage). U lost dit probleem, gebruikt u de VHD met CreateOption = koppelen of juist voorbereiden voor gebruik als een installatiekopie:   <ul><li>Instructies voor Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instructies voor Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  De vereiste VM-grootte is momenteel niet beschikbaar op de geselecteerde locatie.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Resource kan niet worden bijgewerkt op dit moment vanwege een lopende platformupdate. Probeer het later opnieuw.  |
|  StorageAccountLimitation  |  Storage-account '{0}' biedt geen ondersteuning voor pagina-blobs die zijn vereist om schijven te maken.  |
|  StorageAccountLimitation  |  Storage-account '{0}' het toegewezen quotum is overschreden.  |
|  StorageAccountLocationMismatch  |  Storage-account kan niet worden omgezet {0}. Zorg ervoor dat deze is gemaakt via de Storage Resource Provider op dezelfde locatie als de rekenbron.  |
|  StorageAccountNotFound  |  Storage-account {0} niet gevonden. Zorg ervoor dat opslagaccount wordt niet verwijderd en behoort tot dezelfde Azure-locatie als de virtuele machine.  |
|  StorageAccountNotRecognized  |  Gebruik een opslagaccount worden beheerd door Storage Resource Provider. Het gebruik van {0} wordt niet ondersteund.  |
|  StorageAccountOperationInternalError  |  Er is een interne fout opgetreden tijdens het openen van opslagaccount {0}.  |
|  StorageAccountSubscriptionMismatch  |  Storage-account {0} behoort niet tot abonnement {1}.  |
|  StorageAccountTooBusy  |  Storage-account '{0}' is momenteel bezet. Overweeg het gebruik van een ander account.  |
|  StorageAccountTypeNotSupported  |  Schijf {0} gebruikt {1} die een Blob storage-account is. Probeer het opnieuw met een algemeen opslagaccount.  |
|  StorageAccountTypeNotSupported  |  Storage-account {0} is van het {1} type. Diagnostics ondersteunt opstarten {2} opslagaccounttypen.  <ul><li>Deze fout treedt op als u de premium-opslagaccount voor diagnostische gegevens over opstarten. Zie voor meer informatie [het gebruik van diagnostische gegevens over opstarten](../articles/virtual-machines/windows/boot-diagnostics.md). </li></ul> |
|  SubscriptionNotAuthorizedForImage  |  Het abonnement is niet geautoriseerd.  |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} bestaat al. Geef een andere blob-URI voor het maken van een nieuwe lege gegevensschijf '{1}'.  |
|  TargetDiskBlobAlreadyExists  |  Vastleggen bewerking kan niet doorgaan omdat de blob-installatiekopieën {0} bestaat al en de vlag voor het overschrijven van VHD-blobs niet is ingesteld. Verwijder de blob of stel de vlag overschrijven van VHD-blobs en probeer het opnieuw.  |
|  TargetDiskBlobAlreadyExists  |  Vastleggen bewerking kan niet doorgaan omdat de blob-installatiekopieën {0} is geïnstalleerd op een actieve reservering geldt.   |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} bestaat al. Geef een andere blob-URI als doel voor schijf '{1}'.  |
|  TooManyVMRedeploymentRequests  |  Te veel aanvragen voor opnieuw implementeren is ontvangen voor de virtuele machine '{0}' of de virtuele machines in dezelfde beschikbaarheidsset met deze virtuele machine. Probeer het later opnieuw.  |
|  VHDSizeInvalid  |  De opgegeven schijfgrootte van {0} voor schijf '{1}' met blob {2} is ongeldig. Schijfgrootte moet tussen {3} en {4}.  |
|  VMAgentStatusCommunicationError  |  Virtuele machine '{0}' heeft de status voor VM-agent of extensies niet gerapporteerd. Controleer of de virtuele machine heeft een actieve VM-agent en uitgaande verbindingen naar Azure-opslag kunt maken.  |
|  VMArtifactRepositoryInternalError  |  Er is een fout opgetreden bij het communiceren met de artefactopslagplaats om VM-artefactgegevens op te halen.  |
|  VMArtifactRepositoryInternalError  |  Er is een interne fout opgetreden tijdens het ophalen van de VM-artefactgegevens van de artefactopslagplaats.  |
|  VMExtensionHandlerNonTransientError  |  Handler '{0}'heeft een fout gerapporteerd voor VM-extensie'{1}'met terminal foutcode'{2}' en het volgende foutbericht: '{3}'  |
|  VMExtensionManagementInternalError  |  Er is een interne fout opgetreden tijdens het verwerken van VM-extensie '{0}'.  |
|  VMExtensionManagementInternalError  |  Meerdere fouten opgetreden tijdens het voorbereiden van de VM-extensies. Zie de weergave van VM-extensie-exemplaar voor meer informatie.  |
|  VMExtensionProvisioningError  |  VM heeft een fout gerapporteerd bij het verwerken van de extensie '{0}'. Foutbericht weergegeven: '{1}'.  |
|  VMExtensionProvisioningError  |  Meerdere VM-uitbreidingen niet ingericht op de virtuele machine. Zie de instantieweergave van de VM-extensie voor meer informatie.  |
|  VMExtensionProvisioningTimeout  |  Het inrichten van VM-extensie '{0}' is een time-out. Installatie van de extensie duurt mogelijk te lang of de status van extensie kan niet worden opgehaald.  |
|  VMMarketplaceInvalidInput  |  Maken van een virtuele machine van een niet-Marketplace-installatiekopie hoeft niet plangegevens, verwijder de plangegevens in de aanvraag. Naam Besturingssysteemschijf is {0}.  |
|  VMMarketplaceInvalidInput  |  De aankoopgegevens komt niet overeen. Kan niet implementeren vanuit de Marketplace-installatiekopie. Naam Besturingssysteemschijf is {0}.  |
|  VMMarketplaceInvalidInput  |  Maken van een virtuele machine van de Marketplace-installatiekopie, moet gegevens over de planning in de aanvraag. Naam Besturingssysteemschijf is {0}.  |
|  VMNotFound  |  De virtuele machine '{0}' kan niet worden gevonden.  |
|  VMRedeploymentFailed  |  Virtuele machine '{0}' opnieuw implementeren is mislukt vanwege een interne fout. Probeer het later opnieuw.  |
|  VMRedeploymentTimedOut  |  Opnieuw installeren van de virtuele machine '{0}' is niet voltooid binnen de toegewezen tijd. Het kan worden voltooid enige tijd opnieuw. Anders kunt u de aanvraag opnieuw proberen.  |
|  VMStartTimedOut  |  Virtuele machine '{0}' is niet gestart binnen de toegewezen tijd. De virtuele machine mogelijk alsnog worden gestart. Controleer de energiestatus later.  |


## <a name="next-steps"></a>Volgende stappen
Als u meer hulp nodig hebt, kunt u de Azure-experts raadplegen op [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.