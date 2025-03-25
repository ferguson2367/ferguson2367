import pygame
import random

Initialize Pygame
pygame.init()

Constants
WIDTH, HEIGHT = 800, 600
FPS = 60
PLAYER_SPEED = 5
BULLET_SPEED = 10
ENEMY_SPEED = 2

Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

Set up the screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("First Person Shooter Game")

Player class
class Player:1
    def __init__(self):
        self.rect = pygame.Rect(WIDTH // 2, HEIGHT // 2, 50, 50)

    def move(self, dx, dy):
        self.rect.x += dx
        self.rect.y += dy
        self.rect.clamp_ip(screen.get_rect())

Bullet class
class Bullet:
    def __init__(self, x, y):
        self.rect = pygame.Rect(x, y, 5, 5)

    def move(self):
        self.rect.y -= BULLET_SPEED

Enemy class
class Enemy:
    def __init__(self):
        self.rect = pygame.Rect(random.randint(0, WIDTH - 50), 0, 50, 50)

    def move(self):
        self.rect.y += ENEMY_SPEED

Main game loop
def main():
    clock = pygame.time.Clock()
    player = Player()
    bullets = []
    enemies = []
    score = 0

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            player.move(-PLAYER_SPEED, 0)
        if keys[pygame.K_RIGHT]:
            player.move(PLAYER_SPEED, 0)
        if keys[pygame.K_SPACE]:
            bullets.append(Bullet(player.rect.centerx, player.rect.top))

        # Move bullets and enemies
        for bullet in bullets:
            bullet.move()
        bullets = [bullet for bullet in bullets if bullet.rect.y > 0]

        if random.randint(1, 40) == 1:
            enemies.append(Enemy())

        for enemy in enemies:
            enemy.move()
            if enemy.rect.colliderect(player.rect):
                running = False  # Player hit by an enemy
            if enemy.rect.y > HEIGHT:
                score += 1
                enemies.remove(enemy)

        # Draw everything
        screen.fill(BLACK)
        pygame.draw.rect(screen, WHITE, player.rect)
        for bullet in bullets:
            pygame.draw.rect(screen, WHITE, bullet.rect)
        for enemy in enemies:
            pygame.draw.rect(screen, RED, enemy.rect)

        # Display score
        font = pygame.font.Font(None, 36)
        text = font.render(f'Score: {score}', True, WHITE)
        screen.blit(text, (10, 10))

        pygame.display.flip()
        clock.tick(FPS)

    pygame.quit()

if __name__ == "__main__":
    main()
