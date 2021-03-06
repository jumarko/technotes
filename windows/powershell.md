# Windows PowerShell

## Introduction

Basic stuff:

- Remember to use TAB a lot for auto-completing commands (who likes writing too much anyway)
- You are no longer able to run executables with "filename", you need to do ".\filename" (equivalent of ./filename in unix)
  - Autocomplete is your friend here as well

### Using Profile

Where your profile location:

	$PROFILE

Reloading your profile:

	. $PROFILE

More stuff: <http://msdn.microsoft.com/en-us/library/windows/desktop/bb613488%28v=vs.85%29.aspx>

## Aliases

List aliases:

	Get-Alias 

Add alias:

	Set-Alias [alias] [command]

Example:

	Set-Alias lc longcommandname

Exporting to file:

	Export-Alias -Path aliases.txt

Importing from file:

	Import-Alias -Path aliases.txt

You can add this command or individual Set-Alias commands to your PowerShell profile!

### Managing modules with PsGet

Search and install PowerShell modules easy: <http://psget.net/>

Note that you may need to change your execution policy to allow using PsGet.
(You need to run this as administrator):

	Set-ExecutionPolicy RemoteSigned

Now install PsGet:

	(new-object Net.WebClient).DownloadString("http://psget.net/GetPsGet.ps1") | iex

## Modules

### PsUrl - E.g. wget for PowerShell

Install it with PsGet:

	install-module PsUrl
	
**Important: you need to import module before it can be used (otherwise you just get an error):**

	import-module PsUrl

Simple `wget` style get URL:

	get-url http://example.com

Get and save to file:

	get-url http://example.com > example.html

More cool stuff:

	https://github.com/chaliy/psurl/

### Using Git from PowerShell

#### Getting Started

Verify `git/bin` (or whatever is the location of
bin folder under the place you installed git) is in your `PATH`

#### Prompt for Git repositories, tab completion (posh-git)

Posh-git adds some sugar. Features:

- Prompt for Git repositories
- Tab completion for common commands when using git

Install it:

	Install-Module posh-git

If that doesn't work out, check out details at
<https://github.com/dahlbyk/posh-git>.

Also you can try reloading your profile:

	. $PROFILE

### Using Python's Virtualenv from PowerShell

System's execution policies need to be relaxed.
Run this in PowerShell (as administrator):

	Set-ExecutionPolicy AllSigned

For more details on execution policies, type:

	Get-Help Set-ExecutionPolicy

#### Setting up SSH agent for PowerShell

SSH agent is basically a program that remember your SSH
keyphrases so you don't have to repeat them every time
you perform a git operation requiring authentication.

Get the SSH agent management snippet. For details see: http://markembling.info/2009/09/ssh-agent-in-powershell

When you have installed it, run:

	Start-SshAgent

Next time you start your PowerShell the SSH agent is
going to be up and running. It's quite allright
to leave it that way. 

When you want to stop the agent, just run:

	Stop-SshAgent

Note that as long as you want it to remember your
passphrases, you need to keep it running.

## System Administration

### Managing PATH

What is in your path:

    $env:path
    
Add to path:

    $env.path+"C:\Program Files\My Program\bin"
    
Update registry:

    Set-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name PATH –Value $env:path

### Managing Services

List services:

	Get-Service

More stuff: http://www.youtube.com/watch?v=I8fyElEGo60&feature=related

## Creating Your Own Scripts

Example (`helloworld.ps1`):

	"Hello World!"

Example 2: Using Input Parameters:

	param($Param1, $Param2)
	"You specified following parameters:"
	"$Param1"
	"$Param2"

## Recipes

### Writing Errors to Console

	$Host.UI.WriteErrorLine("Your error message")

### Find text in files

	dir -r -i *.* | select-string "mytext"

### Alert WIndow

	[System.Windows.Forms.MessageBox]::Show("Add your alert message here")

### Current Directory vs. Current Location

- When you `cd` PowerShell doesn't change current working directory
- This may become problematic on many occassions

More notes: <http://huddledmasses.org/powershell-power-user-tips-current-directory/>

### Greater than, smaller than

Greater than comparisons:

	$a -gt $b

Smaller than:

	$a -lt $b

Etc.

### Find files by name

    gci -recurse -filter "hosts"
    
## Literature

Bruce Payette. (2007). Windows PowerShell in Action. Manning. <http://manning.com/payette/>
