import os
import shutil
import tkinter as tk
from tkinter import filedialog

def adicionar_pasta_origem():
    pasta = filedialog.askdirectory(title="Selecione uma Pasta de Origem")
    if pasta:
        pastas_origem_listbox.insert(tk.END, pasta)

def remover_pasta_origem():
    selecao = pastas_origem_listbox.curselection()
    if selecao:
        pastas_origem_listbox.delete(selecao)

def definir_pasta_destino():
    pasta_destino = filedialog.askdirectory(title="Selecione a Pasta de Destino")
    pasta_destino_var.set(pasta_destino)

def copiar_pastas_selecionadas():
    pastas_origem = list(pastas_origem_listbox.get(0, tk.END))
    pasta_destino = pasta_destino_var.get()
    nome_pasta = nome_pasta_var.get()

    for origem in pastas_origem:
        origem_completa = os.path.join(origem, nome_pasta)
        destino_completo = os.path.join(pasta_destino, nome_pasta)
        
        if os.path.exists(origem_completa):
            try:
                shutil.copytree(origem_completa, destino_completo)
                status_label.config(text=f"Pasta '{nome_pasta}' copiada para '{destino_completo}'", fg="green")
            except Exception as e:
                status_label.config(text=f"Erro ao copiar: {str(e)}", fg="red")
        else:
            status_label.config(text=f"Pasta '{nome_pasta}' não encontrada na origem.", fg="red")

app = tk.Tk()
app.title("Cópia de Pastas")

pastas_origem_listbox = tk.Listbox(app, selectmode=tk.MULTIPLE)
pastas_origem_listbox.pack(padx=10, pady=10, fill=tk.X)

button_adicionar_origem = tk.Button(app, text="Adicionar Pasta", command=adicionar_pasta_origem)
button_adicionar_origem.pack()

button_remover_origem = tk.Button(app, text="Remover Pasta", command=remover_pasta_origem)
button_remover_origem.pack()

pasta_destino_var = tk.StringVar()

frame_destino = tk.Frame(app)
frame_destino.pack(padx=10, pady=10, fill=tk.X)

label_destino = tk.Label(frame_destino, text="Pasta de Destino:")
label_destino.pack(side=tk.LEFT)

entry_destino = tk.Entry(frame_destino, textvariable=pasta_destino_var)
entry_destino.pack(side=tk.LEFT, padx=5)

button_destino = tk.Button(frame_destino, text="Selecionar", command=definir_pasta_destino)
button_destino.pack(side=tk.LEFT)

nome_pasta_var = tk.StringVar()

frame_nome = tk.Frame(app)
frame_nome.pack(padx=10, pady=10, fill=tk.X)

label_nome = tk.Label(frame_nome, text="Nome da Pasta:")
label_nome.pack(side=tk.LEFT)

entry_nome = tk.Entry(frame_nome, textvariable=nome_pasta_var)
entry_nome.pack(side=tk.LEFT, padx=5)

frame_botoes = tk.Frame(app)
frame_botoes.pack(padx=10, pady=10)

button_copiar = tk.Button(frame_botoes, text="Copiar Pastas", command=copiar_pastas_selecionadas)
button_copiar.pack()

status_label = tk.Label(app, text="", fg="green")
status_label.pack(padx=10, pady=(0, 10))

app.mainloop()
