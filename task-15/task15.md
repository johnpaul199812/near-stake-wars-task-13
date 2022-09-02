i tested Kuumatod on localnet and was playing around with the failover following this guide https://github.com/kuutamolabs/kuutamod/blob/main/docs/run-localnet.md

installed Consul on my Ubuntu machine with these command

```
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

```
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

```
sudo apt update && sudo apt install consul
```

used my previous guide to install neard, stopped at checking the version
https://github.com/johnpaul199812/NEAR-STAKEWARS

found out that i need to install Brew on my ubuntu machine before i can use the command brew to install hivemind which is needed in running kuumatod
used this commands to install brew

```
sudo apt-get install build-essential procps curl file git
```

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/ubuntu/.profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

now need to verify the installation

```
brew doctor
```

now i can use brew to install hivemind

```
brew install hivemind
```

python is needed, so i have to install it

```
sudo apt-get install python-is-python3
```

i install NIX 

```
sh <(curl -L https://nixos.org/nix/install) --daemon
```

restarted my shell after the installation and check the version

```
nix-env --version
```

i clone the kuumtamod repo

```
git clone https://github.com/kuutamolabs/kuutamod
cd kuutamod
```

i installed it with nix

```
nix develop
```

after it was done, i started consul with this

```
hivemind
```

got thses log after sometimes

![image](https://user-images.githubusercontent.com/42779023/188118320-b9b278d1-bd92-47a7-ba72-3c0ca6ef7aa4.png)

later this log

![image](https://user-images.githubusercontent.com/42779023/188120687-e4aceced-17a0-473d-8b74-cf0f1363d5f2.png)

and this

![image](https://user-images.githubusercontent.com/42779023/188122132-911ef7e1-9c91-47c9-874e-5f33326031db.png)


opened a another shell for adding the second node

```
cd kuutamod
nix develop
```

then i run this

```
./target/release/kuutamod --neard-home .data/near/localnet/kuutamod0/ \
  --voter-node-key .data/near/localnet/kuutamod0/voter_node_key.json \
  --validator-node-key .data/near/localnet/node3/node_key.json \
  --validator-key .data/near/localnet/node3/validator_key.json \
  --near-boot-nodes $(jq -r .public_key < .data/near/localnet/node0/node_key.json)@127.0.0.1:33301
  ```
  
  this was the log
  
  ![image](https://user-images.githubusercontent.com/42779023/188122328-c4c6b44f-ea39-4b67-8b5c-6b7939407364.png)


i killed the first node

![image](https://user-images.githubusercontent.com/42779023/188122492-9f66f60d-6353-47c3-ad36-65bd41be3c93.png)


second node took over

![image](https://user-images.githubusercontent.com/42779023/188122615-ee80330a-0f37-4124-b399-7b318eafa3e1.png)


i stopped here, i had issues setting AWS account, could have proceed with the 2nd part




