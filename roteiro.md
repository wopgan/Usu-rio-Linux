# Instalação do Arch Linux

## Preparando a base do sistema

- [ ] Configurar Layout do Teclado

 * *loadkeys br-abnt2*
 * *loadkeys us-acentos*

- [ ] Conectar a internet

 * *wifi-menu*

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