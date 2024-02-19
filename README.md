# heroku-agent-vault

# Configurar env antes de injetar:
```
export VAULT_ADDR=https://vault-cgny-41bd57b8411e.herokuapp.com &&
export VAULT_TOKEN=[TOKEN_VALIDO_AQUI] &&
export CAMINHOS_SECRET_STR="DataBases/Heroku/data/pessoal-db/postgresql-tetrahedral-44904 \
    Testes/data/testes \
    Testes/data/teste1 \
    Testes/data/teste2 \
    Testes/data/teste3"
```

# Para injetar variavei do cofre:
```
compile $(pwd) $(pwd) $(pwd)
```

# Push força deched
```
heroku builds:cache:purge --confirm=web-client-vault && \
    git add . && \
    git commit --allow-empty -m "Purge cache" && \
    git push heroku master
```
