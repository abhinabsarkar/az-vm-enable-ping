# Enable ping (ICMP) on Azure VM
The following steps are required to enable the ping on Azure VM.
### Configure the operating system to answer to Ping/ICMP echo request
```cmd
#For IPv4
netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol="icmpv4:8,any" dir=in action=allow
 
#For IPv6
netsh advfirewall firewall add rule name="ICMP Allow incoming V6 echo request" protocol="icmpv6:8,any" dir=in action=allow
```
### Configure Network Security Group (NSG) to allow ICMP traffic

Add a new inbound port rule for the Azure network security group (NSG).
Command in Azure CLI
```bash
az network nsg rule create -g $rgName --nsg-name AzureVM-WIN01-nsg -n ICMP-Ping \
    --priority 100 --source-address-prefixes VirtualNetwork --destination-address-prefixes '*' \
    --destination-port-ranges '*' --direction Inbound --access Allow --protocol ICMP --description "Allow Ping"
```
Command in PowerShell
```ps
Get-AzNetworkSecurityGroup -Name "AzureVM-WIN01-nsg" | Add-AzNetworkSecurityRuleConfig -Name ICMP-Ping -Description "Allow Ping" -Access Allow -Protocol ICMP -Direction Inbound -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange * | Set-AzNetworkSecurityGroup
```
![Alt text](/images/nsg.png)

## References
* [How to enable Ping (ICMP echo) on an Azure VM](https://www.thomasmaurer.ch/2019/09/how-to-enable-ping-icmp-echo-on-an-azure-vm/)
