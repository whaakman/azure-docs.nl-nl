## <a name="prerequisites"></a>Vereisten

Als u dat nog niet heeft gedaan, kunt u een [Azure proefabonnement](https://azure.microsoft.com/pricing/free-trial/) aanvragen en de [Azure CLI 1.0](../articles/cli-install-nodejs.md) [koppelen aan uw Azure-account](../articles/xplat-cli-connect.md). Ga als volgt te werk om te controleren of de Azure-CLI zich in de modus Resource Manager bevindt:

```azurecli
azure config mode arm
```

## <a name="create-the-scale-set"></a>De schaalset maken

Maak nu de schaalset met behulp van de opdracht `azure vmss quick-create`. In het volgende voorbeeld wordt een Linux-schaalset gemaakt genaamd `myVMSS` met vijf VM-voorbeelden in de resourcegroep met de naam `myResourceGroup`:

```azurecli
azure vmss quick-create -n myVMSS -g myResourceGroup -l westus \
    -u ops -p P@ssw0rd! \
    -C 5 -Q Canonical:UbuntuServer:16.04-LTS:latest
```

In het volgende voorbeeld wordt een Windows-schaalset gemaakt met dezelfde configuratie:

```azurecli
azure vmss quick-create -n myVMSS -g myResourceGroup -l westus \
    -u ops -p P@ssw0rd! \
    -C 5 -Q MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest
```

Wilt u de locatie of image-urn aanpassen, bekijk dan de opdrachten `azure location list` en `azure vm image {list-publishers|list-offers|list-skus|list|show}`.

Deze opdracht wordt geretourneerd als de schaalset klaar is met implementeren. Deze schaalset heeft een load balancer met NAT-regelstoewijzingspoort 50,000+i op de load balancer naar poort 22 op VM i. Dus nadat we de FQDN van de load balancer hebben achterhaald, kunnen we via SSH verbinding maken met onze virtuele machines:

```bash
# (if you decide to run this as a script, please invoke using bash)

# list load balancers in the resource group we created
#
# generic syntax:
# azure network lb list -g RESOURCE-GROUP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb list -g negatvmssrg | grep negatvmssrg)
split_line=( $line )
lb_name=${split_line[1]}

# now that we have the name of the load balancer, we can show the details to find which Public IP (PIP) is 
# associated to it
#
# generic syntax:
# azure network lb show -g RESOURCE-GROUP-NAME -n LOAD-BALANCER-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb show -g negatvmssrg -n $lb_name | grep loadBalancerFrontEnd)
split_line=( $line )
pip_name=${split_line[4]}

# now that we have the name of the public IP address, we can show the details to find the FQDN
#
# generic syntax:
# azure network public-ip show -g RESOURCE-GROUP-NAME -n PIP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network public-ip show -g negatvmssrg -n $pip_name | grep FQDN)
split_line=( $line )
FQDN=${split_line[3]}

# now that we have the FQDN, we can use ssh on port 50,000+i to connect to VM i (where i is 0-indexed)
#
# example to connct via ssh into VM "0":
ssh -p 50000 negat@$FQDN
```