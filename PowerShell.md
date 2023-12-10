See also [Terminal](terminal.md)

## Install PowerShell on macOS

```zsh
brew install powershell
```

## Basics

* check version

```ps1
$PSVersionTable
```

### Configure shells and terminals

* this assumes you have installed `homebrew` and `iterm2` from https://github.com/artmg/macUP/blob/main/terminal.md

```zsh
# recheck local locations
# Brew files
brew shellenv
# validate that the powershell line below is still current
brew info powershell
# PowerShell binary
whereis pwsh
brew --prefix oh-my-posh
```

* Open PowerShell

```powershell
# Make brew work from powershell
If (!(Test-Path $PROFILE.CurrentUserAllHosts)) {New-Item -Path $PROFILE.CurrentUserAllHosts -Force}
Add-Content -Path $PROFILE.CurrentUserAllHosts -Value '$(/opt/homebrew/bin/brew shellenv) | Invoke-Expression'

```

```zsh
# credit https://halfblood.pro/powershell-on-macos
# oh-my-posh uses Nerd Fonts
brew tap homebrew/cask-fonts && brew install --cask font-meslo-lg-nerd-font
brew install jandedobbeleer/oh-my-posh/oh-my-posh

# help https://iterm2.com/documentation-dynamic-profiles.html
cat <<EndOfProfile >> $HOME/Library/Application\ Support/iTerm2/DynamicProfiles/PowerShell.json
{
  "Profiles": [
    {
	  "Name" : "PowerShell",
	  "Description" : "231126.AMG",
	  "Normal Font" : "MesloLGMNFM-Regular 13",
	  "_comment Working Directory" : "\/Users\/username",
	  "Custom Command" : "Yes",
	  "Command" : "\/usr\/local\/bin\/pwsh -l -nol",
	  "Guid" : "68341DCE-A541-4746-A605-0C51641F1139",
    },
  ]
}
EndOfProfile

```

* set up posh in pwsh
	* could move this earlier?
	* alternative themes: https://ohmyposh.dev/docs/themes
```powershell
Add-Content -Path $PROFILE.CurrentUserAllHosts -Value 'oh-my-posh init pwsh --config /opt/homebrew/opt/oh-my-posh/themes/cloud-native-azure.omp.json | Invoke-Expression'
```

### Integrated development environment

You can just use a text editor, and even lightweight Geany has code highlighting
and execution directly into a terminal. However, this assumes you want to use
a more richly-featured editor like Visual Studio Code.

```zsh
# install your development environment (code editor) 

brew install visual-studio-code 

code --install-extension ms-vscode.powershell 
```

* Open VS Code
* Cmd-Shft-P for command palette
* JS - to find Preferences: Open user settings (JSON)
* up to you whether you go for internal or external
	* internal works ok with oh-my-posh

```json
{
    "terminal.external.osxExec": "iTerm.app",
    "terminal.integrated.fontFamily": "MesloLGM Nerd Font Mono",
    "NOT_ACTIVE.terminal.explorerKind": "external"
}
```

* These are credit https://stackoverflow.com/a/38494284
* for more ideas see https://stackoverflow.com/q/70837048


* NB: if you use Oh-my-Zsh in your shell, and it needs update it, this can make the terminal exit unexpectedly.


### Use iTerm for PowerShell from Launchpad

Despite the fact you can open iTerm then open a new tab with PowerShell Profile, 
you might find yourself using the launchpad app icon.
Fortunately https://github.com/PowerShell/PowerShell/issues/19110#issuecomment-1421219051 makes it clear that the 'app icon' is merely a shell script, so...

```
sudo mv /Applications/PowerShell.app/Contents/MacOS/PowerShell.sh{,.OLD}
sudo tee /Applications/PowerShell.app/Contents/MacOS/PowerShell.sh <<EndPsSh
#!/opt/homebrew/bin/zsh
osascript <<EOF
    tell application "iTerm2"
         create window with profile "PowerShell"
    end tell
EOF
EndPsSh
sudo chmod +x /Applications/PowerShell.app/Contents/MacOS/PowerShell.sh
```

NB: credit https://stackoverflow.com/a/75453078 but this is based on [AppleScript, now deprecated by iTerm2](https://iterm2.com/documentation-scripting.html) so they say you should switch to python

First time you launch this you need to give zsh control over iTerm app.

## Syntax Tips

* using 'splatting' to pass multiple parameters
	* https://stackoverflow.com/a/24313253
	* https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_splatting
* about Hash Tables
	* The official explanation: https://learn.microsoft.com/en-us/powershell/scripting/learn/deep-dives/everything-about-hashtable
	* A quick reference: https://ss64.com/ps/syntax-hash-tables.html
	* For more explanation: https://www.sharepointdiary.com/2021/10/hash-tables-in-powershell.html
* All about $null: https://learn.microsoft.com/en-us/powershell/scripting/learn/deep-dives/everything-about-null
* 


## Extensions

### Microsoft 365 administration
#### SharePoint Online

##### Preparation

```powershell
Install-Module PnP.PowerShell
# credit https://www.sharepointdiary.com/2021/02/how-to-install-pnp-powershell-module-for-sharepoint-online.html

# it is ok to accept this 'untrusted repo' as it comes from MS:
# https://learn.microsoft.com/en-us/powershell/sharepoint/sharepoint-pnp/sharepoint-pnp-cmdlets
# 
# was
# Install-Module SharePointPnPPowerShellOnline
```

##### Authentication

This code has been set to use `Connect-PnPOnline` with the `-DeviceLogin` option. 
This allows you to use the browser (and profile/person) of your choice, 
independently of how and where you run this code. 

NB: for many site and library operations, you might need to use credentials 
with Admin privileges over the site or library, not just a Member or Owner. 
This seems to be the case when reading (e.g. export) too, 
as well as when writing (import).

To re-authenticate with different credentials, use ` Disconnect-PnPOnline `.

For some commands, you may need to log on to the admin site instead, e.g.  
```powershell
# parameters you might want to set manually
$Env:SHAREPOINT_SUBDOMAIN = "MySubdomain"
$SiteUrl = "mysite"

$SharePointUrl = "https://$Env:SHAREPOINT_SUBDOMAIN.sharepoint.com/sites/$SiteUrl"

Connect-PnPOnline $SharePointUrl -DeviceLogin
$MySite = Get-PnPSite
$MySite | Select *

# For more complex connections, like getting the Tenant Site (admin site)
# with Device Login credit - https://github.com/pnp/powershell/issues/40#issuecomment-742763792

$SiteConnection = Connect-PnPOnline -Url "https://$Env:SHAREPOINT_SUBDOMAIN.sharepoint.com" -PnPManagementShell -ReturnConnection
$AdminConnection = Connect-PnPOnline -Url "https://$Env:SHAREPOINT_SUBDOMAIN-admin.sharepoint.com" -PnPManagement -ReturnConnection

# this will only get the top level site
$MySite = Get-PnPSite -Connection $SiteConnection
$MySite | Select *


# attach to the specific site and make it so the default document library may be deleted
Connect-PnPOnline -Url $SharePointUrl -Connection $SiteConnection -PnPManagementShell
$ListName = "Documents"
$MyList = Set-PnPList -Identity $ListName -AllowDeletion $true


Get-PnPTenantSite -Identity $SharePointUrl -Connection $AdminConnection | Select -Property *

# Set the site collection links to default to existing people
# Equivalent of 
#   * More Sharing Settings
#   * Default Sharing Link: UNCHECK 
#   * select People with Existing Access 

Set-PnPTenantSite -Identity $SharePointUrl -Connection $AdminConnection -DefaultLinkToExistingAccess $true
```
#### Exchange Online

* Run PowerShell as superuser: `sudo pwsh`

```powershell
Install-Module -Name PSWSMan
Install-WSMan
Install-Module -Name ExchangeOnlineManagement
```
* restart your PowerShell
* 

```powershell
Import-Module ExchangeOnlineManagement

Connect-ExchangeOnline -Device -UserPrincipalName <UPN>
```

The `-Device` switch allows you to use a code into your preferred browser instance
