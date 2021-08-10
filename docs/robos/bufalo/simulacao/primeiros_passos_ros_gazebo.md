# Primeiros Passos com Búfalo, ROS e Gazebo

*O processo de documentação ainda é um trabalho em processo. No meio tempo, confira
o [principal repositório](https://github.com/DarwinRobotica/bufalo_ros) para os pacotes ROS.*

## Configurando um ambiente ROS

O primeiro passo para simular o Búfalo é instalar o ROS, de acordo com a [documentação](http://wiki.ros.org/noetic/Installation/).
Durante o processo de instalação, escolha a versão *Dekstop-Full Install*.

!!! info "Observe"

    Os pacotes incluídos no **bufalo_ros** foram desenvolvidos em cima da versão **Noetic** do ROS.


Uma vez instalado e configurado o ROS, rode o seguinte comando para confirmar a saída:

```bash
echo $ROS_DISTRO
```

a saída esperada é

```
noetic
```

Você também deve instalar as dependências para compilar e rodar os pacotes do Búfalo:

```bash
sudo apt install ros-noetic-controller-manager \
    ros-noetic-ros-control \
    ros-noetic-urdf \
    ros-noetic-xacro \
    ros-noetic-robot-localization \
    ros-noetic-diff-drive-controller \
    ros-noetic-teleop-twist-keyboard \
    python3-catkin-tools
```

Agora, você pode instalar os pacotes que compõe a simulação do Búfalo. No mesmo terminal, crie uma nova pasta e baixe os pacotes do
[GitHub](https://github.com/DarwinRobotica/bufalo_ros).

```bash
mkdir -p ~/catkin_ws/src && cd ~/catkin_ws
catkin init
git clone https://github.com/DarwinRobotica/bufalo_ros.git src/bufalo/
```

e compile os pacotes com

```
catkin build
```

## Rodando a sua primeira simulação

Com tudo instalado e configurado, você já pode utilizar os pacotes que vem com o **bufalo_ros**! Vamos começar subindo um ambiente
no Gazebo com o nosso rover Búfalo, e teleoperá-lo com o teclado. Primeiro, puxe os pacotes recém-compilados em uma sessão do Terminal com

```bash
source ~/catkin_ws/devel/setup.bash
```

e inicie a simulação lançando o arquivo **empty_world.launch** do pacote **bufalo_gazebo**

```bash
roslaunch bufalo_gazebo empty_world.launch
```

Este comando irá abrir duas novas janelas: Gazebo e Rviz.

Com o Gazebo, é possível simular um mundo com física realista para o nosso robô habitar. O programa realiza todos os cálculos
matemáticos por trás para garantir que a simulação respeite as leis da física.

![Gazebo showcase](/img/bufalo_gazebo_full.png)

Por outro lado, o Rviz é uma ferramente desenvolvida para nos permitir enxergar o que o robô enxerga. Já que o Búfalo é
equipado com um LiDAR e uma câmera RGB, nós conseguimos acompanhar o que o robô entende como seu mundo sensorial.

![Rviz showcase](/img/bufalo_rviz.png)

Em outras palavras, o Gazebo representa nosso mundo real, e o Rviz o mundo do robô.

Utilize as ferramentas do Gazebo para inserir um cubo na frente do robô, e confira no Rviz o *stream* da câmera e as medidas do LiDAR.

## Teleoperando o rover

Nosso robô existe num mundo físico e consegue sensoriá-lo. E agora? O próximo passo lógico é fazê-lo mover! Para isso,
usamos o pacote `teleop_twist_keyboard`, que cria uma interface do robô para o nosso teclado. Inicie um node teleoperado
com o seguinte comando:

```bash
rosrun teleop_twist_keyboard teleop_twist_keyboard.py cmd_vel:=/bufalo_velocity_controller/cmd_vel
```

After a couple of seconds a prompt will show up and ask you to press some keys in order to send velocity commands to the
rover. Simplifying, press

Depois de alguns segundos, uma interface irá aparecer oferecendo algumas direções de movimento, que mandarão comandos de
velocidade para o rover. Simplificando,

```
i : para mover para frente
, : para mover para trás
j : para rotacionar para a esquerda
l : para rotacionar para a direita
```

Agora você pode mover o rover! Enquanto o robô se move, observe no Rviz as medidas do LiDAR e as imagens provenientes da câmera.

## Mapas complexos

Uma outra *launch file* está disponível com um outro ambiente no Gazebo (o Willow Garage). Para inicializá-la, junto com uma
instância do Búfalo, rode o comando

```bash
roslaunch bufalo_gazebo willowgarage_world.launch
```

![Búfalo na WillowGarage](/img/bufalo_willowgarage.png)

Neste mapa, as medidas do LiDAR e as imagens da câmera são bem mais interessantes de se analisar. Tente teleoperar o Búfalo e se mover pelo mapa!
