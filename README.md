
import os
import tkinter as tk
from tkinter import messagebox
import pandas as pd
from tkinter import scrolledtext
from tkinter import ttk, filedialog, messagebox
from tkinter import messagebox
from datetime import datetime

class App(tk.Tk):
    def __init__(self):
        super().__init__()

        for i in range(5):
            self.grid_rowconfigure(i, weight=1)
            self.grid_columnconfigure(i, weight=1)

        # Define button colors
        button_colors = [
            ("#FF0100", "white"),  # Red
            ("#00FF00", "white"),  # Green
            ("#0100FF", "white"),  # Blue
            ("#FFFF00", "black"),  # Yellow
            ("#FF10FF", "Blue")   # Magenta
        ]

        buttons_data = [
            ("Debug", DebugApp),
            ("Agenda", AgendaApp),
            ("Conversor", ConversorApp),
            ("Docs", DocsApp),
            ("Processos", ProcessosApp),
            ("Inquilinos", InquilinosApp),
            ("Vencimentos", VencimentosApp),
            ("Gastos", GastosApp),
            ("Cria", CriaApp),
            ("Busca", BuscaApp),
            ("Planilhas", PlanilhasApp),
            ("Calculadora", CalculadoraApp),
            ("Sabesp", SabespApp),
            ("Enel", EnelApp),
            ("Locador", LocadorApp),
            ("Prestador", PrestadorApp),
            ("Ajuda", AjudaApp),
            ("AI", AIApp),
            ("Exporta", ExportarApp),
            ("Contatos", ContatosApp),
            ("Imoveis", ImoveisApp),
            ("Fechar App", self.quit)
        ]

        for i, (text, app_class) in enumerate(buttons_data):
            row = i // 5
            col = i % 5

            # Assign color to each button
            button_color = button_colors[i % len(button_colors)]

            button = tk.Button(self, text=text, command=lambda app_class=app_class: self.open_app(app_class), bg=button_color[0], fg=button_color[1])
            button.grid(row=row, column=col, sticky="nsew")

        # Display current time
        self.time_label = tk.Label(self, text="", font=("Helvetica", 14), bg="black", fg="white")
        self.time_label.grid(row=5, column=0, columnspan=5, pady=10)
        self.update_time()

    def update_time(self):
        # Update the displayed time every second
        current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.time_label.config(text=current_time)
        self.after(1000, self.update_time)

    def open_app(self, app_class):
        app = app_class()
        app.mainloop()



class DebugApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("DebugApp")
        self.geometry("1400x1200")
        self.configure(bg="black")

        self.password = "0000"

        self.create_password_entry()
        self.create_create_folders_button()
        self.create_add_files_button()

    def create_password_entry(self):
        self.password_label = tk.Label(self, text="Senha:", font=("Helvetica", 16), bg="white")
        self.password_label.pack(pady=20)

        self.password_entry = tk.Entry(self, show="*", font=("Helvetica", 16))
        self.password_entry.pack()

        self.enter_button = tk.Button(self, text="Entrar", command=self.verify_password, font=("Helvetica", 16), bg="green", fg="white")
        self.enter_button.pack(pady=5)

    def verify_password(self):
        if self.password_entry.get() == self.password:
            self.create_folders_button.config(state="normal")
            self.add_files_button.config(state="normal")
            messagebox.showinfo("Sucesso", "Senha correta! Agora você pode criar as pastas e adicionar arquivos.")
        else:
            messagebox.showerror("Erro", "Senha incorreta!")

    def create_create_folders_button(self):
        self.create_folders_button = tk.Button(self, text="Criar Pastas", command=self.create_folders, font=("Helvetica", 16), bg="blue", fg="white", state="disabled")
        self.create_folders_button.pack(pady=10)

    def create_add_files_button(self):
        self.add_files_button = tk.Button(self, text="Adicionar Arquivos", command=self.add_files, font=("Helvetica", 16), bg="orange", fg="white", state="disabled")
        self.add_files_button.pack(pady=10)

    def create_folders(self):
        folders_to_create = ["d:/adm", "d:/adm/log", "d:/adm/cpu"]
        for i in range(1, 26):
            folder_path = f"d:/adm/{i}"
            folders_to_create.append(folder_path)

        for folder in folders_to_create:
            try:
                os.makedirs(folder)
                messagebox.showinfo("Sucesso", f"Pasta {folder} criada com sucesso!")
            except FileExistsError:
                messagebox.showwarning("Aviso", f"A pasta {folder} já existe.")

    def add_files(self):
        files_to_add = ["file1.txt", "file2.txt", "file3.txt"]  # List of files to add
        for file in files_to_add:
            file_path = f"d:/adm/{file}"
            try:
                with open(file_path, "w") as f:
                    f.write("Sample content")  # Writing sample content to the file
                messagebox.showinfo("Sucesso", f"Arquivo {file} adicionado com sucesso!")
            except Exception as e:
                messagebox.showerror("Erro", f"Ocorreu um erro ao adicionar o arquivo {file}: {e}")

#fazer script thon  com widget com campos com 1 linha para cada Função 
#campos em formato de coluna
#Data do Evento
#Nome do Evento
#Endereço
#Celular app
#E-mail
#Evento a lembrar
#criar botões para salvar como txt na pasta d:\adm\20 
#criar botões para procurar na cpu arquivo na pasta d:\adm\20 e listar
#criar botões para recomeçar cria botão para anlizar todas pasta em d:\adm reorna com nome celular email printa em nova widget




class AgendaApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Agenda App")
        self.geometry("600x400")
        self.configure(bg="white")

        # Create labels and entry widgets
        fields = ["Data do Evento", "Nome do Evento", "Endereço", "Celular app", "E-mail", "Evento a lembrar"]
        self.entries = {}
        for i, field in enumerate(fields):
            label = tk.Label(self, text=field, bg="white")
            label.grid(row=i, column=0, sticky="w", padx=10, pady=5)
            entry = tk.Entry(self)
            entry.grid(row=i, column=1, padx=10, pady=5)
            self.entries[field] = entry

        # Create buttons
        self.create_save_button()
        self.create_search_button()
        self.create_restart_button()
        self.create_analyze_button()

    def create_save_button(self):
        save_button = tk.Button(self, text="Salvar como TXT", command=self.save_txt)
        save_button.grid(row=len(self.entries), column=0, columnspan=2, padx=10, pady=5)

    def create_search_button(self):
        search_button = tk.Button(self, text="Procurar na pasta", command=self.search_txt)
        search_button.grid(row=len(self.entries) + 1, column=0, columnspan=2, padx=10, pady=5)

    def create_restart_button(self):
        restart_button = tk.Button(self, text="Recomeçar", command=self.restart)
        restart_button.grid(row=len(self.entries) + 2, column=0, columnspan=2, padx=10, pady=5)

    def create_analyze_button(self):
        analyze_button = tk.Button(self, text="Analisar pasta d:\\adm", command=self.analyze_folder)
        analyze_button.grid(row=len(self.entries) + 3, column=0, columnspan=2, padx=10, pady=5)

    def save_txt(self):
        dest_dir = "d:\\adm\\20"
        os.makedirs(dest_dir, exist_ok=True)
        data = "\n".join([f"{field}: {entry.get()}" for field, entry in self.entries.items()])
        with open(os.path.join(dest_dir, "evento.txt"), "w") as f:
            f.write(data)
        print("Data saved successfully.")

    def search_txt(self):
        directory = "d:\\adm\\20"
        if os.path.exists(directory):
            files = [f for f in os.listdir(directory) if f.endswith('.txt')]
            if files:
                print("TXT files found:")
                for file in files:
                    print(file)
            else:
                print("No TXT files found in", directory)
        else:
            print("Directory", directory, "does not exist")

    def restart(self):
        self.destroy()
        AgendaApp().mainloop()

    def analyze_folder(self):
        directory = "d:\\adm"
        if os.path.exists(directory):
            print("Searching for files in", directory)
            for root, dirs, files in os.walk(directory):
                for file in files:
                    if file.endswith('.txt'):
                        file_path = os.path.join(root, file)
                        print("File:", file_path)
                        with open(file_path, 'r') as f:
                            data = f.read()
                            print("Data:", data)
        else:
            print("Directory", directory, "does not exist")





class ConversorApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Conversor App")
        self.geometry("400x200")

        self.create_widgets()

    def create_widgets(self):
        self.label = tk.Label(self, text="Conversor App", font=("Helvetica", 16))
        self.label.pack(pady=5)

        self.convert_button = tk.Button(self, text="Convert CSV to Excel", command=self.convert_csv_to_excel)
        self.convert_button.pack(pady=5)

        self.convert_button = tk.Button(self, text="Convert Excel to CSV", command=self.convert_excel_to_csv)
        self.convert_button.pack(pady=5)

    def convert_csv_to_excel(self):
        csv_file_path = filedialog.askopenfilename(filetypes=[("CSV files", "*.csv")])
        if csv_file_path:
            df = pd.read_csv(csv_file_path)
            excel_file_path = filedialog.asksaveasfilename(defaultextension=".xlsx", filetypes=[("Excel files", "*.xlsx")])
            if excel_file_path:
                df.to_excel(excel_file_path, index=False)
                tk.messagebox.showinfo("Conversion Complete", "CSV converted to Excel successfully.")

    def convert_excel_to_csv(self):
        excel_file_path = filedialog.askopenfilename(filetypes=[("Excel files", "*.xlsx")])
        if excel_file_path:
            df = pd.read_excel(excel_file_path)
            csv_file_path = filedialog.asksaveasfilename(defaultextension=".csv", filetypes=[("CSV files", "*.csv")])
            if csv_file_path:
                df.to_csv(csv_file_path, index=False)
                tk.messagebox.showinfo("Conversion Complete", "Excel converted to CSV successfully.")



class DocsApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Docs App")
        self.geometry("400x200")

        self.create_widgets()

    def create_widgets(self):
        self.label = tk.Label(self, text="Digite o conteúdo para o documento:", pady=5)
        self.label.pack()

        self.textbox = tk.Text(self, height=5, width=40)
        self.textbox.pack()

        self.generate_button = tk.Button(self, text="Gerar Documento", command=self.generate_document)
        self.generate_button.pack(pady=5)

    def generate_document(self):
        content = self.textbox.get("1.0", tk.END).strip()
        if content:
            try:
                document = Document()
                document.add_paragraph(content)
                document.save("generated_document.docx")
                messagebox.showinfo("Sucesso", "Documento gerado com sucesso!")
            except Exception as e:
                messagebox.showerror("Erro", f"Ocorreu um erro ao gerar o documento: {str(e)}")
        else:
            messagebox.showwarning("Aviso", "Digite o conteúdo para o documento.")




class ProcessosApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Processos App")
        self.geometry("1200x1000")  # Set the initial size of the window
        
        self.label = tk.Label(self, text="Welcome to Processos App!", font=("Arial", 10))
        self.label.pack(pady=20)
        
        self.process_button = tk.Button(self, text="Start Process", command=self.start_process)
        self.process_button.pack(pady=5)
        
        self.result_label = tk.Label(self, text="", font=("Arial", 10))
        self.result_label.pack(pady=5)
        
    def start_process(self):
        # This is just a dummy function to simulate some process
        # You can replace it with your actual processing logic
        self.result_label.config(text="Processing...")  # Update label to indicate processing
        # Simulate a long-running process
        self.after(2000, self.process_complete)
        
    def process_complete(self):
        # This method is called when the process is complete
        self.result_label.config(text="Process Completed!")  # Update label to indicate completion
        

#fazer script thon  com widget com campos com 1 linha para cada Função 
#campos em formato de coluna
#Data do Evento
#Nome do Evento
#Endereço
#Celular app
#E-mail
#Evento a lembrar
#criar botões para salvar como txt na pasta d:\adm\2 
#criar botões para procurar na cpu arquivo na pasta d:\adm\2 e listar
#criar botões para recomeçar 


class InquilinosApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Inquilinos App")

        self.create_widgets()

    def create_widgets(self):
        fields = ['Data do Evento', 'Nome do Evento', 'Endereço', 'Celular app', 'E-mail', 'Evento a lembrar']
        self.entries = {}
        for i, field in enumerate(fields):
            label = tk.Label(self, text=field)
            label.grid(row=i, column=0, sticky='e')
            entry = tk.Entry(self)
            entry.grid(row=i, column=1)
            self.entries[field] = entry

        save_button = tk.Button(self, text="Salvar como TXT", command=self.save_data)
        save_button.grid(row=len(fields), column=0, columnspan=2, pady=10)

        search_button = tk.Button(self, text="Procurar arquivo", command=self.search_file)
        search_button.grid(row=len(fields) + 1, column=0, columnspan=2, pady=10)

        restart_button = tk.Button(self, text="Recomeçar", command=self.restart_form)
        restart_button.grid(row=len(fields) + 2, column=0, columnspan=2, pady=10)

    def save_data(self):
        data = ''
        for field, entry in self.entries.items():
            data += f"{field}: {entry.get()}\n"
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", initialdir="d:/adm/2", title="Salvar como TXT")
        if file_path:
            with open(file_path, 'w') as file:
                file.write(data)
            tk.messagebox.showinfo("Sucesso", "Dados salvos com sucesso!")

    def search_file(self):
        file_path = filedialog.askopenfilename(initialdir="d:/adm/2", title="Procurar arquivo")
        if file_path:
            with open(file_path, 'r') as file:
                tk.messagebox.showinfo("Arquivo Encontrado", file.read())

    def restart_form(self):
        for entry in self.entries.values():
            entry.delete(0, 'end')

class VencimentosApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("VencimentosApp ")
        self.geometry("600x400")
        self.configure(bg="white")



class GastosApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Gastos App")
        self.create_buttons()

    def create_buttons(self):
        # Creating three buttons
        button1 = tk.Button(self, text="Button 1", command=self.button1_clicked)
        button1.pack(pady=5)

    def button1_clicked(self):
        print("Button 1 clicked")



 
class CriaApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Cria App")
        self.create_buttons()

    def create_buttons(self):
        # Creating three buttons
        button1 = tk.Button(self, text="Button 1", command=self.button1_clicked)
        button1.pack(pady=5)

        button2 = tk.Button(self, text="Button 2", command=self.button2_clicked)
        button2.pack(pady=5)

        button3 = tk.Button(self, text="Button 3", command=self.button3_clicked)
        button3.pack(pady=5)

    def button1_clicked(self):
        print("Button 1 clicked")

    def button2_clicked(self):
        print("Button 2 clicked")

    def button3_clicked(self):
        print("Button 3 clicked")




class BuscaApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Busca App")
        self.geometry("1400x1200")
        self.configure(bg="black")
        
        # Functions to list rental information, receipts, RGI, and duplicates
        def list_rentals():
            with open(r'd:\adm\3\alugueis.txt', 'r') as file:
                rental_info = file.read()
            display_info(rental_info)
        
        def list_receipts():
            with open(r'd:\adm\3\comprovantes.txt', 'r') as file:
                receipt_info = file.read()
            display_info(receipt_info)
        
        def list_rgi():
            with open(r'd:\adm\3\rgi.txt', 'r') as file:
                rgi_info = file.read()
            display_info(rgi_info)
        
        def list_duplicates():
            with open(r'd:\adm\3\2via.txt', 'r') as file:
                duplicate_info = file.read()
            display_info(duplicate_info)
        
        # Function to display information in a label widget
        def display_info(info):
            label.config(text=info)
        
        # Create buttons
        button_texts = ["Alugueis", "Comprovantes", "RGI", "2 Via"]
        button_functions = [list_rentals, list_receipts, list_rgi, list_duplicates]
        buttons = []
        for i in range(3):
            for j in range(3):
                index = i * 3 + j
                if index < len(button_texts):
                    button = tk.Button(self, text=button_texts[index], bg="green", fg="white", command=button_functions[index])
                    button.grid(row=i, column=j, padx=5, pady=5, ipadx=20, ipady=5)
                    buttons.append(button)
        
        # Create label to display information
        label = tk.Label(self, bg="black", fg="white", justify="left", font=("Arial", 12), wraplength=1300)
        label.grid(row=3, column=0, columnspan=3, padx=20, pady=20, sticky="nsew")


class PlanilhasApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Planilhas App")
        self.geometry("1400x1200")
        
        # Frame principal
        self.main_frame = tk.Frame(self)
        self.main_frame.pack(fill=tk.BOTH, expand=True)

        # Lista para armazenar dados dos arquivos TXT
        self.data = []

        # Barra de rolagem
        self.scrollbar = ttk.Scrollbar(self.main_frame)
        self.scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

        # Text widget para exibir dados
        self.text_widget = tk.Text(self.main_frame, wrap=tk.NONE, yscrollcommand=self.scrollbar.set)
        self.text_widget.pack(fill=tk.BOTH, expand=True)
        self.scrollbar.config(command=self.text_widget.yview)

        # Botão para analisar arquivos TXT
        self.analyze_button = tk.Button(self, text="Analisar Arquivos TXT", command=self.analyze_txt_files)
        self.analyze_button.pack()

        # Botão para exportar planilha para Excel
        self.export_button = tk.Button(self, text="Exportar para Excel", command=self.export_to_excel)
        self.export_button.pack()

    def analyze_txt_files(self):
        # Limpar dados anteriores
        self.data.clear()
        self.text_widget.delete(1.0, tk.END)

        # Diretório contendo arquivos TXT
        directory = "D:/adm"

        # Analisar todos os arquivos TXT dentro do diretório
        for filename in os.listdir(directory):
            if filename.endswith(".txt"):
                file_path = os.path.join(directory, filename)
                with open(file_path, "r") as file:
                    content = file.read()
                    # Adicionar dados à lista
                    self.data.append(content)
                    # Adicionar dados ao widget de texto
                    self.text_widget.insert(tk.END, content + "\n")

    def export_to_excel(self):
        if self.data:
            # Criar DataFrame com os dados
            df = pd.DataFrame({"Dados": self.data})
            # Solicitar local para salvar o arquivo Excel
            file_path = filedialog.asksaveasfilename(defaultextension=".xlsx")
            if file_path:
                # Exportar DataFrame para Excel
                df.to_excel(file_path, index=False)
                messagebox.showinfo("Exportar para Excel", "Planilha exportada com sucesso!")



class CalculadoraApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Calculadora App")
        self.geometry("1400x1200")
        self.configure(bg="black")

        self.result_label = tk.Label(self, text="", font=('Arial', 24), bg='black', fg='white')
        self.result_label.pack(pady=20)

        buttons_frame = tk.Frame(self, bg="black")
        buttons_frame.pack()

        buttons = [
            ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
            ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
            ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
            ('0', 4, 0), ('.', 4, 1), ('=', 4, 2), ('+', 4, 3)
        ]

        for (text, row, col) in buttons:
            button = tk.Button(buttons_frame, text=text, font=('Arial', 18), bg='black', fg='white',
                               command=lambda t=text: self.on_button_click(t))
            button.grid(row=row, column=col, padx=5, pady=5, sticky="nsew")

    def on_button_click(self, char):
        if char == '=':
            try:
                result = eval(self.result_label["text"])
                self.result_label["text"] = str(result)
            except Exception as e:
                self.result_label["text"] = "Error"
        else:
            current_text = self.result_label["text"]
            if current_text == "Error":
                current_text = ""
            self.result_label["text"] = current_text + char




class SabespApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Sabesp App")
        self.geometry("1400x1200")
        self.configure(background="black")

        # Campos de entrada
        campos = ["Data Cadastro Sabesp:", "Tipo Imovel:", "Nome Responsável Contratos:",
                  "Numero RGI:", "Valor Reparos Sabesp:", "Valor Divida Sabesp:"]

        for i, campo in enumerate(campos):
            tk.Label(self, text=campo, fg="white", bg="black").grid(row=i, column=0, sticky="e", padx=5, pady=5)
            tk.Entry(self, bg="white").grid(row=i, column=1, padx=5, pady=5)

        # Botões
        tk.Button(self, text="Recomeçar", command=self.recomecar).grid(row=len(campos)+1, column=0, pady=5)
        tk.Button(self, text="Salvar TXT", command=self.salvar_txt).grid(row=len(campos)+1, column=1, pady=5)
        tk.Button(self, text="Procurar TXT", command=self.procurar_txt).grid(row=len(campos)+1, column=2, pady=5)
        tk.Button(self, text="Abrir Arquivo", command=self.abrir_arquivo).grid(row=len(campos)+1, column=3, pady=5)

    def recomecar(self):
        # Limpar os campos de entrada
        for widget in self.winfo_children():
            if isinstance(widget, tk.Entry):
                widget.delete(0, tk.END)

    def salvar_txt(self):
        # Obter os dados dos campos de entrada
        dados = []
        for widget in self.winfo_children():
            if isinstance(widget, tk.Entry):
                dados.append(widget.get())
        
        # Salvar os dados em um arquivo de texto
        try:
            with open("d:/adm/6/sabesp_data.txt", "w") as file:
                for dado in dados:
                    file.write(dado + "\n")
            messagebox.showinfo("Sucesso", "Dados salvos com sucesso!")
        except Exception as e:
            messagebox.showerror("Erro", f"Erro ao salvar dados: {e}")

    def procurar_txt(self):
        # Procurar arquivo TXT na pasta especificada
        try:
            filepath = filedialog.askopenfilename(initialdir="d:/adm/6", title="Selecione o arquivo", filetypes=(("Arquivos de Texto", "*.txt"),))
            if filepath:
                with open(filepath, "r") as file:
                    # Exibir o conteúdo do arquivo
                    conteudo = file.read()
                messagebox.showinfo("Conteúdo do Arquivo", conteudo)
        except Exception as e:
            messagebox.showerror("Erro", f"Erro ao abrir arquivo: {e}")

    def abrir_arquivo(self):
        # Abrir arquivo para salvar na pasta especificada
        try:
            filepath = filedialog.asksaveasfilename(initialdir="d:/adm/6", title="Salvar como", filetypes=(("Arquivos de Texto", "*.txt"),))
            if filepath:
                with open(filepath, "w") as file:
                    messagebox.showinfo("Sucesso", "Arquivo salvo com sucesso!")
        except Exception as e:
            messagebox.showerror("Erro", f"Erro ao salvar arquivo: {e}")





class EnelApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Enel App")
        self.geometry("1400x1200")
        self.configure(bg="black")

        self.create_fields()
        self.create_buttons()

    def create_fields(self):
        fields = [
            "Data Cadastro Enel", 
            "Tipo Imovel", 
            "Nome Responsável Contratos", 
            "Numero RGI", 
            "Valor Reparos Sabesp", 
            "Valor Divida Sabesp"
        ]

        for i, field in enumerate(fields):
            label = tk.Label(self, text=field, bg="black", fg="white")
            label.grid(row=i, column=0, padx=5, pady=5, sticky="e")
            entry = tk.Entry(self, bg="white", fg="black")
            entry.grid(row=i, column=1, padx=5, pady=5)

    def create_buttons(self):
        restart_button = tk.Button(self, text="Recomeçar", command=self.restart)
        restart_button.grid(row=6, column=0, pady=20)

        save_button = tk.Button(self, text="Salvar", command=self.save)
        save_button.grid(row=6, column=1, pady=20)

        browse_button = tk.Button(self, text="Procurar", command=self.browse)
        browse_button.grid(row=6, column=2, pady=20)

        open_button = tk.Button(self, text="Abrir", command=self.open_file)
        open_button.grid(row=6, column=3, pady=20)

    def restart(self):
        # Logic to reset all fields
        pass

    def save(self):
        # Logic to save data to a txt file in d:\adm\7
        pass

    def browse(self):
        # Logic to browse txt files in d:\adm\7 and list them
        pass

    def open_file(self):
        file_path = filedialog.askopenfilename(initialdir="d:/adm/7", title="Select File")
        # Logic to open and read the selected file
        pass





class LocadorApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Locador App")
        self.geometry("1400x1200")
        self.configure(bg="black")
        
        # Labels
        tk.Label(self, text="Nome do Contrato:", bg="black", fg="white").pack()
        self.nome_contrato_entry = tk.Entry(self, bg="white", fg="black")
        self.nome_contrato_entry.pack()
        
        tk.Label(self, text="Data do Contrato:", bg="black", fg="white").pack()
        self.data_contrato_entry = tk.Entry(self, bg="white", fg="black")
        self.data_contrato_entry.pack()
        
        tk.Label(self, text="Tipo de Contrato:", bg="black", fg="white").pack()
        self.tipo_contrato_options = tk.StringVar(self)
        self.tipo_contrato_options.set("Selecione")
        tk.OptionMenu(self, self.tipo_contrato_options, "Aluguel", "Enel").pack()
        
        # Buttons
        tk.Button(self, text="Salvar", command=self.salvar).pack()
        tk.Button(self, text="Procurar", command=self.procurar).pack()
        tk.Button(self, text="Recomeçar", command=self.recomecar).pack()
        
    def salvar(self):
        nome_contrato = self.nome_contrato_entry.get()
        tipo_contrato = self.tipo_contrato_options.get()
        file_name = f"d:/adm/2/{tipo_contrato}.txt"
        data_contrato = self.data_contrato_entry.get()
        with open(file_name, "w") as file:
            file.write(f"Nome do Contrato: {nome_contrato}\n")
            file.write(f"Data do Contrato: {data_contrato}\n")
            file.write(f"Tipo de Contrato: {tipo_contrato}\n")
        print("Salvo com sucesso!")
        
    def procurar(self):
        file_path = filedialog.askopenfilename(initialdir="C:/adm", title="Selecione o arquivo")
        print("Arquivo selecionado:", file_path)
        
    def recomecar(self):
        self.nome_contrato_entry.delete(0, tk.END)
        self.data_contrato_entry.delete(0, tk.END)
        self.tipo_contrato_options.set("Selecione")



class PrestadorApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("PrestadorApp")
        self.geometry("1400x1200")
        self.configure(bg="black")

        # Entry fields for data entry
        self.entry_labels = ["Nome:", "Idade:", "Profissão:", "Endereço:", "Telefone:"]
        self.entry_fields = []
        for i, label_text in enumerate(self.entry_labels):
            label = tk.Label(self, text=label_text, fg="white", bg="black")
            label.grid(row=i, column=0, padx=5, pady=5, sticky="e")
            entry = tk.Entry(self)
            entry.grid(row=i, column=1, padx=5, pady=5, sticky="w")
            self.entry_fields.append(entry)

        # Create buttons
        open_button = tk.Button(self, text="Abrir", command=self.open_file)
        open_button.grid(row=len(self.entry_labels), column=0, padx=5, pady=5)

        save_button = tk.Button(self, text="Salvar", command=self.save_file)
        save_button.grid(row=len(self.entry_labels), column=1, padx=5, pady=5)

        export_button = tk.Button(self, text="Exportar", command=self.export_data)
        export_button.grid(row=len(self.entry_labels), column=2, padx=5, pady=5)

    def open_file(self):
        file_path = filedialog.askopenfilename(filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
        if file_path:
            # Do something with the opened file
            print("File opened:", file_path)

    def save_file(self):
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
        if file_path:
            # Save data to the file
            print("File saved as:", file_path)

    def export_data(self):
        # Example export functionality
        print("Data exported.")

        self.create_widgets()

    def create_widgets(self):
        self.label_cadastro = tk.Label(self, text="Data do Cadastro do Prestador:", fg="white", bg="black")
        self.label_cadastro.grid(row=0, column=0, sticky="w", padx=5, pady=5)
        self.entry_cadastro = tk.Entry(self, width=20)
        self.entry_cadastro.grid(row=0, column=1, padx=5, pady=5)

        self.label_nome = tk.Label(self, text="Nome do Prestador:", fg="white", bg="black")
        self.label_nome.grid(row=1, column=0, sticky="w", padx=5, pady=5)
        self.entry_nome = tk.Entry(self, width=20)
        self.entry_nome.grid(row=1, column=1, padx=5, pady=5)

        self.label_endereco = tk.Label(self, text="Endereço:", fg="white", bg="black")
        self.label_endereco.grid(row=2, column=0, sticky="w", padx=5, pady=5)
        self.entry_endereco = tk.Entry(self, width=20)
        self.entry_endereco.grid(row=2, column=1, padx=5, pady=5)

        self.label_cep = tk.Label(self, text="CEP:", fg="white", bg="black")
        self.label_cep.grid(row=3, column=0, sticky="w", padx=5, pady=5)
        self.entry_cep = tk.Entry(self, width=20)
        self.entry_cep.grid(row=3, column=1, padx=5, pady=5)

        self.label_celular = tk.Label(self, text="Celular app:", fg="white", bg="black")
        self.label_celular.grid(row=4, column=0, sticky="w", padx=5, pady=5)
        self.entry_celular = tk.Entry(self, width=20)
        self.entry_celular.grid(row=4, column=1, padx=5, pady=5)

        self.label_email = tk.Label(self, text="E-mail:", fg="white", bg="black")
        self.label_email.grid(row=5, column=0, sticky="w", padx=5, pady=5)
        self.entry_email = tk.Entry(self, width=20)
        self.entry_email.grid(row=5, column=1, padx=5, pady=5)

        self.label_oficio = tk.Label(self, text="Oficio:", fg="white", bg="black")
        self.label_oficio.grid(row=6, column=0, sticky="w", padx=5, pady=5)
        self.entry_oficio = tk.Entry(self, width=20)
        self.entry_oficio.grid(row=6, column=1, padx=5, pady=5)

        self.button_save = tk.Button(self, text="Salvar", command=self.save_data)
        self.button_save.grid(row=7, column=0, padx=5, pady=5)

        self.button_open = tk.Button(self, text="Abrir", command=self.open_data)
        self.button_open.grid(row=7, column=1, padx=5, pady=5)

        self.button_restart = tk.Button(self, text="Recomeçar", command=self.restart)
        self.button_restart.grid(row=7, column=2, padx=5, pady=5)

        self.text_area = scrolledtext.ScrolledText(self, width=100, height=20, bg="black", fg="white")
        self.text_area.grid(row=8, columnspan=3, padx=5, pady=5)

    def save_data(self):
        data = {
            "Data do Cadastro": self.entry_cadastro.get(),
            "Nome do Prestador": self.entry_nome.get(),
            "Endereço": self.entry_endereco.get(),
            "CEP": self.entry_cep.get(),
            "Celular app": self.entry_celular.get(),
            "E-mail": self.entry_email.get(),
            "Oficio": self.entry_oficio.get()
        }

        file_path = os.path.join("d:/adm/14", "dados_prestador.txt")
        with open(file_path, "w") as file:
            for key, value in data.items():
                file.write(f"{key}: {value}\n")
        messagebox.showinfo("Sucesso", "Dados salvos com sucesso!")

    def open_data(self):
        file_path = os.path.join("d:/adm/14", "dados_prestador.txt")
        if os.path.exists(file_path):
            with open(file_path, "r") as file:
                data = file.read()
            self.text_area.delete(1.0, tk.END)
            self.text_area.insert(tk.END, data)
        else:
            messagebox.showerror("Erro", "Arquivo não encontrado!")

    def restart(self):
        self.entry_cadastro.delete(0, tk.END)
        self.entry_nome.delete(0, tk.END)
        self.entry_endereco.delete(0, tk.END)
        self.entry_cep.delete(0, tk.END)
        self.entry_celular.delete(0, tk.END)
        self.entry_email.delete(0, tk.END)
        self.entry_oficio.delete(0, tk.END)
        self.text_area.delete(1.0, tk.END)




class AjudaApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Ajuda App")
        self.geometry("1400x1200")
        self.configure(bg="black")
        
        info_text = """
        Sabesp, salva Contratoss na cpu
        Enel, salva Contratoss na cpu
        Planilhas, gera planilhas em exel e exporta par cpu
        Vencimentos, lista todas as datas de vencimentos
        Busca, arquivos no programa e exporta 
        Configurar, configura senha e outros debug
        Gastos, cadastra Gastos
        Prestador, cadastra
        Criar Recibo, cria recibo de aluguel
        Imobilhiarias, cadastra
        Inadimplência, cadastra Dividas
        Notas fiscais, lista
        Administrador, cadastra
        Agenda, avisa eventos e agenda
        """
        
        label = tk.Label(self, text=info_text, bg="black", fg="white", justify="left", font=("Arial", 12))
        label.pack(fill="both", expand=True, padx=20, pady=20)




class AIApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Inteligência Artificial App")
        self.geometry("1400x1200")
        
        # Frame principal
        self.main_frame = tk.Frame(self)
        self.main_frame.pack(fill=tk.BOTH, expand=True)

        # Lista para armazenar dados dos arquivos TXT
        self.data = []

        # Barra de rolagem
        self.scrollbar = ttk.Scrollbar(self.main_frame)
        self.scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

        # Text widget para exibir dados
        self.text_widget = tk.Text(self.main_frame, wrap=tk.NONE, yscrollcommand=self.scrollbar.set)
        self.text_widget.pack(fill=tk.BOTH, expand=True)
        self.scrollbar.config(command=self.text_widget.yview)

        # Botão para analisar arquivos TXT
        self.analyze_button = tk.Button(self, text="Analisar Arquivos TXT", command=self.analyze_txt_files)
        self.analyze_button.pack()

        # Botão para exportar planilha para Excel
        self.export_button = tk.Button(self, text="Exportar para Excel", command=self.export_to_excel)
        self.export_button.pack()

    def analyze_txt_files(self):
        # Limpar dados anteriores
        self.data.clear()
        self.text_widget.delete(1.0, tk.END)

        # Diretório contendo arquivos TXT
        directory = "D:/adm"

        # Analisar todos os arquivos TXT dentro do diretório
        for filename in os.listdir(directory):
            if filename.endswith(".txt"):
                file_path = os.path.join(directory, filename)
                with open(file_path, "r") as file:
                    content = file.read()
                    # Adicionar dados à lista
                    self.data.append(content)
                    # Adicionar dados ao widget de texto
                    self.text_widget.insert(tk.END, content + "\n")

    def export_to_excel(self):
        if self.data:
            # Criar DataFrame com os dados
            df = pd.DataFrame({"Dados": self.data})
            # Solicitar local para salvar o arquivo Excel
            file_path = filedialog.asksaveasfilename(defaultextension=".xlsx")
            if file_path:
                # Exportar DataFrame para Excel
                df.to_excel(file_path, index=False)
                messagebox.showinfo("Exportar para Excel", "Planilha exportada com sucesso!")




class ExportarApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Exporta App")
        
        # Create 10 entry fields
        self.entry_fields = []
        for i in range(10):
            label = tk.Label(self, text=f"Campo {i+1}:")
            label.grid(row=i, column=0, padx=5, pady=5)
            entry = tk.Entry(self)
            entry.grid(row=i, column=1, padx=5, pady=5)
            self.entry_fields.append(entry)
        
        # Create buttons
        open_button = tk.Button(self, text="Abrir", command=self.open_file)
        open_button.grid(row=10, column=0, padx=5, pady=5)

        save_button = tk.Button(self, text="Salvar", command=self.save_file)
        save_button.grid(row=10, column=1, padx=5, pady=5)

        export_button = tk.Button(self, text="Exportar", command=self.export_data)
        export_button.grid(row=10, column=2, padx=5, pady=5)

    def open_file(self):
        file_path = filedialog.askopenfilename(filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
        if file_path:
            # Do something with the opened file
            print("File opened:", file_path)

    def save_file(self):
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
        if file_path:
            # Save data to the file
            print("File saved as:", file_path)

    def export_data(self):
        # Export data from entry fields
        for i, entry in enumerate(self.entry_fields):
            field_value = entry.get()
            print(f"Campo {i+1}: {field_value}")

#cadastra contatos cria

class ContatosApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Contatos App")
        
        # Create entry fields
        self.create_entry_fields()
        
        # Create buttons
        self.create_restart_button()
        self.create_save_button()
        self.create_search_button()
        self.create_open_file_button()
    
    def create_entry_fields(self):
        self.entry_fields = []
        for i in range(10):
            entry_field = tk.Entry(self)
            entry_field.pack()
            self.entry_fields.append(entry_field)
    
    def create_restart_button(self):
        restart_button = tk.Button(self, text="Recomeçar", command=self.restart)
        restart_button.pack()
    
    def create_save_button(self):
        save_button = tk.Button(self, text="Salvar TXT", command=self.save_txt)
        save_button.pack()
    
    def create_search_button(self):
        search_button = tk.Button(self, text="Procurar TXT", command=self.search_txt)
        search_button.pack()
    
    def create_open_file_button(self):
        open_file_button = tk.Button(self, text="Abrir Arquivo", command=self.open_file)
        open_file_button.pack()
    
    def restart(self):
        # Destroy current window and recreate the application
        self.destroy()
        ContatosApp().mainloop()
    
    def save_txt(self):
        # Get current working directory
        cwd = os.getcwd()
        # Set the destination directory
        dest_dir = os.path.join(cwd, "d:\\adm\\6")
        # Ensure destination directory exists
        os.makedirs(dest_dir, exist_ok=True)
        # Save the file
        with open(os.path.join(dest_dir, "contatos.txt"), "w") as f:
            for entry_field in self.entry_fields:
                f.write(entry_field.get() + "\n")
            f.write("\n")
    
    def search_txt(self):
        # Get the list of files in the directory
        directory = "d:\\adm\\6"
        if os.path.exists(directory):
            files = [f for f in os.listdir(directory) if f.endswith('.txt')]
            if files:
                print("TXT files found:")
                for file in files:
                    print(file)
            else:
                print("No TXT files found in", directory)
        else:
            print("Directory", directory, "does not exist")
    
    def open_file(self):
        file_path = filedialog.askopenfilename(initialdir="/", title="Select file",
                                               filetypes=(("Text files", "*.txt"), ("all files", "*.*")))
        if file_path:
            # Get current working directory
            cwd = os.getcwd()
            # Set the destination directory
            dest_dir = os.path.join(cwd, "d:\\adm\\6")
            # Ensure destination directory exists
            os.makedirs(dest_dir, exist_ok=True)
            # Copy the selected file to destination directory
            file_name = os.path.basename(file_path)
            dest_file_path = os.path.join(dest_dir, file_name)
            try:
                with open(file_path, 'rb') as src, open(dest_file_path, 'wb') as dest:
                    dest.write(src.read())
                print("File copied successfully to", dest_file_path)
            except Exception as e:
                print("Error:", e)

#fazer script thon  com widget com campos com 1 linha para cada Função 
#campos em formato de coluna
#Data Compra Imovel 
#Tipo de Imovel Numero
#Bairro
#Cidade 
#Estado 
#CEP em formato de cep
#CIB 
#Numero Registro do Imovel
#Numero Matricula do imovel
#Nùmero iptu  do imovel
#Valor do iptu 
#Vencimento do iptu abrir uma seleção para adicionar como opicçao de  meses 
#Valor Estimado do Imovel 
#Valor Venal 
#Valor IR 
#Valor Tributos 
#numero Rgi	
#Numero enel
#criar botões para salvar como txt na pasta d:\adm\14 com Nome da imobilhiaria
#criar botões para abrir arquivo txt em d:\adm\16 e listar em widget 
#criar botões para recomeçar
#criar botões para procurar na pasta d:\adm\16 listar em nova widget 1400x1000



class ImoveisApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Imóveis App")
        self.geometry("400x300")
        
        self.create_widgets()
        
    def create_widgets(self):
        fields = ["Data Compra Imóvel", "Tipo de Imóvel Número", "Bairro", "Cidade", 
                  "Estado", "CEP", "CIB", "Número Registro do Imóvel", 
                  "Número Matrícula do Imóvel", "Número IPTU do Imóvel", "Valor do IPTU",
                  "Vencimento do IPTU", "Valor Estimado do Imóvel", "Valor Venal", 
                  "Valor IR", "Valor Tributos", "Número RGI", "Número Enel"]
        
        self.entries = {}
        for i, field in enumerate(fields):
            label = tk.Label(self, text=field)
            label.grid(row=i, column=0, sticky="w")
            entry = tk.Entry(self)
            entry.grid(row=i, column=1)
            self.entries[field] = entry
        
        self.month_options = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
                              "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"]
        self.month_var = tk.StringVar()
        self.month_var.set("Selecione o Mês")
        self.month_menu = ttk.Combobox(self, textvariable=self.month_var, values=self.month_options)
        self.month_menu.grid(row=11, column=1)

        self.save_button = tk.Button(self, text="Salvar como txt", command=self.save_to_txt)
        self.save_button.grid(row=len(fields)+1, column=0)

        self.open_button = tk.Button(self, text="Abrir arquivo txt", command=self.open_txt_file)
        self.open_button.grid(row=len(fields)+1, column=1)

        self.restart_button = tk.Button(self, text="Recomeçar", command=self.restart)
        self.restart_button.grid(row=len(fields)+1, column=2)

        self.search_button = tk.Button(self, text="Procurar na pasta", command=self.search_in_folder)
        self.search_button.grid(row=len(fields)+1, column=3)

    def save_to_txt(self):
        filename = filedialog.asksaveasfilename(initialdir="D:/adm/14", defaultextension=".txt", filetypes=[("Text files", "*.txt")])
        if filename:
            with open(filename, "w") as f:
                for field, entry in self.entries.items():
                    f.write(f"{field}: {entry.get()}\n")
                f.write(f"Vencimento do IPTU: {self.month_var.get()}\n")

    def open_txt_file(self):
        filename = filedialog.askopenfilename(initialdir="D:/adm/16", filetypes=[("Text files", "*.txt")])
        if filename:
            with open(filename, "r") as f:
                content = f.read()
                self.show_in_widget(content)

    def show_in_widget(self, content):
        top = tk.Toplevel()
        text_widget = tk.Text(top)
        text_widget.insert(tk.END, content)
        text_widget.pack()

    def restart(self):
        for entry in self.entries.values():
            entry.delete(0, tk.END)
        self.month_var.set("Selecione o Mês")

    def search_in_folder(self):
        folder_path = filedialog.askdirectory(initialdir="D:/adm/16")
        if folder_path:
            files = [f for f in os.listdir(folder_path) if f.endswith(".txt")]
            self.show_in_widget("\n".join(files))





if __name__ == "__main__":
    app = App()
    app.mainloop()

