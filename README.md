# wp-migration
Boas práticas para migrações WordPress

### Guia para Migrações de Sites WordPress.org de um Servidor para Outro

Migrar um site WordPress de um servidor para outro pode parecer um desafio, mas com as ferramentas e práticas corretas, o processo pode ser simplificado. Abaixo está um guia passo a passo, incluindo boas práticas para garantir uma migração suave.

### Guia para Migrações de Sites WordPress.org de um Servidor para Outro

#### Secção Preliminar: Elementos a Migrar

Existem três componentes principais que precisa copiar ao migrar um site WordPress:

1. **Ficheiros do Sistema (excluindo wp-content/uploads):**
   - Estes incluem os ficheiros do core do WordPress, temas e plugins.
   - A pasta `wp-content/uploads` é frequentemente de grande porte e pode demorar mais tempo a copiar. Por isso, muitas vezes é copiada separadamente no final.

2. **Base de Dados:**
   - Contém todo o conteúdo dinâmico do site, como posts, páginas, configurações e dados de plugins.

3. **Uploads:**
   - Esta pasta inclui todas as imagens, vídeos e outros ficheiros que foram carregados para o site. A transferência destes ficheiros pode ser demorada, por isso, normalmente, é deixada para o fim e pode ser feita em paralelo após a migração principal.

#### Como Criar um Arquivo do Filesystem Excluindo a Pasta dos Uploads

Para criar um arquivo dos ficheiros do sistema excluindo a pasta `wp-content/uploads`, podes usar o comando `tar` no terminal. Aqui está um exemplo de como o fazer:

```sh
tar --exclude='wp-content/uploads' -czvf site-files.tar.gz /caminho/para/site
```

Este comando cria um arquivo comprimido (`site-files.tar.gz`) de todos os ficheiros do seu site WordPress, excluindo a pasta `wp-content/uploads`.

#### 1. Utilizar o Plugin "All-in-One WP Migration"

**Procedimento Básico:**
1. **No servidor de origem:**
   - Instale o plugin [All-in-One WP Migration](https://wordpress.org/plugins/all-in-one-wp-migration/).
   - Vá ao menu do plugin e faça um backup completo do site.
   - Exporte o backup gerado.

2. **No servidor de destino:**
   - Instale o plugin [All-in-One WP Migration](https://wordpress.org/plugins/all-in-one-wp-migration/).
   - Importe o backup (extensão .wpress) previamente exportado.

**Nota:** Este método pode não ser viável para sites grandes devido a limitações de tamanho de ficheiros e restrições de memória nos servidores. 

#### 2. Migração Manual (Para Sites de Grande Porte)

**Passos para migração manual:**

1. **Copiar Ficheiros:**
   - Acesse os ficheiros do site no servidor de origem via FTP ou SSH.
   - Transfira todos os ficheiros do WordPress para o servidor de destino, mantendo a mesma estrutura de diretórios.

2. **Exportar Base de Dados:**
   - Acesse o phpMyAdmin ou utilize a linha de comando para exportar a base de dados do WordPress no servidor de origem.
   - No phpMyAdmin, selecione a base de dados e clique em "Exportar".

3. **Importar Base de Dados no Servidor de Destino:**
   - Crie uma nova base de dados no servidor de destino.
   - Utilize o phpMyAdmin ou [WP-CLI](https://wp-cli.org/) para importar a base de dados exportada.
     ```sh
     wp db import nome_do_ficheiro.sql
     ```

4. **Atualizar URLs na Base de Dados:**
   - Após a importação, substitua o URL antigo pelo novo utilizando o comando `search-replace` do WP-CLI:
     ```sh
     wp search-replace 'http://url-antigo.com' 'http://url-novo.com' --skip-columns=guid
     ```

#### 3. Aumentar o Tamanho Máximo de Upload (Opcional)

Se estiver a usar o Local by Flywheel e precisar aumentar o limite de tamanho de upload de ficheiros, siga este [vídeo tutorial](https://sharing.clickup.com/clip/p/t24463362/a48d624b-e70f-453b-8d45-74321ff35883/screen-recording-2024-05-24-16:47.webm).

### Boas Práticas para Migrações WordPress

- **Backup Completo:** Antes de iniciar qualquer migração, faça um backup completo do site, incluindo ficheiros e base de dados.
- **Verificação de Compatibilidade:** Assegure-se de que o servidor de destino tem os requisitos necessários para correr WordPress (PHP, MySQL, extensões, etc.).
- **Segurança:** Utilize conexões seguras (SFTP/SSH) para transferir ficheiros e proteja as credenciais da base de dados.
- **Testes Pós-Migração:** Após a migração, teste exaustivamente o site no novo servidor para garantir que tudo está a funcionar corretamente. Verifique links internos, imagens, plugins e temas. Faça o flush dos permalinks.

### Conclusão

A migração de um site WordPress pode ser uma tarefa complexa, mas com as ferramentas e procedimentos adequados, pode ser realizada de forma eficiente. Utilize plugins confiáveis para migrações simples e, para sites maiores, siga as práticas de migração manual, assegurando-se sempre de realizar backups e testes detalhados no novo ambiente.