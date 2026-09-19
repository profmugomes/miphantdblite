# MiPhantDBLite

**MiPhantDBLite** é uma biblioteca **leve, minimalista e fluente escrita em PHP** para trabalhar com **SQLite3**, focada em **simplicidade**, **baixo consumo de recursos** e **controle total do SQL gerado**.

Ela é a versão **Lite** do MiPhantDB, pensada para cenários onde **SQLite** é a melhor escolha: aplicações desktop, CLIs, sistemas embarcados, ferramentas locais, cache persistente e pequenos bancos de dados.

Sem ORM pesado, sem abstrações mágicas, apenas **SQL claro, previsível e encadeável**.

---

## ✨ Características

* Abstração leve sobre **SQLite3**
* API fluente (encadeável)
* Suporte a **SELECT, INSERT, UPDATE, DELETE**
* Suporte opcional a **prepared statements**
* Construção dinâmica de `WHERE`, `ORDER BY` e `LIMIT`
* Suporte a `LIKE`, `IN`, `AND`, `OR`
* Criação de tabelas via código
* Modo **sandbox** para debug
* Zero dependências externas
* Compatível com **PHP 8.4 ou superior**

---

## 📦 Instalação

### Via Composer (recomendado)

```bash
composer require mugomes/miphantdblite
```

### Manual

Copie os arquivos da pasta `MiPhantDBLite` para o seu projeto e utilize `require` ou autoload.

---

## 🔌 Conexão com o banco SQLite

```php
use MiPhantDBLite\database;

$db = new database(
    'database.sqlite',
    true // modo sandbox (erros exibidos na tela)
);
```

Também é possível usar opções de abertura:

```php
$db = new database(
    'database.sqlite',
    false,
    database::READWRITE | database::CREATEONLY
);
```

---

## 📖 SELECT

```php
use MiPhantDBLite\select;

$select = new select('database.sqlite');

$select->table('users')
    ->column('id')
    ->column('name')
    ->where('status', 'ativo')
    ->order('name')
    ->limit(0, 10)
    ->select();

while ($row = $select->fetch()) {
    echo $row['name'];
}

$select->close();
```

---

## 🔐 Prepared Statements

```php
$select->activatePrepared()
    ->table('users')
    ->where('id', 1)
    ->select();

$row = $select->fetch();
$select->close();
```

* Evita SQL Injection
* Ideal para dados dinâmicos
* Ativado explicitamente via `activatePrepared()`

---

## ➕ INSERT

```php
use MiPhantDBLite\insert;

$insert = new insert('database.sqlite');

$insert->activatePrepared()
    ->table('users')
    ->insertValue('name', 'João')
    ->insertValue('email', 'joao@email.com')
    ->insert();

$id = $insert->idInsert();
$insert->close();
```

---

## ✏️ UPDATE

```php
use MiPhantDBLite\update;

$update = new update('database.sqlite');

$update->activatePrepared()
    ->table('users')
    ->updateValue('email', 'novo@email.com')
    ->where('id', 1)
    ->update();

$update->close();
```

---

## ❌ DELETE

```php
use MiPhantDBLite\delete;

$delete = new delete('database.sqlite');

$delete->table('users')
    ->where('id', 1)
    ->delete();

$delete->close();
```

---

## 🧱 Criação de tabelas

```php
use MiPhantDBLite\table;

$table = new table('database.sqlite');

$table->table('users')
    ->setInt()->primaryKey()->autoIncrement()->insertColumn('id')
    ->insertColumn('name')
    ->insertColumn('email')
    ->create();

$table->close();
```

---

## 🔒 Encerrando a conexão (`close`)

```php
$db->close();
```

### O que o `close()` faz?

* Libera resultados ativos
* Finaliza prepared statements (quando usados)
* Fecha corretamente a conexão SQLite
* Evita vazamento de recursos em scripts longos ou CLIs

> 💡 **Boa prática:**
> Embora o PHP finalize a conexão ao fim do script, o uso explícito de `close()` é altamente recomendado em aplicações CLI, loops e processos persistentes.

---

## 🧠 Outras Informações

* SQLite continua sendo SQLite
* Sem ORM pesado
* Sem reflexão ou proxies mágicos
* SQL previsível e fácil de depurar
* Ideal para projetos pequenos, locais ou embarcados

---

## 👤 Autor

**Murilo Gomes Julio**

🔗 [https://www.bluice.com.br](https://www.bluice.com.br)

📺 [https://youtube.com/@mugomesoficial](https://youtube.com/@mugomesoficial)

---

## 🤝 Support

* GitHub Sponsors: [https://github.com/sponsors/mugomes](https://github.com/sponsors/mugomes)
* Apoie o projeto: [https://www.bluice.com.br/apoie.html](https://www.bluice.com.br/apoie.html)

---

## 📜 License

The MiPhantDBLite is provided under:

[SPDX-License-Identifier: LGPL-2.1-only](https://github.com/mugomes/miphantdblite/blob/main/LICENSE)

Beign under the terms of the GNU Lesser General Public License version 2.1 only.

All contributions to the MiPhantDBLite are subject to this license.
