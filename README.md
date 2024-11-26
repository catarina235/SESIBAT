import pygame
import sys

# Inicialização do Pygame
pygame.init()

# Definição de cores
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# Configurações da tela
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Jogo de Luta")

# Carregar a imagem de fundo
background = pygame.image.load('mk')

# Carregar as imagens dos personagens
player1_image = pygame.image.load('1.png')
player2_image = pygame.image.load('2.png')

# Variáveis dos personagens
player1_health = 100
player2_health = 100
player1_x, player1_y = 50, 300
player2_x, player2_y = 700, 300
player_width, player_height = player1_image.get_width(), player1_image.get_height()
player_speed = 5

clock = pygame.time.Clock()

# Função para desenhar as barras de vida
def draw_health_bars():
    pygame.draw.rect(screen, RED, (50, 30, player1_health * 2, 20))
    pygame.draw.rect(screen, GREEN, (550, 30, player2_health * 2, 20))

# Função para desenhar os personagens
def draw_characters():
    screen.blit(player1_image, (player1_x, player1_y))
    screen.blit(player2_image, (player2_x, player2_y))

# Loop principal do jogo
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Movimento dos personagens
    keys = pygame.key.get_pressed()
    if keys[pygame.K_a]:
        player1_x -= player_speed
    if keys[pygame.K_d]:
        player1_x += player_speed
    if keys[pygame.K_w]:
        player1_y -= player_speed
    if keys[pygame.K_s]:
        player1_y += player_speed

    if keys[pygame.K_LEFT]:
        player2_x -= player_speed
    if keys[pygame.K_RIGHT]:
        player2_x += player_speed
    if keys[pygame.K_UP]:
        player2_y -= player_speed
    if keys[pygame.K_DOWN]:
        player2_y += player_speed

    # Diminuir a barra de vida do player 2 quando a tecla "K" for pressionada
    if keys[pygame.K_k]:
        player2_health -= 1
        if player2_health < 0:
            player2_health = 0
    # Diminuir a barra de vida do player 1 quando a tecla "L" for pressionada
    if keys[pygame.K_l]:
        player1_health -= 1
        if player1_health < 0:
            player1_health = 0

    # Limitando os movimentos dentro da tela
    player1_x = max(0, min(player1_x, WIDTH - player_width))
    player1_y = max(0, min(player1_y, HEIGHT - player_height))
    player2_x = max(0, min(player2_x, WIDTH - player_width))
    player2_y = max(0, min(player2_y, HEIGHT - player_height))

    # Desenhar a imagem de fundo
    screen.blit(background, (0, 0))

    # Desenhar os personagens e barras de vida
    draw_characters()
    draw_health_bars()

    # Atualizar a tela
    pygame.display.flip()

    # Controlar a taxa de atualização
    clock.tick(60)
