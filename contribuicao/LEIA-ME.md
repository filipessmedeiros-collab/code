# Contribuição ao projeto original

O patch 01 altera somente os três arquivos de implementação/testes em craft-ui. A descrição pronta para o primeiro Pull Request está em CRAFT-LIBS-PR.md. O patch pode ser aplicado também ao main verificado ae2f796b673f29cf3161f36eb6a8933f0a3fe028: os três arquivos não mudaram em relação à revisão usada pelo PhotoCraft.

## Próxima ação necessária

Criar um fork de https://github.com/storytold/craft-libs/fork na conta filipessmedeiros-collab, mantendo o nome craft-libs. A API deste ambiente retornou Forbidden ao tentar criá-lo; a publicação via Git no repositório code continua funcionando.

Com o fork disponível, será possível publicar a branch da correção e gerar o link de comparação para abrir o Pull Request. Ainda não existe Pull Request no projeto original.

A segunda contribuição ao PhotoCraft dependerá do commit aprovado da biblioteca: atualizar a revisão real e o Cargo.lock e incluir o teste de regressão. Não usar revisões fictícias.
