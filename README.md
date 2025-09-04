//Rapaziada, espero que todos estejam bem. Estou no segundo semestre de Engenharia da Computação e estou aprendendo Python, a baixo deixarei o algoritmo.
//Se puderem avaliar, por favor, ainda estou no começo dessa linguagem. Obrigado a todos.
//O download está em .py

//Guys, I hope you're all doing well. I'm in my second semester of Computer Engineering and I'm learning Python. I'll leave the algorithm below.
//If you could please rate it, I'm still in the early stages of learning this language. Thank you all.
//The download is in .py

import tkinter as tk
from tkinter import messagebox

# Cria a janela principal
# --- LÓGICA DO JOGO ---

# Variável para controlar de quem é a vez
jogador_atual = "X"

# Matriz 3x3 para representar o estado do tabuleiro. None = vazio.
tabuleiro = [[None, None, None],
             [None, None, None],
             [None, None, None]]

# Lista para guardar os objetos dos botões e poder acessá-los depois
botoes = [[None, None, None],
          [None, None, None],
          [None, None, None]]

def verificar_vencedor():
    # Verificar linhas
    for linha in range(3):
        if tabuleiro[linha][0] == tabuleiro[linha][1] == tabuleiro[linha][2] and tabuleiro[linha][0] is not None:
            return tabuleiro[linha][0]
    
    # Verificar colunas
    for col in range(3):
        if tabuleiro[0][col] == tabuleiro[1][col] == tabuleiro[2][col] and tabuleiro[0][col] is not None:
            return tabuleiro[0][col]
            
    # Verificar diagonal principal
    if tabuleiro[0][0] == tabuleiro[1][1] == tabuleiro[2][2] and tabuleiro[0][0] is not None:
        return tabuleiro[0][0]
        
    # Verificar diagonal secundária
    if tabuleiro[0][2] == tabuleiro[1][1] == tabuleiro[2][0] and tabuleiro[0][2] is not None:
        return tabuleiro[0][2]
        
    return None

def verificar_empate():
    for linha in tabuleiro:
        for celula in linha:
            if celula is None:
                return False # Ainda existe um espaço vazio
    return True # Não há espaços vazios
janela = tk.Tk()
janela.title("Jogo da Velha (alpha)")

# --- INTERFACE GRÁFICA (GUI) ---

# Frame para conter os botões do tabuleiro
frame_tabuleiro = tk.Frame(janela)
frame_tabuleiro.pack() # Adiciona o frame à janela

def resetar_jogo():
    global jogador_atual, tabuleiro
    jogador_atual = "X"
    tabuleiro = [[None, None, None],
                 [None, None, None],
                 [None, None, None]]
    
    # Limpa a interface
    for linha in range(3):
        for coluna in range(3):
            botoes[linha][coluna].config(text="", state='normal')

# Botão para reiniciar o jogo
botao_reiniciar = tk.Button(janela, text="Reiniciar Jogo", command=resetar_jogo)
botao_reiniciar.pack(pady=10) # pady dá um espaço vertical

def ao_clicar(linha, coluna):
    global jogador_atual
    
    # Verifica se a célula está vazia e se o jogo não acabou
    if tabuleiro[linha][coluna] is None:
        # Atualiza a lógica do tabuleiro
        tabuleiro[linha][coluna] = jogador_atual
        
        # Atualiza o texto do botão na interface
        botoes[linha][coluna].config(text=jogador_atual, state='disabled')
        
        # Verifica se houve um vencedor
        vencedor = verificar_vencedor()
        if vencedor:
            messagebox.showinfo("Fim de Jogo", f"O jogador '{vencedor}' venceu!")
            
        elif verificar_empate():
            messagebox.showinfo("Fim de Jogo", "O jogo empatou!")
            
        else:
            # Troca o jogador
            jogador_atual = "O" if jogador_atual == "X" else "X"

for linha in range(3):
    for coluna in range(3):
        botoes[linha][coluna] = tk.Button(
            frame_tabuleiro, 
            text="", 
            font=('Arial', 40, 'bold'), 
            width=5, 
            height=2,
            # O comando lambda é crucial aqui para passar os argumentos corretos
            command=lambda l=linha, c=coluna: ao_clicar(l, c)
        )
        botoes[linha][coluna].grid(row=linha, column=coluna) # Posiciona o botão na grade

# Inicia o loop principal da aplicação (mantém a janela aberta)
janela.mainloop()
