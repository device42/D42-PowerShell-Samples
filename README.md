# D42-PowerShell-Samples
Example script for running Windows PowerShell scripts against the Device42 API's


## Sample GET call using PowerShell
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

## Sample POST call using PowerShell

```
$d42_username = "d42_user"
$d42_password = "d42_password"
$d42_headers = @{"Authorization" = "Basic "+[System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($d42_username+":"+$d42_password ))}
$d42_url = "https://d42_url/api/1.0/devices/"
try {
   $pos_data = "uuid=$($json.uuid)&notes=Updated from PowerShell"
   $result = Invoke-WebRequest -Uri $d42_url -Headers $d42_headers -Body $pos_data -Method post
   $json_post = $result | ConvertFrom-Json
   Write-Output "Result:"
   Write-Output $json_post.msg
}
catch {
   $result = $_.Exception.Response.GetResponseStream()
   $reader = New-Object System.IO.StreamReader($result)
   $responseBody = $reader.ReadToEnd();
}
```

## Sample PUT call using PowerShell

```
$d42_username = "d42_user"
$d42_password = "d42_password"
$d42_headers = @{"Authorization" = "Basic "+[System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($d42_username+":"+$d42_password ))}
$d42_url = "https://d42_url/api/1.0/device/custom_field/"
try {
   $pos_data = "name=device_name&key=key_test&value=value_test"
   $result = Invoke-WebRequest -Uri $d42_url -Headers $d42_headers -Body $pos_data -Method put -ContentType "application/x-www-form-urlencoded"
   $json_post = $result | ConvertFrom-Json
   Write-Output "Result:"
   Write-Output $json_post.msg
}
catch {
   $result = $_.Exception.Response.GetResponseStream()
   $reader = New-Object System.IO.StreamReader($result)
   $responseBody = $reader.ReadToEnd();
   Write-Output $responseBody
}
write-output "comlete"
```

## Notes
Please substitute your Device42 credentials for *$d42_username* and *$d42_password* and your Device42 URL for *$d42_url*.

Default results are in JSON format so use *ConvertFrom-Json* to get JSON object for easier data manipulation. 

The *catch* block is used to handle errro results returned from Device42 and will be in JSON.
