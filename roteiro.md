# Instalação do Arch Linux

**Introdução, quem sou eu, Por que usar Linux e ainda por cima o Arch Linux, Comunidade**

## Preparando a base do sistema

- [ ] Configurar Layout do Teclado

 * *loadkeys br-abnt2*
 * *loadkeys us-acentos*

- [ ] Conectar a internet

 * Novo modo em breve

Para conectar a uma rede wifi

- [ ] Particionamento
 
 * *fdisk ou cfdisk*

Particionamento teste com 2 partições

| Unidade   | Ponto de montagem | Tamanho      |
| ----------|:-----------------:|:------------:|
| /dev/sda1 | /boot             | 256 ou 512 Mb|
| /dev/sda2 | /                 | restante da unidade |

### Por que particionar assim ?

Nesse ambiente virtual vamos estar criando apenas essas 2 partições para nosso estudo.
Para um ambiente em produção vocẽ pode além das 2 partições criar também uma partição UEFI que pode ser a partição criada logo depois da partição boot ou mesmo antes.

- [ ] Formatação

 * *mkfs.ext4 /dev/sda1*
 * *mkfs.ext4 /dev/sda2*

 **No nosso ambiente virtualizado estamos lidando apenas com 2 partições, mas como já avisado anteriormente podemos criar em um hipotético ambiente de produção uma partição UEFI e vamos formatar ela com *mkfs.fat -F32 /dev/sdax***

- [ ] Vamos criar e montar os pontos de montagem nos lugares certos

 * *mount /dev/sda2 /mnt*
 Montar o root do nosso novo sistema.

 * *mkdir /mnt/boot*
 Criar o ponto de montagem

 * *mount /dev/sda1 /mnt/boot*
 vamos montar a unidade **/dev/sda1** no ponto de montagem **/mnt/boot**

 **Para montagem da unidade UEFI teremos que criar um ponto de montagem dentro de** */mnt/boot/EFI* **e montar ela como já fizemos antes assim:** *mount /dev/sdax /mnt/boot/EFI*

- [ ] Vamos instalar a base do nosso sistema novo

Vamos fazer uma rápida configuração para melhorar a performance da velocidade da nossa instalação fazendo uma pequena edição do arquivo mirrorlist localizado em */etc/pacman.d/mirrorlist*. **Utilize o editor de texto de sua preferência** 

 * *pacstrap /mnt base base-devel linux linux-firmware vim bash-completion*

 Logo após a instalação dos pacotes vamos encerrar essa etapa criando o nosso arquivo **fstab**.

 * *genfstab /mnt >> /mnt/etc/fstab*


 ## Acessando o novo sistema

 - [ ] Conferir FSTAB e acessar CHROOT para iniciar a configuração do novo sistema.

 * *cat /mnt >> /mnt/etc/fstab*
 * *arch-chroot /mnt*

 Estamos agora no que vai ser a base do seu sistema, não estamos mais lidando indiretamente com o sistema em live e sim com o sistema que se tornara o seu queridão em poucos minutos. 
 **IMPORTANTE: apartir de agora cada comando dado será tão importante quanto os comandos dados em live, só que agora estamos configurando o produto final da sua instalação, tenha muito cuidado e atenção para seguir cada passo sugerido aqui nesse manual, lembre-se que apartir de agora tudo que for feito é para o melhor funcionamento ou não do seu sistema.**

 - [ ] Configurar Idioma pt-BR e teclado.

 * *vim /etc/locale.gen*

 Localize a linha **pt_BR.UTF-8** e retire o **#** (descomentando a linha).

 * *locale-gen*

 Aguarde terminar o processo.

 * *echo LANG=pt_BR.UTF-8 >> /etc/locale.conf*

 Agora o Idioma do sistema estará setado para português do Brasil
 Agora vamos configurar o Teclado.
 
 * *echo KEYMAP=br-abnt2 >> /etc/vconsole.conf*
 
 Pronto isso definirá o seu teclado como o padrão ABNT2 com a presença do "Ç", caso seu teclado seja do padrão americano e vc queira continuar tendo os acentos funcionando normalmente substitua o **br-abnt2** por **us-acentos**.

 ## Preparando root e o boot.

 - [ ] Preparar o ambiente no novo sistema.

 * *mkinicpio -p linux*

 Esse comando ira criar um ambiente minimo de ramdisk, será levantando modulos do kernel para que o sistema inicialize de forma correnta.

 - [ ] Instalar e configurar GRUB

 * *grub-install /dev/sda*

 Comando para instalar o grub no disco em qual estamos instalando o novo sistema Operacional.

 * *grub-mkconfig -o /boot/grub/grub.cfg*

 comando para gerar o grub cfgen

 ## Criar novo usuário e senha para o Root

 - [ ] Criar senha do root

 * *passwd*

 Nesse momento após o acionamento desse comando vamos ser solicitados que inserimos uma nova senha para o ROOT do sistema. Adicione a senha 2 vezes para que possa ser criada e confirmada a senha.

 - [ ] Vamos criar o novo usuário do sistema

 * *useradd -m -g users -G wheel -s /bin/bash novousuario*

 Nesse instante estaremos criando o novo usuário do sistema, com esse usuário você vai usar o sistema, logar e ter todas as permissões necessárias utilizar o sistema.

 * *passwd novousuario*

 vamos criar a senha para o novo usuário da mesma forma q fizemos com o root.

 ## Pronto!

 Até o presente momento temos uma boa parte do nosso sistema operacional (Arch Linux) instalado.
 Até então já temos o suficiente para iniciarmos o nosso sistema em modo de texto, só que não é o que precisamos, precisamos do sistema pronto para usar. Apartir de agora vamos caminhar de 2 maneiras diferentes, pois vamos estar configurando o KDE e o GNOME, então apartir de agora fique atento ao processo que você vai estar seguindo pois estaremos tendo 2 resultados diferenciados e estarei com você até o final dessa instalação e até você poder estar **#usandolinux** de verdade!

 ### Vamos lá!

 ## KDE Desktop

 - [ ] Instalação do ambiente de area de trabalho **KDE** 

 * *pacman -S xorg-server xorg-xinit xf86-video-vmware konsole dolphin dolphin-plugins plasma ark spectacle okular kio kio-extras unrar zip unzip firefox kate p7zip ntfs-3g exfat-utils sudo*

 Pronto apartir do comando acima nós termoes o básico para iniciar e utilizar o ambiente gráfico KDE, uma rápida explicação será dada sobre tudo o q esta sendo instalado.

 Logo após a isntalação vamos informar ao sistema o que ele deve subir ao inicializarmos pela primeira vez o computador.

 * *systemctl enable NetworkManager*
 * *systemctl enable sddm*

 Pronto feito isso podemos sair do ambiente do CHROOT (sim ainda estamos na live do arch linux até agora) desmontar as unidades montadas e reiniciar o sistema! 

 Se tudo correu bem até agora, o SDDM vai iniciarlizar mostrando seu usário e vamos dar o boot pela primeira vez no nosso novo sistema! 

 Vamos começar a customizar nosso KDE. 

 
