# heroku-agent-vault

# Procedimento para injetar var localhost

# 1 - Clonar projeto buildpack:
```
git clone git@github.com:Bonny5171/heroku-agent-vault.git

echo export "PATH=$(pwd)/bin:"'$PATH' >> ~/.zshrc
source ~/.zshrc

ou
echo export "PATH=$(pwd)/bin:"'$PATH' >> ~/.bash_rc
source ~/.zshrc
```

# 2 - Configurar variaveis: VAULT_ADDR, VAULT_TOKEN e CAMINHOS_SECRET_STR
```
export VAULT_ADDR=https://vault-cgny-41bd57b8411e.herokuapp.com &&
export VAULT_TOKEN=[TOKEN_VALIDO_AQUI] &&
export VAULT_SECRETS_STR="DataBases/Heroku/data/pessoal-db/postgresql-tetrahedral-44904 \
    Testes/data/testes \
    Testes/data/teste1 \
    Testes/data/teste2 \
    Testes/data/teste3"
```

# 3 - Para injetar variavei do cofre em um .env no local onde foi executado:
```
compile $(pwd) $(pwd)
```



# Procedimento para injetar var na Heroku

# 1 - Adicionar buildpack ao app na Heroku
Importenta a order dos buildplack. Primeiro deve vir o buildpack e depois o builder da aplicação de fato.
```
heroku buildpacks:add https://github.com/heroku/heroku-buildpack-nodejs.git -a nome-da-sua-aplicacao
heroku buildpacks:add heroku/nodejs -a nome-da-sua-aplicacao
```
 
# 2 - Configurar variaveis: VAULT_ADDR, VAULT_TOKEN e CAMINHOS_SECRET_STR
```
heroku config:set VAULT_ADDR=[http://meu-servidor-vault.com.br] -a nome-da-sua-aplicacao
heroku config:set VAULT_TOKEN=[_seu_token_super_secreto_aqui__] -a nome-da-sua-aplicacao
heroku config:set CAMINHOS_SECRET_STR=[secrets_separadas_por_espaço] -a nome-da-sua-aplicacao

```

# 3 - Push com limpeza de Cache de buil da Heroku: "apos qualquer atualização da var: VAULT_SECRETS"
As variaveis serão injetadas antes do build da aplicação.
```
heroku builds:cache:purge --confirm=[APP_NAME] && \
    git add . && \
    git commit --allow-empty -m "Purge cache" && \
    git push heroku master
```




# Push normal para a Heroku: "enviar o codigo"
```
git add . && git commit -am "make it better" && git push heroku master
```
