---
title: Algemene foutcodes voor virtuele machine in Azure | Microsoft Docs
description: Enkele van de algemene foutcodes aangetroffen bij het inrichten en beheren van virtuele machines in Azure
services: virtual-machines
documentationcenter: ''
author: xujing-ms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure
ms.date: 5/22/2017
ms.author: xujing
ms.openlocfilehash: 80b99880362c3979ae5ead6d1cdff4091d23d065
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118972"
---
# <a name="understand-common-error-messages-when-you-manage-virtual-machines-in-azure"></a>Informatie over veelvoorkomende foutberichten bij het beheren van virtuele machines in Azure

In dit artikel worden enkele van de meest voorkomende foutcodes en berichten die optreden kunnen bij het maken of beheren van virtuele machines (VM's) in Azure beschreven.

>[!NOTE]
> U kunt opmerkingen achterlaten op deze pagina voor feedback of via [feedback over Azure](https://feedback.azure.com/forums/216843-virtual-machines) met #azerrormessage tag.

## <a name="error-response-format"></a>Fout antwoordindeling 
Azure-VM's gebruiken de volgende JSON-indeling voor foutbericht:

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

Reactie op een fout bevat altijd een statuscode en een foutobject. Elk foutobject bevat altijd een foutcode en een bericht. Als de virtuele machine is gemaakt met een sjabloon, bevat de foutobject ook een sectie voor informatie die een binnenste niveau van foutcodes en het bericht bevat. Normaal gesproken is het meest binnenste niveau van het foutbericht de root-fout.


## <a name="common-virtual-machine-management-errors"></a>Veelvoorkomende fouten in virtual machine management

Deze sectie vindt u de algemene foutberichten die optreden kunnen bij het beheren van virtuele machines:

|  Foutcode  |  Foutbericht  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Kan geen lease verkrijgen tijdens het maken van schijf '{0}' met blob met URI {1}. BLOB wordt al gebruikt.  |  
|  AllocationFailed  |  Toewijzing is mislukt. Verminder de VM-grootte of het aantal virtuele machines, probeer het later opnieuw of probeer te implementeren naar een andere Beschikbaarheidsset of een andere Azure-locatie.  |  
|  AllocationFailed  |  De VM-toewijzing is mislukt vanwege een interne fout. Probeer het later opnieuw of probeer te implementeren naar een andere locatie.  |
|  ArtifactNotFound  |  De VM-extensie met uitgever '{0}en typ'{1}'kan niet worden gevonden in de locatie'{2}'.  |
|  ArtifactNotFound  |  Extensie met uitgever '{0}', type '{1}', en type handlerversie '{2}' kan niet worden gevonden in de extensieopslagplaats.  |
|  ArtifactVersionNotFound  |  Er is geen versie gevonden in de artefactopslagplaats die overeenkomt met de aangevraagde versie '{0}'.  |
|  ArtifactVersionNotFound  |  Er is geen versie gevonden in de artefactopslagplaats die overeenkomt met de aangevraagde versie '{0}'voor VM-extensie met uitgever'{1}en typ'{2}'.  |
|  AttachDiskWhileBeingDetached  |  Kan geen gegevensschijf koppelen '{0}'aan de virtuele machine'{1}' omdat de schijf momenteel wordt losgekoppeld. Wacht totdat de schijf volledig losgekoppeld is en probeer het opnieuw.  |
|  BadRequest  |  Uitgelijnd ' Beschikbaarheidssets worden nog niet ondersteund in deze regio.  |
|  BadRequest  |  Toevoegen van een VM met beheerde schijven aan niet-beheerde Beschikbaarheidsset of toevoeging van een VM met blobschijven aan een beheerde Beschikbaarheidsset wordt niet ondersteund. Maak een Beschikbaarheidsset met de eigenschap 'beheerde' is ingesteld om een VM met beheerde schijven aan toe te voegen.  |
|  BadRequest  |  Beheerde schijven worden niet ondersteund voor deze regio.  |
|  BadRequest  |  Meerdere VM per handler worden niet ondersteund voor besturingssysteemtype '{0}'. VMExtension '{1}'met handler'{2}is al toegevoegd of opgegeven in de invoer.  |
|  BadRequest  |  Bewerking '{0}'wordt niet ondersteund voor de Resource'{1}' met beheerde schijven.  |
|  CertificateImproperlyFormatted  |  De JSON-weergave van het geheim die is opgehaald uit {0} bevat een gegevensveld dat niet een onjuist geformatteerd PFX-bestand of het opgegeven wachtwoord is niet worden gedecodeerd het PFX-bestand.  |
|  CertificateImproperlyFormatted  |  De gegevens opgehaald uit {0} is niet opnieuw worden geserialiseerd in JSON.  |
|  Conflict  |  Het wijzigen van de schijfgrootte is alleen toegestaan tijdens het maken van een VM of wanneer de toewijzing van de VM ongedaan wordt gemaakt.  |
|  ConflictingUserInput  |  Schijf '{0}'kan niet worden gekoppeld omdat de schijf al eigendom is van de virtuele machine'{1}'.  |
|  ConflictingUserInput  |  De bron- en doelresourcegroep zijn dezelfde.  |
|  ConflictingUserInput  |  Bron- en storage-accounts voor schijf {0} verschillen.  |
|  ContainerAlreadyOnLease  |  Er is al een lease op het storage-container met de blob met URI {0}.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  De aanvraag resources verplaatsen bevat KeyVault-resources waarnaar wordt verwezen door een of meer {0}s in de aanvraag. Dit wordt niet wordt momenteel ondersteund kruislings abonnement verplaatsen. Controleer de foutdetails voor de resource-id's van de Key Vault.  |
|  DiagnosticsOperationInternalError  |  Er is een interne fout opgetreden tijdens het verwerken van diagnostische profiel van de virtuele machine {0}.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  BLOB {0} wordt al gebruikt door een andere schijf die behoort tot VM '{1}'. U kunt de blobmetagegevens voor de schijfverwijzingsgegevens bekijken.  |
|  DiskBlobNotFound  |  Kan geen VHD-blob met URI vinden {0} voor schijf '{1}'.  |
|  DiskBlobNotFound  |  Kan geen VHD-blob met URI vinden {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} geheim beschikt niet over de {1} tags. Werk de geheime versie, de vereiste labels toevoegen en probeer het opnieuw.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Uitpakken van het geheim {0} -waarde met sleutel {1} is mislukt.  |
|  DiskImageNotReady  |  Schijfinstallatiekopie {0} is in {1} staat. Probeer het opnieuw als de image gereed is.  |
|  DiskPreparationError  |  Een of meer fouten opgetreden tijdens het voorbereiden van VM-schijven. Zie de weergave voor meer informatie.  |
|  DiskProcessingError  |  Het verwerken van de schijf is gestopt omdat de virtuele machine andere schijven heeft in niet-werkende schijven heeft.  |
|  ImageBlobNotFound  |  Kan geen VHD-blob met URI vinden {0} voor schijf '{1}'.  |
|  ImageBlobNotFound  |  Kan geen VHD-blob met URI vinden {0}.  |
|  IncorrectDiskBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} voor schijf '{1}' is van het type blok-blob.  |
|  IncorrectDiskBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} is van het type '{1}'.  |
|  IncorrectImageBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} voor schijf '{1}' is van het type blok-blob.  |
|  IncorrectImageBlobType  |  Schijfblobs kunnen alleen worden van het type pagina-blob. BLOB {0} is van het type '{1}'.  |
|  InternalOperationError  |  Storage-account kan niet worden omgezet {0}. Zorg ervoor dat deze is gemaakt via de Storage Resource Provider op dezelfde locatie als de compute-resource.  |
|  InternalOperationError  |  {0} taken voor Doelzoeken is mislukt.  |
|  InternalOperationError  |  Er is een fout opgetreden bij het valideren van het netwerkprofiel van VM '{0}'.  |
|  InvalidAccountType  |  Het AccountType {0} is ongeldig.  |
|  InvalidParameter  |  De waarde van parameter {0} is ongeldig.  |
|  InvalidParameter  |  Het opgegeven beheerderswachtwoord is niet toegestaan.  |
|  InvalidParameter  |  "Het opgegeven wachtwoord moet tussen {0}-{1} tekens lang zijn en moet voldoen aan ten minste {2} van vereisten voor wachtwoordcomplexiteit van de volgende opties: <ol><li> Bevat een hoofdletter</li><li>Bevat een kleine letter</li><li>Een numerieke cijfer bevat</li><li>Een speciaal teken bevat.</li></ol>  |
|  InvalidParameter  |  De opgegeven gebruikersnaam voor een beheerder is niet toegestaan.  |
|  InvalidParameter  |  Kan een bestaande OS-schijf niet koppelen als de VM is gemaakt op basis van een installatiekopie van een platform of gebruiker.  |
|  InvalidParameter  |  Containernaam {0} is ongeldig. Namen van containers moeten tussen de 3 en 63 tekens lang zijn en mag alleen kleine letters, alfanumerieke tekens en afbreekstreepjes bevatten. Afbreekstreepje moet worden voorafgegaan en gevolgd door een alfanumeriek teken.  |
|  InvalidParameter  |  Containernaam {0} in URL {1} is ongeldig. Namen van containers moeten tussen de 3 en 63 tekens lang zijn en mag alleen kleine letters, alfanumerieke tekens en afbreekstreepjes bevatten. Afbreekstreepje moet worden voorafgegaan en gevolgd door een alfanumeriek teken.  |
|  InvalidParameter  |  De blobnaam in URL {0} bevat een schuine streep. Dit wordt momenteel niet ondersteund voor schijven.  |
|  InvalidParameter  |  De URI {0} lijkt niet de juiste blob-URI.  |
|  InvalidParameter  |  Een schijf met de naam '{0}' al gebruikmaakt van hetzelfde LUN: {1}.  |
|  InvalidParameter  |  Een schijf met de naam '{0}' bestaat al.  |
|  InvalidParameter  |  Er kan niet worden opgegeven dat de gebruikersinstallatiekopie moet worden overschreven voor een schijf die al is gedefinieerd in de opgegeven installatiekopieverwijzing.  |
|  InvalidParameter  |  Een schijf met de naam '{0}' wordt al gebruikt dezelfde VHD URL {1}.  |
|  InvalidParameter  |  Het aantal opgegeven foutdomeinen {0} moet vallen binnen het bereik {1} naar {2}.  |
|  InvalidParameter  |  Het licentietype {0} is ongeldig. Geldige licentietypen zijn: Windows_Client of Windows_Server, hoofdlettergevoelig.  |
|  InvalidParameter  |  Linux-hostnaam mag niet meer dan {0} tekens lang of de volgende tekens bevatten: {1}.  |
|  InvalidParameter  |  Het doelpad voor openbare Ssh-sleutels is momenteel beperkt tot de standaardwaarde {0} vanwege een bekend probleem in Linux-inrichtingsagent.  |
|  InvalidParameter  |  Een schijf voor LUN {0} bestaat al.  |
|  InvalidParameter  |  Abonnement {0} van de aanvraag moet overeenkomen met het {1} die deel uitmaken van de beheerde schijf-id.  |
|  InvalidParameter  |  Aangepaste gegevens in OSProfile moeten met base64 en met een maximale lengte van {0} tekens.  |
|  InvalidParameter  |  Blobnaam in URL {0} moet eindigen met '{1}' extensie.  |
|  InvalidParameter  |  {0}' is niet een geldig voorvoegsel van het vastgelegde VHD-blob. Een geldig voorvoegsel komt overeen met reguliere expressie '{1}'.  |
|  InvalidParameter  |  Certificaten kunnen niet worden toegevoegd aan uw virtuele machine als de VM-agent is niet ingericht.  |
|  InvalidParameter  |  Een schijf voor LUN {0} bestaat al.  |
|  InvalidParameter  |  Kan de virtuele machine maken omdat de aangevraagde grootte {0} is niet beschikbaar in het cluster waar de beschikbaarheidsset momenteel is toegewezen. De beschikbare grootten zijn: {1}. Lees meer over VM-grootte strategy bij https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  De aangevraagde VM-grootte {0} is niet beschikbaar in de huidige regio. De grootten die beschikbaar zijn in de huidige regio zijn: {1}. Meer informatie over de beschikbare VM-grootten in elke regio op https://aka.ms/azure-regions.  |
|  InvalidParameter  |  De aangevraagde VM-grootte {0} is niet beschikbaar in de huidige regio. Meer informatie over de beschikbare VM-grootten in elke regio op https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Windows-beheerdersgebruikersnaam mag niet meer dan {0} tekens lang, eindigen met een punt of de volgende tekens bevatten: {1}.  |
|  InvalidParameter  |  Windows-computernaam mag niet meer dan {0} tekens lang, uitsluitend uit cijfers bestaan, of de volgende tekens bevatten: {1}.  |
|  MissingMoveDependentResources  |  De aanvraag voor het verplaatsen van resources bevat niet alle afhankelijke resources. Controleer de foutdetails op ontbrekende resource-id.  |
|  MoveResourcesHaveInvalidState  |  De aanvraag voor verplaatsen van Resources bevat VM's die gekoppeld met ongeldige opslagaccounts zijn. Controleer de details voor deze resource-id's en de namen van opslagaccounts waarnaar wordt verwezen.  |
|  MoveResourcesHavePendingOperations  |  De aanvraag voor het verplaatsen van resources bevat resources waarvoor een bewerking in behandeling is. Controleer de details voor deze resource-id. Voer de bewerking opnieuw uit nadat de bewerkingen in behandeling is voltooid.  |
|  MoveResourcesNotFound  |  De aanvraag voor het verplaatsen van resources bevat resources die niet is gevonden. Controleer de details voor deze resource-id.  |
|  NetworkingInternalOperationError  |  Onbekende netwerktoewijzingsfout.  |
|  NetworkingInternalOperationError  |  Onbekende netwerktoewijzingsfout  |
|  NetworkingInternalOperationError  |  Er is een interne fout opgetreden tijdens het verwerken van het netwerkprofiel van de VM.  |
|  NotFound  |  De Beschikbaarheidsset {0} kan niet worden gevonden.  |
|  NotFound  |  Bron van de virtuele Machine '{0}is opgegeven in de aanvraag bestaat niet op deze Azure-locatie.  |
|  NotFound  |  Tenant met id {0} niet gevonden.  |
|  NotFound  |  De installatiekopie van het {0} kan niet worden gevonden.  |
|  NotSupported  |  Het licentietype is {0}, maar de installatiekopie blob {1} is niet van on-premises.  |
|  OperationNotAllowed  |  Beschikbaarheidsset {0} kan niet worden verwijderd. Voordat u verwijdert een Beschikbaarheidsset Zorg ervoor dat deze geen een virtuele machine bevat.  |
|  OperationNotAllowed  |  Beschikbaarheidsset wijzigen-SKU van 'Uitgelijnde' naar 'Classic' is niet toegestaan.  |
|  OperationNotAllowed  |  Kan geen extensies wijzigen in de VM wanneer de VM niet wordt uitgevoerd.  |
|  OperationNotAllowed  |  De actie vastleggen wordt alleen ondersteund op een virtuele Machine met op blobs gebaseerde schijven. Gebruik de 'Image' resource-API's voor het maken van een installatiekopie van een beheerde virtuele Machine.  |
|  OperationNotAllowed  |  De resource {0} kan niet worden gemaakt op basis van de installatiekopie {1} totdat de installatiekopie is gemaakt.  |
|  OperationNotAllowed  |  Updates van encryptionSettings is niet toegestaan wanneer de virtuele machine is toegewezen, probeer het opnieuw nadat de VM ongedaan is gemaakt  |
|  OperationNotAllowed  |  Het toevoegen van een beheerde schijf aan een VM met blobschijven wordt niet ondersteund.  |
|  OperationNotAllowed  |  Het maximum aantal gegevensschijven kunnen worden gekoppeld aan een VM van deze grootte is {0}.  |
|  OperationNotAllowed  |  Het toevoegen van een blobschijf aan een VM met beheerde schijven wordt niet ondersteund  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op de installatiekopie'{1}' omdat de installatiekopie is gemarkeerd voor verwijdering. U kunt alleen opnieuw proberen te verwijderen (of wachten tot een lopende om te voltooien).  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de virtuele machine is gegeneraliseerd.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan als herstelpuntverzameling'{1}' is gemarkeerd voor verwijdering.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM-extensie'{1}' omdat deze is gemarkeerd voor verwijdering. U kunt alleen opnieuw proberen te verwijderen (of wachten tot een lopende om te voltooien).  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan omdat de virtuele Machines'{1}'worden ingericht met behulp van de'{2}'.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan omdat de virtuele-Machineschaalset'{1}'wordt momenteel gebruikt voor de installatiekopie van het'{2}'.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de virtuele machine is gemarkeerd voor verwijdering. U kunt alleen opnieuw proberen te verwijderen (of wachten tot een lopende om te voltooien).  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de virtuele machine ongedaan is gemaakt of gemarkeerd voor het ongedaan maken.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de virtuele machine wordt uitgevoerd. Schakel uit expliciet in het geval u afsluiten van de virtuele machine uit in het gastbesturingssysteem.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat de virtuele machine niet ongedaan is gemaakt.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}'Omdat de virtuele machine uitbreidings-'{2}' status mislukt.  |
|  OperationNotAllowed  |  Bewerking '{0}'is niet toegestaan op VM'{1}' omdat een andere bewerking uitgevoerd wordt.  |
|  OperationNotAllowed  |  De bewerking '{0}'moet de virtuele Machine'{1}' worden gegeneraliseerd.  |
|  OperationNotAllowed  |  Voor de bewerking is het vereist dat de VM wordt uitgevoerd (of is ingesteld om te worden uitgevoerd).  |
|  OperationNotAllowed  |  Schijf met de grootte {0}GB, die kleiner is dan de grootte {1}GB van de overeenkomstige schijf in de afbeelding, is niet toegestaan.  |
|  OperationNotAllowed  |  VM-Schaalset-extensies van handler '{0}' kan alleen op het moment dat de VM-Schaalset gemaakt worden toegevoegd.  |
|  OperationNotAllowed  |  VM-Schaalset-extensies van handler '{0}' kan alleen op het moment van verwijderen van VM-Schaalset worden verwijderd.  |
|  OperationNotAllowed  |  Virtuele machine '{0}' maakt al gebruik van beheerde schijven.  |
|  OperationNotAllowed  |  Virtuele machine '{0}'behoort tot 'Classic' availability set'{1}'. Werk de beschikbaarheidsset 'Uitgelijnde' SKU gebruiken en voer de conversie.  |
|  OperationNotAllowed  |  Virtuele machine maken met de installatiekopie kan niet op blobs gebaseerde schijven hebben. Alle schijven moeten beheerde schijven.  |
|  OperationNotAllowed  |  Opnamebewerking kan niet worden voltooid omdat de VM niet is gegeneraliseerd.  |
|  OperationNotAllowed  |  Bewerkingen voor de VM '{0}' zijn niet toegestaan omdat de VM-schijven worden geconverteerd naar beheerde schijven.  |
|  OperationNotAllowed  |  Een bewerking uitgevoerd waarmee de energiestatus van de virtuele Machine wordt gewijzigd {0} naar {1}. Voer de bewerking {2} na enige tijd.  |
|  OperationNotAllowed  |  Kan niet toevoegen of bijwerken van de virtuele machine. De aangevraagde VM-grootte {0} mogelijk niet beschikbaar in de bestaande toewijzingseenheid. Lees meer over VM-grootte strategy bij https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Kan grootte van de virtuele machine omdat de aangevraagde grootte {0} is niet beschikbaar in het cluster waar de beschikbaarheidsset momenteel is toegewezen. De beschikbare grootten zijn: {1}. Lees meer over VM-grootte strategy bij https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Kan grootte van de virtuele machine omdat de aangevraagde grootte {0} is niet beschikbaar in het cluster waar de virtuele machine momenteel is toegewezen. Om het formaat van uw virtuele machine te {1} toewijzing ongedaan maken (dit is Stop-bewerking in Azure portal) en voer de grootte van de bewerking opnieuw uit. Lees meer over VM-grootte strategy bij https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  Besturingssysteem inrichten is mislukt voor de virtuele machine '{0}' omdat het gastbesturingssysteem wordt momenteel ingericht.  |
|  OSProvisioningClientError  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is mislukt. Foutdetails: {1} Zorg ervoor dat de installatiekopie juist is voorbereid (gegeneraliseerd). <ul><li>Instructies voor Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Sleutel genereren voor SSH-host is mislukt. Details van fout: {0}. Om op te lossen dit probleem controleren als Linux-agent juist is ingesteld. <ul><li>U kunt de instructies op controleren: https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux/ </li></ul> |
|  OSProvisioningClientError  |  Gebruikersnaam is opgegeven voor de virtuele machine is ongeldig voor deze distributie Linux. Details van fout: {0}.  |
|  OSProvisioningInternalError  |  Besturingssysteem inrichten is mislukt voor de virtuele machine '{0}' vanwege een interne fout.  |
|  Osprovisioningtimedout geretourneerd  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is niet binnen de toegewezen tijd voltooid. De virtuele machine mogelijk nog steeds wordt inrichten is voltooid. Controleer de Inrichtingsstatus later.  |
|  Osprovisioningtimedout geretourneerd  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is niet binnen de toegewezen tijd voltooid. De virtuele machine mogelijk nog steeds wordt inrichten is voltooid. Controleer de Inrichtingsstatus later. Controleer ook of de installatiekopie juist is voorbereid (gegeneraliseerd).   <ul><li>Instructies voor Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instructies voor Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  Osprovisioningtimedout geretourneerd  |  Inrichting van het besturingssysteem voor de virtuele machine '{0}' is niet binnen de toegewezen tijd voltooid. De VM-gastagent is echter gedetecteerd uitgevoerd. Dit kan erop wijzen het gastbesturingssysteem is niet correct voorbereid om te worden gebruikt als een VM-installatiekopie (met CreateOption = FromImage). U lost dit probleem, gebruikt u de VHD met CreateOption = Attach of juist voorbereiden voor gebruik als een installatiekopie:   <ul><li>Instructies voor Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instructies voor Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Het vereiste VM-grootte is momenteel niet beschikbaar in de geselecteerde locatie.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Resource kan niet worden bijgewerkt vanwege een lopende platformupdate. Probeer het later opnieuw.  |
|  StorageAccountLimitation  |  Storage-account '{0}' biedt geen ondersteuning voor pagina-blobs die zijn vereist voor het maken van schijven.  |
|  StorageAccountLimitation  |  Storage-account '{0}' het toegewezen quotum is overschreden.  |
|  StorageAccountLocationMismatch  |  Storage-account kan niet worden omgezet {0}. Zorg ervoor dat deze is gemaakt via de Storage Resource Provider op dezelfde locatie als de compute-resource.  |
|  StorageAccountNotFound  |  Storage-account {0} niet gevonden. Controleer of de storage-account is niet verwijderd en behoort tot dezelfde Azure-locatie als de virtuele machine.  |
|  StorageAccountNotRecognized  |  Gebruik een opslagaccount dat wordt beheerd door Storage Resource Provider. Het gebruik van {0} wordt niet ondersteund.  |
|  StorageAccountOperationInternalError  |  Er is een interne fout opgetreden tijdens het openen van storage-account {0}.  |
|  StorageAccountSubscriptionMismatch  |  Storage-account {0} behoort niet tot abonnement {1}.  |
|  StorageAccountTooBusy  |  Storage-account '{0}' is momenteel bezet. Overweeg het gebruik van een ander account.  |
|  StorageAccountTypeNotSupported  |  Schijf {0} maakt gebruik van {1} die een Blob storage-account is. Probeer het opnieuw met een opslagaccount voor algemeen gebruik.  |
|  StorageAccountTypeNotSupported  |  Storage-account {0} is van het {1} type. Ondersteunt het diagnostische gegevens over opstarten {2} storage-accounttypen.  <ul><li>Deze fout treedt op als u de premium-opslagaccount voor diagnostische gegevens over opstarten. Zie voor meer informatie, [over het gebruik van diagnostische gegevens over opstarten](boot-diagnostics.md). </li></ul> |
|  SubscriptionNotAuthorizedForImage  |  Het abonnement is niet geautoriseerd.  |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} bestaat al. Geef een andere blob-URI te maken van een nieuwe lege gegevensschijf '{1}'.  |
|  TargetDiskBlobAlreadyExists  |  Capture kan niet worden voortgezet omdat de blob-doelafbeelding {0} bestaat al en de vlag voor het overschrijven van VHD-blobs is niet ingesteld. Verwijder de blob of stel de vlag overschrijven van VHD-blobs en probeer het opnieuw.  |
|  TargetDiskBlobAlreadyExists  |  Capture kan niet worden voortgezet omdat de blob-doelafbeelding {0} heeft een actieve lease erop.   |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} bestaat al. Geef een andere blob-URI als doel voor schijf '{1}'.  |
|  TooManyVMRedeploymentRequests  |  Te veel aanvragen voor opnieuw implementeren is ontvangen voor de virtuele machine '{0}' of de virtuele machines in dezelfde beschikbaarheidsset als aan deze virtuele machine. Probeer het later opnieuw.  |
|  VHDSizeInvalid  |  De opgegeven schijfgrootte van {0} voor schijf '{1}' met de blob {2} is ongeldig. De schijfgrootte moet tussen {3} en {4}.  |
|  VMAgentStatusCommunicationError  |  Virtuele machine '{0}' heeft de status voor VM-agent of de extensies niet gerapporteerd. Controleer of de virtuele machine heeft een actieve VM-agent en uitgaande verbindingen naar Azure storage kunt maken.  |
|  VMArtifactRepositoryInternalError  |  Er is een fout opgetreden bij het communiceren met de artefactopslagplaats om VM-artefactgegevens op te halen.  |
|  VMArtifactRepositoryInternalError  |  Er is een interne fout opgetreden tijdens het ophalen van de VM-artefactgegevens van de artefactopslagplaats.  |
|  VMExtensionHandlerNonTransientError  |  Handler '{0}'is een fout gerapporteerd voor VM-extensie'{1}'met terminal foutcode'{2}' en het volgende foutbericht: '{3}'  |
|  VMExtensionManagementInternalError  |  Er is een interne fout opgetreden tijdens het verwerken van VM-extensie '{0}'.  |
|  VMExtensionManagementInternalError  |  Meerdere fouten opgetreden tijdens het voorbereiden van de VM-extensies. Zie de weergave van een VM-extensie-exemplaar, voor meer informatie.  |
|  VMExtensionProvisioningError  |  Virtuele machine heeft een fout gerapporteerd bij het verwerken van extensie '{0}'. Foutbericht weergegeven: "{1}'.  |
|  VMExtensionProvisioningError  |  Meerdere VM-extensies kunnen niet worden ingericht op de virtuele machine. Raadpleeg de instantieweergave van de VM-extensie voor meer informatie.  |
|  VMExtensionProvisioningTimeout  |  Het inrichten van VM-extensie '{0}' is een time-out. Installatie van de extensie duurt mogelijk te lang of de status van extensie kan niet worden opgehaald.  |
|  VMMarketplaceInvalidInput  |  Het maken van een virtuele machine uit een niet-Marketplace-installatiekopie hoeft niet plangegevens, verwijder de plangegevens in de aanvraag. Naam van de Besturingssysteemschijf is {0}.  |
|  VMMarketplaceInvalidInput  |  De aankoopgegevens komen niet overeen. Kan niet uit de Marketplace-installatiekopie implementeren. Naam van de Besturingssysteemschijf is {0}.  |
|  VMMarketplaceInvalidInput  |  Een virtuele machine maken van Marketplace-installatiekopie, moet u gegevens over de planning in de aanvraag. Naam van de Besturingssysteemschijf is {0}.  |
|  VMNotFound  |  De virtuele machine '{0}' kan niet worden gevonden.  |
|  VMRedeploymentFailed  |  Virtuele machine '{0}' opnieuw implementeren is mislukt vanwege een interne fout. Probeer het later opnieuw.  |
|  VMRedeploymentTimedOut  |  Opnieuw implementeren van virtuele machine '{0}' is niet voltooid binnen de toegewezen tijd. Het kan worden voltooid enige tijd opnieuw uit. Anders kunt u de aanvraag opnieuw proberen.  |
|  VMStartTimedOut  |  Virtuele machine '{0}' is niet gestart binnen de toegewezen tijd. De virtuele machine mogelijk alsnog worden gestart. Controleer de energiestatus later opnieuw.  |


## <a name="next-steps"></a>Volgende stappen
Als u meer hulp nodig hebt, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.