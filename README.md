import pygame
from constants import *
class Player(pygame.sprite.Sprite):
  '''Этот класс описывает поведение и управление игроком'''

  #конструктор класса Player
  def __init__(self, x, y, img = 'anime.png'):
    super().__init__() 
  #Загрузка изображение игрока
  self.image = pygame.image.load(img).convert_alpha()
  #Указываем положение игрока на экране
  self.rect = self.image.get_rect()
  self.rect.y = y
  self.rect.x = x
  #Указываем скорость игрока
  self.change_x = 0
  self.change_y = 0
  self.score = 0
  self.lives = 1


    
  self.platforms = pygame.sprite.Group()
  self.artifacts = pygame.sprite.Group()


  def update(self):
    #Учитываем гравитацию
    self.calc_graph()
    #Пересчет положения спрайта на экране

    #Смешение вправро-лево
    self.rect.x += self.change_x

    #Проверка сотлконовения с препятсвиями(по горизонтали)
    block_hit_list = pygame.sprite.spritecollide(self, self.platforms, False)
  
    for block in block_hit_list:
      #Если персонаж двигался вправо, остановим его слева от препятствия
      if self.change_x > 0:
        self.rect.right = block.rect.left
      #Если персонаж двигался влево, остановим его справа от препятствия 
      elif self.change_x < 0:
        self.rect.left = block.rect.right 

    #Движение вверх-вниз
    self.rect.y += self.change_y
    
    #Проверка сотлконовения с препятсвиями(по вертикали)
    block_hit_list = pygame.sprite.spritecollide(self, self.platforms, False)
  
    for block in block_hit_list:
      #Если персонаж двигался вниз, остановим его сверху от препятствия
      if self.change_y > 0:
        self.rect.bottom = block.rect.top
      #Если персонаж двигался вверх, остановим его снизу от препятствия 
      elif self.change_y < 0:
        self.rect.top = block.rect.bottom 
      
      #Если персонаж столкнулся в прыжке с препятсвием движение должно остановиться
      self.change_y = 0
    
    #Проверка столкновения с артефактом
    artifact_hit_list = pygame.sprite.spritecollide(self, self.artifacts, False)

    for artifact in artifact_hit_list:
      self.score += 1
      artifact.kill()
  
    
    


  #Функция расчета гравитации
  def calc_graph(self):
    if self.change_y == 0:
       self.change_y = 1
    else:
      #Создаем эффект свободного падения
      self.change_y += 0.3
      #Проверяем персонаж на земле или нет
    if self.rect.y >= WIN_HEIGHT - self.rect.height and self.change_y >= 0:
      self.change_y = 0
    #Проверяем выходит ли персонаж за левую границу экрана
    if self.rect.x < 0:
        self.rect.x = 0
        self.change_x = 0


      
  def jump(self):
      #Чтобы убедиться что под игроком есть платформа - двигаемся на 2 пикселя вниз
      self.rect.y += 2
      platform_hit_list = pygame.sprite.spritecollide(self, self.platforms, False)
      self.rect.y -= 2

      #Если есть платформа прыгаем
      if len(platform_hit_list) > 0 or self.rect.bottom >= WIN_HEIGHT:
          self.change_y = - 11   

   
  def go_left(self):
    #Движение влево
    self.change_x = -5
  def go_right(self):
    #Движение вправо
    self.change_x = 5
  def stop(self):
      #Конец движения по горизонатали
      self.change_x = 0


class Platform(pygame.sprite.Sprite):
  images = ['platform_1.png', 'platform_2.png', 'platform_3.png']
  #Конструктор класса платформы, создание препятсвий для пользователей по кооторым он может по ним но чне через них
  def __init__(self, x, y, type):
    super().__init__()
    #Загрузка изображений спрайта(плтформ)
    self.image = pygame.image.load(Platform.images[0]).convert_alpha()

    #Поместили спрайт в rect и на определенные координаты
    self.rect = self.image.get_rect()
    self.rect.y = y
    self.rect.x = x
class Artifact(pygame.sprite.Sprite): 
  def __init__(self, x, y, img = 'coin.png'):
    super().__init__()
    #Загрузка изображений спрайта(артифакта)
    self.image = pygame.image.load(img).convert_alpha()

    #Поместили спрайт в rect и на определенные координаты
    self.rect = self.image.get_rect()
    self.rect.y = y
    self.rect.x = x   


