<!-- # Nome do projeto -->

<!-- ### Descrição -->

## Requisitos
É necessário ter ter na sua máquina o [Docker](https://www.docker.com/get-started) e o [Docker Compose](https://docs.docker.com/compose/).

## Instalação
O primeiro passo é clonar este repositório para sua máquina em uma pasta. 

> Importante: Se o seu sistema operacional for Windows, não crie a pasta diretamente na raiz do diretório `C:\` porque o Docker não tem permissão para ler os arquivos dessa pasta. Crie a pasta em seu diretório de `Documentos` ou na `Área de Trabalho`.

Após o repositório ter sido clonado entre na pasta `docker`

Execute o arquivo `build` com os parâmetros:

- Parâmetro `a`: Nome do grupo de containers no Docker [Obrigatório]

- Parâmetro `p`: Porta para acesso da aplicação [Opcional; Padrão: 8000]

- Parâmetro `d`: Porta para acesso ao banco de dados [Opcional; Padrão: 3306]


Exemplo: 
```bash
$ ./build -a app -d 3309 -p 8080
```

<!-- ## Fluxo de commit
Você deverá abrir um [*Merge Request*](https://docs.gitlab.com/ee/gitlab-basics/add-merge-request.html) (similar ao *Pull Request* do GitHub) com as informações das alterações realizadas. -->

## Informações sobre o código em PHP
### Padronização de código
Para manter o código mais organizado, limpo e de fácil leitura é sugerido a utilização de alguns padrões para codificação nos sistemas usando com base o PSR-12 e Slevomat.

Também há algumas definições que podem ser seguidas:

- O código ser escrito em **inglês**, incluindo as variáveis, métodos, funções, classes e comentários.

- Usar os verbos HTTP corretamente 

    - `GET` para buscar informações; 

    - `POST` para cadastrar dados; 

    - `PUT` para atualizar dados; e 

    - `DELETE` para remover 

- As rotas devem ter seus recursos sempre no plural. Ex: `GET https://dominio.com.br/companies/`

- O código ser 100% coberto por testes.

Os comandos que podem ser utilizados estão listados abaixo.

Verificar se a codificação está no padrão requirido.
```bash
$ docker exec -i [nome_do_container] vendor/bin/phpcs
```

Corrigir os erros de codificação automaticamente (quando disponível) rode:
```bash
$ docker exec -i [nome_do_container] vendor/bin/phpcbf
```

Para verificar se há erros no código:
```bash
$ docker exec -i [nome_do_container] vendor/bin/phpstan
```

### Testes
Para rodar os testes da aplicação, utilize o [phpunit](https://phpunit.de/), que já vem instalado:

```bash
$ docker exec -i [nome_do_container] vendor/bin/phpunit
```

### Informações adicionais
##### > Models
Foi incluído a biblioteca [`reliese/laravel`](https://github.com/reliese/laravel) para geração de models.
O comando que deve ser usado para gerar a model é:
```bash
$ docker exec -i [nome_do_container] code:models --table=TABELA
```

> Importante: não rode esse comando sem o parâmetro `--table`, pois irá recriar as models para todas as tabelas e perder qualquer alteração feita nelas.

***
##### > Filtros
Existe um sistema de filtragem para as models usando a biblioteca [`tucker-eric/eloquentfilter`](https://github.com/Tucker-Eric/EloquentFilter). Para utilizar siga os passos abaixo:

Na model inclua a biblioteca
```php
<?php

namespace App\Models;

use EloquentFilter\Filterable; // <--- Incluir
use Illuminate\Database\Eloquent\Model;

class ModelExample extends Model
{
    use Filterable; // <--- Incluir

    ...
}
```

Para criar o filtro use o comando:
```bash
$ docker exec -i [nome_do_container] php artisan model:filter NOME_DA_MODEL
```
Esse comando criará um arquivo na pasta `app/Models/Filters` com o nome da model seguido por `Filter`. Ex: `app/Models/Filters/ModelExampleFilter.php`

Acesse o arquivo e realize as configurações.

Quando desejar usar o filtro chame pelo eloquent usando a função `filter()`. 
Ex:
```php
ModelExample::filter(['name' => 'Test'])->get();
```

***
##### > Checagem de qualidade de código
Caso deseje rodar todas as checagens de qualidade de código que rodam no CI, rode o comando abaixo:

```bash
$ docker exec -i [nome_do_container] vendor/bin/grumphp run
```

Para ativar essas checagens automaticamente antes de cada commit, utilize o `git:init` do _grumphp_:

```bash
$ docker exec -i [nome_do_container] vendor/bin/grumphp git:init
```

Para checar em detalhes a cobertura de testes no código da aplicação, após rodar o _grumphp_,
abra o arquivo `tests/_output/coverage/index.html` em seu navegador.
