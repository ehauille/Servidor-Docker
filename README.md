# Configuração do sevidor em CentOS 8 para Zabbix:

# Instalar atualizações
    yum update -y

# Instalar utilitários
    yum install -y net-tools vim nano wget curl tcpdump yum-utils sysstat ntp

# Instalar EPEL Release para HTOP
    rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    yum install htop -y

# Verificar o timezone atual
    timedatectl status

# Definir timezone
    timedatectl set-timezone your_time_zone

# Para listar todos os timezone disponíveis
    timedatectl list-timezones | grep your_time_zone

# Validar alteração do timezone
    timedatectl status

# Verifique a data e hora atual
    date

# Configurar regra de firewall NTP
    firewall-cmd --add-service=ntp --permanent
    firewall-cmd --reload

# Iniciar e Habilitar o NTP
    systemctl enable --now ntpd

# Validar os ntp servers disponível
    ntpq -p

# Sincronizar data e hora
    systemctl restart ntpd

# Validar status SELINUX
    sestatus

# Editar o arquivo de configuração e desabilitar o SELINUX
    vim /etc/selinux/config
    SELINUX=disabled

- É necessario reiniciar o servidor

# Habilitar SYSSTAT
    systemctl enable --now sysstat

# Validar perfomance S.O
    sar 
    
# Instalação do Docker
    yum remove docker \
                      docker-client \
                      docker-client-latest \
                      docker-common \
                      docker-latest \
                      docker-latest-logrotate \
                      docker-logrotate \
                      docker-engine

    yum install device-mapper-persistent-data lvm2 -y

    yum-config-manager \
        --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo

    yum update -y

    yum install -y docker-ce docker-ce-cli containerd.io

    systemctl start docker
    systemctl enable docker

    systemctl status docker

# Instalacao Docker Compose
    curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    docker-compose --version
    curl \
        -L https://raw.githubusercontent.com/docker/compose/1.29.2/contrib/completion/bash/docker-compose \
        -o /etc/bash_completion.d/docker-compose
    bash

# REGRAS DE FIREWALL DOCKER
    firewall-cmd --permanent --add-masquerade 
    firewall-cmd --permanent --add-port=2376/tcp
    firewall-cmd --permanent --add-port=2377/tcp
    firewall-cmd --permanent --add-port=7946/tcp
    firewall-cmd --permanent --add-port=7946/udp
    firewall-cmd --permanent --add-port=4789/udp
    firewall-cmd --permanent --add-port=80/tcp
    firewall-cmd --permanent --add-port=443/tcp
    firewall-cmd --permanent --add-port=10051/tcp
    firewall-cmd --permanent --add-port=10050/tcp
    firewall-cmd --reload

# INICIAR CLUSTER DOCKER
    docker swarm init
    docker node ls

# Instalar MySql 8 CLIENT
    Add Repo
    rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
    sed -i 's/enabled=1/enabled=0/' /etc/yum.repos.d/mysql-community.repo
    yum --enablerepo=mysql80-community install mysql
    ![image](https://user-images.githubusercontent.com/64754364/126711575-d0f55b05-79d7-45dd-a302-b008629caf64.png)
