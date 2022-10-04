# Template Docker Laravel

### Descrição
Esse projeto tem como objetivo oferecer um template para novos projetos em laravel. Ele foi desenvolvido para criar containers em Docker com o serviço do apache para executar o Laravel e como banco de dados do MySQL. Dentro do Laravel é adicionado algumas bibliotecas de apoio.

## Requisitos
É necessário ter ter na sua máquina o [Docker](https://www.docker.com/get-started) e o [Docker Compose](https://docs.docker.com/compose/).

Caso você esteja utilizando Windows, também será necessário o [Git For Windows](https://gitforwindows.org/).

## Utilização
Primeiros passos:
- Faça fork desse repositório para o seu Github. 
- Crie um projeto usando esse template. 
- Clonar este repositório para sua máquina em uma pasta. 

> Importante: Se o seu sistema operacional for Windows, não crie a pasta diretamente na raiz do diretório `C:\` porque o Docker não tem permissão para ler os arquivos dessa pasta. Crie a pasta em seu diretório de `Documentos` ou na `Área de Trabalho`.

Após o repositório ter sido clonado entre na pasta `docker`

Execute o arquivo `build` com os parâmetros:

- Parâmetro `a`: Nome do grupo de containers no Docker [Obrigatório]

- Parâmetro `p`: Porta para acesso da aplicação [Opcional; Padrão: 8000]

- Parâmetro `d`: Porta para acesso ao banco de dados [Opcional; Padrão: 3306]

> Se você estiver utilizando Windows, use o `Git BASH` para executar o arquivo.

Exemplo: 
```bash
$ ./build -a app -d 3309 -p 8080
```

Os containers gerados serão `[NOME_DO_CONTAINER]-app` para o Laravel e `[NOME_DO_CONTAINER]-db` para o MySQL.

Acesso do MySQL:
- Usuário: `root`
- Senha: `root`

## Bibliotecas utilizadas
Foram incluídos as seguintes bibliotecas no projeto:
- `reliese/laravel`
- `tucker-eric/eloquentfilter`
- `your-app-rocks/eloquent-uuid`
- `phpstan/phpstan`
- `phpro/grumphp`
- `slevomat/coding-standard`
- `squizlabs/php_codesniffer`
- `nunomaduro/larastan`

Caso queira mais detalhes sobre a utilização das bibliotecas olhe o [template-readme.md](config/template-readme.md) na pasta `config/template-readme.md`
