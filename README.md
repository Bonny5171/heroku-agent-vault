<a href="https://cogny.co/">
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="width: 100%; height: 100%" viewBox="0 0 200 50" preserveAspectRatio="none" width="100%" height="100%"><use href="#svg1088834902_7166"></use></svg>
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

    $ heroku buildpacks:add https://github.com/Bonny5171/heroku-agent-vault.git -a app-name

    $ heroku config:set VAULT_ADDR=http://meu-servidor-vault.com.br -a app-name
    Setting VAULT_ADDR and restarting ⬢ novo-app... done, v19
    VAULT_ADDR: http://meu-servidor-vault.com.br

    $ heroku config:set VAULT_TOKEN=[token] -a app-name
    Setting VAULT_TOKEN and restarting ⬢ novo-app... done, v19
    VAULT_TOKEN: [token]

    $ heroku config:set CAMINHOS_SECRET_STR=[secrets_separadas_por_espaço] -a app-name
    Setting CAMINHOS_SECRET_STR and restarting ⬢ novo-app... done, v19
    CAMINHOS_SECRET_STR: teste1

    Importante registar as 3/3 chaves requiridas.
    
O `heroku-buildpack-vault` necessita de um build com limpeza de cache para injetar as secrets.

    $ heroku builds:cache:purge --confirm=[app-name] && \
        git add . && \
        git commit --allow-empty -m "Purge cache" && \
        git push heroku master

Notes
-----

VAULT_ADDR: é o endereço do seu servidor Vault.
VAULT_TOKEN: usado para obter as secrets, este token deve conter as policies necessarias para as secrets solicitas.
CAMINHOS_SECRET_STR: paths secrets separados por espaço.


```
    $ heroku run bash -a app-name
    cat .env

    exit
```




Usage local
-----

Example usage:

    $ export VAULT_ADDR=https://vault-cgny-41bd57b8411e.herokuapp.com
    $ export VAULT_TOKEN=[token]
    $ export VAULT_SECRETS_STR="DataBases/Heroku/data/pessoal-db/postgresql-tetrahedral-44904 \
            Testes/data/testes \
            Testes/data/teste1 \
            Testes/data/teste2 \
            Testes/data/teste3"

    Primeiro e segundo parametro "$(pwd)" é o path onde o builder vai injetar o variaveis em um arquivo >>> ".env"
    $ bin/compile $(pwd) $(pwd)
   