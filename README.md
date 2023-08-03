import pygame
import random

pygame.init()

# Game settings
SCREEN_WIDTH, SCREEN_HEIGHT = 800, 600
GRID_SIZE = 20
GRID_WIDTH = SCREEN_WIDTH // GRID_SIZE
GRID_HEIGHT = SCREEN_HEIGHT // GRID_SIZE
FPS = 10

# Colors
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Directions
UP = (0, -1)
DOWN = (0, 1)
LEFT = (-1, 0)
RIGHT = (1, 0)

# Helper functions
def draw_grid(screen):
    for x in range(0, SCREEN_WIDTH, GRID_SIZE):
        pygame.draw.line(screen, WHITE, (x, 0), (x, SCREEN_HEIGHT))
    for y in range(0, SCREEN_HEIGHT, GRID_SIZE):
        pygame.draw.line(screen, WHITE, (0, y), (SCREEN_WIDTH, y))

def generate_random_position():
    return random.randint(0, GRID_WIDTH - 1) * GRID_SIZE, random.randint(0, GRID_HEIGHT - 1) * GRID_SIZE

def main():
    screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
    pygame.display.set_caption('Snake Game')
    clock = pygame.time.Clock()

    snake = [(GRID_WIDTH // 2 * GRID_SIZE, GRID_HEIGHT // 2 * GRID_SIZE)]
    direction = RIGHT
    food_position = generate_random_position()

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP and direction != DOWN:
                    direction = UP
                elif event.key == pygame.K_DOWN and direction != UP:
                    direction = DOWN
                elif event.key == pygame.K_LEFT and direction != RIGHT:
                    direction = LEFT
                elif event.key == pygame.K_RIGHT and direction != LEFT:
                    direction = RIGHT

        # Move the snake
        head_x, head_y = snake[-1]
        dx, dy = direction
        new_head = ((head_x + dx * GRID_SIZE) % SCREEN_WIDTH, (head_y + dy * GRID_SIZE) % SCREEN_HEIGHT)

        # Check for collision with food
        if new_head == food_position:
            snake.append(food_position)
            food_position = generate_random_position()
        else:
            snake.append(new_head)
            snake.pop(0)

        # Check for collision with itself
        if len(snake) != len(set(snake)):
            running = False

        # Draw the game
        screen.fill((0, 0, 0))
        draw_grid(screen)

        # Draw the snake
        for segment in snake:
            pygame.draw.rect(screen, GREEN, (segment[0], segment[1], GRID_SIZE, GRID_SIZE))

        # Draw the food
        pygame.draw.rect(screen, RED, (food_position[0], food_position[1], GRID_SIZE, GRID_SIZE))

        pygame.display.flip()
        clock.tick(FPS)

    pygame.quit()

if __name__ == "__main__":
    main()

