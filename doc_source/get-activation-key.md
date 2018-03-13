# Getting Activation Key<a name="get-activation-key"></a>

To get the activation key for your gateway, you make a web request to the gateway VM and it returns a redirect that contains the activation key\. This activation key is passed as one of the parameters to the ActivateGateway API to specify the configuration of your gateway\. The request you make to the gateway VM contains the activation region, and the url returned by the redirect in the response contains a query string parameter called `activationkey`\. This query string parameter is your activation key\. The format of the query string looks like: ` http://gateway_ip_address/?activationRegion=activation_region`\.

The following example shows you how to fetch the HTTP response, parse HTTP headers and get the activation key\.

Linux \(bash/zsh\) example

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

Windows \(PowerShell\) example

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