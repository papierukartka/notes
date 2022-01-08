# Windows snippet cookbook

## table of contents

- [Disable password expiration](#Disable-password-expiration)
- [Change user password](#Change-user-password)
- [Disable windows defender](#Disable-windows-defender)
- [Dont't ask for password on start](#Enable-autologin)

## snippets

### Disable password expiration

```powershell
Set-LocalUser -Name "localusername" -PasswordNeverExpires 1
```

### Change user password

#### Local user

```batch
net user localusername "NEWPASS QUOTEIFSPACESPRESENT"
net user USERNAME *  # prompts for password twice
```

#### Domain user

```powershell

```

### Disable Windows defender

#### Until next reboot

```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```

```yaml
- gpedit.msc
	- Computer Configuration
		- Administrative Templates
			- Windows Components
				- Microsoft Defender Antivirus
					- Turn off ... -> Enabled
```

#### Until next reboot, and a little longer (not permanently)

```powershell
$regpath = HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender"
Set-ItemProperty $regpath "DisableAntiSpyware" -Value "1" -type DWord

$regpath = HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender\Real-Time Protection"
Set-ItemProperty $regpath "DisableBehaviorMonitoring" -Value "1" -type DWord
Set-ItemProperty $regpath "DisableOnAccessProtection" -Value "1" -type DWord
Set-ItemProperty $regpath "DisableScanOnRealtimeEnable" -Value "1" -type DWord
```

### Enable autologin

```powershell
$userName = "LOCALUSERNAME"
$password = "PASSWORD"
$regpath = "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"

Set-ItemProperty $regpath "AutoAdminLogon" -Value "1" -type String
Set-ItemProperty $regpath "DefaultUsername" -Value "$defaultUsername" -type String
Set-ItemProperty $regpath "DefaultPassword" -Value "$defaultPassword" -type String
```