<a href="https://cogny.co/">
<img alt="Cron To Go logo" src="https://scontent.fcgh57-1.fna.fbcdn.net/v/t39.30808-1/358675908_609545864597557_6874745183075393893_n.jpg?stp=dst-jpg_p200x200&_nc_cat=104&ccb=1-7&_nc_sid=596444&_nc_ohc=QLLVgZoiNB8AX8Kk_BN&_nc_ht=scontent.fcgh57-1.fna&oh=00_AfCqCsoq1_wPV3UQ2FHZ917hCwfTfpK-kO2pqi0AePGqlA&oe=65DBA7F1" height="80" />
</a>

_BuildPack: Injeta secrets do [HashCorp Vault](https://developer.hashicorp.com/vault) no arquivo .env dentro do dyno.

*BuildPack permite você injetar secrets do seu cofre Vault em seu app Heroku.*

Heroku buildpack
==================================================

Este é um [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) 
que permite injetar secrets dentro de um app em um dyno para aplicações nodejs.


Usage
-----

Example usage:

    $ heroku config:set VAULT_ADDR=http://meu-servidor-vault.com.br -a app-name
    $ heroku config:set VAULT_TOKEN=[token] -a app-name
    $ heroku config:set VAULT_SECRETS=[secrets_separadas_por_espaço] -a app-name

    $ heroku buildpacks:add https://github.com/Bonny5171/heroku-agent-vault.git -a app-name

    Importante registar as 3/3 chaves requeridas.
    
O `heroku-buildpack-vault` necessita de um build com limpeza de cache para injetar as secrets.

    $ heroku builds:cache:purge --confirm=[app-name] && \
        git add . && \
        git commit --allow-empty -m "Purge cache" && \
        git push heroku master




Usage local
-----

Example usage:

    $ export VAULT_ADDR=https://vault-cgny-41bd57b8411e.herokuapp.com
    $ export VAULT_TOKEN=[token]
    $ export VAULT_SECRETS_STR=[Testes/data/testes,Testes/data/teste1,Testes/data/teste2,Testes/data/teste3,DataBases/Heroku/data/pessoal-db/postgresql-tetrahedral-44904]

    export VAULT_SECRETS_STR=[Testes/data/testes]

    Confere!
    echo $VAULT_ADDR && echo $VAULT_TOKEN && echo $VAULT_SECRETS_STR 

    Local ondem vai ser criado/atualizado o ".env"
    $ echo pwd
    $ bin/compile $(pwd) $(pwd)
   


Notes
-----

VAULT_ADDR: é o endereço do seu servidor Vault.
VAULT_TOKEN: usado para obter as secrets, este token deve conter as policies necessarias para as secrets solicitas.
CAMINHOS_SECRET_STR: paths secrets separados por espaço.


```
    Conferencia manual do .env dentro do dyno
    $ heroku run bash -a app-name
    cat .env

    exit
```