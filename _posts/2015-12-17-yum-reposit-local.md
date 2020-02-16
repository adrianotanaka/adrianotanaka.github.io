---
title: "[Linux] Criando um repositório local YUM"
date: 2015-12-17 09:30:23 Z
categories:
- posts
---

**Antes de começarmos a configurar  o nosso repositório local, primeiro devemos saber o que é um repositório:**

Um repositório yum, nada mais é do que um local com diversos pacotes no formato RPM que é o formato que os programas das distribuições baseadas no Red Hat vem empacotados, esse local pode ser um diretório no disco (repositório local) ou do tipo remoto (FTP, HTTP ou HTTPS).
Vantagens de se instalar um software usando um repositório:

•	**Fácil manutenção:** Instalação, atualização ou remoção de pacotes são feitas de forma simples.

•	**Resolução de dependências:** as dependências de um pacote são resolvidas de forma automática.

**Configurando um repositório local:**

Nesse exemplo iremos precisar de uma ISO do sistema Oracle Linux.

Com o uso de um repositório local, podemos evitar a necessidade de uma conexão com a internet para instalar nossos pacotes.
Para a configuração de um repositório yum local, devemos antes de tudo criar um diretório para armazenar nossos pacotes .rpm:

`mkdir –p /etc/yum/repositorio_local`

Após o diretório criado, devemos mover o conteúdo da ISO para ele:

    cp /media/OL6.5\ x86_64\ Disc\ 1\ 20131125/Packages/* /etc/yum/repositorio_local/

    cp -R /media/OL6.5\ x86_64\ Disc\ 1\ 20131125/repodata /etc/yum/repositorio_local/

    cp  /media/OL6.5\ x86_64\ Disc\ 1\ 20131125/RPM-GPG-KEY* /etc/yum/repositorio_local/

Cuidado com essa operação de copia pois um grande espaço é necessário, na versão do Oracle Linux 6.5 são necessários 3.4Gb.

Assim que a copia for concluída, iremos configurar o arquivo que vai apontar o repositório local para o yum.

Com seu editor preferido, crie um arquivo com o nome que você deseja dentro do diretório `/etc/yum.repos.d/`

No meu caso, estou criando o seguinte arquivo:

`vi /etc/yum.repos.d/repositorio_local.repo`


Nesse arquivo iremos adicionar as seguintes informações:

    [LOCAL]
    name=Oracle Linux 6.5 Local
    baseurl=file:///etc/yum/repositorio_local/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY
    gpgcheck=1
    enabled=1

Onde a palavra entre `[]` significa um identificador do reposótorio.

`name` significa o nome do repositório.

`baseurl` é de onde ele vai buscar os pacotes, nesse campo podemos especificar um diretório local (file://caminho) ou um diretório em algum lugar da rede (ftp://, http:// ou https://)

`gpgkey` é a localização da chave publica GPG

<p>Se o parâmetro `gpgcheck` estiver definido como 1, o yum vai verificar a autenticidade dos pacotes consultando a chave pública, se você estiver instalando um pacote sem assinatura, esse parâmetro deve ser definido como 0, mas atente-se pois assim você pode instalar um pacote vunerável.</p>

Caso queira deixar o repositório configurado, mas que ele não seja usado pelo yum, basta definir o parâmetro `enabled` como 0.

Se existir algum arquivo de configuração de repositorio no diretório `/etc/yum.repos.d/` você deve ou excluir o arquivo ou definir o parametro `enabled` dele como 0.

Depois dessa primeira configuração, podemos verificar se o repositório esta disponível com o comando `yum repolist` :

    [root@oracle OL6.5 x86_64 Disc 1 20131125]# yum repolist
    Loaded plugins: refresh-packagekit, security
    repo id                                                                              repo name                                                                                           status
    LOCAL                                                                                Oracle Linux 6.5 Local                                                                              3,669
    repolist: 3,669

O próximo passo é instalar os pacotes necessários.
