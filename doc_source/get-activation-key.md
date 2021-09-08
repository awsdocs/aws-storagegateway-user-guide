# Getting an Activation Key for Your Gateway<a name="get-activation-key"></a>

To get an activation key for your gateway, you make a web request to the gateway VM and it returns a redirect that contains the activation key\. This activation key is passed as one of the parameters to the `ActivateGateway` API action to specify the configuration of your gateway\. The request you make to the gateway VM contains the AWS Region in which activation occurs\. 

The URL returned by the redirect in the response contains a query string parameter called `activationkey`\. This query string parameter is your activation key\. The format of the query string looks like the following: ` http://gateway_ip_address/?activationRegion=activation_region`\.

**Topics**
+ [AWS CLI](#get-activation-key-cli)
+ [Linux \(bash/zsh\)](#get-activation-key-linux)
+ [Microsoft Windows PowerShell](#get-activation-key-powershell)

## AWS CLI<a name="get-activation-key-cli"></a>

If you haven't already done so, you must install and configure the AWS CLI\. To do this, follow these instructions in the *AWS Command Line Interface User Guide:*
+ [ Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)
+ [ Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

The following example shows you how to use the AWS CLI to fetch the HTTP response, parse HTTP headers and get the activation key\.

```
wget 'ec2_instance_ip_address/?activationRegion=eu-west-2' 2>&1 | \
grep -i location | \
grep -i key | \
cut -d'=' -f2 |\
cut -d'&' -f1
```

## Linux \(bash/zsh\)<a name="get-activation-key-linux"></a>

The following example shows you how to use Linux \(bash/zsh\) to fetch the HTTP response, parse HTTP headers, and get the activation key\.

```
function get-activation-key() {
  local ip_address=$1
  local activation_region=$2
  if [[ -z "$ip_address" || -z "$activation_region" ]]; then
    echo "Usage: get-activation-key ip_address activation_region"
    return 1
  fi
  if redirect_url=$(curl -f -s -S -w '%{redirect_url}' "http://$ip_address/?activationRegion=$activation_region"); then
    activation_key_param=$(echo "$redirect_url" | grep -oE 'activationKey=[A-Z0-9-]+')
    echo "$activation_key_param" | cut -f2 -d=
  else
    return 1
  fi
}
```

## Microsoft Windows PowerShell<a name="get-activation-key-powershell"></a>

The following example shows you how to use Microsoft Windows PowerShell to fetch the HTTP response, parse HTTP headers, and get the activation key\.

```
function Get-ActivationKey {
  [CmdletBinding()]
  Param(
    [parameter(Mandatory=$true)][string]$IpAddress, 
    [parameter(Mandatory=$true)][string]$ActivationRegion
  )
  PROCESS {
    $request = Invoke-WebRequest -UseBasicParsing -Uri "http://$IpAddress/?activationRegion=$ActivationRegion" -MaximumRedirection 0 -ErrorAction SilentlyContinue
    if ($request) {
      $activationKeyParam = $request.Headers.Location | Select-String -Pattern "activationKey=([A-Z0-9-]+)"
      $activationKeyParam.Matches.Value.Split("=")[1]
    }
  }
}
```