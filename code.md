import pygame
import random

# Initialize the game
pygame.init()

# Set the game screen size
screen_width = 500
screen_height = 400
screen = pygame.display.set_mode((screen_width, screen_height))

# Set the game title
pygame.display.set_caption("Snake Game")

# Set the clock for controlling game speed
clock = pygame.time.Clock()

# Set the block size and font size
block_size = 10
font_size = 20
font = pygame.font.Font(None, font_size)

# Define the colors
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)

# Define the initial position of the snake
snake_x = 300
snake_y = 300
snake_list = [(snake_x, snake_y)]

# Define the initial position of the food
food_x = random.randint(0, screen_width // block_size - 1) * block_size
food_y = random.randint(0, screen_height // block_size - 1) * block_size

# Define the initial direction of the snake
dx = 0
dy = -block_size

# Initialize the score
score = 0

# Initialize the game loop
running = True
desired_speed = 8  # Adjust this value for the desired speed
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_UP] or keys[pygame.K_w]:
        dx = 0
        dy = -block_size
    if keys[pygame.K_DOWN] or keys[pygame.K_s]:
        dx = 0
        dy = block_size
    if keys[pygame.K_LEFT] or keys[pygame.K_a]:
        dx = -block_size
        dy = 0
    if keys[pygame.K_RIGHT] or keys[pygame.K_d]:
        dx = block_size
        dy = 0

    snake_x += dx
    snake_y += dy

    if snake_x >= screen_width:
        snake_x = 0
    elif snake_x < 0:
        snake_x = screen_width - block_size
    if snake_y >= screen_height:
        snake_y = 0
    elif snake_y < 0:
        snake_y = screen_height - block_size

    snake_list.insert(0, (snake_x, snake_y))

    if snake_x == food_x and snake_y == food_y:
        food_x = random.randint(0, screen_width // block_size - 1) * block_size
        food_y = random.randint(0, screen_height // block_size - 1) * block_size
        score += 1
    else:
        snake_list.pop()

    if (
        snake_x >= screen_width or snake_x < 0 or
        snake_y >= screen_height or snake_y < 0
    ):
        running = False

    for block in snake_list[1:]:
        if snake_x == block[0] and snake_y == block[1]:
            running = False

    screen.fill(black)
    for block in snake_list:
        pygame.draw.rect(screen, white, (block[0], block[1], block_size, block_size))
    pygame.draw.rect(screen, red, (food_x, food_y, block_size, block_size))

    score_text = font.render(f"Score: {score}", True, white)
    screen.blit(score_text, (10, 10))

    pygame.display.update()

    clock.tick(desired_speed)

pygame.quit()
