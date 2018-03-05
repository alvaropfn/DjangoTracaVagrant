# Tutorial de instalação do Vagrant
uma versão completa pode ser encontrada na pagina oficial do [Vagrant](https://www.vagrantup.com/intro/getting-started/install.html).

1. Fazer o [download](https://www.vagrantup.com/downloads.html) enstalar o Vagrant ou realizar a instalação direto pelo terminal
2. No terminal executar o seguinte comando deve mostrar a versão atualmente instalada

```
$ vagrant --version
```

3. Criar uma pasta para alocar a maquina virtual e entrar nela.
4. No terminal executar o comando abaixo ira inicializar e baixar um box vagrant dentro da pasta recem criada.

```
$ vagrant init
$ vagrant box add ubuntu/xenial64
```
um box não esta necessariamente preso a um tipo de maquina virtual mas nesse caso usaremos uma restrita ao [virtualbox](https://www.virtualbox.org/wiki/Downloads) que aqui é chamado de `provider` sendo necessario realizar o download para poder executar o `box`.

5. editar o aquivo `Vagrantfile` de odo a ficar igual a este ou fazer o download da configuração [provida](#link) 

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port",guest: 8000, host: 8080, host_ip: "127.0.0.1"
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y build-essential tcl
    cd /tmp
    
    wget http://download.redis.io/releases/redis-4.0.8.tar.gz
    tar xzf redis-4.0.8.tar.gz
    cd redis-4.0.8
    make
    
    sudo apt-get update
    sudo apt-get install -y python3-pip
    pip3 install --upgrade pip
    cd "/vagrant"
    pip3 install -r requeriments.txt
  SHELL
end
```
6. No terminal execute os seguintes comandos para levantar o `box` no seu respectivo `provider` e iniciar uma sessão ssh com ele.
```
$ vagrant up
$ vagrant ssh
```

7. Para sair da sessão va no terminal em que o `vagrant` foi inicializado
```
$ logout
```
este comando irá finalizar a conexao com o box mas a maquina ainda continuará em execução. para finaliza-la use o comando
```
$ vagrant halt
```