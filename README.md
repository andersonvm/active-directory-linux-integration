# active-directory-linux-integration
Como integrar um computador com sistema Linux ao Active Directory

Integrar o Active Directory (AD) a sistemas Linux envolve o uso de ferramentas como o SSSD (System Security Services Daemon) para autenticação e autorização. Vou guiar você por um tutorial básico para realizar essa integração em um sistema Linux.

Passo a passo para integrar o Active Directory a sistemas Linux:

## Requisitos Iniciais:

Acesso administrativo: Certifique-se de ter privilégios de administrador no sistema Linux.
Conexão de rede: Garanta uma conexão de rede para acessar o Active Directory.

## Instalação de Pacotes Necessários:

SSSD e Kerberos: Instale os pacotes necessários no seu sistema Linux.

### No Ubuntu/Debian:

```
sudo apt-get update
sudo apt-get install sssd sssd-tools realmd adcli krb5-user -y
```

### No CentOS/RHEL:

```
sudo yum install sssd realmd oddjob oddjob-mkhomedir adcli samba-common-tools -y
```

## Configuração do Sistema para Integração:

### Conectar ao domínio do Active Directory:

```
sudo realm join -U administrator@NOME_DO_SEU_DOMINIO.EXEMPLO seu_dominio_exemplo
```

Substitua NOME_DO_SEU_DOMINIO.EXEMPLO pelo nome do seu domínio AD.

### Configuração do SSSD:

### Abra o arquivo de configuração do SSSD:

```
sudo vim /etc/sssd/sssd.conf
```

### Adicione as seguintes linhas ao arquivo:

```
[sssd]
domains = seu_dominio_exemplo
config_file_version = 2
services = nss, pam

[domain/seu_dominio_exemplo]
id_provider = ad
access_provider = ad
ad_domain = seu_dominio_exemplo
ad_server = controlador_do_dominio.seu_dominio_exemplo
```

Substitua seu_dominio_exemplo pelo nome do seu domínio AD e controlador_do_dominio.seu_dominio_exemplo pelo endereço do controlador de domínio.

### Reinicie o Serviço SSSD:

```
sudo systemctl restart sssd
```

Teste da Autenticação: Use id nome_de_usuario para verificar se o usuário do AD pode ser reconhecido no sistema Linux.

Acesso e Permissões: Ajuste as permissões e grupos do sistema conforme necessário para conceder acesso aos usuários do Active Directory.

## Notas Importantes:

Certifique-se de que o servidor Linux pode se comunicar com o controlador de domínio do Active Directory. Verifique as configurações de rede, DNS e firewall.

Faça backup dos arquivos de configuração antes de fazer alterações.
