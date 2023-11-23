# "Virtualização" no Ryzentosh

Fala pessoal, gostaria de começar falando sobre o ryzentosh e porque ele não faz virtualização, para isso temos que entender como a propria apple faz a utilização de um recurso da intel chamado **Intel VT-x** com ele se torna possível fazer a virtualização nos sistemas da Apple. Como o próprio nome já fala **Ryzen**-tosh, é feito com processadores amd da linha ryzen, que por sua vez não tem o **Intel VT-x** e sim o **AMD-v** que não tem compatilibidade com o **MacOS**, assim tornando impossível a utilização do recurso de virtualização.

![Image hipervisor](https://docs-assets.developer.apple.com/published/f7344dbd5a/e960d911-a9fe-4678-9a10-45f268db1970.png)
Aqui está uma foto explicando o funcionamento. [Apple Developer](https://developer.apple.com/)

----------

## Virtualização x Emulação

Emuladores e máquinas virtuais são duas abordagens fundamentais para a execução de software em ambientes que podem diferir significativamente de suas configurações originais. Ambas as tecnologias proporcionam flexibilidade e adaptabilidade, mas suas abordagens fundamentais distinguem-nas em seus propósitos e usos.

- **Emuladores**:
Os emuladores são ferramentas poderosas que replicam integralmente as instruções de uma máquina real em uma camada de software. Essa camada de software atua como uma ponte, traduzindo as instruções da arquitetura emulada para o conjunto de instruções do sistema hospedeiro. Essa abstração completa permite que programas desenvolvidos para uma arquitetura específica sejam executados em ambientes completamente distintos. Os emuladores são frequentemente utilizados para possibilitar a execução de software legado ou de sistemas operacionais específicos em hardware moderno.

- **Máquinas Virtuais**:
Máquinas virtuais, por outro lado, implementam uma abordagem mais flexível. Elas utilizam software, muitas vezes chamado de Monitor de Máquina Virtual (MMV) ou Hypervisor, para criar ambientes isolados conhecidos como máquinas virtuais. Ao contrário dos emuladores, as máquinas virtuais não abstraem completamente todas as propriedades do hardware hospedeiro. O MMV gerencia as instruções provenientes dos sistemas convidados, encaminhando algumas delas diretamente para o processador real. Esse método permite uma execução mais eficiente e um melhor compartilhamento de recursos entre as máquinas virtuais e o sistema hospedeiro.

- **Diferenças e Aplicações**:
A diferença fundamental reside na extensão da abstração do hardware. Enquanto os emuladores recriam completamente o ambiente de hardware, as máquinas virtuais compartilham recursos com o sistema hospedeiro, permitindo uma coexistência mais eficiente de sistemas operacionais e aplicativos. Emuladores são ideais para cenários nos quais a replicação exata de uma máquina específica é necessária, enquanto máquinas virtuais são escolhas pragmáticas para ambientes onde a flexibilidade e a eficiência são cruciais.

----------

## QEMU

![QEMU](https://www.gta.ufrj.br/ensino/eel879/trabalhos_vf_2017_2/kvm/imagens/Diagrama.jpg)
O QEMU atua como uma ponte entre o hardware real e as máquinas virtuais, utilizando uma abordagem baseada em processos e threads para representar as diferentes instâncias de máquinas virtuais e suas CPUs virtuais. Essa integração eficiente no ambiente Linux contribui para a confiabilidade e eficácia da virtualização proporcionada pelo mesmo.

Cada máquina virtual no QEMU é associada a um processo QEMU correspondente no sistema hospedeiro, e para cada "CPU" virtual atribuída a uma máquina virtual, existe uma thread dentro desse processo QEMU. Isso significa que o QEMU opera em um modelo em que a granularidade da virtualização é estabelecida em nível de thread, permitindo uma gestão eficiente de recursos.

Do ponto de vista do kernel do Linux, as threads nos processos QEMU são tratadas como processos de usuário convencionais. Esse design proporciona uma implementação eficaz da virtualização, uma vez que as threads de QEMU podem ser gerenciadas e escalonadas de maneira semelhante a outros processos no sistema operacional.

----------

### Instalação

Devemos ter o Homebrew instalado ou o MacPorts:

Para instalar com o **Homebrew**

```sh
brew install qemu
```

Para instalar com o **MacPorts**

```sh
sudo port install qemu
```

Baixe a ISO desejada, no meu caso eu baixei o ubuntu server [download](https://releases.ubuntu.com/22.04.3/ubuntu-22.04.3-live-server-amd64.iso).

OBS: Caso tenha familiaridade com o QEMU fique a vontade para fazer por ```command line``` ou por essa dica de ouro.

**Instalar o GUI para o QEMU, o [UTM](https://mac.getutm.app/), ele funciona apenas para ajudar no gerenciamento das VMs.**

Para utilizar a interface é muito simples:

- **Criação da máquina**:
	- Vá em adicionar uma nova máquina
	- Selecione Emulation(Emulação)
	- Selecione Linux ou Windows
  - Coloque a ISO do seu gosto ಠ_ಠ
  - Faça toda a configuração de memória ram, CPU e de disco
  - Após finalizar, ainda não "starte" a máquina

- ***Essa parte é opcional***. Caso queira saber mais sobre o [Network](https://docs.getutm.app/settings-qemu/devices/network/network/) do QEMU pelo UTM
  - Click com o mouse direito em cima da VM e vá em edit
  - Network e selecione como bridge para fazer utilização da layer2 da rede

Após isso é so rodar a VM e ser feliz !!!

Caso queira rodar docker, aqui está o [link](https://docs.docker.com/engine/), basta seguir os passos.