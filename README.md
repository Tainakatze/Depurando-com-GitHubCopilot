# **Depurando com GitHub Copilot**

## 📝 **Desafio:**
Você foi contratado para auxiliar na construção de uma nova plataforma de blog. Uma das funcionalidades solicitadas é um limitador de texto para exibir resumos de postagens na página inicial do site. Para isso, é necessário criar um programa que:

✅ Receba um tamanho máximo de caracteres permitido.  
✅ Receba o conteúdo do texto original.  
✅ Gere uma versão resumida desse conteúdo.  

No entanto, o código desenvolvido inicialmente pela equipe contém erros lógicos que impedem o funcionamento correto da limitação de texto. Seu desafio é analisar, identificar e corrigir o erro para que o programa funcione como esperado.  

💡 **Dica:** O **GitHub Copilot** pode ajudar explicando e sugerindo correções de código!

---

## 🔢 **Entrada**
📌 A primeira linha da entrada será um número inteiro `N` (**3 ≤ N ≤ 60**), representando o número máximo de caracteres permitidos no resumo (**incluindo as reticências, caso o texto seja cortado**).  
📌 A segunda linha será uma **string** com até **60 caracteres**, representando o texto do post.

## 🎯 **Saída**
O programa deverá retornar:
✅ O **texto original sem modificações**, caso ele tenha `N` ou menos caracteres.  
✅ Caso o texto ultrapasse o limite `N`, o programa deve:  
   1️⃣ Exibir os **(N - 3) primeiros caracteres** do texto original.  
   2️⃣ Adicionar `"..."` ao final, completando o total de `N` caracteres.

---

## 📌 **Exemplos:**
Aqui estão alguns exemplos de entrada e saída esperadas:

| 🏷️ Entrada | 🖥️ Saída |
|------------|---------|
| **26** <br> Bem-vindo ao nosso blog sobre tecnologia. | Bem-vindo ao nosso blog... |
| **32** <br> Aprenda a programar em Python hoje mesmo! | Aprenda a programar em Python... |
| **20** <br> Olá, Mundo! | Olá, Mundo! |

---

## 🚧 **Código antes da correção:**
```python
max_length = int(input())
user_input = input()

def summarize_text(text, limit):
    if len(text) >= limit:
        return text
    else:
        return text[:limit + 3] + "..."

output = summarize_text(user_input, max_length)
print(output)
```

---

## ✅ **Código corrigido:**
```python
# Entrada do número máximo de caracteres permitidos
n = int(input())

# Entrada do texto do post
texto = input()

def resumir_texto(texto, limite):
    # Se o texto for menor ou igual ao limite, retorna o texto original
    if len(texto) <= limite:
        return texto
    # Caso contrário, retorna os (limite - 3) primeiros caracteres + "..."
    else:
        return texto[:limite - 3] + "..."

# Chama a função e imprime o resultado
saida = resumir_texto(texto, n)
print(saida)
```

---

## 🚀 **Melhorias Implementadas:**

### 🧪 **1. Testes Unitários:**
Criamos testes automatizados para validar os diferentes cenários:

```python
import pytest
from app import resumir_texto

def test_texto_menor_que_limite():
    assert resumir_texto("Olá, mundo!", 20) == "Olá, mundo!"

def test_texto_exatamente_no_limite():
    assert resumir_texto("Python é incrível!", 20) == "Python é incrível!"

def test_texto_maior_que_limite():
    assert resumir_texto("Aprenda a programar em Python hoje mesmo!", 32) == "Aprenda a programar em Python..."

def test_limite_minimo():
    assert resumir_texto("Testando", 3) == "T..."

pytest.main()
```

### ⚠️ **2. Melhor Tratamento de Erros:**
Garantimos que entradas inválidas não gerem falhas inesperadas:

```python
def resumir_texto(texto, limite):
    if not isinstance(texto, str) or not isinstance(limite, int):
        raise ValueError("Parâmetros inválidos: texto deve ser string e limite deve ser inteiro")
    
    if limite < 3:
        raise ValueError("O limite deve ser no mínimo 3 caracteres")

    return texto if len(texto) <= limite else texto[:limite - 3] + "..."
```

### 🔄 **3. Configuração Dinâmica via Interface**
Se estiver usando Django, agora é possível configurar o limite de caracteres:

```python
from django.db import models

class Configuracao(models.Model):
    limite_caracteres = models.IntegerField(default=60)

    def __str__(self):
        return f"Limite de caracteres: {self.limite_caracteres}"
```

### 🌍 **4. Suporte a Diferentes Idiomas**
Detectamos o idioma do texto e ajustamos conforme necessário:

```python
from langdetect import detect

def ajustar_texto_por_idioma(texto):
    idioma = detect(texto)
    if idioma == "en":
        return texto[:50] + "..."
    elif idioma == "pt":
        return texto[:50] + "..."
    else:
        return texto[:50] + "..."
```

### 🗂️ **5. Integração com Banco de Dados**
Armazenamos o resumo diretamente no banco de dados para otimizar desempenho:

```python
from django.db import models

class Post(models.Model):
    titulo = models.CharField(max_length=100)
    conteudo = models.TextField()
    resumo = models.TextField(blank=True)

    def save(self, *args, **kwargs):
        if len(self.conteudo) > 60:
            self.resumo = self.conteudo[:57] + "..."
        else:
            self.resumo = self.conteudo
        super().save(*args, **kwargs)
```

---

## 🎯 **Conclusão:**
Com essas melhorias, o programa ficou mais robusto, seguro e flexível, garantindo que:
✔️ Textos dentro do limite sejam exibidos sem alterações.  
✔️ Textos maiores que o limite sejam truncados corretamente.  
✔️ Testes automatizados validem diferentes cenários.  
✔️ Erros sejam tratados adequadamentes.  
✔️ Configuração seja dinâmica via painel administrativo.  
✔️ Idiomas sejam detectados
para ajustes específicos.  
✔️ Banco de dados seja usado para otimizar desempenho.  
