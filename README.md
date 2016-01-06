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

## License
Copyright 2016 EcoHealth Alliance

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
