# D42-PowerShell-Samples
Example script for running Windows PowerShell scripts against the Device42 API's


## Sample GET call using PowerShell
The below sample shows how to do a simple GET request from Device42. Supply your credentials and url.

```
$d42_username = "d42_user"
$d42_password = "d42_password"
$d42_headers = @{"Authorization" = "Basic "+[System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($d42_username+":"+$d42_password ))}
$d42_url = "https://d42_url/api/1.0/devices/id/10482/"


try {
   $result = Invoke-WebRequest -Uri $d42_url -Headers $d42_headers -Method Get
   $json = $result | ConvertFrom-Json
   Write-Output "Device Id = $($json.id)"
   Write-Output "Device UUID = $($json.uuid)"
} catch {
   $result = $_.Exception.Response.GetResponseStream()
   $reader = New-Object System.IO.StreamReader($result)
   $responseBody = $reader.ReadToEnd();
}
```
