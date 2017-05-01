# Mini-Game1
	import pygame
	import numpy as np
	from apple_class import *
	from snake_class import *
	from explosion_class import *
	from bullets_class import *

	score = 0
	pygame.init()
	screen_width = 640
	screen_height = 400
	FPS = 40
	screen = pygame.display.set_mode((screen_width, screen_height))
	clock = pygame.time.Clock()

	snake = Snake()
	apple = Apple()
	explosion = Explosion()

	apple_size = 12
	snake_size = 8
	bulletlist = []

	def CollisionDet(x1, y1, x2, y2, size1, size2):
		if x1 >= x2 and x1 <= x2 + size2 or x1 + size1 >= x2 and x1 + size1 <= x2 + size2:
			if y1 >= y2 and y1 <= y2 + size2:        
				return True
			elif y1 + size1 >= y2 and y1 + size1 <= y2 + size2 or y2 >= y1 and y2 + size2 <= y1 + size1:
				return True


	running = 1
	while running:
		for event in pygame.event.get():
			if event.type == pygame.QUIT:
				pygame.quit()
				running = False

		score += 0.1
		key = pygame.key.get_pressed()

		if snake.hit_points > 0:
			snake.handle_keys()

		screen.fill((255,255,255))
		snake.draw(screen)

		if apple.x <= 0 or apple.x == 0 and apple.y <= 0:
			side = 1

		if apple.y <= 0:
			apple.y = 0

		if apple.x < 620 and side == 1:
			apple.move_par()
		elif apple.x >= 620:
			side = 2
		elif apple.y >= 380:
			side = 3

		if side == 2:
			apple.move_par_right()

		if side == 3:
			apple.move_par_bot()

		apple.draw(screen)

		if CollisionDet(snake.x, snake.y, apple.x, apple.y, snake_size, apple_size):
			snake.hit_points -= 1
			print(snake.hit_points)


		##FIRING BULLETS##
		if key[pygame.K_SPACE]:
			bullet = Bullets(snake.x + 5, snake.y)
			bulletlist.append(bullet)
		for b in bulletlist:
			b.move()
			b.draw(screen)

			if CollisionDet(b.x, b.y, apple.x, apple.y, 5, apple_size):
				apple.x = 0
				apple.y = 0



		if snake.hit_points <= 0:
			print('you lose, dingo')
			explosion.x = snake.x
			explosion.y = snake.y

			explosion.draw(screen)


		pygame.display.update()

		clock.tick(FPS)
