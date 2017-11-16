---
title: Naslaginformatie over Azure Machine Learning-Model Management opdrachtregelinterface | Microsoft Docs
description: Azure Machine Learning-Model Management opdrachtregelinterface verwijzing.
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/08/2017
ms.openlocfilehash: 373abb8f40a8acf557b7cd4a0d0b3fb55f4a545c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="model-management-command-line-interface-reference"></a>Verwijzing naar een model management opdrachtregelinterface

## <a name="base-cli-concepts"></a>Concepten van Base CLI:

    account : Manage model management accounts. 
    env     : Manage compute environments.
    image   : Manage operationalization images.
    manifest: Manage operationalization manifests.
    model   : Manage operationalization models.
    service : Manage operationalized services.

## <a name="account-commands"></a>Account-opdrachten
Een model-management-account is vereist voor het gebruik van de services waarmee u kunt implementeren en beheren van modellen. Gebruik `az ml account modelmanagement -h` om te zien in de volgende lijst:

    create: Create a Model Management Account.
    delete: Delete a specified Model Management Account.
    list  : Gets the Model Management Accounts in the current subscriptiong.
    set   : Set the active Model Management Account.
    show  : Show a Model Management Account.
    update: Update an existing Model Management Account.

**Model Management-Account maken**

Een model managementaccount maken met de volgende opdracht. Dit account wordt gebruikt voor facturering.

`az ml account modelmanagement create --location [Azure region e.g. eastus2] --name [new account name] --resource-group [resource group name to store the account in]`

Lokale argumenten:

    --location -l       [Required]: Resource location.
    --name -n           [Required]: Name of the model management account.
    --resource-group -g [Required]: Resource group to create the model management account in.
    --description -d              : Description of the model management account.
    --sku-instances               : Number of instances of the selected SKU. Must be between 1 and
                                    16 inclusive.  Default: 1.
    --sku-name                    : SKU name. Valid names are S1|S2|S3|DevTest.  Default: S1.
    --tags -t                     : Tags for the model management account.  Default: {}.
    -v                            : Verbosity flag.

## <a name="environment-commands"></a>Omgeving opdrachten

    cluster        : Switch the current execution context to 'cluster'.
    delete         : Delete an MLCRP-provisioned resource.
    get-credentials: List the keys for an environment.
    list           : List all environments in the current subscription.
    local          : Switch the current execution context to 'local'.
    set            : Set the active MLC environment.
    setup          : Sets up an MLC environment.
    show           : Show an MLC resource; if resource_group or cluster_name are not provided, shows
                     the active MLC env.

**De implementatie-omgeving instellen**

De setup-opdracht moet u bijdrager toegang hebben tot het abonnement. Als u die hebt, moet u ten minste Inzender toegang tot de resourcegroep die u wilt implementeren in. Hiertoe de laatste, moet u de Resourcegroepnaam opgeven als onderdeel van de setup-opdracht via `-g` de vlag. 

Er zijn twee opties voor implementatie: *lokale* en *cluster*. Instellen van de `--cluster` (of `-c`) vlag schakelt Clusterimplementatie, die voorziet in een ACS-cluster. De syntaxis van de basisinstellingen is als volgt:

`az ml env setup [-c] --location [location of environment resources] --name[name of environment]`

Dit uw Azure machine learning-omgeving met een opslagaccount, ACR-register en App Insights-service gemaakt in uw abonnement wordt geïnitialiseerd. Standaard wordt de omgeving geïnitialiseerd voor alleen lokale implementaties (geen ACS) als er geen vlag is opgegeven. Als u schalen, service wilt, geeft u de `--cluster` (of `-c`) vlag voor het maken van een ACS-cluster.

Opdrachtdetails van de:

    --location -l        [Required]: Location for environment resources; an Azure region, e.g. eastus2.
    --name -n            [Required]: Name of environment to provision.
    --acr -r                       : ARM ID of ACR to associate with this environment.
    --agent-count -z               : Number of agents to provision in the ACS cluster. Default: 3.
    --cert-cname                   : CNAME of certificate.
    --cert-pem                     : Path to .pem file with certificate bytes.
    --cluster -c                   : Flag to provision ACS cluster. Off by default; specify this to force an ACS cluster deployment.
    --key-pem                      : Path to .pem file with certificate key.
    --master-count -m              : Number of master nodes to provision in the ACS cluster. Acceptable values: 1, 3, 5. Default: 1.
    --resource-group -g            : Resource group in which to create compute resource. Will be created if it does not exist.
                                     If not provided, resource group will be created with 'rg' appended to 'name.'.
    --service-principal-app-id -a  : App ID of service principal to use for configuring ML compute.
    --service-principal-password -p: Password associated with service principal.
    --storage -s                   : ARM ID of storage account to associate with this environment.
    --yes -y                       : Flag to answer 'yes' to any prompts. Command will fail if user is not logged in.

Algemene argumenten
```
    --debug                        : Increase logging verbosity to show all debug logs.
    --help -h                      : Show this help message and exit.
    --output -o                    : Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query                        : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose                      : Increase logging verbosity. Use --debug for full debug logs.
```
## <a name="model-commands"></a>Model opdrachten

    list
    register
    show

**Registreren van een model**

Opdracht voor het registreren van het model.

`az ml model register --model [path to model file] --name [model name]`

Opdrachtdetails van de:

    --model -m [Required]: Model to register.
    --name -n  [Required]: Name of model to register.
    --description -d     : Description of the model.
    --tag -t             : Tags for the model. Multiple tags can be specified with additional -t arguments.
    -v                   : Verbosity flag.

Algemene argumenten

    --debug              : Increase logging verbosity to show all debug logs.
    --help -h            : Show this help message and exit.
    --output -o          : Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query              : JMESPath query string. See http://jmespath.org/ for more information and
                           examples.
    --verbose            : Increase logging verbosity. Use --debug for full debug logs.

## <a name="manifest-commands"></a>Manifest opdrachten

    create: Create an Operationalization Manifest. This command has two different
            sets of required arguments, depending on if you want to use previously registered
            model/s.
    list
    show

**Maken van het manifest**

Maakt een manifestbestand voor het model. 

`az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]`

Opdrachtdetails van de:

    --manifest-name -n [Required]: Name of the manifest to create.
    -f                 [Required]: The code file to be deployed.
    -r                 [Required]: Runtime of the web service. Valid runtimes are spark-py|python.
    --conda-file -c              : Path to Conda Environment file.
    --dependency -d              : Files and directories required by the service. Multiple
                                   dependencies can be specified with additional -d arguments.
    --manifest-description       : Description of the manifest.
    --schema-file -s             : Schema file to add to the manifest.
    -p                           : A pip requirements.txt file needed by the code file.
    -v                           : Verbosity flag.

Geregistreerd Model argumenten

    --model-id -i                : [Required] Id of previously registered model to add to manifest.
                                   Multiple models can be specified with additional -i arguments.

Niet-geregistreerde Model argumenten

    --model-file -m              : [Required] Model file to register. If used, must be combined with
                                   model name.

Algemene argumenten

    --debug                      : Increase logging verbosity to show all debug logs.
    --help -h                    : Show this help message and exit.
    --output -o                  : Output format.  Allowed values: json, jsonc, table, tsv.
                                   Default: json.
    --query                      : JMESPath query string. See http://jmespath.org/ for more
                                   information and examples.
    --verbose                    : Increase logging verbosity. Use --debug for full debug logs.


## <a name="image-commands"></a>Opdrachten van afbeeldingen

    create: Creates a docker image with the model and its dependencies. This command has two different sets of
            required arguments, depending on if you want to use a previously created manifest.
    list
    show
    usage

**Installatiekopie maken**

U kunt een installatiekopie maken met de optie van de manifest voordat hebben gemaakt. 

`az ml image create -n [image name] --manifest-id [the manifest ID]`

Of u kunt maken van het manifest en de installatiekopie met één opdracht. 

`az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]`

Opdrachtdetails van de:

    --image-name -n [Required]: The name of the image being created.
    --image-description       : Description of the image.
    --image-type              : The image type to create. Defaults to "Docker".
    -v                        : Verbosity flag.

Geregistreerde Manifest argumenten

    --manifest-id             : [Required] Id of previously registered manifest to use in image creation.

Niet-geregistreerde Manifest argumenten

    --conda-file -c           : Path to Conda Environment file.
    --dependency -d           : Files and directories required by the service. Multiple dependencies can
                                be specified with additional -d arguments.
    --model-file -m           : [Required] Model file to register.
    --schema-file -s          : Schema file to add to the manifest.
    -f                        : [Required] The code file to be deployed.
    -p                        : A pip requirements.txt file needed by the code file.
    -r                        : [Required] Runtime of the web service. Valid runtimes are python|spark-py.


## <a name="service-commands"></a>Service-opdrachten
De volgende opdrachten worden ondersteund voor de Service. Als de parameters voor elke opdracht weergeven, gebruikt u de optie -h. Bijvoorbeeld: `az ml service create realtime -h` om te zien opdrachtdetails maken.

    create
    delete
    keys
    list
    logs
    run
    show
    update
    usage

**Een service maken**

Als een service maken met een installatiekopie van een eerder gemaakte, gebruik de volgende opdracht:

`az ml service create realtime --image-id [image to deploy] -n [service name]`

Gebruik de volgende opdracht voor het maken van een service manifest en de installatiekopie met één opdracht:

`az ml service create realtime --model-file [path to model file(s)] -f [path to model scoring file, e.g. score.py] -n [service name] -r [run time included in the image, e.g. spark-py]`

Details van de opdrachten:

    -n                                : [Required] Webservice name.
    --autoscale-enabled               : Enable automatic scaling of service replicas based on request demand.
                                        Allowed values: true, false. False if omitted.  Default: false.
    --autoscale-max-replicas          : If autoscale is enabled - sets the maximum number of replicas.
    --autoscale-min-replicas          : If autoscale is enabled - sets the minimum number of replicas.
    --autoscale-refresh-period-seconds: If autoscale is enabled - the interval of evaluating scaling demand.
    --autoscale-target-utilization    : If autoscale is enabled - target utilization of replicas time.
    --collect-model-data              : Enable model data collection. Allowed values: true, false. False if omitted.  Default: false.
    --cpu                             : Reserved number of CPU cores per service replica (can be fraction).
    --enable-app-insights -l          : Enable app insights. Allowed values: true, false. False if omitted.  Default: false.
    --memory                          : Reserved amount of memory per service replica, in M or G. (ex. 1G, 300M).
    --replica-max-concurrent-requests : Maximum number of concurrent requests that can be routed to a service replica.
    -v                                : Verbosity flag.
    -z                                : Number of replicas for a Kubernetes service.  Default: 1.

Geregistreerde installatiekopie argumenten

    --image-id                        : [Required] Image to deploy to the service.

Niet-geregistreerde installatiekopie argumenten

    --conda-file -c                   : Path to Conda Environment file.
    --image-type                      : The image type to create. Defaults to "Docker".
    --model-file -m                   : [Required] The model to be deployed.
    -d                                : Files and directories required by the service. Multiple dependencies can be specified
                                        with additional -d arguments.
    -f                                : [Required] The code file to be deployed.
    -p                                : A pip requirements.txt file of package needed by the code file.
    -r                                : [Required] Runtime of the web service. Valid runtimes are python|spark-py.
    -s                                : Input and output schema of the web service.

Algemene argumenten

    --debug                           : Increase logging verbosity to show all debug logs.
    --help -h                         : Show this help message and exit.
    --output -o                       : Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query                           : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose                         : Increase logging verbosity. Use --debug for full debug logs.


Houd er rekening mee op de `-d` vlag voor het koppelen van afhankelijkheden: als u de naam van een map die nog niet gebundeld (zip, tar, enzovoort), die map automatisch tar'ed opgehaald en wordt doorgegeven samen en vervolgens automatisch zonder op het andere uiteinde. 

Als u in een map die al is gebundeld doorgeeft, we behandelen als een bestand en doorgeeft langs is. Het is niet mogelijk afzonderlijke automatisch; ervan uitgegaan dat u voor het afhandelen van die in uw code.

**Servicedetails van ophalen**

Details van de service-URL, gebruik (inclusief voorbeeldgegevens als een schema is gemaakt) waaronder ophalen.

`az ml service show realtime --name [service name]`

Opdrachtdetails van de:

    --id -i    : The service id to show.
    --name -n  : Webservice name.
    -v         : Verbosity flag.

Algemene argumenten

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.

**De service wordt uitgevoerd**

`az ml service run realtime -n [service name] -d [input_data]`

Opdrachtdetails van de:

    --id -i    : The service id to show.
    --name -n  : Webservice name.
    -v         : Verbosity flag.

Algemene argumenten

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.

Opdracht

    az ml service run realtime

Argumenten--id -i: [vereist] te beoordelen op basis van de service-id.
-d: de gegevens te gebruiken voor het aanroepen van de webservice.
-v: uitgebreidheid vlag.

Algemene argumenten

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.
