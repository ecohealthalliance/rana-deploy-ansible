Ansible deployment for Rana, the Ranavirus reporting interface.

### Deploying to an AWS instance

Edit the inventory.ini file to add instance ip addresses you want to deploy to.
Then run a command like this:

```
ansible-playbook site.yml -i inventory.ini --vault-password-file ~/.rana_vault_password --private-key ~/.keys/rana-dev.pem
```

### Restoring data from a backup

On the server, while logged in as rana, run commands like these:

```
aws s3 cp --recursive s3://mantle-data/rana/ restore-data
mongorestore restore-data --drop
```

### Giving an user administrator privileges

SSH into the server and open the mongo shell:
```
mongo
```
The rana database should contain all the collections for the application:
```
use rana
show collections
```
Check that the user has registered a GRRS accounts:
```
db.users.find({"emails.address":"example@ecohealthalliance.org"}).pretty()
```
Get the id of the rana group:
```
ranaGroupId = db.groups.findOne({path:"rana"})._id
```
Update the user's profile with the admin role for the rana group:
```
modifierObj = {}
modifierObj["roles." + ranaGroupId] = "admin"
db.users.update({"emails.address":"example@ecohealthalliance.org"}, {$addToSet: modifierObj})
```
