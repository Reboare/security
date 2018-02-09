# Lightweight Directory Access Protocol

The Lightweight Directory Access Protocol \(LDAP\)

## Querying

Finding computers and devices attached to the domain:

```powershell
$domaininfo = new-object DirectoryServices.DirectoryEntry("LDAP://dc.example.local/OU=People,DC=example,DC=local","DOMAIN\LDAPUser","pass123")
$searcher = New-Object System.DirectoryServices.DirectorySearcher($domaininfo)  
$searcher.Filter ="((objectCategory=computer))" 
$searcher.FindOne().properties
$searcher.FindAll().properties
```



