$WorkFolder = "D:\USIS\AutomatedScripts\work-folder" # Main folder for cloning and backup

$LocalGitRepo = "$WorkFolder\tempgitrepo\$(get-date -f yyyyMMddTHHmmssZ)" # folder for temporary git clone with date

$BackupFolder = "$WorkFolder\backups" # backup folder

$WebsiteDocumentRoot = "C:\xampp\htdocs\sampleweb" # server folder

$NewBackupFolder = "$BackupFolder\$(get-date -f yyyyMMddTHHmmssZ)" # date for backup folder


cd $WorkFolder 

##  Remove-Item -Path "$WorkFolder\*.*" -Hidden -Recurse
##  Get-ChildItem -Path $LocalGitRepo -Include * | remove-Item -Recurse 

##  Get-ChildItem $LocalGitRepo -Hidden -Recurse | Remove-Item -Force -Verbose

mkdir $LocalGitRepo #making temporary clone from github
git clone https://github.com/barath-at-github/simple-webpage.git $LocalGitRepo

mkdir $NewBackupFolder #making backup file

Copy-item -Force -Recurse -Verbose "$WebsiteDocumentRoot\*" -Destination $NewBackupFolder #taking backup from server

Copy-item -Force -Recurse -Verbose "$LocalGitRepo\*" -Destination $WebsiteDocumentRoot #copying files from temporary folder to server

Remove-Item -Path $LocalGitRepo -Recurse #removing files in temporary folder

Get-ChildItem $LocalGitRepo -Hidden -Recurse | Remove-Item -Force -Verbose #removing hidden files in temporary folder
