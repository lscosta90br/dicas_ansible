## Generate an SSH Key
```sh
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/lc/.ssh/id_rsa): id_rsa_rdo
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in id_rsa_rdo.
Your public key has been saved in id_rsa_rdo.pub.
The key fingerprint is:
SHA256:GKW7yzA1J1qkr1Cr9MhUwAbHbF2NEGRTgZXeOUOz3Us rdo@klar
The key's randomart image is:
+---[RSA 2048]----+
|.*++ o.o.        |
|.+B + oo.        |
| +++ *+.         |
| .o.Oo.+E        |
|    ++B.S.       |
|   o * =.        |
|  + = o          |
| + = = .         |
|  + o o          |
+----[SHA256]-----+
```

## Copia a chave para o servidor
```sh
$ ssh-copy-id -i ~/.ssh/id_rsa-rdo.pub root@192.168.x.x
```

```sh

ssh -i ~/.ssh/lc_id_rsa lc@192.168.3.210 

# para utilizar de forma automatica
❯ ssh pop-os                              
Welcome to Pop!_OS 20.04 LTS (GNU/Linux 5.8.0-7642-generic x86_64)

 * Homepage: https://pop.system76.com
 * Support:  https://support.system76.com

Last login: Sun Mar 21 22:12:02 2021 from 192.168.x.x
rdo@pop-os:~$ 

```

# Arquivo de configuração ~/.ssh/config
```sh
ServerAliveInterval 120
ServerAliveCountMax 3

Host xpto_server
	HostName 192.168.x.x
	User root
	IdentityFile arquivo_com_chave_privada

```

## Fazendo o root logar no ubuntu server 20.04
```sh
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo service ssh restart

https://linuxconfig.org/allow-ssh-root-login-on-ubuntu-18-04-bionic-beaver-linux
```