
O projeto desta atividade se resume a um robô que fazia interações humanas em uma máquina real, mas trabalhando sozinho, precisando somente fazer o start (rodar/iniciar o código).  

O código principal e a página de onde eram extraídas as informações pertencem à empresa mantenedora. Não postei o projeto enquanto atuava nela, mas consigo exemplificar seu uso e funcionamento.  

Tínhamos links que já haviam sido tratados e seu funcionamento restabelecido.  
Quando o link de um equipamento parava de funcionar, abríamos um chamado de reparo para a concessionária responsável e, nesse momento, era feito o contingenciamento do link principal por um link móvel.  
Muitas vezes o link não era restabelecido no momento da conjunta, ou o técnico não estava com tempo hábil para retirar o modem de contingência, e essa tratativa ficava pendente.  
Ao estar pendente a retirada da contingência, tínhamos que testar diariamente o link principal para averiguar se ainda estava operante, evitando realizar uma atividade improdutiva caso o link principal voltasse a falhar. Assim, era testado e, caso tivesse parado novamente, seria aberto um novo chamado de reparo e cancelaríamos a retirada da contingência por inoperância do link.  

### Fluxo da automação:  

1. Pegávamos a lista de PCs com pendência de retirada de contingência.  
2. Fazíamos um `for` com uma iteração do `length` do tamanho da lista para ser executada.  
3. Já deixávamos a página do **ARS/Helix** logada com usuário e senha para ser acessada.  
4. Também deixávamos logada a ferramenta que realiza o teste de ping.  

### **Execução:**  

1. O robô pega o número da máquina (PC) do Excel e copia.  
2. Clica no **ARS/Helix**, cola o número do PC e pressiona **Enter** para fazer a pesquisa.  
3. Com os dados carregados na tela, dávamos um **"Ctrl+A"** para selecionar todo o conteúdo e um **"Ctrl+C"** para copiar.  
   - Neste momento, todos os dados iam para a área de transferência, então utilizávamos uma biblioteca do Python chamada **Pyperclip**, que trata os dados copiados na área de transferência do Windows.  
4. Através desses dados da área de transferência, fazíamos um **Regex** (expressões regulares), uma ferramenta poderosa para buscar, encontrar e manipular textos com base em padrões específicos. No Python, a biblioteca utilizada era a **re**.  
   - Filtrávamos os seguintes valores:  
     - **Tipo de concessionária (Fornecedor)**: Vivo / Embratel-Claro / Hughes / Telespazio - **Variável** (`fornecedor`)  
     - **Número do PC**: (Número da máquina para nossa identificação) - **Variável** (`numeroDoPc`)  
     - **IP da WAN do Router** - **Variável** (`ipWanRouter`)  
     - **IP da WAN da Concessionária** - **Variável** (`ipWanConcessionaria`)  
     - **Primeiros três dígitos do IP da WAN do Router**, pois, no caso da Vivo, tínhamos dois concentradores para acessar de acordo com seus prefixos - **Variável** (`tresPrimeiros`).  
   - Com o Regex, fazíamos vários **if** para definir as variáveis corretamente.  
5. Entrávamos no **Concentrador** de acordo com a **Concessionária**.  
6. Fazíamos ping nos **IPs da WAN do Router** (nosso roteador) e da **WAN da Concessionária** (roteador de borda da concessionária).  
7. No terminal do Windows, onde testávamos o ping, dávamos novamente **"Ctrl+A"** para selecionar todo o conteúdo da tela e **"Ctrl+C"** para copiar para a área de transferência.  
   - Analisávamos o resultado para verificar se o teste de ping do link foi bem-sucedido.  
   - Se **sim**, registrávamos **"Link operante"**.  
   - Se **não**, registrávamos **"Link inoperante"**.  
8. Salvávamos o resultado no **Bloco de Notas**, mas também poderia ser inserido diretamente na página, interagindo com os elementos. No entanto, foi decidido finalizar a ferramenta dessa forma.  

Temos fotos e um vídeo breve do funcionamento.  

---
