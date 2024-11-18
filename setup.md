### Generate your key: 
```
ssh-keygen -t rsa -b 4096 -C your_email@abc.com
(should be named according to the service e.g. github_rsa)
(you can leave phassphrase empty it is like another layer of security you will be asked for password when key will be used.)
```
```
Copy the PUBLIC key from ~/.ssh/your_stored_key.pub
Paste your PUBLIC key to the github.com to settings for example or other platfrom. 
Or if you are connecting to other machine use this commands:

### for Windows:

type $env:USERPROFILE\.ssh\your_stored_key.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"

### for Linux:

ssh-copy-id -i ~/.ssh/your_stored_key.pub user@host
```
```
Create config file, especially good when you have more keys stored and used. 
vi config in .ssh directory
When you are using HTTPS you have to generate access token on github.com and use it instead of your regular password.
```
### Example:
```
Host github.com				          
User git
Port 22
Hostname github.com
IdentityFile ~/.ssh/your_stored_key (without .pub)
TCPKeepAlive yes
IdentitiesOnly yes
```
```
When using .pem file u can use:

eval $(ssh-agent -s) -> start SSH agent.
ssh-add /path/to/.pem -> add key identity to agent.

U can add this lines to .bashrc for automatic start and add identity.
```
