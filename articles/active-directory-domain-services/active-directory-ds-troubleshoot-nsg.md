---
title: 'Azure Active Directory Domain Services: Felsökning Nätverkssäkerhetsgruppen konfiguration | Microsoft Docs'
description: Felsöka NSG konfigurationen för Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: ergreenl
ms.openlocfilehash: 2336277a960925a92af3578850453ba6ae78abda
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/04/2018
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Felsöka ogiltig nätverkskonfigurationen för din hanterade domän
Den här artikeln hjälper dig att felsöka och lösa nätverksrelaterade konfigurationsfel som resulterar i följande varningsmeddelande:

## <a name="alert-aadds104-network-error"></a>Varning AADDS104: Nätverksfel
**Varningsmeddelande:** *Microsoft kan inte nå domänkontrollanterna för den här hanterade domänen. Detta kan inträffa om en nätverkssäkerhetsgrupp (NSG) som har konfigurerats på din virtuella nätverk blockerar åtkomst till den hanterade domänen. En annan möjlig orsak är att om det finns en användardefinierad väg som blockerar inkommande trafik från internet.*

Ogiltig NSG konfigurationer är den vanligaste orsaken till nätverksfel för Azure AD Domain Services. Den Nätverkssäkerhetsgrupp (NSG) har konfigurerats för det virtuella nätverket måste tillåta åtkomst till [specifika portar](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services). Om dessa portar blockeras kan inte Microsoft övervaka eller uppdatera din hanterade domän. Dessutom påverkas synkroniseringen mellan Azure AD-katalogen och din hanterade domän. När du skapar din NSG att dessa portar hålls öppna för att undvika avbrott i tjänsten.

### <a name="checking-your-nsg-for-compliance"></a>Kontrollera din NSG för efterlevnad

1. Navigera till den [Nätverkssäkerhetsgrupper](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) sida i Azure-portalen
2. Välj NSG: N som är associerade med undernätet där din hanterade domän aktiveras från tabellen.
3. Under **inställningar** i den vänstra panelen klickar du på **inkommande säkerhetsregler**
4. Granska reglerna på plats och identifiera vilka regler som blockerar åtkomst till [portarna](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services).
5. Redigera NSG: N för att säkerställa kompatibilitet genom att ta bort regeln, lägger till en regel eller skapa en ny NSG helt. Steg för att [lägga till en regel](#add-a-rule-to-a-network-security-group-using-the-azure-portal) eller [skapa en ny, kompatibla NSG](#create-a-nsg-for-azure-ad-domain-services-using-powershell) är nedan.

## <a name="sample-nsg"></a>exempel NSG
Följande tabell visar ett exempel på en NSG som vill skydda din hanterade domän samtidigt som Microsoft för att övervaka, hantera och uppdatera informationen.

![exempel NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Azure AD Domain Services kräver obegränsad utgående åtkomst från det virtuella nätverket. Vi rekommenderar inte för att skapa alla ytterligare NSG-regler som begränsar utgående åtkomst för det virtuella nätverket.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Lägga till en regel till en Nätverkssäkerhetsgrupp med Azure-portalen
Om du inte vill använda PowerShell kan du manuellt lägga till regler som enda NSG: er med hjälp av Azure portal. Utför följande steg för att skapa regler i säkerhetsgrupp för nätverk:

1. Navigera till den [Nätverkssäkerhetsgrupper](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) sida i Azure-portalen
2. Välj NSG: N som är associerade med undernätet där din hanterade domän aktiveras från tabellen.
3. Under **inställningar** i den vänstra panelen klickar du antingen **inkommande säkerhetsregler** eller **utgående säkerhetsregler**.
4. Skapa regeln genom att klicka på **Lägg till** och fylla i informationen. Klicka på **OK**.
5. Kontrollera att regeln har skapats genom att söka efter tabellen.


## <a name="create-a-nsg-for-azure-ad-domain-services-using-powershell"></a>Skapa en NSG för Azure AD Domain Services med hjälp av PowerShell
Den här NSG är konfigurerad för att tillåta inkommande trafik till de portar som krävs av Azure AD Domain Services, medan andra oönskad inkommande åtkomst nekas.

**Nödvändig förutsättning: Installera och konfigurera Azure PowerShell** Följ instruktionerna för att [installera Azure PowerShell-modulen och ansluta till din Azure-prenumeration](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

>[!TIP]
> Vi rekommenderar att du använder den senaste versionen av Azure PowerShell-modulen. Om du redan har en äldre version av Azure PowerShell-modulen installerad uppdatera till den senaste versionen.
>

Använd följande steg för att skapa en ny NSG med hjälp av PowerShell.
1. Logga in på Azure-prenumerationen.

  ```PowerShell
  # Log in to your Azure subscription.
  Connect-AzureRmAccount
  ```

2. Skapa en NSG med tre regler. Följande skript definierar tre regler för NSG: N som tillåter åtkomst till de portar som behövs för att köra Azure AD Domain Services. Skriptet skapar sedan en ny NSG som innehåller dessa regler. Använd samma format för att lägga till ytterligare regler som tillåter andra inkommande trafik, om det krävs av arbetsbelastningar som distribuerats i det virtuella nätverket.

  ```PowerShell
  # Allow inbound HTTPS traffic to enable synchronization to your managed domain.
  $SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD -Description "Allow synchronization with Azure AD" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443

  # Allow management of your domain over port 5986 (PowerShell Remoting)
  $PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting -Description "Allow management of domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986

  #The following two rules are optional and needed only in certain situations.

  # Allow management of your domain over port 3389 (remote desktop).
  $RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD -Description "Allow management of domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix 207.68.190.32/27, 13.106.78.32/27, 10.254.32.0/20, 10.97.136.0/22, 13.106.174.32/27, 13.106.4.96/27 -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389

  # Secure LDAP rule, it is recommended to change the source address prefix to include only the IP addresses
  $SecureLDAPRule = New-AzureRmNetworkSecurityRuleConfig -Name SecureLDAP -Description "Allow access through secure LDAP port" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 104 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 636

  # Create the NSG with the rules above (if you need the remote desktop rule and secure ldap rule, add it below)
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDomainServices-NSG" -SecurityRules $SyncRule, $PSRemotingRule
  ```

3. Till sist ska koppla NSG: N till vnet och undernät föredrar.

  ```PowerShell
  # Find vnet and subnet
  $Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
  $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

  # Set the nsg to the subnet and save the changes
  $Subnet.NetworkSecurityGroup = $Nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
  ```

## <a name="full-script-to-create-and-apply-an-nsg-for-azure-ad-domain-services"></a>Fullständig skript för att skapa och använda en NSG för Azure AD Domain Services
>[!TIP]
> Vi rekommenderar att du använder den senaste versionen av Azure PowerShell-modulen. Om du redan har en äldre version av Azure PowerShell-modulen installerad uppdatera till den senaste versionen.
>

```PowerShell
# Change the following values to match your deployment
$ResourceGroup = "ResourceGroupName"
$Location = "westus"
$VnetName = "exampleVnet"
$SubnetName = "exampleSubnet"

# Log in to your Azure subscription.
Connect-AzureRmAccount

# Allow inbound HTTPS traffic to enable synchronization to your managed domain.
$SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD -Description "Allow synchronization with Azure AD" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

# Allow management of your domain over port 5986 (PowerShell Remoting)
$PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting -Description "Allow management of domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 5986

# The following two rules are optional and needed only in certain situations.

# Allow management of your domain over port 3389 (remote desktop).
$RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD -Description "Allow management of domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix 207.68.190.32/27, 13.106.78.32/27, 10.254.32.0/20, 10.97.136.0/22, 13.106.174.32/27, 13.106.4.96/27 -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

# Secure LDAP rule, it is recommended to change the source address prefix to include only the IP addresses
$SecureLDAPRule = New-AzureRmNetworkSecurityRuleConfig -Name SecureLDAP -Description "Allow access through secure LDAP port" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 104 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 636

# Create the NSG with the rules above (if you need the remote desktop rule and secure ldap rule, add it below)
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "AADDomainServices-NSG" -SecurityRules $SyncRule, $PSRemotingRule

# Find vnet and subnet
$Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

# Set the nsg to the subnet and save the changes
$Subnet.NetworkSecurityGroup = $Nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
```


## <a name="need-help"></a>Behöver du hjälp?
Kontakta produktteamet Azure Active Directory Domain Services för att [dela feedback eller support](active-directory-ds-contact-us.md).
