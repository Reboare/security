# Lightweight Directory Access Protocol

The Lightweight Directory Access Protocol \(LDAP\)

## Querying

Finding computers and devices attached to the domain:

```powershell
$domaininfo = new-object DirectoryServices.DirectoryEntry("LDAP://dc.example.local/OU=People,DC=example,DC=local","DOMAIN\LDAPUser","pass123")
$searcher = New-Object System.DirectoryServices.DirectorySearcher($domaininfo)  
$searcher.Filter ="((objectCategory=computer))" 
$searcher.FindOne().properties # Will grab and display the top of the search results
$searcher.FindAll().properties # Will grab and display all the search results
```



