# Como esconder Banner do SSH

## Conexto na segurança

Uma etapa muito importante durante um ataque é a coleta de informações. Essa coleta é feita de várias formas, mas básicamente, consiste em saber as versões dos serviços que estão ativos no alvo, e pesquisar sobre possíveis vulnerabilidades e CVEs públicas para as versões encontradas. Um serviço muito comum nos servidores Linux é o SSH. Esse permiti acesso remoto ao servidor, e é bem seguro, porém possui algumas vulneravbilidades, principalmente algumas versões específicas. Portanto uma boa prática seria esconder o banner do serviço.

Nesse tutorial vou ensinar a esconeder o banner do ssh. Mas o mesmo conceito pode ser aplicado a outros serviços ativos no Linux.


## Instale o hexedit

    apt-get update && apt-get install hexedit

## Faça backp do Binário SSH

    cp -p /usr/sbin/sshd /tmp/sshd.bkp
    cp -p /usr/sbin/sshd /tmp/sshd.novo

## Atualize o Binário com o hexedit

    # hexedit /tmp/sshd.novo

Pressione 'TAB' para mudar para a coluna ASCII  

<img src="hexedit1.png">

Use CTRL + S para exibir a pesquisa e procure o texto que você deseja ocultar. Por exemplo "OpenSSH_8.0"

<img src="hexedit2.png">

Você verá algo parecido com isso:



Use as teclas para destacar o inicio da sequência, e digite o texto que deseja substituir.



No caso OpenSSH (por exemplo).

<img src="hexedit3.png">

Tenha cuidado para permanecer dentro dos limites do banner original.

<img src="hexedit4.png">
Depois de alteraado, salve com CTRL +x e Y.


    # rm /usr/sbin/sshd
    cp -p /tmp/sshd.novo /usr/sbin/sshd
    systemctl restart sshd

## Verifique se consegue fazer o SSH.

Para verificar se consegue fazer o ssh e se a informação do banner sumiu, basta executar o comando:

    ssh -vv user@ip

Obs: Pode ser prudente criar uma tarefa na cron para restauração do backup do arquivo, caso algo de errado.

