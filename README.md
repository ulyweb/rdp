# rdp

Sure, I can help with that! To enable Remote Desktop with Network Level Authentication (NLA) via PowerShell, you can follow these steps:

1. **Open PowerShell with administrative privileges**:
   - Press `Win + X` and select **Windows PowerShell (Admin)**.

2. **Run the following PowerShell command**:
   ```powershell
   Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name 'fDenyTSConnections' -value 0
   ```
   This command enables Remote Desktop connections.

3. **Enable Remote Desktop through the firewall**:
   ```powershell
   Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
   ```
   This command allows Remote Desktop connections through the Windows Firewall.

4. **Restart your computer** to apply the changes.

If the option is still greyed out, it might be due to domain policies or other system configurations. In that case, you might need to check with your system administrator.

Does this help resolve your issue?







```
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name 'fDenyTSConnections' -value 0
# Enable Network Level Authentication
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name 'UserAuthentication' -Value 1
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```



```
# Enable Remote Desktop connections
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name 'fDenyTSConnections' -value 0

# Enable Network Level Authentication
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name 'UserAuthentication' -Value 1

# Allow logon through Remote Desktop Services
$RDPGroup = "Remote Desktop Users"
$SID = (New-Object System.Security.Principal.NTAccount($RDPGroup)).Translate([System.Security.Principal.SecurityIdentifier]).Value
$policyList = (Get-WmiObject -Namespace "ROOT\RSOP\Computer" -Class "RSOP_UserPrivilegeRight" | Where-Object { $_.UserRights -match "SeRemoteInteractiveLogonRight" }).AccountList
if ($policyList -notcontains $SID) {
    $policyList += $SID
    (Get-WmiObject -Namespace "ROOT\RSOP\Computer" -Class "RSOP_UserPrivilegeRight" | Where-Object { $_.UserRights -match "SeRemoteInteractiveLogonRight" }).Put()
}

# Enable Remote Desktop through the firewall
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```
