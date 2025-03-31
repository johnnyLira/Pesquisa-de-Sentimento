import tkinter as tk
from tkinter import messagebox
import csv
from datetime import datetime
import os

class PesquisaSentimentoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Pesquisa de Sentimento")
        self.root.geometry("600x400")
        self.root.configure(bg="#ADD8E6")  # Cor de fundo azul claro

        # Criar arquivo CSV se não existir
        self.arquivo_csv = "sentimentos.csv"
        if not os.path.exists(self.arquivo_csv):
            with open(self.arquivo_csv, mode="w", newline="", encoding="utf-8") as file:
                writer = csv.writer(file)
                writer.writerow(["Sentimento", "Data/Hora"])

        # Contadores para cada sentimento
        self.contadores = {
            "Muito Feliz": 0,
            "Feliz": 0,
            "Neutro": 0,
            "Triste": 0,
            "Muito Triste": 0
        }
        self.carregar_contadores()  # Carrega os contadores do CSV ao iniciar

        # Senha para o administrador
        self.senha_admin = "admin123"

        # Inicia com a tela de senha
        self.tela_senha_inicial()

    def tela_senha_inicial(self):
        # Frame centralizado para a tela de senha inicial
        self.frame_senha_inicial = tk.Frame(self.root, bg="#ADD8E6")
        self.frame_senha_inicial.place(relx=0.5, rely=0.5, anchor="center")

        # Título
        tk.Label(self.frame_senha_inicial, text="Digite a Senha do Administrador", 
                 font=("Arial", 16, "bold"), bg="#ADD8E6", fg="#333").pack(pady=20)

        # Campo de entrada da senha
        self.senha_entry_inicial = tk.Entry(self.frame_senha_inicial, show="*", font=("Arial", 12), justify="center")
        self.senha_entry_inicial.pack(pady=10)

        # Botão para verificar a senha
        tk.Button(self.frame_senha_inicial, text="Confirmar", font=("Arial", 12), bg="#fff", fg="#333",
                  command=self.verificar_senha_inicial).pack(pady=10)

    def verificar_senha_inicial(self):
        senha_digitada = self.senha_entry_inicial.get()
        if senha_digitada == self.senha_admin:
            self.frame_senha_inicial.destroy()
            self.tela_abertura()
        else:
            messagebox.showerror("Erro", "Senha incorreta!")
            self.senha_entry_inicial.delete(0, tk.END)

    def tela_abertura(self):
        # Frame centralizado para a tela de abertura
        self.frame_abertura = tk.Frame(self.root, bg="#ADD8E6")
        self.frame_abertura.place(relx=0.5, rely=0.5, anchor="center")

        # Título
        tk.Label(self.frame_abertura, text="Bem-vindo à Pesquisa de Sentimento", 
                 font=("Arial", 20, "bold"), bg="#ADD8E6", fg="#333").pack(pady=50)

        # Botão para ir à tela de pesquisa
        tk.Button(self.frame_abertura, text="Iniciar Pesquisa", font=("Arial", 14), bg="#fff", fg="#333",
                  command=self.tela_pesquisa).pack(pady=20)

    def tela_pesquisa(self):
        # Remove a tela anterior
        if hasattr(self, 'frame_abertura'):
            self.frame_abertura.destroy()
        if hasattr(self, 'frame_admin'):
            self.frame_admin.destroy()
        if hasattr(self, 'frame_senha'):
            self.frame_senha.destroy()

        # Frame centralizado para a tela de pesquisa
        self.frame_pesquisa = tk.Frame(self.root, bg="#ADD8E6")
        self.frame_pesquisa.place(relx=0.5, rely=0.5, anchor="center")

        # Interface da pesquisa
        self.label = tk.Label(self.frame_pesquisa, text="COMO VOCÊ ESTÁ SE SENTINDO HOJE?", 
                              font=("Arial", 16), bg="#ADD8E6")
        self.label.pack(pady=20)

        self.emoji_frame = tk.Frame(self.frame_pesquisa, bg="#ADD8E6")
        self.emoji_frame.pack(pady=10)

        # Botões de Emoji
        self.emojis = [
            ("Muito Feliz", "😊👍"),
            ("Feliz", "😊"),
            ("Neutro", "😐"),
            ("Triste", "😢"),
            ("Muito Triste", "😢👎")
        ]

        self.nome_frame = tk.Frame(self.frame_pesquisa, bg="#ADD8E6")
        self.nome_frame.pack(pady=5)

        for sentimento, emoji in self.emojis:
            btn = tk.Button(self.emoji_frame, text=emoji, font=("Arial", 20), bg="#fff", fg="#FFD700",  
                            relief="flat", command=lambda s=sentimento: self.enviar_sentimento(s))
            btn.pack(side="left", padx=10)
            btn.bind("<Enter>", lambda e, b=btn: b.config(bg="#e0e0e0"))
            btn.bind("<Leave>", lambda e, b=btn: b.config(bg="#fff"))

            nome_label = tk.Label(self.nome_frame, text=sentimento, font=("Arial", 12), bg="#ADD8E6", fg="black")
            nome_label.pack(side="left", padx=14)

        # Mensagem de confirmação
        self.mensagem_label = tk.Label(self.frame_pesquisa, text="", font=("Arial", 12), bg="#ADD8E6", fg="#666")
        self.mensagem_label.pack(pady=10)

        # Frame para botões de ação
        self.botoes_frame = tk.Frame(self.frame_pesquisa, bg="#ADD8E6")
        self.botoes_frame.pack(pady=10)

        tk.Button(self.botoes_frame, text="Administração", font=("Arial", 12), bg="#FFA500", fg="white",
                  command=self.tela_senha).pack(side="left", padx=5)
        tk.Button(self.botoes_frame, text="Sair do Aplicativo", font=("Arial", 12), bg="red", fg="white",
                  command=self.sair_aplicativo).pack(side="left", padx=5)

    def tela_senha(self):
        self.frame_pesquisa.destroy()

        # Frame centralizado para a tela de senha
        self.frame_senha = tk.Frame(self.root, bg="#ADD8E6")
        self.frame_senha.place(relx=0.5, rely=0.5, anchor="center")

        tk.Label(self.frame_senha, text="Digite a Senha do Administrador", 
                 font=("Arial", 16, "bold"), bg="#ADD8E6", fg="#333").pack(pady=20)

        self.senha_entry = tk.Entry(self.frame_senha, show="*", font=("Arial", 12), justify="center")
        self.senha_entry.pack(pady=10)

        tk.Button(self.frame_senha, text="Confirmar", font=("Arial", 12), bg="#fff", fg="#333",
                  command=self.verificar_senha).pack(pady=10)
        tk.Button(self.frame_senha, text="Voltar", font=("Arial", 12), bg="#fff", fg="#333",
                  command=self.tela_pesquisa).pack(pady=10)

    def verificar_senha(self):
        senha_digitada = self.senha_entry.get()
        if senha_digitada == self.senha_admin:
            self.tela_admin()
        else:
            messagebox.showerror("Erro", "Senha incorreta!")
            self.senha_entry.delete(0, tk.END)

    def tela_admin(self):
        self.frame_senha.destroy()

        # Frame centralizado para a tela de administração
        self.frame_admin = tk.Frame(self.root, bg="#ADD8E6")
        self.frame_admin.place(relx=0.5, rely=0.5, anchor="center")

        tk.Label(self.frame_admin, text="ESCOLA TÉCNICA ESTADUAL GOVERNADOR EDUARDO CAMPOS", 
                 font=("Arial", 20, "bold"), bg="#ADD8E6", fg="#333").pack(pady=10)
        tk.Label(self.frame_admin, text="PAINEL DE ADMINISTRAÇÃO", 
                 font=("Arial", 20, "bold"), bg="#ADD8E6", fg="#333").pack(pady=10)

        self.sentimentos_frame = tk.Frame(self.frame_admin, bg="#ADD8E6")
        self.sentimentos_frame.pack(pady=10)

        self.resultado_labels = {}
        for sentimento, emoji in self.emojis:
            sentimento_frame = tk.Frame(self.sentimentos_frame, bg="#ADD8E6")
            sentimento_frame.pack(side="left", padx=10)

            btn = tk.Button(sentimento_frame, text=emoji, font=("Arial", 20), bg="#fff", fg="#FFD700", 
                            relief="flat", state="disabled")
            btn.pack(side="top", pady=2)

            nome_label = tk.Label(sentimento_frame, text=sentimento, font=("Arial", 12), bg="#ADD8E6", fg="black")
            nome_label.pack(side="top", pady=2)

            resultado_label = tk.Label(sentimento_frame, text=f"{self.contadores[sentimento]}", 
                                       font=("Arial", 12), bg="#ADD8E6", fg="black")
            resultado_label.pack(side="top", pady=2)
            self.resultado_labels[sentimento] = resultado_label

        self.total_pessoas = sum(self.contadores.values())
        self.total_label = tk.Label(self.frame_admin, text=f"Total de Pessoas Pesquisadas: {self.total_pessoas}", 
                                    font=("Arial", 12), bg="#ADD8E6", fg="#333")
        self.total_label.pack(pady=5)

        tk.Button(self.frame_admin, text="Resetar Dados", font=("Arial", 14), bg="#FF4500", fg="white",
                  command=self.resetar_dados).pack(pady=10)
        tk.Button(self.frame_admin, text="Voltar à Pesquisa", font=("Arial", 14), bg="#fff", fg="#333",
                  command=self.tela_pesquisa).pack(pady=10)

    def carregar_contadores(self):
        if os.path.exists(self.arquivo_csv):
            with open(self.arquivo_csv, mode="r", encoding="utf-8") as file:
                reader = csv.DictReader(file)
                for row in reader:
                    sentimento = row["Sentimento"]
                    if sentimento in self.contadores:
                        self.contadores[sentimento] += 1

    def enviar_sentimento(self, sentimento):
        try:
            data_hora = datetime.now().strftime("%d/%m/%Y %H:%M:%S")
            with open(self.arquivo_csv, mode="a", newline="", encoding="utf-8") as file:
                writer = csv.writer(file)
                writer.writerow([sentimento, data_hora])
            self.contadores[sentimento] += 1
            self.mensagem_label.config(text="Obrigado por sua resposta!")
            self.root.after(2000, lambda: self.mensagem_label.config(text=""))
        except Exception as e:
            messagebox.showerror("Erro", f"Erro ao gravar: {str(e)}")

    def resetar_dados(self):
        if messagebox.askyesno("Confirmação", "Tem certeza que deseja resetar os dados? Isso apagará todos os registros."):
            for sentimento in self.contadores:
                self.contadores[sentimento] = 0
            self.total_pessoas = sum(self.contadores.values())
            with open(self.arquivo_csv, mode="w", newline="", encoding="utf-8") as file:
                writer = csv.writer(file)
                writer.writerow(["Sentimento", "Data/Hora"])
            for sentimento in self.resultado_labels:
                self.resultado_labels[sentimento].config(text=f"{self.contadores[sentimento]}")
            self.total_label.config(text=f"Total de Pessoas Pesquisadas: {self.total_pessoas}")
            messagebox.showinfo("Sucesso", "Dados resetados com sucesso!")

    def sair_aplicativo(self):
        self.root.destroy()

    def run(self):
        self.root.mainloop()

# Inicialização do aplicativo
if __name__ == "__main__":
    root = tk.Tk()
    app = PesquisaSentimentoApp(root)
    app.run()
