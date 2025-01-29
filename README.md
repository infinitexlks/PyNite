# PyNite
Fortnite made out of Python!
# How to run
1. Install Flask with this command:
```bash
pip install pygame
```
2. Make a file called PyNite.py
```py
# PyNite.py
import pygame
import random

pygame.init()

# Screen dimensions
screen_width = 800
screen_height = 600

# Colors
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)

# Player settings
player_size = 50
player_pos = [screen_width // 2, screen_height - 2 * player_size]

# Enemy settings
enemy_size = 50
enemy_pos = [random.randint(0, screen_width - enemy_size), 0]
enemy_list = [enemy_pos]

# Speed
speed = 10

# Screen setup
screen = pygame.display.set_mode((screen_width, screen_height))
game_over = False
clock = pygame.time.Clock()

# Function to detect collisions
def detect_collision(player_pos, enemy_pos):
    p_x, p_y = player_pos
    e_x, e_y = enemy_pos

    if (e_x >= p_x and e_x < (p_x + player_size)) or (p_x >= e_x and p_x < (e_x + enemy_size)):
        if (e_y >= p_y and e_y < (p_y + player_size)) or (p_y >= e_y and p_y < (e_y + enemy_size)):
            return True
    return False

# Game loop
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    keys = pygame.key.get_pressed()

    if keys[pygame.K_LEFT] and player_pos[0] > 0:
        player_pos[0] -= speed
    if keys[pygame.K_RIGHT] and player_pos[0] < screen_width - player_size:
        player_pos[0] += speed

    screen.fill(black)

    drop_enemies = random.randint(1, 10)
    if drop_enemies == 1:
        enemy_list.append([random.randint(0, screen_width - enemy_size), 0])

    for enemy_pos in enemy_list:
        enemy_pos[1] += speed
        if enemy_pos[1] > screen_height:
            enemy_list.remove(enemy_pos)

    for enemy_pos in enemy_list:
        if detect_collision(player_pos, enemy_pos):
            game_over = True
            break

    for enemy_pos in enemy_list:
        pygame.draw.rect(screen, red, (enemy_pos[0], enemy_pos[1], enemy_size, enemy_size))

    pygame.draw.rect(screen, white, (player_pos[0], player_pos[1], player_size, player_size))

    clock.tick(30)
    pygame.display.update()

pygame.quit()
```
