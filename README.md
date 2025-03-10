# doc-pygame

# Pygame Comprehensive Reference Guide

## Table of Contents
1. [Setup and Installation](#setup-and-installation)
2. [Basic Components](#basic-components)
3. [Drawing and Graphics](#drawing-and-graphics)
4. [Input Handling](#input-handling)
5. [Sound and Music](#sound-and-music)
6. [Collision Detection](#collision-detection)
7. [Sprite Management](#sprite-management)
8. [Animation Techniques](#animation-techniques)
9. [Text and Fonts](#text-and-fonts)
10. [Time and Clock](#time-and-clock)
11. [Advanced Techniques](#advanced-techniques)
12. [Performance Optimization](#performance-optimization)
13. [Best Practices](#best-practices)
14. [Common Patterns](#common-patterns)
15. [Debugging Tools](#debugging-tools)

## Setup and Installation

### Basic Setup
```python
import pygame
pygame.init()  # Initialize all pygame modules
screen = pygame.display.set_mode((800, 600))  # Create display surface
pygame.display.set_caption("My Game")  # Set window title
clock = pygame.time.Clock()  # Create clock object to manage frame rate
```

### Game Loop Template
```python
running = True
while running:
    # Process events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    
    # Update game state
    
    # Render
    screen.fill((0, 0, 0))  # Fill screen with black
    
    # Draw objects here
    
    # Update display
    pygame.display.flip()
    
    # Cap the frame rate
    clock.tick(60)  # 60 FPS

pygame.quit()  # Uninitialize all pygame modules
```

### Installation
```bash
pip install pygame
```

## Basic Components

### Key Objects and Modules
- `pygame.display`: For setting up and controlling the display window
- `pygame.Surface`: Object for representing images, where drawing occurs
- `pygame.Rect`: Rectangle object, used for positioning and collision detection
- `pygame.event`: For handling user input and other events
- `pygame.time.Clock`: For controlling game timing and frame rate
- `pygame.mixer`: For loading and playing sounds
- `pygame.font`: For rendering text
- `pygame.image`: For loading and saving images
- `pygame.transform`: For resizing and rotating surfaces
- `pygame.draw`: For drawing shapes

### Display Control
```python
# Set display mode
screen = pygame.display.set_mode((width, height), flags)
# Common flags: pygame.FULLSCREEN, pygame.RESIZABLE, pygame.NOFRAME, pygame.DOUBLEBUF

# Get current display surface
screen = pygame.display.get_surface()

# Update whole screen
pygame.display.flip()

# Update only portions that changed
pygame.display.update()  # Updates entire screen by default
pygame.display.update([rect1, rect2])  # Update specific rectangles

# Set window caption
pygame.display.set_caption("Game Title")

# Get window size
width, height = screen.get_size()
```

## Drawing and Graphics

### Colors
```python
# RGB format
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# RGBA format (with alpha/transparency)
TRANSPARENT_RED = (255, 0, 0, 128)  # 50% transparent

# Get color of a pixel
color = screen.get_at((x, y))
```

### Drawing Shapes
```python
# Fill screen with color
screen.fill(BLACK)

# Draw rectangle
pygame.draw.rect(screen, RED, (x, y, width, height))  # Filled
pygame.draw.rect(screen, RED, pygame.Rect(x, y, width, height), width=2)  # Outline

# Draw circle
pygame.draw.circle(screen, GREEN, (center_x, center_y), radius)
pygame.draw.circle(screen, GREEN, (center_x, center_y), radius, width=2)  # Outline

# Draw ellipse
pygame.draw.ellipse(screen, BLUE, (x, y, width, height))
pygame.draw.ellipse(screen, BLUE, (x, y, width, height), width=2)  # Outline

# Draw line
pygame.draw.line(screen, WHITE, (start_x, start_y), (end_x, end_y), width=1)

# Draw multiple lines
pygame.draw.lines(screen, WHITE, closed, [(x1, y1), (x2, y2), ...], width=1)

# Draw polygon
pygame.draw.polygon(screen, RED, [(x1, y1), (x2, y2), (x3, y3), ...])
pygame.draw.polygon(screen, RED, [(x1, y1), (x2, y2), (x3, y3), ...], width=2)  # Outline

# Draw arc
pygame.draw.arc(screen, GREEN, (x, y, width, height), start_angle, stop_angle, width=1)
```

### Working with Images
```python
# Load image
image = pygame.image.load("filename.png")
image = pygame.image.load("filename.png").convert()  # Faster blitting
image = pygame.image.load("filename.png").convert_alpha()  # Preserves transparency

# Create empty Surface
surface = pygame.Surface((width, height))
surface = pygame.Surface((width, height), pygame.SRCALPHA)  # Transparent surface

# Blit (copy) one surface onto another
screen.blit(image, (x, y))
screen.blit(image, destination_rect)
screen.blit(image, destination_rect, source_rect)  # Copy only part of the image

# Transform images
scaled_image = pygame.transform.scale(image, (new_width, new_height))
rotated_image = pygame.transform.rotate(image, angle_degrees)
flipped_image = pygame.transform.flip(image, flip_x, flip_y)
```

## Input Handling

### Event Processing
```python
for event in pygame.event.get():
    if event.type == pygame.QUIT:
        running = False
    elif event.type == pygame.KEYDOWN:
        if event.key == pygame.K_ESCAPE:
            running = False
        elif event.key == pygame.K_SPACE:
            # Space bar pressed
            pass
    elif event.type == pygame.KEYUP:
        if event.key == pygame.K_SPACE:
            # Space bar released
            pass
    elif event.type == pygame.MOUSEBUTTONDOWN:
        if event.button == 1:  # Left mouse button
            print("Mouse position:", event.pos)
    elif event.type == pygame.MOUSEMOTION:
        mouse_x, mouse_y = event.pos
        rel_x, rel_y = event.rel  # Relative movement since last event
```

### Keyboard State
```python
# Get state of all keys
keys = pygame.key.get_pressed()
if keys[pygame.K_LEFT]:
    # Move left
    player_x -= 5
if keys[pygame.K_RIGHT]:
    # Move right
    player_x += 5
```

### Mouse State
```python
# Get mouse position
mouse_x, mouse_y = pygame.mouse.get_pos()

# Get mouse button state
left, middle, right = pygame.mouse.get_pressed()
if left:
    # Left mouse button is being held down
    pass

# Set mouse position
pygame.mouse.set_pos((x, y))

# Hide cursor
pygame.mouse.set_visible(False)
```

### Controller/Joystick Input
```python
# Initialize joystick module
pygame.joystick.init()

# Count connected joysticks
joystick_count = pygame.joystick.get_count()

# Initialize first joystick
if joystick_count > 0:
    joystick = pygame.joystick.Joystick(0)
    joystick.init()
    
    # Get joystick name
    name = joystick.get_name()
    
    # Get axis values (-1.0 to 1.0)
    x_axis = joystick.get_axis(0)
    y_axis = joystick.get_axis(1)
    
    # Get button state
    button_pressed = joystick.get_button(0)  # First button
```

## Sound and Music

### Playing Sounds
```python
# Initialize the mixer
pygame.mixer.init()

# Load sound
sound = pygame.mixer.Sound("sound.wav")

# Play sound
sound.play()  # Play once
sound.play(loops=3)  # Play 4 times (once + 3 loops)
sound.play(loops=-1)  # Loop indefinitely

# Set volume (0.0 to 1.0)
sound.set_volume(0.5)  # 50% volume

# Stop sound
sound.stop()
```

### Background Music
```python
# Load and play music
pygame.mixer.music.load("music.mp3")
pygame.mixer.music.play()  # Play once
pygame.mixer.music.play(loops=-1)  # Loop indefinitely

# Set volume (0.0 to 1.0)
pygame.mixer.music.set_volume(0.5)  # 50% volume

# Pause/unpause music
pygame.mixer.music.pause()
pygame.mixer.music.unpause()

# Stop music
pygame.mixer.music.stop()

# Fade out music
pygame.mixer.music.fadeout(2000)  # Fade out over 2 seconds
```

## Collision Detection

### Rectangle Collision
```python
# Create rectangles
rect1 = pygame.Rect(x1, y1, width1, height1)
rect2 = pygame.Rect(x2, y2, width2, height2)

# Check if rectangles overlap
if rect1.colliderect(rect2):
    print("Collision detected!")

# Check if point is inside rectangle
if rect1.collidepoint(point_x, point_y):
    print("Point inside rectangle!")

# Check if rectangle collides with any rectangle in a list
collided_index = rect1.collidelist([rect2, rect3, rect4])
if collided_index != -1:
    print(f"Collision with rectangle {collided_index}")

# Get list of all rectangles colliding with rect1
collided_indices = rect1.collidelistall([rect2, rect3, rect4])
```

### Mask-Based Collision (Pixel Perfect)
```python
# Create masks from images
mask1 = pygame.mask.from_surface(image1)
mask2 = pygame.mask.from_surface(image2)

# Check overlap
offset = (image2_x - image1_x, image2_y - image1_y)
if mask1.overlap(mask2, offset):
    print("Pixel-perfect collision detected!")
```

## Sprite Management

### Basic Sprite
```python
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill((255, 0, 0))  # Red square
        self.rect = self.image.get_rect()
        self.rect.center = (400, 300)
        
    def update(self):
        # Move based on keyboard input
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            self.rect.x -= 5
        if keys[pygame.K_RIGHT]:
            self.rect.x += 5
```

### Sprite Groups
```python
# Create groups
all_sprites = pygame.sprite.Group()
enemies = pygame.sprite.Group()

# Create and add sprites
player = Player()
all_sprites.add(player)

for i in range(5):
    enemy = Enemy()
    all_sprites.add(enemy)
    enemies.add(enemy)

# Update all sprites
all_sprites.update()

# Draw all sprites
all_sprites.draw(screen)

# Check for collisions between player and enemies
hits = pygame.sprite.spritecollide(player, enemies, False)
if hits:
    print("Player hit by", len(hits), "enemies")
    
# Pixel-perfect collisions
hits = pygame.sprite.spritecollide(player, enemies, False, pygame.sprite.collide_mask)
```

## Animation Techniques

### Spritesheet Animation
```python
class AnimatedSprite(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        # Load spritesheet
        self.spritesheet = pygame.image.load("spritesheet.png").convert_alpha()
        
        # Create list of frames
        self.frames = []
        for i in range(8):  # 8 frames
            # Extract frame from spritesheet
            frame = self.spritesheet.subsurface((i * 64, 0, 64, 64))
            self.frames.append(frame)
            
        self.current_frame = 0
        self.image = self.frames[self.current_frame]
        self.rect = self.image.get_rect()
        
        # Animation speed
        self.animation_speed = 0.1
        self.last_update = pygame.time.get_ticks()
        
    def update(self):
        # Update animation
        current_time = pygame.time.get_ticks()
        if current_time - self.last_update > 100:  # 100ms per frame
            self.last_update = current_time
            self.current_frame = (self.current_frame + 1) % len(self.frames)
            self.image = self.frames[self.current_frame]
```

### Smooth Movement
```python
class SmoothMover(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill((0, 255, 0))
        self.rect = self.image.get_rect()
        
        # Position as floats for smoother movement
        self.position = pygame.math.Vector2(400, 300)
        self.velocity = pygame.math.Vector2(0, 0)
        self.acceleration = pygame.math.Vector2(0, 0)
        
        self.max_speed = 5
        
    def update(self):
        # Apply acceleration
        self.velocity += self.acceleration
        
        # Limit speed
        if self.velocity.length() > self.max_speed:
            self.velocity.scale_to_length(self.max_speed)
            
        # Update position
        self.position += self.velocity
        
        # Update rect position
        self.rect.center = (round(self.position.x), round(self.position.y))
        
        # Reset acceleration
        self.acceleration = pygame.math.Vector2(0, 0)
```

## Text and Fonts

### Basic Text Rendering
```python
# Initialize font module
pygame.font.init()

# Create font object
font = pygame.font.Font(None, 36)  # Default font, size 36
font = pygame.font.Font("font.ttf", 36)  # Custom font file
font = pygame.font.SysFont("arial", 36)  # System font

# Render text
text = font.render("Hello, World!", True, (255, 255, 255))  # White text
text = font.render("Hello, World!", True, (255, 255, 255), (0, 0, 255))  # With blue background

# Get text rectangle
text_rect = text.get_rect()
text_rect.center = (400, 300)  # Center text on screen

# Draw text
screen.blit(text, text_rect)
```

### Multiline Text
```python
def draw_multiline_text(surface, text, pos, font, color):
    words = [word.split(' ') for word in text.splitlines()]
    space = font.size(' ')[0]
    max_width, max_height = surface.get_size()
    x, y = pos
    
    for line in words:
        for word in line:
            word_surface = font.render(word, True, color)
            word_width, word_height = word_surface.get_size()
            
            if x + word_width >= max_width:
                x = pos[0]
                y += word_height
                
            surface.blit(word_surface, (x, y))
            x += word_width + space
            
        x = pos[0]
        y += word_height
```

## Time and Clock

### Timing and Frame Rate
```python
# Create clock object
clock = pygame.time.Clock()

# In game loop
dt = clock.tick(60) / 1000.0  # dt in seconds, cap at 60 FPS

# Time-based movement
player.x += player.speed * dt  # Move consistently regardless of FPS

# Get time in milliseconds
current_time = pygame.time.get_ticks()

# Create timer event
pygame.time.set_timer(pygame.USEREVENT + 1, 1000)  # Fire event every 1000ms
```

### Frame Rate Display
```python
def show_fps(surface, clock, font, color, pos):
    fps = str(int(clock.get_fps()))
    fps_text = font.render(f"FPS: {fps}", True, color)
    surface.blit(fps_text, pos)
```

## Advanced Techniques

### Camera System
```python
class Camera:
    def __init__(self, width, height):
        self.camera = pygame.Rect(0, 0, width, height)
        self.width = width
        self.height = height
        
    def apply(self, entity):
        return entity.rect.move(self.camera.topleft)
        
    def update(self, target):
        # Keep target centered on screen
        x = -target.rect.centerx + int(self.width / 2)
        y = -target.rect.centery + int(self.height / 2)
        
        # Restrict camera movement
        # x = min(0, x)  # Left
        # y = min(0, y)  # Top
        # x = max(-(self.width - SCREEN_WIDTH), x)  # Right
        # y = max(-(self.height - SCREEN_HEIGHT), y)  # Bottom
        
        self.camera = pygame.Rect(x, y, self.width, self.height)

# In game loop
camera.update(player)
for sprite in all_sprites:
    screen.blit(sprite.image, camera.apply(sprite))
```

### Particle System
```python
class Particle(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.Surface((5, 5))
        self.image.fill((255, 255, 0))  # Yellow
        self.rect = self.image.get_rect()
        
        self.position = pygame.math.Vector2(x, y)
        self.velocity = pygame.math.Vector2(
            random.uniform(-1, 1),
            random.uniform(-3, -1)
        )
        self.gravity = 0.1
        self.lifetime = random.randint(30, 60)  # frames
        
    def update(self):
        # Apply gravity
        self.velocity.y += self.gravity
        
        # Update position
        self.position += self.velocity
        self.rect.center = (int(self.position.x), int(self.position.y))
        
        # Reduce lifetime
        self.lifetime -= 1
        if self.lifetime <= 0:
            self.kill()
            
# Creating particles
def create_explosion(pos, group):
    for _ in range(20):
        particle = Particle(pos[0], pos[1])
        group.add(particle)
```

### Game State Management
```python
class GameState:
    def __init__(self):
        self.state = 'menu'
        
    def menu(self):
        # Process menu input
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return False
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RETURN:
                    self.state = 'game'
        
        # Draw menu
        screen.fill((0, 0, 0))
        # Draw menu items...
        
        return True
        
    def game(self):
        # Process game input
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return False
            if event.key == pygame.K_ESCAPE:
                self.state = 'menu'
        
        # Update game objects
        
        # Draw game
        screen.fill((0, 0, 128))
        # Draw game objects...
        
        return True
        
    def state_manager(self):
        if self.state == 'menu':
            return self.menu()
        elif self.state == 'game':
            return self.game()

# In main loop
game_state = GameState()
running = True
while running:
    running = game_state.state_manager()
    pygame.display.flip()
    clock.tick(60)
```

## Performance Optimization

### Rendering Optimization
```python
# Use dirty rect animation
dirty_rects = []
background = pygame.Surface((800, 600))
background.fill((0, 0, 0))

# Initial render of static elements
screen.blit(background, (0, 0))
for static_sprite in static_sprites:
    screen.blit(static_sprite.image, static_sprite.rect)
pygame.display.flip()

# In game loop
for moving_sprite in moving_sprites:
    # Add old position to dirty rects
    dirty_rects.append(moving_sprite.rect.copy())
    
    # Update sprites
    moving_sprite.update()
    
    # Add new position to dirty rects
    dirty_rects.append(moving_sprite.rect)
    
# Redraw only dirty rects
for rect in dirty_rects:
    # Draw background over old sprite positions
    screen.blit(background, rect, rect)
    
# Draw all sprites at their new positions
for sprite in all_sprites:
    screen.blit(sprite.image, sprite.rect)
    
# Update only the changed portions
pygame.display.update(dirty_rects)
```

### Profiling
```python
import cProfile

def run_game():
    # Game code here...
    pass

# Profile the game
cProfile.run('run_game()', 'game_profile')

# Analyze with:
# python -m pstats game_profile
```

## Best Practices

### Code Organization
```python
# game.py
import pygame
import sys
from settings import *
from sprites import *
from states import *

class Game:
    def __init__(self):
        pygame.init()
        self.screen = pygame.display.set_mode((WIDTH, HEIGHT))
        pygame.display.set_caption(TITLE)
        self.clock = pygame.time.Clock()
        self.running = True
        
    def new(self):
        # Start a new game
        self.all_sprites = pygame.sprite.Group()
        self.player = Player()
        self.all_sprites.add(self.player)
        self.run()
        
    def run(self):
        # Game loop
        self.playing = True
        while self.playing:
            self.clock.tick(FPS)
            self.events()
            self.update()
            self.draw()
            
    def events(self):
        # Game loop - events
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                self.playing = False
                self.running = False
                
    def update(self):
        # Game loop - updates
        self.all_sprites.update()
        
    def draw(self):
        # Game loop - draw
        self.screen.fill(BLACK)
        self.all_sprites.draw(self.screen)
        pygame.display.flip()
        
    def main_menu(self):
        # Game main menu
        pass

# settings.py
WIDTH = 800
HEIGHT = 600
FPS = 60
TITLE = "My Game"

# Color definitions
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# sprites.py
import pygame
from settings import *

class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH / 2, HEIGHT / 2)
        
    def update(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            self.rect.x -= 5
```

### Memory Management
- Use `convert()` or `convert_alpha()` when loading images
- Reuse surfaces when possible
- Keep sprite groups organized
- Remove unused sprites with `sprite.kill()`
- Use `weakref` for objects that need to reference each other

### Error Handling
```python
try:
    pygame.init()
except pygame.error as e:
    print(f"Could not initialize pygame: {e}")
    sys.exit(1)

try:
    sound = pygame.mixer.Sound("sound.wav")
except pygame.error as e:
    print(f"Could not load sound: {e}")
    sound = None  # Fallback
```

## Common Patterns

### Resource Loading
```python
def load_image(name, colorkey=None, scale=1):
    fullname = os.path.join('data', 'images', name)
    try:
        image = pygame.image.load(fullname)
    except pygame.error as e:
        print(f"Cannot load image: {fullname}")
        raise SystemExit(e)
        
    image = image.convert_alpha()
    
    if scale != 1:
        image = pygame.transform.scale(image, 
               (int(image.get_width() * scale), 
                int(image.get_height() * scale)))
                
    if colorkey is not None:
        if colorkey == -1:
            colorkey = image.get_at((0, 0))
        image.set_colorkey(colorkey, pygame.RLEACCEL)
        
    return image, image.get_rect()
```

### Scene Manager
```python
class SceneManager:
    def __init__(self):
        self.scenes = {}
        self.current_scene = None
        
    def add_scene(self, name, scene):
        self.scenes[name] = scene
        
    def set_scene(self, name):
        if name in self.scenes:
            if self.current_scene:
                self.current_scene.cleanup()
            self.current_scene = self.scenes[name]
            self.current_scene.setup()
            
    def update(self):
        if self.current_scene:
            self.current_scene.update()
            
    def render(self, screen):
        if self.current_scene:
            self.current_scene.render(screen)
            
class Scene:
    def __init__(self):
        pass
        
    def setup(self):
        # Initialize scene
        pass
        
    def cleanup(self):
        # Clean up resources
        pass
        
    def update(self):
        # Update logic
        pass
        
    def render(self, screen):
        # Render scene
        pass
```

## Debugging Tools

### Visual Debugging
```python
def draw_debug_info(screen, player, font):
    # Draw player position
    pos_text = font.render(f"Pos: {player.rect.x}, {player.rect.y}", True, (255, 255, 255))
    screen.blit(pos_text, (10, 10))
    
    # Draw player rectangle
    pygame.draw.rect(screen, (255, 0, 0), player.rect, 1)
    
    # Draw player velocity vector
    pygame.draw.line(screen, (0, 255, 0), 
                    player.rect.center, 
                    (player.rect.centerx + player.velocity.x * 10, 
                     player.rect.centery + player.velocity.y * 10), 2)
```

### Logging
```python
import logging

logging.basicConfig(filename='game.log', level=logging.DEBUG,
                    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger('game')

# In game
logger.debug(f"Player position: {player.rect}")
logger.info(f"Level {level_id} loaded")
logger.warning(f"Resource not found: {resource_name}")
logger.error(f"Error loading save file: {e}")
```
