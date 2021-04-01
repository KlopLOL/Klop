import pygame
from constants import *


class Game:
    def __init__(self):
        pygame.init()
        pygame.mixer.init()

        self.clock = pygame.time.Clock()

        self.screen = pygame.display.set_mode([WIN_WIDTH,WIN_HEIGHT])
        pygame.display.set_caption('Platformer Game')
        self.background_image = pygame.image.load('background.png').convert()
        self.all_sprite_list = pygame.sprite.Group()

        #Создание платформ
        self.platform_list = pygame.sprite.Group()
        self.create_platfroms()

        #Создание артефактов
        self.artifact_list = pygame.sprite.Group()
        self.create_artifacts()

        #Создание спрайта игрока
        self.player = objects.Player(0, 550)
        self.player.platforms = self.platform_list
        self.player.artifact = self.artifact_list
        self.all_sprite_list.add(self.player)

    #Создаем платформы и стены
    def create_platfroms(self):
        platform_coords = [
            [200, 450],
            [200, 500],
            [200, 550],
            [200, 600],
            [300, 450],
            [300, 500],
            [300, 450],
            [450, 450],
            [900, 250],
             ]
        for coord in platform_coords:
            platform = objects.Platform(coord[0], coord[1], 2)
            self.platform_list.add(platform)
            self.all_sprite_list.add(platform)
    #Создаем монетки
    def create_artifacts(self):
        artifacts_coords = [
            [200, 500],
            [200, 550],
            [200, 650],
            [200, 650],
            [300, 450],
            [300, 500],
            [300, 450],
            [450, 450],
            [900, 250],

        ]
        for coord in artifacts_coords:
            artifact = objects.Artifact(coord[0], coord[1])
            self.artifact_list.add(artifact)
            self.all_sprite_list.add(artifact) 
    
    
    def run(self):
        done = False

        self.screen.blit(self.background_image, (0, 0))
        self.all_sprite_list.draw(self.screen)

        #Запускаем главный экран
        while not done:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    done = True
            pygame.display.update()
            pygame.display.flip() 
            self.clock.tick(FPS)
        
        pygame.quit

game = Game()

game.run()

