# Primeiro-Projeto
from pathlib import Path
import customtkinter as ctk
from tkinter import *
from tkinter import messagebox
import openpyxl, xlrd
import pathlib
from openpyxl import Workbook

WORKBOOK=Workbook()

ctk.set_appearance_mode("System")
ctk.set_default_color_theme("dark-blue")


class Cadastro(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.layout_conf()
        self.appearance()
        self.todo_sistema()

    def layout_conf(self):
        self.title("Sistema de Cadastro de Clientes")
        self.geometry("700x500")

    def appearance(self):
        self.lb_apm = ctk.CTkLabel(self, text="Tema", bg_color="transparent", text_color=["#000", "#fff"]).place(x=50,
                                                                                                                 y=430)
        self.opt_apm = ctk.CTkOptionMenu(self, values=["Light", "Dark", "System"], command=self.change_apm).place(x=50,
                                                                                                                  y=460)


    def change_apm(self, nova_aparencia):
        ctk.set_appearance_mode(nova_aparencia)



    def todo_sistema(self):
        global contact_value, gender_combobox, obs_entry, name_value, address_value, age_value
        frame = ctk.CTkFrame(self, width=700, height=50, corner_radius=0, bg_color="teal", fg_color="red").place(x=0,
                                                                                                                 y=10)
        title = ctk.CTkLabel(frame, text="Sistema de Gestão de Clientes", font=("Gill Sans", 24), text_color="#fff").place(x=190, y=20)

        Subtitle = ctk.CTkLabel(self, text="Preencha o formulário", font=("Gill Sans", 16),
                                text_color=["#000", "#fff"]).place(x=50, y=70)


        ficheiro = pathlib.Path("Clientes.xlsx")

        if ficheiro.exists():
            pass
        else:
            ficheiro=Workbook()
            folha=ficheiro.active
            folha['A1']="Nome Completo"
            folha['B1']="Contato"
            folha['C1']="Data de Nascimento"
            folha['D1']="Genero"
            folha['E1']="Endereco"
            folha['F1']="Observacoes"

            ficheiro.save("Clientes.xlsx")

        def submit():
            Name = name_value.get()
            Contato = contact_value.get()
            Age = age_value.get()
            Gender = gender_combobox.get()
            Address = address_value.get()
            Observacao = obs_entry.get(0.0, END)

            ficheiro = openpyxl.load_workbook('Clientes.xlsx')
            folha = ficheiro.active
            folha.cell(column=1, row=folha.max_row+1, value=name)
            folha.cell(column=2, row=folha.max_row, value=contact)
            folha.cell(column=3, row=folha.max_row, value=age)
            folha.cell(column=4, row=folha.max_row, value=gender)
            folha.cell(column=5, row=folha.max_row, value=address)
            folha.cell(column=6, row=folha.max_row, value=obs)

            ficheiro.save(r"Clientes.xlsx")
            messagebox.showinfo("Sistema", "Dados salvos com sucesso!")


        def clear():
            name_value.set("")
            contact_value.set("")
            age_value.set("")
            address_value.set("")
            obs_entry.delete(0.0, END)

        #textos variaveis
        name_value = StringVar()
        contact_value = StringVar()
        age_value = StringVar()
        address_value = StringVar()

        #Entrys
        name_entry = ctk.CTkEntry(self, width=350, textvariable=name_value, font=("Gill Sans", 16),fg_color="transparent")
        contact_entry = ctk.CTkEntry(self, width=200, textvariable=contact_value, font=("Gill Sans", 16), fg_color="transparent")
        age_entry = ctk.CTkEntry(self, width=150, textvariable=age_value, font=("Gill Sans", 16), fg_color="transparent")
        address_entry = ctk.CTkEntry(self, width=200, textvariable=address_value, font=("Gill Sans", 16), fg_color="transparent")

        #Combobox
        gender_combobox = ctk.CTkComboBox(self, values=["Masculino", "Feminino", "Prefiro não comentar"], font=("arial", 14), width=150)
        gender_combobox.set("Masculino")

        #obs
        obs_entry = ctk.CTkTextbox(self, width=470, height=150, font=("arial", 18), border_color="#aaa", border_width=2, fg_color="transparent")

        #Labels
        name = ctk.CTkLabel(self, text="Nome Completo", font=("Gill Sans", 16), text_color=["#000","#fff"])
        contact = ctk.CTkLabel(self, text="Contato", font=("Gill Sans", 16), text_color=["#000", "#fff"])
        age = ctk.CTkLabel(self, text="Data de Nascimento", font=("Gill Sans", 16), text_color=["#000", "#fff"])
        gander = ctk.CTkLabel(self, text="Genero", font=("Gill Sans", 16), text_color=["#000", "#fff"])
        address = ctk.CTkLabel(self, text="Endereço", font=("Gill Sans", 16), text_color=["#000", "#fff"])
        obs = ctk.CTkLabel(self, text="Observação", font=("Gill Sans", 16), text_color=["#000", "#fff"])

        btn_submit = ctk.CTkButton(self, text="Salvar".upper(), command=submit, fg_color="#151", hover_color="#131").\
            place(x=300, y=420)
        btn_clear = ctk.CTkButton(self, text="Limpar Dados".upper(), command=clear, fg_color="#555", hover_color="#131").\
            place(x=500, y=420)

        #posicionando os elementos
        name.place(x=50, y=120)
        name_entry.place(x=50, y=150)

        contact.place(x=450, y=120)
        contact_entry.place(x=450, y=150)

        age.place(x=300, y=190)
        age_entry.place(x=300, y=220)

        gander.place(x=500, y=190)
        gender_combobox.place(x=500, y=220)

        address.place(x=50, y=190)
        address_entry.place(x=50, y=220)

        obs.place(x=50, y=260)
        obs_entry.place(x=150, y=260)



if __name__ == "__main__":
    cadastro= Cadastro()
    cadastro.mainloop()
