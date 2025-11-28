# projeto-Edkallen
projeto Arvore B
# -*- coding: utf-8 -*-
#
# IMPLEMENTAÇÃO SIMPLIFICADA DE ÁRVORE B (B-TREE)
# Foco na lógica de Inserção, Busca e a operação essencial de SPLIT (Divisão de Nó).
# A Ordem (t) da árvore é definida na inicialização.
#
# Requisito: Python 3
#

class NoArvoreB:
    """
    Representa um Nó na Árvore B.
    Cada nó pode armazenar até 2*t - 1 chaves.
    """
    def __init__(self, t, eh_folha=False):
        self.t = t                          # Grau mínimo (Ordem)
        self.chaves = []                    # Lista de chaves armazenadas no nó
        self.filhos = []                    # Lista de ponteiros para os filhos
        self.eh_folha = eh_folha            # Booleano: True se for um nó folha

    def __str__(self):
        """Representação amigável das chaves do nó."""
        return f"Chaves: {self.chaves}"

class ArvoreB:
    """
    Estrutura principal da Árvore B.
    """
    def __init__(self, t):
        self.t = t                          # Grau mínimo (Ordem)
        self.raiz = NoArvoreB(t, eh_folha=True)

    # --- FUNÇÃO DE BUSCA (SIMPLES) ---

    def buscar_chave(self, chave, no=None):
        """
        Busca uma chave na árvore. Se encontrada, retorna o Nó e o índice.
        """
        if no is None:
            no = self.raiz

        i = 0
        # Percorre as chaves no nó para encontrar a posição ou o ponteiro
        while i < len(no.chaves) and chave > no.chaves[i]:
            i += 1

        # Chave ENCONTRADA no nó atual
        if i < len(no.chaves) and chave == no.chaves[i]:
            return no, i

        # Se for uma folha e não encontramos, a chave não existe
        if no.eh_folha:
            return None

        # Desce recursivamente para o nó filho apropriado
        return self.buscar_chave(chave, no.filhos[i])

    # --- FUNÇÃO DE INSERÇÃO ---

    def inserir_chave(self, chave):
        """
        Inicia a inserção da chave. Trata o caso especial de a RAIZ estar cheia.
        """
        r = self.raiz
        
        # O nó está cheio se tiver 2*t - 1 chaves
        if len(r.chaves) == (2 * self.t) - 1:
            # 1. Cria uma NOVA RAIZ (crescimento da árvore em altura)
            nova_raiz = NoArvoreB(self.t, eh_folha=False)
            self.raiz = nova_raiz
            nova_raiz.filhos.append(r)
            
            # 2. Divide a raiz antiga (r)
            self._dividir_filho(nova_raiz, 0, r)
            
            # 3. Insere a chave na nova raiz não cheia
            self._inserir_nao_cheio(nova_raiz, chave)
        else:
            # Insere a chave na raiz existente (não cheia)
            self._inserir_nao_cheio(r, chave)

    def _inserir_nao_cheio(self, no, chave):
        """
        Insere a chave em um nó que NÃO está cheio. Desce até a folha.
        """
        i = len(no.chaves) - 1
        
        # 1. Se o nó é FOLHA: insere na ordem correta
        if no.eh_folha:
            no.chaves.append(None) 
            while i >= 0 and chave < no.chaves[i]:
                no.chaves[i+1] = no.chaves[i]
                i -= 1
            no.chaves[i+1] = chave
            
        # 2. Se NÃO é folha: encontra o filho para descer
        else:
            # Acha o índice do ponteiro correto
            while i >= 0 and chave < no.chaves[i]:
                i -= 1
            i += 1
            
            # Verifica se o FILHO está cheio ANTES de descer
            filho_a_descer = no.filhos[i]
            if len(filho_a_descer.chaves) == (2 * self.t) - 1:
                # SE O FILHO ESTIVER CHEIO: faz o SPLIT
                self._dividir_filho(no, i, filho_a_descer)
                
                # Após o split, o nó PAI recebeu a chave mediana.
                # Se a chave a inserir for MAIOR que a mediana promovida, desce pelo novo filho.
                if chave > no.chaves[i]:
                    i += 1
            
            # Chamada recursiva para descer e inserir no filho
            self._inserir_nao_cheio(no.filhos[i], chave)

    def _dividir_filho(self, no_pai, indice_filho, filho_cheio):
        """
        OPERAÇÃO CHAVE: Divide o 'filho_cheio' em dois e move a chave mediana para o 'no_pai'.
        
        """
        # Cria o novo nó, que será o irmão à direita
        novo_irmao = NoArvoreB(self.t, eh_folha=filho_cheio.eh_folha)
        
        # 1. Identifica a chave MEDIANA que irá subir para o nó pai
        chave_promovida = filho_cheio.chaves[self.t - 1]
        
        # 2. Move as chaves da metade DIREITA do filho_cheio para o novo_irmao
        novo_irmao.chaves = filho_cheio.chaves[self.t:]
        
        # 3. Se não for folha, move os ponteiros de filhos correspondentes
        if not filho_cheio.eh_folha:
            novo_irmao.filhos = filho_cheio.filhos[self.t:]

        # 4. Ajusta o filho_cheio, mantendo apenas as chaves da metade ESQUERDA
        filho_cheio.chaves = filho_cheio.chaves[:self.t - 1]
        
        # 5. Insere o novo_irmao no array de filhos do nó pai (na posição i+1)
        no_pai.filhos.insert(indice_filho + 1, novo_irmao)
        
        # 6. Insere a chave promovida no array de chaves do nó pai (na posição i)
        no_pai.chaves.insert(indice_filho, chave_promovida)


    # --- FUNÇÃO PARA DEMONSTRAÇÃO (IMPRESSÃO) ---

    def imprimir_arvore(self, no=None, nivel=0):
        """
        Imprime a estrutura da árvore em formato hierárquico.
        """
        if no is None:
            no = self.raiz

        # Imprime o nó atual (Chaves)
        print("  " * nivel + f"| Nível {nivel}: {no}")
        
        # Se não for folha, chama recursivamente para os filhos
        if not no.eh_folha:
            for filho in no.filhos:
                self.imprimir_arvore(filho, nivel + 1)

# --- Exemplo de Uso Prático para Demonstração ---
if __name__ == "__main__":
    # Definimos t=3. O nó pode ter no máximo (2*3 - 1) = 5 chaves.
    T = 3 
    arvore = ArvoreB(T)

    print(f"--- DEMONSTRAÇÃO ÁRVORE B (Ordem T={T}) - Máx. 5 Chaves/Nó ---")
    
    # Sequência de inserção que força o SPLIT (10, 20, 30, 40, 50, 60)
    chaves = [10, 20, 30, 40, 50, 60]

    for chave in chaves:
        print(f"\n--- INSERINDO CHAVE: {chave} ---")
        arvore.inserir_chave(chave)
        # Se a chave for 60, o split da raiz deve ter ocorrido
        if chave == 60:
             print("--- *** SPLIT NA RAIZ OCORREU! (30 Subiu) *** ---")
        arvore.imprimir_arvore()


    print("\n--- TESTE DE BUSCA ---")
    
    # Busca por uma chave existente (40)
    resultado = arvore.buscar_chave(40)
    if resultado:
        print("Resultado da Busca por 40: ENCONTRADO.")
    
    # Busca por uma chave inexistente (99)
    resultado_inexistente = arvore.buscar_chave(99)
    if resultado_inexistente is None:
        print("Resultado da Busca por 99: NÃO ENCONTRADO (Correto).")
