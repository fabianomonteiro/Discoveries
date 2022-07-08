# Discoveries

$Folder = "C:\Teste\wwwroot"
$User = "IIS_IUSRS"
$permission = (Get-Acl $Folder).Access | ?{$_.IdentityReference -match $User} | Select IdentityReference,FileSystemRights
If (!$permission){
    #$permission | % {Write-Host "User $($_.IdentityReference) has '$($_.FileSystemRights)' rights on folder $folder"}
    #Write-Host "$User Doesn't have any permission on $Folder"

    $permissions = $User, 'ReadAndExecute', 'ContainerInherit,ObjectInherit', 'None', 'Allow'
    # Create a new FileSystemAccessRule object
    $rule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permissions

    $acl = Get-Acl $Folder
    #$rule = New-Object  System.Security.Accesscontrol.FileSystemAccessRule("IIS_IUSRS","Read","Allow")
    $acl.SetAccessRule($rule)
    Set-Acl $Folder $acl
}
#Else {
    #Write-Host "$User has permission on $Folder"
#}
