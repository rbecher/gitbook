## Git Hooks ## 

Hooks são pequenos scripts que você pode colocar no diretório GIT_DIR/hooks
para disparar um ação em certos pontos. Quando git-init é executado, uns
exemplos úteis de hooks são copiados no diretório hooks do novo repositório,
mas por padrão eles são todos desativados. Para ativar um hook, renomeie ele
removendo o seu sufixo .sample.


### applypatch-msg ### 

    GIT_DIR/hooks/applypatch-msg 

Esse hook é invocado pelo script 'git-am'. Ele leva um simples parâmetro, o nome
do arquivo que detém a mensagem de commit proposta. Saindo com um status diferente
de zero faz com que 'git-am' aborte antes de aplicar o patch.

Nesse hook é permitido editar o arquivo de mensagem substituindo-o, e pode ser usado
para normalizar a mensagem dentro em algum formato padrão de mensagens (se o projeto
tem um). Ele pode também ser usado para recusar o commit depois de inspecionar o
arquivo de mensagens.
O hook 'applypatch-msg' padrão, quando ativado, executa o hook 'commit-msg', se este também
estiver ativado.


### pre-applypatch ###

    GIT_DIR/hooks/pre-applypatch

Esse hook é invocado pelo 'git-am'. Ele não leva nenhum parâmetro, e é invocado depois
que o patch é aplicado, mas antes que um commit seja feito.
Se ele sai com um status diferente de zero, então não será realizado o commit depois de
aplicar esse patch.

Ele pode ser usado para inspecionar a árvore de trabalho atual e recusar realizar um
commit se ele não passar em certos testes.
O hook 'pre-applypatch' padrão, quando ativado, executa o hook 'pre-commit', se este também
estiver ativado.


### post-applypatch ###

    GIT_DIR/hooks/post-applypatch

Esse hook é invocado pelo 'git-am'. Ele não leva nenhum parâmetro, e é invocado
depois que o patch é aplicado e um commit é feito.

Esse hook é usado essencialmente para notificações, e não pode afetar o resultado
do 'git-am'.


### pre-commit ###

    GIT_DIR/hooks/pre-commit

Esse hook é invocado pelo 'git-commit', e pode ser ignorado com a opção `\--no-verify`.
Ele não leva nenhum parâmetro, e é invocado antes de obter a mensagem do commit proposta
e realizar o commit. Saindo com status diferente de zero desse script faz com que 'git-commit'
seja cancelado.

O hook 'pre-commit' padrão, quando ativado, captura o início das linhas com espaços vazios
e cancela o commit quando alguma linha é encontrada.

Todos os hooks 'git-commit' são invocados com a variável de ambiente `GIT_EDITOR=:` se o commando
não carregar um editor para modificar o mensagem de commit.

Aqui é um exemplo de um script Ruby que executa testes RSpec antes de permitir um commit.

	ruby
	html_path = "spec_results.html"
	`spec -f h:#{html_path} -f p spec` # run the spec. send progress to screen. save html results to html_path

	# find out how many errors were found
	html = open(html_path).read
	examples = html.match(/(\d+) examples/)[0].to_i rescue 0
	failures = html.match(/(\d+) failures/)[0].to_i rescue 0
	pending = html.match(/(\d+) pending/)[0].to_i rescue 0

	if failures.zero?
	  puts "0 failures! #{examples} run, #{pending} pending"
	else
	  puts "\aDID NOT COMMIT YOUR FILES!"
	  puts "View spec results at #{File.expand_path(html_path)}"
	  puts
	  puts "#{failures} failures! #{examples} run, #{pending} pending"
	  exit 1
	end


### prepare-commit-msg ###

    GIT_DIR/hooks/prepare-commit-msg

Esse hook é invocado pelo 'git-commit' depois de preparar a mensagem
de log padrão, e antes de iniciar o editor.

Ele leva de um a três parâmetros. O primeiro é o nome do arquivo da
mensagem de commit. O segundo é a origem da mensagem de commit, e pode ser:
`message` (se a opção `-m` ou `-F` foi dada);
`template` (se a opção `-t` foi dada ou a opção de configuração `commit.template`
está configurada);
`merge` (se o commit é um merge ou o arquivo `.git/MERGE_MSG` existe);
`squash` (se o arquivo `.git/SQUASH_MSG` existe);
`commit`, seguido por um id SHA1 (se a opção `-c`, `-C` ou `\--amend` foi dada ).

Se o status de saída é diferente de zero, 'git-commit' será cancelado.

A proposta desse hook é editar o arquivo de mensagem, e se ele não for anulado
pela opção `\--no-verify`. Uma saída diferente de zero significa uma falha nesse
hook e cancela o commit. Ele não deveria ser usado como substituto do hook
'pre-commit'.

O exemplo do hook `prepare-commit-msg` que vem com o git comenta a parte do
`Conflicts:` de uma mensagem de commit durante um merge.


### commit-msg ###

    GIT_DIR/hooks/commit-msg

Esse hook é invocado pelo 'git-commit', e pode ser anulado com a opção
`\--no-verify`. Ele leva um simples parâmetro, o nome do arquivo que detém
a mensagem de commit proposta. Saindo com status diferente de zero faz com que
o 'git-commit' aborte.

Com esse hook é permitido editar o arquivo de mensagem, e pode ser usado para
normalizar a mensagem em algum formato padrão de mensagens (se o projeto tem um).
Ele pode se usado para recusar o commit depois de inspecionar o arquivo de mensagem.

O hook 'commit-msg' padrão, quando ativado, detecta duplicadas linhas "Signed-off-by", e
cancela o commit se um for encontrada.


### post-commit ###

    GIT_DIR/hooks/post-commit

Esse hook é invocado pelo 'git-commit'. Ele não leva nenhum parâmetro, e é invocado
depois que o commit é feito.

Esse hook é usado essencialmente para notificações, e não pode afetar o resultado do
'git-commit'.


### pre-rebase ###

    GIT_DIR/hooks/pre-rebase

Esse hook é chamado pelo 'git-rebase' e pode ser usado para previnir que num branch
seja realizado um rebase.


### post-checkout ###

    GIT_DIR/hooks/post-checkout

Esse hook é invocado quando um 'git-checkout' é executado depois de ter atualizado
a árvore de trabalho. Para esse hook é dado 3 parâmetros: a referência de um HEAD
anterior, a referência para um novo HEAD (que pode ou não ser mudado), e uma flag
indicando se o checkout foi um branch checkout (alterando branchs, flag=1) ou o
checkout de um arquivo (recuperando um arquivo do index, flag=0).
Esse hook não afeta o resultado do 'git-checkout'.

Esse hook pode ser usado para realizar checagens de validação no repositório, mostrar
as diferenças de um HEAD anterior caso seja diferente, ou configurar as propriedades
dos metadados do diretório.


### post-merge ###

    GIT_DIR/hooks/post-merge

Esse hook é invocado pelo 'git-merge', que acontece quando um 'git-pull' é feito
sobre um repositório local. O hook leva um simples parâmetro, um flag de status
especificando se é um merge que está sendo feito ou não era um merge do tipo squash.
Esse hook não afeta o resultado do 'git-merge' e não é executado, se o merge falha
devido aos conflitos.

Esse hook pode ser usado em conjunto com o hook 'pre-commit' correspondente para
salvar e restaurar qualquer forma de metadados associdados com a árvore de trabalho
(ex.: permissões/donos, ACLS, etc).


### pre-receive ###

    GIT_DIR/hooks/pre-receive

Esse hook é invocado pelo 'git-receive-pack' sobre o repositório remoto,
que acontece quando um 'git-push' é feito sobre o repositório local.
Só antes de iniciar uma atualização das referências sobre o repositório
remoto, o hook 'pre-receive' é invocado. Seu status de saída determina o
sucesso ou falha da atualização.

Esse hook executa uma vez para receber a operação. Ele não leva nenhum argumento,
mas para cada referência ser atualizada ele recebe sobre a entrada padrão uma
linha no formato:

  <old-value> SP <new-value> SP <ref-name> LF

onde `<old-value>` é o nome do objeto antigo armazenado na referência, `<new-value>`
é o nome do novo objeto para ser armazenado na referência e `<ref-name>` é o nome
completo da referência.
Quando criar uma nova referência, `<old-value>` é 40 `0`.

Se o hook sai com status diferente de zero, nenhum resultado será atualizado. Se
o hook sai com zero, atualizações de referências individuais podem ainda ser
previnidos pelo hook 'update'.

Ambas as saídas e erros padrões são encaminhadas para 'git-send-pack'
na outra extremidade, então você pode simplesmente fazer um `echo` nas mensagens
para o usuário.

Se você escreveu ele em Ruby, você pode conseguir os argumentos dessa forma:

	ruby
	rev_old, rev_new, ref = STDIN.read.split(" ")

Ou em um script bash, alguma coisa assim funcionaria:

	#!/bin/sh
	# <oldrev> <newrev> <refname>
	# update a blame tree
	while read oldrev newrev ref
	do
		echo "STARTING [$oldrev $newrev $ref]"
		for path in `git diff-tree -r $oldrev..$newrev | awk '{print $6}'`
		do
		  echo "git update-ref refs/blametree/$ref/$path $newrev"
		  `git update-ref refs/blametree/$ref/$path $newrev`
		done
	done


### update ###

    GIT_DIR/hooks/update

Esse hook é invocado pelo 'git-receive-pack' sobre o repositório remoto,
que acontece quando um 'git-push' é feito sobre um repositório local.
Só que antes de atualizar a referência sobre o repositório remoto, o hook
'update' é invocado. Seu status de saída determina o sucesso ou falha da
atualização da referência.

O hook executa uma vez para cada referência para ser atualizada, e leva
3 parâmetros:

 - o nome da referência sendo atualizada,
 - o nome do antigo objeto armazenado na referência,
 - e o novo nome do objeto que será armazenado na referência.

Um saída zero do hook 'update' permite que a referência seja atualizada.
Saindo com status diferente de zero previne 'git-receive-pack' de atualizar
aquela referência.

Esse hook pode ser usado para previnir uma atualização 'forçada' sobre certas
referências pela certeza de que o nome do objeto é um objeto commit que é um
descedente do objeto commit nomeado pelo antigo nome de objeto.
Que é, fazer valer somente uma política de "fast forward".

Ele também poderia ser usado para ver o status dos logs entre antigo..novo. Contudo, ele
faz com que não saiba o completo conjunto de branchs, embora ele terminaria
enviando um email por referência quando usado ingenuamente. O hook
<<post-receive,'post-receive'>> é mais adequado.

Outro uso sugerido na lista de discussão é usar esse hook para implementar
controle de acesso que é mais refinado do que um baseado em grupos no sistema
de arquivos.

Ambas as saídas e erros padrões são encaminhadas para 'git-send-pack'
na outra extremidade, então você pode simplesmente fazer um `echo` nas mensagens
para o usuário.

O hook 'update' padrão, quando ativado--e com a opção `hooks.allowunannotated`
ligada--previne tags não definidas sejam enviadas.


### post-receive ###

    GIT_DIR/hooks/post-receive

Esse hook é invocado pelo 'git-receive-pack' sobre o repositório remoto, que
acontece quando um 'git-push' é feito sobre um repositório local.
Ele executa sobre o repositório remoto uma vez depois de todas as referências
tem sido atualizadas.

Esse hook executa uma vez para receber a operação. Ele não leva nenhum argumento
, mas consegue a mesma informação quando o hook <<pre-receive,'pre-receive'>>
faz sobre a sua entrada padrão.

Esse hook não afeta o resultado do 'git-receive-pack', quando ele é chamado depois
que o trabalho real é feito.

Ele substitui o hook <<post-update,'post-update'>> no qual ele consegue ambos antigos
e novos valores de todas as referências além de seus nomes.

Ambas as saídas e erros padrões são encaminhadas para 'git-send-pack'
na outra extremidade, então você pode simplesmente fazer um `echo` nas mensagens
para o usuário.

O hook 'post-receive' padrão é vazio, mas existe um script de exemplo 'post-receive-email'
fornecido no diretório `contrib/hooks` na distribuição do Git, que implementa o envio de
emails.


### post-update ###

    GIT_DIR/hooks/post-update

Esse hook é invocado pelo 'git-receive-pack' sobre o repositório remoto,
que acontece quando um 'git-push' é feito sobre o repositório local.
Ele executa sobre o repositório remoto uma vez depois que todas as referências
tem sido atualizadas.

Ele leva um número variável de parâmetros, cada qual é o nome da referência que
na verdade foi atualizada.

Esse hook é usado essencialmente para notificações, e não pode afetar o resultado
do 'git-receive-pack'.

O hook 'post-update' pode dizer quais são os heads que foram enviados,
mas ele não sabe quais deles são valores original ou atualizados, por isso é um
mau lugar para fazer ver os logs entre o antigo..novo. O hook <<post-receive,'post-receive'>>
consegue ambos originais e atualizados valores das referências. Pode ser que você
considere ele, por exemplo, se precisar dele.

Quando ativado, o hook 'post-update' padrão executa 'git-update-server-info' para
manter a informação usada no transporte mudo (ex.: HTTP) atualizado. Se você está
publicando um repositório git que é acessível via HTTP, você deveria provavelmente
ativar esse hook.

Ambas as saídas e erros padrões são encaminhadas para 'git-send-pack'
na outra extremidade, então você pode simplesmente fazer um `echo` nas mensagens
para o usuário.


### pre-auto-gc ###

    GIT_DIR/hooks/pre-auto-gc

Esse hook é invocado pelo 'git-gc --auto'. Ele não leva nenhum parâmetro, e a saída
com o status diferente de zero desse script faz com que o 'git-gc --auto' seja
cancelado.


### Referências ###

[Git Hooks](http://www.kernel.org/pub/software/scm/git/docs/githooks.html)  
[Git hooks make me giddy](http://probablycorey.wordpress.com/2008/03/07/git-hooks-make-me-giddy/)
