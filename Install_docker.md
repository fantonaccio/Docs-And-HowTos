# Install Docker on Debian

#### Unistall olds versions
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```

#### Set up the repository

1. Update 

```
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release
```

2. Add Docker's official GPG Key:

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3. Add stable repositury

```
 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### Install Docker

1. Install docker comunity edition

```
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
```

2. Verify that Docker is installed

```
 sudo docker run hello-world
```

#### Install Docker compose

1. Download for binary

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2.  Apply executable permissions to the binary

```
sudo chmod +x /usr/local/bin/docker-compose
```
