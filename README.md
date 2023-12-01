- 👋 Hi, I’m @be2311
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
be2311/be2311 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import random

# Inicialização do Pygame
pygame.init()

# Configurações do jogo
largura, altura = 800, 600
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Jogo de Zumbi")

# Cores
branco = (255, 255, 255)
vermelho = (255, 0, 0)

# Jogador
player_size = 50
player_x = largura // 2 - player_size // 2
player_y = altura - 2 * player_size
player_speed = 5

# Zumbi
zombie_size = 50
zombie_speed = 3
zombie_list = []

# Função para criar zumbis aleatórios
def criar_zumbi():
    x = random.randint(0, largura - zombie_size)
    y = random.randint(0, altura - zombie_size)
    zombie_list.append([x, y])

# Loop principal do jogo
running = True
clock = pygame.time.Clock()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Movimento do jogador
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < largura - player_size:
        player_x += player_speed

    # Movimento dos zumbis
    for zombie in zombie_list:
        zombie[1] += zombie_speed
        if zombie[1] > altura:
            zombie_list.remove(zombie)
            criar_zumbi()

    # Verifica colisões com zumbis
    player_rect = pygame.Rect(player_x, player_y, player_size, player_size)
    for zombie in zombie_list:
        zombie_rect = pygame.Rect(zombie[0], zombie[1], zombie_size, zombie_size)
        if player_rect.colliderect(zombie_rect):
            print("Game Over!")
            running = False

    # Atualização da tela
    tela.fill(branco)
    pygame.draw.rect(tela, vermelho, [player_x, player_y, player_size, player_size])

    for zombie in zombie_list:
        pygame.draw.rect(tela, branco, [zombie[0], zombie[1], zombie_size, zombie_size])

    pygame.display.flip()

    # Controle de FPS
    clock.tick(30)

# Encerra o jogo
pygame.quit()
