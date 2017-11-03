---
title: Foutcodes voor Azure Machine Learning REST API | Microsoft Docs
description: Deze foutcodes kunnen worden geretourneerd door een bewerking op een Azure Machine Learning-webservice.
keywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 0923074b-3728-439d-a1b8-8a7245e39be4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 11/16/2016
ms.author: garye
ms.openlocfilehash: 5cf7d5bb878f323e4e3559822dc745359e43608e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="machine-learning-rest-api-error-codes"></a>Machine Learning-REST API-foutcodes
 
De volgende foutcodes kunnen worden geretourneerd door een bewerking op een Azure Machine Learning-webservice.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (HTTP-statuscode 400)
 
Ongeldig argument opgegeven.
 
Deze klasse van fouten betekent een ergens opgegeven argument is ongeldig. Dit kan een referentie of de locatie van Azure storage op iets doorgegeven aan de web-service zijn. Raadpleeg het veld van de fout 'code' in de sectie 'informatie' voor het vaststellen van welke specifieke argument ongeldig is.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| BadParameterValue | De opgegeven parameterwaarde voldoet niet aan de parameter-regel op de parameter |
| BadSubscriptionId | De abonnements-Id die wordt gebruikt om te beoordelen is niet aanwezig in de resource |
| BadVersionCall | Ongeldige versie-parameter is doorgegeven tijdens de API-aanroep: {0}. Controleer de help-pagina van de API voor het doorgeven van de juiste versie en probeer het opnieuw. |
| BatchJobInputsNotSpecified | De volgende vereiste input(s) niet zijn opgegeven met de aanvraag: {0}. Controleer of alle ingevoerde gegevens is opgegeven en probeer het opnieuw. |
| BatchJobInputsTooManySpecified | De aanvraag opgegeven meer invoerwaarden dan gedefinieerd in de service. Lijst met geaccepteerde input(s): {0}. Controleer of alle ingevoerde gegevens correct is opgegeven en probeer het opnieuw. |
| BlobNameTooLong | Azure blob storage-pad opgegeven voor diagnostische uitvoer te lang is: {0}. Het pad korter en probeer het opnieuw. |
| BlobNotFound | Geen toegang tot de opgegeven Azure blob - {0}.  Azure-foutbericht: {1}. |
| ContainerIsEmpty | Er is geen Azure-opslag-containernaam is opgegeven. Geef een geldige container op en probeer het opnieuw. |
| ContainerSegmentInvalid | De containernaam is ongeldig. Geef een geldige container op en probeer het opnieuw. |
| ContainerValidationFailed | BLOB-container-validatie is mislukt vanwege de volgende fout: {0}. |
| DataTypeNotSupported | Niet-ondersteund gegevenstype opgegeven. Geef geldige gegevenstypen en probeer het opnieuw. |
| DuplicateInputInBatchCall | De batch-aanvraag is ongeldig. Kan geen invoer van één of meerdere opgeven op hetzelfde moment. Verwijder een van deze items uit de aanvraag en probeer het opnieuw. |
| ExpiryTimeInThePast | Verlooptijd opgegeven is in het verleden: {0}. Geef een toekomstige vervaldatum UTC-tijd en probeer het opnieuw. Om nooit verlopen, verlopen tijd niet instellen op NULL. |
| IncompleteSettings | Diagnostische instellingen zijn onvolledig. |
| InputBlobRelativeLocationInvalid | Er is geen Azure storage blob-naam opgegeven. Geef een geldige blob-naam en probeer het opnieuw. |
| InvalidBlob | Ongeldige blob-specificatie voor blob: {0}. Controleer of deze verbindingsreeks / relatief pad of SAS-token specificatie juist is en probeer het opnieuw. |
| InvalidBlobConnectionString | De verbindingsreeks die is opgegeven voor een van de i/o-blobs in een ongeldig: {0}. Corrigeer deze en probeer het opnieuw. |
| InvalidBlobExtension | De blobverwijzing: {0} heeft een ongeldige of ontbrekende bestandsextensie. Bestandsextensies ondersteund voor dit uitvoertype zijn: '{1}'. |
| InvalidInputNames | Ongeldige service invoer namen die zijn opgegeven in de aanvraag: {0}. De ingevoerde gegevens toewijzen aan de juiste service-invoer en probeer het opnieuw. |
| InvalidOutputOverrideName | Ongeldige uitvoer naam overschrijven: {0}. De service heeft geen een knooppunt uitvoer met deze naam. Geef een juiste uitvoer knooppuntnaam voor het onderdrukken van (hoofdlettergevoeligheid van toepassing). |
| InvalidQueryParameter | Ongeldige queryparameter {0}. {1} |
| MissingInputBlobInformation | Ontbrekende informatie voor Azure storage-blob. Geef een geldige verbindingsreeks en de relatief pad of de URI en probeer het opnieuw. |
| MissingJobId | Er is geen taak-Id opgegeven. Een taak Id wordt geretourneerd wanneer een taak voor de eerste keer is verzonden. Controleer of dat de taak-Id juist is en probeer het opnieuw. |
| MissingKeys | Er zijn geen sleutels opgegeven of een van de primaire of secundaire sleutel is niet opgegeven. |
| MissingModelPackage | Geen pakket-Id van model of model pakket die zijn opgegeven. Geef de pakket-Id van een geldig model of model pakket en probeer het opnieuw. |
| MissingOutputOverrideSpecification | De blob-specificatie voor onderdrukking van de uitvoer {0} ontbreekt in de aanvraag. Geef een geldige blob-locatie met de aanvraag, of verwijder de uitvoerspecificatie geen overschrijving locatie desgewenst. |
| MissingRequestInput | De webservice invoer verwacht, maar er is niets is opgegeven. Zorg ervoor dat geldige invoer worden geleverd op basis van de gepubliceerde ingangspoorten in het model en probeer het opnieuw. |
| MissingRequiredGlobalParameters | Niet alle vereiste web service parameters opgegeven. Controleer of de parameters verwacht voor de modules die juist zijn en probeer het opnieuw. |
| MissingRequiredOutputOverrides | Bij het aanroepen van een versleutelde service-eindpunt is verplicht om op te geven onderdrukt de uitvoer voor de uitvoer van de service. Onderdrukkingen op dit moment voor deze uitvoer ontbreekt: {0} |
| MissingWebServiceGroupId | Er is geen groep web service-Id opgegeven. Geef een geldige webservice-servicegroep Id en probeer het opnieuw. |
| MissingWebServiceId | Geen webservice-Id opgegeven. Geef een geldige webservice Id en probeer het opnieuw. |
| MissingWebServicePackage | Er is geen web Service-pakket opgegeven. Geef een geldige web service-pakket op en probeer het opnieuw. |
| MissingWorkspaceId | Er is geen werkruimte-Id opgegeven. Geef een geldige Id-werkruimte en probeer het opnieuw. |
| ModelConfigurationInvalid | Ongeldig modeltype configuratie in het model-pakket. Controleer de Modelconfiguratie bevat een definitie in de uitvoer-eindpunten, std fout eindpunt, en std eindpunt af en probeer het opnieuw. |
| ModelPackageIdInvalid | Ongeldig modeltype pakket-id. Controleer of de model-pakket-Id juist is en probeer het opnieuw. |
| RequestBodyInvalid | Er is geen aanvraagtekst opgegeven of fout bij het deserialiseren van de aanvraagtekst. |
| RequestIsEmpty | Er is geen aanvraag opgenomen. Geef een geldige aanvraag en probeer het opnieuw. |
| UnexpectedParameter | Onverwachte parameters die worden geleverd. Controleer of alle parameternamen correct zijn gespeld, alleen verwachte parameters worden doorgegeven en probeer het opnieuw. |
| UnknownError | Onbekende fout opgetreden. |
| UserParameterInvalid | {0} |
| WebServiceConcurrentRequestRequirementInvalid | Kan de vereisten voor gelijktijdige aanvragen voor de webservice {0} niet wijzigen. |
| WebServiceIdInvalid | Ongeldige webservice-id opgegeven. Webservice-id moet een geldige guid zijn. |
| WebServiceTooManyConcurrentRequestRequirement | Vereiste gelijktijdige aanvraag kan niet worden ingesteld op meer dan {0}. |
| WebServiceTypeInvalid | Ongeldige web service type dat is opgegeven. Controleer of dat het type van de geldige web-service juist is en probeer het opnieuw. Geldige webservice servicetypen: {0}. |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (HTTP-statuscode 400)
 
Ongeldige gebruiker groepsargument opgegeven.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| InputMismatchError | Invoergegevens komt niet overeen voor invoerpoort schema. |
| InputParseError | Het parseren van de invoer-vector is mislukt.  Controleer of dat de invoer-vector heeft het juiste aantal kolommen en gegevenstypen.  Aanvullende informatie: {0}. |
| MissingRequiredGlobalParameters | Er ontbreken parameters verwacht door de webservice. Controleer of de vereiste parameters verwacht door de webservice juist zijn en probeer het opnieuw. |
| UnexpectedParameter | Controleer of u alleen de vereiste parameters verwacht door de webservice worden doorgegeven en probeer het opnieuw. |
| UserParameterInvalid | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>InvalidOperation (HTTP-statuscode 400)
 
De aanvraag is ongeldig in de huidige context.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| CannotStartJob | De taak kan niet worden gestart omdat het {0} status heeft. |
| IncompatibleModel | Het model is incompatibel met de versie van de aanvraag. De aanvraag-versie ondersteunt alleen één datatable uitvoer modellen. |
| MultipleInputsNotAllowed | Het model kunnen niet meerdere invoer. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (HTTP-statuscode 400)
 
Uitvoering van module is een interne bibliotheek opgetreden.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (HTTP-statuscode 400)
 
Uitvoering van module is een fout opgetreden.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (HTTP-statuscode 400)
 
Ongeldige web service-pakket. Controleer of het opgegeven pakket van web service juist is en probeer het opnieuw.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| FormatError | De web service-pakket is niet goed gevormd. Details: {0} |
| RuntimesError | De grafiek web service-pakket is ongeldig. Details: {0} |
| ValidationError | De grafiek web service-pakket is ongeldig. Details: {0} |
 
## <a name="unauthorized-http-status-code-401"></a>Niet-geautoriseerde (HTTP-statuscode 401)
 
Aanvraag is niet geautoriseerd voor toegang tot bronnen.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| AdminRequestUnauthorized | Niet geautoriseerd |
| ManagementRequestUnauthorized | Niet geautoriseerd |
| ScoreRequestUnauthorized | Ongeldige referenties opgegeven. |
 
## <a name="notfound-http-status-code-404"></a>NotFound (HTTP-statuscode 404)
 
Kan de resource niet vinden.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| ModelPackageNotFound | Model-pakket niet gevonden. Controleer of dat de model-pakket-Id juist is en probeer het opnieuw. |
| WebServiceIdNotFoundInWorkspace | Web-service onder deze werkruimte niet gevonden. Er is een discrepantie tussen het webServiceId en de workspaceId. Controleer of de webservice opgegeven maakt deel uit van de werkruimte en probeer het opnieuw. |
| WebServiceNotFound | De webservice is niet gevonden. Controleer of dat de webservice Id juist is en probeer het opnieuw. |
| WorkspaceNotFound | De werkruimte is niet gevonden. Controleer of dat de werkruimte-Id juist is en probeer het opnieuw. |
 
## <a name="requesttimeout-http-status-code-408"></a>RequestTimeout (HTTP-statuscode 408)
 
De bewerking kan niet worden voltooid in de toegestane tijd.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| RequestCanceled | Aanvraag is geannuleerd door de client. |
| ScoreRequestTimeout | Er is een time-out opgetreden voor de aanvraag voor het uitvoeren. |
 
## <a name="conflict-http-status-code-409"></a>Conflict (HTTP-statuscode 409)
 
Er bestaat al een resource.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Ongeldige uitvoer parameternaam. Gebruik de module van de editor voor metagegevens te wijzigen van kolommen en probeer het opnieuw. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (HTTP-statuscode 413)
 
Het model had overschrijdt het geheugenquotum die is toegewezen.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| OutOfMemoryLimit | Het model verbruikt meer geheugen hebben dan is bestemd is voor deze. Maximaal toegestane hoeveelheid geheugen voor het model is {0} MB. Controleer of het model voor problemen. |
 
## <a name="internalerror-http-status-code-500"></a>InternalError (HTTP-statuscode 500)
 
Uitvoering van een interne fout opgetreden.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentError |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| ContainerProcessTerminatedWithSystemError | Het proces van de container is vastgelopen met systeemfout |
| ContainerProcessTerminatedWithUnknownError | Het proces van de container is vastgelopen met een onbekende fout |
| ContainerValidationFailed | BLOB-container-validatie is mislukt vanwege de volgende fout: {0}. |
| DeleteWebServiceResourceFailed |  |
| ExceptionDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| InvalidMemoryConfiguration | InvalidMemoryConfiguration, ConfigValue: {0} |
| InvalidResourceCacheConfiguration |  |
| InvalidResourceDownloadConfiguration |  |
| InvalidWebServiceResources |  |
| MissingTaskInstance | Er zijn geen argumenten opgegeven. Controleer of dat de geldige argumenten zijn doorgegeven en probeer het opnieuw. |
| ModelPackageInvalid |  |
| ModuleExecutionFailed |  |
| ModuleLoadFailed |  |
| ModuleObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotSupported | Poort-id = {0} heeft een niet-ondersteund gegevenstype: {1}. |
| ResourceDownload |  |
| ResourceLoadFailed |  |
| ServiceUrisNotFound |  |
| SwaggerGeneration | Genereren van swagger is mislukt, Details: {0} |
| UnexpectedScoreStatus |  |
| UnknownBackendErrorResponse |  |
| UnknownError |  |
| UnknownJobStatusCode | Onbekende taak statuscode {0}. |
| UnknownModuleError |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageInvalid | InvalidWebServicePackage, Details: {0} |
| WorkerAuthorizationFailed |  |
| WorkerUnreachable |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (HTTP-statuscode 500)
 
Uitvoering van een interne fout opgetreden. Het systeem is onvoldoende geheugen beschikbaar. Probeer het opnieuw.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (HTTP-statuscode 500)
 
Ongeldig modeltype pakket. Controleer of het opgegeven model-pakket juist is en probeer het opnieuw.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (HTTP-statuscode 500)
 
Ongeldige web service-pakket. Controleer of het opgegeven web-pakket juist is en probeer het opnieuw.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| ModuleError | De grafiek web service-pakket is ongeldig. Details: {0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>InitializingContainers (HTTP-statuscode 503)
 
De aanvraag kan niet uitvoeren omdat de containers zijn geïnitialiseerd.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceUnavailable (HTTP-statuscode 503)
 
Service is tijdelijk niet beschikbaar.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| NoMoreResources | Er zijn geen bronnen beschikbaar zijn voor aanvraag. |
| RequestThrottled | Aanvraag is voor het eindpunt {0} beperkt. De maximale gelijktijdigheid van taken voor het eindpunt is {1}. |
| TooManyConcurrentRequests | Te veel gelijktijdige aanvragen verzonden. |
| TooManyHostsBeingInitialized | Te veel hosts op hetzelfde moment wordt geïnitialiseerd. U kunt beperking / opnieuw uit te voeren. |
| TooManyHostsBeingInitializedPerModel | Te veel hosts op hetzelfde moment wordt geïnitialiseerd. U kunt beperking / opnieuw uit te voeren. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>GatewayTimeout (HTTP-statuscode 504)
 
De bewerking kan niet worden voltooid in de toegestane tijd.
 
| Foutcode | Bericht van gebruiker |
| ---------- |--------------|
| BackendInitializationTimeout | De initialisatie van de web-service kan niet worden voltooid binnen de toegestane tijd. |
| BackendScoreTimeout | Het uitvoeren van de aanvraag van de web-service kan niet worden voltooid binnen de toegestane tijd. |
 
