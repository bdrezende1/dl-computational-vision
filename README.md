# dl-computational-vision
Exemplo de algoritmo de binarização, realizando a implementação em Python para transformar uma imagem colorida para níveis de cinza (0 a 255) e para binarizada (0 e 255), preto e branco.
---

Vamos detalhar cada etapa do código para que fique claro o que está acontecendo:

---

### **Importação das bibliotecas**
```python
import cv2
import numpy as np
from google.colab.patches import cv2_imshow
```
1. **cv2**: É o OpenCV, usado para manipulação e processamento de imagens.
2. **numpy (np)**: Manipula matrizes e operações matemáticas, como empilhar imagens.
3. **cv2_imshow**: Função específica do Google Colab para exibir imagens, já que `cv2.imshow` não funciona diretamente nesse ambiente.

---

### **Leitura da imagem**
```python
img = cv2.imread('img1.png', 0)
```
1. O método `cv2.imread` carrega uma imagem:
   - O primeiro argumento é o nome do arquivo (`'img1.png'`).
   - O segundo argumento (`0`) indica que a imagem será carregada em escala de cinza.
2. A variável `img` contém agora a matriz da imagem, onde cada elemento representa a intensidade de cinza de um pixel (valores entre 0 e 255).

---

### **Suavização da imagem**
```python
suave = cv2.GaussianBlur(img, (5, 5), 0)
```
1. **GaussianBlur** aplica um filtro Gaussiano para suavizar a imagem, reduzindo ruídos.
   - `(5, 5)`: Tamanho do kernel (janela de pixels que será suavizada).
   - `0`: Desvio padrão calculado automaticamente.
2. O resultado (`suave`) é uma versão da imagem com transições mais suaves entre os tons de cinza.

---

### **Binarização da imagem**
```python
(T, bin) = cv2.threshold(suave, 160, 255, cv2.THRESH_BINARY)
(T, binI) = cv2.threshold(suave, 160, 255, cv2.THRESH_BINARY_INV)
```
1. **cv2.threshold** realiza a binarização, separando os pixels em duas categorias (preto ou branco).
   - `suave`: Imagem suavizada.
   - `160`: Limiar. Pixels com valores **maiores ou iguais a 160** serão brancos; os demais, pretos.
   - `255`: Valor máximo para os pixels brancos.
   - `cv2.THRESH_BINARY`: Cria uma imagem binária normal.
   - `cv2.THRESH_BINARY_INV`: Cria uma imagem binária invertida (os brancos se tornam pretos e vice-versa).

O método retorna dois valores:
- `T`: O limiar utilizado (160).
- `bin` e `binI`: As imagens binárias resultantes.

---

### **Combinação das imagens**
```python
resultado = np.vstack([
    np.hstack([suave, bin]),
    np.hstack([binI, cv2.bitwise_and(img, img, mask=binI)])
])
```
1. **np.hstack**: Combina imagens horizontalmente.
   - Primeira linha: A imagem suavizada (`suave`) e a binária normal (`bin`).
   - Segunda linha: A binária invertida (`binI`) e uma imagem mascarada.
2. **cv2.bitwise_and**: Combina a imagem original (`img`) com ela mesma, usando `binI` como máscara.
   - Pixels pretos na máscara (`binI`) permanecem pretos na imagem resultante.

**np.vstack**: Combina as duas linhas para formar uma única imagem.

---

### **Exibição do resultado**
```python
cv2_imshow(resultado)
cv2.waitKey(0)
```
1. **cv2_imshow** exibe a imagem composta (`resultado`) no ambiente Colab.
2. **cv2.waitKey(0)** mantém a janela aberta até que uma tecla seja pressionada.

---

### **Resumo visual do processo**
- **Imagem original em cinza**: Carregada para processamento.
- **Imagem suavizada**: Reduz ruídos.
- **Imagem binária**: Separação em preto e branco com base no limiar.
- **Imagem mascarada**: Combinação da imagem original com uma máscara.

---
