import pygame
import random

# Initialize pygame
pygame.init()

# Set up display
width, height = 640, 480
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")

# Colors
black = (0, 0, 0)
white = (255, 255, 255)
green = (0, 255, 0)
red = (255, 0, 0)

# Snake and food properties
snake_pos = [100, 50]
snake_body = [[100, 50], [90, 50], [80, 50]]
snake_direction = 'RIGHT'
change_to = snake_direction

food_pos = [random.randrange(1, (width // 10)) * 10,
            random.randrange(1, (height // 10)) * 10]
food_spawn = True

# FPS controller
fps_controller = pygame.time.Clock()

# Game Over
def game_over():
    my_font = pygame.font.SysFont('times new roman', 50)
    game_over_surface = my_font.render('Your Score is: ' + str(score), True, red)
    game_over_rect = game_over_surface.get_rect()
    game_over_rect.midtop = (width/2, height/4)
    screen.fill(black)
    screen.blit(game_over_surface, game_over_rect)
    pygame.display.flip()
    pygame.quit()
    quit()

# Main Function
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()
        # Whenever a key is pressed down
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                change_to = 'UP'
            if event.key == pygame.K_DOWN:
                change_to = 'DOWN'
            if event.key == pygame.K_LEFT:
                change_to = 'LEFT'
            if event.key == pygame.K_RIGHT:
                change_to = 'RIGHT'

    # Validation of direction
    if change_to == 'UP' and not snake_direction == 'DOWN':
        snake_direction = 'UP'
    if change_to == 'DOWN' and not snake_direction == 'UP':
        snake_direction = 'DOWN'
    if change_to == 'LEFT' and not snake_direction == 'RIGHT':
        snake_direction = 'LEFT'
    if change_to == 'RIGHT' and not snake_direction == 'LEFT':
        snake_direction = 'RIGHT'

    # Moving the snake
    if snake_direction == 'UP':
        snake_pos[1] -= 10
    if snake_direction == 'DOWN':
        snake_pos[1] += 10
    if snake_direction == 'LEFT':
        snake_pos[0] -= 10
    if snake_direction == 'RIGHT':
        snake_pos[0] += 10

    # Snake body growing mechanism
    snake_body.insert(0, list(snake_pos))
    if snake_pos[0] == food_pos[0] and snake_pos[1] == food_pos[1]:
        score += 1
        food_spawn = False
    else:
        snake_body.pop()

    if not food_spawn:
        food_pos = [random.randrange(1, (width // 10)) * 10,
                    random.randrange(1, (height // 10)) * 10]
    food_spawn = True

    # Background
    screen.fill(black)
    for pos in snake_body:
        pygame.draw.rect(screen, green, pygame.Rect(
            pos[0], pos[1], 10, 10))

    pygame.draw.rect(screen, white, pygame.Rect(
        food_pos[0], food_pos[1], 10, 10))

    # Game Over conditions
    if snake_pos[0] < 0 or snake_pos[0] > width - 10:
        game_over()
    if snake_pos[1] < 0 or snake_pos[1] > height - 10:
        game_over()

    # Touching the snake body
    for block in snake_body[1:]:
        if snake_pos[0] == block[0] and snake_pos[1] == block[1]:
            game_over()

    pygame.display.update()
    fps_controller.tick(15)
