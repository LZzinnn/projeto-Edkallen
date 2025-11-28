🌳 ED2 - Árvore B (B-Tree) - Trabalho Acadêmico

Este repositório contém o código-fonte em Python utilizado para a produção do vídeo educativo sobre a estrutura de dados Árvore B (B-Tree).

O objetivo é demonstrar a lógica por trás da eficiência das Árvores B na indexação de grandes volumes de dados, como em sistemas de arquivos e bancos de dados.

📺 Conteúdo do Vídeo

Nosso vídeo didático aborda:

Conceito e Analogia: Entendimento da Árvore B como uma estrutura "baixa e larga" que minimiza acessos a disco (I/O).

Regra da Ordem (t): Como o grau mínimo define o tamanho e o equilíbrio dos nós.

Algoritmo de Inserção: Foco na operação de SPLIT (Divisão), que é o mecanismo chave para manter o balanceamento da árvore quando um nó atinge sua capacidade máxima.

Implementação em Python: Análise das funções inserir_chave e _dividir_filho no código-fonte.

💾 Código-Fonte

O código está contido no arquivo arvore_b_simples.py. A implementação define a Árvore B com o Grau Mínimo (Ordem) t=3, permitindo um máximo de 5 chaves por nó (2*t - 1).

As classes principais são:

NoArvoreB: Define a estrutura de cada nó (chaves, filhos, status folha).

ArvoreB: Gerencia a raiz e as operações da árvore.

🚀 Como Executar a Demonstração

O script arvore_b_simples.py é autoexecutável e possui uma demonstração embutida que testa a inserção e força a operação de Split na raiz.

Pré-requisitos

Certifique-se de que você tem o Python 3.x instalado em seu sistema operacional.

Instruções

Baixe o arquivo arvore_b_simples.py para um diretório local.

Abra o seu terminal (ou Prompt de Comando) e navegue até esse diretório.

Execute o script Python usando o comando:

python arvore_b_simples.py


Saída do Programa

A saída do console demonstrará a inserção sequencial das chaves [10, 20, 30, 40, 50, 60]. O ponto mais importante é quando a chave 60 é inserida, forçando o nó raiz cheio ([10, 20, 30, 40, 50]) a se dividir, promovendo o 30 para a nova raiz e criando dois nós filhos.
