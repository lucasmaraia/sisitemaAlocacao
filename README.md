# sisitemaAlocacao

Sistema web de locadora de automóveis: cadastro de condutores e veículos, reserva de carros e cálculo da cobrança.

> Projeto de faculdade (abril/2022), feito em PHP orientado a objetos. **O nome do repositório tem um typo** (`sisitema`, com um `i` extra).
>
> Esta é a versão mais completa do trabalho: tem a tela `cobr_210.php` (processamento da inclusão de cobrança) que não existe no repositório [sistemaAlocacao](https://github.com/lucasmaraia/sistemaAlocacao), além de ajustes em vários dos outros arquivos. Os dois repositórios são variações do mesmo projeto.

## Stack

- PHP orientado a objetos (classes com `mysqli`)
- MySQL
- HTML, CSS e JavaScript

## Organização dos arquivos

O projeto usa uma convenção numérica para as telas de cada módulo:

| Sufixo | Função                                  |
|--------|-----------------------------------------|
| `_100` | Listagem dos registros                  |
| `_200` | Formulário de inclusão                  |
| `_210` | Processa a inclusão                      |
| `_300` | Formulário de alteração (já preenchido) |
| `_310` | Processa a alteração                     |
| `_400` | Exclusão                                 |

Os módulos são `condut_` (condutores), `auto_` (automóveis), `reser_` (reservas) e `cobr_` (cobranças).

Cada módulo tem uma classe correspondente com a lógica de acesso ao banco:

```
Condutores.php → ConectaBD, Incluir, Alterar, Excluir
Automoveis.php
Reservas.php
Cobrancas.php
index.html     → menu principal com os quatro módulos
locauto.sql    → script de criação das tabelas
estilos.css
funcoes.js
```

## Banco de dados

O `locauto.sql` cria:

```sql
create table automoveis (
    renavam int primary key,
    marca varchar(20),
    modelo varchar(20),
    placa varchar(7),
    preco float
);

create table condutores (
    cnh int primary key,
    nome varchar(20),
    nasc date
);

create table reservas (
    numero int primary key,
    saida datetime,
    retorno datetime default null,
    preco float,
    cnh int,
    renavam int
);
```

As reservas referenciam condutores (`cnh`) e automóveis (`renavam`), mas sem chave estrangeira declarada.

## Como rodar

1. Suba um ambiente com PHP e MySQL.
2. Importe o `locauto.sql`.
3. Ajuste as credenciais no método `ConectaBD()` de cada uma das quatro classes.
4. Sirva a pasta:

```bash
php -S localhost:8000
```

5. Acesse <http://localhost:8000/index.html>.

## Ressalvas (código de 2022)

- **Credenciais no repositório.** Host, usuário e senha de um banco ClearDB do Heroku estão escritos dentro do método `ConectaBD()` de cada classe. O serviço foi descontinuado e as credenciais estão mortas, mas o padrão certo é usar variáveis de ambiente — e centralizar a conexão em um único lugar, em vez de repetir em quatro classes.
- **SQL injection.** Todas as queries interpolam os atributos do objeto (vindos de `$_REQUEST`) diretamente na string SQL, sem *prepared statements*.
- O `composer.json` está vazio: não há dependências gerenciadas.
- Chaves primárias informadas manualmente (CNH, RENAVAM, número da reserva), sem `AUTO_INCREMENT` onde faria sentido.
